---
layout: post
title: 剑指 Offer 22. 链表中倒数第k个节点
date: 2021-03-31
Author: 龙龙
categories: 
tags: [剑指offer]
comments: true
---



### 问题要求：

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

 

**示例：**

```bash
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```



 ### 解题思路

* 一个解题思路就是先获取链表长度 **len**，再从头节点开始走 **len - k** 步，返回走到的那个节点即可

* 这道题就是典型的使用快慢指针的方法。定义快慢指针，先让快指针走 k 步，然后再让两个指针同时向后走，当快指针走到底的时候，慢指针就走到了我们想要的位置，返回慢指针即可。

### 代码实现

#### 方式一：

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        // 获取链表长度
        int len = 0;
        ListNode cur = head;
        while(cur != null) {
            len++;
            cur=cur.next;
        }
        // 回到头节点，往后走 len - k 步
        cur = head;
        for(int i=0;i<len-k;i++) {
            cur=cur.next;
        }
        // 返回结果
        return cur;
    }
}
```

#### 方式二：

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        // 定义快慢指针
        ListNode fast = head;
        ListNode slow = head;
        // 先让快指针走 k 步
        while (k-- > 0) {
            fast = fast.next;
        }
        // 两个指针同时走
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        // 返回慢指针
        return slow;
    }
}
```

