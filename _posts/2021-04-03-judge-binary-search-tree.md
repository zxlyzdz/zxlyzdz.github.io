---
layout: post
title: 33. 二叉搜索树的后序遍历序列
date: 2021-04-03
Author: 龙龙
categories:
tags: [剑指offer]
comments: true
---

### 问题要求

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

 参考以下这颗二叉搜索树：

```bash
     5
    / \
   2   6
  / \
 1   3
```

**示例 1：**

```
输入: [1,6,3,2,5]
输出: false
```

**示例 2：**

```
输入: [1,3,2,6,5]
输出: true
```

### 解题思路

解决这道题，得先知道二叉搜索树的性质：

* 左子树的值都不大于根节点的值
* 右子树的值都不小于根节点的值

所以一般验证一棵树是否是二叉搜索树：采用中序遍历！判断是否是递增序列就好了。

当然，这道题也可以采用这个思路。虽然这道题给的是后序遍历

步骤:

1. 每次都找到这棵子树的根节点
2. 划分它的左右子树
3. 递归，递归终止条件是：当左端点的值大于等于右端点，说明遍历的最多只有一个节点

如何划分左右子树？

因为中序遍历的顺序是

```bash
[左子树] [根] [右子树]
```

而后序遍历的顺序是

```bash
[左子树] [右子树] [根] 
```

而且因为二叉搜索树的性质，左子树的值肯定是不大于这个根节点的值的，所以在后序遍历的序列中，从左端点定义一个指针 *k*，找到第一个大于根节点的值的位置，从这个位置往左是不是就是左子树的区间了。那从这个位置开始往右，直到遍历到根节点，这个区间是不是就是右子树的区间了。但是如果在一轮遍历之后，指针 *k* 并没有走到根节点的位置，是不是就说明这棵树就不满足二叉搜索树的性质了！

### 代码实现

```java
class Solution {
    public boolean verifyPostorder(int[] postorder) {
        return verify(postorder, 0, postorder.length - 1);
    }

    private boolean verify(int[] postorder, int left, int right) {
        // 如果 left >= right，说明只剩下一个值，返回真
        if (left >= right) return true;
        // 获取树的根节点的值
        int root = postorder[right];
        // 遍历数组用的
        int k = left;
        // 找左子树的区间
        while (postorder[k] < root && k <= right) k++;
        // 找到的左子树的范围
        int ll = left, lr = k - 1;
        // 开始找右子树
        while (postorder[k] > root && k <= right) k++;
        int rl = lr + 1, rr = k - 1;
        // k == right 是用来判断 k 有没有遍历到最后一个
        return k == right && verify(postorder, ll, lr) && verify(postorder, rl, rr);
    }
}
```

