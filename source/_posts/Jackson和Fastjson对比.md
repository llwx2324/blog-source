---
title: Jackson和Fastjson对比
date: 2025-12-03 11:01:00
excerpt: Jackson和Fastjson对比。
---


给你一句**一眼就能记住的答案**：

> **SpringBoot / 企业项目 → 选 Jackson**
>
> **工具脚本 / 简单 JSON 操作 → Fastjson 也行**

下面给你**结构化比较**👇

------

------

# ✅ 一句话结论

✔ **Jackson 是 Spring 官方推荐的 JSON 序列化库**
 ✔ **Fastjson 性能强、使用方便，但稳定性和安全性差点**

------

------

------

# ⭐ Jackson vs Fastjson 深度对比

| 对比项              | Jackson        | Fastjson                 |
| ------------------- | -------------- | ------------------------ |
| SpringBoot 默认集成 | ✔ 自带         | ✘ 需自行引入             |
| 安全性              | ✔ 更严格       | ⚠ 出过反序列化漏洞       |
| 稳定性              | ✔ 大型项目首选 | 较好，但维护节奏慢       |
| 性能                | 较快           | ⭐ 很快（尤其大规模数据） |
| API 设计            | 稍复杂、偏规范 | ⭐ 超级易用               |
| 序列化控制          | 强、细粒度     | 中等                     |
| 在国内框架适配      | ✔ 最广         | 好                       |

------

------

------

# 🔹 为什么 SpringBoot 默认用 `Jackson`？

✔ 对 Java8 LocalDateTime 支持更完善
 ✔ 模块生态丰富
 ✔ 与 Spring MVC 数据绑定深度集成
 ✔ 更稳定、更新快、漏洞响应及时
 ✔ 更适合 Web API

📌 所以你只要用 SpringBoot，Jackson 就是首选。

------

------

------

# 🔹 Fastjson 的优点

⭐ 写起来特别舒服
 ✔ 代码更短
 ✔ 性能很好
 ✔ `JSON.toJSONString()` / `JSON.parseObject()` 调用自然

例如：

```java
List<User> list = JSON.parseArray(json, User.class);
```

非常爽。

------

------

------

# 🔹 但为什么很多大公司不用 Fastjson？

⚠ 多次爆发 **反序列化远程执行漏洞（RCE）**
 ⚠ 默认开启 autoType 可能导致安全隐患
 ⚠ 协议不严谨、异常信息不友好

虽然 Fastjson 2.x 做了安全加强，但：
 👉 企业生产环境一般更倾向稳定与安全 —— 所以用 **Jackson**

------

------

------

# 🔥 用法对比：哪个更优雅？

### ⭐ Fastjson（简洁）

```java
String json = JSON.toJSONString(obj);
User u = JSON.parseObject(json, User.class);
List<User> list = JSON.parseArray(json, User.class);
```

### ✔ Jackson（更规范）

```java
String json = mapper.writeValueAsString(obj);
User u = mapper.readValue(json, User.class);
List<User> users = mapper.readValue(json,new TypeReference<List<User>>() {});
```

➡ **如果你只是写工具/脚本/日志输出** → Fastjson 很爽
 ➡ **如果你是后台系统 / Spring 接口 / Redis封装** → Jackson 更稳

------

------

