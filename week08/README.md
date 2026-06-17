

![Uploading 屏幕截图 2026-04-22 131653 (1).png…]()
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/src
nano square.py
#!/usr/bin/env python3

import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
import time

class SquareMover(Node):

    def __init__(self):
        super().__init__('square_mover')
        self.publisher_ = self.create_publisher(
            Twist,
            '/turtle1/cmd_vel',
            10
        )

    def move_square(self):
        msg = Twist()

        for _ in range(4):

            msg.linear.x = 2.0
            msg.angular.z = 0.0
            self.publisher_.publish(msg)
            time.sleep(2)

            msg.linear.x = 0.0
            msg.angular.z = 1.57
            self.publisher_.publish(msg)
            time.sleep(1)

        msg.linear.x = 0.0
        msg.angular.z = 0.0
        self.publisher_.publish(msg)

def main(args=None):
    rclpy.init(args=args)
    node = SquareMover()
    node.move_square()
    node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
python3 square.py
