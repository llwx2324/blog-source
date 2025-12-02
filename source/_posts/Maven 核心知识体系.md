---
title: Maven 核心知识体系
date: 2025-11-08 16:00:00
excerpt: Maven 核心知识体系。
---
------

------

## 1️⃣ Maven 基本概念

- **Maven** 是一个项目管理和构建工具，用于：
  1. **依赖管理**（自动下载 jar 包及传递依赖）
  2. **项目构建**（编译、打包、测试、部署）
  3. **生命周期管理**（规范化构建流程）
- **核心文件**：`pom.xml`
  - 描述项目的 **坐标**（groupId、artifactId、version）
  - 声明 **依赖**、**插件**、**模块** 等

------

## 2️⃣ Maven 坐标

| 坐标       | 说明                             |
| ---------- | -------------------------------- |
| groupId    | 组织或公司标识，如 `com.yehai`   |
| artifactId | 项目名或模块名，如 `common`      |
| version    | 项目版本，如 `1.0.0`             |
| packaging  | 打包类型：`jar`、`war`、`pom` 等 |

- 父子模块关系：
  - 父 POM 用 `<packaging>pom</packaging>` 管理多个子模块
  - 子模块通过 `<parent>` 引用父 POM

------

## 3️⃣ 依赖管理

### 依赖声明

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>2.7.18</version>
    <scope>compile</scope>
</dependency>
```

- **作用域 (scope)**：

| scope    | 编译时 | 运行时 | 打包 | 典型用途                      |
| -------- | ------ | ------ | ---- | ----------------------------- |
| compile  | ✅      | ✅      | ✅    | 默认，核心依赖                |
| provided | ✅      | ❌      | ❌    | 容器/环境提供，如 servlet-api |
| runtime  | ❌      | ✅      | ✅    | 仅运行时依赖，如 JDBC driver  |
| test     | ❌      | ❌      | ❌    | 测试依赖，如 JUnit            |
| system   | ✅      | ✅      | ❌    | 本地 jar，不推荐              |

### 传递依赖

- Maven 自动解析依赖的依赖（transitive dependency）
- 可以通过 `<dependencyManagement>` 或 `<exclusions>` 控制版本冲突或排除依赖

------

## 4️⃣ Maven 生命周期

| 生命周期    | 阶段     | 作用                                      |
| ----------- | -------- | ----------------------------------------- |
| **clean**   | clean    | 清理上次构建生成的文件（通常是 `target`） |
| **default** | validate | 验证项目结构和必要信息是否完整            |
|             | compile  | 编译主源码                                |
|             | test     | 运行单元测试                              |
|             | package  | 打包成 jar/war                            |
|             | verify   | 验证打包是否正确                          |
|             | install  | 安装到本地Maven仓库 (`~/.m2`)             |
|             | deploy   | 部署到远程Maven仓库                       |
| **site**    | site     | 生成项目文档                              |

- **顺序执行**：运行某个阶段时，会依次执行**同周期**的前面的阶段

  ```bash
  mvn package
  # 等效执行: （没有clean）validate → compile → test → package
  ```

------

## 5️⃣ Maven 插件

- Maven 的功能主要由插件提供
- 常用插件：

| 插件                     | 功能                   |
| ------------------------ | ---------------------- |
| maven-compiler-plugin    | 编译 java 代码         |
| maven-surefire-plugin    | 单元测试执行           |
| maven-jar-plugin         | 打 jar 包              |
| maven-war-plugin         | 打 war 包              |
| maven-dependency-plugin  | 分析/复制依赖          |
| spring-boot-maven-plugin | Spring Boot 打包和运行 |

------

## 6️⃣ 多模块项目

- 父模块 (packaging=pom)
- 子模块通过 `<modules>` 声明
- 子模块可以继承父模块的依赖管理、插件管理、属性
- Maven 会按顺序构建父模块 → 子模块

------

## 7️⃣ 依赖冲突处理

- 当多个版本的依赖出现时，Maven 使用 **最近优先原则**：

  - 以 **依赖树中最短路径**的版本为准

- 可以通过：

  ```bash
  mvn dependency:tree
  ```

  查看依赖树和冲突