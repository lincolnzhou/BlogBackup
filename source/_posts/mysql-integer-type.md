title: 【mysql系列 - 数据类型】 整数类型
date: 2016-06-04 16:10:30
tags: [mysql]
---
> 最近一直在学习mysql，所以就打算将自己学习笔记分享给大家

大多数情况下，在数据库设计中比较随意，字段的定义随心所欲，会导致在性能和维护上成本很大，这是致命的！

# 基础知识
## 字节与位
1 byte(字节) = 8 bit(位)

也就是说，一个字节表示最大的数为2^8 - 1= 255

## 二进制转十进制
整数二进制用数值乘以2的幂次依次相加
101 = 1 \* 2^2 + 0 \* 2^1 + 1 \* 2^0 = 5

## 存储
字段在定义数据类型时就可以确定存储大小，如果你使用了错误类型，虽然使用起来没有问题，但是白白浪费了大量硬盘存储空间和IO消耗

PS：如果你是土豪，请忽略这一点~~~

# 整数类型
如下表为各个整数类型的范围：

| 类型 | 字节(位) | 范围(有符号) | 范围(无符号) | 
| --- | --- | --- | --- |
| TINYINT | 1(8) | -128~127 | 0~255 |
| SMALLINT | 2(16) | -32768~32767  | 0~65535 |
| MEDIUMINT | 3(24) | -8388608~8388607  | 0~16777215 |
| INT | 4(32) | -2147483648~2147483647 | 0~4294967295 |
| BIGINT | 8(64) | -9223372036854775808~9223372036854775807 | 0~18446744073709551615

n表示存储位数

有符号：
-2^(n-1) ~ 2^(n-1) - 1

无符号：
0 ~ 2^n - 1

# 无符号和有符号
有符号可以用来表示正数或负数，所以它是要付出代价的，会将存储的第一位拿来做符号位
比如：
```
# tinyint 有符号
0       0000000
符号位   数据位
(0/1)

范围：-2^7(128) ~ 2^7 - 1(127)

# tinyint 无符号
00000000
数据位

范围：0 ~ 2^8 - 1(255)
```

懂了吗？

# 测试脚本

```mysql
-- 设置数据严格检查
-- 未做严格验证时，可以插入到数据库，但是取值为范围最大值127
-- NO_ENGINE_SUBSTITUTION
-- STRICT_TRANS_TABLES
set session sql_mode="STRICT_TRANS_TABLES"

-- tinyint(1)
create table t_tinyint_1 (
	`id` tinyint(1) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into t_tinyint_1 values(127); 
-- 127

insert into t_tinyint_1 values(128);
-- ERROR 1264 (22003): Out of range value for column 'id' at row 1

-- tinyint(1) unsigned
create table t_tinyint_1_u (
	`id` tinyint(1) unsigned DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into t_tinyint_1_u values(-1); 
-- 0

insert into t_tinyint_1_u values(127); 
-- 127

insert into t_tinyint_1_u values(255); 
-- 255

insert into t_tinyint_1_u values(256); 
-- ERROR 1264 (22003): Out of range value for column 'id' at row 1
```

## int(11)和int(9)区别
11和9的区别主要在于显示的数组长度，而不是表示数值的大小

来看下面的脚本：
```mysql
-- int(11)
create table t_int_11 (
	`id` int(11) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into t_int_11 values(2147483647);
-- 2147483647

insert into t_int_11 values(2147483648);
-- ERROR 1264 (22003): Out of range value for column 'id' at row 1

-- int(9)
drop table if exists t_int_9;
create table t_int_9 (
	`id` int(9) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert into t_int_9 values(1);
-- 1

insert into t_int_9 values(2147483647);
-- 2147483647

insert into t_int_9 values(2147483648);
-- ERROR 1264 (22003): Out of range value for column 'id' at row 1
```

从上面结果可以看出int(11)和int(9)表示的数值范围是一样的

既然11和9表示的数值范围一样，那为何要这样定义呢？马上来揭晓

```mysql
-- 添加zerofill选项，显示位数不足补0
ALTER TABLE `t_int_9`
MODIFY COLUMN `id` int(9) zerofill UNSIGNED NULL DEFAULT NULL FIRST ;
```

你再去数据库里看，数值前面多了很多0

# 场景
## 性别
范围：男、女和未知（大家懂得）

可以使用0,1,2来表示，这里使用tinyint就够了，int白白浪费了3个字节的空间

## 斗鱼房间号
范围：> 0

老司机们，房间号不说你都明白，一串数字就代表着某个主播，脑海里是不是已经想起谁了？

其实房间号在设计上也是有讲究滴，为嘛要用int呢？首先来看看无符号int表示最大范围为4294967295，大约42亿！！！我们再来看看mediumint表示的最大范围为16777215（大约1600万）
很显然1600万是无法满足业务发展的，说不准半年下来id就被用光了，所以选择int来存储，在近5年都很难突破。
