title: Javascript设计模式 - 桥接模式
date: 2014-03-24 14:35:38
tags: [Javascript, 设计模式]
categories: book
---

桥接模式的作用在于将抽象与其实现分离，以便各自独立变化。

### 基本结构
```js
// 抽象方法，不随实现的变化而变化
function doSomething(data) {
  // TODO  something
}

// 桥接方法，根据具体实现收集数据
function doSomethingBridge(e) {
  var data = e.data || $(this).attr('data');
  doSomething(data);
}

addEvent(element, Event, doSomethingBridge);
```

### 主要用途
- 事件监听回调函数

为了简洁，不再延伸桥接模式的高级形式。
