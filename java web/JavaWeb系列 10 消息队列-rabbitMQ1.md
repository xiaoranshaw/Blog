# JavaWeb系列  10 消息队列-rabbitMQ1
#Javaweb

> 第一个例子，使用direct转发器做实验，该转发器绑定队列后，接收者通过路由键 routerkey完全匹配，才能接收到消息。且消息只消费一次
>
> ![E55CD6FA-C30E-4544-B451-B2DE0CCB6DA2](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.8U7FUM/E55CD6FA-C30E-4544-B451-B2DE0CCB6DA2.png)


## spring中集成rabbitmq
spring提供了amqp方式，针对队列进行集中管理
1. 首先导入依赖包
```
<dependency>
      <groupId>org.springframework.amqp</groupId>
      <artifactId>spring-rabbit</artifactId>
      <version>1.7.5.RELEASE</version>
</dependency>
```

2. 新建配置rabbitmq的配置文件
  applicationContext-rabbitmq.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/rabbit http://www.springframework.org/schema/rabbit/spring-rabbit.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    </beans>
```


3. spring容器引入该配置文件
```
<import resource="applicationContext-rabbit.xml"/>
```

4. 编写配置文件信息   针对applicatonContext-rabbit.xml

* 首先连接到指定的rabbitmq服务器
```
<!--配置连接-->
    <rabbit:connection-factory id="connectionFactory"
                               host="${rabbitmq.host}"
                               port="${rabbitmq.port}"
                               username="${rabbitmq.username}"
                               password="${rabbitmq.password}"/>
```
其中，将rabbitmq的机器地址和端口号等信息分离到配置文件中
rabbitmq.properties
```
rabbitmq.host=39.107.68.25
rabbitmq.port=5672
rabbitmq.username=maxwell
rabbitmq.password=maxwell
```

* 其次，声明使用的队列
    durable:是否持久化
    exclusive: 仅创建者可以使用的私有队列，断开后自动删除
    auto_delete: 当所有消费客户端连接断开后，是否自动删除队列
```
    <!-- 声明一个Queue -->
    <rabbit:queue id="test_queue" name="test_queue_name" durable="true" exclusive="false"
                  auto-delete="false">
        <rabbit:queue-arguments>
            <entry key="x-ha-policy" value="all"/>
        </rabbit:queue-arguments>
    </rabbit:queue>
```


* 第三，声明使用的转发器类型，转发器中，可添加相应的队列
    rabbit:direct-exchange：定义exchange模式为direct，意思就是消息与一个特定的路由键完全匹配，才会转发。
    rabbit:binding：设置消息queue匹配的key
```
<!-- 声明使用的exchange定义，在这个规则上绑定了适用其方法的队列 -->
    <rabbit:direct-exchange name="webTestMessExchange">
        <rabbit:bindings>
            <!-- 之前声明的队列 -->
            <rabbit:binding queue="test_queue" key="test_queue_key"/>
        </rabbit:bindings>
    </rabbit:direct-exchange>
```

5. 消息发送到队列中
* 在配置中创建一个可发送消息的bean
  exchange使用之前声明的转发器，将进行自动绑定
```
    <rabbit:template id="sendNoticeTemplate" connection-factory="connectionFactory"
                     exchange="webTestMessExchange"/>
```

* 发送消息
```
@Controller
@RequestMapping("/queue")
public class QueueController {

//在这儿注入bean   bean id为sendNoticeTemplate
    @Autowired
    @Qualifier("sendNoticeTemplate")
    private RabbitTemplate messProducer;


    @RequestMapping(value = "/rabbitmq")
    @ResponseBody
    public String rabbitMq(){
        messProducer.convertAndSend("test_queue_key","hello rabbitmq mess");
        return "rabbitmq is sent";
    }
}
```

6. 监听并接收消息
* 在配置中创建一个队列监听类的bean
```
<bean id="messageReceiver" class="com.lucky.queue.rabbitmqConsumer"/>
```

* 在配置中创建监听方法类的bean
  ref   监听类的bean
  method   接收指定队列的方法
  queue-names  队列名称
```
<rabbit:listener-container connection-factory="connectionFactory">
        <rabbit:listener ref="messageReceiver" method="consumerMess"
                         queue-names="test_queue_name"/>
    </rabbit:listener-container>
```

* 创建一个监听类,里面包含具体的队列接收方法
```
/**
 * rabbitmq的消息消费者
 */
public class rabbitmqConsumer {

    /**
     * 消息消费者
     */
    public void consumerMess(String mess){
        System.out.println(mess);
    }

}

```

7. 加入配置，使其能自动创建
  队列通过指定admin信息，当前producer中的exchange和queue会在rabbitmq服务器上自动生成。
```
<rabbit:admin connection-factory="connectionFactory"/>
```


## 测试
请求接口  http://localhost:8081/queue/rabbitmq
![DC80DC80-C08E-4BE5-9C67-6B30AA0B9D5B](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.VHj5no/DC80DC80-C08E-4BE5-9C67-6B30AA0B9D5B.png)

查看队列中消息，有个消息发出
![11693519-B590-4C3C-93F3-77B643AEF99C](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.HQrV5p/11693519-B590-4C3C-93F3-77B643AEF99C.png)

消息已经接收到
![1930885C-2D34-46AE-B455-3CFCECD3D436](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.5OJcmf/1930885C-2D34-46AE-B455-3CFCECD3D436.png)



### 遇到的坑：

```
channel error; protocol method: #method<channel.close>(reply-code=404, reply-text=NOT_FOUND - no queue 'test_queue_name' in vhost '/', class-id=50, method-id=10)
	at 
```

这个报错的情况是管道中没有找到对应的队列，监听该队列时404，需要手动去创建之后，才能找到。
可加上如下配置，使其找不到队列时，自动创建
```
    <!--通过指定admin信息，当前producer中的exchange和queue会在rabbitmq服务器上自动生成。 -->
    <rabbit:admin connection-factory="connectionFactory"/>
```


## 附录
applicationContext-rabbit.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:rabbit="http://www.springframework.org/schema/rabbit"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/rabbit http://www.springframework.org/schema/rabbit/spring-rabbit.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">



    <!--配置连接-->
    <rabbit:connection-factory id="connectionFactory"
                               host="${rabbitmq.host}"
                               port="${rabbitmq.port}"
                               username="${rabbitmq.username}"
                               password="${rabbitmq.password}"/>

    <!--通过指定admin信息，当前producer中的exchange和queue会在rabbitmq服务器上自动生成。 -->
    <rabbit:admin connection-factory="connectionFactory"/>

    <!-- 声明一个Queue -->
    <!--
    durable:是否持久化
    exclusive: 仅创建者可以使用的私有队列，断开后自动删除
    auto_delete: 当所有消费客户端连接断开后，是否自动删除队列
     -->
    <rabbit:queue id="test_queue" name="test_queue_name" durable="true" exclusive="false"
                  auto-delete="false">
        <rabbit:queue-arguments>
            <entry key="x-ha-policy" value="all"/>
        </rabbit:queue-arguments>
    </rabbit:queue>

    <!-- 声明使用的exchange定义，在这个规则上绑定了适用其方法的队列 -->
    <!--
    rabbit:direct-exchange：定义exchange模式为direct，意思就是消息与一个特定的路由键完全匹配，才会转发。
    rabbit:binding：设置消息queue匹配的key
    -->
    <rabbit:direct-exchange name="webTestMessExchange">
        <rabbit:bindings>
            <!-- 之前声明的队列 -->
            <rabbit:binding queue="test_queue" key="test_queue_key"/>
        </rabbit:bindings>
    </rabbit:direct-exchange>


    <!-- ********************    消息发送模块    ************************-->
    <!--模板类，理论上生产和消费放需要配置，但使用原生的消费接口则可以忽略 -->
    <rabbit:template id="sendNoticeTemplate" connection-factory="connectionFactory"
                     exchange="webTestMessExchange"/>


    <!-- ********************    处理队列的模块   ************************-->
    <!--绑定监听的类-->
    <bean id="messageReceiver" class="com.lucky.queue.rabbitmqConsumer"/>

    <!--配置监听的方法类-->
    <rabbit:listener-container connection-factory="connectionFactory">
        <rabbit:listener ref="messageReceiver" method="consumerMess"
                         queue-names="test_queue_name"/>
    </rabbit:listener-container>




</beans>
```