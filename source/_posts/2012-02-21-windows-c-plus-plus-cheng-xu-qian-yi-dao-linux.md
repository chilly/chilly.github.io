---
layout: post
title: "windows c++程序迁移到linux"
date: 2012-02-21 20:16
comments: true
tags: windows, c++, linux, 移植, 迁移，程序 
categories:
- knowledge
---
今天把程序从windows上迁移到了linux上。主要遇到到问题是: makeFile, lib库, compile error, 编码 encoding.     
####make file   
这里其实可以使用eclipse中到cdt插件，然后就可以从eclipse中写c++。挺方便的，同时也解决了make file的问题。因为创建一个c++ 或者 c project，eclipse会自动创建一系列的makefile文件。所以让make file步骤简单无比。       
####lib库和include库   
这里真的要注意/usr/include和/usr/lib中是否有你想要到文件。当然如果你是纯c++代码，可以尝试下只使用/usr/include/c++. 当如除了-L libpath, 还有-llibname, 这里的libname其实是libXXXX.so中的XXXX. 不过如果不会写，这里还是会费些劲。还有include路径要使用`-I`,每一个路径前都要有一个`-I`. 另外还要在eclipse run configurate中的environment中填入LD_LIBRARY_PATH,这个是你要调用的lib库（这个lib库如果不在/usr/lib中，那么就要手工将路径填入到LD_LIBRARY_PATH变量里）。并且在.bashrc中写：
```bash
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:your_lib_path
```  
####编码
因为windows上最常用到中文编码是GBK,而文件编码最常用到是utf-16el.这里最有可能会出错。在eclipse中或者gcc直接编译，都最好转换为UTF-8编码。文件也需要是UTF-8的编码。否则就会报"程序有游离的XXX, 忽略空字符"等诡异的错误。[详见解决方法](http://chillyc.info/blog/2012/02/20/jie-jue-cheng-xu-zhong-you-you-chi-de-slash-xxx-hu-lue-kong-zi-fu/)    
####compile error
这个可能就多种多样了。不过有c/c++基础的，应该大部分都可以搞定了。   

       
       
如果使用eclipse, 直接build project就可以编译成功。然后找到main函数run就可以了。
