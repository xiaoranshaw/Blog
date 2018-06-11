# JavaWeb系列 6 web.xml文件内容解析
#javaweb
### web.xml是什么
web项目中*初始化*配置信息的配置文件
加载Welcome页面、servlet、servlet-mapping、filter、listener、启动加载级别等。是一个项目配置文件的起点

### 加载顺序
与文件内容位置无关，与节点名称相关
ServletContext -> context-param -> listener -> filter -> servlet

### 怎么用
部分参数解释：

**display-name**：可有可无，控制台展示项目名称
`<display-name>this is project name</display-name>`

 **context-param**
用来设定web站台的环境参数   键值对形式展现 
```
 <context-param>   
    <param-name>my_param</param-name>  
     <param-value>hello</param-value>  
 </context-param>  
```

**listener**  监听器，用来监听类实例
```
 <listener>
    <listener-class>org.springframework.web.context.request.RequestContextListener</listener-class>
  </listener>
```

**filter**
filter定义过滤器用法和属性     filter-mapping实现过滤器用处
     
init-param：过滤类中参数  key-value方式， 使用类中属性的setter方法注入

以下这行的意义是：过滤所有请求（ /*），使用CharacterEncodingFilter 的方式。其中CharacterEncodingFilter具体的实现类是spring的org.springframework.web.filter.CharacterEncodingFilter类。
该类中的属性：encoding = UTF-8    forceEncoding = true
```
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>
org.springframework.web.filter.CharacterEncodingFilter
	  </filter-class>

    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
      <param-name>forceEncoding</param-name>
      <param-value>true</param-value>
    </init-param>
  </filter>

  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

**servlet**
和filter中配置和解析含义相同。主要是处理客户端的请求并将其结果发送到客户端。
load-on-startup    启动顺序
当值为0或者大于0时，表示容器在应用启动时就加载这个servlet； 
当是一个负数时或者没有指定时，则指示容器在该servlet被选择时才加载。 
正数的值越小，启动该servlet的优先级越高。 
```
<servlet>
		<servlet-name>dispatcher</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:dispatcher-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	
	<servlet-mapping>
		<servlet-name>dispatcher</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
```

用的较少的欢迎页,错误页,和异常页
```
<welcome-file-list>
        <welcome-file>login.jsp</welcome-file>
 </welcome-file-list>

	<error-page>  
        <error-code>500</error-code>  
        <location>/views/500.jsp</location>  
    </error-page>

	<error-page>  
        <exception-type>java.lang.Throwable</exception-type>  
        <location>/views/error.jsp</location>  
    </error-page>  
```