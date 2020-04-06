---
title: "30-Day LeetCoding Challenge Day 2: Happy Number"
date: 2020-04-02T13:48:08+08:00
lastmod: 2020-04-02T13:48:08+08:00
draft: false
tags: ["leetcode", "algorithm", "python"]
categories: ["leetcode"]
---

[Leetcode 活动](https://leetcode.com/explore/featured/card/30-day-leetcoding-challenge/)

## Day 2: Happy Number

### 题目

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example:** 

```
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

### 思路1，利用集合，判断是否出现环

1. 一个正整数的每位数字求平方和，重复这个过程，直到结果得到1，则这个数是快乐数，返回True
2. 如果出现循环，则这个数不是快乐数，返回False

### 代码

#### Python

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        s = set()
        while n!=1 and n not in s:
            s.add(n)
            n =  sum(pow(int(i), 2) for i in str(n))
        return n == 1  
```

#### Java

```java
class Solution {
    public boolean isHappy(int n) {
        HashSet<Integer> seen = new HashSet<>();
        while (n != 1) {
            int current = n;
            int sum = 0;
            while (current != 0) {
                sum += (current % 10) * (current % 10);
                current /= 10;
            }

            if (seen.contains(sum)) {
                return false;
            }

            seen.add(sum);
            n = sum;
        }
        return true;
}
}
```

### 思路2，快慢指针判断成环

1. 初始化快慢指针都指向n
2. 慢指针走1步，快指针走2步
3. 任一指针为1时，返回True
4. 快慢相等时，成环，如果相等元素不为1，返回False

### 代码

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        slow = fast = n
        while slow != 1 and fast != 1:
            slow = self.squareSum(slow)
            fast = self.squareSum(self.squareSum(fast))
            if slow == fast:
                return slow == 1
        return True
    
    def squareSum(self, n: int) -> int:
        return sum(int(i)*int(i) for i in str(n))
```

```java
class Solution {
    public boolean isHappy(int n) {
        int slow = n;
        int fast = n;
        do {
            slow = squareSum(slow);
            fast = squareSum(squareSum(fast));
        }
        while(slow != fast);
        
        return slow == 1;
    }
    
    public int squareSum(int n) {
        int sum = 0;
        while (n != 0) {
            sum += (n%10) * (n%10);
            n /= 10;
        }
        return sum;
    }
}
```

### 思路3，判断个位数

1. 总结规律，当数字只有1位时，只有个位数是1和7的才是快乐数
2. 2的倍数是4，3的倍数是9，都会成环

### 代码

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        while True:
            if n == 1 or n == 7:
                return True
            if 2 <= n < 6 or 8 <= n <= 9:
                return False
            sum = 0
            while n > 0:
                tmp = n % 10
                sum += tmp * tmp
                n = n//10
            n = sum
```

```java
class Solution { 
    public boolean isHappy(int n) { 
        while(true){
            if (n==1 || n==7 ) return true;
            if((n>=2 && n<7) ||(n>7 && n<=9)) return false;
            int sum=0;
            while(n>0) {
                int tmp = n%10;
                sum += tmp*tmp;
                n = n/10;
            }
            n=sum;
        }
    }
}
```