计算线速度 v

v=
2
v
r
	​

+v
l
	​

	​


v=
2
1.0+0.5
	​

v=0.75m/s

答案：

v = 0.75 m/s


小乌龟
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

        distance = math.sqrt(x * x + y * y)

        self.get_logger().info(
            f"当前位置: ({x:.2f}, {y:.2f})  距离原点: {distance:.2f} m"
        )

def main(args=None):

    rclpy.init(args=args)

    node = DistanceReader()

    rclpy.spin(node)

    node.destroy_node()

    rclpy.shutdown()

if __name__ == '__main__':
    main()
