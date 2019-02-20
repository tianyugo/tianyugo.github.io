---
layout: post
title:  "Hello MyBlog!无敌的建站教程"
date:   2018-04-12 10:25
categories: you
tags: blog GitHub jekyll
excerpt: 第一篇博客。
mathjax: true
author: Tianyu Shen
---


* content
{:toc}

## 前言 ##

本文主要简述自己这几天建站的过程，和几个良心的教程推荐。

根本不废话，递上自己博客地址：[http://tianyugo.top](http://tianyugo.top) <del>狗就狗吧</del>



## 正片 ##


### 搞定框架 ###
要想用搭建一个个人博客网站，首先你得有一个域名，这样别人才可以通过域名访问，其次你还要一个空间来存放你的页面。[[1]](https://blog.csdn.net/tzs_1041218129/article/details/53214497)

#### 域名  ####
   
有很多可以注册域名的网站，[百度一下](https://www.baidu.com/s?tn=80035161_2_dg&wd=%E6%B3%A8%E5%86%8C%E5%9F%9F%E5%90%8D)你就能得到答案，这里我是在阿里云（万网）上注册的，<del>买一个便宜的吧，万一你后悔了呢</del>。2018/10/19更新 我发现涨价了！

![1](http://wx3.sinaimg.cn/mw690/c31bb60bly1fqdb2m59cmj20ra03qjr8.jpg)

详细注册攻略推荐阅览：[教你免费且快速地搭建个人网站](https://blog.csdn.net/c10WTiybQ1Ye3/article/details/78959859)的“域名怎么来的”部分。买完之后需要认证，所以根据网站提示快认证然后进行下一步。


#### 空间  #### 

这个空间，选择 GitHub /’git·hʌb/ Pages。我也是首次使用 GitHub，之前仅是在整‘微信跳一跳刷分’教程上接触过一次。和我一样第一次用的小伙伴，推荐使用谷歌浏览器 翻译网页，方便操作。所以赶快去[https://github.com/](https://github.com/)注册一个吧。

登陆之后，点击你头像右上角的左边的 **＋**，新建项目。


![2](http://wx4.sinaimg.cn/mw690/c31bb60bly1fqdb2phzoij207p057t8l.jpg)

名字固定格式 ：yourusername.github.io （我也不知道为啥，攻略都这么写= 。=）描述爱写就写，其他默认的就可以，直接创建。

![3](http://wx1.sinaimg.cn/mw690/c31bb60bly1fqdb2tx86fj20l70ic759.jpg)


至此你就完成了域名和空间的创建。接下来要将你的 *域名* 和 *GitHub Pages* 连接(绑定)起来，再将你的博客内容放进你的空间里。

####  绑定 ####

1.进入你的项目，点击设置。

 ![4](http://wx3.sinaimg.cn/mw690/c31bb60bly1fqdb2wc6jzj20t00bm74v.jpg)

往下拉，找到GitHub Pages，1.选择一个模板主题，然后就会多出一个Custom domin ，在2处输入你刚买的的域名，保存就可以了。

![5](http://wx3.sinaimg.cn/mw690/c31bb60bly1fqdb2yr25ij20lj0guq3r.jpg)

2.为你的项目添加一个CHAME。在 code页，creat new file。

![6](http://wx2.sinaimg.cn/mw690/c31bb60bly1fqdb3iln33j20s208nq3g.jpg)

文件名为CHAME，内容为你的域名

![7](http://wx2.sinaimg.cn/mw690/c31bb60bly1fqdb3kpy4dj20tk07y74n.jpg)

3.解析域名

![9](http://wx3.sinaimg.cn/mw690/c31bb60bly1fqdb3shjamj20f00gajs0.jpg)

添加解析，记录类型为 CNAME，主机记录为@，记录值为 github 的仓库路径，如下图

![8](http://wx1.sinaimg.cn/mw690/c31bb60bly1fqdb3pnmrwj216r0gkgmz.jpg)

记得删除 txt  mx 类型的解析，详细理由我也没搞清就不多逼逼了。

现在你试试能不能进入你的域名了，如果成功的话，出现的就是你选择的模板。

 
### 整上网页 ###

我的方法是 日常只需要* GitHub Desktop *上传 和 *makedown* 编译器写博客就可以。
####  两个工具： ####

#####  Github Desktop  #####

可以同步你 github 网站上的项目与本地文件，也就是说改了本地文件内容可以同步到网站，改了网站文件内容可以同步到本地。最近也开始学习Git bush



##### MarkDown编译器  #####

特别多，我是用的是 MarkdownPad 2。[MarkdownPad 2 安装和破解](https://blog.csdn.net/github_35160620/article/details/52158604)，[MarkdownPad 2 在win10下出错：HTML 渲染错误(This view has crashed) 的解决办法](https://blog.csdn.net/wyc12306/article/details/51504906)
> 更新！[MarkdownPad 2的安装、配置](https://blog.csdn.net/xiaojin21cen/article/details/78752561#_24)

#####  其他  #####

如果你想在本地阅览博客再上传 需要安装 ruby 和 jekyll 环境，这一部分我还 *有待学习*。

具体链接：[关于这个简洁明快的博客主题](https://github.com/Gaohaoyang/gaohaoyang.github.io)的README。

![11](http://wx2.sinaimg.cn/mw690/c31bb60bly1fqdb445z2ej209r09kglo.jpg)


#### 开始操作 ####

那么开始，大概就是先找一个博客模板，然后改改，也可以自己建。通过使用Github Desktop作为媒介，使本地修改同步到Github仓库。

##### 初始化 #####

我的博客网站使用了[我的GitHub](https://github.com/tianyugo/tianyugo.github.io)的模板，下载下来先。图片展示的是我的Github仓库。

![10](http://wx4.sinaimg.cn/mw690/c31bb60bly1fqdb3v52k4j20u50htjsh.jpg)

把下载下来的项目，整到自己的仓库中，同步到本地中。过程不再详述。（= =，就是我修改的时候懵了，不想写了）

##### 修改 #####

之后修改你的本地文件，同步到GitHub项目就可以了。打开你的 GitHub desktop，需要登陆，提醒你有change，按1 2 3步骤完成上传。1是主题（必要）2是详细描述（非必要）

![12](http://wx3.sinaimg.cn/mw690/c31bb60bly1fqdb47390qj20v20k6wfx.jpg)

**试试吧，去你的个人网站看看。**

##### 备注 #####

那么怎么修改，修改什么呢。

![13](http://wx3.sinaimg.cn/mw690/c31bb60bly1fqdb4a9zgrj206x0a43yf.jpg)

_config.yml：这是你博客的基本配置文件，里面有你博客的名字，以及存放博主的一些基本信息

_posts：这里面装的就是你的博文啦。。md文件，用MarkdownPad 2修改。按模板用makedown语言写。

favicon：网页的小图标。我找小图片的[网站](https://www.iconfinder.com/)推荐给你。

其他的就不详细说明了可以去 [关于这个简洁明快的博客主题](https://github.com/Gaohaoyang/gaohaoyang.github.io)进一步研究，但还主要还是自己改改看看就会明白啦，所以之前提到的本地阅览还是很有必要学习的，不然修改起来比较麻烦。

## 后记吧 ##
我使用的是基于 jekyll的，也可以使用 Hexo+Github Pages 搭建，我嫌麻烦也没有 *深入研究*。

写完感觉，也不知道能不能有人看明白，写的太简单，如上文中提到的，还有很要多学习，很多改进的地方。

更新2018/10/17 20:12:54 ：有个沙雕学了看不懂，想了想、看了看，更改下格式。

如果有啥问题的话可以通过邮件方式联系我 18221352970@sina.cn 

### 参考文献 ###

1.[教你免费且快速地搭建个人网站](https://blog.csdn.net/c10WTiybQ1Ye3/article/details/78959859)

2.[利用Github Page 搭建个人博客网站](https://blog.csdn.net/tzs_1041218129/article/details/53214497)

3.[手把手教你用github pages搭建博客 最新版](https://blog.csdn.net/superjimmy/article/details/51626842)

4.[傻瓜都可以利用github pages建博客](http://cyzus.github.io/2015/06/21/github-build-blog/)

5.[我是如何利用Github Pages搭建起我的博客，细数一路的坑](https://www.cnblogs.com/jackyroc/p/7681938.html)
