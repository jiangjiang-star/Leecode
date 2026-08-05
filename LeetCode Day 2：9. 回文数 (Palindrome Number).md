**日期**：2026-08-05  
**难度**：Easy  
**状态**：✅ 已完成（基础版 + 进阶版）
## 1. 题目理解
判断一个整数是否是回文数。回文数是指正序和倒序读都是一样的整数。  
- 负数一定不是回文（`-121` 倒序是 `121-`）  
- 末尾为0的非零数一定不是回文（`10` 倒序是 `01`）
## 2. 解题思路

### 基础版（字符串反转）
将整数转为字符串，用左右指针从两端向中间比较。
### 进阶版（数学反转一半）
- 特殊处理负数 和 末尾为0的非零数  
- 通过循环 `while x > rev` 将原始数字的后半部分反转存入 `rev`  
- 最终比较：偶数位 `x == rev`；奇数位 `x == rev // 10`  
- **关键点**：循环条件 `x > rev` 保证了只反转一半数字，循环结束后再比较，不能在循环内提前判断。
## 3. 代码

### 基础版
​```python
class Solution(object):
    def isPalindrome(self, x):
        if x < 0:
            return False
        s = str(x)
        left, right = 0, len(s) - 1
        while left < right:
            if s[left] != s[right]:
                return False
            left += 1
            right -= 1
        return True
​```

### 进阶版
​```python
class Solution(object):
    def isPalindrome(self, x):
        if x < 0:
            return False
        if x != 0 and x % 10 == 0:
            return False
        
        rev = 0
        while x > rev:
            rev = rev * 10 + x % 10
            x //= 10
        
        return x == rev or x == rev // 10
​```

## 4. 复杂度分析
- 基础版：时间 O(n)，空间 O(n)，n 为数字位数。  
- 进阶版：时间 O(log n)，空间 O(1)。

## 5. 踩坑记录
- `int` 对象不可迭代：不能直接 `[int(i) for i in x]`，必须先 `str(x)`。  
- 直接用str加两个指针就够了，不需要列表
- `range(le/2)` 报错：必须用 `le // 2` 整除。  
- 循环内提前判断导致 `x=1` 误判为 False：必须循环结束后再比较。  
- 比较式写反：正确是 `x == rev // 10`，不是 `rev == x // 10`。  
- 末位0的陷阱：`10` 反转后会丢掉前导零，必须提前过滤。

## 6. 总结
用数学反转一半数字，循环结束后比较左右半部分，特别注意奇数位数和末尾零。打脑壳的数学思考