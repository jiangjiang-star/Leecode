**日期**：2026-09-01  
**难度**：Easy  
**状态**：✅ 已完成

## 1. 题目理解
给定两个数组 `nums1` 和 `nums2`，返回它们的交集。结果元素唯一，顺序不限。

## 2. 解题思路

### 核心直觉
> **把一个数组转成集合，遍历另一个数组，判断元素是否在集合中。**

### 方法一：手写哈希集合法（基础版）
1. `set(nums1)` 去重，让查找变成 O(1)。
2. 遍历 `nums2`，如果元素在集合中，加入结果集合 `seen2`。
3. 结果集合自动去重，最后转成列表返回。

### 方法二：手写哈希集合法（简化版）
- 不用第二个集合，直接用列表 `result` 存结果。
- 判断时同时检查 `num in set1` 和 `num not in result` 来去重。

### 方法三：一行集合交集法（最简洁）
- 直接使用 Python 集合运算符 `&` 求交集。
- `list(set(nums1) & set(nums2))` 一步完成。

## 3. 代码

### 方法一：手写哈希集合法（基础版）
​```python
class Solution(object):
    def intersection(self, nums1, nums2):
        seen = set(nums1)
        seen2 = set()
        for num in nums2:
            if num in seen:
                seen2.add(num)
        return list(seen2)
​```

### 方法二：手写哈希集合法（简化版）
​```python
class Solution(object):
    def intersection(self, nums1, nums2):
        set1 = set(nums1)
        result = []
        for num in nums2:
            if num in set1 and num not in result:
                result.append(num)
        return result
​```

### 方法三：一行集合交集法（最简洁）
​```python
class Solution(object):
    def intersection(self, nums1, nums2):
        return list(set(nums1) & set(nums2))
​```

## 4. 复杂度分析

| 方法 | 时间复杂度 | 空间复杂度 |
|------|-----------|-----------|
| 基础版 | O(n + m) | O(n + k) |
| 简化版 | O(n + m) | O(n + k) |
| 一行版 | O(n + m) | O(n + k) |

## 5. 踩坑记录
- `set(nums1) & set(nums2)` 返回的是集合，不是列表，必须用 `list()` 转换。
- 结果需要唯一，所以用集合或 `num not in result` 去重。
- 遍历 `nums2` 时，不一定要统计次数，只需要判断“是否在集合中”。
- 基础版多了一个 `seen2` 集合，虽然直观但可以优化掉。

## 6. 一句话总结
**“集合去重 + O(1) 查找，是处理交集问题的天然工具。”**