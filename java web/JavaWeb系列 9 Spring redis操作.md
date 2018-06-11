# JavaWeb系列 9 Spring redis操作
#javaweb
## 目录
* spring-redis的连接
* spring-redis的使用


1. 导入maven依赖
```
	<dependency>
      <groupId>org.springframework.data</groupId>
      <artifactId>spring-data-redis</artifactId>
      <version>2.0.2.RELEASE</version>
    </dependency>

	  
    <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
      <version>2.9.0</version>
    </dependency>
```

2. 导入jedis配置
```
<bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <property name="maxIdle" value="1"/>
        <property name="maxTotal" value="5"/>
        <property name="blockWhenExhausted" value="true"/>
        <property name="maxWaitMillis" value="30000"/>
        <property name="testOnBorrow" value="true"/>
    </bean>

    <bean id="jedisConnectionFactory" class="org.springframework.data.redis.connection.jedis.JedisConnectionFactory">
        <property name="hostName" value="${redisHostName}"/>
        <property name="port" value="${redisPort}"/>
        <property name="poolConfig" ref="jedisPoolConfig"/>
        <property name="usePool" value="true"/>
    </bean>
```