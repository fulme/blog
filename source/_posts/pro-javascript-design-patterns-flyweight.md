title: Javascript设计模式 - 享元模式
date: 2014-04-22 18:31:08
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,design patterns,设计模式,flyweight pattern,享元模式
---


享元模式是一种优化方法，用于降低内存占用。

### 基本结构
```js
// 首先看一个没有经过优化的类
var Car = function(make, model, year, owner, tag, renewDate) {
  this.make = make;
  this.model = model;
  this.year = year;
  this.owner = owner;
  this.tag = tag;
  this.renewDate = renewDate;
};
Car.prototype = {
  getMake: function() {return this.make},
  getModel: function() {return this.model},
  getYear: function() {return this.year},
  getOwner: function() {return this.owner},
  getTag: function() {return this.tag},
  getRenewDate: function() {return this.renewDate}
};

// example
var car1 = new Car('Audi', 'Q7', '2014', 'fulme', '888888', '2014-04-22');
var car1 = new Car('BMW', 'x1', '2013', 'fulme', '666666', '2013-04-22');

// 用享元模式优化以后的版本
var FlyweightCar = function(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
};
FlyweightCar.prototype = {
  getMake: function() {return this.make},
  getModel: function() {return this.model},
  getYear: function() {return this.year}
};

// 创建Car的工厂，为了复用同一车型的实例，
// 避免重复创建Car的实例，从而降低内存开销
var carFactory = (functino() {
  var cars = {};

  return {
    create: function(make, model, year) {
      var id = [make, model, year].join('-');
      var car = cars[id];

      if (!car) {
        car = new Car(make, model, year);
        cars[id] = car;
      }

      return car;
    }
  };
})();

// 管理器
// 避免创建多个实例对象
var recordManager = (function() {
  var records = {};

  return {
    addCarRecord: function(make, model, year, owner, tag, renewDate) {
      var car = carFactory.create(make, model, year);
      records[tag] = {
        owner: owner,
        car: car,
        renewDate: renewDate
      };
    },
    getOwner: function(tag) {
      return records[tag] ? records[tag].owner:null;
    },
    getRenewDate: function(tag) {
      return records[tag] ? records[tag].renewDate:null;
    }
  };
})();

// example
var car1 = recordManager.addCarRecord('Audi', 'Q7', '2014', 'fulme', '888888', '2014-04-22');
var car1 = recordManager.addCarRecord('BMW', 'x1', '2013', 'fulme', '666666', '2013-04-22');
```

### 主要用途
- 优化类的实现，减小内存开销
- 弱化类与其外部属性的耦合

### 心得体会
享元模式的优势是很明显。不管是性能还是可重用性、可维护性、可扩展性都得到有效提高。  
唯一的瑕疵在于增加实现的复杂度，不利于新手理解和开发。