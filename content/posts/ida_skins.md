---
title: IDA 两个极简主义皮肤
date: 2021-02-24T15:00:00+08:00
tags: [CTF, Reverse, Workspace]
categories: [Workspace]
---
如题.

<!--more-->

## 仓库地址

[Reverier-Xu/IDA-Skins](https://github.com/Reverier-Xu/IDA-Skins)

先放成品.

## 简介

IDA的前端是用Qt撸出来的, 所以老早就感觉可以自定义了. 在7.3之前就有用来自定义皮肤的插件, 用的还是QSS, 7.3之后官方也支持了, 于是抽了点时间出来整了两份样式表, 一黑一白.

大体上的设计是照着`VSCode`的样式抄下来的, 一股浓浓的微软风. 删掉了几乎所有的渐变色, 用纯色来代替. 导航条换成了灰度色, 不过我平时都没看过, 就当是个全局滚动条了.

## 如何使用

把仓库里那俩文件夹甩到`IDA安装目录/themes/`下面, 然后打开IDA, `Options->Colors`, 选择主题即可.

因为我在`Linux`下使用`Wine`运行IDA的, 所以字体默认用的是宋体. 虽然渲染没啥问题但看着总感觉难受, 于是就直接在主题里启用了全局`JetBrains Mono`字体. 如果你的电脑上有JB家的IDE, 那应该已经安装了这个字体, 如果没有的话去[JetBrains Mono: 免费开源的代码字体](https://www.jetbrains.com/zh-cn/lp/mono/)上下载一份装上就行.

## 后续调整

那个工具条我怎么看怎么难受. 调整也调的四不像, 后来想想其实我大部分时间都在用右键菜单和键盘快捷键, 那几个工具条我几乎都没用过, 于是就全部删掉了.

然后瞅了一眼Desktop的布局, 萌生了一个想法: 干脆把VSCode抄到底(?)

于是把布局改成了这样:

![image-20210224151056174](https://i.loli.net/2021/02/24/vhGCqAw7Vr5fPBs.png)

![image-20210224151120244](https://i.loli.net/2021/02/24/DxwRXtVeqvjpJGk.png)

嗯, 虽然感觉还是丑了点, 但是比原版看起来舒服多了.

愉快.jpg
