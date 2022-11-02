---
title: BUUCTF firmware WriteUp
date: 2021-01-12T23:05:09+08:00
tags: [CTF, Reverse]
categories: [CTF]
---

首先下载题目文件, 使用binwalk解压出固件内的文件

```plaintext
[Rx][•ᴗ•] >>> binwalk -e ./51475f91-7b90-41dd-81a3-8b82df4f29d0.bin

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             TP-Link firmware header, firmware version: 1.-20432.3, image version: "", product ID: 0x0, product version: 155254791, kernel load address: 0x0, kernel entry point: 0x80002000, kernel offset: 4063744, kernel length: 512, rootfs offset: 772784, rootfs length: 1048576, bootloader offset: 2883584, bootloader length: 0
69424         0x10F30         Certificate in DER format (x509 v3), header length: 4, sequence length: 64
94080         0x16F80         U-Boot version string, "U-Boot 1.1.4 (Aug 26 2013 - 09:07:51)"
94256         0x17030         CRC32 polynomial table, big endian
131584        0x20200         TP-Link firmware header, firmware version: 0.0.3, image version: "", product ID: 0x0, product version: 155254791, kernel load address: 0x0, kernel entry point: 0x80002000, kernel offset: 3932160, kernel length: 512, rootfs offset: 772784, rootfs length: 1048576, bootloader offset: 2883584, bootloader length: 0
132096        0x20400         LZMA compressed data, properties: 0x5D, dictionary size: 33554432 bytes, uncompressed size: 2203728 bytes
1180160       0x120200        Squashfs filesystem, little endian, version 4.0, compression:lzma, size: 2774624 bytes, 519 inodes, blocksize: 131072 bytes, created: 2015-04-13 09:35:04

```

在解压出来的文件中包含了kernel镜像以及一个squashfs文件系统镜像. 

使用`firmware-mod-kit`工具包内的`unsquashfs_all.sh`解压文件系统镜像, 这个包在仓库`blackarch`中, 直接安装包名即可. `unsquashfs_all.sh`的位置在`/usr/share/firmware-mod-kit/unsquashfs_all.sh`

```plaintext
[Rx][•ᴗ•] >>> /usr/share/firmware-mod-kit/unsquashfs_all.sh ./120200.squashfs
Exception ignored in: <_io.TextIOWrapper name='<stdout>' mode='w' encoding='utf-8'>
BrokenPipeError: [Errno 32] Broken pipe
Attempting to extract SquashFS 4.X file system...

Skipping squashfs-2.1-r2 (wrong version)...
Skipping squashfs-3.0 (wrong version)...
Skipping squashfs-3.0-lzma-damn-small-variant (wrong version)...
Skipping others/squashfs-2.0-nb4 (wrong version)...
Skipping others/squashfs-2.2-r2-7z (wrong version)...
Skipping others/squashfs-3.0-e2100 (wrong version)...
Skipping others/squashfs-3.2-r2 (wrong version)...
Skipping others/squashfs-3.2-r2-lzma (wrong version)...
Skipping others/squashfs-3.2-r2-lzma/squashfs3.2-r2/squashfs-tools (wrong version)...
Skipping others/squashfs-3.2-r2-hg612-lzma (wrong version)...
Skipping others/squashfs-3.2-r2-wnr1000 (wrong version)...
Skipping others/squashfs-3.2-r2-rtn12 (wrong version)...
Skipping others/squashfs-3.3 (wrong version)...
Skipping others/squashfs-3.3-lzma/squashfs3.3/squashfs-tools (wrong version)...
Skipping others/squashfs-3.3-grml-lzma/squashfs3.3/squashfs-tools (wrong version)...
Skipping others/squashfs-3.4-cisco (wrong version)...
Skipping others/squashfs-3.4-nb4 (wrong version)...

Trying ./src/others/squashfs-4.2-official/unsquashfs... Parallel unsquashfs: Using 12 processors

Trying ./src/others/squashfs-4.2/unsquashfs... Parallel unsquashfs: Using 12 processors

Trying ./src/others/squashfs-4.0-lzma/unsquashfs-lzma... Parallel unsquashfs: Using 12 processors
480 inodes (523 blocks) to write

[=======================================================================|           ] 454/523  86%
created 341 files
created 39 directories
created 70 symlinks
created 0 devices
created 0 fifos
File system sucessfully extracted!
MKFS="./src/others/squashfs-4.0-lzma/mksquashfs-lzma"

```

解压出来的文件夹中`tmp`目录下有一个叫做`backdoor`的后门二进制文件, 查壳发现被upx压缩过, `upx -d`解压之, 然后拖进IDA

![image-20210112230311859](https://i.loli.net/2021/01/12/w4fhtJlqxSFE1mW.png)

成了. 
