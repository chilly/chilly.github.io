---
layout: post
title: "build website in ubuntu"
date: 2012-02-11 14:31
comments: true
tags: web,site,build,ubuntu,linux,php
categories:
- knowledge
---
This article will tell you how to build a website in ubuntu system. First you should install apache and php. These two tools can help you to build a small website, like "hello world" websit:D   
{% blockquote %}
// run the following commands
sudo apt-get install apache2 php5 libapache2-mod-php5
/etc/init.d/apache2 restart
{% endblockquote %}
#### important path    
Then you should remember the following paths:   
{% blockquote %}
 * /etc/init.d/apache2 
 * /var/www 
 * /etc/apache2/apache2.conf
 * /etc/apache2/conf.d
 * /etc/apache2/sites-enabled
{% endblockquote %}
  Apache's default document root is /var/www on Ubuntu, and the configuration file is /etc/apache2/apache2.conf. Additional configurations are stored in subdirectories of the /etc/apache2 directory such as /etc/apache2/mods-enabled (for Apache modules), /etc/apache2/sites-enabled (for virtual hosts), and /etc/apache2/conf.d    
#### Test your php
```
vi /var/www/hello.php
```
```
<?php
phpinfo();
?>
```
Then call this file in browser (http://localhost/hello.php)
DONE:D
