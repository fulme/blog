title: hexo系列教程（一） - hexo简介
date: 2014-02-11 16:10:30
tags: [hexo, NodeJS]
categories: blog
keywords: hexo

---

*声明：此系列教程仅仅是在学习的时候根据其它教程（如：[Zippera's blog](http://zipperary.com/categories/hexo/)）整理，主要是供自己学习及收藏之用，能对您有所帮助，纯属巧合，不喜欢请轻拍，谢谢！*

## hexo是什么

[hexo](http://zespia.tw/hexo/zh-CN/)是基于Node.js的静态博客框架，可以很方便地生成静态博客并托管到[github](https://github.com/)或者[heroku](https://www.heroku.com/)上。引用作者[tommy351](https://github.com/tommy351/hexo)的话：

---

*<center>快速、简单且功能强大的 Node.js 博客框架。*

*A fast, simple & powerful blog framework, powered by Node.js.</center>*

---

类似于jekyll、Octopress、Wordpress，我们可以用hexo创建自己的博客，托管到github或Heroku上，绑定自己的域名，用markdown写文章。本博客即使用hexo创建并托管在github上。

##为什么选择hexo

还是引用下作者的话：

---

*<center>不可思议的快速 ─ 只要一眨眼静态文件即生成完成
支持 Markdown  
仅需一道指令即可部署到 GitHub Pages 和 Heroku  
已移植 Octopress 插件  
高扩展性、自订性  
兼容于 Windows, Mac & Linux</center>*

---

## 谁能使用hexo

这是一个免费开源的博客程序，任何人都可以使用和修改。但是不同于wordpress，hexo由于需要使用Github,Git,Markdown,Node.js这样的工具，好多插件、widget都需要自己安装、设置。所以适合那些有一定计算机基础，喜欢折腾的人。但是，不要恐惧，只要跟着本教程走，就能很方便地让自己的博客“飞起来”。

## 注意

本系列相关博客均根据hexo2.4.5版本，但更高版本也几乎完全适用。各版本所做更新修正，请参考[这里](https://github.com/tommy351/hexo/releases)。