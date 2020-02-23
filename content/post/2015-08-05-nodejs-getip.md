---
title:       "nodejs获取访问ip"
subtitle:    ""
description: ""
date:        2015-08-05
author:      "wubin1989"
image:       ""
tags:        ["backend", "nodejs", "expressjs"]
categories:  ["Tech" ]
archives:    "2015"
---

```
function getClientIp(req) {
        return req.headers['x-forwarded-for'] ||
        req.connection.remoteAddress ||
        req.socket.remoteAddress ||
        req.connection.socket.remoteAddress;
    };
```
先判断是否有反向代理IP(头信息：x-forwarded-for)，在判断connection的远程IP，以及后端的socket的IP。
