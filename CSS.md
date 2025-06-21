#### CSS 选择器及其优先级

| **选择器**   | **格式**     | **优先级权重** |
| ------------ | ------------ | -------------- |
| 行内样式     |              | 1000           |
| id 选择器    | #id          | 100            |
| 类选择器     | #classname   | 10             |
| 属性选择器   | a[ref="eee"] | 10             |
| 伪类选择器   | :last-child  | 10             |
| 标签选择器   | div          | 1              |
| 伪元素选择器 | :after       | 1              |
| 后相邻选择器 | h1+p         | 拆分计算       |
| 后同级选择器 | h1~p         | 拆分计算       |
| 子选择器     | ul>li        | 拆分计算       |
| 后代选择器   | li a         | 拆分计算       |
| 通配符选择器 | \*           | 0              |

[Attribute Selectors Part II](https://meyerweb.com/eric/articles/webrev/200008b.html)

The only way to select only `<p class="red white blue">` is to use a non-tilde attribute selector, like this:

```css
P[class="red white blue"]
```

This would not match `<p class="blue red white">` because the values are in a different order. If you want to match any element with only those three values, but with the values in any order, and you have to use attribute selectors, then you would need to write:

```css
P[class~="red"][class~="white"][class~="blue"]
```

#### display 的 block、inline 和 inline-block 的区别

（1）**block：** 会独占一行，多个元素会另起一行，可以设置 width、height、margin 和 padding 属性；

（2）**inline：** 元素不会独占一行，设置 width、height 属性无效。但可以设置水平方向的 margin 和 padding 属性，不能设置垂直方向的 padding 和 margin；内部不能放块级元素。

（3）**inline-block：** 将对象设置为 inline 对象，但对象的内容作为 block 对象呈现，之后的内联对象会被排列在同一行内。内部可以放块级元素。

#### BFC

**块级格式化上下文(Block Formatting Context，BFC)**

BFC 是一块**独立**的渲染区域，只有它内部的**块级**盒子参与它的布局。

> “块级”两字并不是指 BFC 是由块级元素产生的，而是指块级元素会参加到 BFC 的布局中来。

创建 BFC

1、根元素

2、浮动元素，即 float 的值不为 none 的元素；

3、绝对定位元素，即 position 的值为 fixed 或 absolute 的元素；

4、overflow 不为 visible 的元素；

BFC 特点

1、BFC 内部的块级盒子会在垂直方向一个接一个的堆放，并且相邻的块级盒子的外边距(Margin)会折叠（一个盒子的下边距会和另一个盒子的上边距塌陷），以最大的一个外边距作为两个盒子之间的距离；

2、计算 BFC 的高度时，它内部的浮动元素也会被计算进去；

3、BFC 在页面上是一个独立的区域，它内部的元素的布局不会和外部元素的布局产生相互影响；

4、BFC 内部每个元素的 Margin Box 的左边都会和包含块的 Border Box 的左边相接触；在从右往左格式化的情况下，则相反。

#### 水平垂直居中

##### 无宽高

1. 绝对定位和 translate；

```css
position: absolute;
left: 50%;
top: 50%;
transform: translate(-50%, -50%);
```

2. 绝对定位和 margin 实现；

```css
position: absolute;
left: 0;
top: 0;
bottom: 0;
right: 0;
margin: auto;
```

3. 为其父元素设置为 flex 水平垂直轴居中；

```css
display: flex;
align-items: center;
justify-content: center;
```

##### 有宽高

1. 为其父元素设置 line-height=高度配合 text-align: center（仅限行内元素）；

```css
text-align: center;
line-height: 等于高度;
```

2. 绝对定位配合 margin-left 左右偏移

```css
position: absolute;
top: 50%;
left: 50%;
width: 200px;
height: 200px;
margin-top: -100px;
margin-left: -100px;
```

#### flex

**基本概念：**

1. 容器&项目：采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。
2. 主轴&交叉轴：堆叠的方向，默认主轴是水平方向，交叉轴是垂直方向。可通过`flex-derection: column`设置主轴为垂直方向。

**容器属性：**

-   display: flex
-   flex-direction：主轴的方向（即项目的排列方向），row | row-reverse | column | column-reverse;
-   flex-wrap：是否换行，nowrap | wrap | wrap-reverse;
-   flex-flow：direction 和 wrap 简写
-   justify-content：主轴对齐方式，flex-start | flex-end | center | space-between | space-around;
-   align-items：交叉轴对齐方式，flex-start | flex-end | center | baseline | stretch;
-   align-content: 多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。flex-start | flex-end | center | space-between | space-around | stretch;

**项目的属性:**

-   order：项目的排列顺序，数值越小，排列越靠前，默认为 0。
-   flex-grow：放大比例，默认为 0，指定元素分到的剩余空间的比例。
-   flex-shrink：缩小比例，默认为 1，指定元素分到的缩减空间的比例。
-   flex-basis：分配多余空间之前，项目占据的主轴空间，默认值为 auto
-   flex：grow, shrink 和 basis 的简写，默认值为 0 1 auto
-   align-self：单个项目不一样的对齐方式，默认值为 auto，auto | flex-start | flex-end | center | baseline | stretch;
-   参考：[Flex 布局教程：语法篇 - 阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

#### justify-items、align-items、justify-content、align-content

1. **justify-items**

    > 類似表格中的 "對齊設定"
    > ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812QJypEj26Lr.png)

    預設值： `stretch`
    可以接受的值： `start` / `end` / `center` / `stretch`

    |                                       start                                       |                                        end                                        |                                      center                                       |                                      stretch                                      |
    | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
    | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812eAfaMnbGjt.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812FegPr5EIu0.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812C0r4S0XzlL.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/2010381240fqw4MTK2.png) |

2. **align-items**

    > 類似表格中的 "垂直對齊設定"
    > ![img](http://ithelp.ithome.com.tw/upload/images/20161224/201038129iGJuVGg47.png)

    預設值： `stretch`
    可以接受的值： `start` / `end` / `center` / `stretch`

    |                                       start                                       |                                        end                                        |                                      center                                       |                                      stretch                                      |
    | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
    | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/201038121G6gPaUzYB.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812t3bRiOebDW.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812lnLZ25E9zS.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812E26OmG7ZO0.png) |

左右對齊方式：用 justify-content 設定
上下對齊方式：用 align-content 設定

1. justify-content

    預設值： `start`
    可以接受的值： `start` / `end` / `center` / `stretch` / `pace-around` / `space-between` / `space-evenly`

    |                                       start                                       |                                        end                                        |                                      center                                       |                                      stretch                                      |
    | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
    | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812W96gW7aLdS.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812KSrSd2PcMC.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812G4o9hDC8r3.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812r66YE7N2Ob.png) |

    |                                    pace-around                                    |                                   space-between                                   |                                   space-evenly                                    |
    | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
    | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812h2JA29nmIq.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812UpvpOwSLe8.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812UQAFtQSkog.png) |

2. align-content

    預設值： `start`
    可以接受的值： `start` / `end` / `center` / `stretch` / `pace-around` / `space-between` / `space-evenly`

    |                                       start                                       |                                        end                                        |                                      center                                       |                                      stretch                                      |                                    pace-around                                    |                                   space-between                                   |                                   space-evenly                                    |
    | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------: |
    | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812jnI220q6av.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812vlqf4UHMRS.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/201038121M9R0HdzzP.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812W5cyX6Jd4m.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812NNs0fxfqBr.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812KSV5dJ0mW0.png) | ![img](http://ithelp.ithome.com.tw/upload/images/20161224/20103812hVda7XiphZ.png) |

#### CSS Modules

https://www.ruanyifeng.com/blog/2016/06/css_modules.html

https://github.com/css-modules/css-modules

:import、:export

https://github.com/css-modules/icss

作用：生成局部作用域的 CSS 样式

下面是一个 React 组件`App.js`。

```jsx
import React from "react";
import style from "./App.css";

export default () => {
    return (
        // style.title：局部类名
        <h1 className={style.title}>Hello World</h1>
    );
};
```

上面代码中，我们将样式文件`App.css`输入到`style`对象，然后引用`style.title`代表一个`class`。

```css
.title {
    color: red;
}
```

构建工具会将类名`style.title`编译成一个哈希字符串。

```html
<h1 class="_3zyde4l1yATCOkgn-DBWEL">Hello World</h1>
```

`App.css`也会同时被编译。

```css
._3zyde4l1yATCOkgn-DBWEL {
    color: red;
}
```

这样一来，这个类名就变成独一无二了，只对`App`组件有效。

##### 全局、局部作用域

CSS Modules 允许使用`:global(.className)`的语法，声明一个全局规则。凡是这样声明的`class`，都不会被编译成哈希字符串。

`App.css`加入一个全局`class`，即使没加`:global`，也是全局 class，因为是`App.css`全局 css 文件。

```css
.title {
    color: red;
}

:global(.title) {
    color: green;
}

.provider:global(.ant-divider) {
    // ...
}
```

```css
:global {
    // 其它css样式
}
```

`App.js`使用普通的`class`的写法，就会引用全局`class`。

```jsx
import React from "react";
import styles from "./App.css";

export default () => {
    return <h1 className="title">Hello World</h1>;
};
```

CSS Modules 还提供一种显式的局部作用域语法`:local(.className)`，等同于`.className`。

```css
:local(.title) {
    color: red;
}
/** === */
.title {
    color: red;
}
```

**全局作用域和局部作用域可以混合使用**

```css
:global {
    .side {
        :local(.title) {
            :global {
                .head {
                    color: red;
                }
            }
        }
    }
}
```

##### 定制哈希类名

`css-loader`默认的哈希算法是`[hash:base64]`，这会将`.title`编译成`._3zyde4l1yATCOkgn-DBWEL`这样的字符串。

`webpack.config.js`里面可以定制哈希字符串的格式。

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.css$/,
                loader: "css-loader",
                options: {
                    modules: {
                        localIdentName:
                            "[path][name]__[local]--[hash:base64:5]",
                    },
                },
            },
        ],
    },
};
// 或
module.exports = {
    module: {
        rules: [
            // 编译less
            {
                test: /\.less$/,
                use: [
                    { loader: "style-loader" },
                    {
                        loader: "css-loader",
                        options: {
                            importLoaders: 1,
                            modules: {
                                compileType: "module",
                                localIdentName: "[path][name]_[local]",
                                localIdentContext: srcPath,
                            },
                        },
                    }, // 看这行
                    {
                        loader: "postcss-loader",
                        options: { postcssOptions: postcssOptions },
                    },
                    { loader: "less-loader", options: { sourceMap: true } },
                ],
            },
        ],
    },
};
```

> Loaders 可以通过传入多个 loaders 以达到链式调用的效果，它们会从右到左被应用（从最后到最先配置）。
>
> 支持的模板字符串：
>
> -   `[name]` 源文件名称
> -   `[path]` 源文件相对于 `compiler.context` 或者 `modules.localIdentContext` 配置项的相对路径。
> -   `[file]` - 文件名和路径。
> -   `[ext]` - 文件拓展名。
> -   `[hash]` - 字符串的哈希值。基于 `localIdentHashSalt`、`localIdentHashFunction`、`localIdentHashDigest`、`localIdentHashDigestLength`、`localIdentContext`、`resourcePath` 和 `exportName` 生成。
> -   `[<hashFunction>:hash:<hashDigest>:<hashDigestLength>]` - 带有哈希设置的哈希。
> -   `[local]` - 原始类名。
>
> 建议：
>
> -   开发环境使用 `'[path][name]__[local]'`
> -   生产环境使用 `'[hash:base64]'`
>
> `[local]` 占位符包含原始的类。
>
> **注意：**所有保留 (`<>:"/\|?*`) 和控制文件系统字符 (不包括 `[local]` 占位符) 都将转换为 `-`。

##### Class 的组合

在 CSS Modules 中，一个选择器可以继承另一个选择器的规则，这称为"组合"（["composition"](https://github.com/css-modules/css-modules#composition)）。

在`App.css`中，让`.title`继承`.className` 。

```css
.className {
    background-color: blue;
}

.title {
    composes: className;
    color: red;
}
```

`App.js`不用修改。

```jsx
import React from "react";
import styles from "./App.css";

export default () => {
    return <h1 className={styles.title}>Hello World</h1>;
};
```

`App.css`编译成下面的代码。

```css
._2DHwuiHWMnKTOYG45T0x34 {
    color: red;
}

._10B-buq6_BEOTOl9urIjf8 {
    background-color: blue;
}
```

相应地， `h1`的`class`也会编译成`<h1 class="_2DHwuiHWMnKTOYG45T0x34 _10B-buq6_BEOTOl9urIjf8">`。

##### 输入其他模块

选择器也可以继承其他 CSS 文件里面的规则。

`another.css`

```css
.className {
    background-color: blue;
}
```

`App.css`可以继承`another.css`里面的规则。

```css
.title {
    composes: className from "./another.css";
    color: red;
}
```

##### CSS Module 解决全局或本地使用@keyframes 无效问题

原因：@keyframes 名称也被编译了，这样就获取不到 @keyframes 的名称了。

解决方法

```css
:global {
    .foo :local {
        animation: spin 0.5s linear infinite;
    }
}
@keyframes spin {
    from {
        transform: rotate(0deg);
    }

    to {
        transform: rotate(360deg);
    }
}

/** Which compiles to */
.selector {
    animation: [hashOfYourAnimation] 0.5s linear infinite;
}
```

或

```css
.foo {
    animation: spin 0.5s linear infinite;
}
@keyframes :global(spin) {
    from {
        transform: rotate(0deg);
    }

    to {
        transform: rotate(360deg);
    }
}
```

#### 隐藏元素的方法有哪些

-   **display: none**：渲染树不会包含该渲染对象，因此该元素不会在页面中占据位置，也不会响应绑定的监听事件。
-   **visibility: hidden**：元素在页面中仍占据空间，但是不会响应绑定的监听事件。
-   **opacity: 0**：将元素的透明度设置为 0，以此来实现元素的隐藏。元素在页面中仍然占据空间，并且能够响应元素绑定的监听事件。
-   **position: absolute**：通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏。
-   **z-index: 负值**：来使其他元素遮盖住该元素，以此来实现隐藏。
-   **clip/clip-path** ：使用元素裁剪的方法来实现元素的隐藏，这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。
-   **transform: scale(0,0)**：将元素缩放为 0，来实现元素的隐藏。这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。

#### 盒子模型

box-sizing 属性可以被用来调整这些表现：

-   `content-box` 默认值，标准盒子模型。如果你设置一个元素的宽为 100px，那么这个元素的内容区会有 100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。例如，`.box {width: 350px; border: 10px solid black; margin: 100px;}` 在浏览器中的渲染的实际宽度将是 370px。

    尺寸计算公式：

    `width` = 内容的宽度

    `height` = 内容的高度

-   `border-box` 怪异/IE 盒子模型。如果你将一个元素的 width 设为 100px，那么这 100px 会包含它的 border 和 padding，内容区的实际宽度是 width 减去 (border + padding) 的值。例如， `.box {width: 350px; border: 10px solid black;}` 导致在浏览器中呈现的宽度为 350px 的盒子。

    尺寸计算公式：

    `width` = border + padding + 内容的宽度

    `height` = border + padding + 内容的高度

#### 常见问题

##### 父元素宽度由子元素宽度决定

父元素

```css
display: inline-block; // or table
white-space: nowrap; // 如果没有这一条，子元素会自动换行
```

子元素

```css
display: inline-block; // or table
vertical-align: top; // 设置成顶部对齐，默认是底部对齐
```

##### 修改 input-placeholder 样式

```less
@normalFontSize: 13px;

.placeholder {
    font-size: @normalFontSize;
    color: #999;
}

.input-placeholder {
    .input {
        font-size: @normalFontSize;
        color: #333;
        &::-webkit-input-placeholder {
            /* webkit */
            .placeholder();
        }
        &::-moz-placeholder {
            /* Mozilla Firefox 19+ */
            .placeholder();
        }
        &:-moz-placeholder {
            /* Mozilla Firefox 4 to 18 */
            .placeholder();
        }
        &::-ms-input-placeholder {
            /* Internet Explorer 10-11 */
            .placeholder();
        }
    }
}

// 使用
@import "XXX"; // 引入.input-placeholder的less文件

#app {
    .input-placeholder();
}
```

##### 修改滚动条

```less
// less
.webkitScrollbar(@className, @width: 6px, @height: 6px) {
    /*修改滚动条样式*/
    .@{className}::-webkit-scrollbar {
    // 宽高最好设置成一致，否则水平和垂直的滚动条粗细不一样
        width: @width;
        height: @height;
    }
    .@{className}::-webkit-scrollbar-track {
        border-radius: 10px;
        background-color: #fff;
    }
    .@{className}::-webkit-scrollbar-thumb {
        background-color: rgba(144, 147, 153, 0.3);
        border-radius: 10px;
    }
    .@{className}::-webkit-scrollbar-thumb:hover {
        background-color: rgba(110, 110, 111, 0.3);
    }
    // @{className}::-webkit-scrollbar-corner {
    // }
}
参数说明
::-webkit-scrollbar 滚动条整体部分
::-webkit-scrollbar-thumb  滚动条里面的小方块（三角形），能向上向下移动（或往左往右移动，取决于是垂直滚动条还是水平滚动条）
::-webkit-scrollbar-track  滚动条的轨道（里面装有Thumb）
::-webkit-scrollbar-button 滚动条的轨道的两端按钮，允许通过点击微调小方块的位置。
::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）
::-webkit-scrollbar-corner 边角，即两个滚动条的交汇处
::-webkit-resizer 两个滚动条的交汇处上用于通过拖动调整元素大小的小控件

// 使用
@import 'XXX'; // 引入.webkitScrollbar的less文件

#app{
  .webkitScrollbar(类名) // 类名不要带点
}
```

| 滚动条伪元素                    | 作用的位置                                                 |
| ------------------------------- | ---------------------------------------------------------- |
| ::-webkit-scrollbar             | 整个滚动条                                                 |
| ::-webkit-scrollbar-button      | 滚动条上的按钮 (上下箭头)                                  |
| ::-webkit-scrollbar-thumb       | 滚动条上的滚动滑块                                         |
| ::-webkit-scrollbar-track       | 滚动条轨道                                                 |
| ::-webkit-scrollbar-track-piece | 滚动条没有滑块的轨道部分                                   |
| ::-webkit-scrollbar-corner      | 当同时有垂直滚动条和水平滚动条时交汇的部分                 |
| ::-webkit-resizer               | 某些元素的 corner 部分的部分样式(例:textarea 的可拖动按钮) |

##### 移动端避免使用 100vh

在实际设备上测试我们的设计时，我们遇到了几个问题。

-   大部分移动端的 Chrome 和 Firefox 浏览器在顶部都有一个 UI（地址栏等）。
-   在 Safari 浏览器上，地址栏在底部，这就变得更加棘手了。
-   不同的浏览器有不同大小的视口
-   移动设备计算浏览器视口为（顶栏+文档+底栏）=100vh

移动端浏览器的地址栏有时可见有时不可见。

这些浏览器没有将`100vh`高度调整为视口高度变化时屏幕的可见部分，而是将`100vh`设置为浏览器的高度。这就导致地址栏可见时，设置的 100vh 的元素的高度比屏幕的可见部分高度要大，出现滚动条。

解决方案：

**通过 JS 检测应用程序的高度**

```js
const documentHeight = () => {
 	const doc = document.documentElement
 	doc.style.setProperty('--doc-height', `${window.innerHeight}px`)
}
window.addEventListener(‘resize’, documentHeight)
documentHeight()
```

**使用 css 变量**

```css
:root {
    --doc-height: 100%;
}

html,
body {
    padding: 0;
    margin: 0;
    height: 100vh; /* fallback for Js load */
    height: var(--doc-height);
}
```

##### :last-child 无效

注意：当只有一个子元素时，`:first-child` 和 `:last-child`两个选择器的同个样式会按先后顺序进行覆盖

```html
<div>
    <div class="item">11</div>
    <div class="item">11</div>
    <div class="item">11</div>
    <div class="last"></div>
</div>
```

无效

```css
.item.last-child {
    // ...
}
```

因为父元素包裹的不仅仅只有.item，还有.last，它们在同一个父级元素内，所以需要把.item 再用一个元素包裹

```html
<div class="wrap">
    <div>
        <div class="item">11</div>
        <div class="item">11</div>
        <div class="item">11</div>
    </div>
    <div class="last"></div>
</div>
```

##### 0.5px

思路是先放大、后缩小：在目标元素的后面追加一个 ::after 伪元素，让这个元素布局为 absolute 之后、整个伸展开铺在目标元素上，然后把它的宽和高都设置为目标元素的两倍，border 值设为 1px。接着借助 CSS 动画特效中的放缩能力，把整个伪元素缩小为原来的 50%。此时，伪元素的宽高刚好可以和原有的目标元素对齐，而 border 也缩小为了 1px 的二分之一，间接地实现了 0.5px 的效果。

```css
#container[data-device="2"] {
    position: relative;
}
#container[data-device="2"]::after{
        content:"";
        position:absolute;
        top: 0;
        left: 0;
        width: 200%;
        height: 200%;
        transform: scale(0.5);
        transform-origin: left top;
        box-sizing: border-box;
        border: 1px solid #333;
    }
}
```

##### tab 栏外圆角

```css
 "&::before, &::after": {
                        content: "",
                        position: "absolute",
                        top: 22,
                        width: 8,
                        height: 8,
                        background: "radial-gradient(circle at 100% 0%, transparent 8px, #FFF 0)",
                    },
                    "&::before": {
                        left: -8,
                        transform: "rotateY(180deg)",
                    },
                    "&::after": {
                        right: -8,
                    },
```

or

```css
.ihqzYp::after {
    left: -40px;
    content: "";
    background: transparent;
    position: absolute;
    bottom: -16px;
    height: 56px;
    width: 56px;
    border-radius: 100%;
    border-width: 16px;
    border-style: solid;
    border-color: transparent rgb(255, 255, 255) transparent transparent;
    transform: rotate(45deg);
    box-sizing: border-box;
}
```

##### 移动端 300ms 延迟

```css
 {
    touch-action: "manipulation";
}
```

##### 大纲左边框

```css
 "& .MuiTreeItem-content::before": {
            content: "''",
            position: "absolute",
            top: 0,
            right: -8,
            width: 2,
            height: 25,
            backgroundColor: "#e8e9e8",
            transformOrigin: "-123px center",
            transform: "rotateY(180deg)",
        },
```

##### 按钮点击涟漪效果

**html**

```html
<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="content-type" content="text/html; charset=utf-8" />
        <meta
            name="viewport"
            content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no"
        />

        <title>涟漪特效按钮</title>
        <link rel="stylesheet" href="../css/54.css" />
    </head>

    <body>
        <div class="btn-box">
            <button>点赞</button>
            <button>关注</button>
            <button>收藏</button>
            <button>转发</button>
        </div>
        <script type="text/javascript">
            // 获取所有按钮
            const btns = document.querySelectorAll("button");
            // 循环所有按钮，并为每一个按钮添加点击事件
            btns.forEach(btn => {
                btn.addEventListener("click", e => {
                    // 创建span元素，并设置其位置为鼠标点击的位置
                    let span = document.createElement("span");
                    // MouseEvent 接口的只读属性 offsetX 规定了事件对象与目标节点的内填充边（padding edge）在 X 轴方向上的偏移量
                    span.style.left = e.offsetX + "px";
                    span.style.top = e.offsetY + "px";
                    span.style.width = e.currentTarget.clientWidth + "px";
                    span.style.height = e.currentTarget.clientHeight + "px";
                    // 将span元素添加至按钮标签里
                    btn.appendChild(span);
                    // 1秒后删除span元素
                    setTimeout(() => {
                        span.remove();
                        span = null;
                    }, 1000);
                });
            });
        </script>
    </body>
</html>
```

**css**

```css
* {
    /* 初始化 */
    margin: 0;
    padding: 0;
}
body {
    /* 100窗口高度 */
    height: 100vh;
    /* 弹性布局 居中 */
    display: flex;
    justify-content: center;
    align-items: center;
    /* 渐变背景 */
    background: linear-gradient(200deg, #80d0c7, #13547a);
}
.btn-box {
    width: 500px;
    /* 弹性布局 */
    display: flex;
    /* 横向排列 */
    flex-direction: row;
    /* 允许换行 */
    flex-wrap: wrap;
    /* 平均分配宽度给每一个子元素 */
    justify-content: space-around;
}
.btn-box button {
    /* 相对定位 */
    position: relative;
    border: none;
    background: linear-gradient(to right, #52d1c2, #1ab3a1);
    width: 200px;
    height: 60px;
    margin: 20px 0;
    font-size: 18px;
    color: #fff;
    /* 字间距 */
    letter-spacing: 3px;
    border-radius: 30px;
    /* 阴影 */
    box-shadow: 3px 5px 10px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    /* 溢出隐藏 */
    overflow: hidden;
}
.btn-box button:hover {
    box-shadow: 3px 5px 10px rgba(0, 0, 0, 0.2);
}
.btn-box button span {
    /* 绝对定位 */
    position: absolute;
    background-color: #fff;
    transform: translate(-50%, -50%);
    border-radius: 50%;
    /* 执行动画 */
    animation: animate 1s ease;
    /* 设置元素不对指针事件做出反应 */
    pointer-events: none;
}

/* 定义动画 */
@keyframes animate {
    from {
        width: 0;
        height: 0;
        opacity: 0.5;
    }
    to {
        width: 400px;
        height: 400px;
        opacity: 0;
    }
}
```

##### safari 父元素 overflow hidden 遮挡子元素 position z-index

父元素（position relative）的 z-index 设置成与子元素一致。

##### [Pure CSS drop shadow on scroll](https://codepen.io/StijnDeWitt/pen/LryNxa)

[CSS 层级小技巧！如何在滚动时自动添加头部阴影？](https://mp.weixin.qq.com/s/xh2w0vRMk775zdtrB6sd6A)

##### 默认滚动条在底部

https://stackoverflow.com/questions/72636816/scroll-to-bottom-when-new-message-added

-   flex-direction: column-reverse
-   container.scrollTop = container.scrollHeight

##### z-index

https://philipwalton.com/articles/what-no-one-told-you-about-z-index/

##### table-cell 选中时边框样式

-   border 方案

cell 的 border 可能会根据 border-collapse 会合并，当选中时渲染四条边框可能会使 cell 的内容向里面缩。

-   outline 方案

    outline 不能单独设置上下左右

-   box-shadow 方案

    ```css
     {
        position: relative;
        z-index: 1;
        box-shadow: 1px -1px black, // 右上角
            1px 1px black,
            // 右下角
            -1.5px -1px black, // 左下角
            -1.5px 1px black; // 左上角
    }
    ```

-   伪元素方案

##### css 适配 iPhone 屏幕安全区

https://segmentfault.com/a/1190000020887571

多行文本省略

```scss
// mixin.scss
@mixin ellipsis-line($line) {
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: $line;
    overflow: hidden;
    text-overflow: ellipsis;
}

.ellipse-1 {
}
```

##### visibility 为 hidden 时 元素点击事件可触发吗

visibility 为 hidden 时 ，元素仍然会占据空间，但不会触发点击事件。

##### box-shadow 环形描边

```css
box-shadow: 0 0 0 2px #fff, 0 0 0 calc(2px + 2px) #94a3b8;
```

##### 多行水平分布元素

```css
 {
    display: grid;
    grid-template-columns: repeat(auto-fill, 200px);
    // 最小行间距
    column-gap: 10px;
    row-gap: 10px;
    // 水平分布
    justify-content: space-between;
}
```

##### when css position sticky stops sticking

This question: https://stackoverflow.com/a/45530506 answers the problem.

Once the "sticky div" reaches the edge of the screen, it is at the end of the viewport of the parent element. This causes the sticky element to stop and stay at the end of parent's viewport. This code pen provides an example: https://codepen.io/anon/pen/JOOBxg



##### 扫光效果

```css
.flash-card {
  position: relative;
  overflow: hidden;
  
  &::after {
    content: "";
    position: absolute;
    top: -450%;
    left: -50%;
    width: 200%;
    height: 1000%;
    background: linear-gradient(
      90deg,
      hsla(0, 0%, 100%, 0),
      hsla(0, 0%, 100%, 0.12) 44.6%,
      hsla(0, 0%, 100%, 0.12) 56.31%,
      hsla(0, 0%, 100%, 0) 99.37%
    );
    background-repeat: no-repeat;
    background-size: 50% 100%;
    transform: rotate(22.5deg);
    transform-origin: center;
    animation: flash-card 2s linear 10000;
  }
}

@keyframes flash-card {
  0% {
    background-position: -200% 0;
  }
  70% {
    background-position: right -100% top 0;
  }
  100% {
    background-position: right -100% top 0;
  }
}
```





#### 现代布局

https://1linelayouts.glitch.me/

https://mp.weixin.qq.com/s/I23bSM_nj0gQhhMM6NhF5g

#### 5 种瀑布流场景的实现原理解析

https://mp.weixin.qq.com/s/sm6cG98pqnYPniVXqL01TQ

#### 像素、分辨率

##### 数码像素

数码像素是一种虚拟化的数字，大小可以任意，或者说没有实际的物理尺寸大小，像素大小是去适配显示屏的像素大小，或适配打印的大小。

就单个像素来说，本身的形状是任意的，没有被明确定义的，或者是被捕获时的摄像机或相机、或扫描仪等定义的，本身不具备显示功能，数码像素要显示必须依赖显示设备，包括显卡，显示屏。通常我们理解的一个像素是 1：1 的正方形块，这只是它最常见的一种形式，有的摄像机捕获的像素并不是 1：1 的正方向，比如：1：1.21，1：1.09，1：1.46 等等，但摄像机或相机的光感器形状都是矩形（正方形或长方形，其实也可以是圆形，不规则图形，但没人会用圆形或不规则图形）如下图

![img](https://pica.zhimg.com/80/v2-3a812d801555207773b4d6e1848e292c_1440w.webp?source=1940ef5c)

##### 设备像素 DP（device pixels)）

设备像素是指显示屏的像素，包括电视机，手机显示屏等等。

这些像素不是虚拟的，是实实在在存在的，具有物理尺寸大小，通常是 pt (point，磅) = 1/72 inch。

每个物理像素由颜色值和亮度值组成。

> 设计稿的 750X (宽) 是根据 iphone6 的屏幕分辨率（物理像素）1334\*750

这些像素通常来说只有一种比列 1：1 的正方形，并且像素点之间是紧挨着的。但是我们经常会看到户外显示屏的像素点，通常人们叫 LED 屏幕，这些屏幕的像素点就有不同比例的，还有圆形的，因为人们观看广告的距离不是近距离，而是几十米上百米远，所以他们的像素点并不是一颗紧挨着另一颗，像素点之间有空隙和距离。如下图，黑色缝隙挺大的。

![img](https://pic2.zhimg.com/80/v2-24ae12cf29ca3472a306070338c4993d_1440w.webp?source=1940ef5c)

##### 设备独立像素 (Device Independent Pixel)

与设备无关的逻辑像素，代表可以通过程序控制使用的虚拟像素，是一个总体概念。

可以认为是计算机坐标系统中的一个点，这个点代表一个可以由程序使用的虚拟像素（比如：css 像素），然后由相关的系统转为物理像素。

在`javaScript`中可以通过`window.screen.width/ window.screen.height` 查看

在 chrome 里 看到的`375*667` `320*480`等等都是设备独立像素，它们在数值上与 css 数值是相等的。

##### 物理像素比（DPR - device pixel ratio）

设备像素比，设备像素/设备独立像素，代表设备独立像素到设备像素的转换关系，在 JS 中可以通过 window.devicePixelRatio 获取。

##### 屏幕分辨率

屏幕分辨率是 1024×768，也就是说设备屏幕的水平方向上有 1024 个像素点，垂直方向上有 768 个像素点。

像素的大小是没有固定长度的，不同设备上一个单位像素色块的大小是不一样的。

##### 图像分辨率 Resolution

数码图像的分辨率就是指这幅图的像素个数。数码图像的物理尺寸可以是任意的（参考前面：数码像素）。

##### 图像打印分辨率 ppi （pixels per inch）

数码图像 ppi 设置的意义有两点：一是让你的原始数码图像应该打印多大的物理尺寸。二是高 ppi 可以让打印更精细，但是画面元素在物理尺寸上会缩小，反之，画面元素会放大。

比如 3ppi，100ppi，表示打印的时候一英寸打 3 个和 100 个像素点。如果我的图像分辨率是 5000x4000px，数码图像打印分辨率是 100ppi,那么这幅图打印出来的实际尺寸是宽=5000/100=50inch，和高=4000/100=40inch。

> 打印输出尺寸（inch）=图像的 Resolution/数码图像打印 ppi
>
> 数码图像打印分辨率 ppi 不等于 打印机分辨率 dpi

##### 打印机分辨率 dpi（dots per inch）

打印机分辨率指打印的精细度，指的是油墨点。打印机分辨率越高表示打印点越小，1 单位长度就会有更多的点来表示，那么打印就精细和锐利清晰。反之亦然。打印机分辨率 dpi 不能决定打印图像的物理尺寸大小，物理尺寸大小 inch 是由 ppi 决定，或者人为指定一个大小。

打印机分辨率 dpi 与数码图像打印分辨率 ppi：

在整个打印流程里，ppi 是指打印输入的精度，dpi 是指打印输出的精度。

举个例子：图像打印分辨率是 100ppi，当打印机分辨率为 100dpi 时，我们可以理解为 100ppi 由 100dpi 来打印 ，但不能说 100dpi 由 100ppi 来表示，因为 dpi 的服务对象是 ppi,肯定先有输入再有输出，一个像素可以由很多打印点来打印， 100ppi≠100dpi，像素是方形的，但是打印点就不一定了，可能是圆形，椭圆形，或其它形状，油墨点还有流动性，当然由于打印技术的不同，也可能不是油墨打印。

当然了，输入的精度直接决定了输出的精度，ppi 都不高，dpi 设置再高，图像打印出来都是硕大的方块。

##### 三者控制关系

数码图像分辨率（Resolution,总控）>数码图像打印分辨率（ppi，二级控制）＞打印机分辨率（dpi，最后控制）

##### 打印的物理尺寸大小与观看距离的关系

需要远距离观看打印图的时候=高 Resolution+较低 ppi+较低 dpi（如大尺寸广告图）

需要近距离观看打印图的时候=高 Resolution+较高 ppi+较高 dpi（如相片等小尺寸）

##### 屏幕 ppi (pixels per inch)

每英寸设备像素

屏幕 ppi=√（长边像素数量 ²+宽边像素数量 ²）/对角线长度(inch)

同一幅图像，屏幕 ppi 越高，图像显示越小，图像看起来越精细，锐利，清晰，但可能会降低识别度，什么意思，图片太小了看不清楚图上的文字和一些细节，所以屏幕分辨率并不是越高越好。反之亦然。

![img](https://pic3.zhimg.com/80/v2-29017300bd2304c0cf30c930789b8bf3_1440w.webp?source=1940ef5c)

##### px 与 英寸

物理像素数 dps = px \* dpr 得到
总英寸 = dps / dpi

**打印输出尺寸（inch）=图像的 Resolution/数码图像打印 ppi**

当我们定义了“打印输出尺寸（inch）”和“数码图像打印 ppi”时，打印时的“图像的 Resolution”会自动适配，ppi 越高意味着图像的 Resolution 越高，打印纸张大小不变时，打印更清晰，反之亦然。

##### Retina

当设备像素比为 1:1 时（传统屏），使用 1（1×1）个设备像素显示 1 个 CSS 像素

当设备像素比为 2:1 时（二倍屏），使用 4（2×2）个设备像素显示 1 个 CSS 像素

当设备像素比为 3:1 时（三倍屏），使用 9（3×3）个设备像素显示 1 个 CSS 像素

对于 Retina 屏它的分辨率（物理像素数）是传统屏的两倍（假设是两倍），而屏幕大小没有变化，所以它需要的图片的分辨率应该是传统屏幕的两倍，显示时如果按传统屏的大小显示，就会因为图像分辨率不够而造成显示模糊。

一个位图像素是栅格图像(如：png, jpg, gif 等)最小的数据单元，每一个位图像素都包含着一些自身的显示信息(如：显示位置，颜色值，透明度等)。

理论上，1 个位图像素对应于 1 个物理像素，图片才能得到完美清晰的展示。

而对于 dpr=2 的 Retina 屏幕而言，1 个位图像素对应于 4 个物理像素，由于单个位图像素不可以再进一步分割，所以只能就近取色，导致图片看起来比较模糊，如下图

![像素比](http://jay_ohhh.gitee.io/imagehosting/H5C3/像素.png)

所以在 dpr=2 的 retina 屏下要实现传统屏显示图片的效果，需要二倍图（`长*2`，`宽*2`）。

##### 二倍精灵图做法

假设设备像素比是 2：

1、设置显示尺寸 CSS 像素，就写成 background-size: 二倍精灵图的宽度/2;

2、将这张二倍精灵图用 PS 等比例缩小一半，测量图标的坐标，填入 backgroun-position。

##### 普通屏幕下使用多倍图

普通屏幕下，200×300(CSS pixel)img 标签，所对应的物理像素个数就是 200×300 个，而两倍图片的位图像素个数是 200×300×4 个，所以就出现一个物理像素点对应 4 个位图像素点

但它的取色也只能**通过一定的算法取某一个位图像素点上的色值，这个过程叫做缩减像素采样(downsampling)**，肉眼看上去虽然图片不会模糊，但是会觉得图片缺少一些锐利度，或者是有点色差，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021050121231341.png)

#### 雪碧图/精灵图（CSS Sprite）

**优点**

多张小图片合成一张图片，减少对服务器的请求次数，降低服务器压力，高了页面的加载速度。

**缺点**

要把多张图片有序的合理的合并成一张图片，还要留好足够的空间，防止板块内出现不必要的背景。

维护麻烦，无论是修改雪碧图还是修改页面布局，牵一发而动全身

#### Sass

##### 自定义函数

https://sass-lang.com/documentation/at-rules/function

https://www.sass.hk/skill/sass14.html

##### mixin

https://sass-lang.com/documentation/at-rules/import

https://sass-lang.com/documentation/at-rules/mixin

#### 文字波浪动画

https://mp.weixin.qq.com/s/tBG_o4knzmFjqVroqnqrPw

#### CSS Grid 布局

https://mp.weixin.qq.com/s/WNvT3TO6HmlNSEorHwuB4Q

#### CSS Grid repeat 函数

https://mp.weixin.qq.com/s/Ff5e4SXSC_RPMst_GA1wHg

#### 蛇形布局

https://mp.weixin.qq.com/s/prh1YzeyNMm9Vhcc9Yl5TQ

![图片](https://mmbiz.qpic.cn/mmbiz_png/xvBbEKrVNtKybt7O1PBuPE0R7aTVpAnJFiagCpSZo5MEiaTWmsYRQNtHELfX7ibKgOiccgDRQBq6h5KWsltYKQyVicQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### Ordering of vendor-specific CSS declarations

https://stackoverflow.com/questions/7080605/ordering-of-vendor-specific-css-declarations

后面会覆盖前面的值

Ordering is important. To future proof your code you need to make the W3C spec come last, so the cascade favors it above the vendor prefixed versions.

顺序很重要。为了未来使您的代码具备良好的兼容性，您需要将 W3C 规范放在最后，以使级联选择器优先选择它而不是供应商前缀版本。

```css
// 推荐这种写法
// 在 chrome，border-radius 覆盖 -webkit-border-radius
.foo {
    -moz-border-radius: 10px; /* Mozilla */
    -webkit-border-radius: 10px; /* Webkit */
    border-radius: 10px; /* W3C */
}

// 不推荐这种写法
// 在 chrome，-webkit-border-radius 覆盖 border-radius
.foo {
    border-radius: 10px; /* W3C */
    -moz-border-radius: 10px; /* Mozilla */
    -webkit-border-radius: 10px; /* Webkit */
}
```



#### CSS动态检测屏幕视口尺寸

https://www.bilibili.com/video/BV1zNDrYEEcx/?buvid=XX86A928DD6A16D806A4C137433A44B5790D4&from_spmid=tm.recommend.0.0&is_story_h5=false&mid=KFYSn0kHLC5rrNYp8jMuEQ%3D%3D&plat_id=116&share_from=ugc&share_medium=android&share_plat=android&share_session_id=efb976a9-84b3-4a5f-abf1-692521065bbe&share_source=WEIXIN&share_tag=s_i&spmid=united.player-video-detail.0.0&timestamp=1731692616&unique_k=SB0XunL&up_id=651444241&vd_source=30951f1bb74f9e10ab92917414929bae

```css
@property --vw {
  syntax: "<length>";
  inherits: true;
  initial-value: 100vw;
}
@property --vh {
  syntax: "<length>";
  inherits: true;
  initial-value: 100vh;
}

:root {
  --w: tan(atan2(var(--vw), 1px));
  --h: tan(atan2(var(--vh), 1px));
}

body::before {
  content: counter(w) "X" counter(h);
  counter-reset: w var(--w) h var(--h);
  font-size: 150px;
  font-weight: 900;
  position: fixed;
  inset: 0;
  width: fit-content;
  height: fit-content;
  margin: auto;
}

```

