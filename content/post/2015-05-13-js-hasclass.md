---
title:       "Javascript原生hasClass, addClass, removeClass"
subtitle:    ""
description: ""
date:        2015-05-13
author:      "wubin1989"
image:       ""
tags:        ["frontend"]
categories:  ["Tech" ]
archives:    "2015"
---

```
function hasClass(ele,cls) { 
return ele.className.match(new RegExp('(\\s|^)'+cls+'(\\s|$)')); 
} 

function addClass(ele,cls) { 
if (!this.hasClass(ele,cls)) ele.className += " "+cls; 
} 

function removeClass(ele,cls) { 
if (hasClass(ele,cls)) { 
var reg = new RegExp('(\\s|^)'+cls+'(\\s|$)'); 
ele.className=ele.className.replace(reg,' '); 
} 
} 

//call the functions 
addClass(document.getElementById("test"), "test"); 
removeClass(document.getElementById("test"), "test") 
if(hasClass(document.getElementById("test"), "test")){//do something}; 
```