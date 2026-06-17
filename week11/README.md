掌握 Docker 镜像保存和管理

学习 Docker 镜像（Image）与容器（Container）的区别
掌握镜像导出与导入
docker save -o ros2_humble.tar ros:humble
docker load -i ros2_humble.tar
学会查看、删除和管理镜像
docker images
docker rmi 镜像ID

✅ 完成 PyBullet 和 OpenCV 环境配置

安装：

pip install pybullet
pip install opencv-python opencv-contrib-python

测试成功运行：

import pybullet as p
import cv2

print("PyBullet OK")
print("OpenCV OK")

掌握：

OpenCV 图像读取与显示
灰度化处理
PyBullet 机器人仿真环境启动


将 GitHub 作业仓库转换为网页

通过 GitHub Pages 完成部署：

Repository
   ↓
Settings
   ↓
Pages
   ↓
Deploy from branch
   ↓
main / root

网页地址格式：

https://用户名.github.io/仓库名/

实现：

README 自动展示
图片在线访问
Markdown 页面浏览
