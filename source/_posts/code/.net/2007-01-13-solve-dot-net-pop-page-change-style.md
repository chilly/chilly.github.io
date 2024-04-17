---
layout: post
title: '解决.net中服务器控件弹页面而引发的样式改变'
date: 2007-01-13 19:10
comments: true
tags: ['服务器','.net','javascript']
---

在C＃的后台代码中写入
```C#

this.ClientScript.RegisterStartupScript(  typeof  (Page),  "JavaScript",  "<script>window.close('StateChangeApply.aspx','','left=0,top=0,scrollbars=yes,width=600,height=500')</script>" );  

```


可以防止页面变化

