title: Linux下mysql数据库备份及crontab定时备份
date: 2016-04-14 19:56:45
tags: [linux, mysql, 定时任务]
---
> 由于系统操作失误或者系统故障导致数据会丢失，数据的丢失会是致命的打击。所以数据库备份是为了防止此类事发生，做好数据库备份至关重要，不可忽视！！！
当然从备份数据中恢复也取决于备份恢复方案了。此文主要讲述的是使用mysqldump命令和crontab定时备份机制

# 备份数据库-mysqldump
mysqldump主要是对数据库表和数据进行备份

```bash
# mysqldump指令备份某数据库（备份表结构和数据）
# -h host地址
# -u 用户名
# -p 密码
# dbname 数据库名称
mysqldump -hip -uroot -ppassword dbname > dbname_dump_20160414_201900.sql
```

<!--more--> 

# 定时备份-crontab
```bash
# root用户创建执行脚本
mkdir -p /root/mysql_dump
cd /root/mysql_dump
touch mysql_back.sh
chmod 755 mysql_back.sh

# 编辑备份脚本
vim mysql_back.sh

# 以下为备份脚本的内容
#!/bin/sh
# File: /root/mysql_dump/mysql_back.sh
DB_NAME="dbname"
DB_USER="username"
DB_PASS="passwoed"
MYSQL_BIN_DIR="/usr/local/mysql/bin"
BACKUP_DIR="/root/mysql_dump"
DATE=`date +%Y%m%d_%H%M%S`

mkdir -p BACKUP_DIR
$MYSQL_BIN_DIR/mysqldump --opt --single-transaction -h192.168.5.33 -ut_user -p$DB_PASS $DB_NAME \
> $BACKUP_DIR/$DB_NAME.dump_$DATE.sql

# 如果备份文件很大，可以考虑使用gzip压缩一下
$MYSQL_BIN_DIR/mysqldump --opt --single-transaction -h192.168.5.33 -ut_user -p$DB_PASS $DB_NAME | gzip \
> $BACKUP_DIR/$DB_NAME.dump_$DATE.sql

# 备份的文件名格式
# dbname.dump_20160414_180001.sql
```

由于使用mysqldump命令会出现如下提示信息：Warning: Using a password on the command line interface can be insecure.
由于mysql5.6以后出于对安全性的考虑，不建议使用密码在命令行里使用，使用--single-transaction这个命令选项就可以继续正常备份，即使有提示信息

# 添加定时任务列表
```bash
# 添加到crontab
crontab -e

# 每天18:00定时执行脚本
0 18 * * * /bin/sh /root/mysql_dump/mysql_back.sh

# 重启脚本
service crond restart

# 查看定时任务执行情况
tail -f /var/log/cron
```

# crontab格式说明
```bash
cat /etc/crontab 
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```
可以发现crontab的执行周期由五个部分组成：分钟、小时、天、月、每周几

**PS：此教程适用于个人和小型网站进行数据库备份**