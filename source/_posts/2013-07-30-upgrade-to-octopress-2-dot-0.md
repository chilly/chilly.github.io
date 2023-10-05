---
layout: post
title: "upgrade to octopress 2.0"
date: 2013-07-30 15:03
comments: true
tags: upgrade, octopress, 2.0, bugs, solution 
categories:
- knowledge
---

## 0. 前言

 &nbsp;&nbsp;&nbsp;&nbsp;最近在看Octopress的代码，然后想想可能Octopress要更新了。于是手贱了一下，把整个Octopress重新更新了一下。然后整个blog乱成一团了。所以说千万别更新，更新之前一定要git push保存一下进度。如果真的更新挂了，就再rollback.

<!-- more -->
## 1. fix bugs

 * Could not find fast-stemmer-1.0.1 in any of the sources   
   
 &nbsp;&nbsp;首先将 rubygems中的https://rubygems.org修改为 http://rubygems.org/， 这样就可以下载到fast-stemmer-1.0.1.    

 * Could not find sass-3.2.9 in any of the sources    
    
 &nbsp;&nbsp;再将http://rubygems.org修改为 https://rubygems.org/ 发现这个东西被封了。所以需要使用代理：   
        $ export http_proxy=http://username:password@your-proxy-site.com:[your_proxy_port]

 * _Let's Go on._ An error occured while installing rdiscount (2.0.7.3), and Bundler cannot continue. Make sure that `gem install rdiscount -v '2.0.7.3'` succeeds before bundling.   

  &nbsp;&nbsp;这个要用ruby 1.9.3来解决。那么来安装ruby 1.9.3吧。   

        rvm install 1.9.3
        修改.rvmrc
        修改.rvmrc使用的ruby版本。 rvm use 1.9.3
        当然可以退出该目录重新进入一遍。  
   

 * 继续走...发现ruby中的问题    

        /home/chico/.rvm/rubies/ruby-1.9.3-p0/lib/ruby/1.9.1/uri/common.rb:304:in `escape': undefined method `gsub' for 2012:Fixnum (NoMethodError)
        from /home/chico/.rvm/rubies/ruby-1.9.3-p0/lib/ruby/1.9.1/uri/common.rb:623:in `escape'

  &nbsp;&nbsp;这个问题是之前的markdown脚本，现在解析的有问题。有些不兼容了。解决方法是在plugins/sitemap\_generator.rb中把post.name打印出来，然后看最后是在哪个文件中卡死了。然后就修改那个文件的markdown文件。   


 * 然后终于把Octopress启动起来了。内牛满面呀！然后就发现页面全乱了..........   
  &nbsp;&nbsp;然后借小Y之力修改了css.然后就好了。当然你们没有小Y. 下面是那段修改的css. 修改sass/base/\_layout.scss


          #content {
             @extend .group;
        -    overflow: visible;
        -    margin-right: $sidebar-width-medium;
        +    overflow: hidden;
        +    margin-right: 0px!important;
        +    padding-right: 240px;
             position: relative;
            .no-sidebar & { margin-right: 0; border-right: 0; }

## 2. 最终页面

 新的页面是长这样子的：
![新页面](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/FD5A902AD9D743F6BC2AEA01498DD160)
 旧的页面是这样子的：
![旧页面](http://note.youdao.com/yws/public/resource/8a55ddd27d075e71ac8a8d14e6f58a41/34849B1E634C41C9AFB779AF43F0A946)
 因为那个收缩的tab是关联js的。所以该起来还是很困难。

## 3. 结论

  &nbsp;&nbsp;&nbsp;&nbsp;不要因为出了新版本就手贱去更新。否则修改更新问题时，贱手就废掉了。  
  &nbsp;&nbsp;&nbsp;&nbsp;_最后一句话: F**K Octopress 2.0 !!_

