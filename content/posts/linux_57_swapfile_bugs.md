---
title: Linux 5.7.2 swapfile 无法挂载的解决方案
date: 2020-06-12T13:10:08+08:00
tags: [Linux, Workspace]
categories: [Workspace]
---

如题

<!--more-->

```text
[FAILED] Failed to activate swap /swapfile.
[DEPEND] Dependency failed for Swap.
```

开机时报错如上, 开机之后交换文件无法正确被挂载.

在Arch Linux论坛上找到了解决方案如下:

[SOLVED: Swap not working with Kernel 5.7.2](https://bbs.archlinux.org/viewtopic.php?id=256503)

这个问题仅出现在`ext4`文件系统下的 `Linux 5.7.2` 上, 使用`fallocate` 创建的swap交换文件.

[Arch Linux Wiki](https://wiki.archlinux.org/index.php/swap)的解释如下:

> **Note:** dynamic space allocation such as using `fallocate` is not supported, as it causes problems with some file systems such as [F2FS](https://wiki.archlinux.org/index.php/F2FS)[[1\]](https://github.com/karelzak/util-linux/issues/633) and will likely fail to activate at boot time with error "*swapon: swapfile has holes*" as of kernel 5.7. Hence, contiguous allocation, such as `dd`, is the only reliable way to allocate a swap file.[[2\]](https://manpages.courier-mta.org/htmlman8/swapon.8.html#swapon-8_sect3)

**因此删掉原有的`swapfile`, 用`dd`重新分配即可.**

```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress
sudo chmod 600 /swapfile 
sudo mkswap /swapfile 
sudo swapon /swapfile
```
