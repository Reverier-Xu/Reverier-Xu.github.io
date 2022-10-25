---
title: docker的简单使用 - 博客更新
date: 2020-05-16T12:40:54+08:00
tags: [Web, Development]
categories: [Development]
---
`node.js`更新到`14.2`之后, `hexo`就无法正常使用了. 导致我的博客凉了好久..

现在写一下我目前的解决方案.

<!--more-->

## 错误详情

运行`hexo`之后报错: [Hexo deploy error #4281](https://github.com/hexojs/hexo/issues/4281#)

```bash >folded
$ hexo g -d
(node:482144) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(Use `node --trace-warnings ...` to show where the warning was created)
(node:482144) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:482144) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
(node:482144) Warning: Accessing non-existent property 'lineno' of module exports inside circular dependency
(node:482144) Warning: Accessing non-existent property 'column' of module exports inside circular dependency
(node:482144) Warning: Accessing non-existent property 'filename' of module exports inside circular dependency
INFO  Files loaded in 842 ms
INFO  0 files generated in 669 ms
INFO  Deploying: git
INFO  Clearing .deploy_git folder...
INFO  Copying files from public folder...
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html
TypeError [ERR_INVALID_ARG_TYPE]: The "mode" argument must be integer. Received an instance of Object
    at copyFile (fs.js:1890:10)
    at tryCatcher (/home/reverier/文档/Blog/node_modules/bluebird/js/release/util.js:16:23)
    at ret (eval at makeNodePromisifiedEval (/usr/lib/node_modules/hexo-cli/node_modules/bluebird/js/release/promisify.js:184:12), <anonymous>:13:39)
    at /home/reverier/文档/Blog/node_modules/hexo-fs/lib/fs.js:144:39
    at tryCatcher (/home/reverier/文档/Blog/node_modules/bluebird/js/release/util.js:16:23)
    at Promise._settlePromiseFromHandler (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:547:31)
    at Promise._settlePromise (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:604:18)
    at Promise._settlePromise0 (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:649:10)
    at Promise._settlePromises (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:729:18)
    at Promise._fulfill (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:673:18)
    at Promise._resolveCallback (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:466:57)
    at Promise._settlePromiseFromHandler (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:559:17)
    at Promise._settlePromise (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:604:18)
    at Promise._settlePromise0 (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:649:10)
    at Promise._settlePromises (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:729:18)
    at Promise._fulfill (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:673:18)
    at Promise._resolveCallback (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:466:57)
    at Promise._settlePromiseFromHandler (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:559:17)
    at Promise._settlePromise (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:604:18)
    at Promise._settlePromise0 (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:649:10)
    at Promise._settlePromises (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:729:18)
    at Promise._fulfill (/home/reverier/文档/Blog/node_modules/bluebird/js/release/promise.js:673:18)
```

## 官方回复

```text
@Reverier-Xu @stephen-a2z @dumindu @pwittchen

We published v4.2.1.
v4.2.1 has included fixed compatible with Node 14.

Thanks :)
```

但是更新到4.2.1之后问题依旧.

## 另觅方法

### 降级Node.js?

```bash
$ sudo pacman -U /var/cache/pacman/pkg/nodejs-13.13.0-1-x86_64.pkg.tar.zst
```

用`pacman`的缓存降到了`13.13`版本, 这个方法工作了大概一个星期.

紧接着, `Node.js`的依赖库更新了...

于是 全 部 木 大

如果我想继续使用降级`Node.js`, 那我就必须把依赖库都降级, 同时还要降级其它的所有依赖的与被依赖的软件包, 直到官方兼容`Node 14.2`.

可是, 我这是`Arch Linux`啊, 降级? 大规模降级? 恐怕等官方修复了之后我再更新, 就滚挂了吧.

### 虚拟机?

尝试在Linux里面开Linux虚拟机竟然只是为了更新博客? 我感觉我脑子还没坏emm

### docker? docker!

那不如用`docker`吧.

安装好`docker`之后获取官方的`docker`镜像:

```bash
$ sudo docker pull node:13.14.0-stretch
```

用以下命令启动`docker`环境, 并挂载博客目录, 将`hexo`的服务器端口`4000`映射到本机的`4000`:

```bash
$ sudo docker run -it -p 4000:4000 -v <Blog Location>:/Blog node:13.14.0-stretch /bin/bash
```

装好依赖, 配置好`git`的`SSH Key`, 然后使用`sudo docker ps -l`查询容器`pid`, 接着:

```bash
$ sudo docker commit <pid> node:13.14.0-stretch
```

保存配置好的环境.

编写一个简单的`.desktop`文件:

```json
[Desktop Entry]
Comment=
Exec=konsole -e 'sudo docker run -it -p 4000:4000 -v /home/reverier/文档/Blog:/Blog node:13.14.0-stretch /bin/bash'
GenericName=Docker Blog
Icon=
Name=Blog
NoDisplay=false
Path[$e]=
StartupNotify=true
Terminal=0
TerminalOptions=
Type=Application
X-KDE-SubstituteUID=false
X-KDE-Username=

```

扔到`~/.local/share/applications`目录下面.

写博客还和以前一样, 写好之后点一下这个桌面文件, 输入`root`密码, 然后和以前一样更新博客就可以了.

## 结尾

学了一点简单的`docker`用法, 感觉还不错呢~
