<img width="523" height="534" alt="image" src="https://github.com/user-attachments/assets/258cc174-f789-4823-9dc7-c9f07664c0e0" />
| 序号 | 实验内容  | 命令                                                                                                  |
| -- | ----- | --------------------------------------------------------------------------------------------------- |
| 1  | 查看节点  | `ros2 node list`                                                                                    |
| 2  | 查看话题  | `ros2 topic list`                                                                                   |
| 3  | 监听话题  | `ros2 topic echo /turtle1/pose`                                                                     |
| 4  | 小乌龟前进 | `ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear:{x:2.0},angular:{z:0.0}}"` |
| 5  | 画圆    | `ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear:{x:1.5},angular:{z:0.75}}"`       |
| 6  | 顺时针画圆 | `ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear:{x:1.5},angular:{z:-0.75}}"`      |
| 7  | 原地旋转  | `ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear:{x:0.0},angular:{z:1.5}}"`        |
| 8  | 停止    | `ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear:{x:0.0},angular:{z:0.0}}"` |

