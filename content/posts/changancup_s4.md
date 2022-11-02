---
title: 长安杯 S4 关于数据库恢复的二三事
date: 2022-10-31T14:07:41+08:00
draft: true
tags: [Forensics, CTF]
categories: [Forensics]
---

第四届长安杯电子数据取证比赛 @ 西电 不咕鸟3.0

被队友[Ga1@xy](http://www.ga1axy.top/)和[洛千](https://luoq1an.github.io/)带飞拿了第二, 👴主要做了有关前后端和数据库服务器的恢复与取证, 这里想脱离题目, 记录一下本次重构与恢复现场的相关过程. 

> 由于笔者操作系统环境原因, 以下部分只能仅凭记忆进行复现重构, 没法实操给图.

## 网站结构

这次比赛是围绕着一个币圈勒索案件展开的, 检材分为四部分, 分别是 诈骗网站前端/运维的个人电脑/诈骗网站数据库后端/幕后老板的安卓模拟器.

在这次比赛中我所负责的部分主要是服务器与数据库取证, 所以本文主要关注诈骗网站前端和数据库后段的分析, 即检材1和检材3.

其中, 检材1所在的服务器地址为 `172.16.80.133`, 技术员所在的地址为 `172.16.80.100`, 检材3数据库所在的地址为 `172.16.80.128`.

网站结构大致如下:

![](https://files.catbox.moe/tzhyig.png)

我估计是出题人时间仓促, 整个网站的运维可以说是以一种很离谱的方式进行的, 大概可以收录到经典运维反例中.

## 检材1: 前端网站

在检材1的 `/web/app` 目录下放着网站前端的整套源代码, 大致如下:

```shell
$ ls /web/app
admin/          cloud.jar       jdk.tar.gz          market.jar          web/
admin-api.jar   exchange.jar    @logback.path@/     nohup.out           web.tar
admin.tar       jdk/            logs/               ucenter-api.jar
```

在这里其实还要划分一下前后端, 在 `admin` 与 `web` 目录下的是两套使用 `vue2` 框架写成的前端, 而下面的 `cloud.jar`, `market.jar`, `admin-api.jar`, `exchange.jar`, `ucenter-api.jar` 则是一套使用微服务架构的后端.

通过翻看 `bash history` 我们得知, 这个网站的运维长这个画风:

```shell
# 具体的启动顺序记不清了, 网站能不能正常跑起来还跟启动顺序有关🤮
$ nohup java -jar cloud.jar &
$ nohup java -jar exchange.jar &
$ nohup java -jar market.jar &
$ nohup java -jar admin-api.jar &
$ nohup java -jar ucenter-api.jar &
```

这样做的后果是所有服务, 注意是**所有服务的log**会以一种不加锁并且没有任何竞争处理的方式**全部**写入到 `nohup.out` 中.

于是后果是这个log文件的大小高达 250MiB, 里面基本上啥也别想看.

在我打算翻看服务器log来做题的时候, 我发现服务器log全部混杂在一起了, 伴随着各种Java Exception. 甚至有两个Exception同时发生时, traceback全部乱了.

前端的启动方式更加令人不忍直视:

```shell
$ nohup npm run dev &
```

如果说之前那种后台启动方式只是log乱了, 进程不好管理, 那这位更是重量级, 直接使用 npm dev 环境起了一个带 websocket 调试端口的前端服务器.

> (跑了一下官方给的自动部署脚本, 打开网站)
> 
> 👴: 草, 👴 Vue DevTools 怎么都亮了
> 
> ...
> 
> (看完 bash history)
> 
> 👴: 这河里🐎

没有任何 web server, 没有 log 分流管理, 不同服务的访问全靠不同端口...... 这种技术员还是早点被优化比较好.

通过反编译 `admin-api.jar`, 我们可以获知大部分与数据库相关的信息, 包括检材3数据库服务器所在的地址, 开放的端口, 数据库类型等等等.

![](https://files.catbox.moe/fic8ul.png)

有关 admin 相关的各种 API, 直接去扒拉 `admin-api.jar` 不是个什么明智的做法, 这个jar本身都200多MB了, 反编译的结果中也能看出来. 虽然大部分都是 SpringBoot 相关的库, 但业务代吗还是很多很多, 并且 API 组织的很混乱. 但是网站又没有什么 API 文档之类的东西, 这个时候我们可以选择从前端入手.

![](https://files.catbox.moe/dm8bds.PNG)

在前端框架的代码中我们可以很轻松地找到所需要的信息, 通过前端界面逻辑配合着后端代码一起看, 就能够快速理解网站逻辑架构然后定位到所需要的信息上了.

## 检材3: 数据库

这个网站所配置的数据库不止一个, 有 MongoDB 和 MySQL 两种, 同时使用 Redis 作为缓存层. 题目中 MongoDB 基本上没咋用, 假想数据主要是围绕着 MySQL 来的. 通过检材1中 `admin-api.jar` 配置文件所包含的数据库配置, 我们能够很轻松地得到数据库连接, 名称, 用户名, 密码, 所开放的端口等一系列信息.

在阅读过 bash history 之后, 能发现这个技术员最开始是打算本地直接起一个 MySQL 数据库的, 甚至废了老大力气整了个deb包, 结果依赖没整上, 最后装好了 MySQL 也跑不动. 这里做题的时候我也试着直接用service启动数据库了, 不太彳亍. 继续读 history, 就发现他放弃这条路了, 转头拉了一个 docker, 然后在 `/data/mysql` 下贴了个 `docker-compose.yml` 直接一把梭了:

![](https://files.catbox.moe/f13vey.png)

同样地, 在这里我们也可以得知数据库用户名密码地址端口等等一系列信息, 跟在检材1后端配置文件中的条目一致.

接下来我们只需要将 docker daemon 跑起来就可以了:

```
$ systemctl start docker
```

由于docker会自动启动托管服务, 所以在 docker daemon 启动之后MySQL也启动成功了, 可以 `docker ps` 查看一下服务列表.

接下来就可以配置数据库连接软件连上去看看数据库里都有什么东西了.

### 小话题: 备份恢复



## 重构网站

这里由于技术所限, 只能使用火眼的一件式仿真, 虚拟化软件自然而然的也就只有 VMWare 可以用, 没办法用 KVM, Oracle Virtual Box 等我比较熟悉的虚拟化方案. 不过总的来说, 大同小异.

在重构时
