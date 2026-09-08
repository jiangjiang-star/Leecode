**日期**：2026-09-07  
**难度**：Easy  
**状态**：✅ 已完成（迭代法 + 递归法）
## 1. 题目理解
给定单链表的头节点 `head`，反转链表，并返回反转后的链表。

**示例**：
- 输入：`1 -> 2 -> 3 -> 4 -> 5 -> NULL`
- 输出：`5 -> 4 -> 3 -> 2 -> 1 -> NULL`

## 2. 核心概念：链表 ≠ 列表
- **Python 列表 `list`**：连续内存，可用下标访问，反转有内置方法。
- **链表（Linked List）**：由节点组成，每个节点包含 `val`（值）和 `next`（指向下一个节点的引用）。
- 反转链表考察的是**指针操作**，不是调用 `reverse()`。

## 3. 解题思路

### 方法一：迭代法（推荐掌握）
用三个指针 `prev`、`curr`、`next_temp` 逐个反转节点指向。

**核心步骤**：
1. 初始化 `prev = None`，`curr = head`。
2. 遍历链表，对当前节点：
   - `next_temp = curr.next`（保存下一个节点，防止断链）
   - `curr.next = prev`（反转指向）
   - `prev = curr`（prev 前进）
   - `curr = next_temp`（curr 前进）
3. 当 `curr` 为 `None` 时，`prev` 就是新头节点。

### 方法二：递归法
假设后面部分已经反转好，只处理当前节点：
1. 递归终止条件：`not head or not head.next`。
2. 递归反转 `head.next`，得到新头节点 `new_head`。
3. `head.next.next = head`（让下一个节点反向指向自己）。
4. `head.next = None`（断开原指向，防止成环）。
5. 返回 `new_head`。

**对比**：
| 方法 | 时间复杂度 | 空间复杂度 | 特点 |
|------|-----------|-----------|------|
| 迭代法 | O(n) | O(1) | 空间最优，面试首选 |
| 递归法 | O(n) | O(n) | 代码简洁，但可能栈溢出 |

## 4. 代码

### 迭代法
​```python
class Solution(object):
    def reverseList(self, head):
        prev = None
        curr = head
        while curr:
            next_temp = curr.next   # 保存下一个节点
            curr.next = prev        # 反转指向
            prev = curr             # 移动 prev
            curr = next_temp        # 移动 curr
        return prev                 # 新的头节点
​```

### 递归法
​```python
class Solution(object):
    def reverseList(self, head):
        if not head or not head.next:
            return head
        
        new_head = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        
        return new_head
​```

## 5. 踩坑记录
- 迭代法中，必须先保存 `next_temp`，否则 `curr.next = prev` 后链表会断。
- 递归法中，`head.next = None` 不能漏，否则会形成环。
- 空链表或单节点链表：迭代法直接返回 `prev`（None 或该节点），递归法直接返回 `head`。
- 链表节点定义：`ListNode(val, next)`，`next` 是引用不是值。

## 6. 一句话总结
**“反转链表就是逐个改变 next 指向；迭代法用三个指针，递归法先反转后面再处理当前节点。”**