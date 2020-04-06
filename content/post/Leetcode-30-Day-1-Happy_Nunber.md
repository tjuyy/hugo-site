---
title: "30-Day LeetCoding Challenge Day 1: Happy Nunber"
date: 2020-04-01T13:48:08+08:00
lastmod: 2020-04-01T13:48:08+08:00
draft: false
tags: ["leetcode", "algorithm", "python"]
categories: ["leetcode"]
---

[Leetcode 活动](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/)

## Day 1: Single Number

### 题目

Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

**Note:**

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

**Example 1:**

```
Input: [2,2,1]
Output: 1
```

**Example 2:**

```
Input: [4,1,2,1,2]
Output: 4
```

### 思路

根据异或运算的真值表，两个相同的数，按位异或结果为0。将array中所有的元素按位异或，最后剩下的数就是结果。

### 代码

#### Python

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        from functools import reduce
        return reduce(lambda x, y: x ^ y, nums)
```

#### Java

```java
class Solution {
    public int singleNumber(int[] nums) {
        int a = 0;
        for (int i:nums) {
            a ^= i;
        }
        return a;
    }
}
```