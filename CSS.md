#### 样式模块化 CSS Modules

https://www.ruanyifeng.com/blog/2016/06/css_modules.html

https://github.com/css-modules/css-modules

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

结局：在调用@keyframes的元素后面添加 :local 就行了

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
```

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
.webkitScrollbar(@className) {
	/*修改滚动条样式*/
	.@{className}::-webkit-scrollbar {
    // 宽高最好设置成一致，否则水平和垂直的滚动条粗细不一样
		width: 5px;
		height: 5px;
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

| 滚动条伪元素                    | 作用的位置                                              |
| ------------------------------- | ------------------------------------------------------- |
| ::-webkit-scrollbar             | 整个滚动条                                              |
| ::-webkit-scrollbar-button      | 滚动条上的按钮 (上下箭头)                               |
| ::-webkit-scrollbar-thumb       | 滚动条上的滚动滑块                                      |
| ::-webkit-scrollbar-track       | 滚动条轨道                                              |
| ::-webkit-scrollbar-track-piece | 滚动条没有滑块的轨道部分                                |
| ::-webkit-scrollbar-corner      | 当同时有垂直滚动条和水平滚动条时交汇的部分              |
| ::-webkit-resizer               | 某些元素的corner部分的部分样式(例:textarea的可拖动按钮) |

##### 移动端避免使用100vh

移动端浏览器的地址栏有时可见有时不可见。

 这些浏览器没有将`100vh`高度调整为视口高度变化时屏幕的可见部分，而是将`100vh`设置为浏览器的高度。这就导致地址栏可见时，设置的100vh的元素的高度比屏幕的可见部分高度要大，出现滚动条。

解决方案：

- 将高度设置为window.innerHeight

  iOS Safari解决：

- ```css
  body{
  	height:100vh;
  }
  @supports (-webkit-touch-callout:none){
  	body{
  		height:-webkit-fill-available;
  	}
  }
  ```

  

