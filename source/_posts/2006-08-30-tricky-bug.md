---
layout: post
title: '无奈于BUG'
date: 2006-08-30 09:58
comments: true
tags: ['jbuilder','javascript','编译器']
categories:
- [knowledge, js]
- [code]
---

几个星期前接到了一个系统。那个系统其实已经发布使用，只是要让我们改改系统环境。按道理来说也没有什么。
并且交给我们项目的人说这个系统以前经过严格的测试。于是我认为应该改动的地方也不多，bug可以说是微乎其微。

而且几个星期过去，当这个系统改完配置后再此运行的时候，我才发现：几乎每一个页面都有几个bug。再加上系统本来带有的bug，整个系统的bug应该超过30几个。
没有想到过去经过严格测试的系统，其bug数量竟然如此之多。想想可能是自己移植系统的时候处错了吧。调来过去的系统看看，bug还是那么多。看样不是自己移植的问题。

还有些bug很奇怪.举个简单的例子，如下代码：
```js

<script language="JavaScript">  
<!--  
    function verifyInput(){  
    strUserName = document.adduserForm.name.value;  
    if ((strUserName == ""))  
    {  
    alert("用户名不为空");  
    
    return false;  
    }

    return true;

    }

\-->

</script>

<form method="get" name="adduserForm" action ="/userlogin/controller"
onsubmit="return verifyInput()" >


```


我页面提交的时候要判断true还是false才能提交。结果是不论false还是true都能提交表单。。

最后还是在源script代码中添加了
```js

document.adduserForm.name.value="";  
document.adduserForm.name.focus();  
return false;

```


才使得页面不能提交。

有时候自己在想：

一个环境（windows）有bug，一个编译器（jvm）可能有bug，一个集成编译环境（jbuilder）可能有bug，一个数据库（mysql）也可能有bug，
那自己的编出来的程序如果真的是一个bug没有，恐怕这也是奇迹了。

