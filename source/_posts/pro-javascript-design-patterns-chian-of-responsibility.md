title: Javascript设计模式 - 责任链模式
date: 2014-05-08 21:20:39
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,Design Patterns,chain of responsibility pattern,设计模式,责任链模式
---

责任链模式也是一种优化方法，典型的应用场景是配[合组合模式](/2014/03/24/pro-javascript-design-patterns-composite/)使用。

```js
// 组合父类
var CompositeParent = function(id, method, action) {
  this.element = document.createElement('ul');
  this.children = [];
};

// 父类方法
CompositeParent.prototype = {
  hide: function(leaf) {
    this.element.style.display = 'none';
    // 按照组合模式的原理，
    // 此处应该遍历子元素并逐个调用其hide方法
    // 但是元素的css隐藏元素时会自动隐藏起子元素
    // 所以完全没有必要调用
  }
};

// 组合子类
var CompositeLeaf = function(id) {
  this.element = document.createElement('li');
};
CompositeLeaf.prototype = {
  hide: function() {
    this.element.style.display = 'none';
  }
};

// 具体叶子实现类
var Leaf1 = function(id) {
  // init
};
extend(CompositeLeaf, Leaf1); // 继承CompositeLeaf类
Leaf1.prototype = {
  hide: function() {
    // Implement
  },
};

var Leaf2 = function() {
  // init
};

// 实现部分就没什么特别饿了
var parent = new CompositeParent('parent-id', 'POST', '/action.php');
parent.add(new Leaf1('first-name', 'first-name'));
parent.add(new Leaf2('last-name', 'last-name'));

// 只需要调用根节点的方法，其余的子节点同名的方法都会被调用
addEvent(window, 'unload', parent.save);
```

### 主要用途
- 优化性能，阻断一些不必要的处理
- 动态分配任务，用于在运行时将任务分派给最恰当的处理对象

### 心得体会
这个模式用的很多，只是不会感觉到是在用一种模式而已。
典型的应用有事件冒泡处理。