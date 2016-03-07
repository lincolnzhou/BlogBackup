title: 我用爬虫一个小时爬取了Coding.net活跃用户数据
date: 2016-03-08 01:40:19
tags: [Coding.net,爬虫]
---
之前看到“伯乐在线”一篇文章介绍用PHP爬取知乎百万用户数据，之后才有这个想法，一直对爬虫比较感兴趣，所以决定自己开始动手模仿着写爬虫程序，而我的网站目标就是 [Coding.net](https://coding.net/) 

**Github项目地址**：https://github.com/lincolnzhou/coding-spider

看知乎上用python去写爬虫程序较多，反正PHP是世界上最好的语言，貌似大家都已经默认了吧，哈哈～～～不要撕逼哈，自己擅长什么语言就用什么语言

截止到2016-03-06 17:56，Coding.net的总用户数为184700，你肯定要问我怎么知道的吧？这个嘛，私聊我吧，我就告诉你

我用PHP写了一个多进程爬虫程序，只用了不到一个小时的时间，就抓取了Coding.net2万多用户，也就是说真正有价值的用户量其实只有这么多，剩下没有被爬到的很明显就是无价值的用户，我想里面肯定存在大量被机器人注册的帐号，哈哈，扯远了，还是开始我们的正题

<!--more--> 

先来简单的看一下爬取的数据截图：
**总人数** 
{% asset_img user-total-count.png 总用户数 %}
**部分用户信息截图**
{% asset_img user-info-1.png 用户信息 %}

{% asset_img user-info-2.png 用户信息 %}

## 爬虫程序设计
### 基本思路
爬虫程序先从一个入口进，这个入口使用了我自己的帐号lincolnzhou，首先读取帐号信息，然后读取关注和粉丝用户的个性后缀（GK），之后再读取关注和粉丝用户信息，重复以上操作即可爬取有效的账户信息

### 相关技术
1. PHP多进程，pcntl扩展
2. PDO数据库扩展
3. Curl模拟请求
4. Redis

### 程序目录说明
{% asset_img application.png 程序目录结构 %}
core 核心库
 － bootstrap.php 启动文件，设置环境和程序常量
 － config.php 配置文件，数据库配置及地区信息配置
 － curl.php Curl封装请求类
 － db.php 数据库类
 － log.php 日志类
 － predis.php Redis封装类
log 日志数据
result 数据分析程序
sql 数据库脚本
 － user.sql 用户数据表
 － user_tag.sql 用户标签表
function.php 封装爬虫常用方法，获取用户信息，获取关注用户信息，获取粉丝用户信息
get_spider_time.php 查询爬虫开始时间和结束时间
get_user_tag.php 获取网站用户所有标签信息
index.php 爬虫运行脚本

### 接口准备
Coding.net API文档地址：http://api-doc.coding.io/

爬虫使用到的接口:
**获取用户数据**：https://coding.net/api/user/key/{username}
**获取用户关注数据**：https://coding.net/api/user/friends/{username}
**获取用户粉丝数据**：https://coding.net/api/user/followers//{username}

PS：Coding.net真是贴心，API设计真是太棒了，相信它技术是杠杠的，不然我也不会这么容易爬取到

### 程序思路
1. 多进程抓取数据
2. Redis存储需抓取的用户个性后缀（GK）和已抓取的用户个性后缀（GK）
3. Mysql存储用户详细信息数据

首先将LincolnZhou压入Redis的request_queue队列中，通过$max_connect控制每次最大的进程数，也就是每组进程最大爬取用户数

之后将LincolnZhou推出request_queue队列，抓取自身的用户数据即调用saveUserInfo，抓取关注和粉丝用户信息，即调用getUserFriends和getUserFollowers

将关注和粉丝用户信息压入request_queue队列之前要做存在性判断，减少重复抓取用户数据，之后将自身压入already_get_queue

然后重复上述过程持续抓取用户数据，直到request_queue队列为空停止抓取

### 运行截图
有意思的是今天来运行爬虫程序的时候，发现不行了，获取数据都是失败，貌似Coding.net接口规则改了，不知道是不是防我，管他呢，数据是如下图：
{% asset_img ungzip.png 错误 %}

哈哈，这个貌似不难，查了查资料，并去看了Coding.net网站接口的响应头，发现多了一项Content－Encoding，嘻嘻，原来是要我gzip一下啦
{% asset_img response.png 请求头 %}

不得不说PHP是最好的语言，一句代码就可以搞定这个事，curl_setopt($ch, CURLOPT_ENCODING, 'gzip');

于是就可以愉快的运行了，运行图如下：
{% asset_img runtime-1.png 运行图 %}
{% asset_img runtime-2.png 运行图 %}
{% asset_img redis-1.png 运行图 %}
{% asset_img redis-2.png 运行图 %}

### 数据分析
别问我为毛要抓这些数据，理由很简单，我闲的蛋疼！我闲的蛋疼！我闲的蛋疼！重要的事说三遍

拿到了这些数据就可以好好分析一下啦，也可以拿来吹吹牛逼做大数据分析

以下是比较常见的数据分析：
1. 男女比例
2. 人群地区分布
3. 职业
4. 标签
5. Coding公司员工岗位，男女比，日访问量等等

若你还能想到好玩的统计，记得在评论中分享出你的统计想法～～～

下面就是用这些数据做出的一些有趣图表：
{% asset_img chart.png 统计图表 %}