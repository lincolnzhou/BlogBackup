title: PHP几种字符串拼接方式比较
date: 2016-03-27 21:45:41
tags: [PHP]
---
PHP大致有如下几种拼接方式：
1. 直接用"."
2. 用".="
3. 先压入数组，再用数组拼接函数

本文章用两种场景来模拟

# 运行时间函数
``` php
function get_tm() {
    list($usec, $sec) = explode(" ", microtime());
    return ((float)$usec + (float)$sec);
}
```

<!--more--> 

# 第一种场景
> 将字符串"SmallDuck"拼接100000次

## 第一种方式
``` php
$start = get_tm();
$start_m = memory_get_usage(true);
$str = '';
for($i = 1; $i <= 100000; $i++) {
    $str = $str . 'SmallDuck';
}
$end = get_tm();
$end_m = memory_get_usage(true);
echo 'first solution(time):' . ($end - $start) . "s\n";
echo 'first solution(memory):' . ($end_m - $start_m) . " byte\n";
```

## 第二种方式
``` php
$start = get_tm();
$start_m = memory_get_usage(true);
$str = '';
for($i = 1; $i <= 100000; $i++) {
    $str .= 'SmallDuck';
}
$end = get_tm();
$end_m = memory_get_usage(true);
echo 'second solution(time):' . ($end - $start) . "s\n";
echo 'second solution(memory):' . ($end_m - $start_m) . " byte\n";
```

## 第三种方式
``` php
$start = get_tm();
$start_m = memory_get_usage(true);
$table = '';
for($i = 1; $i <= 100000; $i++) {
    $table[] = 'SmallDuck';
}
$str = join('', $table);
$end = get_tm();
$end_m = memory_get_usage(true);
echo 'third solution(time):' . ($end - $start) . "s\n";
echo 'third solution(memory):' . ($end_m - $start_m) . " byte\n";
```

## 运行结果
first solution(time):2.9791338443756s
first solution(memory):1048576 byte
second solution(time):0.0062241554260254s
second solution(memory):786432 byte
third solution(time):0.02541708946228s
third solution(memory):15728640 byte

由此可以看出，使用"."方式拼接字符串效率最低，其次是"数组"方式，最优是".="方式

# 第二种场景
> 逐行读取某文件内容，将这些内容拼接起来；这里我们使用数组来模拟，比如：将100000个字符串"SmallDuck"存储在某变量table中，之后逐行读取拼接

## 第一种方式
``` php
$start = get_tm();
$start_m = memory_get_usage(true);
$data = [];
for($i = 1; $i <= 100000; $i++) {
    $data[] = 'SmallDuck';
}

$str = '';
foreach ($data as $item) {
    $str = $str . $item;
}
$end = get_tm();
$end_m = memory_get_usage(true);
echo 'first solution(time):' . ($end - $start) . "s\n";
echo 'first solution(memory):' . ($end_m - $start_m) . "\n";
```

## 第二种方式
``` php
$start = get_tm();
$start_m = memory_get_usage(true);
$data = [];
for($i = 1; $i <= 100000; $i++) {
    $data[] = 'SmallDuck';
}

$str = '';
foreach ($data as $item) {
    $str .= $item;
}
$end = get_tm();
$end_m = memory_get_usage(true);
echo 'second solution(time):' . ($end - $start) . "s\n";
echo 'second solution(memory):' . ($end_m - $start_m) . "\n";
```

## 第三种方式
``` php
$start = get_tm();
$start_m = memory_get_usage(true);
$data = [];
for($i = 1; $i <= 100000; $i++) {
    $data[] = 'SmallDuck';
}

$table = [];
foreach ($data as $item) {
    $table[] = $item;
}
$str = join('', $table);
$end = get_tm();
$end_m = memory_get_usage(true);
echo 'third solution(time):' . ($end - $start) . "s\n";
echo 'third solution(memory):' . ($end_m - $start_m) . "\n";
```

## 运行结果
first solution(time):3.3972160816193s
first solution(memory):15990784
second solution(time):0.028364896774292s
second solution(memory):15990784
third solution(time):0.052934885025024s
third solution(memory):26214400


由此可以看出，使用"."方式拼接字符串效率最低，其次是"数组"方式，最优是".="方式

**之所以写这篇文字是因为学Lua时，大规模拼接字符串使用数组拼接比字符串相加快，测试了PHP运行环境，瞬间让我有种看"."操作符实现源码的冲动了，应该比较吊，特别是内存回收的问题**
