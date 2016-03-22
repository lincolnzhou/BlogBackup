title: "常用指令"
date: 2016-03-22 20:18:55
tags: [Command]
---
# Git
```
# 获取远程仓库最新版到本地
git fetch

# 从远程仓库获取最新版并merge到本地
git pull

# 将本地代码推送到远程仓库
git push <远程主机名> <本地分支名>:<远程分支名>

# 添加所有改动的已跟踪文件和未跟踪文件（不管是修改的还是新添加的都会添加到一个commit）
git add -A

# 添加一个commit并且附带说明
git commit -m "xxxxxxx"

# 还原某一个修改的文件到最新版,地址替换[path_file]
# 特别是调试服务器代码时，哈哈，一般人都懂的！
git checkout [path_file]
```

<!--more--> 

# Linux|Mac CLI
```
# 查看当前终端机下的所有程序，grep指查找[search string]
ps -aux | grep "[search string]"
```

**该文章会持续更新，方便自己也方便他人学习！！！**