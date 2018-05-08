---
layout: post
title:  "一些自用数据处理的算法（Matlab）"
date:   2018/4/16 16:32:45  
categories: me
tags: Matlab DataProcessing
excerpt: 归一化，重构序列
mathjax: true
author: Tianyu Shen
---

* content
{:toc}

## 1. 重构序列，做训练集 ##

虽然没标注，应该没啥大问题。

~~~
%% 时间序列，转化成训练集  
   %a为原序列，1×N
   %嵌入维数 n
   %textx n× bujieshouzhiyi
function [trainx,trainy]=getchange(a,n)
n=n-1;
N=size(a,2);
trainx=[];
trainy=[];
for i=1:N-n-1
    b=[];
    for j=i:n+i
        b=[b;a(j)];
    end
    trainx=[trainx,b];
end

for i=n+2:N
    trainy=[trainy,a(i)];
end
~~~

更新：多步
~~~
function [trainx,trainy]=getchange(a,n,k)
%% 时间序列，转化成神经网络训练集沈天宇2018/4/16
   %a为原序列，1×N
   %嵌入维数 n
   %k步预测
   %trainx n× bujieshouzhiyi
   
n=n-1;
N=size(a,2);
trainx=[];
trainy=[];
for i=1:N-n-k
    b=[];
    for j=i:n+i
        b=[b;a(j)];
    end
    trainx=[trainx,b];
end

for i=n+1+k:N
    trainy=[trainy,a(i)];
end
~~~


## 2. 归一化函数  ##

关于归一化函数（mapminmax）的三种常用格式。
~~~
[Y,PS] = mapminmax(X,YMIN,YMAX)
Y = mapminmax('apply',X,PS)

X = mapminmax('reverse',Y,PS)
~~~

## 2. 删除NAN值  ##

删除NAN值。
~~~
test(isnan(test(:,1))==1)=[];

%2016b中
c = rmmissing(a)
~~~
[1](https://jingyan.baidu.com/article/5bbb5a1beca14513eaa17962.html)|[2](https://jingyan.baidu.com/article/0a52e3f4e86e5ebf62ed728a.html)


