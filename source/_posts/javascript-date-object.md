title: Javascript Date 对象
date: 2014-05-07 17:18:49
tags: [Javascript]
categories: diary
keywords: Javascript,Date,日期对象
---

[Date 对象](http://www.w3school.com.cn/jsref/jsref_obj_date.asp)的详细描述在这里就不照搬了，重点说几个平时可能忽略的点。

### 获取当前日期
获取当前日期有两种方法：`Date()`和`new Date()`。

- Date作为构造器调用：
```js
// 返回距离GMT+stamp毫秒数对应的时间
// 不传参数返回当前时间
var date = new Date(stamp);

// 参数可以是多个，类型可以是字符串，但是怎么转换的不清楚，
// 起码不是强制转换成Number那么简单，感兴趣的同学可以研究一下
```

- Date作为函数调用：
```js
// args可以是任意参数，都会被忽略
var date = Date(args);
```

### 性能测试
在chrome上测试发现new Date()的性能明显比Date()高。
```js
var testTimes = 10000000;

// new Date()
console.time('new Date');
for (var i=0; i<testTimes; i++) {
  new Date();
}
console.timeEnd('new Date');

// Date()
console.time('Date');
for (var i=0; i<testTimes; i++) {
  Date();
}
console.timeEnd('Date');

// new Date: 11455.000ms
//     Date: 18292.000ms
```

### 几种情况
```js
var a = Date(0);
var b = new Date(0);
var c = new Date();

console.log(a != b);//true
console.log(b != c);//true
console.log(a == c);//false

console.log(a !== c);//false!!
```