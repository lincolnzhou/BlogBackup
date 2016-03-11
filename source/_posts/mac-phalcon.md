title: Mac下安装phalcon
date: 2016-02-27 12:06:30
tags: [Phalcon]
---
# 开发环境
- Mac OS X EI Capitan 10.11.2
- php5.5.30

<!--more--> 

# 安装
```
git clone git://github.com/phalcon/cphalcon.git
cd cphalcon/build
sudo ./install
```
若遇到如下问题：
```
/usr/include/php/ext/pcre/php_pcre.h:29:10: fatal error: 'pcre.h' file not found
#include "pcre.h"
```
解决方案如下：
```
brew install pcre
find / --name pcre.h
```
找到pcre.h，然后将这个文件拷贝到／usr/include文件件下，再执行install就可以成功安装phalcon
