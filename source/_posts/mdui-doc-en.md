---
title: hexo-theme-mdui document English version
date: 2017/03/23 20:01:00
updated: 2017/05/06 13:25:00
thumbnail: https://static.pexels.com/photos/36487/above-adventure-aerial-air.jpg
categories:
- Document
tags:
- Hexo
pinned: true
---
This is hexo-theme-mdui document English version
<!--more-->

您可以在[这里](https://blog.halyul.cc/2017/03/23/mdui-doc-cn/)找到中文版本.

I write this theme for my personal use, but if you want to use it too, please clone my theme and follow this document to properly configure it.

This document follows the `_config.yml` configurations order. If the doc doesn't mention some configurations, just ignore them, I will remove them in later development.

### Install
#### Install the theme
You need following things：
- a hexo
- my theme

The theme just supports `git clone`, `npm install` will be supported later (or not supported).
```` bash
$ git clone https://github.com/Halyul/hexo-theme-mdui.git
````

Once you have finished clone, copy the folder to hexo `theme` foler , and rename to `mdui`
> The folder name can be changed to whatever you want, but needs to match the name below.

#### Enable the theme
Then open hexo's `_config.yml`, find `theme` and change to folloing name to `mdui`(or the one you have set just now.)

**Then run the following code to install `prismjs` in the `hexo` folder to prevent from getting errors when you run hexo**
```` bash
npm install prismjs --save
````

Run `hexo s --debug` in terminal, open [http://localhost:4000](http://localhost:4000) to make sure the site is running without any problems.

### Config
#### location
- location: For Mathjax. available option is `cn`, for the users in China Mainland to access a better cdn service. For users who are not in China Mainland, the theme uses `Cloudflare` cdn service.

#### style
- bg_img: parallax image in `archive page`
- avatar: author's avatar
- slogan: author's slogan/description
- hoverable: all the cards can hover or not

#### HTML Head
Here you can set HTML head information.
- favicon
- high_res_favicon: high resolution favicon
- high_res_favicon: iOS luncher icon
- keywords: website keywords.

#### Color
Setting up the theme color.
- primary_color: [Primary color]((#Primary-colors)
- accent_color: [Accent color](#Accent-colors)
- layout: [Layout](#Layout)
- random: random color, available options are `global` (global site random color, namely every time you generate or deploy you blog, every pages' color in the blog will change to another.) and `post`(only the post color will change)
> If you define a color in post's `fort-matter`, the defined color takes precedence over the random color.
> for some reasons, when the option is `post`, the custom pages' color would change.

- bg_color: Global background color (needs to be set as HEX format)

**For now, the Android Chrome status bar color changing does't support the color with color scale.
For some reasons again (lol), please change the space to `-` when you use the color.
eg. `deep purple` -> `deep-purple`**
##### Primary colors
![red](https://img.shields.io/badge/primary_color-red-%23F44336.svg?style=flat-square&colorB=F44336) ![pink](https://img.shields.io/badge/primary_color-pink-%23E91E63.svg?style=flat-square&colorB=E91E63) ![purple](https://img.shields.io/badge/primary_color-purple-%239C27B0.svg?style=flat-square&colorB=9C27B0) ![deep-purple](https://img.shields.io/badge/primary_color-deep_purple-%23673AB7.svg?style=flat-square&colorB=673AB7) ![indigo](https://img.shields.io/badge/primary_color-indigo-%233F51B5.svg?style=flat-square&colorB=3F51B5) ![blue](https://img.shields.io/badge/primary_color-blue-%232196F3.svg?style=flat-square&colorB=2196F3) ![light-blue](https://img.shields.io/badge/primary_color-light_blue-%2303A9F4.svg?style=flat-square&colorB=03A9F4) ![cyan](https://img.shields.io/badge/primary_color-cyan-%2300BCD4.svg?style=flat-square&colorB=00BCD4) ![teal](https://img.shields.io/badge/primary_color-teal-%23009688.svg?style=flat-square&colorB=009688) ![green](https://img.shields.io/badge/primary_color-green-%234CAF50.svg?style=flat-square&colorB=4CAF50) ![light-green](https://img.shields.io/badge/primary_color-light_green-%238BC34A.svg?style=flat-square&colorB=8BC34A) ![lime](https://img.shields.io/badge/primary_color-lime-%23CDDC39.svg?style=flat-square&colorB=CDDC39) ![yellow](https://img.shields.io/badge/primary_color-yellow-%23FFEB3B.svg?style=flat-square&colorB=FFEB3B) ![amber](https://img.shields.io/badge/primary_color-amber-%23FFC107.svg?style=flat-square&colorB=FFC107) ![orange](https://img.shields.io/badge/primary_color-orange-%23FF9800.svg?style=flat-square&colorB=FF9800) ![deep-orange](https://img.shields.io/badge/primary_color-deep_orange-%23FF5722.svg?style=flat-square&colorB=FF5722) ![brown](https://img.shields.io/badge/primary_color-brown-%23795548.svg?style=flat-square&colorB=795548) ![grey](https://img.shields.io/badge/primary_color-grey-%239E9E9E.svg?style=flat-square&colorB=9E9E9E)
##### Accent colors
![red](https://img.shields.io/badge/accent_color-red-%23FF5252.svg?style=flat-square&colorB=FF5252) ![pink](https://img.shields.io/badge/accent_color-pink-%23FF4081.svg?style=flat-square&colorB=FF4081) ![purple](https://img.shields.io/badge/accent_color-purple-%23E040FB.svg?style=flat-square&colorB=E040FB) ![deep-purple](https://img.shields.io/badge/accent_color-deep_purple-%237C4DFF.svg?style=flat-square&colorB=7C4DFF) ![indigo](https://img.shields.io/badge/accent_color-indigo-%23536DFE.svg?style=flat-square&colorB=536DFE) ![blue](https://img.shields.io/badge/accent_color-blue-%23448AFF.svg?style=flat-square&colorB=448AFF) ![light-blue](https://img.shields.io/badge/accent_color-light_blue-%2340C4FF.svg?style=flat-square&colorB=40C4FF) ![cyan](https://img.shields.io/badge/accent_color-cyan-%2318FFFF.svg?style=flat-square&colorB=18FFFF) ![teal](https://img.shields.io/badge/accent_color-teal-%2364FFDA.svg?style=flat-square&colorB=64FFDA) ![green](https://img.shields.io/badge/accent_color-green-%2369F0AE.svg?style=flat-square&colorB=69F0AE) ![light-green](https://img.shields.io/badge/accent_color-light_green-%23B2FF59.svg?style=flat-square&colorB=B2FF59) ![lime](https://img.shields.io/badge/accent_color-lime-%23EEFF41.svg?style=flat-square&colorB=EEFF41) ![yellow](https://img.shields.io/badge/accent_color-yellow-%23FFFF00.svg?style=flat-square&colorB=FFFF00) ![amber](https://img.shields.io/badge/accent_color-amber-%23FFD740.svg?style=flat-square&colorB=FFD740) ![orange](https://img.shields.io/badge/accent_color-orange-%23FFAB40.svg?style=flat-square&colorB=FFAB40) ![deep-orange](https://img.shields.io/badge/accent_color-deep_orange-%23FF6E40.svg?style=flat-square&colorB=FF6E40)
##### Layout
The theme has two `layouts`, the default layout is `white`, you change use the `dark` layout.
- layout: "dark"

#### appbar
- title: This is the default title in the appbar, it will always show, and will not change or hide, you can leave it blank
> Personally, I'm not recommend you leaving it blank, because it provides a go-to-index-page function.
> you can find a `back` button in the appbar when you visit some page, this button just provides a return function, which is the as the browser provided.

Also, the header in the `drawer` uses this setting.

#### post
- entry_excerpt: how many posts summary words will be displayed in the index page.
- qr_code: posts/pages' qr code, for more information, please click [here](#QR_CODE)
- prism: code highlight
  - theme: internal theme setting, including `default`, `coy`, `dark`, `funky`, `okaidia`, `solarizedlight`, `tomorrow`, `twilight`
  - line_number: whether show line numbers before the code, available options are `true` and `false`
- donate: post donate, please configure it with the examples. How to use icons please click [here](#ICONS)

#### share_menu
Setting uo the share menu. If you don't need any share options, please remove following contents and set `share_menu: false`
- rss: article rss, the link is the same as the `path` in ``hexo`` `_config.yml` under `feed`, available options are `true` and `false`, more detailed click [here](#RSS).
- weibo: Sina Weibo, available options are `true` and `false`
- twitter: Twitter, available options are `true` and `false`
- facebook: Facebook, available options are `true` and `false`
- googleplus: Google+, available options are `true` and `false`
- linkedin: Linkedin, available options are `true` and `false`
- qq: QQ, available options are `true` and `false`
- telegram: Telegram, available options are `true` and `false`

#### comment
comment service plugin, available options are `disqus` and `custom`
- use: `disqus` or `custom`
- shortname: disqus short name, for example, `example.disqus.com`, so short name is `example`, if `use: custom`, this line is unavailable

If you want to use the comment service that I haven't add support, please click [here](#customComment) for how to configure

#### pagination
Pagination setting
- show: after how many pages the number will be hidden. eg. the original pagination is `1 2 3 4 5 6`, then `show: 2`, the pagination will change to `1 2 ... 5 6`

#### footer
- since: specify the date when the blog was setup
> I haven't add a judgement here, it will lead to following problem.
> If your blog was setup in in 2017, and the year now is 2017, so the copyright will display `2017 - 2017`, please avoid this.

- ICP: this is for users who are in China Mainland to show his/her ICP number.

#### pages
How to use icons please click [here](#ICONS)
##### uncategorized pages
Custom pages setting, please follow format which is left in the `_config.yml` or the one below.
```` yaml
- Test1:
  - link: "/test1"
  - icon: "`&#xe84d;`"
````
These lines will generate an entrance of a link called `Test1`, links to `yoursite.com/test1`, and the icon is `&#xe84d;`.

##### categorized pages
Custom pages support the pages which belong to the same category to be shown under a section, if you wants to enable it, please configure it with the format that the left in the file.
**Key option is `cascade: true`**

#### sns
Here you can set up your contact information, please follow the format that left in the file. How to use icons please click [here](#ICONS)
> or you can fill `false` to disable.

```` yaml
Twitter:
    link: https://twitter.com
    fa: 'fa-twitter'
````

### Vendor
The css and js files haven't cdn service support, so they are accessed from your server directly. If you have your own cdn service, you can do following configurations.
- style_min_css: direct address
- js_min_js: direct address
- outdatedbrowser_min_js: direct address

> Through this feature you can modify the css and js.

### Plugins

#### busuanzi
busuanzi is a plugin to count how many page views and unique visitors.
If you don't need it, please delete following lines and set `busuanzi: false`
- all_site_uv: All site unique visitors
- post_pv: how many times the page has been viewed
- busuanzi_pure_mini_js: busuanzi js address, it is hosted in China Mainland.

#### search
For now, this cant be disabled. Please install `hexo-generator-search` to prevent errors.
```` bash
$ npm install hexo-generator-search --save
````
more details can be found [here](https://github.com/PaicHyperionDev/hexo-generator-search)

#### Mathjax
If your post or page needs `Mathjax`, please add `mathjax: true` in the `font-matter`.

### Advanced settings
#### Custom css and js
If you want to add some custom code like `font-face`, `Google Analytics`, etc., please create a `custom` folder in the `layout` folder in theme folder, then create a `custom_head.ejs` file (the code inside  goes before `</head>` ) or a `custom_js.ejs` file (the code inside  goes before `</body>` ) to put your own code.

#### ICONS
The theme supports `font awesome icons` and `Material icons`.
Add `fa: ` to the option that you want to use `font awesome icons`.
> You can find icons [here](http://fontawesome.io/icons/)

Add `md: ` to the option that you want to use `Material icons`.
> For icons that support modern browsers, you can find from [here](https://material.io/icons). When you find the one you need, click it, and copy the characters **after `https://material.io/icons/#ic_`**
> For icons that support all mainstream browsers, you can find from [here](http://www.mdui.org/docs/material_icon), but the site hasn't an English version. You can just browse the icon list or search an icon, click the one you need, copy the characters like `&#xe84d;`.

Add `null_icon: true` as a placeholer.

#### lightgallery
**unspported now**
Lightgallery is a gallery plugin, you can see what it can do just click the following picture.
![demo](https://bestwallpaperhd.com/wp-content/uploads/2016/02/Google-IO-Plane-Design.jpg)

#### RSS
If you need rss, please install `hexo-generator-feed`
````bash
$ npm install hexo-generator-feed --save
````
more details can be found [here](https://github.com/hexojs/hexo-generator-feed).

#### service_worker
The theme has internal Service Worker support, if you need this feature, please open `source/sw.js` in the **THEME FOLDER**, and configure `ignoreFetch` to match your site setting, for more information, please google or yahoo; then, create a `offline.md` under `source` in the **HEXO FLODER**, **the format is the same as the hexo markdown**

available options are `true` and `pwa`

- true: only inject `Service Worker`
- pwa: The site supports Google's `Progressive Web Apps`

If you use `pwa`, please open `source/manifest.json` in the **THEME FOLDER** to configure, for how to configure please google or yahoo.

#### archive page
##### about_me
This is the `about` dialog in the `archive page`. If you need it, please create `about.yml` under `source/_data` in the **HEXO FOLDER**, and choose a following format to fill in. You can directly copy and paste to see the style.

Options:
```` yaml
slogan: show the slogan/description or not, available options is `true`, or just leave it blank.
others: the text under slogan/description. (these two options are independent)
  - '1'
  - '2'
````
Format 1:
```` yaml
hellow:
  main:
    hi:
      content: xxxxxx
      s_img: /img/avatar.png
    hi1:
      content: xx
      link: //google.com
      s_img: /img/favicon.png
    hi2:
      content: aaaa
  more:
    hi3:
      content: '561456415341'
      link: //google.com
      s_img: /img/favicon.png
    hi4:
      content: 'xxxxxxx'
      s_img: /img/favicon.png
````
Format 2:
```` yaml
hellow1:
  main:
    hi:
      content: xxxxxx
      one_line: true
    hi1:
      content: xx
      fa: 'fa-beer'
    工作經歷:
      content: xxxx
      content_1: Designer & Developer，2222
  more:
    hi3:
      content: '1111'
      one_line: true
      s_img: /img/favicon.png
    hi4:
      content: '1111'
      one_line: true
      c_img: /img/favicon.png
````
Format 3:
```` yaml
hellow2:
  main:
    hi:
      content: xxxxxx
      one_line: true
      md: 'home'
    hi1:
      content: xx
      link: 'xxxx/xxx'
      fa: 'fa-beer'
    工作經歷:
      content: xxxx
      content_1: Designer & Developer，2222
  more:
    hi3:
      content: '1111'
      one_line: true
      null_icon: true
    hi4:
      content: '1111'
      one_line: true
      fa: 'fa-qq'
````

Some options meaning:
- one_line: single line content, if this is `true`, the text in `content: ` will not be rendered.
- fa/md/null_icon: How to use icons please click [here](#ICONS)
- content_1: text that below the text from `content: `
- link: web address
- s_img: square picture that shows before the section content.
- c_img: circular picture that shows before the section content.

##### random_pics
The random pictures on the `Categories`, `Tags`, `Years` in the `archive page`(defining a certain picture is not supported). Those pictures are stored in `source/img/random` in the **THEME FOLDER**.
If you add picture(s), change the total pictures number.
**Only `jpg is supported**

### Post
#### QR_CODE
QR Code for posts and custom pages, available options are `true` and `false`, you need to install `hexo-helper-qrcode` plugin to enbale the feature. Please run following command in terminal in hexo folder.
```` bash
$ npm install hexo-helper-qrcode --save
````

#### Thumbnail
Post and custom page support thumbnail.
- thumbnail: By using this method, the post thumbnail will be showed between `post title` and `post content`.

#### Custom primary &/ accent color in post and custom page
When you write post, add `primary_color` and/or `accent_color` in the `font-matter` can change this post's theme color. Colors name is the same as [Color](#Color)
eg.
```` markdown
title: This is title
primary_color: grey
accent_color: red
-----------
````

#### No comment, No category, No tag
add following content to `font-matter` to disable comment, disable category, disable tag
- comment: set it `false` to disable comment
- category: set it `false` to disable category
- tag: set it `false` to disable tag

#### Pinned Post
Just add `pinned: true` to `font-matter` to pin the post to top

#### customComment
please open `layout/custom/custom_comment.ejs` in the theme folder, and paste the comment service code to this file.
If the comment service needs you to set the `source id (or something like that)` of the page, please fill it with `<%- config.url + config.root + page.path %>`

### Custom pages
#### Friends page
Enter the `source` folder in `hexo` folder, create a folder with the name you like, then enter it and create a `index.md` file, fill it with following content
```` markdown
---
title: MY FRIENDS
layout: friends
header: header of friends page, can be left blank
---
````
then go back to the `source` folder, enter the `_data` folder (if there isn't any, please create one), and create a file called `friends.yml` (**the name can't be changed**), using following format to creat links.
```` yaml
Example:
    link: Site address
    avatar: Avatar
    descr: Slogan or descrpition
````

#### Gallery
Gallery supports multi album and single album.
##### multi album
Create a `galleries.yml` in `source/_data` in the **HEXO FOLDER**, and configure it with following format.
And add a custom page anywhere in `source` in the **HEXO FOLDER**, and fill `layout: galleries`
```` yaml
Albun name:
  link: URL
  descr: album description or something else
  bg: default image
  logo: this can be ignored, a circular image, configure to see what the style like.
````

##### single album
Please create a custom page anywhere in `source` in the **HEXO FOLDER**, and fill `layout: gallery`, **then add `photos: ` with following format in the `font-matter`**
```` markdown
photos:
  x:
    img: /img/bg.png
    descr: 'description goes here'
````
Configure to see what the style like.

### Known issues
#### Using hexo-neat plugin
Because `hexo-neat` plugin will add note when it neats the files, this will break the Android Chrome status bar color changing, if you want to/ are using this plugin, please do following modifications.

Enter the `node_modules` folder in the `hexo` folder, find and enter `hexo-neat` folder, then enter `lib` folder, use your editor open `filter.js`, find following code and **delete**

**The number of rows for reference only, please seach the code**

Line 31, 32
```` javascript
var prefix = '<!-- build time:' + Date() + " -->";
var end = '<!-- rebuild by neat -->';
result = prefix + result + end;
````

Line 58, 59
```` javascript
var prefix = '/* build time:' + Date().toLocaleString() + "*/\n";
var end = '\n/* rebuild by neat */';
````

Line 87, 88
```` javascript
var prefix = '// build time:' + Date().toLocaleString() + "\n";
var end = '\n//rebuild by neat ';
````

Find the following codes and **modify**
**The number of rows for reference only, please seach the code**

Line 60
```` javascript
var css_result = prefix + result.styles + end;
````
change to
```` javascript
var css_result = result.styles;
````

Line 89
```` javascript
var js_result = prefix + result.code + end;
````
change to
```` javascript
var js_result = result.code;
````
