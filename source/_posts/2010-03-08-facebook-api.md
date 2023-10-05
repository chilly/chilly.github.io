---
layout: post
title: "Facebook API"
date: 2010-3-8
wordpress_id: 97
comments: true
tags: ["api", "facebook", "php", "see-hear", "structure"]
categories:
- code
- program
---
<meta name="_edit_last" content="1" />
<meta name="views" content="887" />
Now I should do some work in investgate Facebook API. The Facebook API can be called as the following format( I use php as an example):


{% codeblock %}&lt;?php
/**
* Set the configuration settings for Facebook
*/
$facebook_config['debug'] = false;
$facebook_config['api_key'] = '&lt;your_api_key&gt;';
$facebook_config['secret_key'] = '&lt;your_secret_key&gt;';
/**
* include the Facebook client library
*/
require_once('&lt;path_to_client_library&gt;/facebook.php');
/**
* Set up the facebook object
*/
$facebook = new Facebook($facebook_config['api_key'],
$facebook_config['secret']);
/**
* Ensure the user has logged on to Facebook
*/
$user = $facebook-&gt;require_login();
/**
* Make an API call to call get a user's friends using the PHP library's
* library
*/
$friends = $facebook-&gt;api_client-&gt;friends_get();
echo "&lt;pre&gt;Friends:" . print_r($friends, true). "&lt;/pre&gt;";
?&gt;{% endcodeblock %}


The structure of the above codes is:
<ul>
	<li>set api_key and secret_key by your own</li>
	<li>send facebook_config to Facebook class and create an object of Facebook.</li>
	<li>call some method of this object and get some data of facebook which you want</li>
	<li>using those data to create an application</li>
</ul>
In technology, facebook uses RESTful technology, such as web services. facebook api transform data in JSON or XML format. And FBML( facebook Markup Language ) is used to combine and present data coming from developers' application servers. There are some technologic points: first, APIs should be packet into web services. Second, developers' application data should be presented in web pages. But I don't think there is new technology here.

The flow diagram of calling facebook api is:

<a href="http://chillyc.info/wp-content/uploads/2010/03/08/facebook-api/the-flow-diagram-of-calling-facebook-api.jpg"><img class="aligncenter size-medium wp-image-98" title="the flow diagram of calling facebook api" src="http://chillyc.info/wp-content/uploads/2010/03/08/facebook-api/the-flow-diagram-of-calling-facebook-api-290x300.jpg" alt="" width="290" height="300" /></a>

Chinese version is here:

<strong><strong><a href="../wp-content/uploads/2010/03/Facebook_API.doc" target="newUpload">Facebook_API.doc</a></strong></strong>
