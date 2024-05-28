---
title: csdn-blog-export
date: 2017-06-12 22:41:59
tags: ['csdn','blog','export','markdown','md']
categories:
- daily

---

因为感觉csdn的界面越来越乱了，是不是的还有各种广告。另外CSDN借工信部的名义强制要求输入手机号。像我这样根本没有手机的人，再也没有办法登陆了。所以还是把所有的blog迁移过来。从此我就再也不写csdn的blog.专心写我的这里的blog吧。


迁移工具从`git@github.com:gaocegege/csdn-blog-export.git`那边copy过来的。修改了一下code的生成地方和tags抽取。大家如果想将csdn的blog转换成markdown.可以参考我copy过来的这个工具。详细地址是: [csdn\_export](https://github.com/chilly/csdn_export)

先安装一下pip 
然后使用pip 安装一下 BeautifulSoup
```shell
sudo pip install BeautifulSoup
git clone https://github.com/chilly/csdn_export
cd csdn_export
./main.py -u [你的csdn的username] -f markdown
```
我之前的blog是blog.csdn.net/cctt\_1。所以命令为:
```shell
./main.py -u cctt_1 -f markdown
```

