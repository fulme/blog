title: Javascript设计模式 - 门面模式
date: 2014-03-31 11:41:50
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,design patterns,设计模式,facade pattern,门面模式
---

所谓门面，就是一个功能的快捷入口，就如window系统桌面的一个快捷方式，点击以后会执行快捷方式对应的一个应用程序。
门面模式就是用来实现门面功能的。

### 基本结构
```js
// 跨浏览器的事件绑定
function addEvent(elem, type, fn) {
  if (window.addEventListener) {
    elem.addEventListener(type, fn);
  } else if (window.attachEvent) {
    elem.attachEvent('on' + type, fn);
  } else {
    elem['on' + type] = fn;
  }
}
```

### 主要用途
- 简化类的接口
- 消除类与调用者的耦合


### 心得体会
跟前面降到的[工厂模式](/blog/2014/03/20/pro-javascript-design-patterns-factory/)有些类似。  
说说自己的理解吧，工厂模式强调生产出某个类的实例，类的种类可能随时增加。  
门面模式强调简洁、一致的功能调用入口，就是一个方法，用于实现特定的功能。