+++
title = 'Robot Bounded in Circle'
date = 2025-08-23T21:04:27-07:00
+++

## Problem Description

**LeetCode Problem:** [Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle/description/)

## Input/Output

### Input - `instructions: str`
a string of instructions, which will be repeated forever.
- `1 <= instructions.length <= 100`
- `instructions[i]` is `'G'`, `'L'` or `'R'`

For each instruction:
- `'G'`: moves the robot 1 step forward by adding the current direction to its position
- `'L'` and `'R'`: rotate the direction vector 90 degrees

### Output - `True/False`
- **True:** The robot is bounded in a circle (will return to origin eventually)
- **False:** The robot will move infinitely far from the origin

To determine if the robot is bounded, we check after one execution of the instructions:

**True (Bounded):**
- If the robot returns to origin `(0,0)`
- If the robot's direction has changed from the initial direction
  - The robot can only face 4 directions, so if direction changes after one cycle, it will form a closed shape within 4 cycles

**False (Unbounded):**
- If the robot is not at origin AND still facing the same direction, it will move infinitely in that direction

**Summary:**
- **True:** `final_pos == (0, 0)` or `final_dir != initial_dir`
- **False:** `final_pos != (0, 0)` and `final_dir == initial_dir`

## Analysis

To determine True/False, we need to track:
1. The robot's position
2. Its direction

in 2D space

**Direction options:** 
To represent direction, we have two options: 
1. Update the angle θ and use (cosθ,sinθ)
2. Simply choose from four unit vectors: (0,1), (1,0), (0,-1), (-1,0)

Here we choose the second option to solve it:

## Solution

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

## Complexity

- **Time:** O(n) - iterate through each instruction once
- **Space:** O(1) - only use fixed variables for position and direction

## Generalization

This approach extends to higher dimensions with discrete rotations and variable step sizes:

**Core Principle (Any Dimension):**
- **Bounded condition:** `final_position == origin` OR `final_direction != initial_direction`
- **Rotation constraints:**
  - **2D:** Simple angles (90°, 60°, 45°) - rational multiples of 2π
  - **3D:** More complex - rotations around different axes, Euler angles
  - **nD:** Even more complex - rotations in multiple planes simultaneously
  - **Key requirement:** Rotations must form a finite group (discrete, not continuous)

**Higher Dimensions:**
- **Direction representation:**
  - 2D: `[dx, dy]` unit vector
  - 3D: `[dx, dy, dz]` unit vector  
  - nD: `[d1, d2, ..., dn]` where `d1² + d2² + ... + dn² = 1`
- **Rotation:** n×n orthogonal matrices
  - In our 2D solution, although we use simple pattern-based coordinate transformations to find the transformed (x,y), this is fundamentally a rotation problem around the origin that can be explained using linear algebra:
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
- **Same logic:** If direction changes after one cycle, robot forms closed path in finite steps

**Variable Step Size:**
- Replace `pos += direction * 1` with `pos += direction * step_length`

**3D Extension - Variable Step Lengths with X/Y/Z-Axis Rotations:**
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

**Key Insight:** In any dimension with discrete rotations, the fundamental bounded condition remains the same regardless of step size.
