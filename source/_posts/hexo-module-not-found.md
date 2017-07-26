---
title: 解决Hexo MODULE NOT FOUND问题
date: 2017/03/22 21:14:00
categories:
- 杂
tags:
- Hexo
---
Hexo上最玄学的问题，没有之一
<!--more-->

## ~~原油~~缘由
之前给电脑上了Hackintosh， 然后就要开始准备Hexo来写博客和主题，然而，我却遇到了这个玄学问题。

在执行一切`hexo`命令时，均出现
```` bash
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
````
Google了一下，发现hexo的issues里面有[这个](https://github.com/hexojs/hexo/issues/1055).他们都用
````bash
$ npm install hexo --no-optional
````
解决了，不幸的是，我这里解决不了这个问题。。。。

各种搜寻后，终于发现了解决方法。

## 解决
我发现在`node_modules`中，`dtrace-provider`这个模块是存在的，然而hexo是报错，想必某些玄学原因没有读取到这货，我就~~大胆的~~机智的运行了
```` bash
$ npm uninstall dtrace-provider
````
然后再运行
```` bash
npm install dtrace-provider
````

**<big>终于没有这该死的错误了！</big>**
