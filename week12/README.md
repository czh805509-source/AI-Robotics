WSL + 手机视频流 + ArUco 识别实验
实验目标

本次实验完成以下内容：

在 WSL 中安装并配置 Tailscale
使用手机浏览器访问 HTML5 相机页面
将手机摄像头视频流传输至 WSL
使用 OpenCV 实现 ArUco Marker 实时识别
（选做）完成相机标定并测量 ArUco 距离
实验环境
项目	版本
Windows	Windows 11
WSL	Ubuntu 24.04
Python	3.12
OpenCV	4.x
Tailscale	最新版
一、安装 Tailscale

安装：

curl -fsSL https://tailscale.com/install.sh | sh

启动：

sudo tailscale up

登录成功后查看 IP：

tailscale ip -4
二、手机视频流接入

启动 HTML5 摄像头页面：

python3 -m http.server 8000

手机浏览器访问：

http://WSL_IP:8000

允许摄像头权限后开始推流。

三、OpenCV 接收视频流

安装依赖：

pip install opencv-python opencv-contrib-python

测试摄像头：

import cv2

cap = cv2.VideoCapture("视频流地址")

while True:
    ret, frame = cap.read()

    if not ret:
        break

    cv2.imshow("Camera", frame)

    if cv2.waitKey(1) == 27:
        break

cap.release()
cv2.destroyAllWindows()
四、ArUco 实时识别

代码核心：

import cv2
import cv2.aruco as aruco

dictionary = aruco.getPredefinedDictionary(
    aruco.DICT_4X4_50
)

detector = aruco.ArucoDetector(dictionary)

corners, ids, rejected = detector.detectMarkers(frame)

识别结果：

成功检测到 Marker
正确显示 Marker ID
实现实时跟踪
五、实验结果
1. 手机视频流进入 WSL






2. ArUco 实时识别






3. Marker ID 显示

<img width="1672" height="941" alt="6 18 4" src="https://github.com/user-attachments/assets/5474eb42-00e8-46e7-aba0-1e3a49c48a62" />






选做内容
相机标定

生成：

camera_calib.npz

保存内容：

cameraMatrix
distCoeffs
ArUco 距离测量

利用：

cv2.solvePnP()

计算 Marker 与摄像头距离：

Distance = 0.52 m

并实时显示在视频画面中。

实验总结

本实验成功完成 Tailscale 网络连接、手机视频流接入 WSL、OpenCV 图像处理以及 ArUco Marker 实时识别。通过实验掌握了跨设备视频传输、计算机视觉基础以及机器人视觉定位的基本方法，为后续 ROS2 视觉导航实验奠定基础。
