**日期**：2026-08-20  
**难度**：Easy  
**状态**：✅ 已完成

## 1. 题目理解
使用队列实现栈的 `push`、`pop`、`top`、`empty` 四种操作。  
队列是先进先出（FIFO），栈是后进先出（LIFO）。  
本题用**单队列**完成，核心动作是“旋转”。

## 2. 解题思路

### 核心直觉（一句话抓住本质）
> **入队时把新元素旋转到队首，出队时直接弹出队首。**

### 单队列“旋转”原理
- `push(x)`：
  1. 正常入队：`q.append(x)`
  2. 把前面 `n-1` 个元素依次出队再入队，让新元素到达队首。
- `pop()`：直接弹出队首（因为队首永远是最后入队的元素）。
- `top()`：直接查看队首。
- `empty()`：队列为空则返回 `True`。

### 与双队列的对比
| 方法 | 核心动作 | 操作时机 |
|------|---------|---------|
| 双队列 | 留最后一个 | 出队时转移前 n-1 个元素 |
| 单队列 | 旋转 | 入队时把新元素转到队首 |

## 3. 代码

​```python
class MyStack(object):
    def __init__(self):
        self.q = []

    def push(self, x):
        self.q.append(x)           # 正常入队
        size = len(self.q)
        for _ in range(size - 1):  # 旋转：把前 n-1 个元素出队再入队
            self.q.append(self.q.pop(0))

    def pop(self):
        return self.q.pop(0)       # 队首就是栈顶

    def top(self):
        return self.q[0]

    def empty(self):
        return not self.q
​```

## 4. 复杂度分析
- **push**：O(n)，需要旋转 n-1 次。
- **pop**：O(1)。
- **top**：O(1)。
- **empty**：O(1)。
- **空间复杂度**：O(n)，只使用一个队列。

## 5. 踩坑记录
- 队列出队必须用 `pop(0)`，不是 `pop()`。`pop()` 是栈的操作。
- `push` 中旋转次数是 `size - 1`，不是 `size`。循环 `size` 次会导致新元素又回到队尾。
- 单队列和双队列的本质区别：单队列在**入队时**旋转，双队列在**出队时**转移。

## 6. 一句话总结
**“单队列模拟栈，靠的是入队时旋转，把新元素顶到队首。”**
# （补）：225. 用队列实现栈（双队列版）
## 1. 题目理解
使用两个队列实现栈的 `push`、`pop`、`top`、`empty` 四种操作。  
队列先进先出，栈后进先出。  
核心是：**出队时只留最后一个元素，让它暴露出来，因为它是最后入队的。**

## 2. 解题思路

### 核心直觉（一句话抓住本质）
> **出队时，把前 n-1 个元素转移到辅助队列，只留最后一个元素，这个就是栈顶。**

### 两个队列的角色
- `q1`：主队列，正常接收 `push`。
- `q2`：辅助队列，在 `pop` / `top` 时临时存放被转移的元素。
- **关键操作**：转移后交换 `q1` 和 `q2`，让 `q1` 重新成为主队列。

### 算法步骤
1. `push(x)`：直接入队 `q1`。
2. `pop()`：
   - 当 `q1` 长度大于 1 时，把队首元素出队并转入 `q2`。
   - `q1` 只剩一个元素，弹出它并保存为 `result`。
   - 交换 `q1` 和 `q2`。
   - 返回 `result`。
3. `top()`：
   - 类似 `pop`，但需要把最后一个元素也保留并放回。
4. `empty()`：两个队列都为空则返回 `True`。

## 3. 代码

​```python
class MyStack(object):
    def __init__(self):
        self.q1 = []
        self.q2 = []

    def push(self, x):
        self.q1.append(x)

    def pop(self):
        # 把前 n-1 个元素从 q1 转移到 q2
        while len(self.q1) > 1:
            self.q2.append(self.q1.pop(0))
        # 剩下的是栈顶
        result = self.q1.pop(0)
        # 交换，让 q1 重新成为主队列
        self.q1, self.q2 = self.q2, self.q1
        return result

    def top(self):
        while len(self.q1) > 1:
            self.q2.append(self.q1.pop(0))
        top_element = self.q1[0]
        self.q2.append(self.q1.pop(0))  # 保留最后一个元素到 q2
        self.q1, self.q2 = self.q2, self.q1
        return top_element

    def empty(self):
        return not self.q1 and not self.q2
​```

## 4. 复杂度分析
- **push**：O(1)
- **pop**：O(n)，需要转移 n-1 个元素
- **top**：O(n)
- **empty**：O(1)
- **空间复杂度**：O(n)，两个队列合计存储所有元素

## 5. 踩坑记录
- 队列出队必须用 `pop(0)`，误用 `pop()` 会弹出末尾元素。
- 转移后忘记交换 `q1` 和 `q2`，导致下一次操作逻辑混乱。
- `top` 中不能丢失最后一个元素，必须把它也转移到 `q2` 再交换。
- 循环条件 `while len(self.q1) > 1` 容易写成 `if not self.q1`，这是相反的逻辑。

## 6. 一句话总结
**“双队列模拟栈，靠的是出队时转移前 n-1 个元素，只留最后一个，然后交换队列。”**