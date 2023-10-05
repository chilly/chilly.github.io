---
layout: post
title: "上传大文件"
date: 2012-05-16 23:45
comments: true
tags: upload,上传,nginx,resin,apache,spring,文件 
categories:
- knowledge
---
今天解决了上传大文件问题。这个问题对于简单的web程序基本是不用关注的。不过需要考虑http超时和socket超时等情况。对于nginx代理跳转的web程序，需要在nginx中设置client_max_body_size 和 proxy_read_timeout这两个地方。前一个参数是http body size,例如你要上传10M的文件，你这里需要设置11M或者更大，因为http body中不光有文件的二进制信息。后者是设置proxy server与后端server的连接后等待返回的时间。例如后面使用的是JBoss那就是与JBOSS的连接。

确定问题比较容易。如果一个请求发生，但是后端Server没有打印access log.那就说明请求还没有到后端server，需要检查代理服务器的设置。如果请求写入access log,但是没有被server后端的webapp打印出log,一般说明在后端的server处挂掉，需要检查后端server的相应配置。当然也可能是webapp自己直接做了限制，例如设置如下类的参数   

```
org.springframework.web.multipart.commons.CommonsMultipartResolver
```

这里设置后会导致在`doFilter()`之后parseRequest时挂掉。如果看到报类似SizeLimit exception或者 max upload file exception,大概就是这里的问题。

最后要检测app代码中是否也有这些限制。以及检查client端socket timeout等情况。以下代码是针对client socket提前超时设置的。    
```
HttpParams params = new BasicHttpParams();
HttpConnectionHttpParams.setConnectionTimeout(params,60000); // 连接超时1分钟
HttpConnectionHttpParams.setSoTimeout(params, 60000); // socket 读取写入超时
DefaultHttpClient client = new DefaultHttpClient(params);
```


