1️⃣ 四足机器人：需要控制哪些关节？最少几个电机？

四足机器人（机器狗）每条腿通常有 3个自由度（DOF）：

髋关节：前后摆动（Hip pitch）
髋关节：左右摆动（Hip roll，部分结构有）
膝关节：伸缩（Knee pitch）
✅ 常见结构

一条腿：

最少：2个电机（简化模型）
标准：3个电机

四条腿：

最少控制电机数：
2 × 4 = 8个电机
标准情况：
3 × 4 = 12个电机
💡 核心理解

行走不是“腿动一下”，而是：

每个关节都在协调产生步态（gait）

🚁 2️⃣ 四旋翼无人机如何实现前后左右移动？

四旋翼无人机（quadrotor drone）靠的是：

改变四个螺旋桨的转速组合，而不是直接“推着走”

✈️ 三种基本姿态控制
① 前进/后退（Pitch）
前后电机转速不一样
机身“倾斜”
推力变成水平分量 → 向前飞
② 左右移动（Roll）
左右电机转速差
机身侧倾
产生横向速度
③ 上升/下降
四个电机一起加速或减速
只改变高度
⚠️ 和轮式机器人的本质区别
项目	无人机	轮式机器人
运动方式	空间力平衡	地面摩擦
控制方式	姿态 + 推力	轮速差
自由度	3D空间	2D平面
稳定性	天生不稳定	天生稳定
🤖 3️⃣ 机械臂末端精确到达某点，需要什么信息？

机械臂（robotic manipulator）要控制末端到达空间某点，需要：

✅ 必须信息
① 目标位置（Position）
x, y, z 坐标
② 目标姿态（Orientation）
roll / pitch / yaw
或四元数
③ 机械臂参数
连杆长度
关节结构
DH参数（Denavit-Hartenberg）
④ 当前关节状态
每个关节角度（θ）
💡 核心问题

本质是在解：

逆运动学（Inverse Kinematics）

🦿 4️⃣ 为什么双足机器人比轮式更难？

双足机器人（humanoid robot）难的原因主要有4点：

⚠️ ① 天生不稳定
只有两个支撑点
重心稍微偏移就会倒
⚠️ ② 动态平衡问题
不是“站稳”就行
需要不断调整：
重心
步频
支撑力
⚠️ ③ 控制维度高
多关节协同
腿 + 躯干 + 手臂都影响平衡
⚠️ ④ 必须实时反馈
IMU（加速度/陀螺仪）
力传感器
视觉系统
🚗 对比轮式机器人

轮式机器人：

永远有稳定支撑面（三角形/四边形）
重心不容易掉出支撑区
控制简单
🧾 一句话总结（考试可用）
四足机器人：靠多关节协调步态实现稳定行走
无人机：通过姿态倾斜把推力分解为水平运动
机械臂：依赖逆运动学求解末端位置与姿态
双足机器人：因动态不稳定和高维控制，难度远高于轮式系统


import pybullet as p
import pybullet_data
import time
import numpy as np
<img width="1448" height="1086" alt="6 18 1" src="https://github.com/user-attachments/assets/25c00584-283a-4db5-b4f2-a130dcdb99af" />

# 连接GUI
p.connect(p.GUI)

p.setAdditionalSearchPath(
    pybullet_data.getDataPath()
)

p.setGravity(0, 0, -9.8)

# 地面
p.loadURDF("plane.urdf")

# Panda机械臂
robot = p.loadURDF(
    "franka_panda/panda.urdf",
    [0,0,0],
    useFixedBase=True
)

# 摄像机
p.resetDebugVisualizerCamera(
    cameraDistance=1.5,
    cameraYaw=45,
    cameraPitch=-35,
    cameraTargetPosition=[0.5,0,0.2]
)

# 创建彩色方块
cube_ids = []

colors = [
    [1,0,0,1],
    [0,1,0,1],
    [0,0,1,1],
    [1,1,0,1]
]

for i in range(4):

    visual = p.createVisualShape(
        p.GEOM_BOX,
        halfExtents=[0.02]*3,
        rgbaColor=colors[i]
    )

    collision = p.createCollisionShape(
        p.GEOM_BOX,
        halfExtents=[0.02]*3
    )

    cube = p.createMultiBody(
        baseMass=1,
        baseCollisionShapeIndex=collision,
        baseVisualShapeIndex=visual,
        basePosition=[
            0.45,
            -0.1 + i*0.08,
            0.02
        ]
    )

    cube_ids.append(cube)

# 关节控制滑块
sliders = []

for i in range(7):

    slider = p.addUserDebugParameter(
        f"Joint {i+1}",
        -3.14,
        3.14,
        0
    )

    sliders.append(slider)

# 夹爪滑块
gripper = p.addUserDebugParameter(
    "Gripper",
    0,
    0.04,
    0.04
)

while True:

    # 控制机械臂
    for i in range(7):

        target = p.readUserDebugParameter(
            sliders[i]
        )

        p.setJointMotorControl2(
            robot,
            i,
            p.POSITION_CONTROL,
            targetPosition=target
        )

    # 控制夹爪
    grip = p.readUserDebugParameter(
        gripper
    )

    p.setJointMotorControl2(
        robot,
        9,
        p.POSITION_CONTROL,
        targetPosition=grip
    )

    p.setJointMotorControl2(
        robot,
        10,
        p.POSITION_CONTROL,
        targetPosition=grip
    )

    # 获取末端执行器状态
    state = p.getLinkState(
        robot,
        11
    )

    pos = state[0]

    p.addUserDebugText(
        f"X={pos[0]:.2f} "
        f"Y={pos[1]:.2f} "
        f"Z={pos[2]:.2f}",
        [0.3,-0.4,0.8],
        textSize=1,
        lifeTime=0.1
    )

    p.stepSimulation()

    time.sleep(1/240)<img width="1448" height="1086" alt="6 18 1" src="https://github.com/user-attachments/assets/99f9e9dd-cb98-43d3-b85e-3750cf9d3746" />

