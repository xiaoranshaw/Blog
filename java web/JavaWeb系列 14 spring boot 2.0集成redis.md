# JavaWeb系列 14 spring boot 2.0集成redis
#javaweb
1. 引入pom依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!--spring2.0集成redis所需common-pool2-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
    <version>2.4.2</version>
</dependency>
```

2. 新建配置项
  application.properties
```
## Redis 配置
## Redis数据库索引（默认为0）
spring.redis.database=0
## Redis服务器地址
spring.redis.host=39.107.68.25
## Redis服务器连接端口
spring.redis.port=6379
## Redis服务器连接密码（默认为空）
spring.redis.password=
```

3. 使用java config方式创建bean
```
@EnableCaching
@Configuration
@ConditionalOnClass(RedisOperations.class)
@EnableConfigurationProperties(RedisProperties.class)
public class RedisConfig extends CachingConfigurerSupport {

    @Bean(name = "redisTemplate")
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();

        template.setKeySerializer(new StringRedisSerializer());
        template.setHashKeySerializer(new StringRedisSerializer());

        template.setConnectionFactory(redisConnectionFactory);
        return template;
    }

}
```

4. spring boot集成redis非常简单，到此配置完成，使用测试类进行测试
```
@RunWith(SpringJUnit4ClassRunner.class)
@SpringBootTest(classes = EnjoyDiyApplication.class,webEnvironment = SpringBootTest.WebEnvironment.DEFINED_PORT)
public class RedisTest {

    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    //直接使用redisTemplate存取字符串
    public void setAndGet() {
        redisTemplate.opsForValue().set("testKey", "testValue1");
        Assert.assertEquals("testValue1", redisTemplate.opsForValue().get("testKey"));
    }

}
```