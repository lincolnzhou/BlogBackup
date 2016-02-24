title: Hexo博客搭建
date: 2015-10-22 11:42:48
tags: [hexo]
---
一直想来搭建一个博客的，最后还是选定使用Markdown来编写，比较方便，不用搭建数据库等步骤了。最后选定使用[Hexo](https://hexo.io)

## 环境准备
使用brew轻松搞定！！！

Node

```
brew install node
```

Git

```
brew install git
```

hexo
```
npm install -g hexo-cli
```

<!--more-->

##  初始化博客
```
hexo init .
npm install
```

##  基本指令
### generate
```
hexo generate [-w] [-d]
```
### server
```
hexo server [-p 4000]
```

## 草稿