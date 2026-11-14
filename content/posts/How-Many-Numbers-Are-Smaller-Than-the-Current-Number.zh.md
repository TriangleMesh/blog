+++
title = '有多少小于当前数字的数字'
date = 2025-08-26T18:13:35-07:00
# draft = true
aliases = ['/zh/leetcode/how-many-numbers-are-smaller-than-the-current-number/']
+++

## 题目描述

**LeetCode 题目：** [How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/description/)

## 输入与输出

### 输入

- `nums`：List[int]，整数数组。
- 约束：
  - 2 <= nums.length <= 500
  - 0 <= nums[i] <= 100

### 输出

- List[int]：对于输入数组中的每个元素，返回数组中比它小的数字的数量。
- 输出数组与输入数组的长度相同。

这是一道简单题，很适合在面试中展示多种不同复杂度的解法。

## 暴力解法

```python
class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        rst = []
        count = 0
        for n in nums:
            for m in nums:
                if m < n:
                    count += 1
            rst.append(count)
            count = 0
        return rst
```

### 复杂度

- 时间复杂度：`O(n²)`
- 空间复杂度：`O(n)`

## 优化：排序 + 哈希表

可以发现，暴力解法进行了大量重复计算：对每个元素，都重新统计一遍比它小的数字有多少。

为了避免重复计算，可以通过预处理，用空间换时间。关键在于：先将数组排序，每个元素的下标就能告诉我们有多少数字比它小。使用哈希表保存这种映射，就能以 O(1) 的时间查询。

**算法步骤：**

1. 对数组排序，确定各元素的位置。
2. 建立哈希表：数值 → 比它小的数字数量。
3. 遍历原数组，从哈希表中查出每个元素对应的数量。

```python
class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        sorted_nums = sorted(nums)
        d = {}
        for i, n in enumerate(sorted_nums):
            if n not in d:
                d[n] = i
        rst = []
        for n in nums:
            rst.append(d[n])
        return rst
```

### 复杂度

- 时间复杂度：`O(n log n)`
- 空间复杂度：`O(n)`

## 计数排序（桶方法）

当可以使用哈希表时，也应该考虑能否用**数组**替代，也就是桶或计数的方法。如果取值范围有限且事先已知，这种做法尤其有效。

**关键思路：** 由于 `0 <= nums[i] <= 100`，可以使用长度固定为 101 的数组代替哈希表。这种方法称为**计数排序**，因为我们会统计每个值出现的次数。

**算法步骤：**

1. 创建频次数组，统计每个数字出现的次数。
2. 将频次转换为累计数量，也就是前缀和。
3. 对每个元素，直接从数组中查询比它小的数字数量。

```python
class Solution:
    def smallerNumbersThanCurrent(self, nums: List[int]) -> List[int]:
        lst = [0] * 101
        rst = []
        for n in nums:
            lst[n] += 1
        for i, n in enumerate(lst):
            if i != 0:
                lst[i] += lst[i - 1] 
        for n in nums:
            rst.append(lst[n - 1] if n >= 1 else 0)
        return rst
```

### 复杂度

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`
