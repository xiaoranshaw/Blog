# CentOS7 一.搭建Java环境
#linux
* 检查java环境
  `java -version`

[image:683560E6-29AF-4F1A-948E-0C7738C31771-36887-0001EC5CF903D2BF/28944D9A-5978-468A-9BDB-1031BA65E47A.png]

* 没有找到java环境，通过导入JDK压缩包的方式导入
  下载jdk8，然后传入到linux下自己的文件夹中

* 解压jdk文件，并改名（改名操作非必须）
```
tar xvf jdk-8u161-linux-x64.tar.gz

mv jdk1.8.0_161 jdk1.8
```

[image:40F0455D-CD6C-4168-9530-1ABA3BE3926E-36887-0001EC5CF957676A/8C7F8384-8735-40FA-8CA7-5F3A5AE8BD9E.png]

* 将解压的文件拷贝到/usr/java文件夹下面，/usr文件用于存放应用程序和文件
  `mv /home/java/jdk1.8 /usr/java`

* 配置环境变量

`vi /etc/profile`

添加对应的shell脚本
```
#set java environment
JAVA_HOME=/usr/java/jdk1.8
CLASSPATH=$JAVA_HOME/lib/
PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH

```

* 让页面生效
  `source /etc/profile`

* 检查版本，安装完成

[image:5C20D536-4A75-43B9-B7A0-624466CD10C7-36887-0001EC5CF9364E06/013CA1DB-37CF-4BB2-8ECE-E7DDF0E817B1.png]

## 总结
重点就两个地方，1，jdk解压到/usr下面的文件夹   2，环境变量改动