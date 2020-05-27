---
layout: post
title:  "阿里校招笔试题复盘1题"
date:   2020/04/28 10点33分        
categories: you
tags: Java
excerpt: 参加了2020.4.28的阿里实习校招笔试，复盘一下
mathjax: true
author: Sky
---

* content
{:toc}
### 阿里骰子走法（笔试实例）

有一个骰子在n×n的矩阵上，翻转行动，初始状态和骰子状态如下图

![12jpg](C:\Users\Sty\Desktop\12jpg.jpg)

文字可以描述为：6朝下，1朝上，2朝右，5朝左，3正对自己，4对面。

所以起点S-翻译为6，向右滚变为2，如此。

**输入描述**:

```
第一行：T  表示 T次实验
第二行：n  表示 n×n的地图（矩阵）
下面n行： n×n的矩阵 和 骰子翻转走过的路线  #为走过的路线   S表示起点   E表示终点
		（路线保证时唯一的）
```

**输出描述:**

```
走过路线时，底面留下的数组，填入矩阵，然后输出
*就相当于吧除了. 之外的字符翻译成数字
```

**示例1**

**输入**

```
1
5
S # # # .
. . . # .
. . . # .
E # . # .
. # # # .
```

**输出**

```
6 2 1 5 .
. . . ? .
. . . ? .
? ? . ? .
. ? ? ? .
```

?是具体的值，我忘了，也懒得推了。



​        

```java
import java.util.Scanner;

public class Main {
	public static void main(String[] args) {
		Scanner scanner = new Scanner(System.in);
		int T = scanner.nextInt();// 找到T
		for (int i = 0; i < T; i++) {
			// 每一次实验，T
			int n = scanner.nextInt();// 找到n
			scanner.nextLine();
			// 获取整个路线矩阵
			char[][] table = new char[n][n];
			for (int a = 0; a < n; a++) {
				String line = scanner.next();
				for (int b = 0; b < n; b++) {
					table[a][b] = line.charAt(b);
				}
			}
			// 定义初始状态
			int si = 0;
			int sj = 0;
			char now = '6';
			char up = '4';
			char down = '3';
			char left = '5';
			char right = '2';
			char oppo = '1';
			// 找到起点S
			for (int a = 0; a < n; a++) {
				for (int b = 0; b < n; b++) {
					if (table[a][b] == 'S') {
						si = a;
						sj = b;
					}
				}
			}
			// 开始填数，直到到达E
			while (table[si][sj] != 'E') {
				table[si][sj] = now;

				if (si + 1 < n && table[si + 1][sj] == '#' ) {
					// turnRight
					si = si + 1;
					char tem = now;
					now = right;
					right = oppo;
					oppo = left;
					left = tem;
					continue;

				}

				if (si - 1>=0 && table[si - 1][sj] == '#' ) {
					// turnLeft
					si = si - 1;
					char tem = now;
					now = left;
					left = oppo;
					oppo = right;
					right = tem;
					continue;
				}
				if (sj - 1>=0&&table[si][sj - 1] == '#' ) {
					// turnUp
					sj = sj - 1;
					char tem = now;
					now = up;
					up = oppo;
					oppo = down;
					down = tem;
					continue;
				}
				if (sj + 1<n&&table[si][sj + 1] == '#' ) {
					// turnDown
					sj = sj + 1;
					char tem = now;
					now = down;
					down = oppo;
					oppo = up;
					up = tem;
					continue;
				}
				if (si + 1 < n && table[si + 1][sj] == 'E' ) {
					// turnRight
					si = si + 1;
					char tem = now;
					now = right;
					right = oppo;
					oppo = left;
					left = tem;
					continue;

				}

				if (si - 1>=0 && table[si - 1][sj] == 'E' ) {
					// turnLeft
					si = si - 1;
					char tem = now;
					now = left;
					left = oppo;
					oppo = right;
					right = tem;
					continue;
				}
				if (sj - 1>=0&&table[si][sj - 1] == 'E' ) {
					// turnUp
					sj = sj - 1;
					char tem = now;
					now = up;
					up = oppo;
					oppo = down;
					down = tem;
					continue;
				}
				if (sj + 1<n&&table[si][sj + 1] == 'E' ) {
					// turnDown
					sj = sj + 1;
					char tem = now;
					now = down;
					down = oppo;
					oppo = up;
					up = tem;
					continue;
				}
				

			}
			// 把E的也输出了
			table[si][sj] = now;

			for (int a = 0; a < n; a++) {
				for (int b = 0; b < n; b++) {
					System.out.print(table[a][b]);
				}
				System.out.println("");
			}

		}

	}
}
```

















2020年4月8日 10点38分

联系我：shentianyu18@163.com









  


