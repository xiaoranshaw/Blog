# JavaWeb系列 7 Spring 注入的三种方式
#javaweb
* 自动化装配   
* 显式装配 Java
* 显式装配 xml

## 1.自动化装配
两个维度：
* 组件被扫描     spring自动发现并创建bean
* 去扫描的配置类       去发现并满足bean之间的依赖

#### 1.1 创建可被发现的bean
@Component        @Controller      @Service等
写到具体的实现类
更改bean的id:@Component(“xxxxxx”),若不设置，则采用默认的首字母小写

#### 1.2 扫描spring的bean
扫描配置：  两种方式
1. class配置类扫描方式
  @Configuration
  @ComponentScan(“com.lucky”)
  或者@ComponentScan(basePackages=“com.lucky”)

当不设置后面的值时，默认扫描类当前的包

2. xml配置扫描方式
  <context:component-scan base-package="com.lucky"/>

#### 1.3  注入到使用的地方
@Autowired   @Resource   @Inject


## 显示装配  Java
#### 声明简单的bean    @Bean方式创建类型的实例
@Bean 注解会告诉 Spring 这个方法将会返回一个对象，该对象要注册为 Spring 应 用上下文中的 bean 。方法体中包含了最终产生 bean 实例的逻辑。

在@Configuration修饰的配置类里面
```
	@Bean
    public MusicBox musicBox(){
        return new MusicBox();
    }
```

## 显示配置 xml
复杂，可读性差



---
问题
---
1.spring 注入为什么要采用接口，而不是用具体的实现来创建bean
2.一个接口有多个实现，是否是单例才能注入
3.@Autowired   @Resource的区别
[@Autowired和@Resource的区别 博客园](https://www.cnblogs.com/wangxiaoce/p/7056907.html)