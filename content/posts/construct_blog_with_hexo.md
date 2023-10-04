---
title: 使用GitHub和HEXO的博客搭建过程
date: 2019-08-18T18:40:54+08:00
tags: [Web, Development]
categories: [Development]
---

## 废话

作为一个小白, 在WEB方面的小白, 搭建一个简单的静态博客确实费了许多精力. 其中参考了无数资料, 有官方的, 非官方的...反正搭建不易,现在就详述一下, 博客是怎么搭建起来的.

<!--more-->

没错,看到这个一级标题了吧, 这篇文章里面不是一个水到渠成的方法, 我会把我所踩过的坑以及踩坑用的方法写进来, 所以...😜

噢,笔者很穷, 用不起Mac, 所以就不会Mac的做法咯~

原创,转载请注明来源

## 准备工作

### Git

准备什么咧? 因为我们的博客是建在GitHub上啦,所以你需要一个Git. [Git 下载地址](https://git-scm.com/)

按照Windows下面装软件的惯例,就 下一步 下一步 下一步 我同意 下一步 下一步 安装 完成 就装好啦~

如果是Linux, Git 应该已经集成到大部分的系统里啦~如果没有,就

DEB系: Debian, Ubuntu, Deepin, Linux Mint以及各种衍生版本,包括Kali Linux和Parrot Sec Linux

```bash
sudo apt-get install git
```

RedHat系: Fedora, RHEL, CentOS

```bash
sudo dnf install git
```

emmm如果是CentOS这种万年不更新的系统的话,用yum就好啦:

```bash
sudo yum install git
```

Open SUSE: 没办法,SUSE自成一派,不过 YasT 确实好用,笔者电脑上就是Windows 10 + Open SUSE

```bash
sudo zypper install git
```

Arch系:Arch Linux, Manjaro, Gentoo

```bash
sudo pacman -S git
```

我猜装个git不需要用AUR吧😨

(别问笔者为啥各大Linux的安装包管理器背的这么熟练,说出来都是泪)

### Node.js

好了,Git 装完了,接下来装Hexo(我们的主角)的必备依赖!

他就是Node.js!(反正笔者也没学过,感觉很厉害就是了) [Node.js下载地址](http://nodejs.cn/download/)

### GitHub账号配置

接下来我们得注册一个GitHub 账号...... 喂,这么重要的东西,应该早就有了吧(手动滑稽

咳

注册完后,我们继续......

现在我们来配置一下Git (什么?Hexo还没装呢?就先配置Git? )

Windows用户:

装好Git后,会有一个叫做Git Bash的东东出现在你的桌面上 ( Linux用户:不存在的 )

然后...... 你可以找个清闲的地方 ( 不是叫你喝茶 ) ,新建一个文件夹,取一个喜欢的名字. ( 喂,不是给女儿起名字,那么认真干嘛 ) 然后右键,选择Git Bash Here.

Linux用户:

```bash
mkdir ./MyBlog ##你喜欢就好
cd ./MyBlog ##进入这个目录
```

然后,现在我们就可以把Windows和Linux统一起来说啦.

先配置一下你刚注册的账户~

```bash
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
```

( 啥? 没有一个叫 你的GitHub用户名 的用户? 不至于吧,我说的是你的GitHub的用户名,不是叫"你的GitHub用户名"的用户😓)

我们为了避免每次使用都要输入密码登录,可以配置一个叫SSH Key的东西:

```bash
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
```

( 你不至于连open-ssh都没有吧......那得是一个多么精简的系统 )

接下来会让你输入各种信息,啥都不输,无脑回车就行了.

然后找到生成的.ssh的文件夹中的id_rsa.pub密钥, 将内容全部复制.

再然后...... ( 我为哈说啥都要带个然后 )

打开[GitHub_Settings_keys](https://github.com/settings/keys) 页面, 新建new SSH Key

为了保证访问速度......我就不在博客上添加图片了哈,不然GitHub该生气了(手动滑稽

Title随便写,想写啥写啥,然后把刚刚复制的id_rsa.pub的内容粘贴到下面的Key里面,点击 Add SSH key,就OK啦!

然后我们回到Bash ( Linux用户别抬杠, 是个shell都可以, 笔者用的是zsh )

```bash
ssh git@github.com
```

如果出现了 Hi XXXX! You've successfully......balabala......就说明配置成功啦! 具体原理如下:

引自知乎 [吴润的文章](https://zhuanlan.zhihu.com/p/26625249)

> 这里之所以设置GitHub密钥原因是, 通过非对称加密的公钥与私钥来完成加密, 公钥放置在GitHub上, 私钥放置在自己的电脑里. GitHub要求每次推送代码都是合法用户, 所以每次推送都需要输入账号密码验证推送用户是否是合法用户, 为了省去每次输入密码的步骤, 采用了ssh, 当你推送的时候, git就会匹配你的私钥跟GitHub上面的公钥是否是配对的, 若是匹配就认为你是合法用户, 则允许推送. 这样可以保证每次的推送都是正确合法的.

## 正式开始

### 安装HEXO

当当当! 我们的主角HEXO要登场啦!

```bash
npm install -g hexo-cli
```

在我们的Git Bash里使用这条指令来安装hexo!安装会 ~~比较~~ 狠 慢! 因为......我们有伟大的万里长城防火墙啊...... ( 逃

### 初始化博客

安装好之后,检查一下你当前的路径是不是刚刚让你找的清闲之地------一个你想把博客布置起来的空文件夹.

怎么检查?

```bash
pwd
```

然后看输出的路径是不是 xxx/MyBlog 或者其他你取的名字.

确认完毕后,输入:

```bash
hexo init
```

开始初始化你的博客吧!

然后一屏令人目眩的代码飞过......

我们现在打开你的博客文件夹,就能看见下面多出了很多文件啦!

### 本地预览博客

输入:

```bash
hexo g
hexo s
```

然后在浏览器点开 [本地博客](http://localhost:4000) 就能看见我们的博客啦!

### HEXO的正确使用姿势

现在我们瞅一眼 hexo 的正确使用姿势:

```bash
npm install hexo -g #安装Hexo
npm update hexo -g #升级
hexo init #初始化博客
```

命令简写:

```bash
hexo n "文章名称" == hexo new "文章名称" #新建文章,注意,""里面是你的文章名称,不是 文章名称 ......
hexo g == hexo generate #生成本地文件
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署到远程服务器
hexo server #Hexo会监视文件变动并自动更新, 无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存, 若是网页正常情况下可以忽略这条命令
```

就这样, 我们就可以掌握 hexo 的用法啦!

### 发布到GitHub

看我们刚搭好的博客, 是不是有些激动?

什么? 嫌丑?

那不是现在要操心的事情......应当操心的是,如何才能发布到网站上?

这个时候,GitHub就要大显神威了. 打开你的GitHub页面, 然后新建一个仓库......仓库取名叫

```text
你的用户名.github.io
```

我也不知道为啥非要写用户名,但是不写用户名后面的配置会出各种各样的奇怪问题......

然后创建就可以啦.

然后! 用Visual Studio Code / Vim / Emacs / Sublime / Atom / Kate / Notepad ...... ( 是个编辑器都行 ) 打开位于博客文件夹下的_config.yml, 然后把光标扯到最下面, 看到尾端有一个deploy的东东,编辑一下:

```yml
## Deployment
### Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: git@github.com:你的用户名/你的用户名.github.io.git
  branch: master
```

回到Bash, 输入下面的指令安装部署插件:

```bash
npm install hexo-deployer-git --save
```

然后! 我们就可以很方便的部署网站啦! 两条指令搞定!

```bash
hexo clean #清除上次的缓存
hexo g -d #等价于执行hexo g 后再执行一次 hexo d , 这两个指令已经介绍过了.
```

打开 你的用户名.github.io,你的网站就已经部署好啦!

## 日常使用

那我们怎么写博客呢?

只要打开Git Bash, cd到你的博客文件夹 ( Windows用户可以点开博客文件夹,右键 Git Bash Here)然后输入:

```bash
hexo new post "文章名称"
```

就能新建一篇文章啦!

你可以去下载一个 [Typora](https://typora.io/) ,一款很好用的MarkDown编辑器–-----好用到你根本不会觉得你在写一个叫做MarkDown的标记语言…....事实上, 笔者现在仍旧对MarkDown保持着一股生疏感.安装好Typora之后,你可以打开设置,然后就可以设置每次打开的文件夹啦! 我们把文件夹设置为:

```bash
MyBlog\source\_posts
```

以后写的文章就可以保存在这里啦!

想更新到GitHub的话,就是:

```bash
hexo clean
hexo g -d
```

然后打开你的博客地址 : 你的用户名.github.io就可以看见你刚写的文章啦!

PS: 由于GitHub Pages需要每次重建,所以你可能不会很快的看见自己的文章咯~这样的话最好等上两分钟,你的文章就会出现在博客列表里啦!

## 未完待续~

下集预告:安装Next主题, 配置我的博客!
