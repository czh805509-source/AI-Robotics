1️⃣ 什么是闭环控制？

闭环控制（Closed-loop Control）是指：

系统通过“反馈”不断修正自身输出，使结果更接近期望值的控制方式。

🔁 核心结构：
给定目标值（Setpoint）
实际输出（Measured value）
比较误差（Error）
控制器调整输出
反馈回系统
🤖 举例（机器人）

比如小车要走直线：

目标：速度 = 1 m/s
实际：0.8 m/s
误差：0.2 m/s
系统自动加速修正
🧠 一句话总结：

闭环控制 = 有反馈 + 自动纠错

⚙️ 2️⃣ PID控制器各部分作用

PID 控制器（PID controller）由三部分组成：

🔵 P（比例 Proportional）

👉 作用：

根据“当前误差”进行调整

公式思想：

误差越大，修正越强

✔ 优点：

反应快

❌ 缺点：

可能有稳态误差
🟡 I（积分 Integral）

👉 作用：

累积历史误差

✔ 解决问题：

消除“长期偏差”

❌ 缺点：

可能导致超调（过冲）
🔴 D（微分 Derivative）

👉 作用：

预测误差变化趋势

✔ 优点：

提前减速，减少震荡

❌ 缺点：

对噪声敏感
🧠 总结一句话：
P：现在错多少就改多少
I：过去错的也一起算
D：预测未来怎么变
💻 实践题
🚨 任务：遇到障碍物时执行

“三次后退 + 左转”

🤖 思路（ROS/机器人逻辑）

假设你检测到障碍物（如 /scan 或 /cmd_vel 判断）

🔁 控制逻辑
if 检测到障碍物:
    后退3次
    左转
🧾 ROS2 Python示例（常见写法）

（适用于 ROS 2）

import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

class ObstacleAvoid(Node):
    def __init__(self):
        super().__init__('obstacle_avoid')
        self.pub = self.create_publisher(Twist, '/cmd_vel', 10)
        self.back_count = 0
        self.obstacle = False

    def move_back(self):
        msg = Twist()
        msg.linear.x = -0.2
        self.pub.publish(msg)

    def turn_left(self):
        msg = Twist()
        msg.angular.z = 0.8
        self.pub.publish(msg)

    def stop(self):
        msg = Twist()
        self.pub.publish(msg)

    def control_loop(self):
        if self.obstacle and self.back_count < 3:
            self.move_back()
            self.back_count += 1

        elif self.back_count >= 3:
            self.turn_left()
            self.back_count = 0
            self.obstacle = False
🧠 核心逻辑解释
检测到障碍物 → 触发状态
连续后退3次（或3个周期）
然后执行左转
清除状态继续前进



![Uploading 6.18.png…]()

目标速度 → 控制器 → 电机 → 编码器反馈 → 控制器

特点：

有反馈
自动修正误差
控制精度高


PID公式：

u(t)=K
p
	​

e(t)+K
i
	​

∫e(t)dt+K
d
	​

dt
de(t)
	​
#!/usr/bin/env python3

import rclpy
from rclpy.node import Node

from sensor_msgs.msg import LaserScan
from geometry_msgs.msg import Twist

import time


class ObstacleAvoid(Node):

    def __init__(self):

        super().__init__('obstacle_avoid')

        self.publisher = self.create_publisher(
            Twist,
            '/cmd_vel',
            10
        )

        self.subscription = self.create_subscription(
            LaserScan,
            '/scan',
            self.scan_callback,
            10
        )

        self.safe_distance = 0.8

    def move_forward(self):

        msg = Twist()

        msg.linear.x = 0.2

        self.publisher.publish(msg)

    def move_backward(self):

        msg = Twist()

        msg.linear.x = -0.2

        self.publisher.publish(msg)

    def turn_left(self):

        msg = Twist()

        msg.angular.z = 1.0

        self.publisher.publish(msg)

    def stop_robot(self):

        msg = Twist()

        self.publisher.publish(msg)

    def scan_callback(self, msg):

        front_distance = min(msg.ranges)

        if front_distance < self.safe_distance:

            self.get_logger().info(
                'Obstacle detected!'
            )

            # 后退三次
            for i in range(3):

                self.move_backward()

                self.get_logger().info(
                    f'Backward {i+1}'
                )

                time.sleep(1)

                self.stop_robot()

            # 左转

            self.turn_left()

            self.get_logger().info(
                'Turn Left'
            )

            time.sleep(2)

            self.stop_robot()

        else:

            self.move_forward()


def main(args=None):

    rclpy.init(args=args)

    node = ObstacleAvoid()

    rclpy.spin(node)

    node.destroy_node()

    rclpy.shutdown()


if __name__ == '__main__':
    main()
