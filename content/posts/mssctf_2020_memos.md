---
title: MssCTF 2020 赛后总结
date: 2020-09-21T3:00:25+08:00
categories: [Memo]
tags: [CTF, Memo]
---
如题

<!--more-->

## 运维与反作弊

在讨论平台的反作弊措施时有人提出了能否给不同选手分发不同题目文件的想法, 思索了一下觉得可行, 我便答应下来, 然后投入到静态题目文件自动分发插件的工作当中. 由于时间紧急就没能好好研究CTFd的文件上传与储存机制, 最开始的想法是创建一种新的挑战类型, 然后慢慢改; 后来写好之后发现数据库冲突了, 测试了好久依旧无法实现. 经[Frank](https://blog.frankli.site/)的提示, 发现其实只要创建一种新的`flag`类型即可. 时间紧急就采用了最简单的写法, 在`api/v1/challenges.py`里直接判断`flag`类型然后返回对应的文件, 简单粗暴. 这样写出来的插件由于更改了平台原有的文件, 所以没法即插即用, 等有时间了好好研究一下如何在不更改原有`api`的情况下实现自动分发.

初赛太辛苦`BlackW@tch`了. 初赛Web题目采用了静态`docker`部署的方式, 所有选手共用一个环境. 赛前去看他部署的时候发现`apache`的日志输出直接映射到了`stdout`, 这样搞的没办法查看`log`了, 但是想着题目应该不会出啥大问题, 折腾了一会儿没折腾好就放弃了. 第二天就直接上线.

然后`web`题成功出问题了. 有一名选手拿到`flag`之后挂了个脚本持续删掉`flag`, 重启`docker`之后继续删, 活生生的把`CTF`变成了出题人和选手之间的`AD`. `bw`只想着赶快修好环境, 也忘记先导出日志抓人, 日志又被重定向到了`stdout`, 重启一下`docker`啥都没了, 最后想起来的时候人也没抓着, 比赛也快结束了.

初赛过程中自动分发插件倒工作良好, 没出什么幺蛾子.

复赛的账号分发任务交给我来做了, 采用自动注册脚本没费什么力气, 然后发送邮件拜托[洛千](https://luoq1an.github.io/)用工具人做法全部发送到了选手的邮箱. 自动分发插件依旧沿用初赛的插件. [Frank](https://blog.frankli.site/)收集了所有`web`和`pwn`题目之后采用`CTFd-Whale`插件把题目弄成了动态的, 选手启动自己专属的`docker`环境做题, `flag`也各不相同, 防止作弊的同时也有效避免了初赛选手删`flag`的问题. PPC评测由于`Windows`下换行符`CRLF`的问题导致测试题目部分选手写的代码没有通过, 不过没什么大碍. 整个复赛过程中平台运行情况挺稳定的, 整个复赛过程中动态题目总共创建了487次`docker`环境, 其中用于测试题目创建了51次, 选手解题创建了436次, 其中陈培琛启动了35次题目环境, 杜厚德启动了32次, 陈鸿嘉启动了29次, 在`"浪费服务器性能排行榜"`上夺得前三.

## 逆向出题

初赛逆向题目放出了三道, 有两道题目是我出的, 考点分别是指令虚拟化和全排列, 难度中等, 没爆0. 复赛题目放出了四道, 上午Java逆向和花指令, Java逆向是一个六元一次方程组求解, 两解; flower直接爆0了. 下午本来准备上happy出的一道vm, 但是上午看了看解题情况, 吓得不敢放了, 于是临时出了一个签到题, F5就能看见flag 的那种. 加上一道换表base64解密, 下午成功没有爆0. 有一说一题目质量出的有点低了... 没把握好选手的整体水平, 最后导致题目难度极不合理.

## 总结

这大概是我参与命题的第四场比赛, 我参与运维的第二场比赛, 总体感觉挺良好的, ~~就是有点浪费肝~~.

经验大概就是要好好把握选手的水平, 以后出题争取能把难度梯度设置的更合理. 运维方面可能就是要提前准备好, 预留足够多的时间, 把平台整好. 本次比赛期间我同时在维护新生赛`MoeCTF`的平台, 初赛前一周还在搭建MSC微软俱乐部的招新网站, 以及维护了一个福利题平台, 加上`MssCTF`反作弊插件的编写, 网信实验班的作业记录平台, QQ机器人, 还有战队考核等其他乱七八糟的事情, 这仨星期通宵了不知道多少次, 感觉生命值快烧完了都. 现在看了看时间表, 除了满满当当的课程之外好像没别的什么大任务了. ~~一定要好好调整作息, 免得英年早逝~~

PS: 初赛复赛总共蹭了五顿饭和一堆零食, 真好吃.jpg
