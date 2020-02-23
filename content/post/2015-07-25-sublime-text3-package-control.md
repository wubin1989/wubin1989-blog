---
title:       "sublime text3 安装package_control插件代码"
subtitle:    ""
description: ""
date:        2015-07-25
author:      "wubin1989"
image:       ""
tags:        ["frontend"]
categories:  ["Tech" ]
archives:    "2015"
---

先打开"view/show console"，再输入代码：  
```
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```