title: Windows上搭建git服务器
date: 2014-02-17 16:25:09
tags: [git, copssh]
categories: blog
keywords: Windows,git,copssh
---

## 需要软件
* git([git for windows](http://git-scm.com/download/win))
* copssh([安装包](http://sudu.newhua.com:82/down/Copssh_4.1.0_Installer.zip))

## 安装
* git安装（这个很简单，下一步到底即可，教程也很多，就不写了。）
* copssh安装（这个也很简单，下一步到底即可，**注意：不要修改默认的用户名、密码**）
* copssh配置
    1. 创建用户  
    通过命令行创建访问用户：net user 用户名(git) 密码(123456789) /add，接下来只需要输入yes并按回车键即可。为了方便理解及学习一下几个步骤，建议用户就用git。
    2. 添加用户  
    开始->01. COPSSH Control Panel打开copssh控制面板，  
    选中Users，点击Add按钮，之后在select a user界面选择刚才新建的用户（git）  
    在select options界面勾掉Allow password authentication，然后Forward即可。
    3. 访问权限  
    在C:\ICW\home\git\.ssh目录下新建一个authorized_keys（注意：没有后缀）文件，将需要访问git用户的公钥（id_rsa.pub）添加到此文件。现在就可以用[Xshell](http://dlsw.baidu.com/sw-search-sp/soft/7b/15201/Xshell4_4.0.0129.3864788376.exe)通过ssh连接服务器了：ssh git@127.0.0.1（这个过程需要载入用户的私钥，即id_rsa）
    4. 防火墙  
    打开C:\ICW\etc\ssh_config的默认端口22（即去掉Port 22这一行前面的#号），如果需要修改默认端口号，需要同时修改C:\ICW\etc\ssh_config、C:\ICW\etc\sshd_config两个文件中的端口
    copssh默认使用22端口，如果需要非本机拉取代码，需要在windows防火墙中打开22号端口（只需要针对局域网打开即可）。
    4. 初始化git仓库  
    在C:\ICW\home\git目录下新建一个文件夹（如：test）  
    右键test文件夹，点击Git bash，然后键入：git init --bare初始化仓库（注意：必须要--bare）
    到此，仓库就已经创建完成，可以在其他目录通过：git clone git@127.0.0.1:test拉取仓库了，其它用户只需将127.0.0.1换成你的IP即可（注意：仅限局域网）