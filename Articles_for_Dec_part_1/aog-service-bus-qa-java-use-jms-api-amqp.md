# 使用 JAVA AMQP 协议如何订阅启用分区的 Topic 的消息  

## 问题：  

通常借助 Java JMS  API 使用 AMQP 协议订阅启用分区的 Azure Service Bus Topic，会报错, 而订阅未启用分区功能的 Azure Service Bus Topic 就正常。  

**报错信息为：**  

    JMS Exception: Cannot open a Topic client for entity type Subscriber

## 解决方法：  

使用 Azure Service Bus Queue 订阅的方式订阅 Topic 消息，Topic 订阅者对应 queue 的 entity path 为  `[Topic Name]/Subscriptions/[Subscription Name]`

**代码如下：**  

```java

Context context = new InitialContext();
ConnectionFactory factory = (ConnectionFactory) context.lookup("myFactoryLookup");
Connection connection = factory.createConnection(USER, PASSWORD);
connection.setExceptionListener(new ExceptionListener());
connection.start();

TopicSession session = (TopicSession) connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
Topic topic = session.createTopic("test");
// 对未启用partition的topic可以用TopicSubscriber订阅消息
// TopicSubscriber subscriber = session.createDurableSubscriber(topic, "subscription1");
// subscriber.setMessageListener(new MessageListener());

// 对启用partition的topic 只能用MessageConsumer来订阅消息
MessageConsumer messageConsumer = session.createConsumer(session.createQueue("test/Subscriptions/sub1"));
messageConsumer.setMessageListener(new MessageListener());

MessageProducer messageProducer = session.createProducer(topic);
Message message = session.createTextMessage("Hello world1213!");
messageProducer.send(message);

```
