---
layout: post
title: '用java通过odbc连mysql'
date: 2006-08-13 11:44
comments: true
tags: ['java','mysql','数据库']
---

**首先**装一个mysql的驱动程序：`mysql-connector-odbc-3.51.12-win32.zip` 解压点击setup即可，可能会碰到与java运行时有冲突，点击yes即可。 

下一步下载一个`mysql-connector-java-3.1.11-bin.jar`，把它放在java－sdk的lib库里。

在管理工具的odbc数据源里用户DSN选项卡里添加：mysql odbc 3.51

Driver在connector中login选项卡添加：
```txt
DataSourceName：user（这里是程序要使用到的名字）

server：localhost

user：root

password：123

Database：user（这里的名字要和mysql中的要使用的数据库名字一样）

```

点击TEST，测试成功后点确定即可...

写如下程序进行测试：

```java
public static void main(String[] args)  
{  
    Connection connection = null;  
    Statement statement;  
    ResultSet resultSet;  
    // 加载驱动程序以连接数据库  
    try  
    {  
        Class.forName("sun.jdbc.odbc.JdbcOdbcDriver");//这个驱动不能是其他的..  
        connection = DriverManager.getConnection("jdbc:odbc:user","root","123");
        //user是data Source ，root是用

        //户名，123是进入mysql的密码  
        System.out.println("open easy");  
        String query = "Select * from userrole";  
        statement = connection.createStatement();  
        resultSet = statement.executeQuery( query );  
        connection.close();  
        System.out.println("close easy");  
        
    }  
    // 捕获加载驱动程序异常  
    catch ( ClassNotFoundException cnfex )  
    {  
        System.err.println(  
        "装载 JDBC/ODBC 驱动程序失败。" );  
        cnfex.printStackTrace();  
        System.exit( 1 ); // terminate program  
    }  
    // 捕获连接数据库异常  
    catch ( SQLException sqlex )  
    {  
        System.err.println( "无法连接数据库" );  
        sqlex.printStackTrace();  
        System.exit( 1 ); // terminate program  
    }  
    catch (Exception e)  
    {  
        System.out.println(e.toString());  
    }
}
```


在运行中输入：`net start mysql`
以启动mysql服务

运行程序测试即可。

