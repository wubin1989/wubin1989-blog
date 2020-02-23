---
title:       "jQuery操作DOM"
subtitle:    ""
description: ""
date:        2014-11-30
author:      "wubin1989"
image:       ""
tags:        ["jQuery", "frontend"]
categories:  ["Tech"]
archives:    "2014"
---

## append()
描述: 在匹配css选择器的每个元素的末尾添加内容  
用法: 
### $(selector).append(content [, content])
参数类型: html字符串, 元素, 文本, 数组, jQuery对象
用例: 
```
$("p").append("<b>Hello World!</b>");
```
### $(selector).append(function)
参数类型: (index, html) => html字符串/元素/文本/jQuery对象
说明: index表示当前元素在所有匹配元素中的序号，html表示当前元素的innerHTML值，返回值会添加到原值的后面，而不是替换原值
用例: 
```
$("div").append(function(index, currentValue){
    console.log(index);
    console.log(currentValue);
    return "Hello World";
});
```

## appendTo()
描述: appendTo()方法和append()方法执行的任务相同。不同之处在于：appendTo的调用者是要添加的内容的jQuery对象，参数是被添加的每个匹配元素。  
用法: 
### $(content).appendTo(selector)
用例：
```
$("<b>Hello World!</b>").appendTo("p");
```

## html()
描述: 有两种用法，无参数和有参数
用法: 
### $(selector).html()
说明: 返回匹配元素中**第一个**元素的innerHTML值
用例: 
```
$("div").html();
```
### $(selector).html(htmlString)
说明: 设置每一个匹配元素的innerHTML值为参数值
用例: 
```
$("div").html("<p>Hello World!</p>");
```
### $(selector).html(function)
参数类型: (index, oldhtml) => newhtml
说明: 设置每一个匹配元素的innerHTML值为函数返回值
用例: 
```
$("div").html(function(index, oldhtml) {
  return "<p>new content</p>";
});
```

## prepend()
描述: 在匹配css选择器的每个元素的开头添加内容  
用法: 
### $(selector).prepend(content [, content])
参数类型: html字符串, 元素, 文本, 数组, jQuery对象
用例: 
```
$("p").prepend("<b>Hello World!</b>");
```
### $(selector).prepend(function)
参数类型: (index, html) => html字符串/元素/文本/jQuery对象
说明: index表示当前元素在所有匹配元素中的序号，html表示当前元素的innerHTML值，返回值会添加到原值的开头，而不是替换原值
用例: 
```
$("div").prepend(function(index, currentValue){
    console.log(index);
    console.log(currentValue);
    return "Hello World";
});
```

## prependTo()
描述: prependTo()方法和prepend()方法执行的任务相同。不同之处在于：prependTo的调用者是要添加的内容的jQuery对象，参数是被添加的每个匹配元素。  
用法: 
### $(content).prependTo(selector)
用例：
```
$("<b>Hello World!</b>").prependTo("p");
```

## text()
描述: 有两种用法，无参数和有参数
用法: 
### $(selector).text()
说明: 返回合并所有匹配元素的textContent值后的字符串
用例: 
```
$("p").text();
```
### $(selector).text(text)
参数类型: 字符串, 数字, 布尔值
说明: 设置每一个匹配元素的textContent值为参数值
用例: 
```
$("p").text("Hello World!");
```
### $(selector).text(function)
参数类型: (index, oldtext) => newtext
说明: 设置每一个匹配元素的textContent值为函数返回值
用例: 
```
$("p").text(function(index, oldtext) {
  return "new text";
});
```

## after()
描述: 在匹配css选择器的每个元素的后面添加内容  
用法: 
### $(selector).after(content [, content])
参数类型: html字符串, 元素, 文本, 数组, jQuery对象
用例: 
```
$("p").after("<b>Hello World!</b>");
```
### $(selector).after(function)
参数类型: (index, html) => html字符串/元素/文本/jQuery对象
说明: index表示当前元素在所有匹配元素中的序号，html表示当前元素的innerHTML值，返回值会添加到当前元素的后面
用例: 
```
$("div").after(function(index, currentValue){
    console.log(index);
    console.log(currentValue);
    return "Hello World";
});
```

## insertAfter()
描述: insertAfter()方法和after()方法执行的任务相同。不同之处在于：insertAfter的调用者是要添加的内容的jQuery对象，参数是被添加的每个匹配元素。  
用法: 
### $(content).insertAfter(selector)
用例：
```
$("<b>Hello World!</b>").insertAfter("p");
```

## before()
描述: 在匹配css选择器的每个元素的前面添加内容  
用法: 
### $(selector).before(content [, content])
参数类型: html字符串, 元素, 文本, 数组, jQuery对象
用例: 
```
$("p").before("<b>Hello World!</b>");
```
### $(selector).before(function)
参数类型: (index, html) => html字符串/元素/文本/jQuery对象
说明: index表示当前元素在所有匹配元素中的序号，html表示当前元素的innerHTML值，返回值会添加到原值的前面
用例: 
```
$("div").before(function(index, currentValue){
    console.log(index);
    console.log(currentValue);
    return "Hello World";
});
```

## insertBefore()
描述: insertBefore()方法和before()方法执行的任务相同。不同之处在于：insertBefore的调用者是要添加的内容的jQuery对象，参数是被添加的每个匹配元素。  
用法: 
### $(content).insertBefore(selector)
用例：
```
$("<b>Hello World!</b>").insertBefore("p");
```

## unwrap()
描述: 删除匹配css选择器的每个元素外层的父亲元素  
用法: 
### $(selector).unwrap()
用例: 
```
var pTags = $( "p" );
$( "button" ).click(function() {
  if ( pTags.parent().is( "div" ) ) {
    pTags.unwrap();
  } else {
    pTags.wrap( "<div></div>" );
  }
});
```
### $(selector).unwrap([selector])
参数类型: css字符串
说明: selector是可选参数，用来检查当前元素的父亲元素是否匹配该选择器。如果不匹配，则什么都不做
用例: 
```
$("p").unwrap("div");
```

## wrap()
描述: 在匹配css选择器的每个元素外层包装父亲元素
用法: 
### $(selector).wrap(wrappingElement)
参数类型: css选择器, html字符串, 元素, jQuery对象
用例：
```
$(".inner").wrap("p");
```
### $(selector).wrap(function)
参数类型: (index) => html字符串, jQuery对象
用例：
```
$(".inner").wrap(function() {
  return "<div class='" + $(this).text() + "'></div>";
});
```

## wrapAll()
描述: 在匹配css选择器的所有元素外层包装父亲元素
用法: 
### $(selector).wrapAll(wrappingElement)
参数类型: css选择器, html字符串, 元素, jQuery对象
用例：
```
$(".inner").wrapAll("<div class='new' />");
```
### $(selector).wrapAll(function)
参数类型: () => html字符串, jQuery对象
用例：
```
$(".inner").wrapAll(function() {
  return "<div></div>";
});
```