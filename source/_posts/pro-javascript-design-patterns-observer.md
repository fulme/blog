title: Javascript设计模式 - 观察者模式
date: 2014-04-30 15:51:19
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,design patterns,设计模式,observer pattern,观察者模式
---

观察者模式也称发布者 - 订阅者模式，其实就是实现一个事件管理器。   
跟`jQuery`的[on](http://api.jquery.com/on/)和[trigger](http://api.jquery.com/trigger/)事件机制是一致的。

### 基本结构
```js
// 事件管理器 类
function EventManager() {
  this.handlers = {};

  // 事件监听
  this.addListener = function(name, handler) {
    (this.handlers[name]||(this.handlers[name]=[])).push(handler);
  };
};
EventManager.prototype = {
  /**
   * 注册事件监听函数，有三种调用方式：
   * on('evt', evtFunc);
   * on(['evt1', evt2], evtFunc);
   * on({'evt1': evtFuc1, 'evt2': evtFunc2});
   *
   * @param  {String|Array<String>|Object<String, Function>} arg1
   * @param  {Function|undefined} arg2
   */
  on: function(arg1, arg2) {
    var arg1Type = Object.prototype.toString.call(arg1);
    var arg2Type = Object.prototype.toString.call(arg2);

    if(arg1Type == '[object String]'){
      this.addListener(arg1,arg2);
    } else if(arg1Type == '[object Array]'){
      for(var i=0,len=arg1.length;i<len;i++){
        this.addListener(arg1[i],arg2);
      }
    } else if(arg1Type == '[object Object]'){
      for(var evt in arg1){
        this.addListener(evt,arg1[evt]);
      }
    } else{
      throw new Error('arguments error');
    }
  },

  /**
   * 注销事件监听
   * @param  {String} name
   * @param  {Function} handler
   */
  un: function(name, handler) {
    if (this.handlers[name]) {
      var hs = this.handlers[name];
      this.handlers[name] = hs.filter(function(h) {retrun h != handler});
    }
  },

  /**
   * 触发事件
   * @param  {String} name
   * @param  {*} args
   */
  fire: function(name, args) {
    if (this.handlers[name]) {
      var hs = this.handlers[name];
      hs.every(function(h) {h(args)});
    }
  }
};


// example
// 实例化事件管理器
var evtMgr = new EventManager();

// 订阅一个事件
evtMgr.on('something', function(args) {
  // 当事件发生时需要处理的事情
});

// 发布一个事件
evtMgr.fire('something', {});
```

### 主要用途
- 事件管理器

### 心得体会
统一的事件管理在模块化开发且个模块实现相互有实时数据依赖的情况下很有用。  
比如，在一个模块删除了一些数据，其它模块可能也需要做一些调整。  
虽然这种情况也可以通过接口的方式实现，但是带来的麻烦就是增加了各模块之间的耦合。
