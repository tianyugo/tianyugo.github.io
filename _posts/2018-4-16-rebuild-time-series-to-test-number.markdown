---
layout: post
title:  "重构序列，做训练集"
date:   2018/4/16 10:46:56  
categories: me
tags: matlab
excerpt: 将一位序列重构为嵌入n维的数据集
mathjax: true
author: Tianyu Shen
---

* content
{:toc}

# "重构序列，做训练集 #

~~~
%% 时间序列，转化成神经网络训练集2018/4/16
   %a为原序列，1×N
   %嵌入维数 n
   %textx n× bujieshouzhiyi
function [testx,testy]=getchange(a,n)
n=n-1;
N=size(a,2);
testx=[];
testy=[];
for i=1:N-n-1
    b=[];
    for j=i:n+i
        b=[b;a(j)];
    end
    testx=[testx,b];
end

for i=n+2:N
    testy=[testy,a(i)];
end

~~~