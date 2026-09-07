**日期**：2026-08-22  
**难度**：Easy  
**状态**：✅ 已完成
## 1. 题目理解
判断两个字符串 `s` 和 `t` 是否互为字母异位词。  
字母异位词：两个字符串包含的字母**完全相同**，只是顺序不同。

## 2. 解题思路

### 方法一：排序法（简单直观）
- 如果两个字符串是异位词，排序后必然完全相同。
- 直接比较 `sorted(s) == sorted(t)`。

### 方法二：哈希表统计法（空间换时间）
- 用字典 `counts` 统计 `s` 中每个字符的出现次数。
- 遍历 `t`，每遇到一个字符就从 `counts` 中减 1。
- 如果 `t` 中出现 `s` 没有的字符，或某个字符数量超过 `s`，返回 `False`。
- 最后所有计数应该都归零（前提是长度相等）。

### 核心直觉（一句话抓住本质）
> **先统计 s 的字符频率，再拿 t 去核销；出现多余字符或库存变负数就失败。**

## 3. 代码

### 排序法
​```python
class Solution(object):
    def isAnagram(self, s, t):
        return sorted(s) == sorted(t)
​```

### 哈希表统计法
​```python
class Solution(object):
    def isAnagram(self, s, t):
        if len(s) != len(t):
            return False

        counts = {}
        for ch in s:
            counts[ch] = counts.get(ch, 0) + 1

        for ch in t:
            if ch not in counts:   # t 中出现 s 没有的字符
                return False
            counts[ch] -= 1
            if counts[ch] < 0:     # t 中该字符数量比 s 多
                return False

        return True
​```

## 4. 复杂度分析
- **排序法**：时间 O(n log n)，空间 O(1)（取决于排序实现）
- **哈希表法**：时间 O(n)，空间 O(n)（字典存储字符频率）

## 5. 踩坑记录
- 忘记先检查 `len(s) != len(t)`，导致后面判断复杂化。
- 第二个循环中，两种情况要返回 False：字符不存在 / 库存减到负数。
- 字典统计用 `counts.get(ch, 0) + 1` 比 `if ch in counts` 更简洁。
- 排序法虽然简单，但时间复杂度较高，面试时尽量用哈希表法。

## 6. 一句话总结
**“排序法最简单，哈希表法最快；字母异位词的本质是字符频率相同。”**
