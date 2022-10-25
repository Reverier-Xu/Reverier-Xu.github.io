---
title: wine-wechat 窗口阴影置顶解决方案
date: 2020-02-16T19:41:41+08:00
tags: [Linux, Workspace]
categories: [Workspace]
---

## 前言

archlinuxcn中有打包好的wine-wechat应用, 但是该应用启动后有边框阴影特效, 此时如果有另一个应用覆盖在微信的窗口之前, 阴影就会被置顶, 从而造成一些视觉阻碍.

<!--more-->

像下面这样:

![Screenshot_20200214_145311.png](https://i.loli.net/2020/02/14/mYfeQ8w3puBs5Wy.png)

非强迫症还好, 强迫症简直不能忍受. 即使不是强迫症, 这个阴影边框竟然还有调整大小的控件, 对日常使用造成了一定的阻碍. 本文介绍如何关闭这个阴影. 从理论上来说, 所有的出现问题的wine应用都可以应用此办法来解决.

## 解决过程

### xwininfo和wmctrl的使用

`xwininfo`是一个窗口查看工具. 我们可以在打开微信的情况下在终端输入`xwininfo`然后回车, `xwininfo`会让你用鼠标选取窗口, 然后输出这个窗口的详细信息.

![Screenshot_20200214_145939.png](https://i.loli.net/2020/02/14/cTFpRZSfzBoymb8.png)

选中阴影后我们可以查看窗口的`window id`, 在这里是`0xae00014`:

![Screenshot_20200214_150004.png](https://i.loli.net/2020/02/14/tOlIMNgXpK4scE2.png)

然后我们运行`wmctrl -l -G -p -x`来查看当前所有活动的窗口:

![Screenshot_20200214_150030.png](https://i.loli.net/2020/02/14/5uYAympMkr1TzdR.png)

通过多次实验, 我发现微信窗口后四位所对应的不同窗口层次是固定的. 主窗口是`0xXXXX000a`, 那么阴影所对应的窗口就是`0xXXXX0014`.

### 通过xdotool来隐藏阴影窗口

`xdotool`是一个很方便实用的工具. 我们可以通过它来隐藏某个窗口, 只需要提供`window id`就可以了. 使用方法如下:

```bash
$ xdotool windowunmap <window-id>
```

于是我们只要找到`wechat.exe`的`<window-id`然后用`xdotool windowunmap 0xXXXX0014`就可以达成我们的目标了.

### 编写Python脚本并集成到系统里

有了上面介绍的三个工具, 我们就可以通过写一个简单的小脚本来随着微信的启动而启动, 每五秒检测一次, 然后隐藏wechat的窗口阴影, 检测到wechat退出就自动退出.

```python
#!/usr/bin/env python3

import time
import os

while True:
    time.sleep(5)
    exist = os.popen("ps -ef | grep WeChat.exe")
    e = exist.readlines()
    if len(e) < 3:
        print(e)
        print("WeChat not started. Exit...")
        exit()
    output = os.popen("wmctrl -l -G -p -x")
    s = output.readlines()
    print(s)
    id = ''
    for item in s:
        if item.find("wechat.exe.Wine") != -1:
            id = item.split()[0]
            break
    output.close()
    print(id)
    if id != '':
        shadow = id[:-4] + "0014"
        print(shadow)
        os.system("xdotool windowunmap " + shadow)
    else:
        print("WeChat not display yet.")

```

给这个脚本放在一个合适的位置, 加上权限后链接到`/bin/disable-wechat-shadow`

然后编写启动脚本:

```bash
#!/bin/sh
env WINEDEBUG=-all /bin/wechat & /bin/disable-wechat-shadow
```

然后把这个脚本加上执行权限, 链接到`/bin/wechat-start`.

修改开始菜单中wechat的启动命令.将其改为`/bin/wechat-start`.

然后再打开微信, 登录后的主窗口阴影过一小会儿就消失了.

## 小问题

如果你把wechat给关闭了, 或者最小化了, 它每次恢复窗口的时候都会重新绘制阴影. 所以无奈之下只能在wechat运行期间让脚本定时检测了. 时间间隔不敢设置的太短, 导致总有一小段时间会显示阴影, 不过也就3~4秒, 不耽误使用.

## ToDo

* 在[Limstash的博客:通过 wine 完美运行最新版网易云音乐](https://www.limstash.com/articles/201905/1391)这篇文章里作者用C++和X11的接口实现了一个简单的窗口劫持, 在启动网易云之后只需要运行一次即可关闭, 不需要一直挂着脚本. 我看到这篇博文之前也写过一个类似的程序, 效果还不如这位大佬的, 就不贴出来了. 在尝试把代码简单的修改来适应微信的阴影关闭时发现这两个程序有一些不一样的地方, 修改后的程序也没法正常运行, 遂作罢. 等再有时间了好好从头撸一个.

* wine的应用没法和KDE的媒体管理器进行集成, 也就影响到`KDE Connect`的效果, 在来电话时不能够自动暂停音乐的播放, 也不能通过远程媒体控制器来遥控电脑. 不过这个问题解决起来恐怕就要动大工程了.
