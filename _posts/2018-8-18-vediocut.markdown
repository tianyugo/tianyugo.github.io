---
layout: post
title:  "视频剪辑及后期字幕"
date:   2018/8/18 20:43:20   
categories: you
tags: vedio 
excerpt: 分享一些近期学习使用的视频剪辑相关软件
mathjax: true
author: Sky
---

* content
{:toc}

<script src="http://code.jquery.com/jquery-1.7.2.min.js"></script>
<script type="text/javascript">
 //是否显示导航栏
 var showNavBar = true;
 //是否展开导航栏
 var expandNavBar = true;
 
 function isShowHiden(){
	$(".BlogAnchor").hide();
	showNavBar=false;
 }

 function items(){
 
	var h1s = $("body").find("h1");
    var h2s = $("body").find("h2");
    var h3s = $("body").find("h3");
    var h4s = $("body").find("h4");
    var h5s = $("body").find("h5");
    var h6s = $("body").find("h6");

    var headCounts = [h1s.length, h2s.length, h3s.length, h4s.length, h5s.length, h6s.length];
    var vH1Tag = null;
    var vH2Tag = null;
    for(var i = 0; i < headCounts.length; i++){
        if(headCounts[i] > 0){
            if(vH1Tag == null){
                vH1Tag = 'h' + (i + 1);
            }else{
                vH2Tag = 'h' + (i + 1);
            }
        }
    }
    if(vH1Tag == null){
        return;
    }

    $("body").prepend('<div class="BlogAnchor">' + 
        '<span style="color:red;position:absolute;top:-6px;left:0px;cursor:pointer;" onclick="isShowHiden()">×</span>' +
        '<p>' + 
            '<b id="AnchorContentToggle" title="收起" style="cursor:pointer;">目录▲</b>' + 
        '</p>' + 
        '<div class="AnchorContent" id="AnchorContent"> </div>' + 
    '</div>' );

    var vH1Index = 0;
    var vH2Index = 0;
    $("body").find("h1,h2,h3,h4,h5,h6").each(function(i,item){
        var id = '';
        var name = '';
        var tag = $(item).get(0).tagName.toLowerCase();
        var className = '';
        if(tag == vH1Tag){
            id = name = ++vH1Index;
            name = id;
            vH2Index = 0;
            className = 'item_h1';
        }else if(tag == vH2Tag){
            id = vH1Index + '_' + ++vH2Index;
            name = vH1Index + '.' + vH2Index;
            className = 'item_h2';
        }
        $(item).attr("id","wow"+id);
        $(item).addClass("wow_head");
        $("#AnchorContent").css('max-height', ($(window).height() - 180) + 'px');
        $("#AnchorContent").append('<li><a class="nav_item '+className+' anchor-link" onclick="return false;" href="#" link="#wow'+id+'">'+name+" · "+$(this).text()+'</a></li>');
    });

    $("#AnchorContentToggle").click(function(){
		var visibleFlag=$("#AnchorContent").is(":visible");
        if(visibleFlag){
            $(this).html("目录▼");
            $(this).attr({"title":"展开"});
			expandNavBar=false;
			$("#AnchorContent").hide();
        }else{
            $(this).html("目录▲");
            $(this).attr({"title":"收起"});
			expandNavBar=true;
			$("#AnchorContent").show();
        }		
    });
    $(".anchor-link").click(function(){
        $("html,body").animate({scrollTop: $($(this).attr("link")).offset().top}, 500);
    });

    var headerNavs = $(".BlogAnchor li .nav_item");
    var headerTops = [];
    $(".wow_head").each(function(i, n){
        headerTops.push($(n).offset().top);
    });
    $(window).scroll(function(){
        var scrollTop = $(window).scrollTop();
        $.each(headerTops, function(i, n){
            var distance = n - scrollTop;
            if(distance >= 0){
                $(".BlogAnchor li .nav_item.current").removeClass('current');
                $(headerNavs[i]).addClass('current');
                return false;
            }
        });
    });

    if(!showNavBar){
        $('.BlogAnchor').hide();
		return ;
    }else {
		$('.BlogAnchor').show();
	   if(expandNavBar){
		   //执行代码块			
			$("#AnchorContent").show();
		}else{
			$("#AnchorContent").hide();
		}	   
    }
 }
 
$(window).resize(function(){
	//console.log("showNavBar="+showNavBar);
	//console.log("expandNavBar="+expandNavBar);
	
	if(!showNavBar){	
		return ;
	}else{	
		$('.BlogAnchor').remove();
		items();      
    }  
});
 
$(function(){
	items();   
});
</script>
<style>
    /*导航*/
    .BlogAnchor {
        background: #f1f1f1;
        padding: 10px;
        line-height: 180%;
        position: fixed;
        right: 48px;
        top: 48px;
        border: 1px solid #aaaaaa;
    }
    .BlogAnchor p {
        font-size: 18px;
        color: #15a230;
        margin: 0 0 0.3rem 0;
        text-align: right;
    }
    .BlogAnchor .AnchorContent {
        padding: 5px 0px;
        overflow: auto;
    }
    .BlogAnchor li{
        text-indent: 0.5rem;
        font-size: 14px;
        list-style: none;
    }
    .BlogAnchor li .nav_item{
        padding: 3px;
    }
    .BlogAnchor li .item_h1{
        margin-left: 0rem;
    }
    .BlogAnchor li .item_h2{
        margin-left: 2rem;
        font-size: 0.8rem;
    }
    .BlogAnchor li .nav_item.current{
        color: white;
        background-color: #5cc26f;
    }
    #AnchorContentToggle {
        font-size: 13px;
        font-weight: normal;
        color: #FFF;
        display: inline-block;
        line-height: 20px;
        background: #5cc26f;
        font-style: normal;
        padding: 1px 8px;
    }
    .BlogAnchor a:hover {
        color: #5cc26f;
    }
    .BlogAnchor a {
        text-decoration: none;
    }
</style>


## 前言 ##
起因是有人随便揽活，要帮其同事剪辑视频，结果活来到了我这，于是我想起了某天在a站上看到的up主*赛强1992*分享的[添加字幕教程](http://www.acfun.cn/v/ac4072765)。干完活之后想分享一下所使用的工具及网站。
## 正片 ##
### 视频剪辑 Eduis 7 ###
首先使用的视频剪辑软件是Eduis 7，下载地址可以直接百度，或者根据我的[百度云链接](https://pan.baidu.com/s/1dq7lbAnOr7SK9jG4IVIMog) 密码: mi42，先setup然后再用傻瓜式的破解，update安装包我是没有安装成功，竟然直接给我卸载了。

![](https://i.imgur.com/W1YrgnX.png)

没有看明白安装过程的，或者有兴趣的小伙伴可以参考下赛强视频里提到的百度[安装链接](http://www.lookae.com/edius7cark/)。

此外没有视频剪辑软件基础的小伙伴，可以参考b站[edius剪辑软件基础讲解](https://www.bilibili.com/video/av5466027)进行简单的学习即可上手。
### 字幕相关TimeM & Aegisub ###
字幕软件[链接](http://pan.baidu.com/s/1hrFmfi8) 密码：v69a
具体过程可以参考赛强的视频，讲的详细，建议投币交学费呦。

不作累述：1. 使用TimeM将准备好的TXT文件转换为字幕文件，TXT文件需要以换行。2. 将字幕文件及视频的音频文件导入Aegisub，以音频选段方式确定字幕的时间段。3. 格式工厂合成。

至于字幕的字体等属性问题，可以在格式工厂中，选项-字幕字体中修改，暂时还没有找到完美的解决办法，希望可以大家一起交流。
![](https://i.imgur.com/3RcVLUH.png)

### 美化视频素材网站 ###
因为需要在视频中添加一切小东西，找到了一个免费的小图片[网站](https://www.easyicon.net/language.zh-cn/)。将图片变为透明底的[在线网站](http://www.aigei.com/bgremover/)。

## <del>后记</del>牢骚 ##

也是为了之后想做个vlog提前做准备了，现阶段还是使用到了比较少的功能，勉励自己多学吧。也是为了练习一下写文，听已工作的同学github快去学学用啊，而我也只是会用desktop客户端，又躺了半个月，四个月没更新了也是再次活了，总之就是勉励下自己。2018/8/18 21:56:54 

联系我：18221352970@sina.cn

![](https://i.imgur.com/WBd0PHs.png)








  


