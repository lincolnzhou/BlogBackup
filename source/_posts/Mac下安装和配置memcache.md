title: Mac下安装和配置memcache
date: 2015-12-11 21:09:14
tags: [Mac,memcache]
---
# 安装memcache
安装之前先查找一下，看有没有
```
brew search memcache
```
返回的结果
libmemcached    memcache-top    memcached   memcacheq
可以发现都与memcache有关，其中第一个是客户端，第三个为服务器端

<!--more--> 

## 安装服务器端memcached
```
b
```rew install memcached
安装日志过程如下：
{% asset_img memcache-install.png memcache-install %}

可以看出memcached安装依赖libevent，并且安装在/usr/local/Cellar下

启动服务
```
sudo memcached -m 1024 -p 11211 -d -u root
```
|选项|描述|
|------|------|
|-d| 以守护进程方式运行memcached|
|－m|设置memcached可以使用的内存大小，单位M|
|-l|设置监听的IP地址，默认为本机|
|-p|设置监听端口号，默认为11211|
|-u|指定用户，如果当前为root，需要此参数指定用户|

使用telnet测试是否启动成功
```
LincolnhoudeMBP:memcached lincolnzhou$ telnet localhost 11211
Trying ::1...
Connected to localhost.
Escape character is '^]'.
```

# 安装客户端memcache
采用源码编译安装，下载地址为[这里](http://pecl.php.net/package/memcache) ，下载解压进入目录后
```
sudo phpize
```
若出现如下日志信息
grep: /usr/include/php/main/php.h: No such file or directory
grep: /usr/include/php/Zend/zend_modules.h: No such file or directory
grep: /usr/include/php/Zend/zend_extensions.h: No such file or directory
Configuring for:
PHP Api Version:        
Zend Module Api No:     
Zend Extension Api No:  

解决办法：
首先要安装Xcode，其中路径中存在版本号，看系统的版本号即可
```
sudo ln -s /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/usr/include/ /usr/include
```
执行之后可能会出现如下错误
ln: /usr/iclude: Operation not permitted
可去查找/usr/下是没有include这个文件夹的，还有mkdir include创建不了文件夹，一样报Operation not permitted

链接失败原因，10.11加强了系统保护/usr没有操作权限，解决方案如下：
> 按下开机键时即刻按住 command R（“R”字母键），中间的苹果标志及进度条出现后放开按键，等待恢复安装界面和 “OS X 实用工具”
窗口出现后，点击顶部菜单栏的 “实用工具”，在其下拉菜单点选运行 “终端”，在终端闪动字符的位置直接输入“csrutil disable”
并回车，重新启动电脑。

## autoconf安装
由于phpize依赖于autoconf，没有安装的话会出现如下错误
```
Cannot find autoconf. Please check your autoconf installation and the
$PHP_AUTOCONF environment variable. Then, rerun this script.
```

采用brew安装，并且添加系统链接
```
brew install autoconf
brew link --overwrite autoconf
```

## 编译memcache
一定要下载stable版本的！！！不然PHP会连接不上memcache
```
sudo ./configure
sudo make
sudo make install
```
编译好的 memcache.so 一般被安装到如下目录：
Installing shared extensions: /usr/lib/php/extensions/no-debug-non-zts-xxxxxx/

之后在php.ini中配置并且加载so文件：
extension=/usr/lib/php/extensions/no-debug-non-zts-xxxxxx/memcache.so

注：xxxxxx不要直接使用，具体看自己的目录名，同时记得重启apache，不然配置会无效

打开phpinfo()页面，查看memcache是否加载成功
{% asset_img php-memcache.png php-memcache %}





