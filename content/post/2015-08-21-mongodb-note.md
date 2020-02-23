---
title:       "mongodb学习笔记"
subtitle:    ""
description: ""
date:        2015-08-21
author:      "wubin1989"
image:       ""
tags:        ["mongodb"]
categories:  ["Tech" ]
archives:    "2015"
---

* 根据字段模糊查询所有文档
```
exampleField: {$regex:''}
```
* 查看某一次执行的性能情况
```
db.terminal.find({status:1000}).explain("executionStats")
```
* _id中提取时间戳
```
var timestamp = _id.toString().substring( 0, 8 ); 
var date = new Date( parseInt( timestamp, 16 ) * 1000 ); 
```
* 统计某一字段的总数
```
db.runCommand({"group":{ 
    "ns":"message", 
    "key":"toUid", 
    "initial":{"total":0}, 
    "$reduce" : function(doc,prev){ 
        prev.total += doc.unreadCount; 
    }, 
    "condition":{"toUid":"F34780D6A3CA543DB020D584A91CC8BB"} 
}});

{gameCode:'T51',takeTime:{$gt:1442851261000}}
```
* 导入csv文件
```
mongoimport --db users --collection contacts --type csv --headerline --file /opt/backups/contacts.csv
```
* 重命名列名
```
db.students.update( { _id: 1 }, { $rename: { 'nickname': 'alias', 'cell': 'mobile' } } )
```
* 导出集合
```
mongoexport -h 127.0.0.1 -d baaaaby-oss-dev -c xbj.srv.coursetypes -f _id,name,is_active,type,remark --type=csv -o coursetype.csv
mongoexport -h 127.0.0.1 -d baaaaby-oss-dev -c xbj.srv.coursetypes -o coursetype.json
```