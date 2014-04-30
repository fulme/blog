title: Javascript设计模式 - 代理模式
date: 2014-04-25 09:27:09
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,design patterns,设计模式,proxy pattern,代理模式
---

代理模式最基本的形式就是实现对另外的对象（本体）进行访问控制，实现与本体相同的接口，也是是一种优化模式。  
**注意：**代理对象不添加新的功能，具体的功能都在本体实现。  

### 基本结构
```js
// 实现类
var RealClass = function (Obj) {
  this.obj = obj;
  this.properties = [];

  // 大量（耗时、耗资源）初始化操作
  for (var i in obj) {
    if (obj.hasOwnerProperty(i)) {
      this.properties.push(i);
      // TODO ...
    }
  }
};
RealClass.prototype = function () {
  method1: function() {},
  method2: function() {}
};

// 代理类
var ProxyClass = function (obj) {
  // 缓存构造器所需参数
  this.obj = obj;

  // 缓存具体类的实例
  this.realObj = null;
};
ProxyClass.prototype = {
  // 辅助方法，用于延迟初始化具体的对象
  // 仅仅在对象真正被调用的时候才初始化
  initialize_: function () {
    if (!this.realObj) {
      this.realObj = new RealClass(this.obj);
    }
  },

  // 实现与RealClass相同的接口
  method1: function() {
    this.initialize_();
    this.realObj.method1();
  },
  method2: function() {
    this.initialize_();
    this.realObj.method1();
  }
};


// 调用
var data = {a: false, b: 0, c: '2'};
var obj = new ProxyClass(data);
obj.method1();
obj.method2();
```

### 主要用途
- 控制对创建开销昂贵对象的访问

### 心得体会
代理模式是一种优化模式，同时也增加一个新的辅助类，代码复杂度增加倒是不明显也不难理解。
