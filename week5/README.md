🧠 理论题
1️⃣ 列举3种外部传感器和3种内部传感器

在机器人系统中（ROS 2 生态里也常这么分类），传感器一般分为：

🌍 外部传感器（感知环境）

用于“看外界发生什么”：

① 激光雷达（LiDAR）
测距 + 建图
获取周围障碍物点云
② 摄像头（RGB / 深度相机）
图像识别
目标检测
视觉SLAM
③ 超声波传感器
短距离测距
防碰撞（简单避障）
⚙️ 内部传感器（感知自身状态）

用于“知道自己怎么动”：

① IMU（惯性测量单元）
加速度 + 陀螺仪
判断姿态（倾斜、旋转）
② 编码器（Encoder）
测量电机转角/速度
控制关节位置
③ 电流/力矩传感器
判断负载大小
检测是否卡住或碰撞
🧠 一句话总结

外部传感器看世界，内部传感器看自己

📡 2️⃣ 激光雷达 vs 超声波传感器区别
项目	激光雷达	超声波
原理	激光反射	声波反射
精度	高（厘米级甚至毫米级）	较低
测距范围	远（几米到上百米）	短（通常 < 5m）
速度	快	较慢
抗干扰	强	易受环境影响
成本	高	低
🚀 核心区别一句话：
激光雷达 = “高清3D扫描”
超声波 = “简易距离检测”
💻 实践题
🖥️ 1️⃣ 用 RViz 显示一个话题的数据

RViz（RViz）是 ROS2 中的可视化工具。

🚀 Step 1：启动 ROS2 环境
source /opt/ros/humble/setup.bash
🚀 Step 2：启动 RViz
rviz2
🚀 Step 3：添加显示话题

在 RViz 中：

点击：

👉 “Add” → 选择显示类型

常见选择：

LaserScan（激光雷达）
Image（摄像头）
TF（坐标系）
Pose（位姿）
然后设置 topic：

例如：

/scan

或：

/camera/image
🧠 关键点

只要：

Topic 正确 + Fixed Frame 设置正确（如 map / odom）

就能显示数据

💾 2️⃣ 保存 RViz 配置
方法一（推荐 GUI）

在 RViz 中：

👉 File → Save Config As

保存为：

my_rviz_config.rviz
方法二（命令行启动加载）

以后可以直接：

rviz2 -d my_rviz_config.rviz
🧠 保存的作用

保存内容包括：

显示的 topic
视角设置
Fixed Frame
颜色/坐标显示


激光雷达测距原理

激光发射后碰到障碍物反射回来。

测量往返时间：

d=
2
ct
	​


其中：

d：距离
c：光速
t：往返时间


#!/usr/bin/env python3

import rclpy
from rclpy.node import Node
from turtlesim.msg import Pose
import math

class DistanceReader(Node):

    def __init__(self):
        super().__init__('distance_reader')

        self.subscription = self.create_subscription(
            Pose,
            '/turtle1/pose',
            self.pose_callback,
            10
        )

    def pose_callback(self, msg):

        x = msg.x
        y = msg.y

        distance = math.sqrt(x**2 + y**2)

        self.get_logger().info(
            f"当前位置: x={x:.2f}, y={y:.2f}, 距离原点={distance:.2f} m"
        )

def main(args=None):

    rclpy.init(args=args)

    node = DistanceReader()

    rclpy.spin(node)

    node.destroy_node()

    rclpy.shutdown()

if __name__ == '__main__':
    main()
