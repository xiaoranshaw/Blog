# CentOS7 六  Nginx  安装和基本操作
#linux
# mac版本的安装
## 安装
mac下安装
`brew install nginx`
默认安装位置
`/usr/local/Cellar/nginx`
配置位置
`/usr/local/etc/nginx`

## 访问与基本命令
默认访问地址
`http://localhost:8080/`

```
开启
./nginx
重新载入配置文件
nginx -s reload
重启
nginx -s reopen
关闭
nginx -s stop
```

```
nginx -V     查看版本和基本信息，包括配置文件
nginx -v     查看版本
nginx -c     指定配置文件
```


# linux （基于centos）安装
### 安装编译文件和库文件
```
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```

### 安装PCRE
PCRE 作用是让 Nginx 支持 Rewrite 功能。
1. 创建一个新的文件位置   放置统一的nginx和相关资源文件
```
mkdir /usr/nginx
cd /usr/nginx

wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
```

2. 解压安装包&编译安装
```
tar zxvf pcre-8.35.tar.gz
cd pcre-8.35
./configure
make && make install
```

3. 查看pcre版本
  `pcre-config --version`

### 安装nginx
1. 下载安装nginx
```
cd /usr/nginx
wget http://nginx.org/download/nginx-1.6.2.tar.gz
tar zxvf nginx-1.6.2.tar.gz
```

2. 进入安装包,编译安装
```
cd nginx-1.6.2

./configure --prefix=/usr/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre=/usr/nginx/pcre-8.35

make

make install
```


3. 查看版本并测试访问
  编译好之后，将在nginx文件夹中生成sbin执行文件
```
cd /usr/nginx/sbin

./nginx -v

```
访问地址
`http://39.107.68.25/`