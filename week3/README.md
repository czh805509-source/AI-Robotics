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

