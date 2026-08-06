**日期**：2026-08-06  
**难度**：Easy  
**状态**：✅ 已完成  

## 1. 题目理解
给一个字符串数组，找出最长公共前缀。无则返回 ""。

## 2. 解题思路
**纵向扫描法**：
1. 处理空数组和单元素数组。
2. 找到最短字符串长度 `min_len`。
3. 外层循环按列遍历字符，以内层循环检查所有字符串在该列是否与第一个字符串的对应字符相同。
4. 一旦某列不匹配，立即返回当前结果；全匹配则追加该字符。

## 3. 代码
```python
class Solution(object):

    def longestCommonPrefix(self, strs):

        str1=""

        if not strs:

            return str1

        elif len(strs)==1:

            str1=strs[0]

            return str1

        else:

            for i in range(min(len(s) for s in strs)):

                for j in range(1,len(strs)):

                    if strs[j][i]==strs[0][i]:

                        if j==len(strs)-1:

                            str1+=strs[0][i]

                    else:

                        return str1

        return str1
```
```python
#优化版本
class Solution(object):
    def longestCommonPrefix(self, strs):
        if not strs:
            return ""
        if len(strs) == 1:
            return strs[0]
        
        min_len = min(len(s) for s in strs)
        result = ""
        for i in range(min_len):
            char = strs[0][i]
            for j in range(1, len(strs)):
                if strs[j][i] != char:
                    return result
            result += char
        return result
```


## 4. 复杂度分析
- 时间复杂度：O(n*m)，n为字符串数量，m为最短字符串长度。
- 空间复杂度：O(1)，只用了常数额外空间。

## 5. 踩坑记录
- `not strs` 判断空列表，不能用 `== None`。
- `min(len(s) for s in strs)` 找最短长度，避免索引越界。
- 不匹配时要用 `return` 而非 `break`，否则会继续循环。

## 6. 一句话总结
两个指针进行扫描，一个指向字符串数组，一个指向字符串。还有如何找到字符串数组最小长度字符串的方法。