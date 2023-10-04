---
title: KDE上TIM无法打开文件所在位置的解决方案
date: 2020-02-10T19:41:41+08:00
tags: [Linux, Workspace]
categories: [Workspace]
---
## 前言

自从上次用过`Arch Linux`滚挂之后, 辗转于`Manjaro`, `Deepin`, `Fedora`, 最后还是趁着没法出门, 在家里把`Arch Linux`又整上了. 但是安装好TIM之后还是老问题, KDE下面用TIM下载群文件之后没法点击打开文件夹来方便的定位文件.

正好闲着没事干, 就解决一下这个问题.

<!--more-->

## 过程

### 查看出错

右键应用程序启动器, 打开编辑应用程序, 找到TIM, 复制启动指令, 然后打开一个终端并粘贴指令, 执行. 这样就可以在打开TIM的时候查看详细的应用程序输出了. 果不其然, 在我下载完测试文件点击打开文件夹的时候提示找不到指令`dde-file-manager`.

### 第一次解决

`dde-file-manager`是Deepin操作系统的文件管理器, 而在KDE里面文件管理器是`dolphin`. 于是想办法:

```bash
sudo ln -s /bin/dolphin /bin/dde-file-manager
```

试图通过给`dolphin`链接到`dde-file-manager`来伪造一个Deepin文件管理器, 将使用Deepin文件管理器打开的命令链接到`dolphin`上来解决. 但是点击链接依然打不开.

### 再次查看出错

还是同样的方法, 发现在单击链接之后`dolphin`报错无效的选项`show-item`. Google之, 发现深度文件管理器可以通过`dde-file-manager --show-item $FilePath`的形式来打开路径并高亮选中文件.

### 第二次解决

运行`dolphin --help`:

```bash
$ dolphin --help
Usage: dolphin [options] +[Url]
文件管理器

Options:
  -h, --help           Displays help on commandline options.
  --help-all           Displays help including Qt specific options.
  -v, --version        Displays version information.
  --author             显示作者信息.
  --license            显示版权信息.
  --desktopfile <文件名>  此应用程序的桌面条目的基础文件名
  --select             参数中传递的文件和文件夹会被选中. 
  --split              Dolphin 将以拆分视图启动. 
  --new-window         Dolphin 将明确地在新窗口中打开. 
  --daemon             启动 Dolphin 守护进程 (只在使用 DBus 接口时需要)

Arguments:
  +[Url]               要打开的文档
```

从参数列表来看只需要把`--show-item`参数更换成`--select`参数就可以了. 我选择拿python简单的写几句来实现这个功能. 首先删掉原先的`dde-file-manager`链接: `sudo rm /bin/dde-file-manager`

然后想办法让python接受两个参数, 并调换第一个参数.

```python
##!/bin/python3
## -*- coding: UTF-8 -*-
## 文件名: dde-file-manager.py
import sys
import os
options = sys.argv[1]
path = sys.argv[2]
os.system("dolphin --select "+'"'+path+'"')
```

简单的替换掉就可以了. 加上执行权限, 扔到`$HOME/.local/share`目录下面, 然后:

```bash
sudo ln $HOME/.local/share/dde-file-manager.py /bin/dde-file-manager
```

再然后打开TIM, 接收文件, 点击打开所在文件夹, 就可以非常顺利的用`dolphin`打开文件夹了.

## 结尾

如果网上找不到现有的办法可以解决某些小bug, 就动手自己解决. 通常情况下这些问题并不复杂, 只是大部分人会永远把自己局限在一个新手的层面, 除了Google和百度就不肯自己动手发现问题了. 事实证明, 自己动手, 还是很明智的一个选择.
