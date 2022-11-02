---
title: C++的一些神奇用法
date: 2020-07-05T20:44:48+08:00
tags: ["C++", Learning]
categories: [Learning]
---

## 数组索引, 倒过来还是索引

由于数组索引被解释成`指针+偏移`, 所以我们用`偏移+指针`也未尝不可.

```C++
#include <iostream>
using namespace std;
int main () {
    int a[] = {1, 2, 3, 4, 5, 6, 7};
    cout << "a[4]: " << a[4] << endl;
    cout << "4[a]: " << 4[a] << endl;
    
    return 0;
}
```

## `lambda` 的无限套娃

```C++
int main(){[](){[](){[](){[](){[](){
    cout<<"Hello World"<<endl;
}();}();}();}();}();}
```

> 转自[C++新标准中有哪些浪到没朋友的新写法？ - 王维一的回答 - 知乎](https://www.zhihu.com/question/37135445/answer/71144368)

## 像`Python`一样迭代

```C++
// When you are using C++ 98:
circle *p = new circle(42);
vector<shape*> v = load_shapes();
for (vector<shape*>::iterator i = v.begin(); i != v.end(); i++)
    if (*i && **i == *p)
        cout <<  **i <<"is a match\n";
for (vector<shape*>::iterator i = v.begin(); i != v.end(); i++)
    delete *i;
delete p;


// When you are using the new C++ 17
auto p = make_shared<circle>(42);
auto v = load_shapes();
for (auto &s : v)
    if (s && *s == *p)
        cout << *s << "is a match\n";

```

> 转自[YouTube](https://youtu.be/hEx5DNLWGgA)

除了标准支持的`for`自动迭代器之外, 还有`std::for_each(container.begin(), container.end(), predicate_function)`与`Qt`的`for_each(item, container)`可用. 不过有了新标准之后这种写法更容易理解, 并且没有对库的依赖性. 

## 强大到没朋友的`auto`

据说2038年的C++标准就可以支持这么写了: 

```C++
auto [auto](auto auto) auto {
    auto(auto auto{auto}; auto < auto; auto++)
        auto[auto] = auto;
    auto auto;
}
```

(这个代码就是开个玩笑)

看上一节中的应用, `auto`几乎万能. 

## 像`for`一样在`if`和`while`里使用变量初始化

```C++
if (false; true) {
    std::cout << "Hello World~!" << std::endl;
} // 可以输出

if (auto i = func(parameters); judge(i)) {
    // do something...
} // 根据judge()函数的判断决定要不要执行
```

