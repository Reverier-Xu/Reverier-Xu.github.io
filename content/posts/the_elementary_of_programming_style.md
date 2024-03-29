---
title: The Elementary Of Programming Style
date: 2019-08-18T18:09:06+08:00
tags: [Life, Learning]
categories: [Life]
---
## 前言

这是网上流行的一本绝版书籍,短短几页却浓缩了很多精华.

即使过了30余年, 其中的思想仍旧在编程时十分有参考价值. 时间挡不住作者的真知灼见.

<!--more-->

## 正文

把代码写清楚, 别耍小聪明.

想干什么, 讲的简单点、直接点.

只要有可能, 使用库函数.

避免使用太多的临时变量.

“效率”不是牺牲清晰性的理由.

让机器去干那些脏活.

重复的表达式应该换成函数调用.

加上括号、避免歧义.

不要使用含糊不清的变量名.

把不必要的分支去掉.

使用语言的好特性, 不要使用那些糟糕的特性.

该用逻辑表达式的时候, 不要使用过多的条件分支.

如果逻辑表达式不好理解, 就试着做下变形.

选择让程序更简洁的数据表达形式.

先用伪代码写, 再翻译成你使用的语言.

模块化. 使用过程和函数.

只要你能保证程序的可读性, 能不用 goto 就别用 .

不要给糟糕的代码打补丁 - 重写就是了.

把大的程序分成一小片一小片来写, 分块测试.

使用递归程序来处理递归定义的数据结构.

正确和错误的输入数据都要测试.

确保输入不会超出程序的限制.

依靠文件结束来终止输入, 而不是依赖一个记数.

把文件结束作为一个输入状态来处理.

识别出错误的输入; 如果有可能就修复它.

让输入数据很容易构造出来, 让输出数据不言自明.

使用统一的输入格式.

让输入容易校对.

如有可能, 提供更自由的输入格式.

使用输入提示, 允许使用默认值. 并把它们显示出来.

把输入输出放到子程序里.

确保所有的变量在使用前都有初始化.

不要因为一个 bug 而停止不前.

打开编译程序的调试选项.

常量结构用数据声明初始化, 变量结构用执行代码初始化.

小心 off-by-one 错误.

当循环中有多个跳出点时要小心.

如果什么都不做, 那么也要优雅的表现出这个意思.

用边界值测试程序.

手工检查一些答案.

防御式编程 - 为不可能的情况写几句代码.

10.0 乘 0.1 很难保证永远是 1.0 .

7/8 等于 0 , 而 7.0/8.0 不等于 0 .

不要直接判断两个浮点数相等.

先做对, 再弄快.

先使其可靠, 再让其更快.

先把代码弄干净, 再让它变快.

别为了获得一丁点“性能”就牺牲掉整洁.

让编译器做些简单的优化.

不要过分追求重用代码; 下次用的时候重新组织一下即可.

确保特殊的情况是真的特殊.

保持简洁以获得速度.

不要死磕代码来加快速度 - 找个更好的算法.

用工具分析你的程序. 在做“性能”改进前先评测一下.

确保注释和代码一致.

不要在注释里仅仅重复代码 - 让每处注释都有价值.

不要给糟糕的代码做注释 - 应该重写它.

给变量都起个有意义的名字.

把程序重新整理一下, 让阅读代码的人更容易理解.

为你的数据布局写一个文档.

不要过分注释.
