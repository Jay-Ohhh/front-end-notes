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

SS Modules 允许使用`:global(.className)`的语法，声明一个全局规则。凡是这样声明的`class`，都不会被编译成哈希字符串。

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
```

