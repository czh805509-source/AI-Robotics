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
