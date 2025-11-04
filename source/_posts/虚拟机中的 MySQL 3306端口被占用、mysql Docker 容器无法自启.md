---
title: 虚拟机中的 MySQL 3306端口被占用、mysql Docker 容器无法自启
date: 2025-10-21 10:00:00
# tags: [Java, Stream, 函数式编程]
# categories: [编程技术]
excerpt: 虚拟机中的 MySQL 3306端口被占用、mysql Docker 容器无法自启的解决办法。
---



------

## **问题背景**

- 虚拟机上手动安装的 MySQL (`/usr/local/mysql/bin/mysqld`) 占用 3306
- kill PID 无效（可以先试试杀死占用3306端口的进程），因为有守护进程 `mysqld_safe` 不断重启
- 系统没有 systemd mysqld 服务
- Docker 容器已存在但未启动，需要端口干净并设置开机自启

------

## **完整解决步骤**

### **步骤 1：排查占用端口**

```bash
sudo lsof -i:3306
ps -fp <PID>（在这之前可以先试试杀死占用3306端口的进程）
```

- 找到 mysqld 进程和父进程（守护进程）
- 确认路径，例如 `/usr/local/mysql/bin/mysqld`

------

### **步骤 2：停止所有虚拟机自带 MySQL 进程**

```bash
sudo pkill -f mysqld_safe
sudo pkill -f /usr/local/mysql/bin/mysqld
```

- 确认端口已释放：

```bash
sudo lsof -i:3306
```

- 输出为空表示干净

------

### **步骤 3：禁止虚拟机自带旧 MySQL 开机启动**

1. 禁用 init 脚本：

```bash
sudo chkconfig mysql off
```

1. 检查 rc.local 或 cron：

```bash
sudo cat /etc/rc.local
crontab -l
sudo crontab -l
```

- 注释掉任何启动 MySQL 的命令
- cron 任务如有也需删除

------

### **步骤 4：设置 Docker 容器开机自启**

1. 对已有容器设置自启：

```bash
docker update --restart unless-stopped mysql
```

1. 启动容器：

```bash
docker start mysql
```

- 这样 mysql容器 会占用 3306，并且容器会随系统重启自动启动

------

### **步骤 5：确保 Docker 服务开机启动**

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

- Docker 服务必须开机启动，容器自启才有效

------

### **步骤 6：验证**

1. 查看容器状态：

```bash
docker ps
```

- 确认容器状态是 `Up`

1. 重启虚拟机：

```bash
sudo reboot
```

1. 再次查看容器：

```bash
docker ps
```

- 如果容器 `Up` → 开机自启成功

------

## **关键总结**

1. 手动安装 MySQL 通过 `mysqld_safe` 守护进程不断重启 → 单 kill 无效
2. `/etc/init.d/mysql` 脚本可能开机启动旧 MySQL → `chkconfig mysql off` 禁用
3. 彻底释放端口后，再启动 Docker MySQL
4. Docker 容器使用 `--restart unless-stopped` + Docker 服务开机自启 → 容器随系统重启自动启动

------

