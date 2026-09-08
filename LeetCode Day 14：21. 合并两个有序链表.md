**日期**：2026-09-08  
**难度**：Easy  
**状态**：✅ 已完成
## 1. 题目理解
将两个升序链表合并为一个新的升序链表，并返回。新链表由原两个链表的节点拼接而成。

**示例**：
- 输入：`1 -> 2 -> 4`，`1 -> 3 -> 4`
- 输出：`1 -> 1 -> 2 -> 3 -> 4 -> 4`

## 2. 解题思路

### 核心直觉（一句话抓住本质）
> **双指针分别遍历两个链表，谁小接谁，最后把剩余链表直接接上。**

### 迭代法：哨兵节点 + 双指针
1. 创建哨兵节点 `dummy`，让 `cur` 从 `dummy` 开始。
2. 同时遍历两个链表，只要两个都不为空：
   - 比较 `list1.val` 和 `list2.val`。
   - 把较小节点接到 `cur.next`。
   - 移动较小值所在链表的指针。
   - `cur` 也前进一步。
3. 循环结束后，把不为空的那个链表剩余部分直接接到 `cur.next`。
4. 返回 `dummy.next`。

### 为什么要用哨兵节点？
- 哨兵节点 `dummy` 是假头节点，用来统一处理“结果链表第一个节点”的情况。
- 不用哨兵节点，就需要额外判断结果链表是否为空，代码更繁琐。

## 3. 代码

​```python
class Solution(object):
    def mergeTwoLists(self, list1, list2):
        dummy = ListNode(-1)   # 哨兵节点
        cur = dummy            # 移动指针

        while list1 and list2:
            if list1.val <= list2.val:
                cur.next = list1        # 先接入当前节点
                list1 = list1.next      # 再移动 list1
            else:
                cur.next = list2
                list2 = list2.next
            cur = cur.next              # cur 也前进

        # 收尾：哪个链表还有剩余，就接哪个
        if list1:
            cur.next = list1
        else:
            cur.next = list2

        return dummy.next               # 返回真正的头节点
​```

## 4. 复杂度分析
- **时间复杂度**：O(n + m)，n 和 m 分别是两个链表的长度。
- **空间复杂度**：O(1)，只用了哨兵节点和若干指针，没有额外空间。

## 5. 踩坑记录
- **先移动指针再接入节点**：正确顺序是先 `cur.next = p1`，再 `p1 = p1.next`。反过来会把当前节点丢掉。
- **收尾判断写反**：`if p1: cur.next = p1`，不是 `if p1: cur.next = p2`。
- **忘记返回 `dummy.next`**：直接 `return` 会返回 `None`。
- **循环条件用错变量**：循环里用 `p1/p2`，循环条件却写 `while list1 and list2`，两者应统一。
- **哨兵节点的意义**：不是最终返回 `dummy`，而是 `dummy.next`。

## 6. 一句话总结
**“双指针谁小接谁，先接后移，最后接剩余，哨兵节点保头部。”**