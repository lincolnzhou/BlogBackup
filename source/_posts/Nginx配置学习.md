title: nginx配置学习
date: 2016-03-18 20:30:30
tags: [nginx]
---
## 安装
**环境**
Ubuntu Kylin 14.04

下载安装包[nginx-1.8.1][1]
```
tar -xf nginx-1.8.1.tar.gz
cd nginx-1.8.1
./configure
sudo make
sudo make install

# 将nginx加入用户指令中,之后可以直接使用nginx命令
sudo cp /usr/local/nginx/sbin/nginx /usr/local/bin/nginx

```

<!--more--> 

若遇到如下问题：
1. PCRE library
```
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
```
到PCRE官网http://www.pcre.org/下载最新版本
```
tar -xf pcre-8.38.tar.bz2
cd pcre-8.38
./configure
sudo make
sudo make install
```

## 配置
默认安装目录：
/usr/local/nginx

**启动问题**
libpcre.so.1无法加载
```
/usr/local/webserver/nginx/sbin/nginx: error while loading shared libraries: libpcre.so.1: cannot open shared object file: No such file or directory
```
解决方案
```
sudo ln -s /usr/local/lib/libpcre.so.1 /lib
sudo ln -s /usr/local/lib/libpcre.so.1 /lib64
```

## 编译echo模块
模块源码地址:https://github.com/openresty/echo-nginx-module
源码下载地址：https://github.com/openresty/echo-nginx-module/archive/v0.58.tar.gz

我们将nginx之后需要编译安装的模块放入modules文件夹中
```
cd /usr/local/nginx
sudo mkdir modules
cd modules
wget https://github.com/openresty/echo-nginx-module/archive/v0.58.tar.gz
tar -xf v0.58.tar.gz

# 进入nginx安装源码文件夹,此步骤指令视自己情况而定
sudo ./configure --add-module=/usr/local/nginx/modules/echo-nginx-module-0.58
sudo make

# 此步骤一定不能make install 否则覆盖以前的配置
# make之后在objs文件夹下会出现nginx文件
# 我们先将之前配置的nginx文件备份一次，免得出错
sudo cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak

# 再将新编译的nginx文件放入sbin文件夹
sudo cp nginx /usr/local/nginx/sbin/nginx

# 重启一下
sudo /usr/local/nginx/sbin/nginx

# 此指令可以查看是否已经安装成功
sudo /usr/local/nginx/sbin/nginx -V

# nginx version: nginx/1.8.1
# built by gcc 4.8.4 (Ubuntu 4.8.4-2ubuntu1~14.04.1) 
# configure arguments: --add-module=/usr/local/nginx/modules/echo-nginx-module-0.58
# 出现以上信息表示安装成功
```

## 编译lua-nginx-module模块
模块源码地址:https://github.com/openresty/lua-nginx-module
源码下载地址：

我们将nginx之后需要编译安装的模块放入modules文件夹中
```
# 由于依赖于LuaJIT，所以要先安装LuaJIT
wget http://luajit.org/download/LuaJIT-2.0.4.tar.gz
tar -xf LuaJIT-2.0.4.tar.gz
cd LuaJIT-2.0.4.tar.gz
make && sudo make install

export LUAJIT_LIB=/usr/local/lib
export LUAJIT_INC=/usr/local/include/luajit-2.0
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH

#重新加载一下共享库文件！！！
sudo ldconfig

sudo wget https://github.com/simpl/ngx_devel_kit/archive/v0.3.0rc1.tar.gz
sudo tar -xf v0.3.0rc1.tar.gz

wget https://codeload.github.com/openresty/lua-nginx-module/tar.gz/v0.10.2.tar.gz
tar -xf v0.10.2.tar.gz

sudo ./configure --with-debug --add-module=/usr/local/nginx/modules/echo-nginx-module-0.58 --add-module=/usr/local/nginx/modules/lua-nginx-module-0.10.2 --add-module=/usr/local/nginx/modules/ngx_devel_kit-0.3.0rc1

sudo make -j2
#再重复nginx echo最后重新编译nginx过程
```

## 变量
### echo 使用
基本用法：
```
server {
    listen 8080;
    location /test {
        set foo 'hello';
        echo "$foo world"
    }
}
```

若后面没有空格，比如$fooworld,这种情况nginx会认为是变量$fooworld，可使用${foo}world来表示

变量一旦定义,其可见范围是整个nginx配置,其它地方引用的话（之前没有使用set），不会报错,但是表示的是一个空字符串

### 内部跳转
内部跳转时,变量是共享的
```
server {
    listen 8080;

    location /foo {
        echo "foo = [$foo]";
    }

    location /test {
        set $foo hello;
        #echo "${foo}world";
        echo_exec /foo;
    }
}
```
由此可以看出,Nginx变量值容器的生命期是与当前正在处理的请求绑定的,而与location无关

### 内置变量
```
location /test {
    echo "uri =	$uri";
	echo "request_uri =	$request_uri";
}

curl 'http://localhost:8080/test'
uri	= /test
request_uri	= /test

curl 'http://localhost:8080/test?id=1'
uri	= /test
request_uri	= /test?id=1

curl 'http://localhost:8080/test/hello%20world?a=3&b=4'
uri	= /test/hello world
request_uri	= /test/hello%20world?a=3&b=4
```
**$uri**
获取当前请求的	URI(经过解码,并且不含请求参数)

**$request_uri**
获取请求最原始的URI	(未经解码,并且包含请求参数)

**以$arg_开头的变量**

$arg_xxxx xxxx为请求参数名,大小写不敏感
```
location /test {
    echo 'name: $arg_name';
    echo 'value: $arg_value';
}

curl 'http://localhost:8080/test?name=1&value=12321'
name = 1
value = 12321
```

此变量是在运行到此处才去计算变量值

而使用set指令，比如
```
 set $b '$a, $a';
```
这种是主动计算

### 父子请求
```
location /main {
    set $var 'main';
    echo_location /foo;
    echo_location /bar;
    echo 'main: $var';
}

location /foo {
    set $var 'foo';
    echo 'foo: $var';
}

location /bar {
    set $var 'bar';
    echo 'bar: $var';
}

$ curl 'http://localhost:8080/main'
foo: foo
bar: bar
main: main
```
这种情况下父子请求，变量是单独的变量容器，不会受到彼此的影响

**ngx_auth_request**
在这种指令的子请求变量是共享的下，很容易被子请求修改而不好定位问题所在，需要注意

```
location /main {
    set $var 'main';
    auth_request /foo;
    echo 'main: $var';
}

location /foo {
    set $var 'foo';
    echo 'foo: $var';
}

$ curl 'http://localhost:8080/main'
foo: foo
```

**内置变量**
```
location /main {
    echo "main args: $args";
    echo_location /sub "a=1&b=2";
}

location /sub {
    echo "sub args: $args";
}
```
agrs获取各自请求的参数，父子之前不影响

## 配置指令执行顺序
nginx请求处理阶段有11个,先学习最常用的三个阶段rewrite,access,content

set是在rewrite阶段

echo是在content阶段

所以在同一location配置中，所有set总是在echo之前执行

```
location /test {
    set $a 36;
    echo $a;
    set $a 56;
    echo $b;
}

curl "http://localhost:8080/test"
56
56
#此例说明了set在echo之前执行
```

[1]: http://nginx.org/download/nginx-1.8.1.tar.gz
