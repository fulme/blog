title: nodejs新特性cluster的使用
date: 2014-02-08 16:46:10
categories: blog
tags: [NodeJS]
---

------

Q：cluster是什么？cluster应该怎么用？cluster是多进程吗？  
A：[nodejs](http://yoyo.play175.com/tag/nodejs/)增加了一个模块cluster用于实现node的多进程，其实这是对子进程（child_process）操作的一个封装，让我们更简单的使用多进程来增加程序在多核CPU机器上的性能表现。
<!--more-->
关于cluster的使用，新手或者不了解真相的童鞋很容易犯错。举个例子：
```javascript
var cluster = require('cluster'); 
var share_var = 0;//这里定义数据不会被所有进程共享，各个进程有各自的内存区域 
if (cluster.isMaster) { 
    //当前代码在主进程中 
    var numCPUs = require('os').cpus().length; 
    for (var i = 0; i < numCPUs; i++) { 
        var worker = cluster.fork(); 
    }    
    share_var++; //这里操作的是当前进程中的数据 
} else { 
     //当前代码在子进程中,会被调用numCPUs次 
    share_var++; //这里操作的是当前进程中的数据 
} 
```

不少人以为在全局定义了变量share_var，可以被master和所有worker共享，其实各个进程是不能共享这个变量的。
如果需要共享数据，需要在进程间使用消息通知来达到这个目的。官方文档中的例子很好的说明了进程间如果共享数据：
```javascript
var cluster = require('cluster'); 
var http = require('http'); 
 
if (cluster.isMaster) { 
  var numCPUs = require('os').cpus().length; 
  var numReqs = 0; 
  // 启动多个进程. 
  for (var i = 0; i < numCPUs; i++) { 
   //增加一个进程 
    var worker_process = cluster.fork(); 
    //侦听子进程的message事件 
    worker_process .on('message', function(msg) { 
      if (msg.cmd && msg.cmd == 'notifyRequest') { 
        numReqs++; 
      } 
    }); 
  } 
  //输出变量值 
  setInterval(function() { 
    console.log("numReqs =", numReqs); 
  }, 1000); 
} else { 
  // 每个子进程都有一个http server，他们共享一个8000端口，所以请求时会同时被调用 
  http.Server(function(req, res) { 
    res.writeHead(200); 
    res.end("hello world\n"); 
    //子进程发出一个消息
    process.send({ cmd: 'notifyRequest' }); 
  }).listen(8000); 
} 
```
通过任务管理器（Process Explorer）可以看到，node开了两个子进程：
![][1]

------

作者：YoYo，原文地址：[http://yoyo.play175.com/p/node-cluster.html](http://yoyo.play175.com/p/node-cluster.html)

  [1]: http://yoyo.play175.com/usr/uploads/2012/04/2194592566.jpg
  [2]: http://www.zybuluo.com/mdeditor?url=http://www.zybuluo.com/static/editor/md-help.markdown
  [3]: http://weibo.com/ghosert