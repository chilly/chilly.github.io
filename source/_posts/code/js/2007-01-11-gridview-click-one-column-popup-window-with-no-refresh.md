---
layout: post
title: 'GridView中点击一列无刷新弹窗口'
date: 2007-01-11 09:10
comments: true
tags: ['javascript','asp','脚本']
categories:
- [code,js]
---

不要在后台代码里的`Respose.write();`中写javascript脚本，这样有可能会影响页面显示的效果。

使用GridView，在其中添加模版列，在html中写入：
```html
    <asp:TemplateField  HeaderText  ="修改">  
    <itemtemplate>  
    <a  href="#"  onclick='<%#string.Format("GotoModifyDevice({0})",Eval("deviceID")) %>'>修改</a>  
    </itemtemplate>  
    </asp:TemplateField>
```


这里的`Eval("deviceID")`
中的deviceID必须是GridView中的一列的dataField。

在javascript脚本中写入：
```js
function  GotoModifyDevice(deviceID)  {  
    window.open("设备修改.aspx?deviceID=" + deviceID,"_blank","toolbar=no,height=500px,width=600px,resizable=yes,scrollbars=yes");  
}
```


