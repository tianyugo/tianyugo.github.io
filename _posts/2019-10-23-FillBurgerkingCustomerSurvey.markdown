---
layout: post
title:  "自动填写汉堡王问卷调查by Python+Selenium"
date:   2019/10/23 10:10:50        
categories: me you
tags: Python3 Selenium
excerpt: 使用Python+Selenium实现汉堡王问卷调查的自动填写、收取回执。
mathjax: true
author: Sky
---

* content
{:toc}

## 前言 ##
作为一个薅羊毛爱好者，发现了填写汉堡王小票的问卷调查可以换小薯之后，开始了+小薯的愉快生活。作为一个自动化专业的银，填写调查怎么能不自动化。

## 正片 ##

### Python3

我使用的是 Anaconda3。


注意：Python3 默认已经安装了pip, pip是一个安装和管理Python包的工具，我们可以用这个工具安装selenium，在Windows命令行（cmd）输入pip即可看到。

### Selenium

- 浏览器自动化测试框架：Selenium /səˈliːniəm/

安装方式：在线pip


由于 安装好的 Python 默认有 pip Python 包管理工具，可以通过 pip 非常方便的安装 Selenium。

- 启动命令行工具：Win+R + cmd 

- 输入命令：pip install selenium


可以用来实现：

- 控制浏览器（浏览器打开网址、控制窗口大小、关闭等）

- 定位符进行定位操作（根据网页源代码中HTML的各类标签进行定位）

	`find_elements_by_XXXXX('需要被找查找的元素')` XXX可以选择多种不同方法：id、name、xpath等

- 鼠标键盘的操作（点击、键盘输入等多种操作）

更多内容可以参考：

- 一文详解：[以后再有人问你selenium是什么，你就把这篇文章给他
](https://blog.csdn.net/TestingGDR/article/details/81950593)






### Chromedriver

本项目使用的是Chrome浏览器,下载地址如下：


-  [Chromedriver](http://chromedriver.storage.googleapis.com/index.html)

根据浏览器版本下载对应的驱动，可以通过设置—关于Chrome查看自己浏览器的版本。但在实际选择驱动版本的时候，会发现没有完全对应的版本。例如我的浏览器：Google Chrome 已是最新版本
版本 77.0.3865.90（正式版本），而我选择driver版本：77.0.3865.40  能够成功实现。

下载完成后，需要将Chromedriver.exe 复制到Python.exe同目录下:
![](https://wx2.sinaimg.cn/mw690/c31bb60bly1g87xuibtqyj20j60c23z4.jpg)




-  其他可供选择的驱动：
 


 Firefox:[https://github.com/mozilla/geckodriver/releases/](https://github.com/mozilla/geckodriver/releases/)



IEdriver:[http://www.nuget.org/packages/Selenium.WebDriver.IEDriver/](http://www.nuget.org/packages/Selenium.WebDriver.IEDriver/)


### 程序
根据前人的工作["Python 3 + Selenium 3 实现汉堡王客户调查提交"](https://www.cnblogs.com/herbert/p/10852841.ht )进行了改进，进一步实现了自动提取验证码。

Github地址：[https://github.com/tianyugo/FillBurgerkingCustomerSurvey](https://github.com/tianyugo/FillBurgerkingCustomerSurvey)

其中：


1. what_you_need文件夹：存放了我使用的驱动版本chromedriver.exe
2. FeedbackCode.txt：返回的验证码会存放在这个txt中。
3. main.py：运行程序
4. test_use：我在测试中使用的问卷调查的每一个网页源代码，和测试程序功能（定位操作等）的代码保存在了这个文件夹。


程序基于unittest单元测试框架。暂有改进思路：
	
- 实现多调查码循环完成。
- 对该框架并不了解太多，可改写成普通的脚本。





2019/10/23 10:09:40  

联系我：shentianyu18@163.com









  


