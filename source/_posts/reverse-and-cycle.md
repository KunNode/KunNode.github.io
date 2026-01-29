---
title: 🔄 链表基础：反转与环形检测
date: 2026-01-30 21:00:00
tags:
  - 算法
  - 链表
  - 快慢指针
  - LeetCode
categories:
  - 算法学习
cover: /img/cover3.png
---

> 🎯 **今日目标**：掌握**反转链表**与**环形链表**，深入理解**快慢指针**与 **dummy 节点**的妙用！

<!--more-->

---

## 🔁 反转链表（迭代法）

{% note info flat %}
💡 **核心思想**：维护 `prev` 与 `cur` 双指针，逐个断开并反向指向，像翻书一样逐页翻转！
{% endnote %}

### 代码实现

```cpp
ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* cur = head;
    while (cur) {
        ListNode* nxt = cur->next;  // 📌 先保存下一个
        cur->next = prev;           // 🔄 反转指向
        prev = cur;                 // ⏩ prev 前进
        cur = nxt;                  // ⏩ cur 前进
    }
    return prev;
}
```

### 🎯 关键要点

| 步骤 | 操作 | 说明 |
|:---:|:---|:---|
| 1️⃣ | 保存 `nxt = cur->next` | 避免链表断掉找不到后继 |
| 2️⃣ | `cur->next = prev` | 反转当前节点的指向 |
| 3️⃣ | 移动 `prev` 和 `cur` | 继续处理下一个节点 |

{% note success flat %}
✨ **循环不变量**：`prev` 永远指向已反转好的链表头部！
{% endnote %}

---

## 🔍 环形链表

### 2.1 判断是否有环（Floyd 快慢指针）

{% note warning flat %}
🐢🐰 **龟兔赛跑**：快指针每次走2步，慢指针走1步，若存在环，它们一定会相遇！
{% endnote %}

```cpp
bool hasCycle(ListNode* head) {
    ListNode* slow = head;  // 🐢 慢指针
    ListNode* fast = head;  // 🐰 快指针
    
    while (fast && fast->next) {
        slow = slow->next;        // 🐢 走1步
        fast = fast->next->next;  // 🐰 走2步
        if (slow == fast) return true;  // 🎉 相遇了！
    }
    return false;  // 💨 快指针到达终点，无环
}
```

### 2.2 找环的入口

{% note info flat %}
🔬 **数学证明**：相遇后，令一个指针从 `head` 出发，另一个从相遇点出发，每次都走1步，再次相遇点即为环的入口！
{% endnote %}

```cpp
ListNode* detectCycle(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        
        if (slow == fast) {  // 🎯 第一次相遇
            ListNode* p1 = head;   // 📍 从头出发
            ListNode* p2 = slow;   // 📍 从相遇点出发
            while (p1 != p2) {
                p1 = p1->next;
                p2 = p2->next;
            }
            return p1;  // 🎉 环的入口！
        }
    }
    return nullptr;
}
```

---

## 🎭 Dummy 节点使用场景

{% note primary flat %}
📝 **一句话总结**：当「头节点也可能被删除/插入/需要统一处理」时，用 dummy 把头部边界变成普通情况！
{% endnote %}

### 常见使用场景

| 场景 | 示例题目 | 作用 |
|:---|:---|:---|
| 🗑️ 删除节点 | 删除倒数第N个节点 | 统一处理删除头节点 |
| 🔗 合并链表 | 合并两个有序链表 | 提供固定的起点 |
| 🔄 交换节点 | 两两交换、K个一组翻转 | 简化头部插入操作 |

### 收益

```text
✅ 少写很多 if (head == ...) 的特判
✅ 代码逻辑更统一
✅ 不容易漏边界条件
```

---

## 📝 今日总结

{% note success flat %}
### 🌟 核心收获

| 技巧 | 要点 |
|:---|:---|
| **反转链表** | 迭代三指针最稳 `prev` → `cur` → `nxt` |
| **环形链表** | Floyd 判环 + 二次相遇找入口 |
| **Dummy节点** | 把「头部特殊情况」消掉，链表操作更模板化 |
{% endnote %}

### 🧠 记忆口诀

> 🎵 *反转三指针，环找快慢针，dummy消边界，链表变简单！*

---

*今天也是努力刷题的一天呢~ (ง •_•)ง* ✨
