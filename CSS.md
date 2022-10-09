#### CSS选择器及其优先级

| **选择器**   | **格式**     | **优先级权重** |
| ------------ | ------------ | -------------- |
| 行内样式     |              | 1000           |
| id选择器     | #id          | 100            |
| 类选择器     | #classname   | 10             |
| 属性选择器   | a[ref=“eee”] | 10             |
| 伪类选择器   | :last-child  | 10             |
| 标签选择器   | div          | 1              |
| 伪元素选择器 | :after       | 1              |
| 后相邻选择器 | h1+p         | 拆分计算       |
| 后同级选择器 | h1~p         | 拆分计算       |
| 子选择器     | ul>li        | 拆分计算       |
| 后代选择器   | li a         | 拆分计算       |
| 通配符选择器 | *            | 0              |

#### display的block、inline和inline-block的区别

（1）**block：** 会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性；

（2）**inline：** 元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；内部不能放块级元素。

（3）**inline-block：** 将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内。内部可以放块级元素。






#### BFC

**块级格式化上下文(Block Formatting Context，BFC)**

BFC 是一块**独立**的渲染区域，只有它内部的**块级**盒子参与它的布局。

> “块级”两字并不是指BFC是由块级元素产生的，而是指块级元素会参加到BFC的布局中来。

创建BFC

1、根元素

2、浮动元素，即float的值不为none的元素；

3、绝对定位元素，即position的值为fixed或absolute的元素；

4、overflow不为visible的元素；

BFC特点

1、BFC内部的块级盒子会在垂直方向一个接一个的堆放，并且相邻的块级盒子的外边距(Margin)会折叠（一个盒子的下边距会和另一个盒子的上边距塌陷），以最大的一个外边距作为两个盒子之间的距离；

2、计算BFC的高度时，它内部的浮动元素也会被计算进去；

3、BFC的区域不会和浮动盒子相重叠；

4、BFC内部每个元素的Margin Box的左边都会和包含块的Border Box的左边相接触；在从右往左格式化的情况下，则相反。

5、BFC在页面上是一个独立的区域，它内部的元素的布局不会和外部元素的布局产生相互影响；

#### 水平垂直居中

##### 无宽高

1. 绝对定位和translate；

```css
position: absolute;
left: 50%;
top: 50%;
transform: translate(-50%, -50%);
```

2. 绝对定位和margin实现；

```css
position: absolute;
left: 0;
top: 0;
bottom: 0;
right: 0;
margin: auto;
```

3. 为其父元素设置为flex水平垂直轴居中；

```css
display: flex;
align-items: center;
justify-content: center;
```

##### 有宽高

1. 为其父元素设置line-height=高度配合text-align: center（仅限行内元素）；

```css
text-align: center;
line-height: 等于高度;
```

2. 绝对定位配合margin-left左右偏移

```css
position: absolute;
top: 50%;
left: 50%;
width:200px;
height:200px;
margin-top: -100px;
margin-left: -100px;
```

#### flex

**基本概念：**

1. 容器&项目：采用 Flex 布局的元素，称为 Flex 容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。
2. 主轴&交叉轴：堆叠的方向，默认主轴是水平方向，交叉轴是垂直方向。可通过`flex-derection: column`设置主轴为垂直方向。

**容器属性：**

- display: flex
- flex-direction：主轴的方向（即项目的排列方向），row | row-reverse | column | column-reverse;
- flex-wrap：是否换行，nowrap | wrap | wrap-reverse;
- flex-flow：direction 和 wrap简写
- justify-content：主轴对齐方式，flex-start | flex-end | center | space-between | space-around;
- align-items：交叉轴对齐方式，flex-start | flex-end | center | baseline | stretch;
- align-content: 多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。flex-start | flex-end | center | space-between | space-around | stretch;

**项目的属性:**

- order：项目的排列顺序，数值越小，排列越靠前，默认为0。
- flex-grow：放大比例，默认为0，指定元素分到的剩余空间的比例。
- flex-shrink：缩小比例，默认为1，指定元素分到的缩减空间的比例。
- flex-basis：分配多余空间之前，项目占据的主轴空间，默认值为auto
- flex：grow, shrink 和 basis的简写，默认值为0 1 auto
- align-self：单个项目不一样的对齐方式，默认值为auto，auto | flex-start | flex-end | center | baseline | stretch;
- 参考：[Flex 布局教程：语法篇 - 阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)



#### CSS Modules

https://www.ruanyifeng.com/blog/2016/06/css_modules.html

https://github.com/css-modules/css-modules

:import、:export

https://github.com/css-modules/icss

作用：生成局部作用域的CSS样式

下面是一个React组件`App.js`。

```jsx
import React from 'react';
import style from './App.css';

export default () => {
  return (
    // style.title：局部类名
    <h1 className={style.title}>
      Hello World
    </h1>
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
<h1 class="_3zyde4l1yATCOkgn-DBWEL">
  Hello World
</h1>
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

`App.css`加入一个全局`class`，即使没加`:global`，也是全局class，因为是`App.css`全局css文件。

```css
.title {
  color: red;
}

:global(.title) {
  color: green;
}
```

```css
:global{
    // 其它css样式
}
```

`App.js`使用普通的`class`的写法，就会引用全局`class`。

```jsx
import React from 'react';
import styles from './App.css';

export default () => {
  return (
    <h1 className="title">
      Hello World
    </h1>
  );
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
:global{
  .side{
    :local(.title) {
      :global{
        .head{
          color:red;
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
            localIdentName: "[path][name]__[local]--[hash:base64:5]",
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
          { loader: "css-loader", options: { importLoaders: 1, modules: { compileType: "module", localIdentName: "[path][name]_[local]", localIdentContext: srcPath } } }, // 看这行
          { loader: "postcss-loader", options: { postcssOptions: postcssOptions } },
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
> - `[name]` 源文件名称
> - `[path]` 源文件相对于 `compiler.context` 或者 `modules.localIdentContext` 配置项的相对路径。
> - `[file]` - 文件名和路径。
> - `[ext]` - 文件拓展名。
> - `[hash]` - 字符串的哈希值。基于 `localIdentHashSalt`、`localIdentHashFunction`、`localIdentHashDigest`、`localIdentHashDigestLength`、`localIdentContext`、`resourcePath` 和 `exportName` 生成。
> - `[<hashFunction>:hash:<hashDigest>:<hashDigestLength>]` - 带有哈希设置的哈希。
> - `[local]` - 原始类名。
> 
> 建议：
> 
> - 开发环境使用 `'[path][name]__[local]'`
> - 生产环境使用 `'[hash:base64]'`
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
import React from 'react';
import styles from './App.css';

export default () => {
  return (
    <h1 className={styles.title}>
      Hello World
    </h1>
  );
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

选择器也可以继承其他CSS文件里面的规则。

`another.css`

```css
.className {
  background-color: blue;
}
```

`App.css`可以继承`another.css`里面的规则。

```css
.title {
  composes: className from './another.css';
  color: red;
}
```

##### CSS Module解决全局或本地使用@keyframes无效问题

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



####  隐藏元素的方法有哪些

- **display: none**：渲染树不会包含该渲染对象，因此该元素不会在页面中占据位置，也不会响应绑定的监听事件。
- **visibility: hidden**：元素在页面中仍占据空间，但是不会响应绑定的监听事件。
- **opacity: 0**：将元素的透明度设置为 0，以此来实现元素的隐藏。元素在页面中仍然占据空间，并且能够响应元素绑定的监听事件。
- **position: absolute**：通过使用绝对定位将元素移除可视区域内，以此来实现元素的隐藏。
- **z-index: 负值**：来使其他元素遮盖住该元素，以此来实现隐藏。
- **clip/clip-path** ：使用元素裁剪的方法来实现元素的隐藏，这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。
- **transform: scale(0,0)**：将元素缩放为 0，来实现元素的隐藏。这种方法下，元素仍在页面中占据位置，但是不会响应绑定的监听事件。



#### 盒子模型

box-sizing 属性可以被用来调整这些表现：

- `content-box` 默认值，标准盒子模型。如果你设置一个元素的宽为 100px，那么这个元素的内容区会有 100px 宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。例如，`.box {width: 350px; border: 10px solid black; margin: 100px;}` 在浏览器中的渲染的实际宽度将是 370px。

  尺寸计算公式：

  `width` = 内容的宽度

  `height` = 内容的高度



- `border-box` 怪异/IE盒子模型。如果你将一个元素的 width 设为 100px，那么这 100px 会包含它的 border 和 padding，内容区的实际宽度是 width 减去 (border + padding) 的值。例如， `.box {width: 350px; border: 10px solid black;}` 导致在浏览器中呈现的宽度为 350px 的盒子。

  尺寸计算公式：

  `width` = border + padding + 内容的宽度

  `height` = border + padding + 内容的高度



#### 常见问题

##### 父元素宽度由子元素宽度决定

父元素

```css
display: inline-block; // or table
white-space:nowrap; // 如果没有这一条，子元素会自动换行
```

子元素

```css
display: inline-block; // or table
vertical-align: top; // 设置成顶部对齐，默认是底部对齐
```

##### 修改input-placeholder样式

```less
@normalFontSize: 13px;

.placeholder {
    font-size: @normalFontSize;
    color: #999;
}

.input-placeholder{
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
@import 'XXX'; // 引入.input-placeholder的less文件

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

| 滚动条伪元素                          | 作用的位置                                |
| ------------------------------- | ------------------------------------ |
| ::-webkit-scrollbar             | 整个滚动条                                |
| ::-webkit-scrollbar-button      | 滚动条上的按钮 (上下箭头)                       |
| ::-webkit-scrollbar-thumb       | 滚动条上的滚动滑块                            |
| ::-webkit-scrollbar-track       | 滚动条轨道                                |
| ::-webkit-scrollbar-track-piece | 滚动条没有滑块的轨道部分                         |
| ::-webkit-scrollbar-corner      | 当同时有垂直滚动条和水平滚动条时交汇的部分                |
| ::-webkit-resizer               | 某些元素的corner部分的部分样式(例:textarea的可拖动按钮) |

##### 移动端避免使用100vh

在实际设备上测试我们的设计时，我们遇到了几个问题。

- 大部分移动端的Chrome和Firefox浏览器在顶部都有一个UI（地址栏等）。
- 在Safari浏览器上，地址栏在底部，这就变得更加棘手了。
- 不同的浏览器有不同大小的视口
- 移动设备计算浏览器视口为（顶栏+文档+底栏）=100vh

移动端浏览器的地址栏有时可见有时不可见。

 这些浏览器没有将`100vh`高度调整为视口高度变化时屏幕的可见部分，而是将`100vh`设置为浏览器的高度。这就导致地址栏可见时，设置的100vh的元素的高度比屏幕的可见部分高度要大，出现滚动条。

解决方案：

**通过JS检测应用程序的高度**

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

##### :last-child无效

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
.item.last-child{
 // ...
}
```

因为父元素包裹的不仅仅只有.item，还有.last，它们在同一个父级元素内，所以需要把.item再用一个元素包裹

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

思路是先放大、后缩小：在目标元素的后面追加一个 ::after 伪元素，让这个元素布局为 absolute 之后、整个伸展开铺在目标元素上，然后把它的宽和高都设置为目标元素的两倍，border值设为 1px。接着借助 CSS 动画特效中的放缩能力，把整个伪元素缩小为原来的 50%。此时，伪元素的宽高刚好可以和原有的目标元素对齐，而 border 也缩小为了 1px 的二分之一，间接地实现了 0.5px 的效果。

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



##### tab栏外圆角

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



##### 移动端300ms延迟

```css
{
  touch-action:"manipulation",
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
    <meta http-equiv="content-type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">

    <title>涟漪特效按钮</title>
    <link rel="stylesheet" href="../css/54.css">
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
        const btns=document.querySelectorAll("button");
        // 循环所有按钮，并为每一个按钮添加点击事件
        btns.forEach(btn=>{
            btn.addEventListener("click",e=>{
                // 创建span元素，并设置其位置为鼠标点击的位置
                let span=document.createElement("span");
               // MouseEvent 接口的只读属性 offsetX 规定了事件对象与目标节点的内填充边（padding edge）在 X 轴方向上的偏移量
                span.style.left=e.offsetX+"px";
                span.style.top=e.offsetY+"px";
              	span.style.width = e.currentTarget.clientWidth + 'px';
              	span.style.height = e.currentTarget.clientHeight + 'px';
                // 将span元素添加至按钮标签里
                btn.appendChild(span);
                // 1秒后删除span元素
                setTimeout(() => {
                    span.remove();
                  	span = null
                }, 1000);
            })
        })
    </script>
</body>

</html> 
```

**css**

```css
*{
    /* 初始化 */
    margin: 0;
    padding: 0;
}
body{
    /* 100窗口高度 */
    height: 100vh;
    /* 弹性布局 居中 */
    display: flex;
    justify-content: center;
    align-items: center;
    /* 渐变背景 */
    background: linear-gradient(200deg,#80d0c7,#13547a);
}
.btn-box{
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
.btn-box button{
    /* 相对定位 */
    position: relative;
    border: none;
    background: linear-gradient(to right,#52d1c2,#1ab3a1);
    width: 200px;
    height: 60px;
    margin: 20px 0;
    font-size: 18px;
    color: #fff;
    /* 字间距 */
    letter-spacing: 3px;
    border-radius: 30px;
    /* 阴影 */
    box-shadow: 3px 5px 10px rgba(0,0,0,0.1);
    cursor: pointer;
    /* 溢出隐藏 */
    overflow: hidden;
}
.btn-box button:hover{
    box-shadow: 3px 5px 10px rgba(0,0,0,0.2);
}
.btn-box button span{
    /* 绝对定位 */
    position: absolute;
    background-color: #fff;
    transform: translate(-50%,-50%);
    border-radius: 50%;
    /* 执行动画 */
    animation: animate 1s ease;
    /* 设置元素不对指针事件做出反应 */
    pointer-events: none;
}

/* 定义动画 */
@keyframes animate {
    from{
        width: 0;
        height: 0;
        opacity: 0.5;
    }
    to{
        width: 400px;
        height: 400px;
        opacity: 0;
    }
} 
```



##### safari 父元素 overflow hidden 遮挡子元素 position z-index

父元素（position relative）的 z-index 设置成与子元素一致。



#### 现代布局

https://1linelayouts.glitch.me/

https://mp.weixin.qq.com/s/I23bSM_nj0gQhhMM6NhF5g



#### 5 种瀑布流场景的实现原理解析

https://mp.weixin.qq.com/s/sm6cG98pqnYPniVXqL01TQ



#### Retina

对于 Retina 屏它的分辨率（物理像素数）是传统屏的两倍（假设是两倍），而屏幕大小没有变化，所以它需要的图片的分辨率应该是传统屏幕的两倍，显示时如果按传统屏的大小显示，就会因为图像分辨率不够而造成显示模糊。

通常我们设置的CSS像素（逻辑像素）是传统屏的显示尺寸，假设该尺寸为x，（因为对于传统屏来说，1个CSS像素对应一个传统屏的物理像素），所以我们把相同的CSS像素设置给Retina屏就意味着要在Retina屏的显示尺寸为x。

![像素比](http://jay_ohhh.gitee.io/imagehosting/H5C3/像素.png)

假设设备像素比是2：

图片是在普通屏下设计的，如上图所示，在普通屏幕下，1个位图像素对应着1个物理像素，图片可以完美的显示。可是在Retina屏幕下，1个位图像素对应着4个物理像素，由于位图像素不可以再分割，所以图片就会只能就进取色，导致图片模糊。

设置CSS像素（逻辑像素）是设置显示尺寸。

把50×50个位图像素（=传统屏像素=CSS像素）的图片传进传统屏，传统屏就显示50×50的尺寸，而且物理像素和CSS像素一一对应。

把50×50个位图像素（=传统屏像素=CSS像素）的图片传进Reina屏，Retina屏也是显示50×50的尺寸，但是Retina屏的物理像素和CSS像素不是一一对应，而是一个CSS像素对应4个Retina屏的物理像素，这就会有问题：原先的50×50个位图像素的图片放进了Retina屏里，就意味着用100×100个Retina屏物理像素来显示，且显示尺寸不变，由上述可知位图像素是不可以再分割，导致了原先50×50个位图像素不能把100×100个Retina屏物理像素（显示尺寸）填满，所以Retina屏就会把不够的Retina屏物理像素用进取色填满，这就导致了图片的模糊。如果填满100×100个Retina屏物理像素，即实现传统屏图片在Retina屏上显示一样的效果（其实在Retina屏上显示会更加的细腻），则需要100×100个位图像素，即需要二倍图。

浏览器调试时机型的宽高、显示的像素，是逻辑像素，是无论在什么屏幕下都是显示同样尺寸的大小。

##### 二倍精灵图做法

假设设备像素比是2：

1、设置显示尺寸CSS像素，就写成background-size: 二倍精灵图的宽度/2;

2、将这张二倍精灵图用PS等比例缩小一半，测量图标的坐标，填入backgroun-position。



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
