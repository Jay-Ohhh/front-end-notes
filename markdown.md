[使用PicGo+Gitee建立自己的图床](https://blog.csdn.net/cuoxin123/article/details/106423077?utm_medium=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.wap_blog_relevant_pic&depth_1-utm_source=distribute.wap_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-3.wap_blog_relevant_pic)

[gitee建立图床](https://www.cnblogs.com/AhuntSun-blog/p/12675620.html)，上传图片后，需要手动更新 gitee pages 服务

无论是行内式还是参考式链接

ctrl + 左击链接：新窗口打开链接

[markdown 快捷键](https://www.cnblogs.com/amou/p/9211637.html)

表格：

- 新增一行ctrl+enter

- 删除一行ctrl+shift+de

#### 语法

https://github.com/guodongxiaren/README

##### 图片+链接

图片

```text
![alt](http://path/to/img.jpg "title")
```

链接

```
[alt](http://path/to/img.jpg "title")
```

图片+链接

```
[![alt](http://path/to/img.jpg "title")](http://path/to/img.jpg "title")
```

#### emoji

https://gitmoji.js.org/

https://www.webfx.com/tools/emoji-cheat-sheet/

https://github.com/guodongxiaren/README/blob/master/emoji.md

#### 徽标

第一种写法，直接在md输入，注意`a`标签的结尾`</a>`要和`img`标签紧贴着，不然`a`标签会出现下划线

```html
<p align="center">
  <a href="https://www.npmjs.com/package/anchor-scroll-menu" target="_blank"><img alt="Version" src="https://img.shields.io/npm/v/anchor-scroll-menu.svg" /></a>
  <a href="https://www.npmjs.com/package/anchor-scroll-menu" target="_blank"><img alt="downloads" src="https://img.shields.io/npm/dm/anchor-scroll-menu.svg?color=blue"/></a>
  <a href="https://github.com/Jay-Ohhh/anchor-scroll-menu/blob/master/LICENSE" target="_blank"><img alt="License: MIT" src="https://img.shields.io/github/license/Jay-Ohhh/anchor-scroll-menu" /></a>
</p>
```

效果

<p align="center">
  <a href="https://www.npmjs.com/package/anchor-scroll-menu" target="_blank">
    <img alt="Version" src="https://img.shields.io/npm/v/anchor-scroll-menu.svg" /></a>
  <a href="https://www.npmjs.com/package/anchor-scroll-menu" target="_blank"><img alt="downloads" src="https://img.shields.io/npm/dm/anchor-scroll-menu.svg?color=blue"/></a>
  <a href="https://github.com/Jay-Ohhh/anchor-scroll-menu/blob/master/LICENSE" target="_blank"><img alt="License: MIT" src="https://img.shields.io/github/license/Jay-Ohhh/anchor-scroll-menu" /></a>
</p>

第二种写法，图片链接+引用，这种写法还可嵌套在`html`的元素内，`<p align="center">`下面要空一行

```html
<p align="center">

[![npm version][npm-image]][npm-url] [![downloads][downloads-image]][npm-url] [![license][license-image]][license-url]
  
[npm-url]:https://www.npmjs.com/package/anchor-scroll-menu
[npm-image]:https://img.shields.io/npm/v/anchor-scroll-menu.svg
[downloads-image]:https://img.shields.io/npm/dm/anchor-scroll-menu.svg?color=blue
[license-url]:https://github.com/Jay-Ohhh/anchor-scroll-menu/blob/master/LICENSE
[license-image]:https://img.shields.io/github/license/Jay-Ohhh/anchor-scroll-menu
</p>
```

效果：

<p align="center">
[![npm version][npm-image]][npm-url] [![downloads][downloads-image]][npm-url] [![license][license-image]][license-url]

[npm-url]:https://www.npmjs.com/package/anchor-scroll-menu
[npm-image]:https://img.shields.io/npm/v/anchor-scroll-menu.svg
[downloads-image]:https://img.shields.io/npm/dm/anchor-scroll-menu.svg?color=blue
[license-url]:https://github.com/Jay-Ohhh/anchor-scroll-menu/blob/master/LICENSE
[license-image]:https://img.shields.io/github/license/Jay-Ohhh/anchor-scroll-menu
</p>
