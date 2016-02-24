title: Mac下终端显示设置
date: 2015-10-22 12:10:40
tags: [Mac,终端]
---
## 修该终端前面的中文
一般默认显示“xxxxDeMacBook－Pro”

**修改bashrc**

```
sudo vim /etc/bashrc
```
<!--more--> 

如下图所示，
先用＃注释PS1='\h:\W \u\$ '，再换新行新增PS1='\w \$ '，然后:wq!强制保存退出


{% asset_img bashrc.png bashrc %}

最后重启终端就OK啦