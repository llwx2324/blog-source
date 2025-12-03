---
title: Stream流
date: 2025-10-19 10:00:00
excerpt: Stream流的简要介绍。
---

## 🧩 一、Stream 是什么？

> **Stream（流）** 是对集合（或数组）中元素的一种"高级遍历与处理方式"。

它不是集合本身，而是一个用来描述"数据流动和操作"的抽象层。


 过滤（filter）→ 转换（map）→ 收集结果（collect）
 这一条链式调用就是典型的 **流式编程（Stream API）**。


------

## 🧭 二、Stream 的核心设计理念

Stream 的使用分为 **三步走**：

| 阶段       | 说明                                     | 关键方法                                      |
| ---------- | ---------------------------------------- | --------------------------------------------- |
| 1️⃣ 创建流   | 从集合、数组等创建流                     | `stream()`、`of()`                            |
| 2️⃣ 中间操作 | 对流中的元素进行处理（**不会立即执行**） | `filter()`、`map()`、`sorted()`、`distinct()` |
| 3️⃣ 终止操作 | 执行最终计算（**触发整个流的执行**）     | `collect()`、`count()`、`forEach()`           |

⚠️ **中间操作是惰性的**：
 只有执行终止操作时，流才会真正遍历数据。

------

## 🧮 三、Stream 常用操作系统整理

### （1）创建流

```java
List<String> list = List.of("a", "b", "c");

// 从集合创建
Stream<String> s1 = list.stream();

// 从数组创建
Stream<Integer> s2 = Arrays.stream(new Integer[]{1, 2, 3});

// 静态方法创建
Stream<Double> s3 = Stream.of(3.14, 2.71, 1.0);

// 无限流（需限制）
Stream<Integer> s4 = Stream.iterate(1, n -> n + 1).limit(10);
```

------

### （2）中间操作（不会立刻执行）

| 方法                | 作用             | 示例                                 |
| ------------------- | ---------------- | ------------------------------------ |
| `filter(Predicate)` | 过滤元素         | `.filter(x -> x > 0)`                |
| `map(Function)`     | 元素映射（转换） | `.map(String::length)`               |
| `flatMap(Function)` | 扁平化多个流     | `.flatMap(List::stream)`             |
| `distinct()`        | 去重             | `.distinct()`                        |
| `sorted()`          | 排序             | `.sorted(Comparator.naturalOrder())` |
| `peek(Consumer)`    | 调试查看         | `.peek(System.out::println)`         |
| `limit(n)`          | 截取前 n 个      | `.limit(10)`                         |
| `skip(n)`           | 跳过前 n 个      | `.skip(5)`                           |

------

### （3）终止操作（触发执行）

| 方法                                        | 功能                     |
| ------------------------------------------- | ------------------------ |
| `collect()`                                 | 收集结果（通常转回集合） |
| `count()`                                   | 统计数量                 |
| `forEach()`                                 | 遍历输出                 |
| `findFirst()` / `findAny()`                 | 查找元素                 |
| `allMatch()` / `anyMatch()` / `noneMatch()` | 匹配判断                 |
| `reduce()`                                  | 归约求和、合并操作       |

------

## 🚀 四、Stream 的高级用法

### （1）`map` vs `flatMap`

```java
// map: 一对一
List<String> list = List.of("a,b", "c,d");
list.stream()
    .map(s -> s.split(",")) // Stream<String[]>
    .collect(Collectors.toList());

// flatMap: 一对多，打平
list.stream()
    .flatMap(s -> Arrays.stream(s.split(","))) // Stream<String>
    .collect(Collectors.toList()); // ["a","b","c","d"]
```

------

### （2）并行流（parallelStream）

利用多核 CPU 提高性能：

```java
List<Integer> nums = IntStream.rangeClosed(1, 1000000).boxed().toList();
long sum = nums.parallelStream()
               .mapToLong(Long::valueOf)
               .sum();
```

⚠️ 并行流适合**无共享资源、计算量大**的场景，否则反而更慢。

------

## 📦 五、collect 的常见用法

```java
List<String> names = users.stream()
    .map(User::getName)
    .collect(Collectors.toList());

Set<String> set = list.stream().collect(Collectors.toSet());

Map<Integer, String> map = users.stream()
    .collect(Collectors.toMap(User::getId, User::getName));

Double avg = list.stream()
    .collect(Collectors.averagingInt(String::length));
```

------

## 🧠 六、Stream 的特性总结

| 特性         | 说明                     |
| ------------ | ------------------------ |
| 不存储数据   | 只对原集合的元素进行计算 |
| 不修改源数据 | 操作结果是新的流         |
| 惰性求值     | 中间操作不会立刻执行     |
| 可并行       | 可利用多核并行计算       |

------

## ✅ 七、完整实战例子

```java
List<User> users = List.of(
    new User("Tom", 20),
    new User("Jerry", 22),
    new User("Spike", 19)
);

List<String> result = users.stream()
    .filter(u -> u.getAge() >= 20)     // 过滤
    .sorted(Comparator.comparing(User::getAge)) // 排序
    .map(User::getName)                // 提取名字
    .collect(Collectors.toList());     // 收集结果
```

输出：

```java
["Tom", "Jerry"]
```

------

## 🧩 八、与函数式接口的关系

Stream 中大量使用 Java 8 的函数式接口：

| 操作        | 对应接口         | 参数         | 返回值 |
| ----------- | ---------------- | ------------ | ------ |
| `filter()`  | `Predicate<T>`   | T → boolean  |        |
| `map()`     | `Function<T, R>` | T → R        |        |
| `forEach()` | `Consumer<T>`    | T → void     |        |
| `sorted()`  | `Comparator<T>`  | (T, T) → int |        |



