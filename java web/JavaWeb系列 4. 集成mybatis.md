# JavaWeb系列 4. 集成mybatis
#javaweb
* 采用dbcp 连接池方式


## 集成过程：
1. 新加入maven依赖
```
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring-version}</version>
</dependency>

<!--mybatis-spring-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>
    <!--mybatis依赖包-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.4.6</version>
    </dependency>

<!--数据库连接池-->
    <dependency>
      <groupId>commons-dbcp</groupId>
      <artifactId>commons-dbcp</artifactId>
      <version>1.4</version>
    </dependency>

<!-- https://mvnrepository.com/artifact/commons-pool/commons-pool -->
<dependency>
    <groupId>commons-pool</groupId>
    <artifactId>commons-pool</artifactId>
    <version>1.6</version>
</dependency>

```

2. 加入数据库配置信息
  新建db.properties文件
```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://39.107.68.25:3306/webapp?useUnicode=true&amp;characterEncoding=utf-8
jdbc.username=xiaoran
jdbc.password=jinmeimei
```

3. 分别创建jdbc的连接配置文件和mybatis的连接配置文件
  applicationContext-jdbc.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context
http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 加载数据库连接的资源文件 -->
    <context:property-placeholder location="classpath:db.properties"/>

    <!-- 配置数据源   dbcp数据库连接池 -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

</beans>
```



applicationContext-mybatis.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!-- 配置sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- 数据库连接池 -->
        <property name="dataSource" ref="dataSource"/>
        <!-- 加载Mybatis全局配置文件 -->
        <property name="configLocation" value="classpath:sqlMapConfig.xml"/>
        <property name="mapperLocations" value="classpath:mybatis/*.xml"/>
    </bean>

    <!--下面两种方法，一种是自动是扫描对应的service类，一种是通过注入sqlsessionFactory的方式来获取一个SqlsessionTemplate-->
    <!-- DAO接口所在包名，Spring会自动查找其下的类 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 扫描包路径，如果需要扫描多个包，中间使用半角逗号隔开 -->
        <property name="basePackage" value="com.lucky.mapper"></property>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />

    </bean>
    <!-- 配置SqlSessionTemplate -->
    <!--  通过构造函数注入
    <bean id="sqlSessionTemplate" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory" />
    </bean>
    -->

</beans>
```
![F8958F2B-A771-484E-B2C5-D0EBA6652841](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.ClXEf5/F8958F2B-A771-484E-B2C5-D0EBA6652841.png)


4. 在主spring加载文件中导入这两个文件
```
    <import resource="applicationContext-jdbc.xml"/>
    <import resource="applicationContext-mybatis.xml"/>
```

5. 创建对应的接口类和xml映射文件


## 坑：
1. java.lang.NoClassDefFoundError: org/springframework/jdbc/datasource/TransactionAwareDataSourceProxy

该问题出现原因为缺少 spring-jdbc依赖

解决方案：
```
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring-version}</version>
</dependency>
```

2. 
```
nested exception is org.xml.sax.SAXParseException; lineNumber: 29; columnNumber: 7; cvc-complex-type.2.3: 元素 'beans' 必须不含字符 [子级], 因为该类型的内容类型为“仅元素。
```

该问题表示文件里含有特殊字符，重新另存UTF8，把注释之外的多余内容删除即可。