---
title: 为Hexo的Material主题添加总图库以及分图库功能
date: 2016/12/30 17:33:13
updated: 2016/12/30 20:11:35
category: false
tag: false
license_name: 'XXX'
license_link: 'sssss'
---

嗯现在博客换了主题这个用不上了惹。
<!--more-->
## 起因
今天将整个[Material主题][1]研究了一遍，发现了这个主题自带图库，然而这个图库只能支持一个相册，想将相册分类是不可能的，故而稍加研究，自己添加了这个功能。

总图库由友情链接页面修改得来，子图库则为原图库页面。
[1]: https://material.vss.im/ "Material主题"

## 经过
### 添加总图库
#### 加入总图库入口
在主题文件夹下的` _config.yml`找到` pages`项，插入一行：` 图库: "/galleries/"`
> 其中` 图库`、` galleries`可修改。

然后，在博客主目录的` source`文件夹中新建一个同名文件夹。
> 例如我的文件夹名称为galleries。

并新建一个` index.md`文件，加入以下代码：
```` markdown
---
title: 图库
date: 2016/12/30
layout: galleries
---
````
>` title` 为网页标题，` 图库`可修改，` galleries`可修改，但必须与上面的一致。

再新建一个` _data`文件夹（不可改名）

#### 添加代码
进入主题文件夹的` layout`文件夹中，打开` post.ejs`文件，找一个自己喜欢的位置，添加下面的代码。

```` ejs
<!-- Galleries start -->
<% } else if(page.layout == "galleries") { %>
<%- partial("_widget/page-galleries") %>
<!-- Galleries end -->
````
>其中第二行中` galleries`以及第三行的` page-galleries`可以修改。

然后在` _widget`文件夹中复制` page-links.ejs`文件一份，重命名为` page-galleries.ejs`，打开并找到第83、84、86、87、89行的
` site.data.links`，替换为` site.data.galleries`
，以及第87行` avatar`替换为` pic`。

>` galleries`和` pic`可修改。

### 添加子图库
####  加入子图库入口1
进入博客主目录的` source/_data`文件夹中，新建一个` galleries.yml`文件，加入以下代码
```` yaml
Name:
    link: http://example.com/gallery1
    pic: http://example.com/pic.png
    descr: "这是一个描述"
````
>` Name`修改为你的子图库名称，` gallery1`为你的子图库名称（中文未测试，这个名称后面会用到），` pic`为缩略图。

#### 添加代码
进入主题文件夹的` layout`文件夹中，打开` layout.ejs`文件，在
```` ejs
<% } else { %>
````
前添加以下代码
```` ejs
<!-- gallery1 start-->
<% } else if(page.layout == "gallery1") { %>
<%- partial("_widget/page-gallery1") %>
<!-- gallery1 end-->
````

进入` _widget`文件夹中复制` page-gallery.ejs`文件一份，重命名为` page-gallery1.ejs`，打开并找到第25、26、28、30行
` site.data.gallery`，替换为` site.data.gallery1`
>` gallery1`均可修改，但必须与上面的一致。

####  加入子图库入口2
在博客主目录的` source`文件夹中新建一个` gallery1`文件夹，并新建一个` index.md`文件，加入以下代码：
```` markdown
---
title: 子图库
date: 2016/12/30
layout: gallery1
---
````
>` title` 为网页标题及页面左下角标题，` 子图库`可修改，` galleries`可修改，但必须与上面的一致。

进入` _data`文件夹，新建一个` gallery1.yml`文件，加入以下代码：
```` yaml
Name:
    full_link: http://example.com/pic1.png
    thumb_link: http://example.com/pic1-thumb.png
    descr: "这是一个描述"
````
>` Name`修改为你的一张照片名称，` full_link`为你的照片的连接，` thumb_link`为该照片的缩略图。添加多张图片，只需要根据上面的格式重复填写即可。

## 结果
到这里完成了，自己去看看效果吧！
我的效果如下：

![总图库效果][2]


![子图库效果][3]
[2]: https://img.halyul.com/images/2017/03/15/o_1b7vimbuo745scekls1vj81b8ba.png
[3]: https://img.halyul.com/images/2017/03/15/o_1b7vimqhl7oj112vsjg5pn7vfa.png
