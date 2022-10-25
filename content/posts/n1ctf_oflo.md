---
title: N1CTF-oflo WriteUp
date: 2020-10-26T14:50:00+08:00
tags: [CTF]
categories: [CTF]
---
如题.

<!--more-->

使用`IDA`打开题目文件后, 发现没有识别出`main`函数, 可以初步判断是由于花指令造成的.

在左侧的函数列表里发现了`mprotect`函数, 这个函数是用来更改内存读写权限的, 隐约感觉有`smc`(`Self-Modifying Code`)的存在.

先从`start`函数定位到`main`函数所在的区段, 发现了明显的花指令特征:

![image-20201026135433040](https://i.loli.net/2020/10/26/nLudGhFiqPc3Kbl.png)

试图用`GDB`调试到此位置:

![image-20201026135656530](https://i.loli.net/2020/10/26/pJzdlhiKQ9ejmrn.png)

于是在`IDA`里将光标定位至`0x400BB1`的位置, 按下U键取消定义, 然后在`0x400BB2`的位置按下C键`Make Code`, 然后再用`IDAPython`给`0x400BB1`变成`nop`

```Python
PatchByte(0x400BB1, 0x90)
```

![image-20201026140032321](https://i.loli.net/2020/10/26/tSpYQhRyq5vgVOE.png)

继续往下分析, `0x400BBC`处还有几个神必数据莫得头绪. 不过从`BB7`跳转到了`BBF`, 所以继续往下看, 然后... 它`retn`了.

继续用`gdb`断点到`0x400BB7`, 然后接着调试:

![image-20201026140355769](https://i.loli.net/2020/10/26/qVp6i3Af4I9OoED.png)

可以看到程序控制流从`0x400BD0`跳到了`0x400BBD`, 接着跳转到了`0x400BD1`. 由此可以推测出`0x400BBC`处也是一个花指令. 先在`0x400BBD`的位置变成汇编, 再用相同的办法把`BBC`给`nop`掉:

![image-20201026142128162](https://i.loli.net/2020/10/26/rw6abV9ofCGcqUL.png)

由此可以看出来, 从`0x400BB7`到`0x400BD0`的一大段代码实际上全是无效代码. 用`IDAPython`写个循环全部变成`nop`就完事了:

![image-20201026142313218](https://i.loli.net/2020/10/26/DSlg8c3Rnx1XBAk.png)

在`0x400CBA`处还有一个长得一模一样的东西, 同样处理之, `0x400D04`还有一坨, `nop`掉即可.

全部搞定之后把光标移到最上面的`main`处, 戳一下F5, 代码就反编译成功了:

![image-20201026142819634](https://i.loli.net/2020/10/26/y1P4qCtMxwAaTHf.png)

第`20`行弄了个函数指针, `21`行用`mprotect`函数将某个`text`段的读写执行权限改成了`7`, 也就是`b111`, 即可读可写可执行. `22~23`行通过函数指针, 将该函数的前十个字节与`v4`的前五位异或, 然后第`24`行用刚刚修改过的函数对输入进行验证. 对部分变量重命名一下, 好康点.

![image-20201026143407379](https://i.loli.net/2020/10/26/eYopwS641ufyOij.png)

可以发现是用了`flag`的前五位. 这个比赛的所有`flag`格式均为`n1ctf{xxx}`, 那么前五位必然是`n1ctf`.

定位到`evaluate`函数所在的位置, 用`IDAPython`把正确代码处理回来, 否则没法`F5`.

![image-20201026144002533](https://i.loli.net/2020/10/26/CpXsamqWn5LHzyR.png)

好极了, 代码恢复成功, F5试一下...失败了, 往下面一看, 还是有花指令.

想试着调试一下, 结果发现了

![image-20201026143407379](https://i.loli.net/2020/10/26/eYopwS641ufyOij.png)

第`17`行那个函数, 里面调用了`ptrace`对程序的运行状态进行了监视. 而`gdb`也是通过`ptrace`进行调试的, 同一时间只能有一个调试器挂在程序上, 所以起到了反调试的作用. 不过往下看, `read`函数是在反调函数下面的, 所以可以先运行程序, 跑起来之后等待输入时用`gdb`再`attach`上去.

![](https://i.loli.net/2020/10/26/9RPCKSzqh8itkbY.png)

接下来定位到`0x400ABC`, 然后跟踪一下函数流程, 并把花指令都给处理掉. 有如下几个地方:

```Python
for i in range(0x400AC4, 0x400AC9):
    PatchByte(i, 0x90)
for i in range(0x400B0E, 0x400B28):
    PatchByte(i, 0x90)
```

搞完了之后再`F5`一下, `evaluate`函数的真容就浮现出来了. 因为`smc`和花指令的影响, 之前`main`函数中给`evaluate`传的参数乱七八糟, 重新`F5`一下`main`函数, 找到这几行:

![image-20201026145602058](https://i.loli.net/2020/10/26/tCdFBJEX1gyvA9q.png)

然后再看重命名之后的`evaluate`函数:

![image-20201026150058722](https://i.loli.net/2020/10/26/hN7m1LtlKy3O5vs.png)

你可以右键参数, 然后选择`set lvar type`, 改成指针之后就能显示出来数组形式的表达式了.

第`39`行处理的地址是`&v20+1-32`, 看一下上面的变量声明, 这个地方就是`v5`, 那么这个循环就是遍历了`v5~v18`.

![](https://i.loli.net/2020/10/26/5xzQaCKyLBG671j.png)

还有个难题就是`space`里面存的到底是个什么东西了.

答案其实很简单, 我们点进那个反调试函数里面瞅一眼, 发现他运行了一下`/usr/bin/cat /proc/version`, 动态调试到`evaluate`里面, 输出一下栈里面的东西,

![image-20201026152439543](https://i.loli.net/2020/10/26/8iyECSWMnocX4Qb.png)

发现`space`存的实际上就是`/usr/bin/cat /proc/version`指令的输出, 即你的`linux`版本信息, 然后取前`13`位. 因为是`linux`程序, 所以不管在什么系统上, 前`13`位必然是`Linux Version `, 那就保证唯一性了.

接下来就是快乐的拿`flag`时间了.

![image-20201026152313624](https://i.loli.net/2020/10/26/JUIeNtiKOBwGvVC.png)

脚本:

```Python
#!/bin/python3
# from __future__ import print_function
magic = [53, 45, 17, 26, 73, 125, 17, 20, 43, 59, 62, 61, 60, 95]
space = 'Linux Version '
head = 'n1ctf'
print (head,end='')
for i in range(len(magic)):
    print(chr(magic[i] ^ (ord(space[i]) + 2)), end='')
```

`flag: n1ctf{Fam3_Is_NULL}`
