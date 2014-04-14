title: samba学习笔记
date: 2014-03-19 08:49:51
tags: [samba]
categories: diary

---

## 什么是samba

[samba](http://baike.baidu.com/link?url=fpCneip0_fVu6C1m7k9XFTiFilCi88C4ggR0Eya-ZA5kFsOCeizBnVke-cMze54G)是Unix和Linux上实现SMB协议的免费软件。
[SMB](http://baike.baidu.com/view/262410.htm)（Server Messages Blck, 信息服务块）是局域网上共享文件和打印机的通信协议，它为局域网内不同计算机提供文件及打印机等资源的共享服务。

## 为什么要samba
为了实现Windows主机与Linux主机之间的资源共享，Linux操作系统提供了samba服务，samba为两种操作系统架起了一座桥梁，使得Windows与Linux系统之间方便地实现共享。本人的用途：在开发网站的时候需要配合后端做一些模板替换及渲染工作，且代码需要部署到Linux主机上，但是本人已经习惯了Windows上的开发环境，于是samba派上了用场，通过安装和配置samba，然后在Windows上做一个文件映射就可以实现，在Windows上开发实时同步到Linux主机上了。具体的安装配置，接下来会讲。

## 安装samba
默认情况下，Linux系统已经安装samba了，通过如下的命令可以查看：
``` shell
rpm -qa | grep samp

samba-3.0.33-3.14.el5
samba-common-3.0.33-3.14.el5
```
如果没有安装，可以通过如下命令安装：
```shell
rpm -ivh samba-3.0.33-3.29.el5_6.2.i386.rpm -f --nodeps
rpm -ivh samba-client-3.0.33-3.29.el5_6.2.i386.rpm  -f --nodeps
rpm -ivh samba-common-3.0.33-3.29.el5_6.2.i386.rpm -f --nodeps
```
安装完成后就可以通过上面的查询命令查看了。

## 配置samba
samba的配置信息一般都在`/etc/samba`目录下，主配置文件为`smb.conf`，文件中记录着大量规则和共享信息。在配置之前有必要先了解一下samba工作原理。  
**原理**：*客户端发起访问工项目的请求，samba服务接收到请求以后查看smb.conf文件，如果共享目录存在且访问者有权限则允许访问，最后将访问过程中系统的信息以及采集的用户访问行为信息存放到日志中。*

### 修改配置文件
首先备份一下配置文件：
```shell
cd /etc/sabma
sudo mv smb.config smb.config.bak
```
新建一个配置文件：
```shell
touch smb.config
```
然后将下面这段代码到smb.config末尾：
```shell
[share]
path = /home/share/
valid users = username
public = no
writeable = yes
printable = no
follow symlinks = yes
wide symlinks = yes
wide links = Yes
unix extensions = no
```

### 添加账户
如果用户名不是是当前用户名，则需要添加：
```shell
sudo useradd username
```
然后需要设置密码：
```shell
sudo smbpasswd -a username
```
然后连续输入两次密码就OK了。

### Windows映射
我的电脑->映射网络驱动器->输入`\\yourIP\share`->确定->输入用户名、密码->确定。  
如果操作成功，在我的电脑->网络位置会多出一个盘符，如下图所示：
![](../blog/img/samba.jpg)

## 心得体会
挺方便的，对于习惯windows开发的人员尤其有用。
在windows上开发，保存文件的时候，自动就同步到Linux主机上了。