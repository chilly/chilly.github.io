---
layout: post
title: '2.javascript--第一天的源代码'
date: 2006-07-24 14:22
comments: true
tags: ['javascript','function','html']
categories:
- [code]
- [learn, javascript]
---

sa.js文件

```javascript
document.write("good day");
```



ex1.html 文件
```html
<html>

    <head>
    </head>

    <body>
        <pre>
            <script language = "javaScript">  
            //this is comment  
            document.writeln("hello");  
            var value1 = '100';  
            var value2 = "kill you";  
            var value3 = 342;  
            var value4 = 400;  
            var value5 = " world";  
            var value6 = (value5+value2).search("k");  
            var value7 = (value5+value2).replace("k",'c');  
            var myArray1 = new Array(5);  
            var myArray2 = new Array("A","B","C");  
            if(true)  
            {

            //document.write("hello");  
            document.writeln("hello");  
            }  
            document.writeln(value2);  
            document.writeln(value1);  
            document.writeln(value3);  
            document.writeln(value3+value4);  
            document.writeln(value2+value5);  
            document.writeln(value3+value5);  
            document.writeln(value6);  
            document.writeln(value7.big());  
            for(var i=0;i<5;i++)  
            {  
            myArray1[i] = i;  
            document.writeln(myArray1[i]);  
            }

            for(i=0;i<3;i++)  
            {  
            document.writeln(myArray2[i]);  
            }

            </script>  
            <noscript>  
            document.write("fefef");  
            </noscript>  
        </pre>
    </body>

</html>

```

ex2.html文件
```html
<html>

<head>
    <title>
        for test Array
    </title>
    <script language="javascript">
        function write() {
            document.writeln("看看");
        }

        function add(a, b) {
            document.writeln(a + b);
        }

        function returnValue(a) {
            return a * a;
        }

        function hello() {
            window.alert("hello");
        }
    </script>

    <head>

    <body>
        <pre>
            <a href = "#" onClick = "hello();">onClick</a>

            <a href = "javascript:hello();">javascript</a>  
            <script language = "javascript">

            var arr = new Array(4);  
            arr[0] = "c";  
            arr[1] = "h";  
            arr[2] = "a";  
            arr[3] = "d";

            for(var i=0;i<4;i++)  
            {  
            document.writeln(arr[i]);  
            }  
            document.writeln(arr.sort());

            var mystring = "a,x,d,e";  
            var arr1 = mystring.split(",");  
            for(i=0;i<4;i++)  
            {  
            document.writeln(arr1[i]);  
            }  
            //window.alert("hello");  
            //window.confirm("bye");  
            write();  
            add(4,6);  
            document.writeln(returnValue(5));

            </script>  
        </pre>
    </body>

</html> 
```

ex3.html文件
```html
<html>

    <head>
        <script>
            <!--  
            function warning() {
                window.alert("hello");
            }

            function firm() {
                return window.confirm("Open it?");
                window.setTimeout("firm()", 5000);
            }
            window.setTimeout("firm()", 5000);
            //
            -->
        </script>
    </head>

    <body onload="warning()">
        <script>
            var ok = firm();
            if (ok == true)
                document.writeln("Ok");
        </script>
    </body>

</html>
```


ex4.html文件
```html

<html>

<head>
    <script>
        <!--  
        function warning() {
            window.alert("hello");
        }

        function firm() {
            window.confirm("Open it?");
            window.setTimeout("firm()", 3000);
        }
        //
        -->
    </script>
</head>

<body onUnload="warning()">
    <script>
        var a = window.setTimeout("firm()", 3000);
        window.clearTimeout(a);
    </script>
</body>

</html>


```


ex5.html文件
```html
<html>

    <head>
    </head>

    <body>
        <script language="javascript">
            var b = document.URL;
            document.writeln(b);
            document.write("<p><strong>Dynamic Content</strong></p>");
        </script>
    </body>

</html>


```
