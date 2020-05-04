---
layout: post
title:  "Python数据处理记录"
date:   2020/02/30 10点33分        
categories: me
tags: Python
excerpt: 记录下处理数据的代码，方便自己日后再用
mathjax: true
author: Sky
---

* content
{:toc}
### Python数据处理记录一条



记录下处理数据的代码，方便自己日后再用。

​        

```python
# -*- coding: utf-8 -*-
"""
Created on Mon Apr 20 22:03:44 2020

@author: Sty
"""


import pandas as pd

import glob,os

#
#城市
city = '北京'




path = 'D:\\test\\china_cities_20190101.csv'
filepath=r'C:\\Users\\Sty\\Desktop\\城市_20190101-20191231\\城市_20190101-20191231'
file=glob.glob(os.path.join(filepath, "china_cities_2019*.csv"))
dl= []


for f in file:
 dl.append(pd.read_csv(f, header=0, parse_dates=True, usecols=['date','hour','type',city], index_col=0, encoding='UTF-8'))
df=pd.concat(dl)

#print(df)

#储存路径
savepath = 'D:\\test\\{}_2019.csv'.format(city);
#存储csv文件数据
df.to_csv(savepath, columns=df.columns, index=True, encoding='UTF-8')






#读取文件路径

path = 'D:\\test\\{}_2019.csv'.format(city);

df = pd.read_csv(path, header=0, parse_dates=True, index_col=0, encoding='UTF-8')

#遍历枚举
for i, val in enumerate(['O3','CO','NO2','SO2','PM10','PM2.5','AQI']):
    df1 = df.loc[df['type'] == val]#O3   CO    NO2   SO2   PM10   PM2.5  AQI
    
       

    #存储csv文件数据
    df1.to_csv('D:\\test\\{}_{}_2019.csv'.format(city,val), columns=df1.columns, index=True, encoding='UTF-8')

```

















2020年4月8日 10点38分

联系我：shentianyu18@163.com









  


