---
title:       "碎片笔记(二)"
subtitle:    ""
description: ""
date:        2014-12-12
author:      "wubin1989"
image:       ""
tags:        ["frontend"]
categories:  ["Tech" ]
archives:    "2014"
---

## 动态变静态
方法一：使用现成的插件，比如：ISAPI_Rewrite、IIS Rewrite、Apache HTTP服务器的mod_rewrite等，它们都是基于正则表达式解析器开发的重写引擎。它们的使用方法查看它们自带的帮助即可。
方法二：自己写的代码实现动态网页静态化，方法也有好几种：
1、创建FSO对象，利用此对象将所需的内容动态创建到文件中生成HTML页面；
2、利用模板技术，将模板中特殊代码的值替换为从表单或是数据库字段中接受过来的值 生成HTML文件；
3、使用Server.Transfer转换技术，
方法三：使用HttpWebRequest请求客户端的方式，获取返回资源，生成静态页面。一般这样只需要获取网页内容即可，其它资源可放置在服务器上，自动加载。(注：此方法缺点明显，需要大量更改匹配URL，建议慎用)
方法四：在asp中有IhttpModule接口。Ihttpmodule可以简单理解为一个可以在执行像.aspx,或者mvc中control/action前，添加我们自定义的操作的东西。
我们只需要编写这么一个HttpModule就可以了，当用户第一次请求asp处理时，我们可以在ihttpmodule中拦截到这个请求，然后获取到这次请求应该返回的html代码，然后我们返回这些html给用户，并保存刚才我们获取到的html到文件内，当用户下次请求时，我们只需要直接返回我们已经保存的html文件即可。[2]

## 页面加载完后自动执行一个方法的js代码
1、在body中用onload:
```
<body onload="myfunction()">
```

2、在脚本中用window.onload:
```
<script type="text/javascript"> 
function myfun()
 { alert("this window.onload"); } 
/*用window.onload调用myfun()*/
window.onload=myfun;//不要括号 
</script> 
```

## css边框样式
边框样式值如下：
none : 　无边框。与任何指定的border-width值无关
hidden : 　隐藏边框。IE不支持
dotted : 　在MAC平台上IE4+与WINDOWS和UNIX平台上IE5.5+为点线。否则为实线（常用）
dashed : 　在MAC平台上IE4+与WINDOWS和UNIX平台上IE5.5+为虚线。否则为实线（常用）
solid : 　实线边框（常用）
double : 　双线边框。两条单线与其间隔的和等于指定的border-width值
groove : 　根据border-color的值画3D凹槽
ridge : 　根据border-color的值画菱形边框
inset : 　根据border-color的值画3D凹边
outset : 　根据border-color的值画3D凸边

## 原生js获取屏幕可视画面宽度的代码
```
window.innerWidth
document.documentElement.clientWidth
document.body.clientWidth
```

## 滚动到指定位置
代码如下:
```
$('.scroll_a').click(function(){$('html,body').animate({scrollTop:$('.a').offset().top}, 800);});

$(window).scroll(function()
    {
        var body = document.body && document.body.scrollTop? document.body : document.documentElement;
        var height = parseInt(body.scrollHeight) - parseInt(screen.height);
        if(body.scrollTop >= height)
        {
            //开始做你的事情吧。。。
        }
    });
```

## 弹窗效果js代码
```
<SCRIPT LANGUAGE="javascript"> 
window.open ('page.html', 'newwindow', 'height=100, width=400, top=0,left=0, toolbar=no, menubar=no, scrollbars=no, resizable=no,location=no,status=no') 
</SCRIPT> 
```
解释:  
window.open 弹出新窗口的命令；  
'page.html' 弹出窗口的文件名；   
'newwindow' 弹出窗口的名字（不是文件名），非必须，可用空''代替；   
height=100 窗口高度；   
width=400 窗口宽度；   
top=0 窗口距离屏幕上方的象素值；   
left=0 窗口距离屏幕左侧的象素值；   
toolbar=no 是否显示工具栏，yes为显示；   
menubar，scrollbars 表示菜单栏和滚动栏。   
resizable=no 是否允许改变窗口大小，yes为允许；   
location=no 是否显示地址栏，yes为允许；   
status=no 是否显示状态栏内的信息（通常是文件已经打开），yes为允许；   

## HTML段落自动换行的样式设置
在HTML的P标记中，默认情况下是自动换行的。  
如果你的段落是由中文字符或者英文单词组成的，这基本没什么问题。但是如果你的段落是由不间断的英文字母（浏览器会认为是一个单词）组成，则默认情况下不会换行，将会把段落撑开，效果会很难看！  
这时你可以使用如下的样式属性：word-wrap:break-word; 意思是将单词的回卷特性设置为截断单词。  
另外，为了使段落看不去更美观，可以使用让段落两端对齐的属性：text-align:justify;  


## JQuery获取子元素
1、查找子元素方式1：>  
例如：var aNods = $("ul > a");查找ul下的所有a标签  

2、查找子元素方式2：children()  

3、查找子元素方式3：find()  

这里再简单介绍以下children()和find()的异同：  

1> children及find方法都用是用来获得element的子elements的，两者都不会返回 text node，就像大多数的jQuery方法一样。  
2> children方法获得的仅仅是元素一下级的子元素，即：immediate children。  
3> find方法获得所有下级元素，即：descendants of these elements in the DOM tree  
4> children方法的参数selector 是可选的（optionally），用来过滤子元素，  

但find方法的参数selector方法是必选的。  
5> find方法事实上可以通过使用 jQuery( selector, context )来实现。即$('li.item-ii').find('li')等同于$('li', 'li.item-ii').

## js构造函数自定义对象
```
function createCar(sColor,iDoors,iMpg) {
  var oTempCar = new Object;
  oTempCar.color = sColor;
  oTempCar.doors = iDoors;
  oTempCar.mpg = iMpg;
  oTempCar.showColor = function() {
    alert(this.color);
  };
  return oTempCar;
}

var oCar1 = createCar("red",4,23);
var oCar2 = createCar("blue",3,25);
```

## 禁止双击选中页面内容
```
<div class="picBox"onselectstart="return false;" style="-moz-user-select:none;">屏蔽双击选中文字的区域</div>
```

## Js得到一个月最大天数
JS里 面的new Date("xxxx/xx/xx")这个日期的构造方法有一个妙处，  
当你传入的是"xxxx/xx/0"（0号）的话，得到的日期是"xx"月的前一个 月的最后一天（"xx"月的最大取值是69，题外话），  
当你传入的是"xxxx/xx/1"（1号）的话，得到的日期是"xx"月的后一个 月的第一天（自己理解）  
如果传入"1999/13/0"，会得到"1998/12/31"。而且最大的好处是当你传入"xxxx/3/0"，会得到xxxx年2月的最后一天，它会自动判断当年是否是闰年来返回28或29，不用自己判断，  
所以，我们想得到选择年选择月有多少天的话，只需要  
```
var temp=new Date("选择年/选择月+1/0");  
return temp.getDate()//最大天数  
```
校验的话，也可以用这个方法。 

## javascript取整数的方法
```
Math.round(num) // 四舍五入
Math.floor(num) // 小于等于num的整数
Math.ceil()  // 大于等于num的整数
parseInt(num) // 小于等于num的整数,与floor的区别是parseInt参数可以是string类型，如'5abc'返回5。
```

## JavaScript中取余运算符
JavaScript中取余运算符(%)是一个表达式的值除以另一个表达式的值，返回余数。  
使用方法：  
result = number1 % number2  
其中result是任何变量。  
是number1是任何数值表达式。  
number2是任何数值表达式。   

JavaScript中取余运算符  
取余（或余数）运算符用 number1 除以 number2 （把浮点数四舍五入为整数），然后只返回余数作为 result。例如，在下面的表达式中，A （即 result）等于 5。  
A = 19 % 6.7  


## oninput与onpropertychange失效
oninput事件：  
　　（1）当脚本中改变value时，不会触发；  
　　（2）从浏览器的自动下拉提示中选取时，不会触发；  
onpropertychange事件：  
　　当input设置为disable=true后，不会触发。  

## 去掉iPhone、iPad的默认按钮样式
```
input[type="button"], input[type="submit"], input[type="reset"] {
-webkit-appearance: none;
}
textarea {  -webkit-appearance: none;}   
```
如果还有圆角的问题  
```
.button{ border-radius: 0; } 
```