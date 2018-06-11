# CentOS7 七 kafka的单机安装和配置
#linux

> kafka的单机部署

# kafka的单机部署
Kafka 使用Zookeeper 来保存相关配置信息，Kafka及Zookeeper 依赖Java 运行环境

> 单机部署非常简单，总结下来就是：下载解压使用即可

1. 下载和解压安装包
  下载官网    [Apache Kafka](http://kafka.apache.org/downloads)
```
wget http://mirror.bit.edu.cn/apache/kafka/1.0.0/kafka_2.11-1.0.0.tgz 

tar -xvf kafka_2.11-1.0.0.tgz

mv kafka_2.11-1.0.0 kafka    //更新名称方便以后操作
```

2. 创建一个文件夹，放置解压文件
```
mkdir /usr/kafka

mv kafka /usr/kafka
```

3. (可选)  修改kafka的日志打印位置
  kafka文件夹下面有个config文件夹，存放配置信息
  server.properties存放服务配置信息,在这里面进行修改
```
cd /usr/kafka/kafka
cd config
vim server.properties
```
在文件中修改为     指定的log地址即可
```
log.dirs=/usr/kafka/kafka-logs
```

### 测试
启动kafka之前需要先启动zookeeper。kafka安装包中自带一个zookeeper服务
> 开启多个终端的原因是防止误杀了进程，影响测试.并可以直观看到效果
1. 启动zookeeper
```
cd /usr/kafka/kafka
bin/zookeeper-server-start.sh config/zookeeper.properties
```

2. 开启另一个终端，启动kafka
```
nohup bin/kafka-server-start.sh config/server.properties&
```
nohup &是实现在后台启动。

3. 生产消息
```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test

//就可以从此处发送消息
>    xxxxx
```

4. 消费消息
```
bin/kafka-console-consumer.sh --zookeeper localhost:2181 --topic test --from-beginning
```

bin/kafka-manager