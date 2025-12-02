---
title: component与配置类注册bean的区别、maven作用域（结合实际场景）
date: 2025-11-08 16:00:00
excerpt: component与配置类注册bean的区别、maven provided 作用域（结合实际场景）。
---
------

## 问题背景

1. `hm-common`模块里编写了`RabbitMQHelper`工具类：

```java
package com.hmall.common.utils;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.rabbit.connection.CorrelationData;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.concurrent.ListenableFutureCallback;
import java.util.UUID;

@Slf4j
@Component
@RequiredArgsConstructor
public class RabbitMQHelper {

    private final RabbitTemplate rabbitTemplate;
	// 防止篇幅太长，方法就简写示意一下了
    public void sendMessage(String exchange, String routingKey, Object message) {
        rabbitTemplate.convertAndSend(exchange, routingKey, message);
    }

    public void sendDelayMessage(String exchange, String routingKey, Object message, int delay)

    public void sendMsgWithConfirm(String exchange, String routingKey, Object msg, int maxRetries) {
        sendMsgWithConfirmInternal(exchange, routingKey, msg, maxRetries, 0);
    }

    private void sendMsgWithConfirmInternal(String exchange, String routingKey, Object msg, int maxRetries, int currentRetry) {
}
```

2. `hm-common`模块里有amqp和rabbit相关依赖但是scope是provided

   

3. 其他继承了`hm-common`模块的模块调用`RabbitMQHelper`工具类来发送消息，报错：

   ```java
   Caused by: java.lang.NoClassDefFoundError: org/springframework/amqp/core/Message
   Caused by: java.lang.ClassNotFoundException: org.springframework.amqp.core.Message
   ```

4. 将`RabbitMQHelper`工具类做如下`改动`之后就不报错正常运行了

   - 去掉`@Component`

     ```java
     package com.hmall.common.utils;
     
     import lombok.RequiredArgsConstructor;
     import lombok.extern.slf4j.Slf4j;
     import org.springframework.amqp.rabbit.connection.CorrelationData;
     import org.springframework.amqp.rabbit.core.RabbitTemplate;
     import org.springframework.stereotype.Component;
     import org.springframework.util.concurrent.ListenableFutureCallback;
     import java.util.UUID;
     
     @Slf4j
     @RequiredArgsConstructor
     public class RabbitMQHelper {
     
         private final RabbitTemplate rabbitTemplate;
     	// 防止篇幅太长，方法就简写示意一下了
         public void sendMessage(String exchange, String routingKey, Object message) {
             rabbitTemplate.convertAndSend(exchange, routingKey, message);
         }
     
         public void sendDelayMessage(String exchange, String routingKey, Object message, int delay)
     
         public void sendMsgWithConfirm(String exchange, String routingKey, Object msg, int maxRetries) {
             sendMsgWithConfirmInternal(exchange, routingKey, msg, maxRetries, 0);
         }
     
         private void sendMsgWithConfirmInternal(String exchange, String routingKey, Object msg, int maxRetries, int currentRetry) {
     }
     ```

     

   - 在配置类中注册为`Bean`

     ```java
     	@Bean
         public RabbitMQHelper rabbitMQHelper(RabbitTemplate rabbitTemplate) {
             return new RabbitMQHelper(rabbitTemplate);
         }
     ```

     

## 解读

1. `@Component`

   - 被标注了 `@Component`的类，Spring 在**启动时就会扫描并创建 Bean**，并且，Spring 会**加载类及其构造器、字段类型**。而 `RabbitMQHelper` 构造器有 `RabbitTemplate` 参数，而 `RabbitTemplate` 依赖`org.springframework.amqp.core.Message`等类。`common` 模块里 `spring-rabbit` 依赖的 scope 被标记为 `provided`，**编译可以用，但运行时 JVM 找不到** `Message` 类。

     所以启动时报 `NoClassDefFoundError`。

2. `@Bean`：

   - 使用**配置类注册 Bean** 时，Spring **延迟创建 Bean**，直到**调用**方法时才真正需要 `RabbitTemplate`。这时才会用到 `spring-rabbit` 依赖。而调用方法的模块是有`spring-rabbit` 依赖的

| 方式                                    | 特点                      | 风险                                              |
| --------------------------------------- | ------------------------- | ------------------------------------------------- |
| `@Component + @RequiredArgsConstructor` | Spring 扫描组件，自动注入 | 扫描时必须能加载类依赖，否则 NoClassDefFoundError |
| 配置类 `@Bean` 方法                     | 手动创建 Bean，延迟注入   | 扫描时不解析类依赖，风险低                        |

3. `provided`: （相关详解：[Maven 核心知识体系](https://llwx2324.github.io/blog/2025/11/08/Maven%20%E6%A0%B8%E5%BF%83%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB/)）

- 作用域：编译（有） + 运行（无） + 打包（无）

⚠️ 如果 common 模块不直接依赖 AMQP，只在别的模块使用 `RabbitMQHelper`，那用provided + 配置类注册是最好的方式。



