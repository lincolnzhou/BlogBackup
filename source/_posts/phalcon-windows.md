title: windows下安装phalcon
date: 2016-02-24 13:41:53
tags: [Phalcon]
---
# 开发环境
- wamp php5.5.12
- window7

注：示例机器上php安装目录为D:\Program Files\wamp\bin\php\php5.5.12

# 安装
windows下需下载phalcon扩展文件php_phalcon.dll，下载地址为[这里](https://phalconphp.com/en/download/windows)，选择对应的PHP版本文件

将php_phalcon.dll放置在php\ext文件夹中

修改php安装目录下的php.ini，在extension区域添加extension=php_phalcon.dll

**特别注意！！！一定要注意！！！**
需要修改apache目录下的php.ini，在extension区域添加extension=php_phalcon.dll

因为网页运行时，apache读取的是它安装目录下的php.ini，cli模式下的php读取的是php安装目录下的php.ini（安装phalcon-devtool尤其要注意这一点）