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
