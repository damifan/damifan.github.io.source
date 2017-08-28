---
title: MongoDB设置用户名密码及权限
date: 2017-08-25 16:30:42
tags:
  - mondodb
  - centos
categories:
  - 数据库
thumbnail: /2017/08/24/CentOS-7%E4%B8%8B%E5%AE%89%E8%A3%85MongoDB/mongodb.jpg
---

现在需要创建一个帐号，该账号需要有grant权限，即：账号管理的授权权限。`注意一点，帐号是跟着库走的，所以在指定库里授权，必须也在指定库里验证(auth)`。

我们结下来就在admin 库中创建一个admin用户，并给他读写权限。
管理员用户是存在admin中system.users中
在`./mongo`中运行:
```
> use admin
switched to db admin
> show collections
system.users
system.version
> 
```
<!-- more -->

会列出本库的collections

1.在admin库中，添加用户并授权
```
use admin;
db.createUser(
   {
     user: "admin",
     pwd: "123456",
     roles:
     [
       {
         role: "readWrite",
         db: "admin"
       }
     ]
    }
  );
 ```
2.在admin库中验证,**用户在那里创建，就在哪里验证！！！**
```
 use admin;
 db.auth('admin', '123456')
                       
```
这样我们就完成在admin库中创建admin用户了。

说明：
user：用户名
pwd：密码
roles：指定用户的角色，可以用一个空数组给新用户设定空角色；在roles字段,可以指定内置角色和用户定义的角色。role里的角色可以选：
```
Built-In Roles（内置角色）：
1. 数据库用户角色：read、readWrite;
2. 数据库管理角色：dbAdmin、dbOwner、userAdmin；
3. 集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
4. 备份恢复角色：backup、restore；
5. 所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
6. 超级用户角色：root  
   // 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
7. 内部角色：__system
```
具体角色：
```
Read：允许用户读取指定数据库
readWrite：允许用户读写指定数据库
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
root：只在admin数据库中可用。超级账号，超级权限