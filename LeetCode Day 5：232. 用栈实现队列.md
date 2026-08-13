**日期**：2026-08-13  
**难度**：Easy  
**状态**：✅ 已完成
## 1. 题目理解
使用两个栈实现队列的 `push`、`pop`、`peek`、`empty` 四种操作。  
栈是后进先出（LIFO），队列是先进先出（FIFO），需要用两个栈模拟队列顺序。

## 2. 解题思路

### 核心直觉（一句话抓住本质）
> **入队永远进第一个栈；出队时，第二个栈不为空就直接出，为空就把第一个栈全部倒过来。**

### 两个栈的角色
- `in_stack`：只管接收新元素（入队）。
- `out_stack`：只管输出队首元素（出队、查看）。
- **转移时机**：只有当 `out_stack` 为空时，才把 `in_stack` 的所有元素倒入 `out_stack`，使队首出现在栈顶。

### 算法步骤
1. `push(x)`：直接 `in_stack.append(x)`。
2. `peek()`：
   - 如果 `out_stack` 为空，将 `in_stack` 的元素逐个弹出并压入 `out_stack`。
   - 返回 `out_stack[-1]`（栈顶即队首）。
3. `pop()`：先调用 `peek()` 确保 `out_stack` 有元素，再 `out_stack.pop()`。
4. `empty()`：两个栈都为空才返回 `True`。

## 3. 代码
```python
class MyQueue(object):
    def __init__(self):
        self.in_stack = []
        self.out_stack = []

    def push(self, x):
        self.in_stack.append(x)

    def peek(self):
        if not self.out_stack:
            while self.in_stack:
                self.out_stack.append(self.in_stack.pop())
        return self.out_stack[-1]

    def pop(self):
        self.peek()
        return self.out_stack.pop()

    def empty(self):
        return not self.in_stack and not self.out_stack
```

## 4. 复杂度分析
- **push**：时间复杂度 O(1)，空间 O(1)。
- **pop/peek**：均摊时间复杂度 O(1)。每个元素最多从 `in_stack` 转移到 `out_stack` 一次。
- **empty**：O(1)。

## 5. 踩坑记录
- 误以为入队时需要先确定第二个栈的状态，其实入队只进第一个栈。
- 忘记出队时先检查 `out_stack` 是否为空，导致顺序错乱。
- 变量名与 `self.in_stack` / `self.out_stack` 不统一，无法运行。
- 转移时机理解错误：不是“第一个栈满了才倒”，而是“第二个栈空了才倒”。

## 6. 一句话总结
**“两个栈倒一次，后进先出变先进先出；第二个栈不空，永远直接出队首。”**