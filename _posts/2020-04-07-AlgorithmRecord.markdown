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
算法题目笔记，记录一下子。

## 矩阵 ##

### 旋转矩阵





## 链表 ##

`// Definition for singly-linked list.

  	public class ListNode {

   		int val;

 		  ListNode next;

  		  ListNode(int x) { val = x; }

  	}`

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

`class Solution {

  public ListNode removeNthFromEnd(ListNode head, int n) {

​    ListNode dummy = new ListNode(0);

​    dummy.next = head;

​    ListNode first = dummy;

​    ListNode second = dummy;

​    for(int i = 0; i < n; i++){

​      first = first.next;

​    }



​    while(first.next!= null){

​      first= first.next;

​      second = second.next;

​    }

​    second.next = second.next.next;

​    return dummy.next;

  }



}`























----

2020年4月8日 10点38分

联系我：shentianyu18@163.com









  


