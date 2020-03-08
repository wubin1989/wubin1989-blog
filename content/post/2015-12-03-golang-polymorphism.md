---
title:       "Golang多态"
subtitle:    ""
description: ""
date:        2015-12-03
author:      ""
image:       ""
tags:        ["backend", "golang"]
categories:  ["Tech"]
archives:    "2015"
---

多态是指同一个对象的某一种行为在不同情况下有不同的表现。用在现实生活中，就比如一个女人同时拥有多重角色，在家庭里面对丈夫，是妻子，面对孩子，是母亲，在公司，可能是财务经理，在瑜伽课上，可能是学员，当她在不同的场合，以不同的身份做一个相同行为的时候，具体做法可能完全不同。用在程序中，就是一个接口可以有多个实现类，接口中的方法可以根据调用该方法的具体对象的不同，而做不同的事情。有java背景的程序员都学过多态的三个表现形式：  
* 继承
* 重写
* 父类引用指向子类对象  

golang中的多态，跟java中的多态有类似的地方，但是golang中没有类的概念，也没有继承。golang的多态通过接口`interface`和结构体`struct`来实现，如下面的例子：  
&nbsp;
```
package main

import "fmt"

type Talker interface {
        Talk(words string)
}

type Cat struct {
        name string
}

type Dog struct {
        name string
}

func (c *Cat) Talk(words string) {
        fmt.Printf("Cat " + c.name + " here: " + words + "\n")
}

func (d *Dog) Talk(words string) {
        fmt.Printf("Dog " + d.name + " here: " + words + "\n")
}

func main() {
        var t1, t2 Talker

        t1 = &Cat{"Kit"}
        t2 = &Dog{"Doug"}

        t1.Talk("meow")
        t2.Talk("woof")
}
```
&nbsp;   
上面的代码中，首先定义了一个接口`Talker`，里面有一个待实现的方法`Talk(words string)`，然后定义了两个结构体`Cat`和`Dog`。golang中的接口是非侵入式的接口，也就是结构体想实现某个接口不需要像java那样在定义类的时候用`implement`关键字写明要实现哪个接口，只需要实现某个接口定义的全部方法即可。`Cat`和`Dog`都实现了`Talk`方法，也就都实现了`Talker`接口。在`main`方法中，首先定义了接口`Talker`类型的变量`t1`、`t2`，然后将`Cat`和`Dog`的实例分别赋值给了这两个变量，是不是跟java里的"父类引用指向子类对象"很接近？在golang里是"接口引用指向实现结构体对象"。最后`t1`、`t2`各自调用`Talk`方法，实际执行的是各自指向的实现结构体的同名方法。因为结构体`Cat`和`Dog`各自定义的`Talk`方法的行为不同，所有`t1`、`t2`的`Talk`方法也表现出不同的行为。
