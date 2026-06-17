1. 什么是 Docker？

Docker 是一个开源的容器化（Containerization）平台，用于将应用程序及其运行环境打包到一个独立的容器中，实现：

一次构建，到处运行
环境一致性
快速部署
资源利用率高
易于扩展和维护

简单理解：

Docker 就像一个轻量级的虚拟机，但它不需要安装完整操作系统。

2. Docker 与虚拟机区别
虚拟机（VM）
硬件
 ├── Host OS
      ├── Hypervisor
           ├── Guest OS
           ├── App

特点：

每个虚拟机都有完整操作系统
占用资源较大
启动较慢
Docker 容器
硬件
 ├── Host OS
      ├── Docker Engine
           ├── Container A
           ├── Container B
           ├── Container C

特点：

共用宿主机内核
体积小
启动快（秒级）
3. Docker 核心概念
（1）镜像 Image

镜像是容器运行的模板。

类似：

安装光盘
系统快照

例如：

ubuntu:22.04
python:3.10
ros:humble

查看镜像：

docker images
（2）容器 Container

容器是镜像运行后的实例。

关系：

镜像(Image)
      ↓
运行
      ↓
容器(Container)

例如：

docker run ubuntu

会生成一个 Ubuntu 容器。

查看容器：

docker ps

查看全部：

docker ps -a
（3）仓库 Repository

用于存放镜像。

最常用：

Docker Hub

例如：

docker pull ubuntu

实际上就是从 Docker Hub 下载镜像。

4. Docker 工作流程
Dockerfile
     ↓
docker build
     ↓
Image
     ↓
docker run
     ↓
Container
5. 常用命令
下载镜像
docker pull ubuntu
查看镜像
docker images
创建并运行容器
docker run ubuntu

交互模式：

docker run -it ubuntu bash
查看运行容器
docker ps
停止容器
docker stop 容器ID
删除容器
docker rm 容器ID
删除镜像
docker rmi 镜像ID
6. 数据卷 Volume

容器删除后数据默认丢失。

Volume 用于持久化存储：

docker run -v /home/data:/data ubuntu

映射关系：

主机目录
/home/data

↓ 映射

容器目录
/data
7. 端口映射

让外部访问容器服务。

例如：

docker run -p 8080:80 nginx

表示：

主机:8080
      ↓
容器:80

浏览器访问：

http://localhost:8080
8. Dockerfile

用于自动构建镜像。

示例：

FROM ubuntu:22.04

RUN apt update

RUN apt install -y python3

COPY . /app

WORKDIR /app

CMD ["python3","main.py"]

构建：

docker build -t myapp .

运行：

docker run myapp
9. Docker 在 ROS2 中的应用

以 ROS2 Humble 为例：

拉取官方镜像：

docker pull ros:humble

启动容器：

docker run -it ros:humble

查看 ROS2：

source /opt/ros/humble/setup.bash

ros2 --version

运行小乌龟：

sudo docker run -it \
--net=host \
--env DISPLAY=$DISPLAY \
-v /tmp/.X11-unix:/tmp/.X11-unix \
osrf/ros:humble-desktop

启动：

ros2 run turtlesim turtlesim_node
10. Docker 生命周期
pull
 ↓
Image
 ↓
run
 ↓
Container
 ↓
stop
 ↓
start
 ↓
rm
重点记忆
概念	作用
Image	镜像模板
Container	运行实例
Docker Hub	镜像仓库
Volume	数据持久化
Port Mapping	端口映射
Dockerfile	自动构建镜像
Build	构建镜像
Run	运行容器
一句话总结

Docker = 镜像（Image） + 容器（Container） + 仓库（Repository）

对于 ROS2 开发，最常用的命令通常只有：

docker pull ros:humble

docker run -it ros:humble bash

docker ps

docker stop 容器ID

docker exec -it 容器ID bash
