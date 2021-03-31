---
layout: post
title: 17. 打印从1到最大的n位数
date: 2021-03-27
Author: 龙龙
categories: 
tags: [剑指Offer]
comments: true
---



### 问题要求

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

**示例 1:**

```
输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
```

说明：

- 用返回一个整数列表来代替打印
- n 为正整数

### 解题思路

题目要求打印“从 1 到最大的 n 位数的列表”，所以就考虑以下问题。

1. **最大的 n 位数（记为 end ）和位数 n 的关系。**例如最大的一位数是 9，最大的两位数是 99，最大的三位数是 999，所以可以得出公式：
   $$
   end = 10 ^ n - 1
   $$
   

2. **大数越界问题。**假如问题所给的 n 是 100，长度为100的数，这都超出了 long 的取值范围了。但是本道题的要求返回 int 类型数组，所以暂时不考虑大数越界问题。

因此，本道题的解法就是，定义区间[1, 10^n-1] 和 步长 1，通过 for 循环生成就好了

**代码实现如下：**

```java
class Solution {
    public int[] printNumbers(int n) {
        // 数组长度
        int len = (int) Math.pow(10, n) - 1;
        // 保存结果的数组
        int[] res = new int[len];
        // 每个值都等于下标+1
        for (int i = 0; i < res.length; i++) {
            res[i] = i + 1;
        }
        // 返回结果
        return res;
    }
}
```

但是，如果要考虑大数问题呢？加入问题要求的返回一个 string 类型的呢？是不是就要考虑大数问题了？

如果返回的是 string 类型的话，我们就无法通过 +-*/ 这些操作来进行数字的运算了？该怎么实现达到我们的要求呢？**答案是：递归生成全排列 !**

* 基于分治的思想，先固定首位，再向后面的位数递归。当首位被固定时，添加数字的字符串。例如当 n = 2 时，数字范围是不是 1 - 99。固定十位为 0 - 9，再按顺序依次往下递归，固定各位为0 - 9，终止递归并添加数字字符串。

代码实现如下：

```java
class Solution {
    StringBuffer res;
    public String printNumbers_1(int n) {
        // 获取长度
        int len = (int) Math.pow(10, n) - 1;
        // 创建数组
        res = new StringBuffer();
        for (int digits = 1; digits <= n; digits++) {
            // digits 表示数的位数
            for (char first = '1'; first <= '9'; first++) {
                char[] num = new char[digits];
                num[0] = first;
                dfs(1, num, digits);
            }
        }
        return res.toString();
    }
    private void dfs(int index, char[] num, int digits) {
        // 递归终止条件
        if (index == digits) {
            res.append(String.valueOf(num)+",");
            return;
        }
        for (char i = '0'; i <= '9'; i++) {
            num[index] = i;
            dfs(index + 1, num, digits);
        }
    }
}
```

在long 范围内的数字加减乘除都不难，但是一旦超出了范围，一般来讲都是通过字符串来进行操作，挺有意思的一道题。说简单也简单，说难也能难。
