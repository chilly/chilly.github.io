---
layout: post
title: '删除一条数据库记录的解决方案'
date: 2007-01-11 09:24
comments: true
tags: ['数据库','delete','null']
---

在删除一条记录时有可能遇到一条记录已经被关联外键，那就应该将它其中的一个字段单独拿出来做标记，在程序中判断是否被删除，否则就真正的删除它。

使用`try{}catch(){}`方法。如果首次删除不成功，系统就会抛出异常，然后转到catch中，在其中的程序段中将其中的一个字段单独来做标记。

```java
public  bool  DeleteDevInfo(String  deviceID)  {  


    bool  delete  =  false  ;  

    try  {  

        try  {  

            delete  =  SQLHelper.ExecuteCommon(CommandType.Text, DELETE +  DEVICE_ID +deviceID,  null  );  
        }  catch  (Exception e)  {  

            return  SQLHelper.ExecuteCommon(CommandType.Text, UPDATE_DEVICEID +DEVICE_ID + deviceID,  null  );  
        }  

        return  delete;  

    }  catch  (Exception e)  {  
        throw  new  Exception("删除设备台账时失败!");  
    }  

}
```


这里的SQLHelp类似于petShop中的Uility层中的SQLHelp，封装了底层于数据库的交互

