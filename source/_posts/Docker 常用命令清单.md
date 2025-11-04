---
title: Docker常用命令
date: 2025-10-30 10:00:00
# tags: [Java, Stream, 函数式编程]
# categories: [编程技术]
excerpt: Docker常用命令清单。
---



------

> 适用于 CentOS 7 / Ubuntu / Mac / Windows 等环境  
> Docker 版本：18.x ~ 27.x 通用

---

## 🚀 一、镜像管理（Images）

| 命令                                   | 说明                   |
| -------------------------------------- | ---------------------- |
| `docker images`                        | 查看本地镜像           |
| `docker search nginx`                  | 在 Docker Hub 搜索镜像 |
| `docker pull nginx`                    | 拉取镜像               |
| `docker pull nginx:1.25`               | 拉取指定版本镜像       |
| `docker rmi nginx`                     | 删除镜像               |
| `docker rmi -f <IMAGE_ID>`             | 强制删除镜像           |
| `docker image prune`                   | 删除未使用镜像         |
| `docker save -o nginx.tar nginx`       | 导出镜像到文件         |
| `docker load -i nginx.tar`             | 从文件导入镜像         |
| `docker tag nginx myrepo/nginx:latest` | 给镜像打标签           |

---

## 🧱 二、容器管理（Containers）

| 命令                                          | 说明                                |
| --------------------------------------------- | ----------------------------------- |
| `docker ps`                                   | 查看正在运行的容器                  |
| `docker ps -a`                                | 查看所有容器（包括停止的）          |
| `docker run -d --name web nginx`              | 运行容器（后台模式）                |
| `docker run -it --name test ubuntu /bin/bash` | 启动交互式容器                      |
| `docker exec -it web /bin/bash`               | 进入正在运行的容器                  |
| `docker stop web`                             | 停止容器                            |
| `docker start web`                            | 启动容器                            |
| `docker restart web`                          | 重启容器                            |
| `docker rm web`                               | 删除容器                            |
| `docker rm -f $(docker ps -aq)`               | 删除所有容器                        |
| `docker logs web`                             | 查看容器日志                        |
| `docker logs -f web`                          | 实时查看日志                        |
| `docker inspect web`                          | 查看容器详细信息                    |
| `docker top web`                              | 查看容器内运行的进程                |
| `docker stats`                                | 查看容器资源使用情况（CPU、内存等） |
| `docker rename old new`                       | 重命名容器                          |

---

## 🌐 三、网络管理（Networks）

| 命令                                  | 说明             |
| ------------------------------------- | ---------------- |
| `docker network ls`                   | 查看所有网络     |
| `docker network inspect bridge`       | 查看网络详情     |
| `docker network create mynet`         | 创建自定义网络   |
| `docker network rm mynet`             | 删除网络         |
| `docker network connect mynet web`    | 将容器连接到网络 |
| `docker network disconnect mynet web` | 将容器从网络断开 |
| `docker network prune`                | 删除未使用的网络 |

---

## 📦 四、数据卷管理（Volumes）

| 命令                                                       | 说明                 |
| ---------------------------------------------------------- | -------------------- |
| `docker volume ls`                                         | 查看所有数据卷       |
| `docker volume create mydata`                              | 创建数据卷           |
| `docker volume inspect mydata`                             | 查看数据卷详情       |
| `docker volume rm mydata`                                  | 删除数据卷           |
| `docker volume prune`                                      | 删除未使用数据卷     |
| `docker run -v mydata:/data nginx`                         | 运行容器并挂载数据卷 |
| `docker run -v /home/user/www:/usr/share/nginx/html nginx` | 挂载宿主机目录       |

---

## 🧰 五、构建与管理镜像（Build）

| 命令                                      | 说明                     |
| ----------------------------------------- | ------------------------ |
| `docker build -t myapp:1.0 .`             | 根据 Dockerfile 构建镜像 |
| `docker commit <CONTAINER_ID> myimage:v1` | 从容器生成新镜像         |
| `docker history myimage:v1`               | 查看镜像构建历史         |
| `docker image ls`                         | 查看所有镜像             |
| `docker image rm myimage:v1`              | 删除镜像                 |

---

## 🖧 六、Docker Compose（多容器编排）

| 命令                     | 说明                     |
| ------------------------ | ------------------------ |
| `docker-compose up -d`   | 启动所有服务（后台运行） |
| `docker-compose down`    | 停止并删除容器、网络     |
| `docker-compose ps`      | 查看 Compose 管理的容器  |
| `docker-compose logs -f` | 查看服务日志             |
| `docker-compose restart` | 重启服务                 |
| `docker-compose build`   | 重新构建镜像             |

---

## 🧹 七、系统与资源清理

| 命令                     | 说明                               |
| ------------------------ | ---------------------------------- |
| `docker system df`       | 查看磁盘占用                       |
| `docker system prune`    | 清理未使用资源                     |
| `docker system prune -a` | 清理所有未使用的镜像、容器、网络等 |
| `docker info`            | 查看 Docker 系统信息               |
| `docker version`         | 查看 Docker 版本信息               |

---

## 🔒 八、Docker 服务管理（CentOS 7）

| 命令                       | 说明             |
| -------------------------- | ---------------- |
| `systemctl start docker`   | 启动 Docker 服务 |
| `systemctl stop docker`    | 停止 Docker 服务 |
| `systemctl restart docker` | 重启 Docker      |
| `systemctl status docker`  | 查看 Docker 状态 |
| `systemctl enable docker`  | 设置开机自启     |
| `systemctl disable docker` | 取消开机自启     |

---

## 🧭 九、调试与其他命令

| 命令                                                | 说明                   |
| --------------------------------------------------- | ---------------------- |
| `docker inspect <ID>`                               | 查看详细 JSON 信息     |
| `docker exec -it <容器> bash`                       | 进入容器交互终端       |
| `docker cp web:/usr/share/nginx/html ./backup`      | 从容器拷贝文件到宿主机 |
| `docker cp ./index.html web:/usr/share/nginx/html/` | 从宿主机拷贝文件进容器 |
| `docker diff web`                                   | 查看容器文件系统的变动 |
| `docker events`                                     | 监听 Docker 实时事件   |
| `docker update --restart=always web`                | 设置容器自动重启策略   |

---

## 💡 十、快速清理命令合集

```bash
# 删除所有已退出容器
docker rm $(docker ps -aq -f status=exited)

# 删除所有悬空镜像
docker rmi $(docker images -q -f dangling=true)

# 删除未使用网络、卷、镜像、容器
docker system prune -a -f