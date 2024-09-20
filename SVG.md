[SVG-阮一峰](http://www.ruanyifeng.com/blog/2018/08/svg.html)

#### 概述

 SVG 是一种基于 XML 语法的图像格式，全称是可缩放矢量图（Scalable Vector Graphics）。其他图像格式都是基于像素处理的，SVG 则是属于对图像的形状描述，所以它本质上是文本文件，体积较小，且不管放大多少倍都不会失真。 

 SVG 文件可以直接插入网页，成为 DOM 的一部分，然后用 JavaScript 和 CSS 进行操作。 

 SVG 代码也可以写在一个独立文件中，然后用`<img>`、`<object>`、`<embed>`、`<iframe>`等标签插入网页。 

 CSS 也可以使用 SVG 文件 

```css
.logo {
  background: url(icon.svg);
}
```

SVG 文件还可以转为 BASE64 编码，然后作为 Data URI 写入网页 

```html
<img src="data:image/svg+xml;base64,[data]">
```



#### 语法和行内属性

**`width`和`height`**

```html
<svg width="100%" height="100%">
  <circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
```

`<svg>`的`width`属性和`height`属性，指定了 SVG 图像在 HTML 元素中所占据的宽度和高度。除了相对单位，也可以采用绝对单位（单位：像素）。如果不指定这两个属性，SVG 图像默认宽度是300px , 高度是150px。 
如果设置了viewBox属性时，没指定`width`和`height`，则默认的宽度和高度则是所在的HTML元素的宽高。

**`viewBox`**

如果只想展示 SVG 图像的一部分，就要指定`viewBox`属性。 

```html
<svg width="100" height="100" viewBox="50 50 50 50">
  <circle id="mycircle" cx="50" cy="50" r="50" />
</svg>
```

`viewBox`属性的值有四个数字，分别是左上角的横坐标和纵坐标、视口的宽度和高度。上面代码中，SVG 图像是100像素宽 x 100像素高，`viewBox`属性指定视口从`(50, 50)`这个点开始。所以，实际看到的是右下角的四分之一圆。 

注意，视口必须适配所在的空间。上面代码中，视口的大小是 50 x 50，由于 SVG 图像的大小是 100 x 100，所以视口会放大去适配 SVG 图像的大小，即放大了四倍。 即`viewBox`先通过矩形框裁剪SVG的内容，然后viewBox视口会变成SVG宽高大小，可以用来放大或缩小SVG的内容。[更多](https://www.cnblogs.com/jaycethanks/p/13395655.html)

如果不指定`width`属性和`height`属性，只指定`viewBox`属性，则相当于只给定 SVG 图像的长宽比。这时，SVG 图像的默认大小将等于所在的 HTML 元素的大小。 

**`id`** 
svg 元素的唯一标识

**`class` **
svg 元素的 class 标识，常用于css样式设置

**`stroke` **
定义 svg 元素的描边颜色 

**`stroke-width`**

`stroke-width`: 2px
`stroke-width`: 2em
`stroke-width`: 2
`stroke-width`: 2.5
`stroke-width`: 15%

值为纯数字时（没单位和没百分号）是基于viewBox的大小。`stroke-width`也是SVG的CSS属性。
例如，`stroke-width: 5`  在设置为0 0 100 100（5/100=.05或5%）的viewBox中，`stroke-width`值为5与5%相同；但在0 0 50 50（5/50=.1或10%）的viewBox中为10%。

**`stroke-linecap`** 

定义不同类型的开放路径的终结

-  `butt`：默认值，端点是垂直于线段边缘的平直边缘（与边缘平直）。
-  `round`：端点是在线段边缘处以线宽为直径的半圆（超出边缘）。
-  `square`：端点是在选段边缘处以线宽为长、以一半线宽为宽的矩形（超出边缘）。

**`stroke-linejoin`** 

线条的连接点，三个属性值

- `miter`，默认值，直接连接，创建尖角。
- `bevel`，连接处为斜角（切去尖角）。
- `round`，连接处为圆角。

**`stroke-dasharray`**

用于创建虚线，实线长度—间距—实线长度—间距—...

```javascript
 stroke-dasharray="20,10,5,5,5,10"
```

 **stroke-opacity**
定义 svg 元素的描边透明度 

**fill **
定义 svg 元素的填充颜色 

**`fill-opacity` **
定义 svg 元素的填充透明度 

 **`transform` **
定义 svg 元素的变换，包括移动、旋转、放缩等，与CSS类似，例如 

```html
将圆顺时针旋转30度，向左平移30，向下平移40，缩小成原来的0.5倍
<circle cx="25" cy="25" r="20" transform="rotate(30) translate(-30 40) scale(0.5)" />
```



#### `<circle>`标签（圆形）

```html
<svg width="300" height="180">
  <circle cx="30"  cy="50" r="25" />
  <circle cx="90"  cy="50" r="25" class="red" />
  <circle cx="150" cy="50" r="25" class="fancy" />
</svg>
```

上面的代码定义了三个圆。`<circle>`标签的`cx`、`cy`、`r`属性分别为横坐标、纵坐标和半径，单位为像素。坐标都是相对于`<svg>`画布的左上角原点。 

`class`属性用来指定对应的 CSS 类。

```css
.red {
  fill: red;
}

.fancy {
  fill: none;
  stroke: black;
  stroke-width: 3px;
}
```

 SVG 的 CSS 属性与网页元素有所不同

- fill：填充色
- stroke：描边色
- stroke-width：边框宽度



#### `<line>`标签（直线）

```html
<svg width="300" height="180">
  <line x1="0" y1="0" x2="200" y2="0" style="stroke:rgb(0,0,0);stroke-width:5" />
</svg>
```

 上面代码中，`<line>`标签的`x1`属性和`y1`属性，表示线段起点的横坐标和纵坐标；`x2`属性和`y2`属性，表示线段终点的横坐标和纵坐标；`style`属性表示线段的样式。 

#### `<polyline>`标签（折线）

```html
<svg width="300" height="180">
  <polyline points="3,3 30,28 3,53" fill="none" stroke="black" />
</svg>
```

 `<polyline>`的`points`属性指定了每个端点的坐标，横坐标与纵坐标之间与逗号分隔，点与点之间用空格分隔。 

#### `<rect>`标签（矩形）

```html
<svg width="300" height="180">
  <rect x="0" y="0" height="100" width="200" style="stroke: #70d5dd; fill: #dd524b" />
</svg>
```

 `<rect>`的`x`属性和`y`属性，指定了矩形左上角端点的横坐标和纵坐标；`width`属性和`height`属性指定了矩形的宽度和高度（单位像素）。 

#### `<ellipse>`标签（椭圆）

```html
<svg width="300" height="180">
  <ellipse cx="60" cy="60" ry="40" rx="20" stroke="black" stroke-width="5" fill="silver"/>
</svg>
```

 `<ellipse>`的`cx`属性和`cy`属性，指定了椭圆中心的横坐标和纵坐标（单位像素）；`rx`属性和`ry`属性，指定了椭圆横向轴和纵向轴的半径（单位像素）。 

#### `<polygon>`标签（多边形）

```html
<svg width="300" height="180">
  <polygon fill="green" stroke="orange" stroke-width="1" points="0,0 100,0 100,100 0,100 0,0"/>
</svg>
```

 `points`属性指定了每个端点的坐标，横坐标与纵坐标之间与逗号分隔，点与点之间用空格分隔。 

#### `<path>`标签（路径）

```html
<svg width="300" height="180">
  <path d="
    M 18,3
    L 46,3
    L 46,40
    L 61,40
    L 32,68
    L 3,40
    L 18,40
    Z
  "></path>
</svg>
```

 `<path>`的`d`属性表示绘制顺序，它的值是一个长字符串，每个字母表示一个绘制动作，后面跟着坐标。 

- M：移动到（moveTo）
- L：画直线到（lineTo）
- Z：闭合路径

#### `<text>`标签（文本）
```html
<svg width="300" height="180">
  <text x="50" y="25">Hello World</text>
 </svg>
```
`<text>`的x属性和y属性，表示文本区块基线（baseline）起点的横坐标和纵坐标。文字的样式可以用class或style属性指定。

#### `<use>`标签（复制、深拷贝）
用于引用一个形状。
```html
<svg viewBox="0 0 30 10" xmlns="http://www.w3.org/2000/svg">
  <circle id="myCircle" cx="5" cy="5" r="4"/>

  <use href="#myCircle" x="10" y="0" fill="blue" />
  <use href="#myCircle" x="20" y="0" fill="white" stroke="blue" /></svg>
```
`<use>`的`href`属性指定所要复制的节点，`x`属性和`y`属性是`<use>`左上角的坐标。另外，还可以指定`width`和`height`。

#### `<g>`标签（组，group）

<g>标签用于将多个形状组成一个组（group），方便复用。
```html
<svg width="300" height="100">
  <g id="myCircle">
    <text x="25" y="20">圆形</text>
    <circle cx="50" cy="50" r="20"/>
  </g>

  <use href="#myCircle" x="100" y="0" fill="blue" />
  <use href="#myCircle" x="200" y="0" fill="white" stroke="blue" />
</svg>
```
#### `<defs>`标签（自定义形状，仅供引用）

用于自定义形状，它内部的代码不会显示，仅供引用。
```html
<svg width="300" height="100">
  <defs>
    <g id="myCircle">
      <text x="25" y="20">圆形</text>
      <circle cx="50" cy="50" r="20"/>
    </g>
  </defs>

  <use href="#myCircle" x="0" y="0" />
  <use href="#myCircle" x="100" y="0" fill="blue" />
  <use href="#myCircle" x="200" y="0" fill="white" stroke="blue" />
</svg>
```
#### `<pattern>`标签（模式，填充图案）

`<pattern>`标签常和`<defs>`标签配合使用，用于自定义一个形状，该形状可以被引用来平铺一个区域。
```html
<svg width="500" height="500">
  <defs>
    <pattern id="dots" x="0" y="0" width="100" height="100" patternUnits="userSpaceOnUse">
      <circle fill="#bee9e8" cx="50" cy="50" r="35" />
    </pattern>
  </defs>
  <-- 通过引用pattern的id来对矩形进行填充 -->
  <rect x="0" y="0" width="100%" height="100%" fill="url(#dots)" />
</svg>
```
`patternUnits="objectBoundingBox"`（默认值）表示`<pattern>`的宽度和长度是相对值，此时`width`和`height`必须小于1.0。图案会根据外层元素变化缩放。
`patternUnits="userSpaceOnUse"`表示`<pattern>`的宽度和长度是绝对的像素值。图案不会缩放。

#### `<image>`标签（插入图片）
```html
<svg viewBox="0 0 100 100" width="100" height="100">
  <image xlink:href="path/to/image.jpg"
    width="50%" height="50%"/>
  </svg>
```
上面代码中，`<image>`的`xlink:href`属性表示图像的来源。

#### `<animate>`标签（动画）
用于图形标签内
```html
<svg width="500px" height="500px">
  <rect x="0" y="0" width="100" height="100" fill="#feac5e">
    <animate attributeName="x" from="0" to="500" dur="2s" repeatCount="indefinite" />
  </rect>
</svg>
```
上面代码中，矩形会不断移动，产生动画效果。
- attributeName：发生动画效果的属性名。
- from：单次动画`attributeName`的初始值。
- to：单次动画`attributeName`的结束值。
- dur：单次动画的持续时间。
- repeatCount：动画的循环模式。

可以在多个属性上面定义动画。
```html
<animate attributeName="x" from="0" to="500" dur="2s" repeatCount="indefinite" />
<animate attributeName="width" to="500" dur="2s" repeatCount="indefinite" />
```
#### `<animateTransform>`标签（动画变形）
`<animate>`标签对 CSS 的`transform`属性不起作用，如果需要变形，就要使用`<animateTransform>`标签。
```html
<svg width="500px" height="500px">
  <rect x="250" y="250" width="50" height="50" fill="#4bc0c8">
    <animateTransform attributeName="transform" type="rotate" begin="0s" dur="10s" from="0 200 200" to="360 400 400" repeatCount="indefinite" />
  </rect>
  </svg>
```
上面代码中，`<animateTransform>`的效果为旋转（`rotate`），这时`from`和`to`属性值有三个数字，第一个数字是角度值，第二个值和第三个值是旋转中心的坐标。`from="0 200 200"`表示开始时，角度为0，围绕(200, 200)开始旋转；`to="360 400 400"`表示结束时，角度为360，围绕(400, 400)旋转。

#### JS 操作 SVG
如果 SVG 代码直接写在 HTML 网页之中，它就成为网页 DOM 的一部分，可以直接用 DOM 操作。
```html
<svg id="mysvg">
    <circle id="mycircle" cx="400" cy="300" r="50" />
<svg>
```
然后，可以用 JavaScript 代码操作 SVG。
```JavaScript
var mycircle = document.getElementById('mycircle');
```

##### 获取 SVG DOM

使用<object>、<iframe>、<embed>标签插入 SVG 文件，可以获取 SVG DOM。
```JavaScript
var svgObject = document.getElementById('object').contentDocument;
var svgIframe = document.getElementById('iframe').contentDocument;
var svgEmbed = document.getElementById('embed').getSVGDocument();
```
- 注意，如果使用<img>标签插入 SVG 文件，就无法获取 SVG DOM。


##### 读取 SVG 源码
由于 SVG 文件就是一段 XML 文本，因此可以通过读取 XML 代码的方式，读取 SVG 源码。
使用XMLSerializer实例的`serializeToString()`方法，获取 SVG 元素的代码。

```JavaScript
var svgString = new XMLSerializer().serializeToString(document.querySelector('svg'));
```

##### SVG 图像转为 Canvas 图像
首先，需要新建一个Image对象，将 SVG 图像指定到该Image对象的src属性。
```JavaScript
var img = new Image();
// svgString是上面所述
var svg = new Blob([svgString], {type: "image/svg+xml;charset=utf-8"});
var DOMURL = self.URL || self.webkitURL || self;
var url = DOMURL.createObjectURL(svg);
img.src = url;
```
然后，当图像加载完成后，再将它绘制到<canvas>元素
```JavaScript
img.onload = function () {
    var canvas = document.getElementById('canvas');
    var ctx = canvas.getContext('2d');
    ctx.drawImage(img, 0, 0)
 }
```

`URL.createObjectURL()`方法会根据传入的参数对象创建一个指向该参数对象的URL。 这个URL的生命仅存在于它被创建的这个文档里。新的对象URL指向执行的`File`对象或者是`Blob`对象。
```JavaScript
// 可省略window
objectURL = window.URL.createObjectURL(blob || file);
```
每次调用`createObjectURL`的时候,一个新的URL对象就被创建了。即使你已经为同一个文件创建过一个URL。如果你不再需要这个对象,要释放它,需要使用`URL.revokeObjectURL(
objectURL)`方法。当页面被关闭,浏览器会自动释放它，但是为了最佳性能和内存使用，当确保不再用得到它的时候,就应该释放它。

#### 支持性

- inline SVG 支持资源外链 支持CSS 支持JS

- img SVG 不支持资源外链 支持内部CSS 不支持JS

- background-img SVG 不支持资源外链 支持内部CSS 不支持JS

- background-img BASE64 SVG 不支持资源外链 支持内部CSS 不支持JS

- object SVG 支持资源外链 支持内部CSS 支持内部JS

- embed SVG 支持资源外链 支持内部CSS 支持内部JS

- iframe SVG 支持资源外链 支持内部CSS 支持内部JS

>  你会发现几乎在所有情况下. SVG都支持内部CSS. 即在SVG内部写 style 标签定义其自身的样式. (注意: inline SVG 的 style 标签会污染外部 HTML 的 style)



#### Path 遮罩

https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/mask

![image-20240703201620307](C:\Users\Administrator.DESKTOP-VRJSU5C\AppData\Roaming\Typora\typora-user-images\image-20240703201620307.png)

```xml
<svg  width="200" height="200"
    xmlns="http://www.w3.org/2000/svg" viewBox="5.6785807609558105 5.677976131439209 88.64265441894531 88.63166046142578">
    <defs>
        <linearGradient x1="0" y1="1" x2="0" y2="0" id="logo-linear-gradient">
            <stop offset="0%" stop-color="#fd9248"></stop>
            <stop offset="50%" stop-color="#fa1768"></stop>
            <stop offset="100%" stop-color="#f001ff"></stop>
        </linearGradient>
        <mask id="logo-mask" x="0" y="0">
            <path fill="white" d="m42.422 67.68c2.4414 1 5.0508 1.3789 7.5195 1.3789 2.5898 0.03125 5.3086-0.53906 7.7305-1.7188 2.4492-1.1719 4.5312-2.9297 6.1484-5.0391 0.26172-0.35156 0.78906-0.66016 0.85938-1.1797 0.070312-0.5-0.30859-1.2188-1.9414-2.0898-1.7812-0.87891-2.4688-1.1016-2.8203-0.98047-0.32812 0.10938-0.32031 0.55859-0.51172 0.76953-2.3594 2.6484-5.5195 4.4805-9.1484 4.4883-1.7891 0-3.5586-0.37891-4.9805-1.3281-1.4297-0.91016-2.5195-2.3086-2.9492-3.9219-0.10938-0.37109-0.19922-0.78906-0.78125-0.78125-0.57812 0-1.6602 0.42969-3.8984 1.5391-1.8281 0.94141-2.3789 1.8398-2.3906 2.5586 0 0.71875 0.51953 1.25 0.87891 1.6914 1.6289 2.0391 3.8281 3.6289 6.2695 4.6094z"/>
            <path fill="white" d="m38.988 43.469c2.9219-4.1289-2.4688-9.2617-6.3281-5.8711-5.4297 4.4219 3.2109 11.801 6.3281 5.8711z"/>
            <path fill="white" d="m68.262 43.469c2.9219-4.1289-2.4688-9.2617-6.3281-5.8711-5.4297 4.4219 3.2109 11.801 6.3281 5.8711z"/>
            <path fill="white" d="m7.0508 79.199c0.44141 1.5195 1.0703 3.1094 2 4.6602 0.51172 0.80078 0.96875 1.4219 1.4805 2.0586 0.51172 0.62891 1.0586 1.2383 1.6289 1.8086 0.60156 0.60156 1.2383 1.1719 1.9102 1.6992 0.67188 0.53125 1.3398 1 2.1914 1.4883 1.6289 0.92188 3.3008 1.4688 4.8789 1.8398 3.1797 0.73047 6.1406 0.91016 9.0586 1.0312 0.96094 0.039063 2.3516 1.0586 2.4883-2.6211 0.14844-3.9688-1.2891-2.8984-2.1484-2.9805-2.6797-0.26172-5.3594-0.46094-7.8086-0.98828-1.2188-0.26953-2.3789-0.60937-3.3711-1.1289-0.94141-0.46875-1.9805-1.2695-2.8281-2.0898-0.85156-0.82812-1.6289-1.8086-2.1914-2.8203-0.53125-0.94141-0.85156-2.0586-1.0391-3.25-0.35938-2.4102-0.17188-5.0703 0.37891-7.7305 0.25-1.2383 0.58984-3.0391-4.4102-2.8984-4.1016 0.12109-3.5898 1.9297-3.5508 3.1719 0.10938 2.8086 0.44141 5.6914 1.3086 8.7383z"/>
            <path fill="white" d="m12.801 22.73c0.26953-1.2188 0.60938-2.3789 1.1289-3.3711 0.46875-0.94141 1.2695-1.9805 2.0898-2.8281 0.82812-0.85156 1.8086-1.6289 2.8203-2.1914 0.94141-0.53125 2.0586-0.85156 3.25-1.0391 2.4102-0.35938 5.0703-0.17188 7.7305 0.37891 1.2383 0.25 3.0391 0.58984 2.8984-4.4102-0.12109-4.1016-1.9297-3.5898-3.1719-3.5508-2.8086 0.10938-5.6914 0.44141-8.7383 1.3086-1.5195 0.44141-3.1211 1.0703-4.6602 2-0.80078 0.51172-1.4219 0.96875-2.0586 1.4805-0.62891 0.51172-1.2383 1.0586-1.8086 1.6289-0.60156 0.60156-1.1719 1.2383-1.6992 1.9102-0.53125 0.67188-1 1.3398-1.4883 2.1914-0.92188 1.6289-1.4688 3.3008-1.8398 4.8789-0.73047 3.1797-0.91016 6.1406-1.0312 9.0586-0.039062 0.96094-1.0586 2.3516 2.6211 2.4883 3.9688 0.14844 2.8984-1.2891 2.9805-2.1484 0.26172-2.6797 0.46094-5.3594 0.98828-7.8086z"/>
            <path fill="white" d="m90.949 16.141c-0.51172-0.80078-0.96875-1.4219-1.4805-2.0586-0.51172-0.62891-1.0586-1.2383-1.6289-1.8086-0.60156-0.60156-1.2383-1.1719-1.9102-1.6992-0.67188-0.53125-1.3398-1-2.1914-1.4883-1.6289-0.92188-3.3008-1.4688-4.8789-1.8398-3.1797-0.73047-6.1406-0.91016-9.0586-1.0312-0.96094-0.039063-2.3516-1.0586-2.4883 2.6211-0.14844 3.9688 1.2891 2.8984 2.1484 2.9805 2.6797 0.26172 5.3594 0.46094 7.8086 0.98828 1.2188 0.26953 2.3789 0.60937 3.3711 1.1289 0.94141 0.46875 1.9805 1.2695 2.8281 2.0898 0.85156 0.82812 1.6289 1.8086 2.1914 2.8203 0.53125 0.94141 0.85156 2.0586 1.0391 3.25 0.35938 2.4102 0.17188 5.0703-0.37891 7.7305-0.25 1.2383-0.58984 3.0391 4.4102 2.8984 4.1016-0.12109 3.5898-1.9297 3.5508-3.1719-0.10938-2.8086-0.44141-5.6914-1.3086-8.7383-0.44141-1.5195-1.0703-3.1094-2-4.6602z"/>
            <path fill="white" d="m87.73 87.828c0.60156-0.60156 1.1719-1.2383 1.6992-1.9102 0.53125-0.67188 1-1.3398 1.4883-2.1914 0.92188-1.6289 1.4688-3.3008 1.8398-4.8789 0.73047-3.1797 0.91016-6.1406 1.0312-9.0586 0.039063-0.96094 1.0586-2.3516-2.6211-2.4883-3.9688-0.14844-2.8984 1.2891-2.9805 2.1484-0.26172 2.6797-0.46094 5.3594-0.98828 7.8086-0.26953 1.2188-0.60938 2.3789-1.1289 3.3711-0.46875 0.94141-1.2695 1.9805-2.0898 2.8281-0.82812 0.85156-1.8086 1.6289-2.8203 2.1914-0.94141 0.53125-2.0586 0.85156-3.25 1.0391-2.4102 0.35938-5.0703 0.17188-7.7305-0.37891-1.2383-0.25-3.0391-0.58984-2.8984 4.4102 0.12109 4.1016 1.9297 3.5898 3.1719 3.5508 2.8086-0.10938 5.6914-0.44141 8.7383-1.3086 1.5195-0.44141 3.1211-1.0703 4.6602-2 0.80078-0.51172 1.4219-0.96875 2.0586-1.4805 0.62891-0.51172 1.2383-1.0586 1.8086-1.6289z"/>
        </mask>
    </defs>
    <rect x="0" y="0" width="100" height="100" fill="url(#logo-linear-gradient)" mask="url(#logo-mask)" />
</svg>
```

**解释**

1. **定义渐变**：
   - 使用 `<defs>` 和 `<linearGradient>` 元素定义了一个从红色到蓝色的线性渐变。
2. **定义遮罩**：
   - 使用 `<mask>` 元素定义了遮罩区域。在 mask 中，白色部分显示为底层 rect 的渐变色，黑色部分则不显示底层 rect 的颜色。
   - 在遮罩中使用不同的 `<path>` 元素来定义那些希望显示底层渐变部分的形状。
3. **应用遮罩**：
   - 在 `<rect>` 元素（用于覆盖整个SVG区域并填充渐变）的 `mask` 属性中引用了提前定义的遮罩 `url(#mask)`。

