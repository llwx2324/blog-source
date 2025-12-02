---
title: MVC、Spring MVC和三层架构
date: 2025-12-02 11:26:00
excerpt: MVC、Spring MVC和三层架构的区别对比。
---
------

------



------

# ✅ 1. MVC 简洁理解

| 组件           | 职责                         |
| -------------- | ---------------------------- |
| **Model**      | 数据与业务逻辑               |
| **View**       | 显示数据（页面/响应）        |
| **Controller** | 接收请求、调度业务、返回视图 |

📌 一句话：

> **Controller 协调流程，Model 负责计算存储，View 负责展示输出。**

------

------

# 🟦 2. MVC 架构图

```
     +-------------+
     |   View      |  — 展示层
     +-------------+
            ↑
     +-------------+
     | Controller  |  — 请求调度
     +-------------+
            ↓
     +-------------+
     |   Model     |  — 数据 + 业务
     +-------------+
```

------

------

# 🟩 3. 三层架构与 MVC 区别

三层架构关注**整个系统层次划分**：

```
+-----------------------------+
| Presentation Layer (表现层) |
|  ⇢ Controller + View        |
+-----------------------------+
| Business Layer (业务层)     |
|  ⇢ Service                  |
+-----------------------------+
| Data Access Layer (数据层)  |
|  ⇢ Mapper / Repository      |
+-----------------------------+
```

📌 对比结论：

| 区别点           | MVC                | 三层架构                   |
| ---------------- | ------------------ | -------------------------- |
| 关注范围         | 表现层内部职责划分 | 整个系统结构               |
| Model 包含何物   | 数据 + 业务        | 仅数据模型，业务放 Service |
| 是否涉及数据库层 | ❌ 不涉及           | ✔ 包含                     |

✔ 关键一句话总结：

> **MVC 作用于表示层，而三层架构覆盖整个系统。**

------

------

# 🔶 4. Spring MVC 与 MVC 的关系

Spring MVC 是 Java Web 中对 MVC 的**框架化实现**。

在 Spring MVC 中对应关系：

| MVC 概念   | Spring MVC 实现                      |
| ---------- | ------------------------------------ |
| Controller | `@Controller` / `@RestController`    |
| Model      | Service + 实体类 + ModelMap          |
| View       | Thymeleaf/JSP 页面 / JSON 响应       |
| Dispatcher | Spring 特有机制：`DispatcherServlet` |

------

------

# 🟥 5. Spring MVC 请求执行流程图（关键理解）

```
客户端请求
    ↓
DispatcherServlet (前端控制器)
    ↓
HandlerMapping 找 Controller
    ↓
Controller 处理逻辑，调用 Service
    ↓
Service 操作业务、调用 Mapper
    ↓
返回 Model 数据
    ↓
ViewResolver 找视图
    ↓
View 输出响应（页面/JSON）
```

📌 总结一句话：

> **Spring MVC = MVC 模式 + 调度器（DispatcherServlet）+ 自动路由与视图解析机制。**

------

------

# ⭐ 最终总结版

✔ **MVC 用于拆分表示层职责：Controller 调度，Model 处理数据逻辑，View 展示。**
 ✔ **三层架构比 MVC 更大，它分表现层、业务层、数据层，强调系统整体解耦。**
 ✔ **Spring MVC 是 MVC 在 Java Web 下的实现，引入 DispatcherServlet 完成路由、参数解析、视图解析等工作。**

