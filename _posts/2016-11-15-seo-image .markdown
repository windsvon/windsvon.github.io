---
layout:     post
title:      "理解Javascript闭包"
date:       2016-10-15
author:     "Yu"
header-mask: 0.6
catalog: true
tags:
    - Javascript
---

# 什么是闭包

> **Closures are functions that have access to variables from another function's scope.** -- *Professional Javascript for Web Developers, 3rd Edition*

闭包是一个可以访问父函数作用域中变量的子函数，其可以访问自身作用域，父函数作用域以及全局作用域。闭包通常通过在一个函数中返回另一个函数来实现，例如一个最简单的闭包：

```js
function showNumber() {
  var num = 10;
  return function () {
    console.log(num)
  }
}
var show = showNumber()
show() //10
```

对于普通函数，当函数执行之后，局部变量会被销毁，只保留全局变量，而通过闭包可以避免局部变量的回收。

内部函数不仅可以访问外部函数的变量，还可以直接访问外部函数的参数，例如：

```js
function createComparisonFunction(propertyName) {
  return function(object1, object2) {
    var value1 = object1[propertyName];
    var value2 = object2[propertyName];
    
    if (value1 < value2) {
      return -1;
    } else if (value1 > value2) {
      return 1;
    } else {
      return 0;
    }
  };
}

var compare = createComparisonFunction("num");
var result = compare({ num: 10 }, { num: 20});

console.log(result); //-1
```

这个例子中返回的匿名函数的作用域链包含`createComparisonFunction()`的作用域，因此即使这个匿名函数在其他地方被调用时，依然可以调用到`propertyName`这个变量。


`createComparisonFunction()`返回的匿名函数被储存在变量`compare`中，这个匿名函数可以访问`createComparisonFunction()`函数中的所有变量。同时，当`createComparisonFunction()`执行结束后，其中的局部变量不会被回收，因为这些变量仍然在被返回的匿名函数引用。如果想回收这些变量，需要添加：

```js
compare = null;
```

## 闭包的副作用

**闭包储存的是对父函数中变量的引用（reference）而不是这个变量本身！**因此，当闭包被调用之前，如果父函数中的变量发生变化，那个闭包获得的变量值将是变化后的值。这个问题可以通过如下例子来说明：

```js
function celebrityID() {
    var celebrityID = 999;
    //调用这个函数会返回一个包含多个方法的对象
    //这个对象中的所有方法都可以访问父函数中的变量
    return {

        getID: function() {
            //这个子函数会返回最新的celebrityID
            return celebrityID;
        },
        setID: function(theNewID) {
            //这个子函数会更新父函数中的celebrityID
            celebrityID = theNewID;
        }
    }
}

​var mjID = celebrityID (); 

mjID.getID(); // 999​

mjID.setID(567); 

mjID.getID(); // 567
```

由于闭包的这个特性，如果父函数中用到循环，也会产生问题，如下面这个例子：

```js
function createFunctions() {
  var result = new Array();
  
  for (var i=0; i < 10; i++) {
    result[i] = function() {
      return i;
    };
  }
  
  return result;
}
```

表面上来看这个函数会返回一个数组，数组的每个元素是一个函数，其返回这个元素的位置。可实际上每个函数都会返回10，因为每个函数的作用域链上都包含`createFunctions()`的变量对象，其都指向同一个变量，i，当`createFunctions()`执行结束时，变量 i 的值为10，因此每个函数返回的值都为10。如果想使该函数实现预期功能，可做如下修改：

```js
function createFunctions() {
  var result = new Array();
  
  for (var i=0; i < 10; i++) {
    result[i] = function(num) {
      return function() {
        return num;
      };
    }(i);
  }
  return result;
}
```
这个方法中实际上用了一个javascript中很常见的设计模式，**立即执行函数-- Immediately Invoked Function Expression (IIFE)**。

## 立即执行函数（IIFE）

立即执行函数的主要作用是在模块化开发中使用私有变量，防止全局污染，下面是一个常见的立即执行函数的应用，用来创建一个对象：

```js
var person = function() {
    // Private
    var name = "Robert";
    return {
        getName: function() {
            return name;
        },
        setName: function(newName) {
            name = newName;
        }
    };
}();
console.log(person.name); // Undefined
console.log(person.getName()); // "Robert"
person.setName("Robert Nyman");
console.log(person.getName()); // "Robert Nyman"
```
可以看出立即执行函数的好处是你可以自由决定那些变量是私有的，哪些是全局的。

## 小结

闭包被广泛应用于包括jquey在内的各种javascript的库和框架中，同时闭包也是Node.js实现异步，非阻塞架构的基石。作为一个前端开发者，理解闭包可以帮我们更好地理解工作和学习中遇到的各种js代码。

