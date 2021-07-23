---
title: 关于 JavaScript 中 this 对象
date: 2018-09-09 11:34:37
tags: JavaScript
cover: https://library.kissclipart.com/20181128/tsw/kissclipart-orange-clipart-ecmascript-javascript-ecma-internat-3885764678295034.png
---

# this

this是在运行时调用的，它的值取决于函数的调用位置。

## 调用方式

### 独立函数中的 this  

在全局函数中，this 指向全局对象 window。

``` js
var n = 1

function example () {
  this === window // true
  console.log(this.n)
}

example() // 1
```

严格模式下，this 的值为 undefined。

``` js
var n = 1

function example () {
  'use strict'
  this === undefined // true
  console.log(this.n)
}

example() // TypeError: this is undefined
```

但只在严格模式下调用函数不会影响 this 的值

``` js
var n = 1

function example () {
  this === window // true
  console.log(this.n)
}

(function () {
  'use strict'
  example() // 1
})()
```

匿名函数没有绑定任何对象，因此其 this 通常指向 window（作为独立函数执行）。

``` js
var n = 1

var obj = {
  n: 2,
  example: function () {
    return function () {
      return this.n
    }
  }
}

obj.example()() // 1
```

### 作为对象的方法中的 this

调用对象的方法时，方法里的 this 指向调用这个方法的对象

``` js
var obj = {
  example: function () {
    console.log(this)
  }
}

obj.example() // obj
```

对象属性引用链中只有上一层或者最后一层在调用位置中起作用。方法中的 this 只指向当前一层的对象。

``` js
var obj = {
  n: 1,
  obj2: {
    n: 2,
    example: function () {
      console.log(this.n)
    }
  }
}

obj.obj2.example() // 2 this 指向 obj.obj2
// 谁调用函数，函数中的 this 就指向谁。
// obj.example() this 是 obj
// obj.obj2.example() this 是 obj.obj2
// obj.obj2.obj3.example() this 是 obj.obj2.obj3
```

绑定对象丢失

``` js
var n = 1

var obj = {
  n: 2,
  example: function () {
    console.log(this.n)
  }
}

var obj2 = obj.example
obj2() // 1
```

obj2 引用的是 example 函数本身，obj2() 的调用等同于独立函数调用

参数传递就是一种隐式赋值，因此在函数传值也会出现上例的状况。

``` js
var n = 1

var obj = {
  n: 2,
  example: function () {
    console.log(this.n)
  }
}

function example2 (fn) {
  fn()
}

example2(obj.example) // 1
```

js 的原生方法也不例外

``` js
var n = 1

var obj = {
  n: 2,
  example: function () {
    console.log(this.n)
  }
}

setTimeout(obj.example, 0) // 1
```

### 构造函数调用中的 this

构造函数中的 this 指向新创建的实例对象。
在使用 new 运算符来调用构造函数时，会执行一下操作：

1. 创建一个新对象。
2. 将这个新对象的原型指向构造函数的 Prototype。
3. 将这个新对象赋值给函数的 this。
4. 执行构造函数并返回这个新对象（在函数没有返回其他对象的情况下）。

``` js
function Person (name, age) {
  this.name = name
  this.age = age
}

var tom = new Person('tom', 18)
console.log(tom.name, tom.age) // tom 18
```

### 绑定 this

使用 call()、apply() 和 bind() 方法可以指定函数内 this 的指向

- call() 方法的第一个参数是一个对象，在调用函数时将其绑定到 this。其他参数为函数调用时接收的参数。  
  在非严格模式下第一个参数传入 null 或者 undefined 都会被全局对象代替，其他原始值会变为相应的包装对象。
  `fun.call(thisArg, arg1, arg2, ...)`  

- apply() 方法和 call() 类似，区别在于 apply 的第二个参数是一个数组。  
  `func.apply(thisArg, [argsArray])`

``` js
var n = 1
var obj = {
  n: 2
}
var obj2 = {
  n: 3
}

function example () {
  console.log(this.n)
}


example.call(null) // 1
example.call(obj) // 2
example.call(obj2) // 3
```

- bind() 方法返回一个新函数。新函数为原函数的拷贝，this 为方法的第一个参数。

``` js
var n = 1
var obj = {
  n: 2
}

function example () {
  console.log(this.n)
}

var example2 = example.bind(obj)
example2() // 2
```

### 箭头函数中的 this

箭头函数的 this 是当前的词法作用域决定的
箭头函数中的 this 为外层函数的 this（箭头函数定义时所在作用域的 this）。

``` js
var n = 1
var obj = {
  n: 2,
  f: function () {
    // 箭头函数定义时的作用域(箭头函数外层作用域)
    var n = 3
    example = () => {
      // 箭头函数的作用域
      console.log(this.n)
    }
    return example()
  }
}

obj.f() //2
```

箭头函数中的 this 无法被修改

``` js
var n = 1
var obj = {
  n: 2,
  f: function () {
    var n = 3
    example = () => {
      console.log(this.n)
    }
    return example.call({ n: 4 })
  }
}

obj.f() // 2
```
