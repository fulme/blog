title: hexo源码解析系列（零）
date: 2014-02-28 17:01:52
tags: [hexo, NodeJS, source]
categories: [code]

---

####*本来是想整理一下hexo搭建博客的教程。但是后来想想没啥意思，[官网](http://zespia.tw/hexo/)已经写的很详尽了。本系列号称源码解析，但事实是本人接触[NodeJs](http://nodejs.org/)也不多，看hexo的代码比较费力，所以绝对是本着学习的态度，当着记笔记了。写得不好的地方，望大家广泛讨论，多多指导，不喜请轻拍。当然如果你愿意花掉时间阅读一下，本人不甚荣幸！*  
---


关于hexo的简介这里就不写了，前面的[一篇](/blog/2014/02/11/hexo-guide-0/)也粗略的讲了一下。本系列着重分析`hexo-2.4.5`版本的源码。  

##目录结构  
由于空间的关系，仅列出实现部分代码目录结构。由于目录的结构随着版本的更迭会不断发生变化，所以列出目录的目的不是要讲解目录的安排的合理性，而是为了有个整体印象和方便理全面梳理代码。

``` md
. 
├── bin
  ├── hexo
├── lib
  ├── core
  ├── error
  ├── extend
  ├── loader
  ├── logger
  ├── model
  ├── plugins
  ├── post
  ├── util
  ├── init.js
```

bin目录下仅有个名为`hexo`的node可执行脚本，这个脚本在使用npm安装hexo后会放置到`%appdata%/npm/node_modules/hexo/bin`，被`%appdata%/npm/hexo.cmd`调用。实现代码都在lib目录。

## hexo文件
hexo文件就一行代码：

``` cmd
require('../lib/init')(process.cwd(), require('optimist').argv);
```
[process.cwd()](http://nodejs.org/api/process.html#process_process_cwd)的作用就是获取当前工作环境目录（调用node命令的目录）。
[require('optimist').argv](https://github.com/substack/node-optimist)的作用就是解析命令行参数。
因此，hexo文件的这一行代码的作用就是，将当前工作目录和命令行参数传递给`init`模块。

到此，第一部分就讲完了，下一篇将分析`init`模块。