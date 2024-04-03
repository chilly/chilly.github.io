---
title: how to use tampermonkey
date: 2024-04-02 18:04:16
comments: true
tags: ['tampermonkey']
---

# 1. 下载油猴tampermonkey

首先去chrome webstore下载`tampermonkey`,就是众所周知的油猴。

这东西还是要从专门的渠道下载。因为如果你要做免登录脚本，那么用户名，密码不可能避免的需要被这些脚本知道。


如果是恶意制作的油猴插件，可能这些脚本就会被传送的特殊的服务器上去。用户名及密码就可能被拿到。


我这里最近在尝试学习油猴插件来制作某些网站的免登录脚本。



**具体方法如下:**

1. 先找到要登录的网站
1. 点击油猴的插件图标
1. 选择`create a new script...`
1. 进入后修改`@name` 和 `@match` 一个是这个脚本文件的名字，一个是脚本会在哪个url下执行。 @match支持`*`通配符
1. 然后在 `// Your code here...` 处编写代码 


# 2. 记录一下各个网站的登录脚本
## 1. xxl-job-admin
```js
    $(":text")[0].value="your name";
    $(":password")[0].value='your password';
    $(":checkbox")[0].checked=true;
    var loginButton = document.querySelector("button[type='submit']");
    loginButton.click();


```

## 2. gitlab
```js
    document.querySelector('#username').value="your name";
    document.querySelector('#password').value="your password";

    document.querySelector('.btn-confirm').click();
```

## 3. grafana

```js

    var userId = 'your name';
    var password = 'your password';
    var userInput = document.querySelector('input[name^=user]');
    var passwordInput= document.querySelector('input[name^=password]');
   
    var loginButton = document.querySelector('.css-3coq9d-button');

    userInput.value = userId;


    userInput.blur();


    setTimeout(()=>{


    passwordInput.value = password;

    passwordInput.blur();
    // grafana 是非常奇怪的，这里需要两次click, 缺少任何一次都无法正常登录
    setTimeout(()=>{
        
        loginButton.click();
        },1200);
    },1200);

    loginButton.click();

```


## 4. rancher
```js

// 这个rancher 需要等待 document load 出来。也就是说他渲染界面有点慢。
setTimeout(() => {


        var userInput = document.querySelector("div.labeled-input.edit input[type='text']");

        var passwordInput = document.querySelector("div.labeled-input.edit input[type='password']");

        var loginButton = document.querySelector("div.col.span-12.text-center button[type='submit']");

        var user = 'your name';
        var pass = 'your password';

        userInput.value = user;

        var event = document.createEvent('HTMLEvents');
        event.initEvent("input", true, true);
        event.eventType = 'message';
        // 调度事件
        userInput.dispatchEvent(event);

        passwordInput.value=pass;

        passwordInput.dispatchEvent(event);

        loginButton.click();
    }, 1000);

```


## 5. jenkins
```js
    document.querySelector("input[type='text']").value="your name";
    document.querySelector("input[type='password']").value='your password';
    document.querySelector("input[type='checkbox']").checked=true;
    var loginButton = document.querySelector("button[type='submit']");
    loginButton.click();

```


# 3. 总结
1. 有些非常简单，只需要`input`组件`value`正确, 点击`button`就可以发请求，但有些就需要控制组件失焦， 还有一些需要模拟动作。
1. 有些input非常容易获取到，可以通过id属性来找，有些就只能通过input子属性才能获取到。
1. 每个网站的界面做的都大同小异，但是写脚本仍然非常头疼。登录这部分没有标准。