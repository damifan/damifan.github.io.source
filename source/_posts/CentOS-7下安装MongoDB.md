title: CentOS-7下安装MongoDB.md
tags:
  - mondodb
  - centos
categories:
  - 数据库
date: 2017-08-24 14:25:00
thumbnail: /2017/08/24/CentOS-7%E4%B8%8B%E5%AE%89%E8%A3%85MongoDB/mongodb.jpg
---
**下载安装包**
MongoDB 提供了 linux 各发行版本 64 位的安装包，你可以在官网下载安装包。
下载地址：https://www.mongodb.com/download-center#community

{% asset_img mongodb_download.png 图1 mongodb_download%}
<!-- more -->
下载完安装包，并解压 tgz（以下是在CentOS 7 64位安装） 。

```
curl -O https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.4.7.tgz    # 下载
tar -zxvf mongodb-linux-x86_64-3.4.7.tgz                                   # 解压

mv  mongodb-linux-x86_64-3.4.7/ /usr/local/mongodb                         # 将解压包拷贝到指定目录
```
MongoDB 的可执行文件位于 bin 目录下，所以可以将其添加到 PATH 路径中：
```
export PATH=<mongodb-install-directory>/bin:$PATH
```
`<mongodb-install-directory>`为你MongoDB的安装路径。如本文的`/usr/local/mongodb`。
**eg:`export PATH=/usr/local/mongodb/bin:$PATH`**

**创建数据库目录**
MongoDB的数据存储在data目录的db目录下，但是这个目录在安装过程不会自动创建，所以你需要手动创建data目录，并在data目录中创建db目录。
以下实例中我们将data目录创建于根目录下(/)。 `注意：/data/db 是
MongoDB默认的启动的数据库路径(--dbpath)。`
```
mkdir -p /data/db
```
**命令行中运行 MongoDB 服务**
你可以再命令行中执行mongo安装目录中的bin目录执行mongod命令来启动mongdb服务。
注意：如果你的数据库目录不是/data/db，可以通过 --dbpath 来指定。
输入`./mongod` 显示如下即初步安装成功：
```
[root@localhost bin]# ./mongod
2017-08-24T15:36:24.034+0800 I CONTROL  [initandlisten] MongoDB starting : pid=20694 port=27017 dbpath=/data/db 64-bit host=localhost
2017-08-24T15:36:24.034+0800 I CONTROL  [initandlisten] db version v3.4.7
2017-08-24T15:36:24.034+0800 I CONTROL  [initandlisten] git version: cf38c1b8a0a8dca4a11737581beafef4fe120bcd
2017-08-24T15:36:24.034+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2017-08-24T15:36:24.034+0800 I CONTROL  [initandlisten] modules: none
2017-08-24T15:36:24.034+0800 I CONTROL  [initandlisten] build environment:
2017-08-24T15:36:24.035+0800 I CONTROL  [initandlisten]     distarch: x86_64
2017-08-24T15:36:24.035+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2017-08-24T15:36:24.035+0800 I CONTROL  [initandlisten] options: {}
2017-08-24T15:36:24.056+0800 I -        [initandlisten] Detected data files in /data/db created by the 'wiredTiger' storage engine, so setting the active storage engine to 'wiredTiger'.
2017-08-24T15:36:24.056+0800 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=903M,session_max=20000,eviction=(threads_min=4,threads_max=4),config_base=false,statistics=(fast),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),checkpoint=(wait=60,log_size=2GB),statistics_log=(wait=0),
2017-08-24T15:36:24.650+0800 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/db/diagnostic.data'
2017-08-24T15:36:24.650+0800 I NETWORK  [thread1] waiting for connections on port 27017
```
**MongoDB后台管理 Shell**
如果你需要进入MongoDB后台管理，你需要先打开mongodb装目录的下的bin目录，然后执行mongo命令文件。
MongoDB Shell是MongoDB自带的交互式Javascript shell,用来对MongoDB进行操作和管理的交互式环境。
当你进入mongoDB后台后，它默认会链接到 mongodb 文档（数据库）：
```
[root@localhost bin]# ./mongo
MongoDB shell version v3.4.7
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.7
```
由于它是一个JavaScript shell，您可以运行一些简单的算术运算:
```
> 1+1
2
> 2-2
0
> 41-343
-302
```
现在让我们插入一些简单的数据，并对插入的数据进行检索：
```
> db.runoob.insert({x:10})
WriteResult({ "nInserted" : 1 })
> db.runoob.find()
{ "_id" : ObjectId("599e8515457986461206fafe"), "x" : 10 }
> 
```
第一个命令将数字 10 插入到 runoob 集合的 x 字段中。
**MongoDb web 用户界面**
MongoDB 提供了简单的 HTTP 用户界面。 如果你想启用该功能，需要在启动的时候指定参数 --rest 。
```
./mongod --dbpath=/data/db --rest
```
MongoDB 的 Web 界面访问端口比服务的端口多1000。
如果你的MongoDB运行端口使用默认的27017，你可以在端口号为28017访问web用户界面，即地址为：http://localhost:28017。

{% asset_img mongodb_web.png %}