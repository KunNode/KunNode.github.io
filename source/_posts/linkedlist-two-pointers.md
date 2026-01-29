---
title: 🔗 链表双指针技巧：合并、相交与删除
date: 2026-01-29 21:00:00
tags:
  - 算法
  - 链表
  - 双指针
  - LeetCode
categories:
  - 算法学习
cover: /img/cover1.png
---

> 🎯 三道经典链表题，核心思路均为 **双指针** + **哑节点（dummy）** 边界处理！

<!--more-->

---

## 1️⃣ 合并两个有序链表

{% note info flat %}
💡 **核心思路**：创建哑节点统一边界，双指针同步比较，较小者接入结果链！
{% endnote %}

### 关键步骤

| 步骤 | 操作 |
|:---:|:---|
| 1️⃣ | `current` 始终指向已合并链表的尾部 |
| 2️⃣ | 比较两个节点值，较小者接入 `current->next` |
| 3️⃣ | 对应指针后移一位 |
| 4️⃣ | 循环结束后，接入剩余链表 |

### 代码实现

```cpp
ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
    // 🎭 创建哑节点简化边界处理
    ListNode* dummy = new ListNode(-1);
    ListNode* current = dummy;
    
    // 🔄 双指针遍历两个链表
    while (list1 && list2) {
        if (list1->val <= list2->val) {
            current->next = list1;
            list1 = list1->next;
        } else {
            current->next = list2;
            list2 = list2->next;
        }
        current = current->next;
    }
    
    // ➕ 接入剩余链表
    current->next = list1 ? list1 : list2;
    
    ListNode* head = dummy->next;
    delete dummy;
    return head;
}
```

{% note success flat %}
⏱️ **时间复杂度**：O(m + n) | **空间复杂度**：O(1)
{% endnote %}

---

## 2️⃣ 相交链表

{% note warning flat %}
🔀 **浪漫算法**：两个指针各自走完后，走对方的路，若有缘一定会相遇！
{% endnote %}

### 核心原理

```text
📏 路径长度相等：lenA + lenB = lenB + lenA
🎯 有交点 → 第二轮相遇于交点
❌ 无交点 → 同时到达 nullptr
```

### 代码实现

```cpp
ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
    if (!headA || !headB) return nullptr;
    
    ListNode *pA = headA, *pB = headB;
    
    // 💫 当两个指针不相等时继续遍历
    while (pA != pB) {
        pA = (pA == nullptr) ? headB : pA->next;  // 🔄 走完A走B
        pB = (pB == nullptr) ? headA : pB->next;  // 🔄 走完B走A
    }
    
    return pA;  // 🎉 相遇点 or nullptr
}
```

{% note success flat %}
⏱️ **时间复杂度**：O(m + n) | **空间复杂度**：O(1)
{% endnote %}

---

## 3️⃣ 删除链表的倒数第 N 个结点

{% note info flat %}
🎯 **快慢指针**：快指针先走 n+1 步，当快指针到末尾时，慢指针恰好在待删除节点的前驱！
{% endnote %}

### 关键步骤

| 步骤 | 操作 | 说明 |
|:---:|:---|:---|
| 1️⃣ | 创建 `dummy` 节点 | 统一处理删除头节点 |
| 2️⃣ | `fast` 先走 n+1 步 | 确保间距正确 |
| 3️⃣ | 双指针同步移动 | 直到 `fast` 到达末尾 |
| 4️⃣ | 删除目标节点 | `slow->next = slow->next->next` |

### 代码实现

```cpp
ListNode* removeNthFromEnd(ListNode* head, int n) {
    // 🎭 创建哑节点
    ListNode* dummy = new ListNode(0);
    dummy->next = head;
    
    ListNode* fast = dummy;
    ListNode* slow = dummy;
    
    // 🐰 快指针先走 n+1 步
    for (int i = 0; i <= n; i++) {
        fast = fast->next;
    }
    
    // 🐢🐰 同步移动直到快指针到达末尾
    while (fast != nullptr) {
        fast = fast->next;
        slow = slow->next;
    }
    
    // 🗑️ 删除目标节点
    slow->next = slow->next->next;
    
    ListNode* newHead = dummy->next;
    delete dummy;
    return newHead;
}
```

{% note success flat %}
⏱️ **时间复杂度**：O(n) | **空间复杂度**：O(1)
{% endnote %}

---

## 💡 今日总结

### 🌟 核心技巧

| 技巧 | 应用场景 |
|:---|:---|
| **哑节点 (dummy)** | 统一头节点处理，避免边界判断 |
| **双指针 - 归并** | 同步比较 + 尾插 |
| **双指针 - 相交** | 路径交换消除长度差 |
| **双指针 - 定位** | 快慢指针维持固定间距 |

### ✅ 易错点 Checklist

- [ ] 合并时是否正确更新 `current` 指针
- [ ] 相交链表是否处理了无交点情况
- [ ] 删除节点时 `fast` 先走 `n+1` 步（不是 n 步！）
- [ ] 是否正确释放了哑节点内存

### 📋 通用模板

```cpp
// 🎭 哑节点模板
ListNode* dummy = new ListNode(0);
dummy->next = head;
// ... 操作
ListNode* newHead = dummy->next;
delete dummy;
return newHead;
```

---

*今天也在链表的世界里遨游~ (ノ>ω<)ノ* ✨
