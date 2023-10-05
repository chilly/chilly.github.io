---
layout: post
title: "Wordpress Error: PHP is not running solution"
date: 2011-5-13
wordpress_id: 417
comments: true
tags: ["error", "open-source", "php", "project", "solution", "wordpress"]
categories:
- knowledge
---
<meta name="_edit_last" content="1" />
<meta name="_su_description" content="solution of an error occured in wordpress.
Error: 
Error: PHP is not running" />
<meta name="_su_keywords" content="Error,php,not,wordpress,solution" />
<meta name="_su_rich_snippet_type" content="none" />
<meta name="_su_title" content="Error,php,not,wordpress,solution" />
<meta name="views" content="898" />
<meta name="_wp_old_slug" content="error-php-running" />
why?
you don't have php? your php does not start? your apache server is shutdown?
(uninstall all apache, mysql and php if you will install xampp. Be careful.)
you can download xampp software to solve your problem. But if you are sure that your php is running. put a php file "hello.php" into xampp/htdocs, and type:
<blockquote>http://localhost/hello.php</blockquote>
hello.php is like:



<blockquote>
echo "hello";
</blockquote>



if the web page presents "hello", it shows your php is running.
if you want to configure other dir to put your web, you should see my another blog, <a href="http://chillyc.info/project/windows-apache-%E9%85%8D%E7%BD%AE%E8%99%9A%E6%8B%9F%E4%B8%BB%E6%9C%BA">Here</a>.
Ok, if your php is running. Then config database and wordpress.
<a href="http://codex.wordpress.org/Installing_WordPress">Here is instruction.</a>
Then you should open brower and type:


<blockquote>http://ip:port/wp-admin/install.php</blockquote>

if you open install.php in your local computer with brower, it will report 
<blockquote>Error: PHP is not running</blockquote>
in your brower, the url address does not prefix with "http://"
