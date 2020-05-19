# 1 Mysql概述

[Mysql](https://www.mysql.com/ "Mysql")是数据库管理系统，关系型数据库，既有商用版又有社区版。因为软件开源免费且体量小，许多公司和个人将其作为应用开发的首选数据库。目前最新发布的`Mysql8.0`发行版本有`Mysql Server`,`Mysql NDB Cluster`,`Mysql Enterprise`,`InnDB Cluster`.实际开发中，我们使用最多的是`Mysql Server`。其中`Mysql Cluster`中的[NDB](https://en.wikipedia.org/wiki/Ndb_Cluster "NDB")（表示Network Database）。NDB是存储行表的驱动引擎（很多博客解释是“内存中”的引擎，这里有待进一步研究）。

# 2 Mysql环境搭建

根据不同发行产品和不同操作系统环境，搭建过程存在差异。主要分为 `Linux`,`Windows`,`OSX`三种操作系统的搭建

## 2.1 [Yum](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/ "Yum存储库")
- 添加Mysql Yum存储库 
  1. 下载对应版本的[RPM](https://dev.mysql.com/downloads/repo/yum/ "RPM")包;
  2. 安装下载的发行包
```shell
sudo rpm -Uvh platform-and-version-specific-package-name.rpm
-- demo: sudo rpm -Uvh mysql80-community-release-el6-n.noarch.rpm
```
- 选择发行版本
  1. 默认安装为最新版本（如果需要使用默认版本安装直接下一步）
  2. 查看启动或禁用哪些存储库

```shell
yum repolist all | grep mysql
```

  3. `yum-config-manager`和`dnf config-manager` 分别对应未启动dnf平台和启动dnf平台

```shell
-- 未启动dnf平台
sudo yum-config-manager --disable mysql80-community
sudo yum-config-manager --enable mysql57-community
-- 启动dnf平台
sudo dnf config-manager --disable mysql80-community
sudo dnf config-manager --enable mysql57-community
```
- 安装Mysql数据库
```shell
--安装
sudo yum install mysql-community-server
--开启服务
sudo service mysqld start
--服务状态
sudo service mysqld status
```

## 2.2 [Apt](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/ "Apt存储库")

- 下载对应版本的[APT](https://dev.mysql.com/downloads/repo/apt/ "APT")包；
- 使用命令进行安装下载的发行软件包
```shell
sudo dpkg -i /PATH/version-specific-package-name.deb
-- demo:sudo dpkg -i mysql-apt-config_w.x.y-z_all.deb
```
- 使用命令更新软件包信息
```shell
sudo apt-get update
```
- 安装mysql
```shell
-- 安装mysql
sudo apt-get install mysql-server
-- 启动服务
sudo service mysql start
-- 停止服务
sudo service mysql stop
-- 查看服务状态
sudo service mysql status
```

## 2.3 window

- 下载[安装包](https://dev.mysql.com/downloads/installer/ "安装包")

- 进行图形界面安装，[安装过程](https://www.runoob.com/w3cnote/windows10-mysql-installer.html "安装过程")

## 2.4 OSX
- 下载安装包。
- 进行图形界面安装，[安装过程](https://dev.mysql.com/doc/refman/8.0/en/osx-installation-pkg.html "安装过程")
- 启动方式在Application 中进行设置。

# 3 Mysql使用
- 使用命令行进行连接
```shell
mysql -u root -p

--yum安装方式的初始密码 在错误日志中，使用下面命令查看
sudo grep 'temporary password' /var/log/mysqld.log

--进入mysql 使用下面语句可修改密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';

```
- 使用第三方软件进行管理[MySQL Workbench(Win)](http://www.mysql.com/downloads/workbench/ "MySQL Workbench(Win)"), [Navicat(Win)](https://www.navicat.com/en/products/navicat-for-mysql "Navicat(Win)"),[Sequel Pro(OSX)](http://www.sequelpro.com/ "Sequel Pro(OSX)").
