---
title:       "Angularjs拦截器的例子"
subtitle:    ""
description: ""
date:        2015-12-24
author:      ""
image:       ""
tags:        ["frontend", "angularjs"]
categories:  ["Tech"]
archives:    "2015"
---

```
var interceptor = function ($q, $location) {
    return {
        request: function (config) {
            console.log(config);
            return config;
        },

        response: function (result) {
            console.log('Repos:');
            result.data.splice(0, 10).forEach(function (repo) {
                console.log(repo.name);
            })
            return result;
        },

        responseError: function (rejection) {
            console.log('Failed with', rejection.status, 'status');
            if (rejection.status == 403) {
                $location.url('/login');
            }

            return $q.reject(rejection);
        }
    }
};

angular.module('app', [])
    .config(function ($httpProvider) {
        $httpProvider.interceptors.push(interceptor);
    })
    .run(function ($http) {
        $http.get('https://api.github.com/users/bclinkinbeard/reposefw');
    });
```
