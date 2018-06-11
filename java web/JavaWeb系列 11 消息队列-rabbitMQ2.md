# JavaWeb系列  11 消息队列-rabbitMQ2
#javaweb

> 该篇主要使用fanout转发器和和topic转发器实现消息队列
>
> 本篇文章，将使用java config和xml相结合的方式配置bean，相得益彰，将注册转发器，队列和绑定队列的bean放置到bean中，生产者和消费者均采用xml的方式配置bean



# fanout转发器
> fanout转发器不关注路由键  routerkey,以广播的概念，转发给每个人
>
> ![4B688133-EFAB-4867-ACBA-184FC2BF7ABF](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.mUKlqq/4B688133-EFAB-4867-ACBA-184FC2BF7ABF.png)




1. *创建转发器的bean*
  fanout转发器
```
	  @Bean
    public FanoutExchange fanoutExchange() {
        return new FanoutExchange("fanoutExchange");
    }
```

2. ::创建两个队列::
  这两个队列用来绑定fanout转发器
```
	  @Bean
    public Queue queueForFanoutOne() {
        return new Queue("queueforfanoutone", true);
    }

    @Bean
    public Queue queueForFanoutTwo() {
        return new Queue("queueforfanouttwo", true);
    }

```

3. ::绑定以上创建的转发器和队列::
  fanout不关注routerKey,所以不用再后面加入with
```
    @Bean
    public Binding bindingQueueOne() {
        return BindingBuilder.bind(queueForFanoutOne()).to(fanoutExchange());
    }

    @Bean
    public Binding bindingQueueTwo(){
        return BindingBuilder.bind(queueForFanoutTwo()).to(fanoutExchange());
    }
```

>
> xml中的配置
>


4. ::配置发送模块::
* 创建bean
```
<!--fanout转发器   发送消息模块   -->
<rabbit:template id="sendNoticeTemPlateFanout" connection-factory="connectionFactory"
                     exchange="fanoutExchange"/>
```

* 注入bean，并使用
```
@Resource(name = "sendNoticeTemPlateFanout")
RabbitTemplate sendNoticeTemPlateFanout;

//发送消息   中间的routerKey可以省略
sendNoticeTemPlateFanout.convertAndSend("fanoutExchange","","this is fanout mess");
```

5. ::消息接收模块::
* 创建可监听的类
```
<bean id="messageReceiverFanout" class="com.lucky.queue.rabbitmqConsumerFanout"/>
```
* 创建监听器
```
<!--监听fanout消息-->
    <rabbit:listener-container connection-factory="connectionFactory">
        <!--以下两个队列监听fanout中的消息-->
        <rabbit:listener ref="messageReceiverFanout" method="consumerMessFanoutOne"
                         queue-names="queueforfanoutone"/>
        <rabbit:listener ref="messageReceiverFanout" method="consumerMessFanoutTwo"
                         queue-names="queueforfanouttwo"/>
    </rabbit:listener-container>
```
* 监听方法的具体实现
  监听方法均在监听类中
```
		//第一个队列消费者
	  public void consumerMessFanoutOne(String mess){
        LOGGER.info("第一个接收者接到:{}",mess);
    }
		//第二个队列消费者
    public void consumerMessFanoutTwo(String mess){
        LOGGER.info("第二个接收者接到:{}",mess);
    }
```

## 测试结果：
发送消息后，两个队列都能收到消息
![3A3FC340-B965-4D6B-9F1E-579CF2FC3074](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.CdTSZc/3A3FC340-B965-4D6B-9F1E-579CF2FC3074.png)



# topic转发器
> topic转发器   通过routerKey模糊匹配
> *  代表后面一个单词模糊匹配
> # 代表后面多个单词模糊匹配
> ![4E1C005C-86D7-4947-9958-CBBED7D4F060](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.cG7aVK/4E1C005C-86D7-4947-9958-CBBED7D4F060.png)

1. ::创建转发器::
```
	  @Bean
    public TopicExchange topicExchange(){
        return new TopicExchange("topicExchange");
    }
```

2. ::创建两个队列::
```
	  @Bean
    public Queue queueOneForTopic(){
        return new Queue("queueOneForTopic",true);
    }

    @Bean
    public Queue queueTwoForTopic(){
        return new Queue("queueTwoForTopic",true);
    }
```

3. ::为刚才创建的队列绑定转发器，并指定routerKey类型::
  bind:绑定队列
  to:转发器
  with:routerKey
```
	  @Bean
    public Binding bindingTopicQueueOne(){
        return BindingBuilder.bind(queueOneForTopic()).to(topicExchange()).with(ROUTERKEY_1);
    }

    @Bean
    public Binding bindingTopicQueueTwo(){
        return BindingBuilder.bind(queueTwoForTopic()).to(topicExchange()).with(ROUTERKEY_2);
    }
```

>
> xml中的配置
>


4. ::配置发送模块::
* 创建bean
```
    <rabbit:template id="sendNoticeTemPlateTopic" connection-factory="connectionFactory"
                     exchange="topicExchange"/>
```

* 注入bean，并使用
```
@Resource(name = "sendNoticeTemPlateTopic")
RabbitTemplate sendNoticeTemPlateTopic;


	int num = 0;
  num = Integer.parseInt(channel);
   switch(num){
       case 1:
           routerKey = "weather-router.abc";
           break;
       case 2:
           routerKey = "weather-router.abc.bcd";
           break;
       case 3:
           routerKey = "msg-router.abc";
           break;
       case 4:
           routerKey = "msg-router.abc.bcd";
           break;
   }

sendNoticeTemPlateTopic.convertAndSend("topicExchange",routerKey,routerKey.toString());
```

5. ::消息接收模块::
* 创建一个监听类
```
<bean id="messageReceiverTopic" class="com.lucky.queue.rabbitmqConsumerTopic"/>
```
* 配置监听方法
```
<!--监听topic消息-->
    <rabbit:listener-container connection-factory="connectionFactory">
        <rabbit:listener ref="messageReceiverTopic" method="consumerMessTopicOne"
                         queue-names="queueOneForTopic"/>
        <rabbit:listener ref="messageReceiverTopic" method="consumerMessTopicTwo"
                         queue-names="queueTwoForTopic"/>
    </rabbit:listener-container>
```
* 监听方法具体实现
  监听方法均在监听类中
```
	  /**
     * 消息消费者
     *    weather-router.*
     */
	  public void consumerMessTopicOne(String mess){
        LOGGER.info("第一个接收者接到:{}",mess);
    }

    /**
     * 消息消费者
     *   msg-router.#
     */
    public void consumerMessTopicTwo(String mess){
        LOGGER.info("第二个接收者接到:{}",mess);
    }
```

## 测试
* routerKey为   weather-router.abc
  预期只有队列1收到
  测试现状如下
  ![229ABB66-76CB-4B91-A534-C63E6ADB8125](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.r5115C/229ABB66-76CB-4B91-A534-C63E6ADB8125.png)

* routerKey为   weather-router.abc.bcd
  预期两个队列都没法匹配到，没有队列能消费消息
  测试现状如下
  ![0E7ACA33-D4C2-48EF-8F4A-2777CE2B78FF](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.RWaqUC/0E7ACA33-D4C2-48EF-8F4A-2777CE2B78FF.png)

* routerKey为   msg-router.abc
  预期只有队列2收到
  测试现状如下
  ![F7B0BDED-4217-44F0-90BE-A6C8421EB650](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.lVw5sf/F7B0BDED-4217-44F0-90BE-A6C8421EB650.png)


* routerKey为   weather-router.abc.bcd
  预期队列2能匹配
  测试现状如下
  ![382E6076-BFB5-4102-9972-D894A1150038](/var/folders/t7/kfsxsprj403bcjn1x7tpnfqr0000gp/T/net.shinyfrog.bear/BearTemp.byCtcr/382E6076-BFB5-4102-9972-D894A1150038.png)
