---
layout: post
title: 'GridView点击删除后弹对话框再删除'
date: 2007-01-11 08:20
comments: true
tags: ['javascript','command', 'js']
categories:
- [code,js]
---

添加一个模版列，编辑模版列，并加入一个linkButton ，在onClientClick中加入`JavaScript:returnconfirm('你确定要删除该行记录吗？');`

在html中找到那个模版列在其中加入这个属性`CommandArgument='<%# Bind("ID") %>'`

选中LinkButton的事件在Command项中写delete然后在后台代码中这样实现：

```js
protected  void  delete(object  sender, CommandEventArgs e)  {  

    string  ID  =  e.CommandArgument.ToString();  //  得到该行的ID值（如果你下面的调用是通过ID来删除的话）  
    if  (deviceInfoBLL.Delete(ID)) {  //  这里是调用下层的删除方法  
      

        init();  //  初始化页面  

        Label_warning.Text  =  "  删除成功，编号为：  "  \+  ID;  
    } else  {
        Label_warning.Text  =  "  删除失败，编号为：  "  \+  ID;  
    }

 }

```

完成

