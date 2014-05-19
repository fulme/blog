title: NodeJS+Mongodb 服务端实现
date: 2014-05-12 14:09:37
tags: [NodeJS, Mongodb]
categories: blog
keywords: NodeJS, Mongodb
---

本文是基于一个真实的项目X的总结。
X项目是一次实验性的尝试，之前本人从来没有涉足过服务器端。
在此仅仅是个总结，方便以后查询，绝不是什么教程。
当然，如果有幸对你有所帮助，纯属巧合，不喜勿喷啦啦啦~~

### 学习目标
- *项目组织结构*
- *遇到的问题*
- *优化解决方案*

## 项目背景
通常情况下，服务端的实现是由公司专门的服务端团队负责的。
但是，由于服务端人力资源有限，排队已经在1个月开外了。
于是乎怀着对服务端、新技术的好奇，开启了漫长的[NodeJS](http://nodejs.org/)+[Mongodb](http://www.mongodb.org/)探索之旅。  
选用NodeJS+Mongodb的原因：
- *本人日常工作是JS开发，选用NodeJS没有重新学习语言的成本*
- *NodeJS+Mongodb是一种新的技术，当然也有相比于PHP等其它后端语言不具备的优势*
- *公司已经有实际项目运用了NodeJS+Mongodb的架构，且拥有专门提供存取服务的Mongodb集群*

## 项目组织结构
既然是一次尝试，难免就要折腾！从开始折腾到这是发布上线大致经历了一下三个阶段。  
**说明一下：**下面的代码是不完备的，仅仅是摘出来的关键代码，用于勾勒整体结构。

### 第一阶段 - 开发机
首先是申请了一台开发机，通过[Xshell](http://www.netsarang.com/products/xsh_overview.html)连接上。
然后就是哐叽哐叽一通折腾，安装了Nodejs和Mongodb。

之后就是**demo**(server.js)：
```js
 //  载入依赖模块
var http = require('http');
var express = require('express'); // 需要安装

// 初始化模块
var server = module.exports = express.createServer();

// 初始化server
server.configure(function () {
  server.use(server.router);
}).listen(3000);

// 监听GET请求
server.get('/', function(req, res) {
  var params = req.query;
  for (var key in params) {
    console.log(params[key]);
  }

  // TODO 业务逻辑
});
```
到此为止一个简易Nodejs服务器已经实现了，在命令行执行Node脚本即可：
```shell
node server.js
```
然后在浏览器中输入：http://开发机IP:3000?name=fulme，
在Xshell的控制台就可以看到输出：fulme  
这个过程在windows上同样很容易实现，只要装上NodeJS并根据官网的[EXAMPLE](http://nodejs.org/)稍作改动就可以完成。

### 第二阶段 - 开发机 + Mongodb测试机

### 第三阶段 - 线上机 + Mongodb线上机
4台Nginx+NodeJS+Mongodb集群

## 遇到的问题
- 并发性

- 查询性能

## 优化解决方案
- 并发性
- 查询性能

## 心得体会
NodeJS业务逻辑以及Mongodb操作语句的实现其实很容易入门。
难点在于对后端环境太陌生，没有掌控感，感觉未知数太多。
另外，没有对数据库操作的性能优化的经验，导致多次提测都达不到预期的效果，经历N久的尝试才勉强达到预期。
