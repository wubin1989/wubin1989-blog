---
title:       "angularjs自定义双向绑定元素"
subtitle:    ""
description: ""
date:        2015-10-03
author:      "wubin1989"
image:       ""
tags:        ["frontend", "angularjs"]
categories:  ["Tech" ]
archives:    "2015"
---

```
<div contentEditable="true" ng-model="content" title="Click to edit">Some</div><pre>model = {{content}}</pre>

<style type="text/css">
  div[contentEditable] {
    cursor: pointer;
    background-color: #D0D0D0;
  }</style>
```
```
angular.module('form-example2', []).directive('contenteditable', function() {
  return {
    require: 'ngModel',
    link: function(scope, elm, attrs, ctrl) {
      // view -> model
      elm.on('blur', function() {
        ctrl.$setViewValue(elm.html());
      });

      // model -> view
      ctrl.$render = function() {
        elm.html(ctrl.$viewValue);
      };

      // load init value from DOM
      ctrl.$setViewValue(elm.html());
    }
  };});
```