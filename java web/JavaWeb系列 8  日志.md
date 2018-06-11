# JavaWeb系列 8  日志
#javaweb
### 用什么做日志
现在SLF4J很快就会成为Java世界里标准的日志记录库

#### 原因：
*重点在于 1，2*
1. 让程序独立于任何特定的日志记录库。
  结构如下，可不用关心
  ![8D185E8F-FCD4-4B18-8FFF-62ABCFCC72B5](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.vXkeTe/8D185E8F-FCD4-4B18-8FFF-62ABCFCC72B5.png)

2. 占位符功能，在代码中用{}来表示，提高代码可读性
  通过使用SLF4J，你可以延迟字符串的创建到运行时刻，这意味着只有在需要字符串的时候才创建它

3. 通过使用日志记录方法，直到你使用到的时候，才会去构造日志信息（字符串），这就同时提高了内存和CPU的使用率

4. 做一个侧面的说明，越少的临时字符串，垃圾回收器就意味着越少的工作，这就意味着为你的应用程序提供更好的吞吐量和性能

## slf4j-log4j 依赖
```
<!--版本-->
	<properties>
    <slf4j.version>1.7.21</slf4j.version>
    <log4j.version>2.7</log4j.version>
  </properties> 

<!--具体依赖-->
	<dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>jcl-over-slf4j</artifactId>
      <version>${slf4j.version}</version>
      <scope>runtime</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-api</artifactId>
      <version>${log4j.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-core</artifactId>
      <version>${log4j.version}</version>
    </dependency>
    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-slf4j-impl</artifactId>
      <version>${log4j.version}</version>
    </dependency>
```

## Java web集成日志
	1. 导入pom依赖
	2. 添加log4j2.xml文件
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration status="OFF">
    <properties>
        <property name="LOG_HOME">${web:rootDir}/logs</property>
        <property name="FILE_NAME">likR</property>
    </properties>
    <appenders>
        <Console name="Console" target="SYSTEM_OUT">

            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </Console>
        <File name="Error" fileName="${LOG_HOME}/error.log">
            <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="%d{yyyy.MM.dd 'at' HH:mm:ss z} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </File>
        <RollingFile name="RollingFile" fileName="${LOG_HOME}/${FILE_NAME}.log"
                     filePattern="${LOG_HOME}/$${date:yyyy-MM}/${FILE_NAME}-%d{MM-dd-yyyy}-%i.log.gz">
            <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} %-5level %class{36} %L %M - %msg%xEx%n"/>

            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="10 MB"/>
            </Policies>
        </RollingFile>
    </appenders>
    #root 默认加载
    <loggers>
        <root level="info">
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="Console"/>
            <appender-ref ref="Error" />
        </root>
    </loggers>
</configuration>
```

	3. 配置日志路径   （若不填写，默认采用 WEB-INF/classes/下的log4j2.xml）
```
<context-param>
    <!-- 日志配置文件路径 -->
    <param-name>log4jConfiguration</param-name>
    <param-value>classpath:log4j2.xml</param-value>
  </context-param>
```

	4. 使用
```
private static final Logger LOGGER = LoggerFactory.getLogger(LogInitController.class);

LOGGER.info("日志请求  {}","begin");
```