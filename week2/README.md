1️⃣ “发布者（Publisher）”和“订阅者（Subscriber）”的区别

在 ROS2（ROS 2）通信机制中：

发布者（Publisher）
负责“发送数据”
会把消息发布到某个“话题（topic）”
不关心谁在接收
订阅者（Subscriber）
负责“接收数据”
订阅某个话题
一旦有消息发布就会自动收到

👉 简单理解：

发布者 = 发广播的人
订阅者 = 听广播的人

2️⃣ 同时控制两个机器人需要什么条件？

要同时控制两个机器人，需要：

✅ 关键条件：
不同的命名空间（namespace）
例如：
/turtle1/
/turtle2/
独立的话题或正确重映射（topic remap）
分别发布控制指令
每个机器人都有自己的 /cmd_vel

👉 核心一句话：

必须保证两个机器人“通信隔离”，否则会互相抢控制。

📐 计算题

已知：

线速度 v=1.5m/s
角速度 ω=0.75rad/s
1️⃣ 圆的半径

公式：

r=
ω
v
	​


代入：

r=
0.75
1.5
	​

=2m

✅ 答案：半径 = 2 m

2️⃣ 转一圈需要多少秒？

公式：

T=
ω
2π
	​


代入：

T=
0.75
2π
	​

≈8.38s

（或：T=
v
2πr
	​

 也一样）

✅ 答案：约 8.38 秒

💻 实践题（命令行画正方形）

通常用 turtlesim（小乌龟）实现。

思路：

“直走 + 转弯 90°”重复 4 次

🚀 方法：命令行发布 Twist
1️⃣ 先启动
ros2 run turtlesim turtlesim_node

再开一个终端：

ros2 run turtlesim turtle_teleop_key

（或直接用 topic 控制）

2️⃣ 画正方形核心命令
✔ 前进（直线）
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist \
"{linear: {x: 2.0}, angular: {z: 0.0}}"
✔ 停一下
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist \
"{linear: {x: 0.0}, angular: {z: 0.0}}"
✔ 原地转 90°
ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist \
"{linear: {x: 0.0}, angular: {z: 1.57}}"
🔁 正方形循环逻辑

重复 4 次：

前进
停
转 90°


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

