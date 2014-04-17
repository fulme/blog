title: Javascript设计模式 - 工厂模式
date: 2014-03-20 16:33:37
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,design patterns,设计模式,factory pattern,工厂模式
---

工厂模式是一种有助于消除两个类之间依赖性的模式，它使用一个方法来决定究竟实例化哪一个具体类。

### 基本结构
```js
var Factory = function() {};
Factory.prototype = {
  method1: function(model) {
    var obj;

    switch (model) {
      case 'model1':
      obj = new Class1();
      break;
      case 'model2':
      obj = new Class2();
      break;
    }

    return obj;
  },

  method2: function() {}
};
```
### 主要用途
- 所实例化的类的类型不能在开发期间确定，就是随时可能有新的类加入的情况
- 因环境不同可能有不同实例的情况（如：跨浏览器的Ajax）。

同样为了简洁，不再延伸此模式的高级形式。