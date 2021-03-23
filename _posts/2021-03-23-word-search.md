---
layout: post
title: 79.单词搜索
date: 2021-03-23
Author: 龙龙
categories: 
tags: [leetcode]
comments: true
---

### 问题要求

给定一个二维网格和一个单词，找出该单词是否存在于网格中。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

**示例**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

给定 word = "ABCCED", 返回 true
给定 word = "SEE", 返回 true
给定 word = "ABCB", 返回 false
```

### 解决思路

**整体思路：**深度优先搜索 + 回溯

**方法**：从 board 的左上角开始寻找，当找到 word 的第一个元素之后，开始往四周搜索。假设当前元素的位置为 （i， j），在进入下一层搜索之前，要标记该元素为已经使用过，即 visited [ i ] [ j ] = true。然后再往四周搜索之后，要进行一个回溯。为啥进行回溯呢？有可能存在其他的路径能够更快地找到答案！

**注意点：**

* 边界值判断，如果当前遍历的 i , j 超过了 board 的边界，或者 （i， j）这个位置已经访问过，或者已经找到了目标单词，或者当前遍历的字符与目标字母对应的值不相同，是不是应该往下寻找了。

* 定义一个全局变量 isFind，在找到了目标单词之后直接设置 isFind = true，这样子其他递归的程序在知道这条信息之后就终止递归了。

### 代码实现

```java
class Solution {
    private boolean isFind;
    public boolean exist(char[][] board, String word) {
        // base case
        if (board == null)  {
            return false;
        }
        // 获取行和列
        int m = board.length;
        int n = board[0].length;
        // 判断当前字符是否又被遍历过
        boolean[][] visited = new boolean[m][n];
        // 初始化 isFind
        isFind = false;
        // 遍历
        for(int i=0;i<m;i++) {
            for(int j=0;j<n;j++) {
                // 从左上角的格子开始遍历整个棋盘
                backTracking(i, j, board, word, visited, 0);
            }
        }
        // 返回结果
        return isFind;
    }
    /**
     * i j board：表示棋盘和当前正在遍历的元素怒
     * word: 表示要遍历的单词
     * visited: 表示当前格子是否已经呗访问过
     * pos：记录目标单词中的字符索引
     *
     * */
    private void backTracking(int i, int j, char[][] board, String word, boolean[][] visited, int pos) {
        // 边界情况
        // 超出边界，已经访问过，已找到目标单词，期盼当中的字符和当前遍历的目标单词的字符不一致，终止寻找
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length || visited[i][j] || isFind || 
            board[i][j] != word.charAt(pos)) {
            return ;
        }
        // 如果当前正在遍历的元素符合条件，而且刚好是最后一个，修改isFind=true，终止递归
        if (pos == word.length() - 1) {
            isFind = true;
            return ;
        }
        // 修改当前节点为已访问状态
        visited[i][j] = true;
        // 深搜
        backTracking(i+1, j, board, word, visited, pos+1);
        backTracking(i-1, j, board, word, visited, pos+1);
        backTracking(i, j-1, board, word, visited, pos+1);
        backTracking(i, j+1, board, word, visited, pos+1);
        // 回溯
        visited[i][j] = false;
    }
}
```



