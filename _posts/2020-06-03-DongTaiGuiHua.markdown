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


# 动态规划

## [837. 新21点](https://leetcode-cn.com/problems/new-21-game/)

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





## [983. 最低票价](https://leetcode-cn.com/problems/minimum-cost-for-tickets/)

在一个火车旅行很受欢迎的国度，你提前一年计划了一些火车旅行。在接下来的一年里，你要旅行的日子将以一个名为 days 的数组给出。每一项是一个从 1 到 365 的整数。

火车票有三种不同的销售方式：

1. 一张为期一天的通行证售价为 costs[0] 美元；
2. 一张为期七天的通行证售价为 costs[1] 美元；
3. 一张为期三十天的通行证售价为 costs[2] 美元。

通行证允许数天无限制的旅行。 例如，如果我们在第 2 天获得一张为期 7 天的通行证，那么我们可以连着旅行 7 天：第 2 天、第 3 天、第 4 天、第 5 天、第 6 天、第 7 天和第 8 天。

返回你想要完成在给定的列表 days 中列出的每一天的旅行所需要的最低消费。



### 

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

