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

**图片**

```text
![alt](URL "title")
```

alt和title即对应HTML中的alt和title属性（都可省略）：

- alt表示图片显示失败时的替换文本
- title表示鼠标悬停在图片时的显示文本（注意这里要加引号）

**仓库图片**

URL即图片的url地址，如果引用本仓库中的图片，直接使用**相对路径**就可了，如果引用其他github仓库中的图片要注意格式，即：`仓库地址/raw/分支名/图片路径`

**链接**

```
[alt](URL "title")
```

**图片+链接**

```
[![alt](http://path/to/img.jpg "title")](http://path/to/img.jpg "title")
```

##### 折叠语法


<details>
  <summary>Title</summary>
  content!!!
</details>

```xml
<details>
  <summary>Title</summary>

  content!!!
</details>
```

##### 键盘标签

> 可以使用 `<kbd>` 标签进行包裹，会使文本看起来像按钮

```
<kbd>Q</kbd> |  <kbd>W</kbd>  | <kbd>E</kbd> |  <kbd>R</kbd> 
```

<kbd>Q</kbd> |  <kbd>W</kbd>  | <kbd>E</kbd> |  <kbd>R</kbd> 



##### 差异可视化

可以使用反引号可视化差异，并`diff`根据需要突出显示红色或绿色的线。

````
```diff
- box.onclick = fn.bind(obj, 200);
+ box.onclick = fn.call(obj, 200);
```
````

效果如下

```diff
- box.onclick = fn.bind(obj, 200);
+ box.onclick = fn.call(obj, 200);
```

##### 较小的文字

在`<sup>`或`<sub>`标记中换行以使其变小。



##### 页内锚点/导航

[第一节](#第一节)

```
[第一节](#第一节)

<h2 id="第一节">第一节</h2>
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

