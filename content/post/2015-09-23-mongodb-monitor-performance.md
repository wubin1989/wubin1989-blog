---
title:       "mongodb的监控与性能优化"
subtitle:    ""
description: ""
date:        2015-09-23
author:      "wubin1989"
image:       ""
tags:        ["mongodb"]
categories:  ["Tech" ]
archives:    "2015"
---

## mongodb的监控
mongodb可以通过profile来监控数据，进行优化。   
查看当前是否开启profile功能用命令  
```
db.getProfilingLevel()  返回level等级，值为0|1|2，分别代表意思：0代表关闭，1代表记录慢命令，2代表全部
```
开始profile功能为
```
db.setProfilingLevel(level);  #level等级，值同上
```
level为1的时候，慢命令默认值为100ms，更改为db.setProfilingLevel(level,slowms)如db.setProfilingLevel(1,50)这样就更改为50毫秒
通过db.system.profile.find() 查看当前的监控日志。
如：
```
> db.system.profile.find({millis:{$gt:500}})  
{ "ts" : ISODate("2011-07-23T02:50:13.941Z"), "info" : "query order.order reslen:11022 nscanned:672230  \nquery: { status: 1.0 } nreturned:101 bytes:11006 640ms", "millis" : 640 }  
{ "ts" : ISODate("2011-07-23T02:51:00.096Z"), "info" : "query order.order reslen:11146 nscanned:672302  \nquery: { status: 1.0, user.uid: { $gt: 1663199.0 } }  nreturned:101 bytes:11130 647ms", "millis" : 647 }  
```
这里值的含义是  
* ts：命令执行时间  
* info：命令的内容  
* query：代表查询  
* order.order： 代表查询的库与集合  
* reslen：返回的结果集大小，byte数  
* nscanned：扫描记录数量  
* nquery：后面是查询条件  
* nreturned：返回记录数及用时  
* millis：所花时间  

如果发现时间比较长，那么就需要作优化。
比如nscanned数很大，或者接近记录总数，那么可能没有用到索引查询。
reslen很大，有可能返回没必要的字段。
nreturned很大，那么有可能查询的时候没有加限制。

mongo可以通过db.serverStatus()查看mongod的运行状态
```
> db.serverStatus()  
{  
     "host" : "baobao-laptop",#主机名  
     "version" : "1.8.2",#版本号  
     "process" : "mongod",#进程名  
     "uptime" : 15549,#运行时间  
     "uptimeEstimate" : 15351,  
     "localTime" : ISODate("2011-07-23T06:07:31.220Z"),当前时间  
     "globalLock" : {  
        "totalTime" : 15548525410,#总运行时间（ns）  
        "lockTime" : 89206633,  #总的锁时间（ns）  
        "ratio" : 0.005737305027178137,#锁比值  
        "currentQueue" : {  
            "total" : 0,#当前需要执行的队列  
            "readers" : 0,#读队列  
            "writers" : 0#写队列  
        },  
        "activeClients" : {  
            "total" : 0,#当前客户端执行的链接数  
           "readers" : 0,#读链接数  
           "writers" : 0#写链接数  
       }  
   },  
   "mem" : {#内存情况  
       "bits" : 32,#32位系统  
       "resident" : 337,#占有物理内存数  
       "virtual" : 599,#占有虚拟内存  
       "supported" : true,#是否支持扩展内存  
       "mapped" : 512  
   },  
   "connections" : {  
       "current" : 2,#当前链接数  
       "available" : 817#可用链接数  
   },  
   "extra_info" : {  
       "note" : "fields vary by platform",  
       "heap_usage_bytes" : 159008,#堆使用情况字节  
       "page_faults" : 907 #页面故作  
   },  
   "indexCounters" : {  
       "btree" : {  
           "accesses" : 59963, #索引被访问数  
           "hits" : 59963, #所以命中数  
           "misses" : 0,#索引偏差数  
           "resets" : 0,#复位数  
           "missRatio" : 0#未命中率  
       }  
   },  
   "backgroundFlushing" : {      
       "flushes" : 259,  #刷新次数  
       "total_ms" : 3395, #刷新总花费时长  
       "average_ms" : 13.108108108108109, #平均时长  
       "last_ms" : 1, #最后一次时长  
       "last_finished" : ISODate("2011-07-23T06:07:22.725Z")#最后刷新时间  
   },  
   "cursors" : {  
       "totalOpen" : 0,#打开游标数  
       "clientCursors_size" : 0,#客户端游标大小  
       "timedOut" : 16#超时时间  
   },  
   "network" : {  
       "bytesIn" : 285676177,#输入数据（byte）  
       "bytesOut" : 286564,#输出数据（byte）  
       "numRequests" : 2012348#请求数  
   },  
   "opcounters" : {  
       "insert" : 2010000, #插入操作数  
       "query" : 51,#查询操作数  
       "update" : 5,#更新操作数  
       "delete" : 0,#删除操作数  
       "getmore" : 0,#获取更多的操作数  
       "command" : 148#其他命令操作数  
   },  
   "asserts" : {#各个断言的数量  
       "regular" : 0,  
       "warning" : 0,  
       "msg" : 0,  
       "user" : 2131,  
       "rollovers" : 0  
   },  
   "writeBacksQueued" : false,  
   "ok" : 1  
}  
```

db.stats()查看某一个库的原先状况  
```
> db.stats()  
{  
    "db" : "order",#库名  
    "collections" : 4,#集合数  
    "objects" : 2011622,#记录数  
    "avgObjSize" : 111.92214441878245,#每条记录的平均值  
    "dataSize" : 225145048,#记录的总大小  
    "storageSize" : 307323392,#预分配的存储空间  
    "numExtents" : 21,#事件数  
    "indexes" : 1,#索引数  
    "indexSize" : 74187744,#所以大小  
    "fileSize" : 1056702464,#文件大小  
    "ok" : 1  
}
```  
 
查看集合记录用
```
> db.order.stats()  
{  
    "ns" : "order.order",#命名空间  
    "count" : 2010000,#记录数  
    "size" : 225039600,#大小  
    "avgObjSize" : 111.96,  
    "storageSize" : 307186944,  
    "numExtents" : 18,  
    "nindexes" : 1,  
    "lastExtentSize" : 56089856,  
    "paddingFactor" : 1,  
    "flags" : 1,  
    "totalIndexSize" : 74187744,  
    "indexSizes" : {  
        "_id_" : 74187744#索引为_id_的索引大小  
    },  
    "ok" : 1  
}
```  
 
mongostat命令查看运行中的实时统计，表示每秒实时执行的次数
mongodb还提供了一个机遇http的监控页面，可以访问http://ip:28017来查看，这个页面基本上是对上面的这些命令做了一下综合，所以这里不细述了。

## mongodb的优化
根据上面这些监控手段，找到问题后，我们可以进行优化
上面找到了某一下慢的命令，现在我们可以通过执行计划跟踪一下，如
```
> db.order.find({ "status": 1.0, "user.uid": { $gt: 2663199.0 } }).explain()  
{  
    "cursor" : "BasicCursor",#游标类型  
    "nscanned" : 2010000,#扫描数量  
    "nscannedObjects" : 2010000,#扫描对象  
    "n" : 337800,#返回数据  
    "millis" : 2838,#耗时  
    "nYields" : 0,  
    "nChunkSkips" : 0,  
    "isMultiKey" : false,  
    "indexOnly" : false,  
    "indexBounds" : {#使用索引（这里没有）  
          
    }  
}
```  
对于这样的，我们可以创建索引  
可以通过  db.collection.ensureIndex({"字段名":1}) 来创建索引，1为升序，-1为降序，在已经有多数据的情况下，可用后台来执行，语句db.collection.ensureIndex({"字段名":1} , {backgroud:true}) 
获取索引用db.collection.getIndexes() 查看  
这里我们创建一个user.uid的索引 
```
>db.order.ensureIndex({"user.uid":1})
```
创建后重新执行
```
db.order.find({ "status": 1.0, "user.uid": { $gt: 2663199.0 } }).explain()  
{  
    "cursor" : "BtreeCursor user.uid_1",  
    "nscanned" : 337800,  
    "nscannedObjects" : 337800,  
    "n" : 337800,  
    "millis" : 1371,  
    "nYields" : 0,  
    "nChunkSkips" : 0,  
    "isMultiKey" : false,  
    "indexOnly" : false,  
    "indexBounds" : {  
        "user.uid" : [  
            [  
                2663199,  
                1.7976931348623157e+308  
            ]  
        ]  
    }  
}
```  
 
扫描数量减少，速度提高。mongodb的索引设计类似与关系数据库，按索引查找加快书读，但是多了会对写有压力，所以这里就不再叙述了。  

2.其他优化可以用hint强制索引查找，返回只是需要的数据，对数据分页等。  