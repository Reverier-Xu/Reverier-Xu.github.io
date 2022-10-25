---
title: Ha1cyon-CTF 你好sao啊 题解
date: 2020-04-28T09:32:33+08:00
tags: [CTF, Reverse]
categories: [CTF]
---

## 源代码

<!--more-->

出题人编写的源代码如下:

```C++
#include <cstdlib>
#include <cstring>
#include <iostream>
using namespace std;
const char base[] =
    "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz01234{}789+/=";

static char find_pos(char ch) {
  char *ptr = (char *)strrchr(base, ch);
  return (ptr - base);
}
char *RxEncode(const char *data, int data_len) {
  int ret_len = (data_len / 4) * 3;
  int equal_count = 0;
  char *ret = NULL;
  char *f = NULL;
  int tmp = 0;
  int temp = 0;
  int prepare = 0;
  int i = 0;
  if (*(data + data_len - 1) == '=') {
    equal_count += 1;
  }
  if (*(data + data_len - 2) == '=') {
    equal_count += 1;
  }
  if (*(data + data_len - 3) == '=') {  // seems impossible
    equal_count += 1;
  }
  switch (equal_count) {
    case 0:
      ret_len += 4;  // 3 + 1 [1 for NULL]
      break;
    case 1:
      ret_len += 4;  // Ceil((6*3)/8)+1
      break;
    case 2:
      ret_len += 3;  // Ceil((6*2)/8)+1
      break;
    case 3:
      ret_len += 2;  // Ceil((6*1)/8)+1
      break;
  }
  ret = (char *)malloc(ret_len);
  if (ret == NULL) {
    printf("No enough memory.\n");
    return nullptr;
  }
  memset(ret, 0, ret_len);
  f = ret;
  while (tmp < (data_len - equal_count)) {
    temp = 0;
    prepare = 0;
    while (temp < 4) {
      if (tmp >= (data_len - equal_count)) {
        break;
      }
      prepare = (prepare << 6) | (find_pos(data[tmp]));
      temp++;
      tmp++;
    }
    prepare = prepare << ((4 - temp) * 6);
    for (i = 0; i < 3; i++) {
      if (i == temp) {
        break;
      }
      *f = (char)((prepare >> ((2 - i) * 8)) & 0xFF);
      f++;
    }
  }
  *f = '\0';
  return ret;
}
int main() {
  cout << "Welcome To Reverier-Encode Program!" << endl;
  cout << "Input Your flag:" << endl;
  char flag[] = {-98,-101,-100,-75,-2,112,-45,15,-78,-47,79,-100,2,127,-85,-34,89,101,99,-25,64,-99,-51,-6,4,0,0,0,0,0,0,0,0};
  char inp[33];
  scanf("%33s", inp);
  char *result = RxEncode(inp, 33);
  //for (int i = 0; i < 33; i++){
  // cout << (int)result[i] << ',';
  //}
  if (strlen(inp) != 32) {
      cout << strlen(inp) << endl;
    cout << "Wrong!" << endl;
    return 0;
  }
  //result = RxEncode(inp, 33);
  //for(int i = 0; i < 34; i++){
  //  cout << (int) result[i] << ',';
  //}
  if (!strcmp(result, flag)) {
    cout << "Congratulations!" << endl;
  } else
    cout << "Wrong!" << endl;
  return 0;
}
```

## 原理

一个换表Base64**加密**. 这道题将`flag`当成换表之后的base64子串进行解码, 并将解码之后的数值储存在程序当中.

## 解题思路

首先你要看出来IDA里面那个`RxEncode`的本质是一个换表`Base64`加密函数, 之后将`main`里面的数据处理一下然后换表`Base64`编码即可看见答案.

![Screenshot_20200428_095607.png](https://i.loli.net/2020/04/28/IhViB5zjoX8WHnu.png)
