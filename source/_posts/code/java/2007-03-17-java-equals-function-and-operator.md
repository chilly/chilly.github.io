---
layout: post
title: 'java中equals()与＝＝操作符'
date: 2007-03-17 19:31
comments: true
tags: ['equals','java','integer']
categories:
- [code,java]
---

一般来说`equals()`方法是比较两个对象值的。

例如比较两个`Integer`对象

```java
public  class  Equalival  {  

    public  static  void  main(String[] args) {  
        Integer n1  =  new  Integer(  47  );  
        Integer n2  =  new  Integer(  47  );  
        System.out.println(n1.equals(n2));  
    }
}
```


打印
```shell
true
```

//Thinking in Java中的例子

＝＝操作符是用来比较引用是否相等
```java

public  class  Equalival  {  
    public  static  void  main(String[] args)   {  
        Integer n1  =  new  Integer(  47  );  
        Integer n2  =  new  Integer(  47  );  
        System.out.println(n1  ==  n2);  
        System.out.println(n1  !=  n2);  
    }  
}

```


打印
```shell
false，true
```

感觉以上的例子并没有说明什么看看下面的这个String的例子：

```java

public  class  TestRef {  
    public  static  void  main(String[] args)   {  
        String s  =  new  String(  "a"  );  
        if  (s  ==  "a"  )  
            System.out.println(  "  true1  "  );  
        if  (s.equals(  "a"  ))  
            System.out.println(  "  true2  "  );  
        String ss  =  "a"  ;  
        if  (ss  ==  "a"  )  
            System.out.println(  "  true3  "  );  
        if  (ss.equals(  "a"  ))  
            System.out.println(  "  true4  "  );  
    }  
}  

```

这个打印
```shell
true2，true3，true4
```
因为`new String()`时在堆中生成了空间给`"a"`，对于第一次的`s=="a"`时，栈分配空间给`"a"`，并把`"a"`当成一个匿名的对象。赋值`ss="a"`，意味着在栈中分配了空间给`"a"`，并把`"a"`这个匿名的对象引用赋给`ss`。所以`s`与`"a"`的引用不等。而`"a"`再次出现在代码中时，编译器查找到先前的`"a"`对象，而不是再分配内存给`"a"`。所以`ss`与`"a"`的引用是相等的。但不管引用是否相等，其值都是`"a"`，这便看出了`equals()`的威力。

