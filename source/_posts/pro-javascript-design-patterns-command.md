title: Javascript设计模式 - 命令模式
date: 2014-05-06 20:05:49
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,Design Patterns,command pattern,设计模式,命令模式
---

### 基本结构
```js
var Class = function() {};
Class.prototype = {
  method1: function() {},
  method2: function() {}
};

function makeCommand1(obj) {
  return function() {
    obj.method1();
  };
}

function makeCommand2(ojb) {
  return function() {
    obj.method2();
  };
}

var obj = new Class();
var cmd1 = makeCommand1(obj);
var cmd2 = makeCommand2(obj);

// 注意：下面这行注释是重点！！！
// 可以将cmd1、cmd2传送到任何地方、任何时候执行
cmd1();
cmd2();
```

### 主要用途
- UI与实现分离，可以完全独立变化

### 心得体会
为了简洁，上面提到的结构是最简单的形式。  
从上面的基本结构来看，就是做一些封装的活，感觉也没太大的意义，但是后面的那句注释是重点。  
真是的项目中还是能用的上的，这样做可以极大地提高组件的复用，否则与用户打交道的UI及其相关的逻辑很难实现复用。
