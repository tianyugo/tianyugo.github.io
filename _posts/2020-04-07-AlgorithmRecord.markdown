---
layout: post
title:  "数据结构算法题笔记"
date:   2020/04/08 10点33分        
categories: me 
tags: code
excerpt: leetcode题目笔记
mathjax: true
author: Sky
---

* content
{:toc}

## 前言 ##
算法题目笔记，记录一下子，来自于leetcode。

## 矩阵 ##

### 旋转矩阵
给你一幅由 N × N 矩阵表示的图像，其中每个像素的大小为 4 字节。请你设计一种算法，将图像旋转 90 度。
**不占用额外内存空间能否做到？**
**示例 :**
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],
原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

**思路：**
任意子矩阵/矩阵可以用左上角和右下角坐标唯一确定。根据此思想可以按圈操作旋转。

~~~java
class Solution {
    public void rotate(int[][] matrix) {
        int tR = 0;
        int tC = 0;
        int dR = matrix.length -1;
        int dC = dR;
        while(tR<dR){
            rotateEdge(matrix, tR++, tC++, dR--, dC--);
        }
    }
    
    public void rotateEdge(int[][] matrix, int tR, int tC, int dR, int dC) {
        int times = dR - tR;
        for(int i = 0; i<times;i++){  
            int temp = matrix[dR-i][tC];
            matrix[dR-i][tC] =  matrix[dR][dC-i];
            matrix[dR][dC-i] = matrix[tR+i][dC];
            matrix[tR+i][dC] = matrix[tR][tC+i];
            matrix[tR][tC+i] = temp;
        }

    }

}
~~~

### 机器人的运动范围（dfs）

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

**思路：**简单分析过后，会发现会存在机器人不可达的满足k的区域。

 **各位求和子函数**sumAdd()，如下：

```java
public int sumAdd(int x){
    int s = 0;
    while (x != 0){
        s += x % 10;
        x = x / 10;
    }
    return s;
}
```

 采用暴力递归，越界||不满足k||已访问过：格子0，否则格子+1。

~~~java
public int movingCount(int m, int n, int k) {
    boolean[][] visited = new boolean[m][n];
    return dfs(0,0,m,n,k,visited);
}

  public int dfs(int x,int y,int m, int n, int k, boolean[][] visited){ 
      if( x >= m || y >= n || sumAdd(x)+sumAdd(y) > k || visited[x][y]) return 0;
      visited[x][y] = true;
      return 1+dfs(x+1,y,m,n,k,visited)+dfs(x,y+1,m,n,k,visited)
  }



~~~

 

## 链表 ##

~~~java
// Definition for singly-linked list.
  	public class ListNode {
   		int val;
 		ListNode next;
  		ListNode(int x) { val = x; }
  	}
~~~

### 删除链表倒数第N个节点 ###
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。

**示例：**

给定一个链表: 1->2->3->4->5, 和 n = 2.

当删除了倒数第二个节点后，链表变为 1->2->3->5.

**说明：**

给定的 n 保证是有效的。

**进阶：**

你能尝试使用一趟扫描实现吗？

**思路：**

前提：在链表前添加Dummy节点便于操作。哑结点用来简化某些极端情况，例如列表中只含有一个结点，或需要删除列表的头部。

![](https://pic.leetcode-cn.com/a476f4e932fa4499e22902dcb18edba41feaf9cfe4f17869a90874fbb1fd17f5-file_1555694537876)

方法一：遍历一遍找到最后一个节点，which -> null，求出链表长度L。在遍历一遍，找到L-N的节点。

方法二：两个指针，让一个先走N，那么他们的距离相差为N。然后两个指针同步.Next，先走的到达链尾，后走的就是相距为N的倒数第N个节点。

~~~java
class Solution {
  public ListNode removeNthFromEnd(ListNode head, int n) {
​    ListNode dummy = new ListNode(0);
​    dummy.next = head;
​    ListNode first = dummy;
​    ListNode second = dummy;
​    for(int i = 0; i < n; i++){//这里先走多少步需要扣一下细节
​      first = first.next;
​    }
​    while(first.next!= null){//需要和上面一起抠细节
​      first= first.next;
​      second = second.next;
​    }
​    second.next = second.next.next;
​    return dummy.next;
  }
}
~~~

## 二叉树  ##
### 二叉树的镜像 ###
请完成一个函数，输入一个二叉树，该函数输出它的镜像。**即：**左右子树互换。

**思路：**
很容易想到递归方法，同样也能改成非递归，可以借鉴二叉树先序遍历的非递归的方法。
**递归版：**

~~~java
public TreeNode mirrorTree(TreeNode root) {
        if (root == null) {
            return null;
        } 
        TreeNode tmp = root.left;
        root.left = mirrorTree(root.right);
        root.right = mirrorTree(tmp);
        return root;
     }
~~~
**非递归版：**使用栈，先把头节点压入。循环弹出栈，压入左右子节点，并互换。

~~~java
    public TreeNode mirrorTree(TreeNode root) {
        if (root == null ) return null;
      Stack<TreeNode> stack = new Stack<>();
      stack.add(root);
      while(!stack.isEmpty()){
          TreeNode curr = stack.pop();
          if(curr.left!=null) stack.add(curr.left);
          if(curr.right!=null) stack.add(curr.right);
          TreeNode tmp = curr.left;
          curr.left = curr.right;
          curr.right = tmp;
      }
      return root;
    }
~~~





## 递归与动态规划

### 括号生成

数字 *n* 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 **有效的** 括号组合

**示例：**

​	**输入：**n = 3
​	**输出**：[
​       "((()))",
​       "(()())",
​       "(())()",
​       "()(())",
​       "()()()"
​     ]



**思路：**考虑当前左右括号数，都为零则说明已生成有效括号组合：加入输出中。若左括号数>0，则还可以加入左括号，

~~~java
List<String> res = new ArrayList<>();
  public List<String> generateParenthesis(int n) {
​    dfs(n,n,"");
​    return res;
  }
  public void dfs(int left, int right, String cur){
​    if(left == 0 && right == 0){// 左右括号都不剩余了，递归终止
​      res.add(cur);
​      return;
​    }
​    if(left > 0){// 如果左括号还剩余的话，可以拼接左括号
​      dfs(left-1, right, cur+"(");
​    }
​    if(left < right){// 如果右括号剩余多于左括号剩余的话，可以拼接右括号
​      dfs(left, right-1, cur+")");
​    }
  }
~~~

  

## 常用子程序

### 各位求和

~~~java
public int sumAdd(int x){
    int s = 0;
    while (x != 0){
        s += x % 10;
        x = x / 10;
    }
    return s;
}
~~~







----

2020年4月8日 10点38分

联系我：shentianyu18@163.com









  


