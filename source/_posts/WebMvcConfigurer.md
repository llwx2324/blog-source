---
title: WebMvcConfigurer
date: 2025-12-02 16:47:00
excerpt: WebMvcConfigurer详细介绍。
---



`WebMvcConfigurer` 是 Spring MVC 中一个非常重要的扩展接口，它允许你在 **不修改 Spring MVC 核心源码的情况下，自定义 MVC 行为**。

你可以理解它为：
 👉 **Spring MVC 的“插件式配置入口”**

------

## ✨ 它能干什么？

实现这个接口后，可以做很多常用配置：

### ✔ 1. 注册拦截器

例如登录检查、权限校验、日志拦截等：

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new LoginInterceptor())
            .addPathPatterns("/**")
            .excludePathPatterns("/login", "/register");
}
```

------

### ✔ 2. 配置跨域（CORS）

允许前端跨域访问：

```java
@Override
public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/**")
            .allowedOrigins("*")
            .allowedMethods("GET", "POST", "PUT", "DELETE");
}
```

------

### ✔ 3. 配置静态资源映射

让 `/images/**` 访问本地文件：

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    registry.addResourceHandler("/images/**")
            .addResourceLocations("file:D:/images/");
}
```

------

### ✔ 4. 配置自定义参数解析器 / 信息转换器

比如 JSON 格式化时间、自定义类型转换：

```java
@Override
public void addFormatters(FormatterRegistry registry) {
    registry.addConverter(new MyDateConverter());
}
```

------

### ✔ 5. 配置视图跳转

传统 MVC 中用于返回页面：

```java
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController("/login").setViewName("login.html");
}
```

------

### ✔ 6. 配置消息转换器（例如 JSON 序列化规则）

如使用 FastJson/Jackson 自定义配置：

```java
@Override
public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
    converters.add(new MappingJackson2HttpMessageConverter());
}
```

------

------

## 🌿 如何使用？

你只需要写一个配置类：

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        System.out.println("注册拦截器");
    }

    // 你需要什么就实现什么方法即可
}
```

⚠ 它 **不是必须实现所有方法**，你按需覆盖就好。

------

------

## 🔥 WebMvcConfigurer 什么时候用？

当你需要：

✔ 自定义拦截器[（拦截器）](https://llwx2324.github.io/blog/2025/12/02/%E6%8B%A6%E6%88%AA%E5%99%A8/)
 ✔ 跨域访问
 ✔ 静态资源映射
 ✔ JSON 自定义转换
 ✔ 页面跳转映射
 ✔ 参数绑定 / 类型转换

——就用它。

------

------

## 🌟 总结一句话

👉 `WebMvcConfigurer` 就是 **扩展 Spring MVC 行为的配置接口**
 让你无需替换整个 MVC 机制，也能定制各种框架功能。

------

