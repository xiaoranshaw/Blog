# CentOS7 五 RabbitMQ 的安装和配置
#linux
## linux下安装   
> 基于centos 7

rabbitmq基于erlang语言开发，首先需要安装erlang环境

1. 安装依赖文件
```
yum -y install gcc glibc-devel make ncurses-devel openssl-devel xmlto perl wget
```

### 安装erlang环境
2. 安装erlang语言环境
* 找一个位置存放安装包,  将rabbit安装到 /usr/local 文件夹下
```
cd /home/xiaoran/rabbit

//下载erlang包
wget http://www.erlang.org/download/otp_src_18.3.tar.gz 
      
//解压
tar -xzvf otp_src_18.3.tar.gz  

//切换到安装路径
cd otp_src_18.3/
 
//生产安装配置
./configure --prefix=/usr/local/erlang  

//编译安装
make && make install  
```

3. 验证erlang环境
```
cd /usr/local/erlang/bin
./erl
```
效果如下图，说明安装成功
[image:A350DFB9-CF45-4025-AABB-DDC7F6F426BD-36887-0001EC5CFC44E5ED/1C85711B-0E24-4127-BFB5-B7810127069D.png]


4. 配置erlang语言环境变量
```
vi /etc/profile  
```
追加以下内容
```
    #set erlang environment
    ERL_HOME=/usr/local/erlang
    PATH=$ERL_HOME/bin:$PATH
    export ERL_HOME PATH

source /etc/profile  //生效
```

### 安装rabbitMQ
5. 安装rabbitmq
  下载到自己的文件夹中
```
cd /home/xiaoran 
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.1/rabbitmq-server-generic-unix-3.6.1.tar.xz  
```
解压到指定文件夹  /usr/local
```
xz -d rabbitmq-server-generic-unix-3.6.1.tar.xz
tar -xvf rabbitmq-server-generic-unix-3.6.1.tar -C /usr/local
```
为了方便记忆，可改个名字
```
cd /usr/local
mv rabbitmq_server-3.6.1/ rabbitmq
```
可配置rabbitmq命令为环境变量
```
vim /etc/profile

//追加以下内容
export PATH=$PATH:/usr/local/rabbitmq/sbin

source /etc/profile
```

6. 启动服务,查看状态
```
rabbitmq-server -detached
rabbitmqctl status
```

出现以下界面说明已经安装好了
[image:10CD5617-3808-4426-AB73-D5262FC9037B-36887-0001EC5CFC25BD41/4A0D789B-B65D-4CEE-A89D-5D7855584C0C.png]

7. 相关命令
```
启动服务：rabbitmq-server -detached
查看状态：rabbitmqctl status
关闭服务：rabbitmqctl stop
列出角色：rabbitmqctl list_users
```

8. 安装插件查看状态
```
rabbitmq-plugins enable rabbitmq_management

 若失败，创建文件夹再次尝试    /etc/rabbitmq
```
打开防火墙后，将看到如下界面
访问 http://ip:15672   将看到如下界面
[image:2A1C2C76-61F2-4618-BA4E-913AC10FB32F-36887-0001EC5CFC622767/A4FA2263-ECCB-4FD6-9C8F-96D757FEA396.png]
由于账号guest具有所有的操作权限，并且又是默认账号，出于安全因素的考虑，guest用户只能通过localhost登陆使用，并建议修改guest用户的密码以及新建其他账号管理使用rabbitmq(该功能是在3.3.0版本引入的)。

9. 设置一个可远程登录的用户
```
rabbitmqctl add_user xiaoran xiaoran     //前者为用户名，后者为密码
rabbitmqctl set_permissions -p / xiaoran ".*" ".*" ".*"  //添加权限
rabbitmqctl set_user_tags xiaoran administrator  //修改用户角色

```