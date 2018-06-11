# JavaWeb系列 2. 手把手教你创建新的Java web项目-2
#javaweb
### 说明：
该项目采用最普通的spring mvc配置方式，xml方式配置bean。效果是能实现基础访问

### 配置
#### 配置web.xml
加入spring mvc的如下配置，实现前端控制器。DispatcherServlet

```
<!-- 配置SpringMVC的前端控制器，拦截所有请求-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:dispatcher-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet> 

  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
```

![2B1992BC-1CDE-4DF8-8B5B-4BAF3CD07150](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.8b03qx/2B1992BC-1CDE-4DF8-8B5B-4BAF3CD07150.png)

#### 配置对应的dispatcher-servlet.xml
上一步中的classpath:dispatcher-servlet.xml，是打的war包的根目录一级地址。*根目录为 taregt下war包中的WEB-INF/classes*

![DFCEE551-6D5F-4224-9288-94B82C40A399](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.o8o7jN/DFCEE551-6D5F-4224-9288-94B82C40A399.png)

xml基础配置文件如下
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">


</beans>
```

加入xml扫描方法，扫描基础的java包，如controller等

`<context:component-scan base-package="com.lucky"/>`


![DDD59CC2-C772-4A8B-B2A5-62AA2E45AC7C](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.838kex/DDD59CC2-C772-4A8B-B2A5-62AA2E45AC7C.png)

#### 加入controller类，实现访问
在com.lucky包下面创建java类
![F3DDD1B5-F825-4B49-B912-9F0FEFBFCAE3](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.gwvKQ3/F3DDD1B5-F825-4B49-B912-9F0FEFBFCAE3.png)

#### 至此，spring mvc基础架子搭建完成，测试。
配置相应的tomcat服务
![5343824F-5793-445B-B45A-4F40F173A329](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.LIvq9m/5343824F-5793-445B-B45A-4F40F173A329.png)
![22B7A506-B519-4346-BFBE-94EAD2D5077A](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.2YXpkS/22B7A506-B519-4346-BFBE-94EAD2D5077A.png)
开启服务，浏览器中访问http://localhost:8081/hello   出现返回的效果

![FA1244F9-4B5C-4FC9-9A7A-FD8575DC68AE](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.zeBUL9/FA1244F9-4B5C-4FC9-9A7A-FD8575DC68AE.png)