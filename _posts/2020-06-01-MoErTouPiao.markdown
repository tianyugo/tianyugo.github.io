---
layout: post
title:  "摩尔投票：求众数（n/2；n/3）"
date:   2020/06/01 21点20分       
categories: me
tags: Java
excerpt: 从求众数学习摩尔投票方法
mathjax: true
author: Sky

---

* content
{:toc}


# 摩尔投票

## [169. 多数元素](https://leetcode-cn.com/problems/majority-element/)

给定一个大小为 $ n $ 的数组，找到其中的多数元素。多数元素是指在数组中出现次数大于$ ⌊ n/2 ⌋ $的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1:**

```
输入: [3,2,3]
输出: [3]
```

**示例 2:**

```
输入: [1,1,1,3,3,2,2,2]
输出: [1,2]
```

### 暴力

遍历整个数组，利用`HashMap`，其中`key`为数值，`value`为出现次数；
接着遍历`HashMap`中的每个`Entry`，寻找`value > nums.length / 2`的`key`即可。

### 排序

排完序后，数组中间的元素总是“多数元素”
例：`1 1 1 2 3`；`0 1 1 1 2`；`-1 0 1 1 1`

### 正片

抵消思路，每次删除不同的两票。

具体实现：选定一个‘候选人’A，记录他的票数，如果遇到不是同一个人选 B，给A减1票，然后看下一个投票：相当于删了A，B两个不同的票。遇到当前候选人0票，候选人换人。



~~~java
class Solution {
    public int majorityElement(int[] nums) {
		int num = nums[0];//先以第一个人为候选人
		int count = 1;//记票数为1
		for (int i = 1; i < nums.length; i++) {//从第二个人开始遍历
			if (nums[i] == num) {//同一人加票
				count++;
			} else {//不是同一个，减票
				count = count == 0 ? 1 : count--;
				num = count == 0 ? nums[i] : num;
			}
		}
		return num;
	}
}
~~~





## [229. 求众数 IIB](https://leetcode-cn.com/problems/majority-element-ii/)

给定一个大小为 $ n $ 的数组，找出其中所有出现超过 $ ⌊ n/3 ⌋ $ 次的元素。

**说明:** 要求算法的时间复杂度为  $ O(n)$，空间复杂度为   $O(1)$。

### Map记录+遍历

~~~java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        int n = nums.length;
        int up = n/3;
        Map<Integer, Integer>  map = new HashMap<>();
        Integer size = 0;
        for(int i = 0 ;i<n;i++){
            size = map.get(nums[i]);
            if(size == null){
                map.put(nums[i], 1);
            }else{
                map.replace(nums[i], size+1);
            }
        }

        List<Integer> list = new ArrayList<>();
        for(Map.Entry<Integer,Integer> entry : map.entrySet()){
            if(entry.getValue() > up){
                 list.add(entry.getKey());
             }
        }
        return list;
    }
}
~~~



### 改进的摩尔投票



~~~java
class Solution {
    public List<Integer> majorityElement(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        if (nums == null || nums.length == 0)
        return ans;
    
		int first = nums[0];
		int n = nums.length;
		int num1 = first, num2 = first;
		int count1 = 0, count2 = 0;
		for (int i = 0; i < n; i++) {
			if (nums[i] == num1) {//同1，给1加票
				count1++;
			} else if (nums[i] == num2) {//同2，给2加票
				count2++;
			} else {//都不同：看看谁没票，换人；都有票，都减票（实现了“消除三个不同的票”）
				if (count1 == 0) {
					count1 = 1;
					num1 = nums[i];
				} else if (count2 == 0) {
					count2 = 1;
					num2 = nums[i];
				} else {
					count1--;
					count2--;
				}

			}
		}
		// 检查num1和num2	
		count1 = 0;
		count2 = 0;
		for (int num : nums) {
			if (num1 == num)
				count1++;
			else if (num2 == num)
				count2++;
		}
		if (count1 > nums.length / 3)
			ans.add(num1);
		if (count2 > nums.length / 3)
			ans.add(num2);
		return ans;
	}

}
~~~

