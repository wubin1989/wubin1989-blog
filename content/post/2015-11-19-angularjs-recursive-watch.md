---
title:       "Angularjs的$watch函数"
subtitle:    ""
description: ""
date:        2015-11-19
author:      ""
image:       ""
tags:        ["frontend", "angularjs"]
categories:  ["Tech"]
archives:    "2015"
---

Angularjs的$watch函数可以监听变量的变化，调用回调函数，例如：
```
$scope.$watch('form', function(newVal, oldVal){
    console.log('changed');
}, true);
```
它还接受布尔类型的第三个参数`objectEquality`，当监听的变量引用的是复杂类型，如果值为`true`，会比较所有的属性值是否相等，如果值为`false`，表示只会比较两个对象的引用。默认是`false`。