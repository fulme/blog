title: Javascript接口
date: 2014-04-25 10:45:09
tags: [Javascript, utils]
categories: diary
keywords: Javascript, Interface, 接口, Javascript 接口
---

接口提供了一种说明一个对象应该具有哪些方法的手段。  
Javascript语言本身没有内置接口实现，下面通过一个工具类模拟实现了此功能。

### 实现
```js
/**
 * 构造函数
 * @param {String} name    类名
 * @param {Array.<String>} 类方法名列表
 */
var Interface = function (name, methods) {
  if (arguments.length < 2) {
    // 参数数目不对，抛异常
    throw Error('Interface constructor called with' + 
        arguments.length +
        'arguments, but excepted exactly 2.');
  }

  this.name = name;
  this.methods = methods;

  for (var i = 0, len = methods.length; i < len; i++) {
    var method = methods[i];

    if (typeof method !== 'string') {
      // Interface 构造函数只接受字符串个是的方法名
      throw Error('Interface constructor excepts method names to be passed in as string');
    }

    this.methods.push(method);
  }
};

// Interface的静态方法
Interface.ensureImplements = function (obj) {
  if (arguments.length < 2) {
    // 参数数目不对，抛异常
    throw Error('Interface constructor called with' + 
        arguments.length +
        'arguments, but excepted exactly 2.');
  }

  // 遍历接口(arguments[0]即obj是待检测的对象)
  for (var i = 1, len = arguments.length; i < len) {
    var interface = arguments[i];
    var methods = interface.methods;
    
    // 如果不是Interface的实例，抛异常
    if (interface.constructor !== Interface) {
      throw Error('Function Interface.ensureImplements 
        excepts arguments two and above to be instances of Interface');
    }

    // 检查对象是否实现了接口的方法
    for (var j = 0, methodsLen = methods.length; j < methodsLen; j++) {
      var method = methods[j];

      // 如果没有实现任意一个接口的方法都抛异常
      if (!(obj[method] && typeof obj[method] !== 'function')) {
        throw Error('obj does not implement the ' + interface.name + 
            ' interface.method ' + method +  ' was not fond.');
      }
    }
  }
};
```

### 例子
``` js
var Interface1 = new Interface('Interface1', ['method1', 'method2']);
var Interface2 = new Interface('Interface2', ['method3', 'method4']);
function SomeClass = function () {}
// 实现Interface1和Interface2的方法
someClass.prototype = {
  method1: function() {},
  method2: function() {},
  method3: function() {},
  method4: function() {}
};


function OhterClass = function (someObj) {
  // 如果someObj号称实现了Interface1和Interface2两个接口，
  // 则可以通过下边这一句调用对其进行检测
  Interface.ensureImplements(someObj, Interface1, Interface2);
  
  this.someObj = someObj;
  // TODO something
}

// 调用
OhterClass(new SomeClass());
```
### 心得体会
接口的应用可以很好地解耦各个模块，有利于大项目、多人合作开发。  
其缺点是实现比较繁琐，也会增加很多的函数调用从而会影响性能，多多少少也有悖Javascript灵活性的特点。  
在实际的开发中还没有用过，通常在实现一个类或者对象的时候显示地暴露+在模块文件描述的时候说明一下接口（方法）即可。