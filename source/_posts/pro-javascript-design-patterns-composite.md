title: Javascript设计模式 - 组合模式
date: 2014-03-24 15:26:55
tags: [Javascript, 设计模式]
categories: book
---

组合模式是专门为创建Web上的动态界面量身定制的。将一批具有相同操作的对象组织成一个树形结构，可以用一条命令在多个对象上激发复杂或者递归调用。

### 基本结构
```js
// 组合父类
var CompositeParent = function(id, method, action) {
  // init
  this.children = [];
};

// 父类方法
CompositeParent.prototype = {
  // Implement

  add: function(leaf) {
    this.children.push(leaf);
  },

  save: function() {
    // 重点就在这里！！！
    this.children.forEach(function(child) {
      child.sava();
    });
  };
};

// 组合子类
var CompositeLeaf = function(id) {
  // init
};
CompositeLeaf.prototype = {
  save: function() {},
};

// 具体叶子实现类
var Leaf1 = function(id) {
  // init
};
extend(CompositeLeaf, Leaf1); // 继承CompositeLeaf类
Leaf1.prototype = {
  save: function() {
    // Implement
  },
};

var Leaf2 = function() {
  // init
};
...

// 实现
var parent = new CompositeParent('parent-id', 'POST', '/action.php');
parent.add(new Leaf1('first-name', 'first-name'));
parent.add(new Leaf2('last-name', 'last-name'));

// 只需要调用根节点的方法，其余的子节点同名的方法都会被调用
addEvent(window, 'unload', parent.save);
```
### 使用条件
- 存在一批组织成某种层次体系的对象（**具体的结构在开发阶段可能无法得知**）
- 希望对这批对象或者其中一部分对象实施一个操作（如：验证数据有效性）

### 主要用途
- 具有树形结构和相同操作的集合（如：图片库展现，一个目录下可能有目录、图片，这种情况，图片就是叶子对象，目录就是根对象或者子对象）
- 子节点个数或者操作实现不确定（如：表单验证，表单的数据项是不容易确定的，且有可能因用户不同而表单区域不一样）

###心得体会
组合模式是目前为止讲到的最难理解的一种设计模式，一开始会觉得很冗余。但是后来渐渐地发现，这个模式带来的益处还是非常大的。
首先是在实现部分处理事情很简单、优雅。最重要的是，代码的重用性和可扩展性非常高，设计模式的精髓啊。但是，能用的场合的确很有限。
