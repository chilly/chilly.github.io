---
layout: post
title: 'IIS一个配置错误allowDefinition'
date: 2006-07-30 20:22
comments: true
tags: ['internet','iis','工具']
---

# Question：配置错误。  

## 说明:  
在处理向该请求提供服务所需的配置文件时出错。请检查下面的特定错误详细信息并适当地修改配置文件。  

## 分析器错误信息  :
在应用程序级别以外使用注册为  `allowDefinition='MachineToApplication'` 的部分是错误的。导致该错误的原因可能是在`IIS`中没有将虚拟目录作为应用程序进行配置。

# Answer 解决方案：

在网上没有找到现成的答案。自己动手解决了引起它的一个原因。

在管理工具－>Internet信息服务中网站－>默认网站点右键：

新建－>虚拟目录

点击下一步，输入一个别名a点击下一步，把自己项目所在的路径填上，允许权限随便填填即可，点击下一步，OK。

