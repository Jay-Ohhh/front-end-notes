### src 和 href

src和href都是**用来引用外部的资源**，它们的区别如下：

- **src：** 表示对资源的引用，它指向的内容会嵌入到当前标签所在的位置。src会将其指向的资源下载并应⽤到⽂档内，如请求js脚本。当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执⾏完毕，所以⼀般js脚本会放在页面底部。
- **href：** 表示超文本引用，它指向一些网络资源，建立和当前元素或本文档的链接关系。当浏览器识别到它他指向的⽂件时，就会并⾏下载资源，不会停⽌对当前⽂档的处理。 常用在a、link等标签上。



### 探究网页资源究竟是如何阻塞浏览器加载的

https://mp.weixin.qq.com/s/5hMt69-XbPbM5n2wrROlbA

![img](http://taligarsiel.com/Projects/webkitflow.png)

### script标签

defer和async属性异步加载脚本，文档解析过程不中断。

defer：等文档解析结束之后，DOMContentLoaded 触发之前，defer 脚本执行。对 module script 无效。

async：只要加载完脚本就会执行



如果同时存在defer和async ，则defer 的优先级更高



- `onload`：当页面所有资源（包括 `CSS`、`JS`、图片、字体、视频等）都加载完成才触发，而且它是绑定到 `window` 对象上；
- `DOMContentLoaded`：当 HTML 已经完成解析，并且构建出了 `DOM`，但此时外部资源比如样式和脚本可能还没加载完成，并且该事件需要绑定到 `document` 对象上；

**动态插入的脚本不会阻塞页面解析**

对于 `async` 脚本和动态脚本是不会阻塞 `DOMContentLoaded` 触发的



### meta标签

#### viewport

```html
<!-- PC端 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<!-- 移动端 -->
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=0" />
```

#### http-equiv

##### X-UA-Compatible

我们最常见的`http-equiv`值可能就是`X-UA-Compatible`了，它常常长这个样子：

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
```

它是用来是做IE浏览器适配的。

`IE=edge`告诉浏览器，以当前浏览器支持的最新版本来渲染，IE9就以IE9版本来渲染。

`chrome=1`告诉浏览器，如果当前IE浏览器安装了`Google Chrome Frame`插件，就以chrome内核来渲染页面。

像上图这种两者都存在的情况：如果有chrome插件，就以chrome内核渲染，如果没有，就以当前浏览器支持的最高版本渲染。

另外，这个属性支持的范围是`IE8-IE11`

你可能注意到了，如果在我们的`http`头部中也设置了这个属性，并且和`meta`中设置的有冲突，那么哪一个优先呢？答案是：开发者偏好（`meta`元素）优先于Web服务器设置（HTTP头）。

##### content-type

用来声明文档类型和字符集

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
```

##### x-dns-prefetch-control

一般来说，HTML页面中的a标签会自动启用DNS提前解析来提升网站性能，但是在使用https协议的网站中失效了，我们可以设置：

```html
<meta http-equiv="x-dns-prefetch-control" content="on" />
```

来打开dns对a标签的提前解析

##### 缓存

**和缓存相关的设置：cache-control、Pragma、Expires**

但是遗憾的是这些往往不生效，我们一般都通过`http headers`来设置缓存策略

```html
<!-- 禁止缓存该文档，再访问这个网站要重新下载 -->
<meta
http-equiv="Cache-Control"
content="no-cache, no-store, must-revalidate"
/>
<!-- 是用于设定禁止浏览器从本地机的缓存中调阅页面内容，设定后一旦离开网页就无法从Cache中再调出 -->
<!-- 访问者将无法脱机浏览 -->
<meta http-equiv="Pragma" content="no-cache" />
<!-- 设定网页的到期时间 -->
<!-- content等号后面的过期时间，expires的值既可以为具体的秒数，也可以为特定的时间，这个时间必须为GMT时间 -->
<!-- 如果设置content="0"或content="负数"，则表示网页在本地缓存0秒后就过期了，也就是说，每次打开网页都需要从服务器上重新加载网页内容，才能在浏览器中显示内容！ -->
<meta http-equiv="Expires" content="0" />
```

#### robots

表示爬虫对此页面的处理行为，或者说，应当遵守的规则，是用来做搜索引擎抓取的。

它的`content`可以为：

1. `all`:搜索引擎将索引此网页，并继续通过此网页的链接索引文件将被检索
2. `none`:搜索引擎讲忽略此网页
3. `index`:搜索引擎索引此网页
4. `follow`:搜索引擎继续通过此网页的链接索引搜索其它的网页



#### 其他

```html
<!-- 用于配置浏览器页签+地址栏的颜色（仅支持安卓） -->
<meta name="theme-color" content="#000000" />

<!-- 描述该文档 -->
<meta name="description" content="让人轻松无忧的后台管理平台" />

<!-- 标识创作者 -->
<meta name="author" content="https://gitee.com/Jay_Ohhh/carefree-admin" />

<!-- 关键词 -->
<meta name="keywords" content="a,b,c">

<!--渲染器选择  webkit，ie-comp，ie-stand：极速模式，兼容模式，IE模式，比如360浏览器通过这个设置来指定的渲染方式 -->
<meta name="renderer" content="webkit"> //默认webkit内核
```





### BFC

#### 块级格式化上下文(Block Formatting Context，BFC)

BFC 是一块**独立**的渲染区域，只有它内部的**块级**盒子参与它的布局。与区域外毫不相干，同时它们也不会影响BFC外部的布局。

> 块级格式化上下文中的“块级”两字并不是指BFC是由块级元素产生的，而是指块级元素会参加到BFC的布局中来。

#### 如何创建BFC

1、根元素

> Each element in this tree has exactly one parent, with the exception of the **[root](https://link.zhihu.com/?target=https%3A//www.w3.org/TR/CSS21/conform.html%23root)** element, which has none.
>
> w3的定义：根元素就是没有父节点的 dom 节点

2、浮动元素，即float的值不为none的元素；

3、绝对定位元素，即position的值为fixed或absolute的元素；

4、overflow不为visible的元素；

5、display的值是inline-block、flex、inline-flex、table-cell、table-caption

6、网格元素，即display的值为grid或者inline-grid的元素；

7、HTML默认的某些表格元素：

- table，display的值为table；
- th和td，display的值为table-cell；
- caption，display的值为table-caption；
- tr，display的值为table-row；
- tbody，display的值为table-row-group；
- thead，display的值为table-header-group；
- tfoot，display的值为table-footer-group；

8、流式布局根元素，display值为flow-root的元素；

9、contain的值为layout、contain或者strict的元素；

10、多列容器，column-count或者column-width的值不为auto；column-count的值为1也是多列容器；

11、column-span的值为all的元素始终会创建一个BFC

#### BFC的布局规则

1、BFC内部的块级盒子会在垂直方向一个接一个的堆放，并且相邻的块级盒子的外边距(Margin)会折叠（一个盒子的下边距会和另一个盒子的上边距塌陷），以最大的一个外边距作为两个盒子之间的距离；

> 阻止外边距的折叠的方法有很多，最简单的方法就是将这几个相邻的盒子放在独立的BFC容器中。
>
> 该方法利用了BFC不影响外部元素布局的原理。

2、计算BFC的高度时，它内部的浮动元素也会被计算进去；

> 当我们不给父节点设置高度，子节点设置浮动的时候，会发生高度塌陷（父元素不会把子元素的高度计算进去）。因此我们可以把父节点设置成BFC容器，达到清除浮动的效果。
>
> 该方法利用了BFC不影响外部元素布局的原理。

3、BFC的区域不会和浮动盒子相重叠；

4、BFC内部每个元素的Margin Box的左边都会和包含块的Border Box的左边相接触；在从右往左格式化的情况下，则是每个元素的Margin Box的右边都会和包含块的Border Box的右边相接触；

5、BFC在页面上是一个独立的区域，它内部的元素的布局不会和外部元素的布局产生相互影响；



### 重绘 repaint 和 回流  reflow

重绘：元素的几何尺寸和位置没有发生改变，外观改变

回流：元素的几何尺寸或位置发生改变



### MIME 类型

https://www.runoob.com/http/mime-types.html



### 几个鲜为人知但很有用的 HTML 属性

https://mp.weixin.qq.com/s/nF7PcpxuoY4a0ZQ2q4DNxw



### 常见问题

#### pseudo-element render inside a container

**and <input> can not contain other elements.**

https://stackoverflow.com/questions/2587669/can-i-use-a-before-or-after-pseudo-element-on-an-input-field

Pseudo-elements can only be defined (or better said are only supported) on container elements. Because the way they are rendered is **within** the container itself as a child element. `input` can not contain other elements hence they're not supported. A `button` on the other hand that's also a form element supports them, because it's a container of other sub-elements.

