title: Javascript设计模式 - 装饰者模式
date: 2014-04-10 16:59:24
tags: [Javascript, 设计模式]
categories: book
---

装饰者模式用来透明地把对象包装在具有同样接口的另一对象之中。

### 基本结构
```js
// 抽象类（装饰者基类）
var Decorator = function(Obj) {
  this.obj = obj;  
};
Decorator.prototype = {
  method1: function() {
    return this.obj.method1();
  },
  method2: function() {
    return this.obj.method2();
  }
};

// 具体类（具体装饰者1）
// 需要重新定义特定的方法
var SomeDecorator = function(obj) {
  SomeDecorator.superClass.constructor.call(this, obj);
};
SomeDecorator.prototype = {
  method1: function() {
    return this.obj.method1() + 'some';
  },
  method2: function() {
    return this.obj.method2() + 'some';
  }
};
extend(SomeDecorator, Decorator);

// 具体类（具体装饰者2）
// 需要重新定义特定的方法
var OtherDecorator = function(obj) {
  someDecorator.superClass.constructor.call(this, obj);
};
someDecorator.prototype = {
  method1: function() {
    return this.obj.method1() + 'other';
  },
  method2: function() {
    return this.obj.methdo2() + 'other';
  }
};
extend(OtherDecorator, Decorator);

// 实现类
var Class1 = function() {};
Class1.prototype = {
  method1: function() {
    return 'method1:';
  },
  method1: function() {
    return 'method2:';
  }
};

// 没有装饰者的使用
var obj = new Class1();
alert(obj.method1()); // 'method1'

// 使用装饰者的情况
// 实现对obj方法的多种装饰
var objSome = new SomeDectorator(obj);
var objOther = new OtherDectorator(obj);
alert(objSome.method1());  // 'method1:some'
alert(objOther.method2()); // 'method2:other'

////////////////////////////////////////////
// 函数装饰者

// 装饰者
function upperCaseDecorator(func) {
  return function() {
    func.apply(this, arguments).toUpperCase();
  };
}

// 普通方法
function getDate() {
  return (new Date()).toString();
}

// 装饰者方法
var getDateDecorator = upperCaseDecorator(getDate);
alert(getDate());          // 'Thu Apr 10 2014 19:02:57 GMT+0800 (中国标准时间)'
alert(getDateDecorator()); // 'THU APR 10 2014 19:02:57 GMT+0800 (中国标准时间)'
```

### 主要用途
- 为对象、方法添加功能


### 心得体会
装饰者模式好处在于，可以为方法动态地扩展且不需要修改实现类。  
即，可以通过实现N多个**Decorator得到N多个不同的实例对象，而不需要实现、维护N多个子类。