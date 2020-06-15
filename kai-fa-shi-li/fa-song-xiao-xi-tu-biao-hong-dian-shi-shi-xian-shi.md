# 发送消息图标红点实时显示

### 实现效果

![](https://i.loli.net/2018/08/26/5b82acf3c677b.jpg)

![](https://i.loli.net/2018/08/26/5b82acf3af8f2.png)

> Stomp是一种简单（流）文本定向消息协议，提供了一个可互操作的链接格式。允许stomp客户端与任意stomp消息代理（Broker）进行交互。STOMP协议由于设计简单，易于开发客户端，因此在多种语言和多种平台上得到广泛地应用。

### 后端

* 添加依赖

```text
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-websocket</artifactId>
</dependency>
```

* 配置类

```text
/**
 * @author Exrickx
 */
@Configuration
@EnableWebSocketMessageBroker
public class WebSocketStompConfig implements WebSocketMessageBrokerConfigurer {

    /**
     * 注册stomp端点
     * @param registry
     */
    @Override
    public void registerStompEndpoints(StompEndpointRegistry registry) {

        // 允许使用socketJs方式访问 即可通过http://IP:PORT/ws来和服务端websocket连接
        registry.addEndpoint("/ws").setAllowedOrigins("*").withSockJS();
    }

    /**
     * 配置信息代理
     * @param registry
     */
    @Override
    public void configureMessageBroker(MessageBrokerRegistry registry) {

        // 订阅Broker名称 user点对点 topic广播即群发
        registry.enableSimpleBroker("/user","/topic");
        // 全局(客户端)使用的消息前缀
        registry.setApplicationDestinationPrefixes("/app");
        // 点对点使用的前缀 无需配置 默认/user
        registry.setUserDestinationPrefix("/user");
    }
}
```

* 由于只做广播和点对点的消息推送，这里只用到订阅发布

```text
    @Autowired
    private SimpMessagingTemplate messagingTemplate;

    // 广播
    messagingTemplate.convertAndSend("/topic/subscribe", "您收到了新的系统消息");

    // 通过用户ID实现点对点
    messagingTemplate.convertAndSendToUser(id,"/queue/subscribe", "您收到了新的消息");
```

### 前端

* 使用到 [vue-stomp](https://github.com/FlySkyBear/vue-stomp)
* 首先用户登录加载的时候读取数据获取未读消息数，若有显示红点标记
* 连接Websocket，订阅广播和点对点，收到消息后未读消息数+1

  ```text
  onConnected(frame) {
    console.log("连接ws成功: " + frame);
    // 订阅广播系统通知
    this.$stompClient.subscribe(
      "/topic/subscribe",
      this.responseCallback,
      this.onFailed
    );
    // 订阅点对点 通过用户id指定用户
    this.$stompClient.subscribe(
      "/user/" + this.userId + "/queue/subscribe",
      this.responseCallback,
      this.onFailed
    );
  },
  responseCallback(frame) {
    console.log("收到消息：" + frame.body);
    // 收到消息 未读消息数+1
    this.messageCount = Number(this.messageCount) + 1;
  }
  ```

