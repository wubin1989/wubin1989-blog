---
title:       "HTML5 LocalStorage"
subtitle:    ""
description: ""
date:        2015-11-17
author:      ""
image:       ""
tags:        ["frontend", "html5"]
categories:  ["Tech"]
archives:    "2015"
---

`localStorage`和`sessionStorage`一样都是用来存储客户端临时信息的对象,他们均只能存储字符串类型的对象
`localStorage`生命周期是永久，这意味着除非用户显示在浏览器提供的UI上清除`localStorage`信息，否则这些信息将永远存在
`sessionStorage`生命周期为当前窗口或标签页，一旦窗口或标签页被永久关闭了，那么所有通过`sessionStorage`存储的数据也就被清空了
ps:不同浏览器无法共享`localStorage`或`sessionStorage`中的信息。相同浏览器的不同页面间可以共享相同的`localStorage`（页面属于相同域名和端口），但是不同页面或标签页间无法共享`sessionStorage`的信息。这里需要注意的是，页面及标签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，那么他们之间是可以共享`sessionStorage`的

在HTML5中，本地存储是一个window的属性，包括`localStorage`和`sessionStorage`

1. 首先自然是检测浏览器是否支持本地存储(如`localStorage`)
```
if(window.localStorage){
    alert('This browser supports localStorage');
}else{
    alert('This browser does NOT support localStorage');
}
```

2. 往`localStorage`里边设值
```
localStorage.name = 'zs';//设置name为"zs"
localStorage["name"] = "ls";//设置name为"ls"
localStorage.setItem("name","ww");//设置name为"ww"
```

3. 获取`localStorage`里边的值
```
localStorage.name
localStorage["name"]
localStorage.getItem("name")
```

另外，HTML5还提供了一个key()方法
```
var storage = window.localStorage;
function showStorage(){
    for(var i=0;i<storage.length;i++){
        //key(i)获得相应的键，再用getItem()方法获得对应的值
        document.write(storage.key(i)+ " : " + storage.getItem(storage.key(i)) + "
");
    }
}
```

4. 清除`localStorage`里边的值
```
localStorage.removeItem("name")        // 删除name
localStorage.clear()    //清除所有
```

5. `localStorage`事件
HTML5的本地存储，还提供了一个storage事件，可以对键值对的改变进行监听，使用方法如下
```
if(window.addEventListener){
    window.addEventListener("storage",handleStorage,false);
}else if(window.attachEvent){
    window.attachEvent("onstorage",handleStorage);
}
function handleStorage(e){
    if(!e){e=window.event;}
        //showStorage();
}
```
 
6. 封装`localStorage`
```
var LS = {
    set : function(key, value){
        //在iPhone/iPad上有时设置setItem()时会出现诡异的QUOTA_EXCEEDED_ERR错误
        //这时一般在setItem之前，先removeItem()就ok了
        if( this.get(key) !== null )
            this.remove(key);
        localStorage.setItem(key, value);
    },
    //查询不存在的key时，有的浏览器返回undefined，这里统一返回null
    get : function(key){
        var v = localStorage.getItem(key);
        return v === undefined ? null : v;
    },
    remove : function(key){ localStorage.removeItem(key); },
    clear : function(){ localStorage.clear(); },
    each : function(fn){
        var n = localStorage.length, i = 0, fn = fn || function(){}, key;
        for(; i<n; i++){
            key = localStorage.key(i);
            if( fn.call(this, key, this.get(key)) === false )
                break;
            //如果内容被删除，则总长度和索引都同步减少
            if( localStorage.length < n ){
                n --;
                i --;
            }
        }
    }
}
```
