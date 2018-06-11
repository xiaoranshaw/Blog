# CentOS7 三 安装mysql
#linux
* 环境： centos 7
  CentOS 7 版本，由于 MySQL数据库已从默认的程序列表中移除，通过yum方式会安装不上，因此采用wget的方式导入

## 安装
1. 下载mysql57的安装源
  `wget http://repo.mysql.com/mysql57-community-release-el7-8.noarch.rpm`


2. rpm安装
  `sudo rpm -ivh mysql57-community-release-el7-8.noarch.rpm`

3. 安装mysql
  `sudo yum install mysql-server`

安装完成

## 连接：
```
mysql -uroot -p

无密码直接回车
```
若出现以下情况
`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)`

这是/var/lib/mysql权限问题，修改MySQL权限为当前用户

`sudo chown -R root:root /var/lib/mysql`

 重启MySQL服务

`service mysqld restart`

修改密码
`SET PASSWORD FOR 'root'@'localhost' = PASSWORD('xxxxxxx');`

## 远程连接
`GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;`

myuser/mypassword

刷新
`flush privileges;`