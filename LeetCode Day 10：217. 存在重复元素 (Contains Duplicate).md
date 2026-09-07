**日期**：2026-08-31  
**难度**：Easy  
**状态**：✅ 已完成
## 1. 题目理解
给定整数数组 `nums`，如果存在某个元素出现至少两次，返回 `True`；所有元素互不相同则返回 `False`。

## 2. 解题思路

### 方法一：哈希表统计法（最先想到的）
- 用字典 `seen` 统计每个数字的出现次数。
- 遍历数组，每遇到一个数字，计数 +1。
- 一旦某个数字的计数达到 2，立即返回 `True`。
- 遍历结束都没达到 2，返回 `False`。
- 直接访问不存在的键 `seen[num] = seen[num] + 1` 会报 `KeyError`。

### 方法二：哈希表判断法（更直接）
- 用字典记录“已经出现过的数字”。
- 遍历数组，如果当前数字已在字典中，直接返回 `True`。
- 否则把当前数字加入字典。
- 不需要统计次数，只需要知道“是否见过”。

### 方法三：集合去重法（最简洁）
- 利用集合 `set` 的元素唯一性。
- 如果数组长度 ≠ 去重后集合长度，说明有重复。

## 3. 代码

### 哈希表统计法（最终版）
​```python
class Solution(object):
    def containsDuplicate(self, nums):
        seen = {}
        for num in nums:
            seen[num] = seen.get(num, 0) + 1
            if seen[num] == 2:
                return True
        return False
​```

### 哈希表判断法
​```python
class Solution(object):
    def containsDuplicate(self, nums):
        seen = {}
        for num in nums:
            if num in seen:
                return True
            seen[num] = 1
        return False
​```

### 集合去重法
​```python
class Solution(object):
    def containsDuplicate(self, nums):
        return len(nums) != len(set(nums))
​```

## 4. 复杂度分析

| 方法 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 哈希表统计法 | O(n) | O(n) |
| 哈希表判断法 | O(n) | O(n) |
| 集合去重法 | O(n) | O(n) |

## 5. 踩坑记录
- 直接访问不存在的键 `seen[num] = seen[num] + 1` 会报 `KeyError`。
- 解决：用 `seen.get(num, 0)` 安全获取，或者先 `if num in seen` 判断。
- 判断重复只需要知道“是否见过”，不必真的统计次数。
- 集合 `set` 天然去重，是判断重复最简洁的工具。

## 6. 一句话总结
**“哈希表三种用法：统计次数、判断存在、集合去重；判断重复用 `in` 或 `set` 最直接。”**