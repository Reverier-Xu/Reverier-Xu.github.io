---
title: 记一次终极狗屎的HTTP请求头
date: 2019-09-18T01:21:21+08:00
tags: [CTF, Web]
categories: [CTF]
---

## 题目

得, 不多说了, 看图吧...

<!--more-->

![Screenshot_20190918_011709.png](https://i.loli.net/2019/09/18/iW1H8qcdmYfnsx3.png)

## 做题过程

```text
rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> User-Agent: curl/7.60.0
> Accept: */*
> Referer: www.xidian.edu.cn
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:25:31 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 96
< Content-Type: text/html
<
{ [96 bytes data]
100    96  100    96    0     0    680      0 --:--:-- --:--:-- --:--:--   680HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:25:31 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 96
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_broswer (Windows NT 10.0; Win64; x64; rv:69.0)" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_broswer (Windows NT 10.0; Win64; x64; rv:69.0)
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:33:44 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 96
< Content-Type: text/html
<
{ [96 bytes data]
100    96  100    96    0     0    615      0 --:--:-- --:--:-- --:--:--   615HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:33:44 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 96
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_broswer" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_broswer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:34:00 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 96
< Content-Type: text/html
<
{ [96 bytes data]
100    96  100    96    0     0    615      0 --:--:-- --:--:-- --:--:--   615HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:34:00 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 96
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:35:11 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 143
< Content-Type: text/html
<
{ [143 bytes data]
100   143  100   143    0     0   1014      0 --:--:-- --:--:-- --:--:--  1014HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:35:11 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 143
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: localhost" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: localhost
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:36:04 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 143
< Content-Type: text/html
<
{ [143 bytes data]
100   143  100   143    0     0   1021      0 --:--:-- --:--:-- --:--:--  1144HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:36:04 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 143
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:36:14 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:36:14 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "cookie: identity: xduser" -v-i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> cookie: identity: xduser
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:43:40 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1166      0 --:--:-- --:--:-- --:--:--  1166HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:43:40 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "cookie: identity=xduser" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> cookie: identity=xduser
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:43:50 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:43:50 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Identity: xduser" -v -i http:                               //39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Identity: xduser
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:48:48 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:48:48 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "identity: xduser" -v -i http:                               //39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> identity: xduser
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:48:55 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1166      0 --:--:-- --:--:-- --:--:--  1166HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:48:55 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "From: xduser" -v -i http://39                               .108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> From: xduser
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:52:36 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1300      0 --:--:-- --:--:-- --:--:--  1300HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:52:36 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "From: xduser@stu.xidian.edu.c                               n" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> From: xduser@stu.xidian.edu.cn
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:52:48 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1159      0 --:--:-- --:--:-- --:--:--  1159HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:52:48 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "From: xduser@xidian.edu.cn" -                               v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> From: xduser@xidian.edu.cn
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:52:55 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:52:55 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "From: xduser@mail.xidian.edu.                               cn" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> From: xduser@mail.xidian.edu.cn
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:53:03 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:53:03 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "From: xduer" -v -i http://39.                               108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> From: xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:54:05 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1300      0 --:--:-- --:--:-- --:--:--  1300HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:54:05 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "identity: xduer" -v -i http:/                               /39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> identity: xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:54:16 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1300      0 --:--:-- --:--:-- --:--:--  1300HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:54:16 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "cookies: identity=xduer" -v -                               i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> cookies: identity=xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:55:06 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1300      0 --:--:-- --:--:-- --:--:--  1300HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:55:06 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "cookie: identity=xduer" -v -ihttp://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> cookie: identity=xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:55:10 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:55:10 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "cookie: xduer=true" -v -i htTp://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> cookie: xduer=true
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:55:53 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1166      0 --:--:-- --:--:-- --:--:--  1166HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:55:53 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: identity=xduer" -v -ihttp://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: identity=xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:56:18 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:56:18 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookies: identity=xduer" -v -                               i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookies: identity=xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:56:44 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1300      0 --:--:-- --:--:-- --:--:--  1300HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:56:44 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookies: xduer=true" -v -i hTtp://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookies: xduer=true
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:57:02 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:57:02 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer=true" -v -i htTp://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer=true
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:57:07 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:57:07 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: $identity=xduer" -v -                               i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: =xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:57:50 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:57:50 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: identity=xduer" -v -ihttp://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0>GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: identity=xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:57:56 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 182
< Content-Type: text/html
<
{ [182 bytes data]
100   182  100   182    0     0   1290      0 --:--:-- --:--:-- --:--:--  1290HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:57:56 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 182
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" -v -i http://3                               9.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Accept: */*
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 13:59:59 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1467      0 --:--:-- --:--:-- --:--:--  1467HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 13:59:59 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: text/php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: text/php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:01:28 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1624      0 --:--:-- --:--:-- --:--:--  1624HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:01:28 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:01:57 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1467      0 --:--:-- --:--:-- --:--:--  1467HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:01:57 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: documents/php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: documents/php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:02:06 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1624      0 --:--:-- --:--:-- --:--:--  1624HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:02:06 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: application/php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: application/php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:03:09 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1467      0 --:--:-- --:--:-- --:--:--  1467HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:03:09 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: */php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: */php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:04:58 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1467      0 --:--:-- --:--:-- --:--:--  1467HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:04:58 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept:  */*\r\n" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept:  */*\r\n
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:05:59 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1467      0 --:--:-- --:--:-- --:--:--  1467HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:05:59 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept:  */*" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept:  */*
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:06:15 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1635      0 --:--:-- --:--:-- --:--:--  1635HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:06:15 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept:  text/php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept:  text/php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:09:17 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1458      0 --:--:-- --:--:-- --:--:--  1458HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:09:17 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0*Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:10:53 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1624      0 --:--:-- --:--:-- --:--:--  1624HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:10:53 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:11:10 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1832      0 --:--:-- --:--:-- --:--:--  1832HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:11:10 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: php/php" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: php/php
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:11:25 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1635      0 --:--:-- --:--:-- --:--:--  1635HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:11:25 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:11:54 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1717      0 --:--:-- --:--:-- --:--:--  1717HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:11:54 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Charset: UTF-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Charset: UTF-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:13:21 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1707      0 --:--:-- --:--:-- --:--:--  1707HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:13:21 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Charset: UTF-8" -v -i http://39.108.11.206:10012/

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Charset: utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Charset: utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:16:33 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1914      0 --:--:-- --:--:-- --:--:--  1914HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:16:33 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Charset: utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Charset: utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:17:05 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1900      0 --:--:-- --:--:-- --:--:--  1900HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:17:05 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: text/html; charset=UTF-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: text/html; charset=UTF-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:19:57 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1900      0 --:--:-- --:--:-- --:--:--  1900HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:19:57 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: text/html; charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: text/html; charset=utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:20:04 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1900      0 --:--:-- --:--:-- --:--:--  1900HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:20:04 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: text/html;charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: text/html;charset=utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:20:40 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   2144      0 --:--:-- --:--:-- --:--:--  2144HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:20:40 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: php;charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: php;charset=utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:20:49 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1900      0 --:--:-- --:--:-- --:--:--  1900HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:20:49 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: PHP;charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: PHP;charset=utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:27:04 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1914      0 --:--:-- --:--:-- --:--:--  1914HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:27:04 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: application/x-www-form-urlencoded;charset=UTF-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: application/x-www-form-urlencoded;charset=UTF-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:29:16 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   2144      0 --:--:-- --:--:-- --:--:--  2144HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:29:16 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: text/html; charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: text/html; charset=utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:30:09 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1707      0 --:--:-- --:--:-- --:--:--  1707HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:30:09 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Content-Type: charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Content-Type: charset=utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:30:50 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1900      0 --:--:-- --:--:-- --:--:--  1900HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:30:50 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:31:30 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1914      0 --:--:-- --:--:-- --:--:--  1914HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:31:30 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:31:41 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1900      0 --:--:-- --:--:-- --:--:--  1900HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:31:41 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP;charset=utf-8" -v -i http://39.108.11.206:10012/      *   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP;charset=utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:32:04 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 229
< Content-Type: text/html
<
{ [229 bytes data]
100   229  100   229    0     0   1635      0 --:--:-- --:--:-- --:--:--  1635HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:32:04 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 229
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "charset=utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:32:25 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1717      0 --:--:-- --:--:-- --:--:--  1717HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:32:25 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Encoding: utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Encoding: utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:35:14 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1914      0 --:--:-- --:--:-- --:--:--  1914HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:35:14 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: utf-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: utf-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:36:15 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 268
< Content-Type: text/html
<
{ [268 bytes data]
100   268  100   268    0     0   1717      0 --:--:-- --:--:-- --:--:--  1717HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:36:15 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 268
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:37:04 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 316
< Content-Type: text/html
<
{ [316 bytes data]
100   316  100   316    0     0   2025      0 --:--:-- --:--:-- --:--:--  2025HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:37:04 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 316
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: Chinese" -v -i http://39.108.11.206:10012/
> ^C

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: Chinese" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
> Accept-Language: Chinese
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:37:48 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 316
< Content-Type: text/html
<
{ [316 bytes data]
100   316  100   316    0     0   2025      0 --:--:-- --:--:-- --:--:--  2025HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:37:48 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 316
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: zh" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
> Accept-Language: zh
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:38:28 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 316
< Content-Type: text/html
<
{ [316 bytes data]
100   316  100   316    0     0   2241      0 --:--:-- --:--:-- --:--:--  2241HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:38:28 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 316
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: cn" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
> Accept-Language: cn
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:38:53 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 316
< Content-Type: text/html
<
{ [316 bytes data]
100   316  100   316    0     0   2241      0 --:--:-- --:--:-- --:--:--  2241HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:38:53 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 316
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: cn" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
> Accept-Language: cn
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:38:58 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 316
< Content-Type: text/html
<
{ [316 bytes data]
100   316  100   316    0     0   2257      0 --:--:-- --:--:-- --:--:--  2257HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:38:58 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 316
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: Chinese" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
> Accept-Language: Chinese
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:39:09 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 316
< Content-Type: text/html
<
{ [316 bytes data]
100   316  100   316    0     0   2025      0 --:--:-- --:--:-- --:--:--  2025HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:39:09 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 316
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: zh" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
> Accept-Language: zh
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:39:55 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 316
< Content-Type: text/html
<
{ [316 bytes data]
100   316  100   316    0     0   2241      0 --:--:-- --:--:-- --:--:--  2241HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:39:55 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 316
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$ curl --header "Referer: www.xidian.edu.cn" --header "User-Agent: moectf_browser" --header "X-Forwarded-For: 127.0.0.1" --header "Cookie: xduer" --header "Accept: PHP" --header "Accept-Encoding: UTF-8" --header "Accept-Language: zh-CN" -v -i http://39.108.11.206:10012/
*   Trying 39.108.11.206...
* TCP_NODELAY set
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--
  0* Connected to 39.108.11.206 (39.108.11.206) port 10012 (#0)
> GET / HTTP/1.1
> Host: 39.108.11.206:10012
> Referer: www.xidian.edu.cn
> User-Agent: moectf_browser
> X-Forwarded-For: 127.0.0.1
> Cookie: xduer
> Accept: PHP
> Accept-Encoding: UTF-8
> Accept-Language: zh-CN
>
< HTTP/1.1 200 OK
< Date: Fri, 06 Sep 2019 14:41:00 GMT
< Server: Apache/2.4.7 (Ubuntu)
< X-Powered-By: PHP/5.5.9-1ubuntu4.14
< Vary: Accept-Encoding
< Content-Length: 443
< Content-Type: text/html
<
{ [443 bytes data]
100   443  100   443    0     0   3141      0 --:--:-- --:--:-- --:--:--  3141HTTP/1.1 200 OK
Date: Fri, 06 Sep 2019 14:41:00 GMT
Server: Apache/2.4.7 (Ubuntu)
X-Powered-By: PHP/5.5.9-1ubuntu4.14
Vary: Accept-Encoding
Content-Length: 443
Content-Type: text/html

First of all, you must come from XiDian University<br>Second, you have to use moectf_browser<br>Third, you have to visit from the localhost<br>Fourth, your identity must be xduer<br>Fifth, client can only accept PHP documents<br>Sixth, we only allow UTF-8 encoding<br>Finally, we are only allowed to use Chinese.<br>Congratulations on having a good understanding of HTTP HEADERS and finally getting flag:<br>moectf{Reward_y0u_For_per3everAnce}
* Connection #0 to host 39.108.11.206 left intact

rever@DS-10001-RX MINGW64 ~
$
```

## 备注

来自MoeCTF 2019

为什么不把这个东东写进题解里呢? 因为我觉得有必要纪念一下... 因为我做完这题, 看见`flag`里那个`preseverance`, 我差点把键盘摔了...... 得... 心疼键盘... 不舍得摔... 进了协会摔出题人好了 : )

更新: 然鹅并不敢 (
