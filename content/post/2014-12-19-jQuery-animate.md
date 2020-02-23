---
title:       "jQuery动画小结"
subtitle:    ""
description: ""
date:        2014-12-19
author:      "wubin1989"
image:       ""
tags:        ["jQuery", "frontend"]
categories:  ["Tech"]
archives:    "2014"
---

## animate()
描述: 通过改变数值类的css属性的值来实现动画效果
用法: 
### $(selector).animate( properties [, duration ] [, easing ] [, complete ] )
参数类型:  
* properties: css属性构成的对象，表示动画的最终效果
* duration: 数字或字符串。动画持续时间，默认值400毫秒，'fast'表示200毫秒，'slow'表示600毫秒
* easing: 字符串。缓动函数，默认值swing
* complete: 动画结束时的回调函数
用例: 
```
$( "#clickme" ).click(function() {
  $( "#book" ).animate({
    opacity: 0.25,
    left: "+=50",
    height: "toggle"
  }, 5000, function() {
    // Animation complete.
  });
});
```
### $(selector).animate( properties, options )
参数类型:  
* properties: css属性构成的对象，表示动画的最终效果
* options: 对象，包括以下属性：
    * duration: 数字或字符串。动画持续时间
    - easing: 字符串。缓动函数
    - queue: 布尔值或字符串。布尔值表示是否把动画加入队列中。如果false，动画会立刻执行。默认值true。jquery1.7以后，接受字符串类型的参数，可以自定义队列名称。如果传入了自定义的动画队列名称，动画将不会自动开始，必须要调用.dequeue('queuename')手动触发动画
    - specialEasing: 给css属性分别指定缓动函数
    - step, progress, complete, start, done, fail, always: 都是回调函数。在动画执行的不同阶段触发。匹配css选择器的每个元素会分别执行各自的回调函数。
        - step: 接受now和fx两个参数，在properties里指定的每个css属性每次变化开始时执行，now是改变前的属性值，fx对象即标准动画队列
        - progress: 接受一个含有options和其他相关信息的对象, 一个表示进度的百分比数值，一个剩余毫秒数。每组step完成后执行，即所有构成动画的css属性都完成一个step后执行
        - complete: 整个动画结束时的回调函数
        - start: 匹配css选择器的每个元素的动画开始时分别执行
        - done, fail, always三个函数是jquery1.8以后加入，不常用  


用例: 
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .my-ul {
        width: 300px;
        overflow: hidden;
        background: greenyellow;
      }
      .my-ul li {
          height: 60px;
          border-bottom: 1px solid grey;
      }
    </style>
  </head>
  <body>
    <ul class="my-ul">
      <li>
        语文
      </li>
      <li>
        数学
      </li>
      <li>
        英语
      </li>
    </ul>

    <script src="http://code.jquery.com/jquery-1.11.2.min.js"></script>
    <script>
      $("li").animate(
        {
          opacity: 0.5,
          height: '30px'
        },
        {
          step: function(now, fx) {
            var data = fx.elem.id + " " + fx.prop + ": " + now;
            $("body").append("<div>" + data + "</div>");
          },
          progress: function(animation, progress, remainingms) {
              console.log(animation, progress, remainingms);
          },
        }
      );
    </script>
  </body>
</html>
```

## clearQueue()
描述: 清空动画队列，即把队列中未执行的动画删掉
用法: 
### $(selector).clearQueue( [queueName ] )
参数类型: 队列名字符串。默认是fx，即标准动画队列。
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>clearQueue demo</title>
  <style>
  div {
    margin: 3px;
    width: 40px;
    height: 40px;
    position: absolute;
    left: 0px;
    top: 30px;
    background: green;
    display: none;
  }
  div.newcolor {
    background: blue;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<button id="start">Start</button>
<button id="stop">Stop</button>
<div></div>
 
<script>
$( "#start" ).click(function() {
  var myDiv = $( "div" );
  myDiv.show( "slow" );
  myDiv.animate({
    left:"+=200"
  }, 5000 );
 
  myDiv.queue(function() {
    var that = $( this );
    that.addClass( "newcolor" );
    that.dequeue();
  });
 
  myDiv.animate({
    left:"-=200"
  }, 1500 );
  myDiv.queue(function() {
    var that = $( this );
    that.removeClass( "newcolor" );
    that.dequeue();
  });
  myDiv.slideUp();
});
 
$( "#stop" ).click(function() {
  var myDiv = $( "div" );
  myDiv.clearQueue();
  myDiv.stop();
});
</script>
 
</body>
</html>
```

## delay()
描述: 设置动画延迟时间
用法: 
### $(selector).delay( duration [, queueName ] )
参数类型:  
* duration: 数字，延迟时间，单位毫秒
* queueName: 字符串，队列名称
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>delay demo</title>
  <style>
  div {
    position: absolute;
    width: 60px;
    height: 60px;
    float: left;
  }
  .first {
    background-color: #3f3;
    left: 0;
  }
  .second {
    background-color: #33f;
    left: 80px;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<p><button>Run</button></p>
<div class="first"></div>
<div class="second"></div>
 
<script>
$( "button" ).click(function() {
  $( "div.first" ).slideUp( 300 ).delay( 800 ).fadeIn( 400 );
  $( "div.second" ).slideUp( 300 ).fadeIn( 400 );
});
</script>

</body>
</html>
```

## fadeIn()
描述: 渐入动画
用法: 
### $(selector).fadeIn( [duration ] [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>fadeIn demo</title>
  <style>
  span {
    color: red;
    cursor: pointer;
  }
  div {
    margin: 3px;
    width: 80px;
    display: none;
    height: 80px;
    float: left;
  }
  #one {
    background: #f00;
  }
  #two {
    background: #0f0;
  }
  #three {
    background: #00f;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<span>Click here...</span>
<div id="one"></div>
<div id="two"></div>
<div id="three"></div>
 
<script>
$( document.body ).click(function() {
  $( "div:hidden" ).first().fadeIn( "slow" );
});
</script>
 
</body>
</html>
```  

## fadeOut()
描述: 渐隐动画
用法: 
### $(selector).fadeOut( [duration ] [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>fadeOut demo</title>
  <style>
  p {
    font-size: 150%;
    cursor: pointer;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<p>
  If you click on this paragraph
  you'll see it just fade away.
</p>
 
<script>
$( "p" ).click(function() {
  $( "p" ).fadeOut( "slow" );
});
</script>
 
</body>
</html>
```

## fadeTo()
描述: 改变透明度动画
用法: 
### $(selector).fadeTo( duration, opacity [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* opacity: 透明度目标值
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>fadeTo demo</title>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<p>
Click this paragraph to see it fade.
</p>
 
<p>
Compare to this one that won't fade.
</p>
 
<script>
$( "p" ).first().click(function() {
  $( this ).fadeTo( "slow", 0.33 );
});
</script>
 
</body>
</html>
```

## fadeToggle()
描述: 切换隐藏和出现的动画
用法: 
### $(selector).fadeToggle( [duration ] [, easing ] [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* easing: 缓动函数
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>fadeToggle demo</title>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<button>fadeToggle p1</button>
<button>fadeToggle p2</button>
<p>This paragraph has a slow, linear fade.</p>
<p>This paragraph has a fast animation.</p>
<div id="log"></div>
 
<script>
$( "button" ).first().click(function() {
  $( "p" ).first().fadeToggle( "slow", "linear" );
});
$( "button" ).last().click(function() {
  $( "p" ).last().fadeToggle( "fast", function() {
    $( "#log" ).append( "<div>finished</div>" );
  });
});
</script>
 
</body>
</html>
```

## finish()
描述: 停止所有正在进行的动画，清空动画队列，将动画相关的css属性值设置为目标值
用法: 
### $(selector).finish( [queue ] )
参数类型: 队列名字符串。默认是fx，即标准动画队列。
用例：
```
 <!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>finish demo</title>
  <style>
  .box {
    position: absolute;
    top: 10px;
    left: 10px;
    width: 15px;
    height: 15px;
    background: black;
  }
  #path {
    height: 244px;
    font-size: 70%;
    border-left: 2px dashed red;
    border-bottom: 2px dashed green;
    border-right: 2px dashed blue;
  }
  button {
    width: 12em;
    display: block;
    text-align: left;
    margin: 0 auto;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<div class="box"></div>
<div id="path">
  <button id="go">Go</button>
  <br>
  <button id="bstt" class="b">.stop( true,true )</button>
  <button id="bcf" class="b">.clearQueue().finish()</button>
  <br>
  <button id="bstf" class="b">.stop( true, false )</button>
  <button id="bcs" class="b">.clearQueue().stop()</button>
  <br>
  <button id="bsff" class="b">.stop( false, false )</button>
  <button id="bs" class="b">.stop()</button>
  <br>
  <button id="bsft" class="b">.stop( false, true )</button>
  <br>
  <button id="bf" class="b">.finish()</button>
</div>
 
<script>
var horiz = $( "#path" ).width() - 20,
  vert = $( "#path" ).height() - 20;
 
var btns = {
  bstt: function() {
    $( "div.box" ).stop( true, true );
  },
  bs: function() {
    $( "div.box" ).stop();
  },
  bsft: function() {
    $( "div.box" ).stop( false, true );
  },
  bf: function() {
    $( "div.box" ).finish();
  },
  bcf: function() {
    $( "div.box" ).clearQueue().finish();
  },
  bsff: function() {
    $( "div.box" ).stop( false, false );
  },
  bstf: function() {
    $( "div.box" ).stop( true, false );
  },
  bcs: function() {
    $( "div.box" ).clearQueue().stop();
  }
};
 
$( "button.b" ).on( "click", function() {
  btns[ this.id ]();
});
 
$( "#go" ).on( "click", function() {
  $( ".box" )
    .clearQueue()
    .stop()
    .css({
      left: 10,
      top: 10
    })
    .animate({
      top: vert
    }, 3000 )
    .animate({
      left: horiz
    }, 3000 )
    .animate({
      top: 10
    }, 3000 );
});
</script>
 
</body>
</html>
```

## hide()
描述: 隐藏匹配css选择器的元素
用法: 
### $(selector).hide( [duration ] [, complete ] )
参数类型: 
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>hide demo</title>
  <style>
  p {
    background: #dad;
    font-weight: bold;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<button>Hide 'em</button>
<p>Hiya</p>
<p>Such interesting text, eh?</p>
 
<script>
$( "button" ).click(function() {
  $( "p" ).hide( "slow" );
});
</script>
 
</body>
</html>
```

## show()
描述: 显示匹配css选择器的元素
用法: 
### $(selector).show( [duration ] [, complete ] )
参数类型: 
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>show demo</title>
  <style>
  p {
    background: yellow;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<button>Show it</button>
<p style="display: none">Hello  2</p>
 
<script>
$( "button" ).click(function() {
  $( "p" ).show( "slow" );
});
</script>
 
</body>
</html>
```

## slideDown()
描述: 向下展开动画
用法: 
### $(selector).slideDown( [duration ] [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例:  
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>slideDown demo</title>
  <style>
  div {
    background: #de9a44;
    margin: 3px;
    width: 80px;
    height: 40px;
    display: none;
    float: left;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
Click me!
<div></div>
<div></div>
<div></div>
 
<script>
$( document.body ).click(function () {
  if ( $( "div" ).first().is( ":hidden" ) ) {
    $( "div" ).slideDown( "slow" );
  } else {
    $( "div" ).hide();
  }
});
</script>
 
</body>
</html>
```

## slideUp()
描述: 向上收起动画
用法: 
### $(selector).slideUp( [duration ] [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例:  
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>slideUp demo</title>
  <style>
  div {
    background: #3d9a44;
    margin: 3px;
    width: 80px;
    height: 40px;
    float: left;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
Click me!
<div></div>
<div></div>
<div></div>
<div></div>
<div></div>
 
<script>
$( document.body ).click(function() {
  if ( $( "div" ).first().is( ":hidden" ) ) {
    $( "div" ).show( "slow" );
  } else {
    $( "div" ).slideUp();
  }
});
</script>
 
</body>
</html>
```

## slideToggle()
描述: 切换向上收起，向下展开动画
用法: 
### $(selector).slideToggle( [duration ] [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>slideToggle demo</title>
  <style>
  p {
    width: 400px;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<button>Toggle</button>
<p>
  This is the paragraph to end all paragraphs.  You
  should feel <em>lucky</em> to have seen such a paragraph in
  your life.  Congratulations!
</p>
 
<script>
$( "button" ).click(function() {
  $( "p" ).slideToggle( "slow" );
});
</script>
 
</body>
</html>
```
## stop()
描述: 停止正在进行的动画
用法: 
### $(selector).stop( [queue ] [, clearQueue ] [, jumpToEnd ] )
参数类型:  
* queue: 动画队列名，字符串
* clearQueue: 是否清空队列，布尔值，默认false
* jumpToEnd: 是否立刻结束动画，布尔值，默认false
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>stop demo</title>
  <style>
  .block {
    background-color: #abc;
    border: 2px solid black;
    width: 200px;
    height: 80px;
    margin: 10px;
  }
  </style>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<button id="toggle">slideToggle</button>
<div class="block"></div>
 
<script>
var $block = $( ".block" );
 
// Toggle a sliding animation animation
$( "#toggle" ).on( "click", function() {
  $block.stop().slideToggle( 1000 );
});
</script>
 
</body>
</html>
```

## toggle()
描述: 切换隐藏和显示匹配元素
用法: 
### $(selector).toggle( [duration ] [, complete ] )
参数类型:  
* duration: 数字或字符串，表示动画持续时间。默认值400，单位毫秒
* complete: 动画完成的回调函数
用例：
```
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>toggle demo</title>
  <script src="https://code.jquery.com/jquery-3.4.1.js"></script>
</head>
<body>
 
<button>Toggle</button>
<p>Hello</p>
<p style="display: none">Good Bye</p>
 
<script>
$( "button" ).click(function() {
  $( "p" ).toggle();
});
</script>
 
</body>
</html>
```
