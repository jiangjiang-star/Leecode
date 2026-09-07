**日期**：2026-08-14  
**难度**：Easy  
**状态**：✅ 已完成
## 1. 题目理解
设计一个栈，支持 `push`、`pop`、`top`、`getMin` 四种操作，并且 `getMin` 必须在 **O(1)** 时间内完成。
## 2. 解题思路

### 核心直觉（一句话抓住本质）
> **辅助栈和主栈一一对应，同步保存“截至当前元素为止的最小值”。**

### 两个栈的角色
- `stack`：普通主栈，存所有数据。
- `min_stack`：辅助栈，长度始终和主栈相同，每个位置存 **对应主栈位置时的当前最小值**。
- **同步规则**：
  - `push` 时，主栈正常入栈；辅助栈入栈时比较新元素和当前最小值：
    - 新元素 ≤ 当前最小值 → 辅助栈入新元素。
    - 否则 → 辅助栈重复入当前最小值（保持长度一致）。
  - `pop` 时，主栈和辅助栈 **同时弹出**。
  - `getMin` 直接返回 `min_stack[-1]`，因为它永远等于主栈当前的最小值。

### 算法步骤
1. `__init__`：初始化两个空列表。
2. `push(val)`：
   - `stack.append(val)`。
   - 如果 `min_stack` 为空，或 `val <= min_stack[-1]`，则 `min_stack.append(val)`。
   - 否则 `min_stack.append(min_stack[-1])` 保持同步。
3. `pop()`：`stack.pop()` 和 `min_stack.pop()`。
4. `top()`：返回 `stack[-1]`。
5. `getMin()`：返回 `min_stack[-1]`。

## 3. 代码

```python
class MinStack(object):
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val):
        self.stack.append(val)
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)
        else:
            self.min_stack.append(self.min_stack[-1])

    def pop(self):
        self.stack.pop()
        self.min_stack.pop()

    def top(self):
        return self.stack[-1]

    def getMin(self):
        return self.min_stack[-1]
```

## 4. 复杂度分析
- **push**：O(1)
- **pop**：O(1)
- **top**：O(1)
- **getMin**：O(1)
- **空间复杂度**：O(n)，辅助栈与主栈同步存储，需要额外 O(n) 空间。

## 5. 踩坑记录
- 变量名忘记加 `self.`，导致方法之间无法共享栈。
- 用 `min_stack.pop()` 查看栈顶，导致元素被错误弹出，应该用 `min_stack[-1]`。
- `push` 中 `else` 分支没有重复压入当前最小值，导致两个栈长度不一致，后续 `pop` 错位。
- 最终理解关键：**两个栈必须一一对应、同步弹出，辅助栈顶才始终等于主栈最小值。**

## 6. 一句话总结
**“主栈存数据，辅助栈同步存最小值；一起入栈一起出栈，getMin 直接读辅助栈顶。”**