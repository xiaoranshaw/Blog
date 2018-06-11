# CentOS7 四 redis安装
#linux
## 安装
CentOS7  下的redis的安装

* 下载和解压
```
wget http://download.redis.io/releases/redis-4.0.9.tar.gz
tar xzf redis-4.0.9.tar.gz
cd redis-4.0.9
make         //编译和安装

```

* 开启redis server服务

```
cd redis-4.0.9/src
./redis-server           //开启服务
```

* 开启redis client服务
```
cd redis-4.0.9/src
./redis-cli
```

## 关于远程无法连接到redis的坑
redis的启动可默认启动和选择加载配置启动两种方式，以上为例，默认加载方式，只能使用本地机器连接

解决办法：
	1. 在redis.conf文件中的bind属性确定可连接的机器，默认状态下是127.0.0.1
[image:3675DE70-BA9E-4B0F-A3A5-2C3ED710DD71-1070-0000082A9CFB0460/5E43D9E6-75F8-4094-84D8-7BA52361E0B7.png]

	2. 将其注释，即变为所有的机器都可以访问和使用
	
	3. 然后  启动时使用加载配置文件的方式启动
```
src/redis-server redis.conf

或者后台常驻启动方式
nohup src/redis-server redis.conf &
```

​	