---
title: Gemini CLI无法登录的问题
date: 2025-11-03 19:35:00
# tags: [Java, Stream, 函数式编程]
# categories: [编程技术]
excerpt: Gemini CLI无法登录的问题。
---



------


![image-20251103195828642](https://ye-hai.oss-cn-shenzhen.aliyuncs.com/typora/image-20251103195828642.png)

以下是我成功登入的过程：

初步检查

我已开启 Clash 的 TUN 模式，并配置了相关环境变量。

![image-20251103195843175](https://ye-hai.oss-cn-shenzhen.aliyuncs.com/typora/image-20251103195843175.png)

为了验证代理是否生效，我在管理员权限的 PowerShell 中执行 netsh winhttp show proxy，发现输出仍为 直接访问（没有代理服务器），说明系统代理未正确设置。
手动配置代理

于是，我通过命令 netsh winhttp set proxy 127.0.0.1:7890 手动设置了 WinHTTP 代理。
再次执行 netsh winhttp show proxy，确认代理已生效：
代理服务器: 127.0.0.1:7890
绕过列表: (无)
结果

完成代理配置后，重新启动 Gemini，问题解决，服务成功运行。