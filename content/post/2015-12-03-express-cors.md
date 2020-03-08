---
title:       "Express跨域代码"
subtitle:    ""
description: ""
date:        2015-12-03
author:      ""
image:       ""
tags:        ["backend", "nodejs"]
categories:  ["Tech"]
archives:    "2015"
---


```
var allowCrossDomain = function(req, res, next) {
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', '*');
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    next();
};

// 其他代码

app.use(bodyParser.json({limit: '100mb'}));
app.use(methodOverride());
app.use(allowCrossDomain);
```
