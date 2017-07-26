---
title: 实现主站静态主题更换后，Android手机状态栏随同变色功能
date: 2016/12/31 18:58:00
updated: 2016/12/31 21:20:38
categories:
- 技术
tags:
- Android
---
主页都不知道给我换了多少次了，用不上了嗯。
<!--more-->
## 缘由

### 大背景

Android在KitKat时代加入了变色状态栏，而Chrome在版本v39可以通过读取网页中`` <meta name="theme-color" content="#3F51B5">`` 这段位于`` <head></head>``前的代码实现改变状态栏颜色及地址栏颜色。

![状态栏变色效果][2]![状态栏变色效果][3]
[2]: //img.halyul.com/images/2017/03/15/o_1b7viobs21q6h1b0h1p5v13gf1fmra.png
[3]: https://img.halyul.com/images/2017/03/15/o_1b7vioo0s1re04bb1ld73hfgr6a.png

来源：[Chrome v39 On Lollipop Supports Custom Multitasking Header/Status Bar Colors With A Simple HTML Tag - Android Police][4]
[4]: http://www.androidpolice.com/2014/11/10/chrome-v39-on-lollipop-supports-custom-multitasking-headerstatus-bar-colors-with-a-simple-html-tag/

### 小背景
我的[主站][1]网页代码中有以下几段：
[1]: //halyul.cc
```` js
<!-- jQuery Cookie -->
	<script src="js/jquery.cookie.js"></script>
	<script>
	if ( $.cookie('styleCookie') === 'style-light.css') {
		$('html, body').css('background', '#eeeeee');
//用户读取到上次访问时，网站的样式为style-light.css，全站背景为白色
	} else if ($.cookie('styleCookie') === 'style.css') {
		$('html, body').css('background', '#222222');
//用户读取到上次访问时，网站的样式为style.css，全站背景为黑色
	}
	</script>
	<!-- jQuery Easing -->
	<script src="js/jquery.easing.1.3.js"></script>

````
这几段是涉及到了主页右上角的主题变换开关——黑白风格的变换。

## 实现
而作为Androider我，必须得实现状态栏变色啊！所以各种度娘后，找到了[方法][5]。
[5]: https://zhidao.baidu.com/question/936921982308974372.html

我在源代码以及找到的方法的基础上，在上面的代码中添加以下代码：

``` js
<!-- jQuery Cookie -->
	<script src="js/jquery.cookie.js"></script>
	<!-- Theme changing -->
	<script>
	if ( $.cookie('styleCookie') === 'style-light.css') {
		$('html, body').css('background', '#eeeeee');
//用户读取到上次访问时，网站的样式为style-light.css，全站背景为白色
		var oMeta = document.createElement('meta');
//生成一个新的 <meta>标签
		oMeta.name = 'theme-color';
//给该<meta>标签添加 name="theme-color"属性
		oMeta.content = '#eeeeee';
//给该<meta>标签添加 content = "#eeeeee"属性，状态栏颜色为白色
		document.getElementsByTagName('head')[0].appendChild(oMeta);
//将该<meta>标签放置在<head></head>前
	} else if ($.cookie('styleCookie') === 'style.css') {
		$('html, body').css('background', '#222222');
//用户读取到上次访问时，网站的样式为style.css，全站背景为黑色
		var oMeta = document.createElement('meta');
//生成一个新的 <meta>标签
		oMeta.name = 'theme-color';
//给该<meta>标签添加 name="theme-color"属性
		oMeta.content = '#222222';
//给该<meta>标签添加 content = "#eeeeee"属性，状态栏颜色为黑色
		document.getElementsByTagName('head')[0].appendChild(oMeta);
//将该<meta>标签放置在<head></head>前
	}
	</script>
	<!-- jQuery Easing -->
	<script src="js/jquery.easing.1.3.js"></script>
```
>其中` oMeta`为变量，可以任意修改。


** 由于Chrome每次加载只读取一次`` <meta>``标签，所以更换主题后状态栏颜色必须刷新页面才能生效。**

然后就完成啦~

>然而这个东西卡了我3天。。。之前没有思路，甚至还将主站的主题变换功能去掉了。。。但现在终究是完成了~

## 效果
用户再次访问网站后，即可实现状态栏变色。

![状态栏变色效果][6]![状态栏变色效果][7]
[6]: https://img.halyul.com/images/2017/03/15/o_1b7vip7o71i9710d6kofcs482a.png
[7]: https://img.halyul.com/images/2017/03/15/o_1b7vipr5g4g3tnig521disqh8a.png


从源代码也可以看到在
```` js
<!-- jQuery Easing -->
	<script src="js/jquery.easing.1.3.js"></script>
````

后多了一行黑色状态栏的代码
``` html
<meta name="theme-color" content="#222222">
```

或是白色状态栏的代码
``` html
<meta name="theme-color" content="#eeeeee">
```
