---
layout: post
title: 'java手记---静态和非静态'
date: 2006-07-26 09:44
comments: true
tags: ['java','static','non-static','class']
categories:
- [learn, java]
---

构造函数不能声明为静态。

关于静态与非静态的方法   MainClass.java  
```java
class sub  
{  
    String a;  
    int num;  
    sub()  
    {  
        a = "Hello";  
        num = 3;  
    }  
}

public class MainClass  
{  
    String t;  
    sub s;  
    ///这样写没有错误。  
    MainClass()  
    {  
        s = new sub();  
        t = "hhh";  
    }  
    public static void main(String []arg)  
    {  
        MainClass mc = new MainClass();  
        System.out.println(mc.s.a);  
    }  
}
```
  
下面寫法有錯誤：
```java

public class MainClass  
{  
  String t;  
  sub s;  
  ///这样写有错误。  
  public static void main(String []arg)  
  {  
    s = new sub();  
    System.out.println(s.a);  
  }  
}


```


如果这样写那么必须使s成为static变量  
如果使用static方法则其中使用的变量要么是static的自身类成员，要么是自身类。如果是自身类的话，  
通过自身类调用的类成员可以不是static

纯过程函数一定要加static。（不使用对象的）  
```java
class Test  
{  
int num = 3;  
static void show(Test a)/////////////////如果想在main中直接使用函数名得以实现就必须加static  
{  
System.out.println(a.num);  
}  
}  
//如果在main中这样实现：  
Test out = new Test();  
out.num = 10;  
out.show(out); //This is very important!  
```

原来的函数可以不使用static关键词

非静态函数必须使用"引用. * "。  
静态函数不需加。

只有类的成员和类的函数前可加static，本地变量不可以加static关键词

函数参数列表中即使没有参数也不能加void如show(void). This is wrong.  
类中静态变量可以用"类名.静态变量名"代替"引用.静态变量名".//这样更易懂。

一切都从静态代码开始。  

一个*.java文件先执行public类中的内容:但同样遵循先执行静态类成员缺省赋值,再执行其他的静态代码.  
一个类被导入的时候,其静态成员只能被载入一次.  
静态块和静态类成员变量效果相同(装载时间相同)都是初始化用的.  
一个块和一个静态块效果并不相同.一个块可以使用静态或非静态的变量.但是static块就必须使用static变量.  
否则有可以会出现Compile Error.原因是可能一个类装载的时候先装载静态成员然后才是成员变量.  
这就有可能出现变量未定义的错误.

一个块和static块都可以初始化成员.

一个static是属于类的而不应该是属于对象的.

final的方法不能被override.即不能被任何子类改变.TestAAndB.java , TestB.java

private的方法不能被子类使用.其余的方法均可以被子类所调用.  
```java
TestB b = new TestB();  
TestAAndB ab = b;  
ab.b();////compile  error  
```

即使把子类当成用超类的引用也不能使用这个private方法.  
如果此时b中有b()方法的重写也不能编译通过.

如果把子类当成超类的引用.那么只能使用超类中的非private方法.当然这些方法可能会被子类超越.  
那么这个引用会自动调用子类的超越后的方法.

