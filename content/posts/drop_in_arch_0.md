---
title: 如何在Linux上优雅地堕落-1
date: 2019-11-02T10:50:19+08:00
tags: [Linux, Workspace]
categories: [Workspace]
---

## 前言

为什么折腾了这么久`Linux`, 才想起来整一个博文呢? emmmm因为老是崩盘嘛... 导致我每次重新配置环境都要想半天, 我之前那个软件是怎么装上的. 后来实在受不了, 索性抛弃了`Arch`转向了稍微友好一点的`Manjaro`. 于是我决定写一篇博文, 免得`Manjaro`哪天抽风了又要忙活好久. 不过还是得感叹一下`Manjaro`的稳定性.

小更新: 又换回`Arch Linux`了hhhh. `Manjaro`虽然好, 但是有些地方还是自己手动配置比较舒服.

## 正文

### 安装

由于某些软件和游戏的原因, 暂时还是脱离不了`Windows`的怀抱. 虚拟机的体验太差了, 于是经过多方面考虑, 最终决定采用双系统的办法. 首先在`Windows`上用自带的磁盘管理或者其他的分区工具找好地方, 开辟一个`100GB`左右的空间出来, 然后从镜像站嫖来最新版的`Snapshot`镜像, 用`Ultra ISO`以`RAW`的格式写进一个空间足够大的U盘里, 开机从U盘启动, 安装过程一切顺利. `UEFI`的启动方式需要把硬盘上第一个`FAT32`分区挂载到`/boot/efi`上, 千万不要手抖选择格式化, 否则你就要失去亲爱的`Windows`了.

其他的设置无需多说了, Office套件我选择的是`Libre Office`, 毕竟从现在来说这个发行版是最稳定的, 没有其他一些奇奇怪怪的问题, 同时也比`Open Office`颜值高那么一点.

桌面环境当然首选`KDE`啊, 使用不习惯`Gnome`的操作逻辑. 并且既然是要配置一个自己舒服的桌面环境, 高度可配置的`KDE`当然是第一选择.

### 基本配置

#### 软件源

因为校内网有镜像站, 所以需要修改一下软件源, 这样就可以很方便的安装软件, 进行后面的配置.

```bash
sudo vi /etc/pacman.d/mirrorlist
```

在最上面按照格式添加学校的镜像站, 然后打开`/etc/pacman.conf`添加`Archlinuxcn`源:

```json
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

然后执行

```bash
sudo pacman -Syy
```

来刷新软件源缓存, 然后

```bash
sudo pacman -Syu
```

来将系统更新到最新的版本.

#### 网络加速

因为配置过程中需要大量使用`GitHub`, 所以我们修改一下`/etc/hosts`文件, 以更快的从`GitHub`下载软件.

```bash
sudo nano /etc/hosts
```

然后用`dig`或者`nslookup`指令找出`GitHub`相关域名的IP地址. 2019年11月3日可用的IP如下:

```bash
## GitHub Start
192.30.253.112 github.com
192.30.253.119 gist.github.com
185.199.108.153 assets-cdn.github.com
151.101.128.133 raw.githubusercontent.com
151.101.128.133 gist.githubusercontent.com
151.101.128.133 cloud.githubusercontent.com
151.101.128.133 camo.githubusercontent.com
151.101.128.133 avatars0.githubusercontent.com
151.101.128.133 avatars1.githubusercontent.com
151.101.128.133 avatars2.githubusercontent.com
151.101.128.133 avatars3.githubusercontent.com
151.101.128.133 avatars4.githubusercontent.com
151.101.128.133 avatars5.githubusercontent.com
151.101.128.133 avatars6.githubusercontent.com
151.101.128.133 avatars7.githubusercontent.com
151.101.128.133 avatars8.githubusercontent.com
## GitHub End
```

当然啦, 我可不保证你看到这篇文章的时候这个还管用. 所以找一份最新的, 或者自己用工具找到相关IP才是正确的做法.

#### 基本软件

##### 开发环境

这里的开发环境指的是各种语言的编译环境.

使用以下指令安装几个比较常见的编译工具:

```bash
sudo pacman -S gcc clang python python2
```

java可以用

```bash
yay jdk
```

来选择自己想用的版本. 博主用的是openjdk1.8.0, 因为Minecraft可以用这个版本跑起来 ( 手动狗头 )

##### Shell: Fish

对, 你没有看错, 这是一条鱼. 笔者的`Shell`使用本来受到`Arch Linux`的习惯影响, 是使用`zsh`作为自己的`Shell`的, 但是经过`Konge`学长的推荐, 尝试了这个号称宇宙第一`Shell`的`Fish`. 果不其然, 用户体验良好, 准备常驻了.

要安装`Fish`, 只需要:

```bash
sudo pacman -S fish
```

然后在终端里输入`fish`就可以进入然后开启快乐旅程了.

我们把`fish`设为默认的`Shell`. 首先打开`Konsole`的配置方案管理, 编辑配置方案, 启动命令从`/bin/bash`换成`/bin/fish`. 接着重启`Konsole`. 然后把用户的默认`shell`也换成`fish`:

```bash
sudo chsh -s /usr/bin/fish root
sudo chsh -s /usr/bin/fish [username]
```

`fish`每次启动时都会有打招呼内容, 我们用以下命令来自定义这句话:

```bash
set fish_greeting 'Your Greeting Words'
```

对`fish`进行配置是十分人性化的, 只需要输入`fish_config`就可以了, 然后`fish`会打开本地的一个端口并启动浏览器, 以可视化的方式呈现出`fish`的各项配置. 功能设置十分的详细. 按照自己的喜好调整好就可了.

##### 包管理器: yay

要充分发挥`Arch`系Linux的强大之处, 没有`AUR`怎么能行呢? 所以我们安装`yay`作为辅助`pacman`的包管理器.

```bash
sudo pacman -S yay
```

今后安装软件如果不清楚名称, 就可以

```bash
yay [Package Name]
```

来进行搜索, 然后很方便的安装了.

##### 终端编辑器: Vim

```bash
sudo pacman -S vim
```

即可安装`vim`. 我大概不用介绍`vim`了吧... 这可是一个很知名的编辑器呀.

为了让`vim`更加的好用, 我们可以使用[SpaceVim](https://spacevim.org/cn/)来改善自己的用户体验. 如何安装请参照官网使用指南, 在此不再赘述.

##### 浏览器: Chrome

```bash
sudo pacman -S google-chrome
```

用`Chrome`完全是笔者的个人习惯, `Manjaro`自带的`FireFox`十分的优秀, 如果对浏览器没什么习惯的用`FireFox`完全可以.

##### GUI代码编辑器: Visual Studio Code

经过微软的开源和诸多开发者的贡献之后, Visual Studio Code现在已经无愧于最强代码编辑器的称号了. (当然, 博主并不是在这里引战, 如果你非要用Sublime或者Atom也莫得关系) ArchlinuxCN源中有vscode的二进制版.

```bash
sudo pacman -S visual-studio-code-bin
```

装好之后打开插件搜索chinese装一下中文包, 英语超级优秀的同学请忽略此句.

我一般会对VSCode进行一点小配置, 然后装一些插件. 这里推荐使用Fira Code作为字体并开启字体连字, 某些符号看起来真的无比舒服.

![firacode.png](https://www.hanselman.com/blog/content/binary/Windows-Live-Writer/06e959c4fe7a_BA10/image_db854752-00fd-46d9-adb4-95292dece6a4.png)

还有一些插件, 博主大一学习, 装的插件有C/C++, Red Hat提供的java扩展, python,  intellicode, LeetCode, DailyAnime和VSC Netease music.

#### 扩展软件

##### 下拉式终端: Tilda

本来`Manjaro`是自带了一个`Yakuake`下拉式终端, 但是我实在是用不习惯, 并且它的显示有时候会出些问题. 因此我卸载了`Yakuake`装了`Tilda`. 打开设置, 在设置 > 开机和关机 > 自动启动里添加启动项, 并选择应用程序, 输入`Tilda`并开启.

在桌面上直接敲`Tilda`, 可以从KDE的快速启动器中启动`Tilda`. 启动后我们可以进行一些设置, 比如透明度, 颜色, 启动命令, 延迟, 自动隐藏规则等等.

##### 应用程序启动器: Latte-Dock

虽然我没有用过`Mac OS X`, 但是对其高效的桌面念念不忘. 同时我又想节省一下空间, `Windows`下面, 厚重的任务栏就占去了一部分屏幕, 标题栏又削一层, 菜单栏又削一层... 最后留给应用程序的显示空间就少了不必要的一部分. 于是我想找到一种办法, 把菜单, 标题, 状态栏集中在一个`bar`上面, 任务栏没办法, 要保证同时可用性, 就只能弄一个`dock`当任务栏, 在窗口活动时自动隐藏了. `Latte Dock`原生和KDE集成在一起, 通过它可以享受很棒的`dock`.

```bash
sudo pacman -S latte-dock
```

按照和`tilda`同样的方法把`latte-dock`也设置为自动启动, 然后在快速启动器里面输入`latte`并回车, 就可以看见`dock`了. 然后把任务栏拖动到屏幕上方, 右键任务栏配置面板, 把开始菜单和任务管理器, 虚拟桌面都删掉, 留下显示桌面和状态栏即可. 然后在面板左侧添加空白. 然后添加控件, 选择全屏的应用程序启动面板, 并拖动到`Latte-Dock`的第一个位置. `Latte-Dock`的默认隐藏策略就很智能, 无需单独配置. 在添加控件的侧栏下方安装新控件, 搜索`Application Title`并安装, 然后配置任务栏那一行, 拖动全局菜单到最左侧, 旁边插入空白, 再插入`Application Title`, 再插入空白, 最后是状态栏等控件.

最后我们想办法, 把最大化窗口的标题栏去掉. 在`./.config/kwinrc`中找到`[Windows]`的选项, 然后按照以下方式配置:

```ini
[Windows]
BorderlessMaximizedWindows=true
ElectricBorderCooldown=350
ElectricBorderCornerRatio=0.25
ElectricBorderDelay=150
ElectricBorderMaximize=true
ElectricBorderTiling=true
ElectricBorders=1
RollOverDesktops=true
```

配置完成后的效果大概如图所示.

![blog2.png](https://i.loli.net/2019/11/03/39OkRIvcPWSyeHp.png)

![blog3.png](https://i.loli.net/2019/11/03/hr6Ea92FSNxWKQ1.png)

![blog1.png](https://i.loli.net/2019/11/03/1xjCIT9O7YlNewS.png)

![blog4.png](https://i.loli.net/2019/11/03/ja5GEbLqzo73yxn.png)

##### 输入法: Fcitx-pinyin

作为中国人, 配置了这么久才发现自己连中文输入都不能... 那怎么能行, 当然要装一下中文输入法了.

```bash
sudo pacman -S fcitx-im fcitx-configtool
```

这条指令会安装`fcitx`和`fcitx`的图形化管理界面. 但是安装好后依旧无法输入中文, 这个时候先把`fcitx`设为开机启动, 然后终端输入`fcitx-diagnose`运行诊断, 然后按照诊断来进行配置和修复. 一般情况下你只要配置一下`$HOME/.xprofile`这个文件就好了, 在下面添加如下内容 ( 如果没有这个文件就新建一个 ):

```ini
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
export SAL_USE_VCLPLUGIN=gtk
export LC_ALL="zh_CN.UTF-8"
```

注销后重新登录就可以使用了.

画个节点, 过两天继续更新.

## 安装Arch系统可以参考

{{< bilibili BV1T7411W7Jg >}}

---

{{< bilibili BV1q7411h75E >}}

---

{{< bilibili BV157411N7Hg >}}
