---
title: C++ 虚函数
toc: true
date: 2020-06-18T17:11:33+08:00
tags: ["C++", Learning]
categories: [Learning]
---

对C++虚函数的概念做一个简单的了解.

## 介绍

### 虚函数

虚函数指在基类使用`virtual`关键字声明的成员函数, 在使用对象指针时, 允许用基类的指针来调用子类的这个函数.

### 纯虚函数

定义一个函数为纯虚函数, 代表该函数在基类中只有声明, 但是没有被实现. 子类必须对此函数进行重写才能够使用, 与此同时, 基类由于没有提供纯虚函数的具体实现, 所以无法被实例化为对象而使用.

>一、定义
>　纯虚函数是在基类中声明的虚函数，它在基类中没有定义，但要求任何派生类都要定义自己的实现方法。在基类中实现纯虚函数的方法是在函数原型后加`"=0"`
>　`virtual void funtion1()=0;`
>
>二、引入原因
>　　1、为了方便使用多态特性，我们常常需要在基类中定义虚拟函数。
>　　2、在很多情况下，基类本身生成对象是不合情理的。例如，动物作为一个基类可以派生出老虎、孔雀等子类，但动物本身生成对象明显不合常理。
>　　为了解决上述问题，引入了纯虚函数的概念，将函数定义为纯虚函数（方法：virtual ReturnType Function()= 0;），则编译器要求在派生类中必须予以重写以实现多态性。同时含有纯虚拟函数的类称为抽象类，它不能生成对象。这样就很好地解决了上述两个问题。
>声明了纯虚函数的类是一个抽象类。所以，用户不能创建类的实例，只能创建它的派生类的实例。
>纯虚函数最显著的特征是：它们必须在继承类中重新声明函数（不要后面的＝0，否则该派生类也不能实例化），而且它们在抽象类中往往没有定义。
>定义纯虚函数的目的在于，使派生类仅仅只是继承函数的接口。
>纯虚函数的意义，让所有的类对象（主要是派生类对象）都可以执行纯虚函数的动作，但类无法为纯虚函数提供一个合理的缺省实现。所以类纯虚函数的声明就是在告诉子类的设计者，“你必须提供一个纯虚函数的实现，但我不知道你会怎样实现它”。
>
>来自[虚函数和纯虚函数的区别-hackbuteer1](https://blog.csdn.net/Hackbuteer1/article/details/7558868)

### 抽象类

带有**纯虚函数**的类被称为抽象类.

抽象类只能作为基类被继承, 同时如果子类也没有提供该类中纯虚函数的定义, 那么子类也是一个抽象类.

抽象类不能被实例化为对象来使用.

## 实验

```C++
// virtualFunc.cpp 虚函数实验
#include <stdio.h>

class Father {
  public:
    virtual void virtualFunc() {
        printf("Virtual Function of Father class\n");
    }
    void commonFunc() {
        printf("Common function of Father class\n");
    }
};

class Son : public Father { // Father必须使用public关键字修饰, 否则为不可访问的基类
  public:
    virtual void virtualFunc() {
        printf("Virtual Function of Son class\n");
    }
    void commonFunc() {
        printf("Common function of Son class\n");
    }
};

int main () {
    Father *p = new Son();
    p->virtualFunc();
    p->commonFunc();
    return 0;
}
```

结果:

![image-20200618185932043](https://i.loli.net/2020/06/18/S1ra48zBf35uqEc.png)

## 结尾

[Apache553](https://blog.apache553.com)大哥说的没错, C++是永远学不完的.

