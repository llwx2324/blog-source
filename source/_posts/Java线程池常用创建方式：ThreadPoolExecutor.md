---
title: Java线程池常用创建方式：ThreadPoolExecutor
date: 2025-12-04 16:01:00
excerpt: Java线程池常用创建方式：ThreadPoolExecutor。
---


线程池（Thread Pool）是**管理和复用线程**的机制，用来避免频繁创建、销毁线程带来的性能开销，是 Java 并发中最核心的组件之一。

------

# 🔹一、为什么需要线程池？

### ❌不用线程池的问题

- 每次执行任务都要新建线程 → **开销大**
- 线程过多 → **CPU 上下文切换频繁**
- 线程不受控 → **可能耗尽系统资源**

### ✔️线程池好处

- **线程复用**：任务执行完线程不会销毁，而是继续执行下一个任务
- **限制最大并发数**，防止线程大量创建
- **任务排队执行**
- **提供监控统计能力**

------

# 🔹二、Java 中的线程池

### ⭐最核心的类：

```
java.util.concurrent.ThreadPoolExecutor
```

我们一般推荐**自己创建，而不是直接用 Executors 工厂方法**。

------

# 🔹三、线程池核心参数（ThreadPoolExecutor）

构造函数：

```java
ThreadPoolExecutor(
    int corePoolSize,         // 核心线程数
    int maximumPoolSize,      // 最大线程数
    long keepAliveTime,       // 空闲线程存活时间
    TimeUnit unit,            // 存活时间单位
    BlockingQueue<Runnable> workQueue, // 任务队列
    ThreadFactory threadFactory,       // 线程工厂
    RejectedExecutionHandler handler   // 拒绝策略
)
```

📌**解释一遍你就懂了：**

| 参数              | 作用                                         |
| ----------------- | -------------------------------------------- |
| `corePoolSize`    | 最小保留线程数，线程池会一直保留             |
| `maximumPoolSize` | 高峰时最多能创建多少线程                     |
| `workQueue`       | 超过核心线程时任务进入队列排队               |
| `keepAliveTime`   | 空闲线程超过这个时间会回收（仅限非核心线程） |
| `threadFactory`   | 自定义线程名称、优先级                       |
| `handler`         | 队列满了 & 线程也满了时的处理策略            |

------

# 🔹四、任务执行流程（面试高频）

👇线程池处理一个任务的逻辑：

1️⃣ 小于核心线程数 → **新建线程执行任务**

2️⃣ 核心线程满了 → **任务进入队列**

3️⃣ 队列满了 → **继续创建线程（直到最大线程数）**

4️⃣ 最大线程数满了 → **触发拒绝策略**

------

# 🔹五、任务队列（BlockingQueue）

Java 内置的队列：

✔ `LinkedBlockingQueue` —— 无界队列
 ✔ `ArrayBlockingQueue` —— 有界队列
 ✔ `SynchronousQueue` —— 不存任务，直接交给线程执行（CachedThreadPool用的）
 ✔ `PriorityBlockingQueue` —— 带优先级

------

# 🔹六、拒绝策略（RejectedExecutionHandler）

当任务太多时，线程池会触发策略，默认 4 种：

| 策略                | 行为                     |
| ------------------- | ------------------------ |
| AbortPolicy         | 直接抛异常               |
| CallerRunsPolicy    | 任务回退到调用者线程执行 |
| DiscardPolicy       | 直接丢弃任务             |
| DiscardOldestPolicy | 丢弃队列里最旧的任务     |

------

# 🔹七、线程池常用创建方式

## ✔ 推荐使用 ThreadPoolExecutor

```java
ExecutorService pool = new ThreadPoolExecutor(
        2,
        5,
        60,
        TimeUnit.SECONDS,
        new LinkedBlockingQueue<>(10),
        Executors.defaultThreadFactory(),
        new ThreadPoolExecutor.AbortPolicy()   // 拒绝策略
);
```

------

## ❌ 不推荐 Executors 的原因（面试高频）

| 方法                        | 问题                   |
| --------------------------- | ---------------------- |
| `newFixedThreadPool()`      | 使用无界队列，可能 OOM |
| `newCachedThreadPool()`     | 线程无限增长风险       |
| `newScheduledThreadPool()`  | 任务堆积可能 OOM       |
| `newSingleThreadExecutor()` | 无界队列也可能 OOM     |

> 阿里规范说：**禁止使用 Executors 创建线程池**

------

# 🔹八、线程池支持的工作方式

### ①提交 Runnable 或 Callable

```java
pool.execute(new RunnableTask());
pool.submit(new CallableTask());
```

### ②获取结果

```
Future.get()
Future<Integer> future = pool.submit(() -> 42);
System.out.println(future.get());
```

------

# 🔹九、线程池监控常用指标

可通过 ThreadPoolExecutor 方法查看：

✔ `getPoolSize()` 当前线程数
 ✔ `getActiveCount()` 正在执行任务线程数
 ✔ `getQueue().size()` 排队任务数
 ✔ `getCompletedTaskCount()` 已完成数量

------

# 🔹十、线程池执行完如何关闭？

```java
pool.shutdown(); // 正常关闭
pool.shutdownNow(); // 强制关闭
```

------

# 🔹十一、常见线程池组合（面试+项目最佳实践）

------

## 📌CPU 密集型任务

➡ 核心线程 = CPU 核心数 + 1

```java
Runtime.getRuntime().availableProcessors()
```

------

## 📌IO 密集型

➡ 核心线程 = CPU 数 * 2 或更多

因为 IO 等待时间较多，可以增加并发数。

------

------

# 🔥总结一句话

> **线程池是线程的调度中心，通过复用、排队、拒绝策略、最大并发限制来提升性能并避免系统崩溃，是高并发必备工具。**

