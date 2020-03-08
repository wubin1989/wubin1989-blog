---
title:       "Html5地理位置定位"
subtitle:    ""
description: ""
date:        2016-02-03
author:      ""
image:       ""
tags:        ["frontend", "html5"]
categories:  ["Tech"]
archives:    "2016"
---

地理位置定位的几种方式：IP地址，GPS，Wifi，GSM/CDMA 

## 地理位置获取流程： 
* 用户打开需要获取地理位置的web应用。 
* 应用向浏览器请求地理位置，浏览器弹出询问，询问用户是否共享地理位置。 
* 假设用户允许，浏览器从设别查询相关信息。 
* 浏览器将相关信息发送到一个信任的位置服务器，服务器返回具体的地理位置。 

## HTML5地理地位的实现： 
### 实现基于浏览器（无需后端支持）获取用户的地理位置技术 
Geolocation API 用于将用户当前地理位置信息共享给信任的站点，这涉及用户的隐私安全问题，所以当一个站点需要获取用户的当前地理位置，浏览器会提示用户是“允许” or “拒绝”。 
先看看哪些浏览器支持Geolocation API： 
IE9.0+、FF3.5+、Safari5.0+、Chrome5.0+、Opera10.6+、IPhone3.0+、Android2.0+ 
Geolocation API存在于navigator对象中，只包含3个方法： 
* getCurrentPosition //当前位置 
```
navigator.geolocation.getCurrentPosition( … , function(error){ 
    switch(error.code){ 
        case error.TIMEOUT : 
            alert( " 连接超时，请重试 " ); 
            break; 
        case error.PERMISSION_DENIED : 
            alert( " 您拒绝了使用位置共享服务，查询已取消 " ); 
            break; 
        case error.POSITION_UNAVAILABLE : 
            alert( " ，抱歉，暂时无法为您所在的星球提供位置服务 " ); 
            break; 
    } 
}); 
```
* watchPosition //监视位置 
* clearWatch //清除监视 
```
var watchPositionId = navigator.geolocation.watchPosition(success_callback, error_callback, options); 
navigator.geolocation.clearWatch(watchPositionId );
```
### 精确定位用户的地理位置( 精度最高达10m之内，依赖设备 ) 
HTML 5提供了地理位置等一系列API可以给用户使用，方便用户制作LBS的地理应用，首先在支持HTML 5的浏览器中，当开启API时，会询问是否用户同意使用api，否则不会开启的，保证安全。 
* 开启，判断是否浏览器支持LBS api 
```
function isGeolocationAPIAvailable() { 
    var location = "No, Geolocation is not supported by this browser."; 
    if (window.navigator.geolocation) { 
        location = "Yes, Geolocation is supported by this browser."; 
    } 
    alert(location); 
} 
```
上面的例子中，还在displayError方法中，捕捉了异常； 
* 获得用户的地理位置 
这个使用getCurrentPosition就可以了； 
```
function requestPosition() { 
    if (nav == null) { 
        nav = window.navigator; 
    } 
    if (nav != null) { 
        var geoloc = nav.geolocation; 
        if (geoloc != null) { 
            geoloc.getCurrentPosition(successCallback); 
        } else { 
            alert("Geolocation API is not supported in your browser"); 
        } 
    } else { 
        alert("Navigator is not found"); 
    } 
} 
```
当获得地理位置成功后，会产生一个回调方法了，处理返回的结果， 
```
function setLocation(val, e) { 
    document.getElementById(e).value = val; 
} 
function successCallback(position) { 
    setLocation(position.coords.latitude, "latitude"); 
    setLocation(position.coords.longitude, "longitude"); 
} 
```
### 持续追踪用户的地理位置 
```
function listenForPositionUpdates() { 
    if (nav == null) { 
        nav = window.navigator; 
    } 
    if (nav != null) { 
        var geoloc = nav.geolocation; 
        if (geoloc != null) { 
            watchID = geoloc.watchPosition(successCallback); 
        } else { 
            alert("Geolocation API is not supported in your browser"); 
        } 
    } else { 
        alert("Navigator is not found"); 
    } 
} 
```
然后在successCallback中，就可以设置显示最新的地理位置： 
```
function successCallback(position){ 
    setText(position.coords.latitude, "latitude"); 
    setText(position.coords.longitude, "longitude"); 
} 
```
如果不希望实时跟踪，则可以取消之： 
```
function clearWatch(watchID) { 
    window.navigator.geolocation.clearWatch(watchID); 
} 
```
当遇到异常时，可以捕捉之： 
```
if (geoloc != null) { 
    geoloc.getCurrentPosition(successCallback, errorCallback); 
} 
function errorCallback(error) { 
    var message = ""; 
    switch (error.code) { 
        case error.PERMISSION_DENIED: 
            message = "This website does not have permission to use " 
            + "the Geolocation API"; 
            break; 
        case error.POSITION_UNAVAILABLE: 
            message = "The current position could not be determined."; 
            break; 
        case error.PERMISSION_DENIED_TIMEOUT: 
            message = "The current position could not be determined " 
            + "within the specified timeout period."; 
            break; 
    } 
    if (message == "") { 
        var strErrorCode = error.code.toString(); 
        message = "The position could not be determined due to " 
        + "an unknown error (Code: " + strErrorCode + ")."; 
    } 
    alert(message); 
} 
```
### 与 Google Map、或者 Baidu Map 交互呈现位置信息 
```
function getCurrentLocation() { 
    if (navigator.geolocation) { 
        navigator.geolocation.getCurrentPosition(showMyPosition,showError); 
    } else { 
        alert("No, Geolocation API is not supported by this browser."); 
    } 
} 

function showMyPosition(position) { 
    var coordinates=position.coords.latitude+","+position.coords.longitude; 
    var map_url="http://maps.googleapis.com/maps/api/staticmap?center=" 
    +coordinates+"&zoom=14&size=300x300&sensor=false"; 
    document.getElementById("googlemap").innerHTML="<img src='"+map_url+"' />"; 
} 

function showError(error) { 
    switch(error.code) { 
        case error.PERMISSION_DENIED: 
            alert("This website does not have permission to use the Geolocation API") 
            break; 
        case error.POSITION_UNAVAILABLE: 
            alert("The current position could not be determined.") 
            break; 
        case error.TIMEOUT: 
            alert("The current position could not be determined within the specified time out period.") 
            break; 
        case error.UNKNOWN_ERROR: 
            alert("The position could not be determined due to an unknown error.") 
            break; 
    } 
} 
```
