---
title: SpringBoot的自动装配
date: 2025-10-26 10:00:00
# tags: [Java, Stream, 函数式编程]
# categories: [编程技术]
excerpt: SpringBoot的自动装配。
---



------


## 🌱 什么是自动装配

Spring Boot 的自动装配就是：

> **根据你引入的依赖，自动帮你把相关的 Bean 配好，不用手动写配置。**

例如：
 你引入了 `spring-boot-starter-web`，Spring Boot 就会自动帮你配置：

- 内嵌 Tomcat
- `DispatcherServlet`
- MVC 相关的 Bean

你什么都不用写，系统就能跑起来。

------

## ⚙️ 核心原理

1. 启动类上的注解：

   ```java
   @SpringBootApplication
   ```

   它包含：

   - `@ComponentScan`（扫描组件）
   - `@EnableAutoConfiguration`（开启自动配置）

2. `@EnableAutoConfiguration` 会去加载所有配置类
    （这些类都写在各个 jar 包里的
    `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` 文件中）

3. Spring Boot 会判断：

   - 哪些类在类路径里存在
   - 哪些 Bean 你自己没配
   - 哪些配置项在 `application.yml` 里定义了

   然后“有条件”地加载对应的配置。

------

## 🎯 举个例子

`DataSourceAutoConfiguration`（数据库自动配置类）：

```java
@Configuration
@ConditionalOnClass(DataSource.class)
@ConditionalOnMissingBean(DataSource.class)
public class DataSourceAutoConfiguration {
    @Bean
    public DataSource dataSource() { ... }
}
```

意思是：

- 只有项目里有 `DataSource` 相关类，
- 而且你没自己写 `DataSource` Bean，
- 它才自动帮你创建一个默认的。

------

## 💡 小结

| 关键点                                           | 含义                   |
| ------------------------------------------------ | ---------------------- |
| `@SpringBootApplication`                         | 启动入口               |
| `@EnableAutoConfiguration`                       | 启动自动配置功能       |
| `@ConditionalOnXXX`                              | 条件判断是否生效       |
| `spring.factories` / `AutoConfiguration.imports` | 存放所有自动配置类列表 |

------

