---
title: katzekit 2 - FUSE 文件系统
date: 2024-01-08T12:31:38+08:00
tags: [Development, File System, Forensics]
categories: [Development]
---

文件系统是一种用来存储和组织计算机文件的**软件**。我们日常所说的硬盘只是一种存储介质，可以理解为一张白纸，但是这张纸并不能对上面的数据进行分块管理，也不能保证每一个bit在过了一段时间之后还是正确的。而文件系统就是用来管理这张纸的，除了存储文件的元数据信息之外，大部分文件系统还会提供校验码之类的数据安全保障，保证用户的数据不会因为存储介质的问题发生改变。

文件系统本身作为软件，还可以提供一些额外的功能，例如权限控制、加密、压缩、快照等等。这些功能都是在文件系统层面实现的，所以用户不需要关心具体的实现细节，只需要在使用的时候调用相应的接口就可以了。文件系统甚至还可以建立在另一个文件系统之上，例如 `cvsfs-fuse` 等等。

在用户空间文件系统（User-space Filesystems）出现之前，文件系统开发一直是操作系统内核开发人员的工作。创建一个新的文件系统需要了解内核编程和一些内核技术（例如vfs），但是移动存储设备的新兴和数据管理的需求使得这种文件系统开发方式显得很麻烦，应当有一种更加简洁好使的方案来开发文件系统，并能够在不同的操作系统中与原有API相互配合，于是便有了 **FUSE（Filesystem in Userspace）**。

本文的目的是简单探索 FUSE 的 API 接口与设计方式，在后续 katze 的开发中可以借鉴这一套方案进行设计。

## FUSE 简介

**FUSE（Filesystem in Userspace）** 即 用户空间文件系统，定义了一套简单的 API 接口用于文件系统与内核进行交互。FUSE 本身被设计为一个内核模块，用户空间文件系统通过实现 FUSE API 与 FUSE 模块配合，最终实现了在用户空间中对文件系统的访问。

使用 FUSE 开发的文件系统可以直接链接到 FUSE 库，也就是说使用这套文件系统框架不需要了解内核技术也不需要进行内核编程，极大地方便了文件系统开发工作。

### 历史 & 前身

用户空间文件系统并不是一个新的设计，在 FUSE 出现之前已经有了一些方案：

1. [LUFS](https://en.wikipedia.org/wiki/Linux_Userland_Filesystem) 是一种混合用户空间文件系统框架，可为任何应用程序透明地支持无限数量的文件系统，由内核模块和用户空间守护进程组成；
2. [Ufo](https://dl.acm.org/doi/pdf/10.1145/290409.290410) 项目是 Solaris 的一个全局文件系统，允许用户像对待本地文件一样对待远程文件。

FUSE 的主要目的是将这种文件系统实现引入 Linux。

## FUSE Operations

要在 FUSE 中创建文件系统，需要安装 FUSE 内核模块，然后使用 FUSE 库和 API 集来创建文件系统。

一般来说现代 Linux 发行版仓库里都会有 FUSE，并且作为默认内核模块提供。

fuse_operation 结构体中的必要函数：

```c
struct fuse_operations {
    int (getattr) (const char , struct stat );
    int (readlink) (const char , char , size_t);
    int (getdir) (const char , fuse_dirh_t, fuse_dirfil_t);
    int (mknod) (const char , mode_t, dev_t);
    int (mkdir) (const char , mode_t);
    int (unlink) (const char );
    int (rmdir) (const char );
    int (symlink) (const char , const char );
    int (rename) (const char , const char );
    int (link) (const char , const char );
    int (chmod) (const char , mode_t);
    int (chown) (const char , uid_t, gid_t);
    int (truncate) (const char , off_t);
    int (utime) (const char , struct utimbuf );
    int (open) (const char , struct fuse_file_info );
    int (read) (const char , char , size_t, off_t, struct fuse_file_info );
    int (write) (const char , const char , size_t, off_t,struct fuse_file_info );
    int (statfs) (const char , struct statfs );
    int (flush) (const char , struct fuse_file_info );
    int (release) (const char , struct fuse_file_info );
    int (fsync) (const char , int, struct fuse_file_info );
    int (setxattr) (const char , const char , const char , size_t, int);
    int (getxattr) (const char , const char , char , size_t);
    int (listxattr) (const char , char , size_t);
    int (removexattr) (const char , const char *);
};
```

* `getattr`：获取文件属性。这类似于 `stat()` ，`st_dev` 和 `st_blksize` 将被忽略。除非给出 `use_ino`，否则 `st_ino` 也会被忽略；
* `readlink`：读取符号链接；
* `getdir`：读取目录的内容，此操作是 `opendir()` 、 `readdir()` 、...、 `closedir()` 操作序列组合成的。对于每个目录条目，应调用 `filldir()` 函数；
* `mknod`：将创建一个文件节点；
* `mkdir`：创建一个目录；
* `unlink`：删除一个文件；
* `rmdir`：删除一个目录；
* `symlink`：创建一个符号链接；
* `rename`：重命名一个文件；
* `link`：创建一个硬链接；
* `chmod`：更改文件权限；
* `chown`：更改文件所有者和组；
* `truncate`：更改文件大小；
* `utime`：更改文件访问和修改时间；
* `open`：打开文件；
* `read`：读取文件。`read()` 应准确返回请求的字节数，`EOF` 或错误除外。一个例外是当指定了 `direct_io` 时，`read()` 系统调用的返回值就是 `direct_io` 的返回值；
* `write`：写入文件；
* `statfs`：获取文件系统状态；
* `flush`：刷新缓冲区；
* `release`：释放打开的文件；
* `fsync`：同步文件；
* `setxattr`：设置扩展属性；
* `getxattr`：获取扩展属性；
* `listxattr`：列出扩展属性；

这些操作并不都是绝对必要的，仅实现其中一部分也可以构建一个完整的文件系统。

对于 katze 来说，我们只需要读取操作，所以实现的 API 可以简化不少：

* `getattr`
* `getdir`
* `read`
* `statfs`
* `getprops`

实际实现中还会加一些内部方法。在这些方法中，我主要简化了文件读取操作为单个read，read会返回指定范围的文件内容，不会一次性读取整个文件。在实际实现中这种操作可能需要优化，对于根文件来说应当保留fd，否则每次调用read的时候都要走系统调用重新打开fd再关上，很浪费IO性能。但是对于镜像文件内的文件就没必要这么做了，因为所有的数据都是通过 relay 对象逐步定位到镜像文件上的。
