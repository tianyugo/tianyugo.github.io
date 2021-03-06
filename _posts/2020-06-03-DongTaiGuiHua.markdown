---
layout: post
title:  "动态规划学习"
date:   2020/06/04 21点20分       
categories: me
tags: Java
excerpt: 记录动态规划的学习和例题
mathjax: true
author: Sky

---

* content
{:toc}


# 动态规划

## [837. 新21点](https://leetcode-cn.com/problems/new-21-game/)-后往前

爱丽丝参与一个大致基于纸牌游戏 “21点” 规则的游戏，描述如下：

爱丽丝以 0 分开始，并在她的得分少于 K 分时抽取数字。 抽取时，她从 [1, W] 的范围中随机获得一个整数作为分数进行累计，其中 W 是整数。 每次抽取都是独立的，其结果具有相同的概率。

当爱丽丝获得不少于 `K` 分时，她就停止抽取数字。 爱丽丝的分数不超过 `N` 的概率是多少？

$dp[x]$表示当前 $x$ 分时，抽取数组后分数不超过 $N$ 的概率
$$
\begin{array}{*{20}{c}}
{\rm{0}}& \cdots &K& \cdots &N&{}&{}\\
x&x&1&1&1&0&0
\end{array}
$$



$$
dp[x] = \frac{{dp[x + 1] + dp[x + 2] +  \cdots  + dp[x + W]}}{W}
$$


~~~java
class Solution {
    public double new21Game(int N, int K, int W) {
        double[] dp = new double[K+W];

        for(int i = K;i<K+W&&i<=N;i++){
            dp[i] =1.0;
        }

        for(int i =K-1;i>=0;i--){
            double sum=0;
            for(int j =i+1;j<=i+W;j++){
                sum += dp[j];
            }
            dp[i] = sum/W;
        }

        return dp[0];
    }

}
~~~



考虑对 $ dp$ 的相邻项计算差分，有如下结果：

$$
dp[x] - dp[x+1]=\frac{dp[x+1] - dp[x+W+1]}{W}
$$

其中 $ 0≤x<K−1$。因此可以得到新的状态转移方程：

$$
dp[x]=dp[x+1]-\frac{dp[x+W+1]-dp[x+1]}{W}
$$
其中 $0≤x<K−1$。注意到上述状态转移方程中 $ x$  的取值范围，当 $x=K−1$ 时不适用。因此对于$dp[K−1] $的值，需要通过
$$
dp[K-1]=\frac{dp[K]+dp[K+1]+\cdots+dp[K+W-1]}{W}
$$
计算得到。注意到只有当 $K≤x≤min(N,K+W−1) $ 时才有 $dp[x]=1$，因此
$$
dp[K-1]=\frac{\min(N, K+W-1) - K + 1}{W} = \frac{\min(N-K+1,W)}{W}
$$
可在 $O(1)$ 时间内计算得到  $dp[K−1]$ 的值

~~~java
class Solution {
    public double new21Game(int N, int K, int W) {
        if (K == 0) {
            return 1.0;
        } 
        double[] dp = new double[K+W];


        for(int i = K;i<K+W&&i<=N;i++){
            dp[i] =1.0;
        }
        dp[K-1] = 1.0 * (Math.min(N - K + 1, W)) / W;//转换成double*1.0

        for(int i =K-2;i>=0;i--){       
            dp[i] = dp[i+1]-(dp[i + W + 1] - dp[i + 1]) / W;  
        }

        return dp[0];
    }

}
~~~





## [983. 最低票价](https://leetcode-cn.com/problems/minimum-cost-for-tickets/)-后往前

在一个火车旅行很受欢迎的国度，你提前一年计划了一些火车旅行。在接下来的一年里，你要旅行的日子将以一个名为 days 的数组给出。每一项是一个从 1 到 365 的整数。

火车票有三种不同的销售方式：

1. 一张为期一天的通行证售价为 costs[0] 美元；
2. 一张为期七天的通行证售价为 costs[1] 美元；
3. 一张为期三十天的通行证售价为 costs[2] 美元。

通行证允许数天无限制的旅行。 例如，如果我们在第 2 天获得一张为期 7 天的通行证，那么我们可以连着旅行 7 天：第 2 天、第 3 天、第 4 天、第 5 天、第 6 天、第 7 天和第 8 天。

返回你想要完成在给定的列表 days 中列出的每一天的旅行所需要的最低消费。

money1+dp[x+1]

money7+dp[x+7]

money30+dp[x+30]



~~~java
class Solution {
    public int mincostTickets(int[] days, int[] costs) {
        int len = days.length;
        int[] dp = new int[len];

        dp[len-1] = Math.min(costs[0],Math.min(costs[1], costs[2]));
        
        int i = len -2;
        while(i>=0){
            int  day  = days[i];
            int day7 = day +7-1;
            int day30 = day +30-1;
            
            //算一下7天后，和30天后有没有超范围
            int l7 = 0;
            int l30 = 0;
            while(i+l7 < len &&  days[i+l7] <= day7){
                l7++;
            }
            while(i+l30 < len &&  days[i+l30] <= day30){
                l30++;
            }
            

            int a = costs[0]+dp[i+1];
            int b = costs[1];
            if(day7 < days[len-1] ){
                b = costs[1]+dp[i+l7];
            }
            int c = costs[2];
            if(day30 < days[len-1] ){
                c = costs[2]+dp[i+l30];
            }

            dp[i] = Math.min(c,Math.min(a, b));

            i--;
        }
        return dp[0];
    }
}
~~~



## [面试题46. 把数字翻译成字符串](https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof/)

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

思路可参考走楼梯！走楼梯一定可以走1走2，这题根据条件走1或走2

~~~java
class Solution {
	public int translateNum(int num) {
		String numString = Integer.toString(num);
		if (numString.length() == 1) {
			return 1;
		}
		int[] dp = new int[numString.length()];
		dp[0] = 1;
		int curChar = numString.charAt(1) - '0';
		int preChar = numString.charAt(0) - '0';
		int curnum = preChar * 10 + curChar;
		if (curnum >= 10 && curnum <= 25) {
			dp[1] = 2;
		} else {
			dp[1] = 1;
		}
		for (int i = 2; i < dp.length; i++) {
			curChar = numString.charAt(i) - '0';
			preChar = numString.charAt(i - 1) - '0';
			curnum = preChar * 10 + curChar;
			if (curnum >= 10 && curnum <= 25) {//能翻译
				dp[i] = dp[i - 1] + dp[i - 2];
			} else {
				dp[i] = dp[i - 1];
			}
		}
		return dp[numString.length() - 1];
	}
}
~~~

## [面试题 17.13. 恢复空格](https://leetcode-cn.com/problems/re-space-lcci/)

```
哦，不！你不小心把一个长篇文章中的空格、标点都删掉了，并且大写也弄成了小写。像句子"I reset the computer. It still didn’t boot!"已经变成了"iresetthecomputeritstilldidntboot"。在处理标点符号和大小写之前，你得先把它断成词语。当然了，你有一本厚厚的词典dictionary，不过，有些词没在词典里。假设文章用sentence表示，设计一个算法，把文章断开，要求未识别的字符最少，返回未识别的字符数。 
```

**示例：**

~~~
输入：
dictionary = ["looked","just","like","her","brother"]
sentence = "jesslookedjustliketimherbrother"
输出： 7
解释： 断句后为"jess looked just like tim her brother"，共7个未识别字符。
~~~



~~~java
class Solution {
    public int respace(String[] dictionary, String sentence) {
        HashSet<String> dic = new HashSet<>(Arrays.asList(dictionary));
        int[] dp = new int[sentence.length()+1];
        for (int i = 1; i < dp.length; i++) {
            dp[i] = dp[i - 1] + 1;
            for (int j = 0; j < i; j++) {
                if(dic.contains(sentence.substring(j,i))){
                    dp[i] = Math.min(dp[i],dp[j]);
                }
            }
        }
        return dp[sentence.length()];
    }
}
~~~



## [120. 三角形最小路径和](https://leetcode-cn.com/problems/triangle/)：经典

```
给定一个三角形，找出自顶向下的最小路径和。每一步只能移动到下一行中相邻的结点上。 

相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。 



 例如，给定三角形： 

 [
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]


 自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。 



 说明： 

 如果你可以只使用 O(n) 的额外空间（n 为三角形的总行数）来解决这个问题，那么你的算法会很加分。 
 Related Topics 数组 动态规划
```

 




```java
import java.util.List;

class Solution {

//    public int minimumTotal(List<List<Integer>> triangle) {
//        int n = triangle.size();
//        int[][] dp = new int[n][n];
//
//        //为最后一行赋值
//        for (int i = 0; i < n; i++) {
//            dp[n - 1][i] = triangle.get(n - 1).get(i);
//        }
//        for (int i = n - 2; i >= 0; i--) {
//            for (int j = 0; j <= i; j++) {
//                dp[i][j] = triangle.get(i).get(j) + Math.min(dp[i + 1][j], dp[i + 1][j + 1]);
//            }
//        }
//
//        return dp[0][0];
//    }



    //代码优化 dp数组多赋值一行，省略给最后一行单独赋值

//    int[][] dp = new int[n + 1][n + 1];
//
//        for (int i = n - 1; i >= 0; i--) {
//        for (int j = 0; j <= i; j++) {
//            dp[i][j] = Math.min(dp[i + 1][j], dp[i + 1][j + 1]) + triangle.get(i).get(j);
//        }
//    }

    
    //空间复杂度O(n)

    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();

        int[] dp = new int[n + 1];

        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j <= i; j++) {
                dp[j] = Math.min(dp[j], dp[j + 1]) + triangle.get(i).get(j);
            }
        }

        return dp[0];
    }


}

```