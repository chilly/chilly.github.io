---
layout: post
title: "web config permission of files and dirs"
date: 2012-01-15 23:29
comments: true
tags: 
categories:
- knowledge
---
permission of files: 0644
permission of dirs:  0755
if not, you will not see the web page. Or your site may have an exploit.
配置文件的权限为0644, 配置dir的权限为0755,这样即可以让用户访问到，又不会有太大的安全漏洞。
