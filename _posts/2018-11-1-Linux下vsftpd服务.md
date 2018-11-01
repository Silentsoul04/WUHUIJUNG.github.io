---
layout: post
title: Linux中的vsftpd服务
date: 2018-11-1
categories: blog
tags: [Linux]
description: 文艺的流氓。
---

#### vsftpd是什么？

百度百科上是这样解释的：

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101204920.png)

#### 安装vsftpd服务

使用yum来安装vsftpd指令：`yum install vsftpd -y` (我自己是使用的centos)

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101204228.png)

启动一下

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101205525.png)

我没有进行任何的配置。先使用一下匿名用户访问

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101205804.png)

结果是能够访问进去的。

#### vsftpd的加固方法

##### 1.安装补丁

在安装更新补丁前，备份您的 vsftp 应用配置。从 [VSFTPD官方网站](http://vsftpd.beasts.org/#download) 获取最新版本的 vsftp 软件安装包，完成升级安装。或者，您可以下载最新版 vsftp 源码包，自行编译后安装更新。您也可以执行`yum update vsftpd`命令通过 yum 源进行更新。

##### 2.禁用匿名用户登陆

设置禁止匿名用户登陆需要去`vsftp.conf`˙中设置

修改配置文件 `vsftpd.conf`, 使用vim来进行修改`vim /etc/vsftpd/vsftpd.conf`

设置为`anonymous_enable=NO` 就可以禁止匿名用户登陆（需要重启一下）

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101210646.png)

##### 3.禁止显示banner信息

修改vsftpd配置文件 `vsftpd.conf` ,设置`ftpd_banner=Welcome` 重启一下vsftpd服务后，就可以不显示banner信息(也就是版本信息)在`85`行左右

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101211629.png)

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101211711.png)

##### 4.限制FTP登陆用户

在ftp中的user_list文件中存在的用户名都是不可能登陆ftp的。

![](https://wujinlin.oss-cn-beijing.aliyuncs.com/blog/20181101211858.png)

##### 5.也可以设置一下FTP目录

同样都是修改vsftpd.conf配置文件中的配置：

`1.chroot_list_enable=YES`

`2.chroot_list_file=/etc/vsftpd/chroot_list`

新建 /etc/vsftpd/chroot_list 文件，并添加用户名。例如，将 user1 添加至该文件，则 user1 登录 FTP 服务后，只允许在 user1 用户的 home 目录中活动。

##### 6.修改监听地址和默认端口

修改 VSFTP 配置文件 vsftpd.conf，设置监听127.0.0.1 地址的 9999 端口。

`1.listen_address=127.0.0.1`

`2.listen_port=9999`

##### 7.一些其他的限制

修改 VSFTP 配置文件 `vsftpd.conf`

```//限制连接数
//限制连接数
1.max_clients=100
2.max_per_ip=5
//限制传输速度
1.anon_max_rate=10000
2.local_max_rate=10000
```

#### 一些简单的vsftpd安全配置。









