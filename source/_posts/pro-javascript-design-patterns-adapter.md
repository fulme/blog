title: Javascript设计模式 - 适配器模式
date: 2014-03-31 17:17:55
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,design patterns,设计模式,adapter pattern,适配器模式
---

适配器模式可用来在现有接口与不兼容的类之间进行适配。

### 基本结构
```js
// 现有接口
function existFunc(str1, str2, str3) {}

// 适配器
function adapterFunc(obj) {
  existFunc(obj.string1, obj.string2, obj.string3);
}

// 新的对象
var newObj = {
  string1: '',
  string2: '',
  string3: '',
  ...
};

// 调用
adapterFunc(newObj);
```

### 主要用途
- 使旧的接口和新的类、对象兼容

### 心得体会
这个设计模式是很容易理解的。
