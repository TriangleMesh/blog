+++
title = '困于环中的机器人'
date = 2025-08-23T21:04:27-07:00
+++

## 题目描述

**LeetCode 题目：** [Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle/description/)

## 输入与输出

### 输入：`instructions: str`

一个会被无限重复执行的指令字符串。

- `1 <= instructions.length <= 100`
- `instructions[i]` 为 `'G'`、`'L'` 或 `'R'`。

对于每条指令：

- `'G'`：将当前位置加上当前方向向量，使机器人前进一步。
- `'L'` 和 `'R'`：将方向向量旋转 90 度。

### 输出：`True/False`

- **True：** 机器人被限制在一个圆内，最终会回到原点。
- **False：** 机器人会离原点无限远。

判断机器人是否有界时，只需检查执行一轮指令之后的状态：

**True（有界）：**

- `final_pos == (0, 0)`

或者：

- `final_dir != initial_dir`
  - 机器人只能朝向四个方向，因此只要一轮之后方向发生变化，就会在四轮之内形成闭合路径。

**False（无界）：**

- `final_pos != (0, 0)` 且 `final_dir == initial_dir`

## 分析

为了判断 True/False，需要在二维空间中追踪：

1. 机器人的位置。
2. 机器人的方向。

**方向的表示：**

有两种选择：

1. 更新角度 θ，再使用 (cosθ,sinθ)。
2. 直接从四个单位向量中选择：(0,1)、(1,0)、(0,-1)、(-1,0)。

这里选择第二种方式求解。

## 解法

```python
class Solution:
    def isRobotBounded(self, instructions: str) -> bool:
        direction = [0, 1]
        pos = [0, 0]
        for c in instructions:
            if c == 'L':
                # [0, 1] -> [-1, 0] -> [0, -1] -> [1, 0] -> [0, 1]
                x, y = direction
                direction = [-y, x]
            if c == 'R':
                # [0, 1] -> [1, 0] -> [0, -1] -> [-1, 0]
                x, y = direction
                direction = [y, -x]
            if c == 'G':
                pos[0] += direction[0]
                pos[1] += direction[1]
        return direction != [0, 1] or pos == [0, 0]
```

## 复杂度

- **时间：** O(n)，每条指令遍历一次。
- **空间：** O(1)，仅使用固定数量的变量保存位置和方向。

## 推广

这种方法可以推广到具有离散旋转和可变步长的更高维空间。

**核心原理（任意维度）：**

- **有界条件：** `final_position == origin` 或 `final_direction != initial_direction`。
- **旋转约束：**
  - **二维：** 简单角度，例如 90°、60°、45°，即 2π 的有理数倍。
  - **三维：** 更复杂，涉及绕不同轴旋转和欧拉角。
  - **n 维：** 更复杂，可能同时在多个平面旋转。
  - **关键要求：** 旋转必须构成有限群，也就是离散而非连续的旋转。

**更高维度：**

- **方向表示：**
  - 二维：单位向量 `[dx, dy]`。
  - 三维：单位向量 `[dx, dy, dz]`。
  - n 维：`[d1, d2, ..., dn]`，满足 `d1² + d2² + ... + dn² = 1`。
- **旋转：** n×n 正交矩阵。
  - 在二维解法中，虽然使用了简单的坐标变换规律来求新的 (x,y)，但本质上这是绕原点旋转的问题，可以用线性代数解释：
    ```
    # General rotation matrix for angle θ (counterclockwise):
    ⎡cos(θ)  -sin(θ)⎤ ⎡x⎤   ⎡x·cos(θ) - y·sin(θ)⎤
    ⎣sin(θ)   cos(θ)⎦ ⎣y⎦ = ⎣x·sin(θ) + y·cos(θ)⎦
    
    # For θ = 90° (Left turn): cos(90°)=0, sin(90°)=1
    ⎡0  -1⎤ ⎡x⎤   ⎡-y⎤
    ⎣1   0⎦ ⎣y⎦ = ⎣ x⎦
    
    # For θ = -90° (Right turn): cos(-90°)=0, sin(-90°)=-1  
    ⎡ 0   1⎤ ⎡x⎤   ⎡ y⎤
    ⎣-1   0⎦ ⎣y⎦ = ⎣-x⎦
    ```
- **相同逻辑：** 如果一轮之后方向发生变化，机器人会在有限步内形成闭合路径。

**可变步长：**

- 将 `pos += direction * 1` 替换为 `pos += direction * step_length`。

**三维扩展：可变步长与绕 X/Y/Z 轴旋转：**

```python
import math

def isRobotBounded3D(instructions: str) -> bool:
    # Start facing +Y direction
    direction = [0, 1, 0]
    pos = [0, 0, 0]
    
    def rotate_around_z(vec, angle_deg):
        """
        Rotate vector around z-axis using 3D rotation matrix:
        ⎡cos(θ)  -sin(θ)   0⎤ ⎡x⎤   ⎡x*cos(θ) - y*sin(θ)⎤
        ⎢sin(θ)   cos(θ)   0⎥ ⎢y⎥ = ⎢x*sin(θ) + y*cos(θ)⎥
        ⎣  0        0      1⎦ ⎣z⎦   ⎣        z           ⎦
        
        Z-axis rotation only affects x,y coordinates, z remains unchanged
        """
        angle = math.radians(angle_deg)  # Convert degrees to radians
        cos_a, sin_a = math.cos(angle), math.sin(angle)
        return [
            vec[0] * cos_a - vec[1] * sin_a,  # new_x = x*cos(θ) - y*sin(θ)
            vec[0] * sin_a + vec[1] * cos_a,  # new_y = x*sin(θ) + y*cos(θ)
            vec[2]                            # new_z = z (unchanged)
        ]
    
    def rotate_around_x(vec, angle_deg):
        """
        Rotate vector around x-axis using 3D rotation matrix:
        ⎡1    0       0   ⎤ ⎡x⎤   ⎡        x          ⎤
        ⎢0  cos(θ) -sin(θ)⎥ ⎢y⎥ = ⎢y*cos(θ) - z*sin(θ)⎥
        ⎣0  sin(θ)  cos(θ)⎦ ⎣z⎦   ⎣y*sin(θ) + z*cos(θ)⎦
        
        X-axis rotation only affects y,z coordinates, x remains unchanged
        """
        angle = math.radians(angle_deg)  # Convert degrees to radians
        cos_a, sin_a = math.cos(angle), math.sin(angle)
        return [
            vec[0],                            # new_x = x (unchanged)
            vec[1] * cos_a - vec[2] * sin_a,   # new_y = y*cos(θ) - z*sin(θ)
            vec[1] * sin_a + vec[2] * cos_a    # new_z = y*sin(θ) + z*cos(θ)
        ]
    
    def rotate_around_y(vec, angle_deg):
        """
        Rotate vector around y-axis using 3D rotation matrix:
        ⎡ cos(θ)  0  sin(θ)⎤ ⎡x⎤   ⎡x*cos(θ) + z*sin(θ) ⎤
        ⎢   0     1    0   ⎥ ⎢y⎥ = ⎢        y           ⎥
        ⎣-sin(θ)  0  cos(θ)⎦ ⎣z⎦   ⎣-x*sin(θ) + z*cos(θ)⎦
        
        Y-axis rotation only affects x,z coordinates, y remains unchanged
        """
        angle = math.radians(angle_deg)  # Convert degrees to radians
        cos_a, sin_a = math.cos(angle), math.sin(angle)
        return [
            vec[0] * cos_a + vec[2] * sin_a,   # new_x = x*cos(θ) + z*sin(θ)
            vec[1],                            # new_y = y (unchanged)
            -vec[0] * sin_a + vec[2] * cos_a   # new_z = -x*sin(θ) + z*cos(θ)
        ]
    
    for c in instructions:
        if c == 'L':  # Yaw left 90° (around z-axis)
            direction = rotate_around_z(direction, 90)
        elif c == 'R':  # Yaw right 90° (around z-axis)
            direction = rotate_around_z(direction, -90)
        elif c == 'U':  # Pitch up 90° (around x-axis)
            direction = rotate_around_x(direction, 90)
        elif c == 'D':  # Pitch down 90° (around x-axis)
            direction = rotate_around_x(direction, -90)
        elif c == 'T':  # Roll left 90° (around y-axis)
            direction = rotate_around_y(direction, 90)
        elif c == 'S':  # Roll right 90° (around y-axis)
            direction = rotate_around_y(direction, -90)
        elif c.isdigit():  # Numbers represent step length
            step_length = int(c)
            for i in range(3):
                pos[i] += direction[i] * step_length
    
    initial_direction = [0, 1, 0]
    direction_changed = any(abs(direction[i] - initial_direction[i]) > 0.001 for i in range(3))
    at_origin = all(abs(pos[i]) < 0.001 for i in range(3))
    
    return direction_changed or at_origin

# Example: "L2UT3DS1" means Yaw left, move 2, pitch Up, roll left, move 3, pitch Down, roll right, move 1
```

**关键结论：** 在具有离散旋转的任意维度中，无论步长如何变化，基本的有界条件都相同。
