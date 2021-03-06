title: Javascript设计模式 - 单体模式
date: 2014-03-20 14:04:59
tags: [Javascript, 设计模式]
categories: book
keywords: Javascript,design patterns,设计模式,singleton pattern,单体模式
---

[Javascript设计模式](http://www.apress.com/9781590599082)这本书值得一看。
本系列文章都是这本书的学习笔记。书写的再好，还是会有一些上下文情景或者是一些相关说明与介绍。
虽然此书的每一个部分都很精彩，内容也很精简，但是为了方便自己理解，还是决定做一个笔记以加深映象。


单体模式主要用于划分命名空间或者说模块划分，将一批有一定关联的属性和方法组织在一起。  

### 基本结构如下
``` js
var sigleton = {
  // 属性
  attr1: true,
  attr2: 10,

  // 方法
  method1: function() {},
  method2: function() {},
};
```
### 主要用途
- 将一个比较大功能细分成几个小的模块，每个模块独占一个文件和一个全局名称
- 将一些功能性的函数（不太会调用其他模块函数）组织在一个文件（模块）中，方便各处调用。

当然此模式还有一些高级形式，为了简洁暂不介绍。