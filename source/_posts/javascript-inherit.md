title: Javascript继承
date: 2014-04-11 10:04:23
tags: [Javascript, utils]
categories: diary
---


### 类式继承
```js
function extend(subClass, superClass) {
  // 通过一个空函数创建的对象插入到原型链，
  // 是为了避免创建超类的实例，因为超类可能很庞大，
  // 而且超类的构造函数可能包含大量的初始化操作
  var F = function() {};
  F.prototype = superClass.prototype;
  subClass.prototype = new F();

  // 定义一个构造函数式，其默认的prototype对象是一个Object类型实例，
  // 其constructor属性会被自动设置成构造函数本本身。
  // 但是修改其prototype属性后，新的prototype对象自然就不包含原有的构造函数
  // 因此，需要手动设置其constructor为构造函数本身
  subClass.prototype.constructor = subClass;

  // 为子类添加超类属性，是为了弱化其余超类的耦合
  // 即在子类构造函数中不用知道超类的具体名字
  subClass.superClass = superClass.prototype;

  // 确保超类是Object本身时，其prototype的constructor也能被正确设置
  if (superClass.prototype.constructor == Object.prototype.constructor) {
    superClass.prototype.constructor = superClass;
  }
}

// example
function Person(name) {
  this.name = name;
};
Person.prototype = {
  getName: function() {
    return this.name;
  }
};


function Author(name, blog) {
  Author.superClass.call(this, name);
  this.blog = blog;
}
extend(Author, Person);
Author.prototype.getBlog = function() {
  return this.blog;
};

var person = new Person('default name');
var author = new Author('fulme', 'www.suluf.com/blog/');
alert(person.getName()); // 'default name'
alert(author.getName()); // 'fulme'
alert(author.getBlog()); // 'www.suluf.com/blog/'
```

### 原型链继承

```js
function clone(obj) {
  function F() {}
  F.prototyp = obj;
  return new F();
}

// example
var Person = {
  name: 'default name',
  getName: function() {
    return this.name;
  }
};

var author = clone(Person);
alert(author.getName()); // 'default name'

author.name = 'fulme';
author.blog = 'www.suluf.com/blog/';
author.getBlog = function() {
  return this.blog;
};
alert(author.getName()); // 'fulme'
alert(author.getBlog()); // 'www.suluf.com/blog/'
```

### 心得体会
原型链继承算是Javascript独有的，实现很简单、随意。  
类式继承看起来很严谨，感觉更优雅，也更容易维护。