---
title:       "创建ExpressJs项目"
subtitle:    ""
description: ""
date:        2015-06-09
author:      "wubin1989"
image:       ""
tags:        ["backend", "nodejs", "expressjs"]
categories:  ["Tech" ]
archives:    "2015"
---

1. 安装Nodejs: 下载地址:http://nodejs.org/download/    
2. 设置环境变量，例如我将nodejs装在D:/program文件夹下，则设以下为系统环境变量    
```
D:\Program\nodejs 
``` 
3. 安装Express开发框架：     
```
npm install -g express 
``` 
4. 新建项目  
```
express -t ejs newsproject  
```
5. 按照提示进入项目目录，运行npm安装  
```
cd newsproject  
npm install 
``` 
6. 运行项目    
```
node app.js  
```
7. 浏览器访问:http://127.0.0.1:3000/即可见nodejs站点页面，页面输出:Express
