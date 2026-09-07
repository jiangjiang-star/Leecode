**日期**：2026-08-21  
**难度**：Easy  
**状态**：✅ 已完成
## 1. 题目理解
给定整数数组 `nums` 和目标值 `target`，找出两个数之和等于 `target` 的下标。  
假设只有一个答案，且不能重复使用同一个元素。

## 2. 解题思路

### 核心直觉（一句话抓住本质）
> **遍历时先查字典，再把自己登记进去——先登记后配对。**

### 哈希表解法
- 用字典 `seen` 存“已经遍历过的数字”和它的索引。
- 遍历数组，对于当前数字 `num`：
  1. 计算需要找的另一个数 `complement = target - num`。
  2. 如果 `complement` 已经在字典中，直接返回它的索引和当前索引。
  3. 否则，把当前数字 `num` 和索引 `i` 存入字典。
- 这样每个数字只访问一次，时间复杂度 O(n)。

### 为什么哈希表能加速？
- 字典查找是 O(1)，暴力法中的“找补数”变成即时完成。
- 空间换时间：用额外 O(n) 空间存储已遍历元素。

## 3. 代码

​```python
class Solution(object):
    def twoSum(self, nums, target):
        seen = {}
        for i, num in enumerate(nums):
            complement = target - num
            if complement in seen:
                return [seen[complement], i]
            seen[num] = i
        return []  # 题目保证有答案，但为了语法完整
​```

## 4. 复杂度分析
- **时间复杂度**：O(n)，只遍历一次数组。
- **空间复杂度**：O(n)，最坏情况需要存储 n 个元素。

## 5. 踩坑记录
- 暴力法 `for i in range(size-1)` 和 `for j in range(size-1)` 范围容易重复/漏掉组合。
- 哈希表思路卡在 `seen[num] = i` 这一行，后来明白：**当前数字要留到后面配对**。
- 字典的键是数字本身，不是补数；值是索引。
- 先查 `complement in seen`，再存 `seen[num] = i`，顺序不能反。

## 6. 一句话总结
**“用字典记住每个数字的位置，后面遇到补数时直接配对。”**
# LeetCode Day 8（补）：1. 两数之和（暴力解法）

**日期**：2026-08-21  
**难度**：Easy  
**状态**：✅ 已完成（用于理解优化前的基础思路）

---

## 1. 题目理解
给定整数数组 `nums` 和目标值 `target`，找出两个数之和等于 `target` 的下标。  
假设只有一个答案，且不能重复使用同一个元素。

## 2. 解题思路

### 核心直觉（一句话抓住本质）
> **把所有两两组合都试一遍，找到和等于 target 的那一对。**

### 暴力双循环
- 外层循环 `i` 从 0 到 `len(nums)-1`。
- 内层循环 `j` 从 `i+1` 到 `len(nums)-1`。
- 检查 `nums[i] + nums[j] == target`，找到就返回 `[i, j]`。
- 内层从 `i+1` 开始，避免重复使用同一个元素，也避免重复组合。

## 3. 代码

​```python
class Solution(object):
    def twoSum(self, nums, target):
        size = len(nums)
        for i in range(size):
            for j in range(i + 1, size):
                if nums[i] + nums[j] == target:
                    return [i, j]
        return []  # 题目保证有答案，但为了语法完整
​```

## 4. 复杂度分析
- **时间复杂度**：O(n²)，最坏情况要检查所有两两组合。
- **空间复杂度**：O(1)，只用了常数个变量。

## 5. 踩坑记录
- `for j in range(size-1)` 写错，导致范围不够或重复检查。
- 内层循环从 `i+1` 开始，忘记这点会重复使用同一个元素（比如 `nums[i]+nums[i]`）。
- 找到答案后要用 `return`，不要用 `self.l` 存结果再返回。
- 暴力法能通过，但数组大时会超时，所以需要哈希表优化。

## 6. 一句话总结
**“暴力法是穷举所有组合；哈希表法是用字典记住之前见过的数字，省去内层查找。”**