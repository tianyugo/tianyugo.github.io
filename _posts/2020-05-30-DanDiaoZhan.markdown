---
layout: post
title:  "单调栈：从柱状图中最大的矩形开始"
date:   2020/04/28 10点33分        
categories: me
tags: Java
excerpt: 参加了2020.4.28的阿里实习校招笔试，复盘一下
mathjax: true
author: Sky

---

* content
{:toc}


### 从最大矩形开始单调栈

#### [柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

##### 1.暴力

~~~java
	public static int largestRectangleArea(int[] heights) {
		int len = heights.length;
		int ans = 0;
		for (int i = 0; i < len; i++) {
			// 保存当前高度
			int high = heights[i];
			// 找到不小于当前高度的最左
			int left = i;
			for (int j = i - 1; j >= 0; j--) {
				if (heights[j] < high) {
					break;
				}
				left = j;
			}
			// 找到不小于当前高度的最右
			int right = i;
			for (int j = i + 1; j < len; j++) {
				if (heights[j] < high) {
					break;
				}
				right = j;
			}
			// 计算宽度
			int width = right - left + 1;
			// 保存面积最大值
			ans = Math.max(ans, width * high);
		}
		return ans;
	}
~~~

##### 2.单调栈

~~~java
	public static int largestRectangleArea2(int[] heights) {
		int len = heights.length;
		if (len == 0) {
			return 0;
		}
		if (len == 1) {
			return heights[0];
		}
		Stack<Integer> stack = new Stack<>();
		int area = 0;
		for (int i = 0; i < len; i++) {
			while (!stack.isEmpty() && heights[stack.peek()] > heights[i]) {
				int high = heights[stack.pop()];
				int width;
				if (stack.isEmpty()) {
					width = i;
				} else {
					width = i - stack.peek() - 1;
				}
				area = Math.max(area, high * width);
			}

			stack.add(i);
		}
		while (!stack.isEmpty()) {
			int high = heights[stack.pop()];
			int width;
			if (stack.isEmpty()) {
				width = len;
			} else {
				width = len - stack.peek() - 1;
			}
			area = Math.max(area, high * width);

		}
		return area;

	}
~~~

##### 3.单调栈+哨兵优化

相当于首尾加 0.

~~~java
	public static int largestRectangleArea3(int[] heights) {
		int len = heights.length;
		if (len == 0) {
			return 0;
		}
		if (len == 1) {
			return heights[0];
		}
		int[] newheights = new int[len + 2];
		for (int i = 0; i < len; i++) {
			newheights[i + 1] = heights[i];
		}
		len = len + 2;
		heights = newheights;
		Stack<Integer> stack = new Stack<>();
		stack.add(0);
		int area = 0;
		for (int i = 1; i < len; i++) {
			while (heights[stack.peek()] > heights[i]) {
				int high = heights[stack.pop()];
				int width = i - stack.peek() - 1;

				area = Math.max(area, high * width);
			}

			stack.add(i);
		}

		return area;

	}
~~~







  

