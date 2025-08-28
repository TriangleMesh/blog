+++
title = 'How Many Numbers Are Smaller Than the Current Number'
date = 2025-08-26T18:13:35-07:00
# draft = true
+++

## Problem Description

**LeetCode Problem:** [How Many Numbers Are Smaller Than the Current Number](https://leetcode.com/problems/how-many-numbers-are-smaller-than-the-current-number/description/)

## Input/Output

### Input
- `nums`: List[int] - An array of integers
- Constraints:
  - 2 <= nums.length <= 500
  - 0 <= nums[i] <= 100

### Output
- List[int] - For each element in the input array, return how many numbers in the array are smaller than that element
- The output array has the same length as the input array


This is an easy problem that's very suitable for interviews as it allows you to demonstrate multiple solution approaches with different complexities.


## Brute Force

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

### Complexity
- Time complexity: `O(n²)`
- Space complexity: `O(n)`

## Optimization: Sorting + Hash Map

We notice that the brute force approach has many duplicated computations. For each element, we're recounting how many numbers are smaller than it.

To avoid this duplication, we can make a space-time tradeoff by preprocessing the data. The key insight is: if we sort the array first, then the index of each element tells us exactly how many numbers are smaller than it. We use a hash map to store this mapping for quick O(1) lookups.

**Algorithm steps:**
1. Sort the array to determine positions
2. Create a hash map: value → count of smaller numbers  
3. For each element in the original array, lookup its count from the hash map

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

### Complexity
- Time complexity: `O(n log n)`
- Space complexity: `O(n)`

## Counting Sort (Bucket Method)

When we can use a hash map, we should always consider if we can replace it with an **array** (bucket/counting approach). This is especially effective when the range of possible values is limited and known in advance.

**Key insight**: Since `0 <= nums[i] <= 100`, we can use a fixed-size array of length 101 instead of a hash map. This approach is called **counting sort** because we count the frequency of each value.

**Algorithm steps:**
1. Create a frequency array to count occurrences of each number
2. Convert frequencies to cumulative counts (prefix sum)
3. For each element, lookup its count of smaller numbers directly from the array 

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

### Complexity
- Time complexity: `O(n)`
- Space complexity: `O(n)`