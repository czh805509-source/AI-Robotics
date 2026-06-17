


![Uploading 6.18.3.png…]()
1. 线性代数练习
（1）实现3D旋转矩阵
绕X轴旋转
R
x
	​

(θ)=
	​

1
0
0
	​

0
cosθ
sinθ
	​

0
−sinθ
cosθ
	​

	​

绕Y轴旋转
R
y
	​

(θ)=
	​

cosθ
0
−sinθ
	​

0
1
0
	​

sinθ
0
cosθ
	​

	​

绕Z轴旋转
R
z
	​

(θ)=
	​

cosθ
sinθ
0
	​

−sinθ
cosθ
0
	​

0
0
1
	​

	​

（2）验证旋转矩阵性质

旋转矩阵满足：

R
−1
=R
T

因此：

RR
T
=I

例如：

R
z
	​

=
	​

c
s
0
	​

−s
c
0
	​

0
0
1
	​

	​

R
z
	​

R
z
T
	​

=
	​

1
0
0
	​

0
1
0
	​

0
0
1
	​

	​

=I

验证成立。

（3）组合两个旋转变换

先绕Z轴旋转30°：

R
1
	​

=R
z
	​

(30
∘
)

再绕Y轴旋转45°：

R
2
	​

=R
y
	​

(45
∘
)

组合旋转：

R=R
2
	​

R
1
	​


注意：

矩阵乘法不可交换：

R
2
	​

R
1
	​


=R
1
	​

R
2
	​


因此旋转顺序会影响最终姿态。

2. 运动学练习
2.1 三自由度机械臂正运动学

设：

连杆长度：
L
1
	​

,L
2
	​

,L
3
	​

关节角：
θ
1
	​

,θ
2
	​

,θ
3
	​


末端位置：

x=L
1
	​

cosθ
1
	​

+L
2
	​

cos(θ
1
	​

+θ
2
	​

)+L
3
	​

cos(θ
1
	​

+θ
2
	​

+θ
3
	​

)
y=L
1
	​

sinθ
1
	​

+L
2
	​

sin(θ
1
	​

+θ
2
	​

)+L
3
	​

sin(θ
1
	​

+θ
2
	​

+θ
3
	​

)
2.2 三自由度机械臂逆运动学

已知目标点：

(x,y)

先求腕部中心：

x
w
	​

=x−L
3
	​

cosϕ
y
w
	​

=y−L
3
	​

sinϕ

其中：

ϕ=θ
1
	​

+θ
2
	​

+θ
3
	​


利用余弦定理：

D=
2L
1
	​

L
2
	​

x
w
2
	​

+y
w
2
	​

−L
1
2
	​

−L
2
2
	​

	​

θ
2
	​

=arctan2(±
1−D
2
	​

,D)

求：

θ
1
	​

=arctan2(y
w
	​

,x
w
	​

)−arctan2(L
2
	​

sinθ
2
	​

,L
1
	​

+L
2
	​

cosθ
2
	​

)

最后：

θ
3
	​

=ϕ−θ
1
	​

−θ
2
	​

2.3 工作空间

若：

L
1
	​

=2,L
2
	​

=2,L
3
	​

=1

最大伸展：

R
max
	​

=5

最小伸展：

R
min
	​

=∣L
1
	​

−L
2
	​

−L
3
	​

∣=1


卷积练习
（1）手动实现3×3卷积

公式：

g(i,j)=
m=−1
∑
1
	​

n=−1
∑
1
	​

f(i+m,j+n)k(m,n)

其中：

f：原图
k：卷积核
（2）不同卷积核效果
均值滤波
9
1
	​

	​

1
1
1
	​

1
1
1
	​

1
1
1
	​

	​


效果：

模糊
去噪
锐化
	​

0
−1
0
	​

−1
5
−1
	​

0
−1
0
	​

	​


效果：

边缘增强
Sobel-X
	​

−1
−2
−1
	​

0
0
0
	​

1
2
1
	​

	​


效果：

检测垂直边缘
Sobel-Y
	​

−1
0
1
	​

−2
0
2
	​

−1
0
1
	​

	​


效果：

检测水平边缘
（3）与OpenCV结果比较

理论上：

ManualConv=cv2.filter2D()

结果会有微小差异：

原因：

边界填充方式不同
数据类型转换不同
OpenCV进行了优化
4. 路径规划思考题
（1）BFS扩展过程

起点：

(0,0)
第0层
(0,0)
第1层
(1,0)
(0,1)
第2层
(2,0)
(1,1)
(0,2)
第3层
(3,0)
(2,1)
(1,2)
(0,3)

扩展呈波纹状：

S
○ ○
○ ○ ○
○ ○ ○ ○
（2）A*扩展过程

启发函数：

h=∣x
g
	​

−x∣+∣y
g
	​

−y∣

即曼哈顿距离。

A*优先选择：

离目标更近

的节点。

扩展大致沿对角线：

S
 \
  \
   \
    G

不会像BFS那样向四周均匀扩散。

（3）节点数量比较

BFS：

扩展大量无关节点

例如：

80~90个

A*：

只搜索目标方向：

20~30个

因此：

A
∗

效率明显高于：

BFS
（4）不可采纳启发式为什么不最优

若：

h(n)>h
∗
(n)

即：

高估真实代价。

A*可能认为：

真正最短路径代价很大

从而提前放弃。

结果：

找到一条较长路径就停止。

因此：

不能保证最优解。

（5）RRT 与 A* 适用场景
RRT适合

高维空间：

机械臂
无人机
自动驾驶

特点：

连续空间
自由度高
障碍复杂
A*适合

二维网格：

地图导航
游戏寻路

特点：

离散空间
地图已知
5. 李群思考题
（1）为什么Pitch=90°出现万向锁

欧拉角：

Roll-Pitch-Yaw

当：

Pitch=90
∘

时：

Yaw轴与Roll轴重合。

原本：

3个自由度

变成：

2个自由度

导致：

一个旋转方向丢失

这就是万向锁。

（2）三种表示方式适用场景
旋转矩阵

优点：

计算方便
可直接变换坐标

缺点：

9个参数冗余

适合：

坐标变换
机器人计算
四元数

优点：

无万向锁
存储量小

缺点：

不直观

适合：

姿态存储
SLAM
游戏引擎
so(3)向量

优点：

最小参数化
优化方便

适合：

非线性优化
视觉SLAM
BA优化


pip install numpy matplotlib opencv-python
python robotics_homework.py
import numpy as np
import matplotlib.pyplot as plt
import cv2

# =====================================
# 1. 线性代数练习
# =====================================

def Rx(theta):
    return np.array([
        [1, 0, 0],
        [0, np.cos(theta), -np.sin(theta)],
        [0, np.sin(theta), np.cos(theta)]
    ])

def Ry(theta):
    return np.array([
        [np.cos(theta), 0, np.sin(theta)],
        [0, 1, 0],
        [-np.sin(theta), 0, np.cos(theta)]
    ])

def Rz(theta):
    return np.array([
        [np.cos(theta), -np.sin(theta), 0],
        [np.sin(theta), np.cos(theta), 0],
        [0, 0, 1]
    ])

theta = np.deg2rad(30)

R = Rz(theta)

print("R =")
print(R)

print("\nR * R^T =")
print(R @ R.T)

print("\n是否接近单位矩阵:")
print(np.allclose(R @ R.T, np.eye(3)))

print("\n组合旋转:")

R1 = Rz(np.deg2rad(30))
R2 = Ry(np.deg2rad(45))

R_combined = R2 @ R1

print(R_combined)

# =====================================
# 2. 三自由度机械臂
# =====================================

L1 = 2
L2 = 2
L3 = 1

def forward_kinematics(t1, t2, t3):

    x = (
        L1*np.cos(t1)
        + L2*np.cos(t1+t2)
        + L3*np.cos(t1+t2+t3)
    )

    y = (
        L1*np.sin(t1)
        + L2*np.sin(t1+t2)
        + L3*np.sin(t1+t2+t3)
    )

    return x, y

print("\n正运动学测试")

x, y = forward_kinematics(
    np.deg2rad(30),
    np.deg2rad(45),
    np.deg2rad(20)
)

print("末端位置:")
print(x, y)

# =====================================
# 逆运动学
# =====================================

def inverse_kinematics(x, y, phi):

    xw = x - L3*np.cos(phi)
    yw = y - L3*np.sin(phi)

    D = (
        xw**2 + yw**2
        - L1**2 - L2**2
    ) / (2*L1*L2)

    if abs(D) > 1:
        return None

    t2 = np.arctan2(
        np.sqrt(1-D**2),
        D
    )

    t1 = np.arctan2(yw, xw) - np.arctan2(
        L2*np.sin(t2),
        L1 + L2*np.cos(t2)
    )

    t3 = phi - t1 - t2

    return t1, t2, t3

print("\n逆运动学测试")

angles = inverse_kinematics(
    3.0,
    2.0,
    np.deg2rad(45)
)

print(angles)

# =====================================
# 工作空间绘制
# =====================================

xs = []
ys = []

for t1 in np.linspace(-np.pi, np.pi, 80):
    for t2 in np.linspace(-np.pi, np.pi, 80):
        for t3 in np.linspace(-np.pi, np.pi, 20):

            x, y = forward_kinematics(
                t1, t2, t3
            )

            xs.append(x)
            ys.append(y)

plt.figure(figsize=(6,6))
plt.scatter(xs, ys, s=1)
plt.title("3DOF Workspace")
plt.axis("equal")
plt.show()

# =====================================
# 3. 卷积练习
# =====================================

img = np.zeros((200,200), dtype=np.uint8)

cv2.rectangle(
    img,
    (50,50),
    (150,150),
    255,
    -1
)

kernel = np.array([
    [-1,0,1],
    [-2,0,2],
    [-1,0,1]
])

def manual_conv(image, kernel):

    h, w = image.shape

    output = np.zeros_like(image)

    for i in range(1,h-1):
        for j in range(1,w-1):

            region = image[
                i-1:i+2,
                j-1:j+2
            ]

            value = np.sum(
                region * kernel
            )

            output[i,j] = np.clip(
                abs(value),
                0,
                255
            )

    return output

manual_result = manual_conv(
    img,
    kernel
)

opencv_result = cv2.filter2D(
    img,
    -1,
    kernel
)

plt.figure(figsize=(12,4))

plt.subplot(131)
plt.imshow(img, cmap='gray')
plt.title("Original")

plt.subplot(132)
plt.imshow(manual_result, cmap='gray')
plt.title("Manual Conv")

plt.subplot(133)
plt.imshow(opencv_result, cmap='gray')
plt.title("OpenCV Conv")

plt.show()

print("\n卷积实验完成")
