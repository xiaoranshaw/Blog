# CentOS7 二 Tomcat部署到服务器
#linux
* 下载tomcat tar.gz格式的包
* 上传至服务器
* 解压
  `tar xvf apache-tomcat-8.0.50.tar.gz`

* 名字太长，换个名字
  `mv apache-tomcat-8.0.50 tomcat8`

* 查看指定端口是否占用
  `lsof -i:8080`


# 简单部署方式
将打好的war包放置到tomcat中的webapps文件夹下，然后直接启动
例子   application.war
访问路径：
http://ip:port/application/xxxx

# 去除项目名方式

例子   application.war
访问路径：
http://ip:port/xxxx    
> 即想要通过不加项目名的方式访问

1. 确定一个目录，存放war包
  以   /home/webapp  为例
  war包地址为    /home/webapp/application.war

2. 修改server.xml中的配置信息
```
cd tomcat/conf
vim server.xml
```
加入如下配置
在<Host></Host>标签内加入如下信息
```
	<Context path="" docBase="/home/webapp/application.war" reloadable="false" caseSensitive="false" debug="0"></Context>
```
path=““    不使用项目名
docBase:     war包的路径

3. 注意修改Host标签的参数
  autoDeploy    修改为”false"
  该参数意义是，自动打包
  tomcat实质是将打好的war包复制到webapp中再次部署，若设置为true，将使用ROOT的包覆盖刚打好的包

4. 删除webapps中的ROOT目录
  原理同上

5. 运行

