---
layout: post
title: 'java手记---package'
date: 2006-07-26 09:56
comments: true
tags: ['java','interface','class']
categories:
- [learn, java]
---

程序包就是命名的类组。很像目录。是名称空间。类名在其中必须唯一  
  
sgsware程序包-raytracing子程序包-Mailbox.java中的Mailbox.class  
其全名为sgsware.raytracing.Mailbox  
程序包名为小写。  

```bash
c:>   javac -d /路径 文件名.java        
编译程序包中的java文件  
这时把编译出的*.class 放在这个路径下。  
  
c:>   java -classpath 路径 程序包名.文件
使用此文件。  
  
package 名称；这样编译时就把这个文件的.class放在了这个名称中  
```

```java
package mypackage;  
import mypackage.*;/////程序员永远不用这样定义。
```
  
一个类自动导入其程序包中的其他类。  
  
在程序包中的类其default类型(包访问类型)为非public型  
在同一程序包中的类在其他类中的实例可以调用自己类中的任何类型。  
  
一个受保护的方法可以由属主类的任何子类中被超越。即使子类在其他的程序包中。  
可以被子类的当前实例访问  
```java
package cc;  
public class Cctest  
{  
protected int  id;  
protected void show()  
{  
System.out.println(id);  
}  
}  
  
package dd;  
import cc.Cctest;  
public class C extends Cctest  
{  
float salary;  
protected void show(C A)      //在子类中被超越。  
{  
System.out.println(A.id);///////子类实例可用。  
System.out.println(A.salary);  
}  
}//////////this is right.  
```

一个受保护的数据可以被属主类的任何子类中调用。但不能被超类中的实例访问。  
但可以被子类的实例访问。  
```java
package dd;  
import cc.Cctest;  
public class C extends Cctest  
{  
float salary;  
protected void show(Cctest A)  
{  
System.out.println(A.id);//this is wrong.  
}  
}
```  



  
超类版本访問 | 子类版本访问 | 
-------|--------|
public | public | 
protected | public    protected | 
default |public    protected    default | 
private |public    protected    default    private | 
  
把main放在超类中。这样以后可以用超类的引用子类实例的超越的所有类型的函数。  
  
终结类不能子类化，终结方法不能被超越。终结变量得到初始化后不能被修改。 

```java

private void show()///////////在超类中。  
{  
System.out.println("a = " + a + " b = " + b + " c = " + c);  
}  
public void show()///////////在子类中。  
{  
System.out.println(add());  
}  
PrivateClass pri1 = new SubPrivate();//////////超类引用子类。  
pri1.show();////////////调用的是超类中的show().  
```

但如果超类中的show()是非private得则调用的是子类中的show().  
接口如果是公用的则在任何地方的任何代码中都可以使用。  
接口如果不是公用的则在它的程序包中使用。它不能是private/protected  
接口中可以不声明public,但实现的时候必须有public声明.  
接口的引用只能用接口中声明的方法。 

```java
package Interface;  
interface OverrideB  
{  
void b();  
void c(int s);  
}  
~~~~~~~~~~~~~~~~~~~~
package Interface;  


interface A  
{  
    void b();  
    int c(int s);  
}  

~~~~~~~~~~~~~
package Interface;  
public class Override implements OverrideB,A  
{  
    public void b()//这段代码是正确的.但软件工程不要求这样做.  
    {  
    
    }  
    public int c(int s){//这里不正确.两个接口中的方法可以函数签名和返回值都相同.但不能只有返回值不同.  
    return 3;  
    }  
}  

```

一个接口可以继承多个接口使用extends关键字 
```java
interface AandB extends A,B  
{  
  
}
``` 
  
一个接口不能继承一个虚拟类  
```java
public abstract class AbClass  
{  
   abstract void draw();  
}  
package Interface;  
interface AbInterface extends AbClass  
{  
  
}  

```
  
虚拟类可以实现接口.  

