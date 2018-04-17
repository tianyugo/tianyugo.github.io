---
layout: post
title:  "判断数据是否高斯分布"
date:   2018/4/12 15:24:19 
categories: me
tags: matlab
excerpt: 如题
mathjax: true
author: Tianyu Shen
---

* content
{:toc}

# 判断数据是否高斯分布 #
转自[http://www.ilovematlab.cn/thread-42634-1-1.html](http://www.ilovematlab.cn/thread-42634-1-1.html)

可以先使用normplot对数据的正态性进行定性了解
~~~
x=[79 75.625 79 82.75 76.1 78.275 76.325 60.5  69.15 75.175 84.075 63.1 77.1625 76.5765  76.575  68.375];

normplot(x);
~~~
越接近红线表明服从正态分布的规律越好

定量的正态检验可以使用Kolmogorov-Smirnov 检验，可help kstest

kstest是检验数据是否服从标准正态分布
~~~
y=zscore(x);   %进行标准化处理
h= kstest(y);
~~~
h=0;
说明数据服从正态分布。
