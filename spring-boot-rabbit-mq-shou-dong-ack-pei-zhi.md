* 添加监听器

```java
 @Bean(name = "myListener")
    ChannelAwareMessageListener listener() {
        return new MyListener();
    }

    @Bean
    SimpleMessageListenerContainer msgListener(ConnectionFactory connectionFactory,
                                               @Qualifier("cmsQueue") Queue queue,
                                               @Qualifier("myListener") ChannelAwareMessageListener listener) {
        SimpleMessageListenerContainer simpleMessageListenerContainer = new SimpleMessageListenerContainer(connectionFactory);
        //手动确认
        simpleMessageListenerContainer.setAcknowledgeMode(AcknowledgeMode.MANUAL);
        simpleMessageListenerContainer.setQueues(queue);
        simpleMessageListenerContainer.setMessageListener(listener);
        return simpleMessageListenerContainer;
    } 
```

* 添加自定义监听

```java
import com.rabbitmq.client.Channel;
import com.wh.mobile.logger.service.KafkaLogger;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.core.Message;
import org.springframework.amqp.rabbit.core.ChannelAwareMessageListener;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;


@Component
@Slf4j
public class MyListener implements ChannelAwareMessageListener {

    @Autowired
    HomepageCMSController cmsController;

    @Autowired
    private KafkaLogger logger;

    @Override
    public void onMessage(Message message, Channel channel) throws Exception {
        channel.basicQos(1);
        long deliveryTag = message.getMessageProperties().getDeliveryTag();
        String s = new String(message.getBody());
        logger.info("mq:"+s,"cmsAndmq");
        System.out.println("message:" + s);
        log.info("receiver tag:{%s} msg:{%s}", deliveryTag, message);
        cmsController.getCMSDataFromCMS(s);
        channel.basicAck(deliveryTag, false);
    }
}
```



