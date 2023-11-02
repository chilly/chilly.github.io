---
layout: post
title: "借助有道云笔记写blog"
date: 2013-07-15 22:12
comments: true
tags: [有道云笔记,blog,图片]
categories:
- knowledge
---
## 0. 前言
 &nbsp; &nbsp; &nbsp; &nbsp;下面这个blog介绍一下如何借助有道云笔记来写blog. 这里的blog当然是公开的非私密的。帐前卒 的 blog是借助Octopress搭建在github上，使用的markdown写成。由于github对于免费的成员只提供400MB的空间。对于图文并茂的blog来说，这点空间有点相形见绌。所以急需找到一个能提供图片分享的地方。图片分享的地方很多：  

1.  当然很多图床网站可以提供的(当然也有可能不小心被封了)  
1.  小bbs站(游客身份也可以查看，同时这个小站提供上传图片功能)可以试试[有道云笔记官方论坛](http://notebbs.youdao.com '有道云笔记论坛')  
1.  网易云相册啥的  
1.  各种其他的blog站点，写一篇blog上传，里面加入图片。然后把图片地址放入markdown中  
1.  本文重点要介绍**使用有道云笔记 分享**方式添加图片。不过这一种不如前面几种简单实用。但是可以分享25M以下的任意图片，并且没有数量限制，空间还在不断增长。  

## 1. 分享你的图片
  &nbsp; &nbsp; &nbsp; &nbsp;首先打开有道云笔记的客户端，然后创建一篇新的笔记，然后点击**'分享链接'**
![分享链接 图片](https://photos.google.com/album/AF1QipO83FbqnkVdDXVs-hFs12xqxg6Myp1ec1vWf9M/photo/AF1QipM5pI75erNeSbyGdSKhsneXCMlYr6HUlchcR0E)

## 2. 获取图片地址
  &nbsp; &nbsp; &nbsp; &nbsp;打开浏览器(chrome, 火狐,IE均可) 这里只介绍firefox. 点击[分享链接](http://note.youdao.com/share/?id=d54e01fec212f37df16b5bcd032a83dd&type=note)。然后在调出firebug. 使用它，选中你要获取的图片。然后就可以看到下面这段html. 注意其中的src属性。没错，这就是你的图片url了。
    <img data-inited="true" src="http://note.youdao.com/yws/public/resource/d54e01fec212f37df16b5bcd032a83dd/25FBFC3EADAD4EDEB46F9AD6F147C0D9" data-media-type="image">
![firebug界面](http://note.youdao.com/yws/public/resource/d54e01fec212f37df16b5bcd032a83dd/AB825A2093E648FC8210B144318EDDFA)

## 3. 在markdown中编辑
 &nbsp; &nbsp; &nbsp; &nbsp; 接下来要做的就是在markdown中插入图片. 
    ![图片名] (刚才从有道云笔记分享获取的url)

## 4. 发布
 &nbsp; &nbsp; &nbsp; &nbsp; 这里就没有什么好说的了。**Deploy it!**  
 另外编写markdown需要注意的:   


1.  段首空格需要加 &amp;nbsp; 另外如果需要多个空格，那么每个&amp;nbsp之间都要有空格。  
1.  如果写为段落列表那么需要在每个列表后加入一个空行  

>       [空行]
>       1.  aaa
>       1.  bbb 
