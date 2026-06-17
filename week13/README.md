# AI Assisted Quadruped Trot

## 项目简介

本项目完成课堂作业：

使用 AI 辅助编程工具对给定的机器狗步态程序进行调试和改进，
实现机器人在 PyBullet 仿真环境中的稳定向前小跑（Trot Gait）。

---
---

## 遇到的问题

### 问题1

机器人启动后立即摔倒。

原因：

四条腿采用同步运动，
支撑多边形消失。

解决：

采用 Trot 步态：

LF + RR

RF + LR

交替运动。

---

### 问题2

机器人上下振动严重。

原因：

步高过大。

解决：

降低抬腿高度。

---

### 问题3

机器人向前移动不稳定。

原因：

步幅过大。

解决：

减小 STEP_LENGTH。

---

## 最终结果

机器人能够持续稳定向前运动超过30秒，
无明显翻倒现象。

---

## 运行方法

pip install -r requirements.txt

python examples/quadruped_walk_fixed.py
STEP_LENGTH = 0.05
STEP_HEIGHT = 0.03
FREQ = 2.0

def foot_trajectory(t, phase):
    x = STEP_LENGTH * np.sin(FREQ * t + phase)

    z = STEP_HEIGHT * max(
        0,
        np.sin(FREQ * t + phase)
    )

    return x, z
    PHASES = [
    0,
    np.pi,
    np.pi,
    0
]
