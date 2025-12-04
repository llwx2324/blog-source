---
title: Fastjson、Jackson、Hutool JSONUtil 对比
date: 2025-12-03 11:01:00
excerpt: Fastjson、Jackson、Hutool JSONUtil 对比。
---


**Fastjson、Jackson、Hutool JSONUtil 对比 + 常用示例总结**

------

# 🔥 三大 JSON 工具对比表

| 特点             | **Fastjson2**        | **Jackson**             | **Hutool JSONUtil** |
| ---------------- | -------------------- | ----------------------- | ------------------- |
| 谁开发           | 阿里                 | FasterXML / Spring 默认 | 国人开源工具 Hutool |
| 使用场景         | 性能敏感，高速序列化 | SpringBoot / 企业项目   | 工具类开发、小项目  |
| Spring Boot 支持 | 非默认               | ✔️ 默认                  | ❌ 非默认            |
| 扩展性           | 中等                 | 非常强                  | 偏轻量              |
| 泛型支持         | 简单                 | 需 TypeReference        | 自动推断较方便      |
| JSONPath         | ✔ 支持               | 可插件                  | ✔ 简单              |
| 学习成本         | 低                   | 中等                    | 极低                |

👉 如果你做 SpringBoot 业务系统：**Jackson 是最佳**
 👉 写脚本/工具类：**Hutool JSONUtil 方便简洁**
 👉 关注性能/国内老项目：**Fastjson2 常用**

------

------

# ✅ 每个库的核心用法（能直接复制用）

------

------

## ✔️ Fastjson2

### 📌 Maven

```xml
<dependency>
    <groupId>com.alibaba.fastjson2</groupId>
    <artifactId>fastjson2</artifactId>
    <version>2.0.43</version>
</dependency>
```

### 🔸 对象 → JSON

```java
String str = JSON.toJSONString(obj);
```

### 🔸 JSON → 对象

```java
User u = JSON.parseObject(str, User.class);
```

### 🔸 JSON 数组 → List

```java
List<User> list = JSON.parseArray(str, User.class);
```

📌 **特点**：写法短、性能快，企业旧项目很多用它。

------

------

## ✔️ Jackson（SpringBoot 默认 JSON 工具）

### 📌 SpringBoot 自动带，不需额外依赖

### 🔸 对象 → JSON

```java
ObjectMapper mapper = new ObjectMapper();
String str = mapper.writeValueAsString(obj);
```

### 🔸 JSON → 对象

```java
User u = mapper.readValue(str, User.class);
```

### 🔸 JSON 数组 → List（注意 TypeReference）

```java
List<User> list = mapper.readValue(str, new TypeReference<List<User>>(){});
```

📌 **特点**：

✔ 控制复杂字段输出
 ✔ 适合 Redis JSON 存储
 ✔ 企业项目首选

------

------

## ✔️ Hutool JSONUtil

📌 一个工具化框架，用法超级简单

### 🔸 Maven

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.8.28</version>
</dependency>
```

### 🔸 对象 → JSON

```java
String json = JSONUtil.toJsonStr(obj);
```

### 🔸 JSON → 对象

```java
User u = JSONUtil.toBean(jsonStr, User.class);
```

### 🔸 JSON 数组 → List

```java
List<User> list = JSONUtil.toList(jsonStr, User.class);
```

📌 **特点**：

✔ 语法最短
 ✔ 写 Demo 或工具类最舒服
 ✔ 适合快速开发

------

------

# ⭐ 推荐项目中用法（工程最佳实践）

| 位置                            | 推荐 JSON 工具              |
| ------------------------------- | --------------------------- |
| Controller 参数解析 / 返回值    | Jackson                     |
| Redis JSON 存储                 | Jackson JSON Serializer     |
| SpringBoot 项目统一 JSON 工具类 | Jackson + ObjectMapper Bean |
| 小工具 / 单测脚本               | Hutool JSONUtil             |
| 追求极致性能                    | Fastjson2                   |

------

------

# ⚡ 实战：统一 JSONUtil 封装（能放到你项目 utils 包里）

```java
public class JsonUtil {

    private static final ObjectMapper mapper = new ObjectMapper();

    public static String toJson(Object obj) {
        try {
            return mapper.writeValueAsString(obj);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    public static <T> T toObj(String json, Class<T> clazz) {
        try {
            return mapper.readValue(json, clazz);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    public static <T> T toObj(String json, TypeReference<T> type) {
        try {
            return mapper.readValue(json, type);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

📌 项目依赖 Jackson，其他库想用就局部使用。

------

------

# 🔍 面试思路怎么答？

> **问：Fastjson、Jackson、Hutool JSONUtil 区别？**

答：

✔ Jackson 是 Spring 默认的 JSON 序列化工具，可扩展性最好，生产推荐
 ✔ Fastjson2 性能更高，写法简洁，但生态略弱
 ✔ Hutool JSONUtil 最轻量，写工具类非常方便

> **问：Redis 推荐哪个？**

✔ Jackson，因为 Spring Data Redis 默认就是用 Jackson 序列化器