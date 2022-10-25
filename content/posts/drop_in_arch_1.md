---
title: 如何在Linux上优雅的堕落-2
date: 2019-11-15T15:32:08+08:00
tags: [Linux, Workspace]
categories: [Workspace]
---

## 前言

请阅读上一篇之后再来读本文哦~

## 正文

### 基本配置

#### 扩展软件

##### 聊天软件

TIM, 微信是差不多是阻止大批量用户使用Linux的原因之最. 我已经不想再吐槽某鹅厂了, 这里讲一下如何在Linux里面使用TIM和微信.

使用微信十分的简单, 无论是wine版本还是网页版都挺优秀的. 在这里我们使用electronic-wechat, 是基于electron和网页版微信的第三方版本, 用户体验可以说是十分优秀的. ArchLinuxCN源里也有这个软件包:

```bash
$ sudo pacman -S electronic-wechat
```

TIM使用的是Deepin团队打包的Deepin-wine来运行的Windows版TIM. 10月24日腾讯发布了LinuxQQ 2.0, 但其代码和2008 1.0版的差别并不大, 使用的还是GTK2.0, 在KDE上的兼容性非常不好, 并且有许多已知BUG. 等腾讯真正静下心来做一个说得过去的QQ for Linux, 或许我会去体验一下.

ArchLinuxCN源里也有打包好的TIM:

```bash
sudo pacman -S deepin.com.qq.office
```

但这样我们在KDE环境下是打不开的, Deepin所打包的TIM是适配GTK风格桌面的. 我们安装gnome-settings-daemon来解决这个问题:

```bash
sudo pacman -S gnome-settings-daemon
```

设置/usr/lib/gsd-xsettings为自启动:

```text
系统设置->开机或关机->自动启动->添加脚本->输入/usr/lib/gsd-xsettings
```

重启就好了.

##### 媒体播放器

由于简洁的需要, 我们可以装mplayer来播放一些常见的视频. 如果有更多的需求, 可以装VLC播放器. VLC就不多介绍了, 用过的人除了吐槽界面丑之外都说好. 另外, 深度影院也是非常不错的, 在KDE下面可能显示有一点点小问题, 但总体来说使用起来还是很舒服的.

音乐播放器能基本达到Windows体验的只有网易云音乐了, 还有一些挺优秀的本地音乐播放器, 如AMROK, 深度音乐等, 可以按照需求去装.

还有一个叫做ffmpeg的东东, 是堪比Media Encoder的存在, 用来转码十分的方便, 还可以自定义多种需求.

```bash
$ sudo pacman -S mplayer ffmpeg netease-cloud-music amrok
```

##### MarkDown 写作软件

其实我习惯装个插件然后用Visual Studio Code来撸markdown, 这样写起来挺方便的. 但是如果习惯Word这种图形化操作的话, 也许一个专用的MarkDown编辑器更适合你. 在这里我们使用Typora, 当然还有Notable, 是一款十分优秀的开源md编辑器, 但是嘛... 我找不到安装的办法. Typora的使用体验挺优秀的, 我没体验过Notable, 不知道会不会更好一些.

```bash
$ sudo pacman -S typora
```
##### 办公软件

说实话, Manjaro安装的时候带的OpenOffice和LibreOffice都是很优秀的产品, 但是从Windows过来的用户还真不一定用的习惯... 所以! 我们可以安装WPS的Linux版本. 这是继网易云音乐, 搜狗输入法之后第三个肯在Linux上用心的国产软件! 除了不支持宏操作以外, 其他的使用体验几乎和Windows版本保持了一致.

```bash
$ sudo pacman -S wps-office
```

##### PDF

Manjaro带的Okular已经十分优秀了... 但我不知道你的版本有没有带... 索性加上:

```bash
$ sudo pacman -S okular
```

##### 游戏

###### Steam

这个不说了, Manjaro出厂带的就有, 几乎畅玩主流游戏, Dota2都可以, 不用我这个菜鸡带大家逛steam了吧 : )

###### Minecraft

Minecraft! 作为一名光荣的方块人, 怎么能不想办法玩MC? 其实不费什么功夫, 你把Windows上MC里的`.minecraft`文件夹复制过来就行了. 对了, 你在Linux下面看不见.开头的文件夹... 按`Ctrl+H`就可以看见隐藏文件夹了.

然后去网上找一个`HMCL.jar`, 和`.minecraft`放在同一个文件夹下面, 然后在终端里进入这个文件夹, 输入

```bash
$ java -jar HMCL.jar
```

就可以啦!

但是这么弄还是有点麻烦..

我们在这个文件夹里建一个脚本:

```bash
$ touch ./start.sh

$ vim ./start.sh
```

`start.sh`

```bash
##!/bin/bash
java -jar ./HMCL.jar
```

然后保存退出vim, 继续:

```bash
$ chmod a+x ./start.sh
```

然后在文件夹里一点就开啦! 你也可以拖动到桌面上, 然后选择链接到此处, 也可以链接到Latte-dock里.

##### 开发套件

按需安装就好了, 个人力荐Jetbrains全家桶.

### 高级配置

我们配置好了所需的软件, 接下来弄一些能显著改善用户体验的东东.

#### 触摸板手势

Linux下面原本是没有触摸板手势这一说的. 那怎么能行呢? OS X那么受到程序员喜爱的原因之一不就是强大的触控板手势么? 所以我们一定要在Linux上实现这个功能.

Linux上最容易的实现莫过于Fusuma, 但是我折腾了一会儿发现在KDE上的表现确实很差很差. 最后选择了libinput-gestures来实现, 效果非常好.

我是按照[kde5与archlinux环境下配置libinput-gestures多手势操作](https://segmentfault.com/a/1190000011327776)这篇文章来配置的, 事实证明十分好用. 我就不重新把这篇文章抄一遍了, 只是把我的配置贴出来供大家参考一下.

`$HOME/.config/libinput-gestures.conf`

```bash
## 四指右滑切换到右侧的虚拟桌面
gesture swipe right 4 xdotool key ctrl+F1
## 四指左滑切换到左侧的虚拟桌面
gesture swipe left 4 xdotool key ctrl+F2
## 在这里我把键位映射改变了一下, 可以去设置里面将切换虚拟桌面
##的快捷键改为上方的快捷键

## 三指下滑查看所有任务
gesture swipe down 3 xdotool key ctrl+F9
## 四指下滑最小化当前窗口
gesture swipe down 4 xdotool key super+alt+m

## 三指上滑打开应用程序面板 ( 开始菜单 )
gesture swipe up 3 xdotool key super+space
## 三指左右滑动快速切换不同的窗口
gesture swipe right 3 xdotool key alt+shift+Tab
gesture swipe left 3 xdotool key alt+Tab
gesture pinch in 2 xdotool key ctrl+minus # 2指捏: 缩小
gesture pinch out 2 xdotool key ctrl+plus # 2指张: 放大
## 事实证明这个放大缩小由于是模拟快捷键操作, 所以并不好用, 也并不流畅
## 但有总比没有好..

## 四指上滑立体显示所有桌面
gesture swipe up 4 xdotool key ctrl+F11
```

## 尾语

感觉应该差不多了, 剩下的自己折腾就好了. 加油啦!
