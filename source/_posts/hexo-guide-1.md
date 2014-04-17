title: hexo系列教程（二） - 搭建hexo博客
date: 2014-02-11 17:03:48
tags: [hexo, NodeJS]
categories: blog
keywords: hexo

---

## 环境依赖  
*下载安装即可* 

- [msysgit](http://code.google.com/p/msysgit/)  
- [NodeJs](http://nodejs.org/)  


## 安装hexo
  利用 npm 命令即可安装。（在任意位置点击鼠标右键，选择Git bash）  
``` Javascript
npm install -g hexo
```

## 创建hexo文件夹
在任意文件夹（如：D:\hexo）点击鼠标右键，选择Git bash
``` Javascript
hexo init
```

## 查看效果
启用本地服务  
``` Javascript
hexo generate
hexo server
```
现在我们已经搭建起本地的hexo博客了，执行以下命令(在D:\hexo)，然后到浏览器输入localhost:4000看看。  
好了，至此，本地博客已经搭建起来了。

## 部署到Github
- 注册  
已有账号可以跳过，没有的，请在此进行[注册](https://github.com/signup/free)，很简单，这里就不介绍了。
- 创建repository  
在自己[Github](https://github.com/)主页右下角，创建一个新的repository。比如我的[Github](https://github.com/)账号是[fulme](https://github.com/fulme)，那么我应该创建的repository名字应该是[*fulme.github.io*](https://fulme.github.io)。
- 部署  
编辑_config.yml(在D:\hexo下)。你在部署时，要把下面的fulme都换成你的账号名。
```YUML
deploy:
  type: github
  repository: https://github.com/fulme/fulme.github.io.git
  branch: master
```
执行下列指令即可完成部署。
```
hexo generate
hexo deploy
```

**记住**：每次修改本地文件后，需要hexo generate才能保存。每次使用命令时，都要在D:\hexo目录下。
Okay，我们的博客已经完全搭建起来了，在浏览器访问fulme.github.io就能看到你的成就了！

## 注意
- 有网友反应右键菜单中没有git bash选项，可以进入开始菜单找到git bash，然后通过cd进入相应目录执行命令。
- 在github部署完成之后，马上访问可能出现404错误，这是正常的，（最多）等待十分钟左右就可以访问了。
- 如果在hexo d之后出现fatal: 'username.github.io' does not appear to be a git repository，一是检查 repo 的名字是否合乎规范、是否含有大写字母、config.yml 中的 deploy 配置是否正确，二是把 git bash 关掉，重新打开再执行命令

## TIPS
hexo现在支持更加简单的命令格式了，比如：

- hexo g == hexo generate  
- hexo d == hexo deploy  
- hexo s == hexo server  
- hexo n == hexo new  