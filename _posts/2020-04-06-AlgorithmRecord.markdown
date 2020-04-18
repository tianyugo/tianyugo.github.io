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

## 字符串

### [翻转字符串里的单词](https://leetcode-cn.com/problems/reverse-words-in-a-string/)

给定一个字符串，逐个翻转字符串中的每个单词。

**示例 2：**

输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以**在前面或者后面包含多余的空格**，但是反转后的字符不能包括。

**示例 ：**

输入: "a good   example"
输出: "example good a"
解释: 如果**两个单词间有多余的空格**，将反转后单词间的空格减少到只含一个。

~~~java
public String reverseWords(String s){
    // 除去开头和末尾的空白字符    
    s = s.trim();// Java String无法修改，.strim()生成的是新字符串，并不是修改s。
    //分割字符串
    List<String> wordList = Arrays.asList(s.split("\\s+"));// 正则匹配连续的空白字符作为分隔符分割
    Collections.reverse(wordList);// 反转 list，所以需要将上述String数组，转换为List
    return String.join(" ", wordList);//jdk 1.8 以后，新API
}

~~~

**说明：**

~~~java
//.split()用法	
     String str = "you can you up";
	 String[] str1 = str.split(" ");
	     .split("\\s+");
//Collections.reverse(List<?> list ) 反转list集合
	Collections.reverse(wordList);
~~~

### 字符串转换整数 (atoi)

**方法一**：字符串转换

考验硬功底，底子不硬照抄来的。

~~~java
class Solution {
    public int myAtoi(String str) {
        str = str.trim();
        if (str.length() == 0) return 0;
        if (!Character.isDigit(str.charAt(0))
            && str.charAt(0) != '-' && str.charAt(0) != '+')
            return 0;
        long ans = 0L;
        boolean neg = str.charAt(0) == '-';
        int i = !Character.isDigit(str.charAt(0)) ? 1 : 0;
        while (i < str.length() && Character.isDigit(str.charAt(i))) {
            ans = ans * 10 + (str.charAt(i++) - '0');
            if (!neg && ans > Integer.MAX_VALUE) {
                ans = Integer.MAX_VALUE;
                break;
            }
            if (neg && ans > 1L + Integer.MAX_VALUE) {
                ans = 1L + Integer.MAX_VALUE;
                break;
            }
        }
        return neg ? (int) -ans : (int) ans;
    }
}
~~~

**方法二**：自动机

概念：我们的程序在每个时刻有一个状态 s，每次从序列中输入一个字符 c，并根据字符 c 转移到下一个状态 s'。这样，我们只需要建立一个覆盖所有情况的从 s 与 c 映射到 s' 的表格即可解决题目中的问题。

可以建立如下图所示的自动机：

![fig1](https://assets.leetcode-cn.com/solution-static/8_fig1.PNG)

也可以用下面的表格来表示这个自动机：

' '	+/-	number	other
start	start	signed	in_number	end
signed	end	end	in_number	end
in_number	end	end	in_number	end
end	end	end	end	end

## 二叉树  ##

~~~java

 // Definition for a binary tree node.
  public class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;
    TreeNode(int x) { val = x; }
  }
 
~~~

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



### 二叉树的层序遍历

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

例如:
给定二叉树: [3,9,20,null,null,15,7],

```java
  3
 / \
9  20
   /  \
  15   7
返回：
   [3,9,20,15,7]
```
**思路：**队列实现，将树的节点放入队列中，弹出时打印值并将左右子节点（检查是否存在）加入队列。队列先进先出good！！

~~~java
 public int[] levelOrder(TreeNode root) {
        Queue<TreeNode> que = new LinkedList<>();//队列实现使用双向链表
        ArrayList<Integer> arr = new ArrayList<>();
        if(root!=null){
            que.add(root);  
        }else{    	//别忘了处理空树
            return new int[]{};
            }
        while(!que.isEmpty()){
            TreeNode cur = que.poll();//队列弹出头部推荐用poll，不会有异常
            arr.add(cur.val);
            if(cur.left != null) que.offer(cur.left);//队列加入用offer
            if(cur.right != null) que.offer(cur.right);
        }						//使用.size
        int[] ans = new int[arr.size()]; //ArrayList→Array 输出结果 
        for (int i =0 ; i < arr.size(); i++){
            ans[i] = arr.get(i);
        }
        return ans;
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

  

## 其他

### 有效的括号（遍历字符串）

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

1	左括号必须用相同类型的右括号闭合。
2	左括号必须以正确的顺序闭合。

*注意空字符串可被认为是有效字符串。

**思路：**利用栈结构，遇到左括号加入栈，遇到右括号弹出栈顶并检查是否是一组。检查是否是一组的对应关系可用HashMap的K-V关系表示。但也可以不用map结构，思路如下：遍历字符串：遇到左括号，压入对应的右括号；遇到右括号，检查栈顶是否是相同的，相同则检查字符串中下一个字符，不相同或者栈空了，返回false。遍历完字符串最后检查栈，为空则true，否则false。

~~~java
public boolean isValid(String s) {
        Stack<Character> stack = new Stack<>();
        for (Character cur : s.toCharArray()){
            if(cur =='('){
                stack.push(')');
            }else if(cur == '{'){
                stack.push('}');
            }else if(cur == '['){
                stack.push(']');
            }else if(stack.empty() || cur != stack.pop()){
                return false;
            }
        }
        if(stack.empty())  return true;
        return false;     
    }
~~~

### 合并区间: [1,3]+[2,6]=[1,6]

给出一个区间的集合，请合并所有重叠的区间。

示例 1:

输入: [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:

输入: [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。

~~~java
   public int[][] merge(int[][] intervals) {
        // 先按照区间起始位置排序
        Arrays.sort(intervals, (v1, v2) -> v1[0] - v2[0]);
        // 遍历区间
        int[][] res = new int[intervals.length][2];
        int idx = -1;
        for (int[] interval: intervals) {
            // 如果结果数组是空的，或者当前区间的起始位置 > 结果数组中最后区间的终止位置，
            // 则不合并，直接将当前区间加入结果数组。
            if (idx == -1 || interval[0] > res[idx][1]) {
                res[++idx] = interval;
            } else {
                // 反之将当前区间合并至结果数组的最后区间
                res[idx][1] = Math.max(res[idx][1], interval[1]);
            }
        }
        return Arrays.copyOf(res, idx + 1);
    }
~~~





## 常用API

### I/O

首行数字N，N行String

2
helloo
wooooooow

##### BufferedReade

~~~java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(bf.readLine());//Integer.parseInt(),将括号内容转换为int
        while ((n--) > 0) {
            String s = bf.readLine();
            char[] array = s.toCharArray();
~~~

##### Scanner

~~~java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int line = scanner.nextInt();
        scanner.nextLine();//warning 需要删除首行int 末尾的换行符
        for (int i = 0; i < line; i++) {
            System.out.println(scanner.nextLine().replaceAll("(.)\\1+","$1$1").replaceAll("(.)\\1(.)\\2","$1$1$2"));
        }
    }
}
~~~





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

### 字符串相关

#### String

1. 除去开头和末尾的空白字符 ——s = s.trim();

~~~java
s = s.trim();// Java String无法修改，.strim()生成的是新字符串，并不是修改s。
~~~

2. 分割字符串——String[] str1 = s.split(" ");

~~~java
String str = "you can you up";
String[] str1 =str.split(" ")
~~~

2.*若存在多个空格符——String[] str2 = str.split("\\s+");

~~~java
String str = "you      can you up";
String[] str2 = str.split("\\s+");
	     
~~~

3.字符串变成整型——int a = Integer.valueOf(str);

~~~java
String str="888";
int a=Integer.valueOf(str)
~~~

4.***变成字符串（String.valueOf）——

~~~java
char c[]={'a','b','c','d','e','f'};
int n=2011;
String s1=String.valueOf(c);    //字符或字符数组均可转换
String s2=String.valueOf(c,2,4);
String s3=String.valueOf(n);
~~~

xx对象.toString();必须先创建对象，再调用对象的toString()方法

String.valueOf(XX对象):静态方法，不需要创建任何对象，就可以直接调用

大多数valueOf方法调用的都是toString()方法，建议大家用valueOf方法，因为valueOf在没有对象也可以用，可以避免空指针异常

#### StringBuilder

[StringBuilder的常用方法](https://www.cnblogs.com/jack-Leo/p/6684447.html)

```java
StringBuilder sb = new StringBuilder();
sb.append(array[0]);
sb.toString()//转换为str
```



### 数据结构相关

#### 栈Stack ：Stack

~~~java
//初始化
   Stack stack=new Stack();
//判断是否为空
   stack.empty();
//取栈顶值（不出栈）
   stack.peek();
//进栈
   stack.push(Object);
//出栈
   stack.pop();
~~~

#### 队列Queue： LinkedList

```java
//1.初始化
	Queue<TreeNode> queue = new LinkedList<>(){{ add(root); }};
//2.添加元素
    queue.offer("a");//add
//3.删除第一个
    queue.poll();//remove
//4.返回头部
    queue.peek();//element
```

#### 动态数组ArrayList：ArrayList

```java
ArrayList<Integer> ans = new ArrayList<>();
ans.add();
ans.get();
size() : 获取集合长度，通过定义在ArrayList中的私有变量size得到
isEmpty()：是否为空，通过定义在ArrayList中的私有变量size得到
contains(Object o)：是否包含某个元素，通过遍历底层数组elementData，通过equals或==进行判断
 
Object[] array = arrayList.toArray();
String[] array = (String[])ans.toArray(new String[size]);  

```

#### 数组Array: int[] 

~~~java
int[] ans = new int[n];
//
 List<String> list=Arrays.asList(array); 
~~~

#### 哈希表HashMap







###  [Collections 工具类和 Arrays 工具类常见方法](https://gitee.com/SnailClimb/JavaGuide/blob/master/docs/java/basic/Arrays,CollectionsCommonMethods.md)

####  Collections

#####  排序操作

```java
void reverse(List list)//反转
void shuffle(List list)//随机排序
void sort(List list)//按自然排序的升序排序
void sort(List list, Comparator c)//定制排序，由Comparator控制排序逻辑
void swap(List list, int i , int j)//交换两个索引位置的元素
void rotate(List list, int distance)//旋转。当distance为正数时，将list后distance个元素整体移到前面。当distance为负数时，将 list的前distance个元素整体移到后面。
```

```java
// 定制排序的用法
		Collections.sort(arrayList, new Comparator<Integer>() {

			@Override
			public int compare(Integer o1, Integer o2) {
				return o2.compareTo(o1);
			}
		});
```



#####  查找,替换操作

```java
int binarySearch(List list, Object key)//对List进行二分查找，返回索引，注意List必须是有序的
int max(Collection coll)//根据元素的自然顺序，返回最大的元素。 类比int min(Collection coll)
int max(Collection coll, Comparator c)//根据定制排序，返回最大元素，排序规则由Comparatator类控制。类比int min(Collection coll, Comparator c)
void fill(List list, Object obj)//用指定的元素代替指定list中的所有元素。
int frequency(Collection c, Object o)//统计元素出现次数
int indexOfSubList(List list, List target)//统计target在list中第一次出现的索引，找不到则返回-1，类比int lastIndexOfSubList(List source, list target).
boolean replaceAll(List list, Object oldVal, Object newVal), 用新元素替换旧元素
```

----

####  Arrays类的常见操作

##### 排序 :  Arrays.sort(array)

~~~java

Arrays.sort(a);
	Arrays.sort(b, 2, 6);
Arrays.parallelSort(c);


~~~

##### 查找 : Arrays.binarySearch(array，？)

```java
	// *************查找 binarySearch()****************
	char[] e = { 'a', 'f', 'b', 'c', 'e', 'A', 'C', 'B' };
	// 排序后再进行二分查找，否则找不到
	Arrays.sort(e);
	System.out.println("Arrays.sort(e)" + Arrays.toString(e));
	System.out.println("Arrays.binarySearch(e, 'c')：");
	int s = Arrays.binarySearch(e, 'c');
	System.out.println("字符c在数组的位置：" + s);
```
#####  比较: Arrays.equals(arr,arr)

```java
		// *************比较 equals****************
		char[] e = { 'a', 'f', 'b', 'c', 'e', 'A', 'C', 'B' };
		char[] f = { 'a', 'f', 'b', 'c', 'e', 'A', 'C', 'B' };
		/*
		* 元素数量相同，并且相同位置的元素相同。 另外，如果两个数组引用都是null，则它们被认为是相等的 。
		*/
		// 输出true
		System.out.println("Arrays.equals(e, f):" + Arrays.equals(e, f));
```

#####  填充 : Arrays.fill(arr,?)

```java
		int[] h = { 1, 2, 3, 3, 3, 3, 6, 6, 6, };
		// 数组中指定范围元素重新分配值
		Arrays.fill(h, 0, 2, 9);//[0-2]  9
		System.out.println("Arrays.fill(h, 0, 2, 9);：");
		// 输出结果：993333666
		for (int i : h) {
			System.out.print(i);
		}
```

#####  转字符串:Arrays.toString(k)

```java
		// *************转字符串 toString()****************
		/*
		* 返回指定数组的内容的字符串表示形式。
		*/
		char[] k = { 'a', 'f', 'b', 'c', 'e', 'A', 'C', 'B' };
		System.out.println(Arrays.toString(k));// [a, f, b, c, e, A, C, B]
```

#####  复制: Arrays.copyOf(arr,n)

```java
		// *************复制 copy****************
		// copyOf 方法实现数组复制,h为数组，6为复制的长度
		int[] h = { 1, 2, 3, 3, 3, 3, 6, 6, 6, };
		int i[] = Arrays.copyOf(h, 6);
		System.out.println("Arrays.copyOf(h, 6);：");
		// 输出结果：123333
		for (int j : i) {
			System.out.print(j);
		}
```











2020年4月8日 10点38分

联系我：shentianyu18@163.com









  


