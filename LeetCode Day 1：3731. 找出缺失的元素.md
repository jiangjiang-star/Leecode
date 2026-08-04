# LeetCode Day 1：3731. 找出缺失的元素

**日期**：2026-08-04  
**难度**：Easy  
**状态**：✅ 已完成（基础版）

---

## 1. 题目理解
给一个乱序的整数数组，最小值和最大值还在，但中间可能缺数字。  
找出缺失的所有整数，按顺序返回。  
例子：[1,2,4,5] → 缺失 [3]

## 2. 解题思路
1. 找到数组的最小值 `min_val` 和最大值 `max_val`
2. 用 `range(min_val, max_val+1)` 生成完整序列
3. 遍历完整序列，如果某个数不在原数组里，就是缺失的
4. 把缺失的数加入结果列表

## 3. 代码
​```python
class Solution(object):
    def findMissingElements(self, nums):
        min_val = min(nums)
        max_val = max(nums)
        full_range = list(range(min_val, max_val + 1))
        missing = []
        for num in full_range:
            if num not in nums:
                missing.append(num)
        return missing
​```

## 4. 复杂度分析
- 时间复杂度：O(m × n)，m 是范围大小，n 是数组长度  
  因为 `num not in nums` 每次都要遍历列表查找
- 空间复杂度：O(m)，生成了完整范围列表

## 5. 踩坑记录
- `l1 = l1.append(i)` 返回 None，因为 `append()` 直接修改列表，没有返回值。应改为 `l1.append(i)`。
- `range(a, b)` 不包含 b，必须用 `range(a, b+1)` 才能包含最大值。

## 6. 明天优化方向
- 把 `nums` 转成 `set`，让 `in` 操作变成 O(1)，总复杂度降到 O(m + n)
- 试试列表推导式一行写完
# 收获
对于语法更加理解了，有新思路，对于列表有更清晰的认知了。