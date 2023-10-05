---
layout: post
title: "新浪Oauth"
date: 2012-01-17 00:11
comments: true
tags: [新浪,oauth,OAuth,callback]
categories:
- knowledge
---
<meta name="description" content="这篇文章是关于新浪Oauth认证上的一些经验">
新浪Oauth好像是上Oauth2.0了。不过也支持OAuth 1.0a.但是新浪的OAuth 1.0a是冒牌的。也就是说，应该是1.0～1.0a或者是真正到1.0 alpha.
因为这个Oauth不支持oauth_callback_confirm参数，也就是在request_token之后就仅仅返回oauth_token和oauth_secret.因为没有这个oauth_call_confirm,
也就无法区分Oauth到底是1.0的还是1.0a的。另外如果你真的照着OAuth1.0a的流程与新浪的接口整合。就会发现在获取authorized_token，还是要传给新浪
oauth_callback,否则新浪真的不知道callback到哪里去，于是新浪就展示给你获取oauth_verifier到页面,也就是授权码页面。   

{% pullquote %}
另外如果你真的加了oauth_callback,加入你的callback中没有参数，那谢天谢地，你的程序可以跑得很好。如果你的callback中有一个参数，那也谢天谢地，
你的程序可以跑得良好。如果你的callback中有更多到参数，那就别想跑得好了。因为新浪在某些情况下会丢参数。即使你将oauth_callback中的值整个用urlEncode编码一下（再使用替换个别字符转为Oauth特殊编码），也是有可能给你丢参数的。    
{" 所以请记住：国内就真的没有什么Oauth1.0a. "}
另外传说腾讯Oauth的oauth_callback的特殊值不是oob, 而是null.    
所以最简单有效到方法是将oauth_callback值中的'&'替换为'||'，将'='替换为'|'.然后在callback之后的页面进行反编码。就可以得到自己想要到参数了。
新浪是从Oauth1.0进行改版到Oauth1.0a的。其实就仅仅做了oauth_verifier的生成。然后就真的啥事请都没有做。  
{% endpullquote %}

