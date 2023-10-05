---
layout: post
title: "windows apache 配置虚拟主机"
date: 2011-5-13
wordpress_id: 411
comments: true
tags: ["apache", "my-own", "project", "windows", "虚拟主机"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="在windows中配置apache虚拟主机。采用单ip多端口映射。" />
<meta name="_su_keywords" content="windows,apache,配置,虚拟,主机,多端口,单ip" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="windows,apache,配置,虚拟,主机,多端口" />
<meta name="views" content="363" />
如果我的只有一个ip要在同一个机器上有两个或者多个网站，那就需要端口映射了。
例如ip为192.168.1.122 端口号为80，需要有一个网站a.com
ip为192.168.1.122 端口号为8080，需要有一个网站b.com
在windows/system32/driver/etc中修改hosts 添加
192.168.1.122 a.com
192.168.1.122 b.com
在apache的conf文件夹中，修改httpd.conf找到Listen关键词
然后照样子修改
Listen 80
Listen 8080
然后去conf\extra文件夹下，修改httpd-vhosts.conf文件。添加：
<Directory "c:/yoursitdir">
   AllowOverride FileInfo
   Order allow,deny
   Allow from all
</Directory>

<VirtualHost *:8080>
    ServerAdmin webmaster@a.com
   	DocumentRoot "c:/yoursitedir"
    ServerName a.com
    ServerAlias www.a.com
    CustomLog "logs/a.com-access.log" common
    ErrorLog "logs/a.com-error.log"
</VirtualHost>

<Directory "c:/yoursitdir2">
   AllowOverride FileInfo
   Order allow,deny
   Allow from all
</Directory>

<VirtualHost *:80>
    ServerAdmin webmaster@b.com
    DocumentRoot "c:/yoursitedir2"
    ServerName b.com
    ServerAlias www.b.com
    CustomLog "logs/b.com-access.log" common
    ErrorLog "logs/b.com-error.log"
</VirtualHost>
注意在DocumentRoot "c:/yoursitedir2"和<Directory "c:/yoursitdir2">中路径的最后不能有"/".
然后重启apache即可。

