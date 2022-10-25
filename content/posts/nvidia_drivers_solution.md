---
title: Nvidia Prime的最终折腾方案
date: 2020-05-25T09:46:00+08:00
tags: [Linux, Workspace]
categories: [Workspace]
---
如题.

<!--more-->

## 前言

从折腾好笔记本上的独立显卡到目前已经稳定运行了四个多月左右, 写一下大概的解决方案.

## `Nvidia-Prime`

`Nvidia-Prime`是`Nvidia`官方给`Linux`用户**施舍**的兼容`Optimus`显卡的解决方案.

### 原因

我的笔记本配置:

![neofetch](https://i.loli.net/2020/05/25/I3t1mk2SV4gPyDY.png)

如图. 由于~~混迹于哔哩哔哩~~某些原因需要进行视频渲染, 本身又是个码农, 还是个在校学生, 上个网课之类的, 平时还要背着电脑到处跑, 于是就需要寻找一个完美点的显卡解决方案.

尝试过`Bumblebee`, 由于要配合`bbswitch`使用, 而我的笔记本由于比较特殊, 无法用软件开启或者关闭显卡; 可以从`BIOS`关闭显卡, 但是每次都很麻烦; 显卡状态改变后笔记本需要彻底断电然后冷启动才能够正确的加载显卡活动...

于是最终就只能抛弃了`Bumblebee`.

就在不久之前, `Linus`宣布发布`Linux 5.6`, 介绍中加入了对`RTX`系列显卡的支持. `Nvidia`官方又推出了更新进度虽然缓慢但有总比没有好的`Nvidia-Prime`供`Linux`用户使用, 在`Linux`上折腾显卡才显露出一丝曙光.

### 过程

```bash
$ sudo pacman -Syyu nvidia nvidia-prime nvidia-settings nvidia-utils
```

好了, 没有了.

这几个包可以让你的显卡转起来了. 美中不足的就是它不能有效的关停显卡, 而是让显卡以省电模式继续转.

运行需要独立显卡的程序时, 使用`prime-run xxx`即可.

### 一些软件适配

#### `Davinci Resolve`

`Davinci Resolve`需要安装`opencl-nvidia`才能够正常运行. 否则会报`ld4CXX`相关的错误.

```shell
$ sudo pacman -Syyu opencl-nvidia
```

`Davinci Resolve`启动前有一定延迟, 耐心等待即可.

#### `Osu! Lazer`

使用`prime-run osu-lazer`之后, 调整窗口为全屏.

#### `Minecraft Launcher (Offcial, HMCL)`

卸载`Noto Sans`系字体, 否则打开`launcher`之后界面会保持黑色, 游戏启动时会异常`crash`.

### 已知BUG/影响使用体验的现象

#### `Davinci Resolve`

调出某些子窗口时全部黑屏, 摸索着关掉子窗口之后恢复正常.

例如剪辑`panel`中试图更改片段时长, 或者片段速度时, 在你右键菜单选中这一项并按下时整个屏幕会黑掉. 按一下`Esc`则又会恢复.

无法使用`Fcitx`进行中文输入. `ibus`没有测试.

**可能的解决方案**:

可以尝试使用`Bumblebee`(不带`bbswitch`)与`primusrun`. 使用`primusrun`运行的`Davinci Resolve`不会出现此类BUG. 但是`primusrun`与`Bumblebee`模块的性能较低, 使用`Fusion`制作一些复杂一点的特效时, `Davinci Resolve`会崩溃.

#### `Osu! Lazer`

桌面特效会被强制关闭.

无法输入中文.

#### `Minecraft Launcher (Offcial)`

无法输入中文. 无论是否用独立显卡运行, 都不能输入中文.

#### `Wine`

使用`prime-run`运行的`wine`应用程序无法在全屏显示状态下输入中文. 非全屏显示时没有问题.

使用独显运行时字体渲染有些发虚.

#### `CUDA`

需要使用`CUDA`的软件包在安装`CUDA`之后使用时性能损耗明显, 并且崩溃有些频繁, 只能先凑合着用...

## 结尾

如果你有什么更好的解决方案, 希望能提供在评论区, 不胜感谢.
