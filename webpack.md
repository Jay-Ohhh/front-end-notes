#### 简介

**webpack**是一个用于现代 JavaScript 应用程序的 静态模块 打包工具。当 webpack 处理应用程序时，它会在内部构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 *bundle*。

#### 功能

- 代码转换
- 文件优化
- 代码分割
- 模块合并
- 自动刷新
- 代码校验
- 自动发布

#### 打包过程

https://juejin.cn/post/6854573217336541192#heading-5

- 获取模块内容
- 分析模块
- 收集依赖
- ES6转成ES5（AST）
- 递归获取所有依赖
- 处理两个关键字（import、exports）
- 输出打包结果代码

#### 配置项

##### ouput

###### filename、chunkFilename

此选项决定了每个输出 bundle 的名称。

**chunkFilename**

此选项决定了非入口(non-entry) chunk 文件的名称。

###### 占位符

| **模板**      | **描述**                         |
| ----------- | ------------------------------ |
| [hash]      | 模块标识符(module identifier)的 hash |
| [chunkhash] | chunk 内容的 hash                 |
| [name]      | 模块名称                           |
| [id]        | 模块标识符(module identifier)       |
| [query]     | 模块的 query，例如，文件名 `?` 后面的字符串    |

###### publicPath

```javascript
module.exports = {
  //...
  output: {
    // One of the below
    publicPath: 'auto', // It automatically determines the public path from either `import.meta.url`, `document.currentScript`, `<script />` or `self.location`.
    publicPath: 'https://cdn.example.com/assets/', // CDN（总是 HTTPS 协议）
    publicPath: '//cdn.example.com/assets/', // CDN（协议相同）
    publicPath: '/assets/', // 相对于服务(server-relative)
    publicPath: 'assets/', // 相对于 HTML 页面
    publicPath: '../assets/', // 相对于 HTML 页面
    publicPath: '', // 相对于 HTML 页面（目录相同）
  },
};
```

##### resolve

###### alias

创建 `import` 或 `require` 的别名

pakage.json

```json
resolve: {
    alias: { '@':  path.resolve(__dirname, 'src'), },
},
```

tsconfig.json

```json
{
    "compilerOptions": {
        "paths": {
            "@/*": ["./src/*"]
        }
    },
}
```

###### extensions

```
[string] = ['.js', '.json', '.wasm']
```

尝试按顺序解析这些后缀名。如果有多个文件有相同的名字，但后缀名不同，webpack 会解析列在数组首位的后缀的文件 并跳过其余的后缀。

**webpack.config.js**

```js
module.exports = {
  //...
  resolve: {
    extensions: ['.js', '.json', '.wasm'],
  },
};
```

能够使用户在引入模块时不带扩展：

```js
import File from '../path/to/file';
```

请注意，以上这样使用 `resolve.extensions` 会 **覆盖默认数组**，这就意味着 webpack 将不再尝试使用默认扩展来解析模块。然而你可以使用 `'...'` 访问默认拓展名：

```js
module.exports = {
  //...
  resolve: {
    extensions: ['.ts', '...'],
  },
};
```

##### devtool

此选项控制是否生成，以及如何生成 Source Map。

source map模式详细说明：https://blog.csdn.net/zwkkkk1/article/details/88758726

**开发环境**

- eval-source-map：每个模块使用eval()执行，而SourceMap作为DataUrl添加到eval()中。最初它是缓慢的，但是它提供快速的重建速度和产生真实的文件。行号被正确映射，因为它被映射到原始代码。它产生了最优质的开发资源。

**生产环境**

- （none）：省略配置devtool（默认），不生成 source map。

- nosources-source-map：创建的 source map 不包含 `sourcesContent(源代码内容)`。它可以用来映射客户端上的堆栈跟踪（暴露代码的行），而无须暴露所有的源代码。
  
  > 这仍然会暴露反编译后的文件名和结构，但它不会暴露原始代码。
  > 
  > 不暴露原始代码

> 开发环境 最佳：eval-cheap-module-source-map生产环境 最佳：hidden-source-map

##### optimization

###### chunkIds: 'deterministic'

假设有两个文件: `index.js` 和 `lib.js`，且 index 依赖于 lib，其内容如下。

**index.js**

```js
// index.js
import("./lib").then((o) => console.log(o));
```

**lib.js**

```js
export const a = 3;
```

由 webpack 等打包器打包后将会生生两个 chunk (为了方便讲解，以下 aaaaaa 为 hash 值)

- `index.aaaaaa.js`
- `lib.aaaaaa.js`

问: 假设 lib.js 文件内容发生变更，index.js 由于引用了 lib.js，可能包含其文件名，那么它的 hash 是否会发生变动

答: 不一定。打包后的 `index.js` 中引用 lib 时并不会包含 `lib.aaaaaa.js`，而是采用 chunkId 的形式，如果 chunkId 是固定的话，则不会发生变更。

```js
// 打包前
import("./lib");

// 打包后，201 为固定的 chunkId (chunkIds = deterministic 时)
__webpack_require__.e(/* import() | lib */ 201);
```

在 webpack 中，通过 `optimization.chunkIds` 可设置确定的 chunkId，来增强 Long Term Cache 能力。

```bash
{
  optimization: {
    chunkIds: 'deterministic'
  }
}
```

设置该选项且 `lib.js` 内容发生变更后，打包 chunk 如下，仅仅 `lib.js` 路径发生了变更。

- `index.aaaaaa.js`
- `lib.bbbbbb.js`



###### moduleIds: 'deterministic' 同上





###### splitChunks

使用webpack打包项目时一方面我们要防止单个文件太大，另一方面要防止文件碎片化，即打包文件太多，导致网络请求过多。所以合理的配置应该是兼顾打包文件的数量以及打包文件的个数。

**splitChunks.chunks**

https://mp.weixin.qq.com/s/HbgIXi0zpvU_-kdILHGi3A

Webpack 内部包含三种类型的 Chunk：

- Initial Chunk：基于 Entry 配置项生成的 Chunk，此 chunk 包含为入口起点指定的所有模块及其依赖项

> 因为是入口文件立即加载的模块，可以理解为同步引用，若不是立即加载，为异步引用

- Async Chunk：异步模块引用，如 `import(xxx)` 语句对应的异步 Chunk
- Runtime Chunk：只包含运行时代码的 Chunk

而 `SplitChunksPlugin` 默认只对 Async Chunk 生效，开发者也可以通过 `optimization.splitChunks.chunks`（默认 `async` ） 调整作用范围，该配置项支持如下值：

- 字符串 `'all'` ：对 Initial Chunk 与 Async Chunk 都生效，如果一个模块既被异步引用，也被入口同步引用，那么只会生成一个共享包，建议优先使用该值
- 字符串 `'initial'` ：只对 Initial Chunk 进行分离，不会将同步加载和异步加载一起处理，而是分开处理，如果一个模块既被异步引用，也被同步引用，那么会生成两个包
- 字符串 `'async'` ：只对 Async Chunk 进行分离，webpack会在用到的时候通过webpack jsonp方法动态创建script标签加载相应的文件，我们在react和Vue的懒加载路由中使用的也是这种方式
- 函数 `(chunk) => boolean` ：该函数返回 `true` 时生效

具体可以参考这篇文章: [Webpack 4 Mysterious SplitChunks Plugin](https://link.juejin.cn/?target=https%3A%2F%2Fmedium.com%2Fdailyjs%2Fwebpack-4-splitchunks-plugin-d9fbbe091fd0)

**最佳实践**

那么，如何设置最适合项目情况的分包规则呢？这个问题并没有放诸四海皆准的通用答案，因为软件系统与现实世界的复杂性，决定了很多计算机问题并没有银弹，不过我个人还是总结了几条可供参考的最佳实践：

1. **「尽量将高频第三方库拆为独立分包」**

例如在一个 React + Redux 项目中，可想而知应用中的大多数页面都会依赖于这两个库，那么就应该将它们从具体页面剥离，避免重复加载。`chunks: all`

但对于使用频率并不高的第三方库，就需要按实际情况灵活判断，例如项目中只有某个页面 A 接入了 Three.js，如果将这个库跟其它依赖打包在一起，那用户在访问其它页面的时候都需要加载 Three.js，最终效果可能反而得不偿失，这个时候可以尝试异步引入 Three.js 并使用异步加载功能将 Three.js 独立分包。`chunks: async`

2. **「保持按路由分包，减少首屏资源负载」**

设想一个超过 10 个页面的应用，假如将这些页面代码全部打包在一起，那么用户访问其中任意一个页面都需要等待其余 9 个页面的代码全部加载完毕后才能开始运行应用，这对 TTI 等性能指标明显是不友好的，所以应该尽量保持按路由维度做异步模块加载，所幸很多知名框架如 React、Vue 对此都有很成熟的技术支持

3.  **高频库**

   一个模块被 N(2 个以上) 个 Chunk 引用，可称为公共模块，可把公共模块给抽离出来，形成 `vendor.js`。

   问：那如果一个模块被用了多次 (2 次以上)，但是该模块体积过大(1MB)，每个页面都会加载它(但是无必要，因为不是每个页面都依赖它)，导致性能变差，此时如何分包？

   答：如果一个模块虽是公共模块，但是该模块体积过大，可直接 `import()` 引入，异步加载，单独分包，比如 `echarts` 等

   问：如果公共模块数量多，导致 vendor.js 体积过大(1MB)，每个页面都会加载它，导致性能变差，此时如何分包

   答：有以下两个思路

   1. 思路一: 可对 vendor.js 改变策略，比如被引用了十次以上，被当做公共模块抽离成 verdor-A.js，五次的抽离为 vendor-B.js，两次的抽离为 vendor-C.js
   2. 思路二: 控制 vendor.js 的体积，当大于 100KB 时，再次进行分包，多分几个 vendor-XXX.js，但每个 vendor.js 都不超过 100KB

**splitChunks.minChunks**

某个包的引用次数必须大于等于设置的数值，该模包才能被拆分出来；

“被 Chunk 引用次数”并不直接等价于被 `import` 的次数，而是取决于上游调用者是否被视作 Initial Chunk 或 Async Chunk 处理。



**splitChunks.minSize**

字面意思就可以看出来是满足相应大小的包才会被提取出来，单位是字节，minSize的值为30000，也就是说小于这个数的包不会被提取而是会被打到引用的文件中去，maxSize与之相反，但优先级小于minSize，另外值得注意的是，这两个值只对静态引入的公共包有影响，对于异步引入的包，不管大小多少哪怕minSize设置的很大，也同样是会被提取出来的。

maxSize如果为非0值时，不能小于minSize；

**splitChunks.cacheGroups**

缓存组可以继承和/或覆盖来自 `splitChunks.*` 的任何选项。但是 `test`、`priority` 和 `reuseExistingChunk` 只能在缓存组级别上进行配置。将它们设置为 `false`以禁用任何默认缓存组。

```js
optimization: {
        splitChunks: {
            //优化配置项...
            chunks: "all",
            cacheGroups: {
                vendors: {    //属性名即是缓存的名称，改成common看看
                    test: /[\\/]node_modules[\\/]/, 
                    priority: 2
                },
                default: {        //默认缓存组设置，会覆盖掉全局设置
                    minChunks: 2,
                    priority: 1,
                    reuseExistingChunk: true
                }
            }
        }
    },

```

在`cacheGroups`中，每一个对象就是一个`缓存组`分包策略，`属性名`便是`缓存组`的名称，对象中设置的值可以`继承`全局设置的属性（如`minChunks`等），也存在只有`缓存组`独特的属性，比如`test`（匹配模块名称规则）、`priority`（缓存组的优先级），`reuseExistingChunk`（重用已经被分离出的chunk）。



**splitChunks.cacheGroups.{cacheGroup}.priority**

```
number = -20
```

一个模块可以属于多个缓存组。优化将优先考虑具有更高 `priority`（优先级）的缓存组。默认组的优先级为负，以允许自定义组获得更高的优先级（自定义组的默认值为 `0`）。

###### runtimeChunk

runtimeChunk：将 runtime 代码拆分为一个单独的 chunk。

runtimeChunk：可用来优化持久化缓存的。runtime，以及伴随的 manifest 数据，主要是指：在浏览器运行过程中，webpack 用来连接模块化应用程序所需的所有代码。模块信息清单（runtime 代码、manifest 数据）在每次有模块变更(hash 变更)时都会变更, 所以我们想把这部分代码单独打包出来, 配合后端缓存策略, 这样就不会因为某个模块的变更导致包含模块信息的模块(通常会被包含在最后一个 bundle 中)缓存失效。optimization.runtimeChunk 就是告诉 webpack 是否要把这部分单独打包出来。

```js
// 常用配置
module.exports = {
  //...
  runtimeChunk: {
    name: 'manifest',
  }
};

new HtmlWebpackPlugin({
  chunks:['manifest'], // 加上 'manifest'
})
```

##### externals 选项（外部扩展）

 `string` `object` `function` `RegExp` `[string, object, function, RegExp]`

防止将某些 `import` 的包(package) 打包到 bundle 中，而是在运行时(runtime)再去从外部获取这些 扩展依赖(external dependencies)。

例如：

**webpack.config.js / vue.config.js**

```js
module.exports = {
  //...
  externals: {
    jquery: 'jQuery'
  }
};
```

外部 library 可能是以下任何一种形式：

- **root**：可以通过一个全局变量访问 library（例如，在浏览器环境下通过 script 标签）。
  
  > umd 或 iife 格式的库会暴露一个全局变量以供访问

- **commonjs**：可以将 library 作为一个 CommonJS 模块访问。

- **commonjs2**：和上面的类似，但导出的是 `module.exports.default`.

- **amd**：类似于 `commonjs`，但使用 AMD 模块系统。

可以接受以下语法……

###### string

请查看上面的例子。属性名称是 `jquery`，表示应该排除 `import $ from 'jquery'` 中的 `jquery` 模块。为了替换这个模块，`jQuery` 的值将被用来检索一个全局的 `jQuery` 变量。换句话说，当值为一个字符串时，它将被视为上述的`root`形式。

**若是通过 script 标签 中引入**，则在其引入的全局变量中寻找对应的变量`jQuery `代替 `jquery`。

> **script标签**中的全局作用域下声明变量，实际上是挂载在window上

另一方面，如果你想将一个符合 CommonJS 模块化规则的类库外部化，你可以提供外联类库的类型以及类库的名称。

```javascript
module.exports = {
  // ...
  externals: {
    'fs-extra': 'commonjs2 fs-extra',
  },
};
```

这样的做法会让任何依赖的模块都不变，正如以下所示的代码：

```javascript
import fs from 'fs-extra';
```

会将代码编译成：

```javascript
const fs = require('fs-extra');
```

###### [string]

```javascript
module.exports = {
  //...
  externals: {
    subtract: ['./math', 'subtract'],
  },
};
```

`subtract: ['./math', 'subtract']` 转换为父子结构，其中 `./math` 是父模块，而 bundle 只引用 `subtract` 变量下的子集。该例子会编译成 `require('./math').subtract;`

###### 对象

一个形如 `{ root, amd, commonjs, ... }` 的对象仅允许用于 [`libraryTarget: 'umd'`](https://webpack.docschina.org/configuration/output/#outputlibrarytarget) 这样的配置.它不被允许用于其它的 library targets 配置值.

```javascript
module.exports = {
  externals: {
    lodash: {
      commonjs: 'lodash',
      amd: 'lodash',
      root: '_', // 指向全局变量
    },
  },
};
```

若设置[ouput.library.type = 'umd'](https://webpack.docschina.org/configuration/output/#outputlibrarytype)，即将本项目打包成 `umd`模块， 而本项目用到 `lodash`这个外部库，则此语法用于描述外部 library 所有可用的访问方式。这里 `lodash` 这个外部 library 可以在 AMD 和 CommonJS 模块系统中通过 `lodash` 访问，但在全局变量形式下用 `_` 访问。`subtract` 可以通过全局 `math` 对象下的属性 `subtract` 访问（例如 `window['math']['subtract']`）。

###### RegExp

匹配给定正则表达式的每个依赖，都将从输出 bundle 中排除。

对于想要实现从一个依赖中调用多个文件的那些 library：

```js
import A from 'library/one';
import B from 'library/two';

// ...
```

无法通过在 externals 中指定整个 `library` 的方式，将它们从 bundle 中排除。而是需要逐个或者使用一个正则表达式，来排除它们。

```js
module.exports = {
  //...
  externals: [
    'library/one',
    'library/two',
    // 匹配以 "library/" 开始的所有依赖
    /^library\/.+$/,
  ],
};
```

#### loader和plugin

- loader：webpack 只能理解 JavaScript 和 JSON 文件。**loader** 让 webpack 能够去处理其他类型的文件，并将它们转换为有效 [模块](https://webpack.docschina.org/concepts/modules)
- plugin：plugins扩展了webpack的功能，在webpack运行时会广播很多事件，plugin可以监听这些事件，然后通过webpack提供的API来改变输出结果。

##### loader执行顺序

从右往左进行

```js
{
  test: /\.less$/,
  use: [
    'style-loader',
    'css-loader',
    'less-loader'
  ]
}
```

执行顺序其实是less-loader > css-loader > style-loader

**为什么是从右往左**

只是Webpack选择了compose方式，而不是pipe的方式而已，在技术上实现从左往右也不会有难度。

函数组合：函数组合是函数式编程中非常重要的思想。

函数组合的两种形式：一种是pipe，另一种是compose。前者从左向右组合函数，后者方向相反。

##### plugin执行顺序

plugin会绑定到webpack自身的事件钩子，如果多个plugin绑定同一个事件钩子，则按照配置文件数组索引顺序执行。

#### 常用 loader

https://juejin.cn/post/6844904031240863758#heading-9

https://webpack.docschina.org/loaders/

##### CSS

###### postcss-loader autoprefixer

```sh
npm i -D postcss-loader autoprefixer  
```

配置如下

```js
// webpack.config.js
module.exports = {
    module:{
        rules:[
            {
                test:/\.less$/,
                use:['style-loader','css-loader','postcss-loader','less-loader'] // 从右向左解析原则
           }
        ]
    }
} 
```

对于 Vue、React 的脚手架，内部均使用了 PostCSS，默认开启 autoprefixer 添加浏览器前缀

你可以根据 [Browserslist 规范](https://github.com/browserslist/browserslist#readme) 调整 `package.json` 中的 `browserslist` 来自定义目标支持浏览器。

###### style-loader和MiniCssExtractPlugin.loader

- style-loader：把js中import导入的样式文件打包到js文件中，运行js文件时，将样式自动插入到<style>标签中。

- mini-css-extract-plugin：把js中import导入的样式文件，单独打包成一个css文件并压缩，结合html-webpack-plugin，以link的形式插入到html文件中。

```js
// 编译less
                {
                    test: /\.less$/,
                    use: [
                        {
                            // 打包时用MiniCssExtractPlugin.loader替换掉style-loader
                            loader: MiniCssExtractPlugin.loader,
                            options: {
                                publicPath: '../',
                            },
                        },
                        {
                            loader: 'css-loader',
                            options: {
                                // 用于配置 css-loader 总用于 @import 的资源之前有多少个loader，0 => 无 loader(默认)，1=>post-loader，2=>postcss-loader，less-loader
                                importLoaders: 1,
                                modules: {
                                    compileType: 'module',
                                    auto: /\.module\.\w+$/i,
                                    localIdentName: '[path][name]_[local]_[hash:base64:5]',
                                    localIdentContext: srcPath,
                                },
                            },
                        },
                        { loader: 'postcss-loader', options: { postcssOptions: postcssOptions } },
                        { loader: 'less-loader', options: { sourceMap: false } },
                    ],
                },
```

###### 其余

- [`style-loader`](https://webpack.docschina.org/loaders/style-loader) 将模块导出的内容作为样式并添加到 DOM 中

- [`css-loader`](https://webpack.docschina.org/loaders/css-loader) 加载 CSS 文件并解析 import 的 CSS 文件，最终返回 CSS 代码

- [`less-loader`](https://webpack.docschina.org/loaders/less-loader) 加载并编译 LESS 文件

- [`sass-loader`](https://webpack.docschina.org/loaders/sass-loader) 加载并编译 SASS/SCSS 文件

- [`postcss-loader`](https://webpack.docschina.org/loaders/postcss-loader) 使用 [PostCSS](http://postcss.org/) 加载并转换 CSS/SSS 文件

- [`stylus-loader`](https://webpack.docschina.org/loaders/stylus-loader/) 加载并编译 Stylus 文件
  
  对于 Sass 等 CSS 预处理语言的 loader，在使用 Vue、React 的脚手架创建项目时根据你选择的CSS预处理语言已经配置好。

- [cache-loader](https://github.com/webpack-contrib/cache-loader#getting-started) 在一些性能开销较大的 loader 之前添加此 loader，以将结果缓存到磁盘里。请注意，保存和读取这些缓存文件会有一些时间开销，所以请只对性能开销较大的 loader 使用此 loader。

##### 语法

- [`babel-loader`](https://webpack.docschina.org/loaders/babel-loader) 使用 [Babel](https://babeljs.io/) 加载 ES2015+ 代码并将其转换为 ES5
- [`ts-loader`](https://github.com/TypeStrong/ts-loader) 像加载 JavaScript 一样加载 [TypeScript](https://www.typescriptlang.org/) 2.0+
- `eslint-loader`：通过 ESLint 检查 JavaScript 、TypeScript代码

##### 图片和字体

- `image-loader`：加载并且压缩图片文件

- `file-loader`：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)

- `url-loader`：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
  
  > create react app 已经设置了 导入小于 10,000 字节的图片将返回 [data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) 而不是路径，svg除外。

#### loader占位符

一些loader常见的占位符，具体的请查看loader的文档。

- `[name]` 源文件名称
- `[folder]` 文件夹相对于 `compiler.context` 或者 `modules.localIdentContext` 配置项的相对路径。
- `[path]` 源文件相对于 `compiler.context` 或者 `modules.localIdentContext` 配置项的相对路径。
- `[file]` - 文件名和路径。
- `[ext]` - 文件拓展名。
- `[hash]` - 字符串的哈希值。基于 `localIdentHashSalt`、`localIdentHashFunction`、`localIdentHashDigest`、`localIdentHashDigestLength`、`localIdentContext`、`resourcePath` 和 `exportName` 生成。
- `[<hashFunction>:hash:<hashDigest>:<hashDigestLength>]` - 带有哈希设置的哈希。
- `[local]` - 原始类名。

#### 常用插件

- `html-webpack-plugin`：该插件将为你生成一个 HTML5 文件，页面入口等

- `terser-webpack-plugin`: 支持压缩 ES6 (Webpack4)

- webpack-parallel-uglify-plugin: 多进程执行代码压缩，提升构建速度

- ```javascript
  new webpack.optimize.CommonsChunkPlugin(options); // 抽取公共代码
  ```

- `mini-css-extract-plugin`：将 CSS 提取到单独的文件中，为每个包含 CSS 的 JS 文件创建一个 CSS 文件，并且支持 CSS 和 SourceMaps 的按需加载。
  
  > 抽离 css 样式,防止将样式打包在 js 中文件过大和因为文件大网络请求超时的情况

- `optimize-css-assets-webpack-plugin` ：压缩css   For webpack v5 or above please use [css-minimizer-webpack-plugin](https://github.com/webpack-contrib/css-minimizer-webpack-plugin) instead.

- `css-minimizer-webpack-plugin`：压缩css 

- `clean-webpack-plugin`: build之前，将打包目录清理

- `compression-webpack-analyzer`：gzip压缩

- `webpack-bundle-analyzer`: 可视化分析包大小 (业务组件、依赖第三方模块)
  
  > vue cli 已经集成了该功能

- `webpackbar`：打包进度条

- `open-browser-webpack-plugin`：自动打开浏览器

- `copy-webpack-plugin`：将已存在的单个文件或整个目录复制到构建目录中

- `webpack.DefinePlugin`：这是一个简单的字符串替换插件，将我们所有经过 webpack 打包的 js 文件中代码对应的变量都替换为我们在这个插件中指定的其他值或表达式。DefinePlugin 允许创建一个在编译时可以配置的全局常量。

- `terser-webpack-plugin`：该插件使用 [terser](https://github.com/terser-js/terser) 来压缩 JavaScript，支持压缩 ES6，另外可以去除注释、console、debugger。
  
  webpack v5 开箱即带有最新版本的 `terser-webpack-plugin`。如果你使用的是 webpack v5 或更高版本，同时希望自定义配置，那么仍需要安装 `terser-webpack-plugin`。如果使用 webpack v4，则必须安装 `terser-webpack-plugin` v4 的版本。
  
  ```js
  module.exports = {
    optimization: {
      minimize: true,
      minimizer: [
        new TerserPlugin({
          terserOptions: {
            compress: {
              parallel: true,
              extractComments: true, // 剥离注释,
              terserOptions: {
                compress: {
                     // https://github.com/terser/terser
                  // 默认会去掉debugger
                  drop_console: true, // 去掉console
                  // pure_funcs:['console.log'] // 如果只想去除console.log，而不想去除console.info
                },
                      },
            },
          },
        }),
      ],
    },
  };
  ```

- `webpack.BannerPlugin`：为每个 chunk 文件头部添加 banner。

##### html-webpack-plugin

**inject**

注入选项。有四个选项值 true, body, head, false.

- true
  - 默认值，script标签位于html文件的 body 底部
- body
  - 同 true
- head
  - script 标签位于 head 标签内
- false
  - 不插入生成的 js 文件，只是单纯的生成一个 html 文件

**favicon**

给生成的 html 文件生成一个 favicon。属性值为 favicon 文件所在的路径名。

```
// webpack.config.js
...
plugins: [
    new HtmlWebpackPlugin({
        ...
        favicon: 'path/to/yourfile.ico'
    }) 
]
```

生成的 html 标签中会包含这样一个 link 标签

```
<link rel="shortcut icon" href="example.ico">
```

同 title 和 filename 一样，如果在模板文件指定了 favicon，会忽略该属性。

**minify**

minify 的作用是对 html 文件进行压缩，minify 的属性值是一个压缩选项或者 false 。默认值为false, 不对生成的 html 文件进行压缩。来看看这个压缩选项。

html-webpack-plugin 内部集成了 [html-minifier](https://link.segmentfault.com/?enc=jnwbINXHGXdtyX023fHyRA%3D%3D.Ci3TrbWFUN8CEjxA%2Be4RlQY0pmyFEx1O2YQFSHX5FzS4IhHXPuRQbVJ7A0aoA%2BkXduW4P36yC%2BNbNlCh7uhvUA%3D%3D) ,这个压缩选项同 html-minify 的压缩选项完全一样，
看一个简单的例子。

```
// webpack.config.js
...
plugins: [
    new HtmlWebpackPlugin({
        ...
        minify: {
            removeAttributeQuotes: true // 移除属性的引号
        }
    })
]
<!-- 原html片段 -->
<div id="example" class="example">test minify</div>
<!-- 生成的html片段 -->
<div id=example class=example>test minify</div>
```

**hash**

hash选项的作用是 给生成的 js 文件一个独特的 hash 值，该 hash 值是该次 webpack 编译的 hash 值。默认值为 false 。同样看一个例子。

```js
// webpack.config.js
plugins: [
    new HtmlWebpackPlugin({
        ...
        hash: true
    })
]
<script type=text/javascript src=bundle.js?22b9692e22e7be37b57e></script>
```

执行 webpack 命令后，你会看到你的生成的 html 文件的 script 标签内引用的 js 文件，是不是有点变化了。
bundle.js 文件后跟的一串 hash 值就是此次 webpack 编译对应的 hash 值。

```
$ webpack
Hash: 22b9692e22e7be37b57e
Version: webpack 1.13.2
```

**cache**

默认值是 true。表示只有在内容变化时才生成一个新的文件。

**showErrors**

showErrors 的作用是，如果 webpack 编译出现错误，webpack会将错误信息包裹在一个 pre 标签内，属性的默认值为 true ，也就是显示错误信息。

**chunks**

chunks 选项的作用主要是针对多入口(entry)文件。当你有多个入口文件的时候，对应就会生成多个编译后的 js 文件。那么 chunks 选项就可以决定是否都使用这些生成的 js 文件。

chunks 默认会在生成的 html 文件中引用所有的 js 文件，当然你也可以指定引入哪些特定的文件。

看一个小例子。

```js
// webpack.config.js
entry: {
    index: path.resolve(__dirname, './src/index.js'),
    index1: path.resolve(__dirname, './src/index1.js'),
    index2: path.resolve(__dirname, './src/index2.js')
}
...
plugins: [
    new HtmlWebpackPlugin({
        ...
        chunks: ['index','index2']
    })
]
```

执行 webpack 命令之后，你会看到生成的 index.html 文件中，只引用了 index.js 和 index2.js

```html
...
<script type=text/javascript src=index.js></script>
<script type=text/javascript src=index2.js></script>
```

而如果没有指定 chunks 选项，默认会全部引用。

**excludeChunks**

弄懂了 chunks 之后，excludeChunks 选项也就好理解了，跟 chunks 是相反的，排除掉某些 js 文件。 比如上面的例子，其实等价于下面这一行

```
...
excludeChunks: ['index1.js']
```

**chunksSortMode**

这个选项决定了 script 标签的引用顺序。默认有四个选项，'none', 'auto', 'dependency', '{function}'。

- 'dependency' 不用说，按照不同文件的依赖关系来排序。
- 'auto' 默认值，插件的内置的排序方式，具体顺序这里我也不太清楚...
- 'none' 无序？ 不太清楚...
- {function} 提供一个函数？但是函数的参数又是什么? 不太清楚...

> 如果有使用过这个选项或者知道其具体含义的同学，还请告知一下。。。

**xhtml**

一个布尔值，默认值是 false ，如果为 true ,则以兼容 xhtml 的模式引用文件。

#### 优化打包速度

- thread-loader  多线程打包
- cache-loader  缓存其之前的loaders的结果到磁盘中。读取和保存缓存文件会有开销，隐藏仅仅需要在一些性能开销较大的 loader 之前（数组index靠前）添加此 loader
- babel-loader  options: {cacheDirectory :true} 指定的目录将用来缓存 loader 的执行结果。
- TerserWebpackPlugin：{parallel:true}  开启多线程

#### 优化打包体积

##### webpack-bundle-analyzer

```sh
npm i -D webpack-bundle-analyzer
```

##### externals 选项（外部扩展）

将不需要打包的静态资源从构建逻辑中剔除出去，而使用 `CDN` 的方式，去引用它们

**webpack.config.js / vue.config.js**

```js
module.exports = {
  //...
  externals: {
    jquery: 'jQuery'
  }
};
```

##### 开启 gzip 压缩

**webpack.config.js**

```js
module.exports = {
    plugins: [
      // 打压缩包
      new CompressionWebpackPlugin({
        algorithm: 'gzip',
        test: new RegExp(
          '\\.(' +
          ['js', 'css'].join('|') +
          ')$'
        ),
        threshold: 1024,
        minRatio: 0.8
      }), 
    ]
};
```

##### Tree Shaking

Tree Shaking是一个术语，通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。

为了利用 *tree shaking* 的优势， 你必须...

- 使用 ES2015 模块语法（即 `import` 和 `export`）。
  
  > 因为在使用 CommonJS 时，会导入整个库(library)对象
  >
  > 但是在使用 ES6 模块时，可以只导入(import)我们所需的对象或函数
  >
  > 尽量使用 `name export`，export default 导出整体对象结果，不利于 tree shaking 进行分析；另一方面，export default 导出的结果可以随意命名变量，不利于团队统一管理。
  >
  > `React.lazy` 接受一个函数，这个函数需要动态调用 `import()`。它必须返回一个 `Promise`，该 Promise 需要 resolve 一个 `default` export 的 React 组件。
  
- 确保没有编译器将您的 ES2015 模块语法转换为 CommonJS 的（顺带一提，这是现在常用的 @babel/preset-env 的默认行为，详细信息请参阅[文档](https://babeljs.io/docs/en/babel-preset-env#modules)）。

- 在项目的 `package.json` 文件中，添加 `"sideEffects"` 属性。

- 使用 `mode` 为 `"production"` 的配置项以启用[更多优化项](https://webpack.docschina.org/concepts/mode/#usage)，包括压缩代码与 tree shaking。

你可以将应用程序想象成一棵树。绿色表示实际用到的 source code(源码) 和 library(库)，是树上活的树叶。灰色表示未引用代码，是秋天树上枯萎的树叶。为了除去死去的树叶，你必须摇动这棵树，使它们落下。

如果你对优化输出很感兴趣，请进入到下个指南，来了解 [生产环境](https://webpack.docschina.org/guides/production) 构建的详细细节。

但是要想使其生效，生成的代码必须是ES6模块。不能使用其它类型的模块如`CommonJS`之流。如果使用`Babel`的话，这里有一个小问题，因为`Babel`的预案（preset）默认会将任何模块类型都转译成`CommonJS`类型，这样会导致`tree-shaking`失效。修正这个问题也很简单，在`.babelrc`文件或在`webpack.config.js`文件中设置`modules: false`就好了

```js
// .babelrc
{
  "presets": [
    ["@babel/preset-env",
      {
        "modules": false
      }
    ]
  ]
}
```

或者

```js
// webpack.config.js

module: {
    rules: [
        {
            test: /\.js$/,
            use: {
                loader: 'babel-loader',
                options: {
                    presets: ['@babel/preset-env', { modules: false }]
                }
            }，
            exclude: /(node_modules)/
        }
    ]
}
```

##### package.json 中的 sideEffects

在 Webpack 的 Tree Shaking 配置中，有一个可以在 `package.json` 中配置的叫 `sideEffects`，这个 `sideEffects` 的配置主要是让 Webpack 这种 bundler 知道此项目是否可以做 Tree Shaking 的动作。

假如设定为 `false` 就代表可以将所有的文件进行 Tree Shaking，若读者知道有哪些档案是不能做 Tree Shaking 的，这时候只要在 `sideEffects` 内用一个数组将不能做 Tree Shaking 的文件路径写上去，这时候 bundler 就只会针对这个数组以外的文件进行 Tree Shaking。

```json
{
	// 如果所有代码都不包含副作用，我们就可以简单地将该属性标记为 false，来告知 webpack 它可以安全地删除未用到的 export。
	"sideEffects": false
	// 除了数组内的文件或文件夹，都可以进行 tree shaking
  "sideEffects": [
  	".style/*.css"
  ]
}
```



#### Modules

##### ES6

###### import() 动态引入

https://webpack.docschina.org/api/module-methods/#import

#### hash、chunkhash、contenthash

##### hash

每次构建的生成唯一的一个hash，且所有的文件hash串是一样的

##### chunkhash

chunk的修改才改变对应的hash值

##### contenthash

文件内容修改才会改变的hash值

通常而言：

`chunkHash`可以用于`js`打包`，contentHash`可以用到`css`文件打包。

#### chunk、bundle、module

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6ae2804df7064ee183b64c391e229f09~tplv-k3u1fbpfcp-watermark.awebp) 

**module**：不同文件类型的模块。Webpack 就是用来对模块进行打包的工具，这些模块各种各样，比如：js 模块、css 模块、sass 模块、vue 模块等等不同文件类型的模块。这些文件都会被 loader 转换为有效的模块，然后被应用所使用并且加入到依赖关系图中。相对于一个完整的程序代码，模块化的好处在于，模块化将程序分散成小的功能块，这就提供了可靠的抽象能力以及封装的边界，让设计更加连贯、目的更加明确。而不是将所有东西都揉在一块，既难以理解也难以管理。

**chunk**：数据块。

a. 一种是非初始化的：例如在打包时，对于一些**动态导入的异步代码**，webpack 会帮你分割出共用的代码，可以是自己写的代码模块，也可以是第三方库（node_modules 文件夹里的），这些被分割的代码文件就可以理解为 chunk。

b. 还有一种是初始化的：就是写在**入口文件处** (entry point) 的各种文件或者说模块依赖，就是 chunk ，它们最终会被捆在一起打包成一个 main.js （当然输出文件名你可以自己指定），这个 main.js 可以理解为 bundle，当然它其实也是 chunk。

**bundle**：捆绑好的最终文件。如果说，chunk 是各种片段，那么 bundle 就是一堆 chunk 组成的“集大成者”，比如上面说的 main.js 就属于 bundle。它经历了加载和编译的过程，是源文件的最终版本。





#### runtime和manifest

https://webpack.docschina.org/concepts/manifest/#root

webpack runtime 和 manifest用来引导所有模块的加载和连接交互。

当webpack complier执行、解析和映射应用程序时，它把模块的详细的信息都记录到了manifest中。当模块被打包并运输到浏览器上时，runtime就会根据manifest来处理和加载模块。利用manifest就知道从哪里去获取模块代码。

manifest数据被包含在某个js文件中，可使用 [`optimization.runtimeChunk`](https://webpack.docschina.org/configuration/optimization/#optimizationruntimechunk) 选项将 runtime 代码（包含manifest数据）拆分为一个单独的 chunk。

#### webpack、rollup、gulp

**在开发应用时使用 Webpack，开发库时使用 Rollup**

rollup的定位是偏向于JavaScript库的构建，打包的体积也比webpack的小。

Rollup有一项webpack不具有的功能：通过配置output.format开发者可以选择输出资源的模块形式（cjs（CommonJS），esm（ES6 Modules），amd，umd，iife，system等），此特性对于打包JavaScript库特别有用，因为往往一个库需要支持多种不同的模块形式，而通过rollup几个命令就可以把一份源代码打包为多份。

gulp强调的是前端开发的工作流程，我们可以通过配置一系列的task，定义task处理的事务（例如文件压缩合并、雪碧图、启动server、版本控制等），然后定义执行顺序，来让gulp执行这些task，从而构建项目的整个前端开发流程。

webpack是一个前端模块化方案，更侧重模块打包，我们可以把开发中的所有资源（图片、js文件、css文件等）都看成模块，通过loader（加载器）和plugins（插件）对资源进行处理，打包成符合生产环境部署的前端资源。

#### 常用功能代码

##### 动态生成 entry

```js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')
const glob = require('glob')

// 动态生成 entry 和 html-webpack-plugin
function getMpa() {
  const entry = {}, htmlPlugins = []
  const files = glob.sync('src/*/index.js')
  files.forEach((file) => {
    const filename = file.split('/')[1]
    entry[filename] = path.join(__dirname, file)
    htmlPlugins.push(
      new HtmlWebpackPlugin({
        template: path.join(__dirname, `src/${filename}/index.html`),
        filename: `${filename}.html`,
        chunks: [filename],
      })
    )
  })
  return { entry, htmlPlugins }
}
const mpa = getMpa()

// 动态的配置文件
module.exports = {
  entry: mpa.entry,
  output: {
    path: path.join(__dirname, 'dist'),
    filename: '[name]-[hash:6].js',
  },
  plugins: [...mpa.htmlPlugins],
}
```

#### 采用TS编写webpack配置

https://webpack.docschina.org/configuration/configuration-languages/#typescript

要使用 [Typescript](https://www.typescriptlang.org/) 来编写 webpack 配置，你需要先安装必要的依赖，比如 Typescript 以及其相应的类型声明，类型声明可以从 [DefinitelyTyped](https://definitelytyped.org/) 项目中获取，依赖安装如下所示：

```bash
npm install --save-dev typescript ts-node @types/node @types/webpack
# 如果使用版本低于 v4.7.0 的 webpack-dev-server，还需要安装以下依赖
npm install --save-dev @types/webpack-dev-server
```

完成依赖安装后便可以开始编写配置文件，示例如下：

**webpack.config.ts**

```typescript
import * as path from 'path';
import * as webpack from 'webpack';
// in case you run into any typescript error when configuring `devServer`
import 'webpack-dev-server';

const config: webpack.Configuration = {
  mode: 'production',
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js',
  },
};

export default config;
```

该示例需要 typescript 版本在 2.7 及以上，并在 `tsconfig.json` 文件的 compilerOptions 中添加 `esModuleInterop` 和 `allowSyntheticDefaultImports` 两个配置项。

值得注意的是你需要确保 `tsconfig.json` 的 `compilerOptions` 中 `module` 选项的值为 `commonjs`,否则 webpack 的运行会失败报错。

你可以通过三个途径来完成 module 的设置：

- 直接修改 `tsconfig.json` 文件

- 修改 `tsconfig.json` 并且添加 `ts-node` 的设置。

- 使用 `tsconfig-paths`

**第一种方法**就是打开你的 `tsconfig.json` 文件，找到 `compilerOptions` 的配置，然后设置 `target` 和 `module` 的选项分别为 `"ES5"` 和 `"CommonJs"` (在 `target` 设置为 `es5` 时你也可以不显示编写 `module` 配置)。

**第二种方法** 就是添加 ts-node 设置：

你可以为 `tsc` 保持 `"module": "ESNext"`配置，如果你是用 webpack 或者其他构建工具的话，为 ts-node 设置一个重载（override）。[ts-node 配置项](https://typestrong.org/ts-node/docs/imports/)

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "esModuleInterop": true, /* 通过为导入内容创建命名空间，实现CommonJS和ES模块之间的互操作性，esModuleInterop选项的作用是支持使用import d from 'cjs'的方式引入commonjs包 */
  },
  "ts-node": {
    "compilerOptions": {
      "module": "CommonJS"
    }
  }
}
```

**第二种方法**需要先安装 `tsconfig-paths` 这个 npm 包，如下所示：

```bash
npm install --save-dev tsconfig-paths
```

安装后你可以为 webpack 配置创建一个单独的 TypeScript 配置文件，示例如下：

**tsconfig-for-webpack-config.json**

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5",
    "esModuleInterop": true
  }
}
```

**Tip**

ts-node 可以根据 `tsconfig-paths` 提供的环境变量 `process.env.TS_NODE_PROJECT` 来找到 `tsconfig.json` 文件路径。

`process.env.TS_NODE_PROJECT` 变量的设置如下所示:

**package.json**

```json
{
  "scripts": {
    "build": "cross-env TS_NODE_PROJECT=\"tsconfig-for-webpack-config.json\" webpack"
  }
}
```

之所以要添加 `cross-env`，是因为我们在直接使用 `TS_NODE_PROJECT` 时遇到过 `"TS_NODE_PROJECT" unrecognized command` 报错的反馈，添加 `cross-env` 之后该问题也似乎得到了解决，你可以查看[这个 issue](https://github.com/webpack/webpack.js.org/issues/2733)获取到关于该问题的更多信息。

**package.json**

**需安装 typescript 、ts-node**

```json
{
    scripts:{
        "dev": "cross-env TS_NODE_PROJECT='./build/webpack.tsconfig.json' NODE_ENV=development webpack-dev-server --config build/webpack.conf.ts --color --progress"
    }
}
```

#### webpack初始化项目

- 如果你使用 webpack v4+ 版本，并且想要在命令行中调用 `webpack`，你还需要安装 [CLI](https://webpack.docschina.org/api/cli/)。

- [webpack-dev-server](https://github.com/webpack/webpack-dev-server) 可用于快速开发应用程序。请查阅 [开发指南](https://webpack.docschina.org/guides/development/) 开始使用。

> `webpack-dev-server` 会从 `output.path` 中定义的目录为服务提供 bundle 文件，即，文件将可以通过 `http://[devServer.host]:[devServer.port]/[output.publicPath]/[output.filename]` 进行访问。

- webpack-merge 合并webpack的多个配置，通常用来合并common和pro、dev的配置

```
$ mkdir test-app && cd test-app
$ npm init -y
$ npm install -D webpack webpack-cli webpack-dev-server webpack-merge cross-env
```

**package.json**

--color：Enable colors on console.

--progress：Print compilation progress during build.

```json
"scripts": {
        "start": "cross-env TS_NODE_PROJECT='./build/webpack.tsconfig.json' NODE_ENV=development webpack-dev-server --config build/webpack.conf.ts --color --progress",
        "build": "cross-env TS_NODE_PROJECT='./build/webpack.tsconfig.json' NODE_ENV=production webpack --config build/webpack.conf.ts --color --progress",
},
```

#### babel前置知识

Babel 转译后的代码要实现源代码同样的功能需要借助一些帮助函数，例如，`{ [name]: 'JavaScript' }` 转译后的代码如下所示：

```javascript
'use strict';
function _defineProperty(obj, key, value) {
  if (key in obj) {
    Object.defineProperty(obj, key, {
      value: value,
      enumerable: true,
      configurable: true,
      writable: true
    });
  } else {
    obj[key] = value;
  }
  return obj;
}
var obj = _defineProperty({}, 'name', 'JavaScript');
```

类似上面的帮助函数 _defineProperty 可能会重复出现在一些模块里，导致编译后的代码体积变大。Babel 为了解决这个问题，提供了单独的包 `@babel/runtime` 供编译模块复用工具函数。

启用插件 `@babel/plugin-transform-runtime` 后，Babel 就会使用 `@babel/runtime` 下的工具函数，转译代码如下：

```javascript
'use strict';
// 之前的 _defineProperty 函数已经作为公共模块 `@babel/runtime/helpers/defineProperty` 使用
var _defineProperty2 = require('@babel/runtime/helpers/defineProperty');
var _defineProperty3 = _interopRequireDefault(_defineProperty2);
function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }
var obj = (0, _defineProperty3.default)({}, 'name', 'JavaScript');
```

除此之外，babel 还为源代码的非实例方法（`Object.assign`，实例方法是类似这样的 `"foobar".includes("foo")`）和`@babel/runtime` 下的工具函数自动引用了 polyfill。这样可以避免污染全局命名空间，非常适合于 JavaScript 库和工具包的实现。例如 `const obj = {}, Object.assign(obj, { age: 30 });` 转译后的代码如下所示：

```javascript
'use strict';
// 使用了 core-js 提供的 assign
var _assign = require('@babel/runtime/core-js/object/assign');
var _assign2 = _interopRequireDefault(_assign);
function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }
var obj = {};
(0, _assign2.default)(obj, {
  age: 30
});
```

思考：babel-runtime 为什么适合 JavaScript 库和工具包的实现？

1. 避免 babel 编译的工具函数在每个模块里重复出现，减小库和工具包的体积；
2. 在没有使用 `@babel/runtime` 之前，库和工具包一般不会直接引入 `@babel/polyfill` 。否则像 Promise 这样的全局对象会污染全局命名空间，这就要求库的使用者自己提供 polyfill。这些 polyfill 一般在库和工具的使用说明中会提到，比如很多库都会有要求提供 es5 的 polyfill。在使用 `@babel/runtime`  后，库和工具只要在 package.json 中增加依赖 `@babel/runtime`  、 `@babel/polyfill`，交给 `@babel/runtime` 去引入 polyfill 就行了；

总结：

1. 具体项目还是需要使用`@babel/polyfill`，只使用 `@babel/runtime` 的话，实例方法不能正常工作（例如 `"foobar".includes("foo")`）；
2. JavaScript 库和工具可以使用 `@babel/runtime` ，在实际项目中使用这些库和工具，需要该项目本身提供 polyfill。

> @babel/polyfill在7.4.0已被废弃，请使用安装core-js，并使用corejs选项
> 
> core-js：标准的模块化 JS 库，包含最新的 ESMA 标准和提案，以及相关的 WHATWG/W3C 标准和提案的polyfill。

##### babel插件详解

https://zhuanlan.zhihu.com/p/394782898 babel插件详解

###### `@babel/core` 核心包

首先是 [@babel/core](https://zhuanlan.zhihu.com/p/270936113/@babel/core · Babel)代表 babel 编译器本身，提供了编程方式使用 babel 的 api。

###### @babel/preset-env

[@babel/preset-env](https://zhuanlan.zhihu.com/p/270936113/@babel/preset-env · Babel)是一个使你在**指定环境**使用**最新 js 语法**和**可选的 polyfill**的预设，首先这是个预设即意味着可以转换最新 js 语法，其他实现如下

**指定环境**

通过特定版本的浏览器和对应支持的特性及语法的映射，来确定对特定环境进行哪些处理。 可以通过 targets 选项设置，也可以通过.browserslistrc 文件或 package.json 中的 browserslist 字段配置，如果不指定环境则会转换所有的 ES6+到 ES5

**可选的 polyfill**

利用 **useBuiltIns** 的选项设置 core-js 的使用，注意会直接引用对应 core-js 实现，进而会污染全局作用域
可选值为："usage" | "entry" | false, defaults to false.
另外还需要结合 core-js 选项指定具体版本,其可选值为：2, 3 or { version: 2 | 3, proposals: boolean }, defaults to 2

```text
npm install core-js@3 --save
# or
npm install core-js@2 --save
```

对于 useBuiltIns 选项的各个值意义如下

- false 不使用 core-js
- entry 只能在 app 入口使用 import "core-js"; and import "regenerator-runtime/runtime"，引入多次会报错，会根据运行环境加载具体的包，比如 输入

```text
import "core-js";
```

输出（不同环境有所区别）

```text
import "core-js/modules/es.string.pad-start";
import "core-js/modules/es.string.pad-end";
```

- usage 当项目中使用对应 feature 时自动引入对应实现

###### @babel/plugin-transform-runtime

1. 利用 corejs 选项，在必要情况下利用 core-js 的 helper 函数进行 polyfill
2. 利用 helpers 选项，移除行内 babel helpers 函数，使其引用[@babel/runtime]
3. 利用 regenerator 选项可以在使用 generators/async 函数时自动引入[@babel/runtime/regenerator](https://link.zhihu.com/?target=https%3A//github.com/babel/babel/blob/main/packages/babel-runtime/regenerator/index.js)，而不会污染全局环境。

注意以上功能都是在@babel/runtime 存在的情况下使用的，而且后者会直接参与生产代码的构建，因此要作为生产依赖，而不是开发依赖。 

`corejs`

`false`, `2`, `3` or `{ version: 2 | 3, proposals: boolean }`, defaults to `false`.

通过提供了一个沙箱环境，利用 core-js 中实现的别名来 polyfill 对应功能，避免了污染全局。各个可选值表示以及对应依赖的 runtime helper 包分别为

- false 不会 polyfill，@babel/runtime
- 2，只支持全局变量和静态属性，@babel/runtime-corejs2
- 3，在 2 的基础上添加了实例属性，并且可以利用 proposals 选项启用提案的 polyfill，@babel/runtime-corejs3

| `corejs` option | Install command                             |
| --------------- | ------------------------------------------- |
| `false`         | `npm install --save @babel/runtime`         |
| `2`             | `npm install --save @babel/runtime-corejs2` |
| `3`             | `npm install --save @babel/runtime-corejs3` |

`helpers`

可选值为：boolean, defaults to true
babel 使用过程中的 helper 函数默认会分散在使用到的地方，开启这个选项就会移除行内 helper 函数，改为引用 runtime helper

`regenerator`

可选值：boolean, defaults to true. 开启后可以不污染全局作用域的转换 generator 函数

#### webpack、babel编译 ts 、tsx、jsx，搭建react环境

https://juejin.cn/post/7020972849649156110

ts-loader 可以转换 ts、tsx 文件

babel-loader 可以转换 js、jsx、ts、tsx 文件 （推荐）

这里我们选择使用 babel-loader 

```shell
npm i -D @babel/core @babel/preset-env @babel/preset-typescript @babel/preset-react @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators babel-loader
npm i @babel/runtime-corejs3
```

```sh
npm i react react-dom
npm i -D @types/react @types/react-dom typescript tslib ts-node
```

- ts-node —— 可选安装，在node.js上执行ts文件，可以方便调试ts文件。

- tslib —— 可选安装，TypeScript的运行库，包含所有TypeScript帮助函数。
  
  > ```
  > // tsconfig.json
  > // importHelpers：Allow importing helper functions from tslib once per project, instead of including them per-file.
  > // 使用该属性时需安装 tslib 
  > {
  >     "compilerOptions": {
  >         "importHelpers": true
  >     }
  > }
  > ```

在项目根目录配置 **tsconfig.json**

**webpack.config.js**

```json
{
        module: {
        rules: [
            // js、jsx、ts、tsx
            {
                test: /\.(j|t)sx?$/,
                use: [
                    { loader: 'cache-loader' },
                    { loader: 'thread-loader', options: { workers: 3 } },
                    { loader: 'babel-loader', options: { cacheDirectory: true } },
                ],
                include: [srcPath],
                exclude: /node_modules/,
            },
        ],
    },
}
```

**babel.config.js**

```json
module.exports = {
    // @babel/env === @babel/preset-env  @babel/react === @babel/preset-react ...
    // 当在presets中使用@babel/env，babel会自动加上preset-
    presets: [
        [
            "@babel/preset-env",
            {
                // esm转换成其他模块语法，cjs、amd、umd等
                // Tree Shaking需要设置为false
                modules: false,
                targets: { browsers: ["> 1%", "last 2 versions", "not ie <= 8"] },
            }
        ],
        "@babel/preset-react",
        "@babel/preset-typescript",
    ],
    plugins: [
        ["@babel/plugin-transform-runtime", {
            corejs: {
                version: 3, // 需安装 @babel/runtime-corejs3
                proposals: true
            }
        }],
        ["@babel/plugin-proposal-decorators", { legacy: true }], // 需要放在@babel/plugin-proposal-class-propertie之前
        ["@babel/plugin-proposal-class-properties", { loose: true }], // 用于解析class语法(react必选)
        // @babel/plugin-proposal-private-methods 和 @babel/plugin-proposal-private-property-in-object内置于preset-env,且他们的loose值必须与@babel/plugin-proposal-class-properties的一致
        ['@babel/plugin-proposal-private-methods', { 'loose': true }],
        ['@babel/plugin-proposal-private-property-in-object', { loose: true }],
    ]
}
```

#### 常用配置

[webpack 最佳实践](https://mp.weixin.qq.com/s/xNGQSW4CUn-neIUpiEkdXA)

```js
import path from 'path';
import fs from 'fs';
import fsExtra from 'fs-extra';
import chalk from 'chalk';
import slash from 'slash';
import ip from 'ip';
import clipboardy from 'clipboardy';
import forEach from 'lodash/forEach';
import webpack, { Configuration, DefinePlugin, HashedModuleIdsPlugin, HotModuleReplacementPlugin, Options } from 'webpack';
import WebpackMerge from 'webpack-merge';
import WebpackBar from 'webpackbar';
import MiniCssExtractPlugin from 'mini-css-extract-plugin'; // 将js中的css文件（即引入的css文件）单独抽离出来
import CopyWebpackPlugin from 'copy-webpack-plugin'; // 将已存在的单个文件或整个目录复制到构建目录中
import TerserPlugin from 'terser-webpack-plugin'; // 压缩JS，Webpack v5 已集成，如果需要自定义更多options，仍需安装该插件
import OptimizeCSSAssetsPlugin from 'optimize-css-assets-webpack-plugin';
import { CleanWebpackPlugin } from 'clean-webpack-plugin';
import { BundleAnalyzerPlugin } from 'webpack-bundle-analyzer';
import { settings } from './config';
import { scanJsEntry } from './webpack.scan-js-entry';
import HtmlWebpackPlugin from 'html-webpack-plugin';
import { WebpackAliyunOss } from './plugins/webpack-aliyun-oss';
import { CopyDistFiles } from './plugins/copy-dist-files';
import { aliOssConf, cdnPublicPath, enableCDN } from './oss.config';
const OpenBrowserPlugin = require('open-browser-webpack-plugin');
const CompressionPlugin = require('compression-webpack-plugin');
import AntdDayjsWebpackPlugin from 'antd-dayjs-webpack-plugin';
const FileManagerPlugin = require('filemanager-webpack-plugin');
const ReactRefreshWebpackPlugin = require('@pmmmwh/react-refresh-webpack-plugin');

// 是否是开发模式
const isDevMode = settings.mode === 'development';
// src文件夹绝对路径
const srcPath = path.resolve(settings.rootPath, './src');
// pages文件夹绝对路径
// const pagesPath = path.resolve(rootPath, `${pathPrefix}/src/pages`);
// public文件夹绝对路径
const publicPath = path.resolve(settings.rootPath, './public');
// node_modules文件夹绝对路径
const nodeModulesPath = path.resolve(settings.rootPath, './node_modules');
// 打包输出目录绝对路径
const distPath = path.resolve(settings.rootPath, './dist');
// 网站图标绝对路径
const faviconPath = path.resolve(settings.rootPath, './public/images/favicon.ico');
// 访问地址复制到剪切板(只干一次)
let copyToClipboard = false;

const copyPluginPatterns = [
	{ from: publicPath, to: './public' },
	{ from: `${srcPath}/utils/fastClick.js`, to: './public' },
	// 开发、生产环境下测试下载地址都需要把下面这样取消注释
	{ from: `${settings.rootPath}/CLodop_Setup_for_Win32NT.exe`, to: './static/software' },
	...[
		// react 相关
		'/react/umd/react.profiling.min.js',
		isDevMode ? '/react/umd/react.development.js' : '/react/umd/react.production.min.js',
		'/react-dom/umd/react-dom.profiling.min.js',
		isDevMode ? '/react-dom/umd/react-dom.development.js' : '/react-dom/umd/react-dom.production.min.js',
		isDevMode ? '/react-router-dom/umd/react-router-dom.js' : '/react-router-dom/umd/react-router-dom.min.js',
		// moment 相关
		isDevMode ? '/moment/min/moment-with-locales.js' : '/moment/min/moment-with-locales.min.js',
		// antd 相关
		// isDevMode ? '/antd/dist/antd.js' : '/antd/dist/antd.min.js',
		// isDevMode ? '/antd/dist/antd-with-locales.js' : '/antd/dist/antd-with-locales.min.js',
		// isDevMode ? '/antd/dist/antd.css' : '/antd/dist/antd.min.css',
		// isDevMode ? '/antd/dist/antd.compact.css' : '/antd/dist/antd.compact.min.css',
		// isDevMode ? '/antd/dist/antd.dark.css' : '/antd/dist/antd.dark.min.css',

		// amis 相关
		'/fastlion-amis/sdk/',
	].map((pathItem) => {
		return {
			from: `${slash(nodeModulesPath)}${pathItem}`,
			// 低版本才有这个字段transformPath
			transformPath: (targetPath: string, absolutePath: string) => {
				// 去除nodule_modules的路径：slash(absolutePath).substr(slash(nodeModulesPath).length
				return `./public${slash(absolutePath).substr(slash(nodeModulesPath).length)}`;
			},
		};
	}),
];

const config: Configuration = {
	entry: {
		schemaApp: `${srcPath}/schema-app`,
	},
	module: {
		// noParse: content => {},
		rules: [
			{ test: /\.json$/, use: 'json-loader', type: 'javascript/auto' },

			// 按照以往的套路，直接引用社区的三件套 raw-loader、url-loader、file-loader，安装依赖，配置依赖，一通操作下来就解决了问题。现在我们使用 webpack5就方便多了，不用安装任何依赖，直接修改 webpack.base.js 配置文件 https://webpack.docschina.org/guides/asset-modules/
			// webpack5的配置
			// 	{
			//     test: /\.(png|jpg|gif|jpeg|webp|svg|eot|ttf|woff|woff2)$/,
			//     type: 'asset',
			// },

			/**
			 * url-loader应该是file-loader上加了一层过滤。小于8k采用base64编码，减少一次http请求，大于8k则采用file-loader处理
			 */
			// 图片
			{
				test: /\.(png|jpe?g|gif|ico)$/,
				// limit 单位b，大于等于不会转成base64格式的字符串
				use: [{ loader: 'url-loader', options: { limit: 8192, name: 'images/[name].[hash:8].[ext]', publicPath: '' } }],
			},
			// 字体图标
			{
				test: /\.(woff|woff2|svg|eot|ttf)$/,
				// use: [{ loader: 'file-loader', options: { limit: 8192, name: 'fonts/[name].[ext]?[hash:8]', publicPath: '' } }],
				use: [{ loader: 'file-loader', options: { limit: 8192, name: 'fonts/[name].[hash:8].[ext]', publicPath: '' } }],
			},
			// 音频
			{
				test: /\.(wav|mp3|ogg)?$/,
				use: [{ loader: 'file-loader', options: { limit: 8192, name: 'audios/[name].[ext]?[hash:8]', publicPath: '' } }],
			},
			// 视频
			{
				test: /\.(ogg|mpeg4|webm)?$/,
				use: [{ loader: 'file-loader', options: { limit: 8192, name: 'videos/[name].[ext]?[hash:8]', publicPath: '' } }],
			},
			// js、jsx
			// {
			// 	test: /\.jsx?$/,
			// 	use: [{ loader: 'cache-loader' }, { loader: 'thread-loader', options: { workers: 3 } }, { loader: 'babel-loader', options: { cacheDirectory: true } }],
			// 	include: [srcPath],
			// 	exclude: /node_modules/,
			// },
			// js、jsx、ts、tsx
			{
				test: /\.(j|t)sx?$/,
				use: [
					{ loader: 'cache-loader' },
					{ loader: 'thread-loader', options: { workers: 3 } },
					// {loader: "ts-loader", options: {happyPackMode: true, transpileOnly: true}},
					{
						loader: 'babel-loader',
						options: {
							cacheDirectory: true,
							// React 快速刷新，会保存状态
							plugins: [isDevMode && require.resolve('react-refresh/babel')].filter(Boolean),
						},
					},
				],
				include: [srcPath],
				exclude: /node_modules/,
			},
		],
	},
	plugins: [
		// 这是一个简单的字符串替换插件，将我们所有经过 webpack 打包的 js 文件中代码对应的变量都替换为我们在这个插件中指定的其他值或表达式。
		// DefinePlugin 允许创建一个在编译时可以配置的全局常量。
		// 如果值是字符串，则字符串需要被引号包裹，例如 '"a"',或使用JSON.stringify("a")
		new DefinePlugin({ ...settings.define }),
		new AntdDayjsWebpackPlugin(),
	],
	resolve: {
		extensions: ['.ts', '.tsx', '.js', '.jsx', '.json'],
		modules: [srcPath, nodeModulesPath],
		alias: { '@': srcPath },
	},
	// externals 配置选项提供了「从输出的 bundle 中排除依赖」的方法，依赖不会被打包，而是在运行时(runtime)再去从外部获取这些扩展依赖(external dependencies)。
	// 例如遇到react，会寻找变量window.React
	externals: {
		react: 'window.React',
		'react-dom': 'window.ReactDOM',
		'react-router-dom': 'window.ReactRouterDOM',
		'fastlion-amis': { commonjs: 'amisRequire', amd: 'amisRequire', root: 'amisRequire' },
		// moment: 'moment',
		// antd: 'antd',
		// amis: { commonjs: 'amisRequire', amd: 'amisRequire', root: 'amisRequire' },
		// amis: "amisRequire",
	},
	optimization: {
		noEmitOnErrors: true,
	},
};

// postcss-loader 配置
const postcssOptions = {
	plugins: [
		['postcss-preset-env', {}],
		['autoprefixer', {}],
		['postcss-aspect-ratio-mini', {}],
		['postcss-write-svg', { utf8: false }],
		// ["postcss-px-to-viewport", {
		//   // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
		//   viewportWidth: 750,
		//   // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
		//   viewportHeight: 1334,
		//   // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
		//   unitPrecision: 3,
		//   // 指定需要转换成的视窗单位，建议使用vw
		//   viewportUnit: "vw",
		//   // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
		//   selectorBlackList: [".ignore", ".hairlines"],
		//   // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
		//   minPixelValue: 1,
		//   // 允许在媒体查询中转换`px`
		//   mediaQuery: false
		// }],
	],
};
// 开发模式
if (isDevMode) {
	// @ts-ignore
	// filename是主入口的文件名，chunkFilename是非主入口的文件名。
	const devConfig: Configuration = {
		output: {
			path: distPath,
			filename: 'js/[name].bundle.js',
			chunkFilename: 'js/[name].chunk.js',
			publicPath: '/',
		},
		mode: 'development',
		devtool: 'eval-source-map',
		module: {
			rules: [
				// css
				{
					test: /\.css$/,
					use: [
						{ loader: 'cache-loader' },
						{ loader: 'style-loader' },
						{ loader: 'css-loader', options: {} },
						{ loader: 'postcss-loader', options: { postcssOptions: postcssOptions } },
					],
				},
				// 编译less
				{
					test: /\.less$/,
					use: [
						{ loader: 'cache-loader' },
						{ loader: 'style-loader' },
						// css-loader 会对 @import 和 url() 进行处理，就像 js 解析 import/require() 一样
						{
							loader: 'css-loader',
							options: {
								importLoaders: 1,
								// css模块化 https://github.com/webpack-contrib/css-loader#modules
								modules: {
									compileType: 'module',
									// \w表示任意大小写字母或数字或下划线
									// +号表示1到多个\w，而后面的"$"号，表示限定以\w结尾
									// i不区分大小写
									auto: /\.module\.\w+$/i,
									// [path]：源文件相对于 compiler.context 或者 modules.localIdentContext 配置项的相对路径
									// [local] - 原始类名
									localIdentName: '[path][name]_[local]',
									// 默认：compiler.context
									localIdentContext: srcPath,
								},
							},
						},
						{ loader: 'postcss-loader', options: { postcssOptions: postcssOptions } },
						{ loader: 'less-loader', options: { sourceMap: true } },
					],
				},
			],
		},
		devServer: {
			host: settings.devServer.host,
			port: settings.devServer.port,
			contentBase: `${settings.rootPath}/index.html`,
			// publicPath: "/",
			historyApiFallback: true,
			overlay: true,
			hot: true,
			inline: true,
			noInfo: true,
			// 跳过域名检查
			disableHostCheck: false,
			// 服务端代理配置
			proxy: settings.devServer.proxy,
			// open: settings.devServer.needOpenApp && 'chrome',
		},
		plugins: [
			new CopyWebpackPlugin({
				patterns: [
					...copyPluginPatterns,
					// config.json不打包到dist，手动配置再放到服务器上
					{ from: `${settings.rootPath}/config.json`, to: './' },
				],
				options: { concurrency: 64 },
			}),
			new WebpackBar({
				reporter: {
					// allDone: (context) => {
					//   if (copyToClipboard) {
					//     return;
					//   }
					//   copyToClipboard = true;
					//   clipboardy.writeSync(`http://127.0.0.1:${settings.devServer.port}/schema-app.html`);
					//   const messages = [
					//     "  App running at:",
					//     `  - Local:   ${chalk.cyan(`http://127.0.0.1:${settings.devServer.port}/schema-app.html`)} (copied to clipboard)`,
					//     `  - Network: ${chalk.cyan(`http://${ip.address("public", "ipv4")}:${settings.devServer.port}/schema-app.html`)}`,
					//   ];
					//   console.log(messages.join("\n"));
					// },
					done: (context) => {
						if (!copyToClipboard) {
							copyToClipboard = true;
							clipboardy.writeSync(`http://localhost:${settings.devServer.port}/schema-app.html`);
						}
						const messages = [
							'  App running at:',
							`  - Local:   ${chalk.cyan(`http://localhost:${settings.devServer.port}/schema-app.html`)}`,
							`  - Network: ${chalk.cyan(`http://${ip.address('public', 'ipv4')}:${settings.devServer.port}/schema-app.html`)}`,
						];
						console.log(messages.join('\n'));
					},
				},
			}),
			new HotModuleReplacementPlugin(),
			new ReactRefreshWebpackPlugin(), // React 快速刷新
			new OpenBrowserPlugin({ url: 'http://localhost:8001/schema-app.html' }),
		],
	};
	// @ts-ignore
	config = WebpackMerge(config, devConfig);
}

// 生产模式
if (!isDevMode) {
	const prodConfig: Configuration = {
		// filename是主入口的文件名，chunkFilename是非主入口的文件名。
		output: {
			path: distPath,
			filename: 'js/[name].[chunkhash].bundle.js',
			chunkFilename: 'js/[name].[chunkhash].chunk.js',
			/**
				publicPath: 'https://cdn.example.com/assets/', // CDN（总是 HTTPS 协议）
				publicPath: '//cdn.example.com/assets/', // CDN（协议相同）
				publicPath: '', // 相对于 HTML 页面（目录相同）
			 */
			publicPath: enableCDN ? cdnPublicPath : '',
		},
		mode: 'production',
		module: {
			rules: [
				// css
				{
					test: /\.css$/,
					use: [
						{
							loader: MiniCssExtractPlugin.loader,
							/**
							 * publicPath默认是output.publicPath
							 * MiniCssExtractPlugin将.css文件打包在构建目录dist的CSS目录下
							 * url-loader和file-loader将图片、字体、视频文件打包在构建目录下的对应目录下
							 * 例如1.css文件中 background:url('@/assets/images/1.png')，打包后css文件目录为 dist/css/1.css, 图片文件目录为 dist/images/1.png，css文件夹和images文件夹同级
							 * 打包后的1.css文件中 background:url('../images/1.png')
							 *
							 * 如果dist里面的静态资源放在CDN服务器（不包括dist本身），output.publicPath设置为CDN地址，则无需设置以下的publicPath
							 * 例如：打包后的1.css文件中 background:url('CDN-URL/images/1.png')
							 */
							options: {
								publicPath: '../',
							},
						},
						{ loader: 'css-loader', options: {} },
						{ loader: 'postcss-loader', options: { postcssOptions: postcssOptions } },
					],
				},
				// 编译less
				{
					test: /\.less$/,
					use: [
						{
							// 打包时用MiniCssExtractPlugin.loader替换掉style-loader
							// style-loader：把js中import导入的样式文件打包到js文件中，运行js文件时，将样式自动插入到<style>标签中。
							// mini-css-extract-plugin：把js中import导入的样式文件，单独打包成一个css文件，结合html-webpack-plugin，以link的形式插入到html文件中。此插件不支持HMR，若修改了样式文件，是不能即时在浏览器中显示出来的，需要手动刷新页面。
							loader: MiniCssExtractPlugin.loader,
							options: {
								publicPath: '../',
							},
						},
						{
							loader: 'css-loader',
							options: {
								// 用于配置 css-loader 总用于 @import 的资源之前有多少个loader，0 => 无 loader(默认)，1=>post-loader，2=>postcss-loader，less-loader
								importLoaders: 1,
								modules: {
									compileType: 'module',
									auto: /\.module\.\w+$/i,
									localIdentName: '[path][name]_[local]_[hash:base64:5]',
									localIdentContext: srcPath,
								},
							},
						},
						{ loader: 'postcss-loader', options: { postcssOptions: postcssOptions } },
						{ loader: 'less-loader', options: { sourceMap: false } },
					],
				},
			],
		},
		plugins: [
			new CopyWebpackPlugin({
				patterns: copyPluginPatterns,
				options: { concurrency: 64 },
			}),
			new HashedModuleIdsPlugin(),
			new CompressionPlugin({
				filename: '[path][base].gz',
				algorithm: 'gzip',
				test: /\.(js|css|html|svg)$/,
				// 只处理大于xx字节 的文件，默认：0
				threshold: 10240,
				// 示例：一个1024b大小的文件，压缩后大小为768b，minRatio : 0.75
				minRatio: 0.8, // 默认: 0.8
				// 是否删除源文件，默认: false
				deleteOriginalAssets: false,
			}),
			// @ts-ignore
			new MiniCssExtractPlugin({
				ignoreOrder: true,
				filename: 'css/[name].[contenthash].css',
				chunkFilename: 'css/[name].[contenthash].css',
			}),
			// webpack 5.20.0+ 支持 ouput.clean
			new CleanWebpackPlugin({}),
			new WebpackBar({
				reporter: {
					allDone: (context) => {
						const messages = ['build completed'];
						console.log(messages.join('\n'));
					},
				},
			}),
			// 压缩
			new FileManagerPlugin({
				events: {
					onEnd: {
						archive: [{ source: `${settings.rootPath}/dist`, destination: `${settings.rootPath}/dist.zip` }],
					},
				},
			}),
		],
		optimization: {
			// moduleIds: 'deterministic' 在 webpack 5 中被添加，而且 moduleIds: 'hashed' 相应地会在 webpack 5 中废弃。
			// webpack5 生产环境长期缓存配置
			// chunkIds:'deterministic',
			// chunkIds: 'deterministic',
			moduleIds: 'hashed',
			// runtimeChunk：将 runtime 代码拆分为一个单独的 chunk。
			// runtimeChunk：可用来优化持久化缓存的。runtime，以及伴随的 manifest 数据，主要是指：在浏览器运行过程中，webpack 用来连接模块化应用程序所需的所有代码。模块信息清单（runtime 代码、manifest 数据）在每次有模块变更(hash 变更)时都会变更, 所以我们想把这部分代码单独打包出来, 配合后端缓存策略, 这样就不会因为某个模块的变更导致包含模块信息的模块(通常会被包含在最后一个 bundle 中)缓存失效. optimization.runtimeChunk 就是告诉 webpack 是否要把这部分单独打包出来。
			runtimeChunk: {
				name: 'manifest',
			},
			// 允许你通过提供一个或多个定制过的 TerserPlugin 实例，覆盖默认压缩工具(minimizer)。
			minimizer: [
				new TerserPlugin({
					parallel: true,
					extractComments: true, // 剥离注释,
					terserOptions: {
						compress: {
							// 默认会去掉debugger
							drop_console: true, // 去掉console
						},
					},
				}),
				new OptimizeCSSAssetsPlugin({
					cssProcessorPluginOptions: {
						preset: ['default', { discardComments: { removeAll: true } }],
					},
				}),
			],
			//可以在这里直接设置抽离代码的参数，最后将符合条件的代码打包至一个公共文件
			splitChunks: {
				// 设置缓存组用来抽取满足不同规则的chunk，满足这个条件输出为一个chunks
				cacheGroups: {
					commons: {
						chunks: 'all',
						minChunks: 2, // 最小共用次数
						name: 'commons',
						// minSize: 1024 * 1024,
						priority: 0,
					},
					'async-commons': {
						// 其余异步加载包
						chunks: 'async',
						minChunks: 2,
						name: 'async-commons',
						priority: 1,
					},
					schema: {
						// [xxx] 目标文本需包含【任意一个包含在括号中的元素】
						// [\\/] 兼容 \path\ 和 /path/
						test: /[\\/]src[\\/]pages[\\/].*\.(schema|react)\.(ts|tsx|js|jsx|json)$/,
						chunks: 'async',
						minSize: 1024 * 256,
						maxSize: 1024 * 1024,
						// 告诉 webpack 忽略 splitChunks.minSize、splitChunks.minChunks、splitChunks.maxAsyncRequests 和 splitChunks.maxInitialRequests 选项，并始终为此缓存组创建 chunk。
						// enforce: true,
						priority: 2,
					},
					// 提取 node_modules 中代码
					// vendor: {
					// 	name: 'vendor',
					// 	test: /[\\/]node_modules[\\/](react|react-dom|react-dom-router|babel-polyfill)[\\/]/,
					// 	chunks: 'all',
					// 	priority: 10,
					// },
					// 'fastlion-amis': {
					// 	name: 'fastlion-amis',
					// 	test: /[\\/]node_modules[\\/]fastlion-amis[\\/]/, // 这种写法兼容window、mac平台
					// 	chunks: 'async',
					// 	priority: 20,
					// },
					// monaco-editor
					monacoEditor: {
						name: 'monacoEditor',
						test: /[\\/]node_modules[\\/]monaco-editor[\\/]/,
						chunks: 'async',
						priority: 30,
					},
					// tinymce
					tinymce: {
						name: 'tinymce',
						test: /[\\/]node_modules[\\/]tinymce[\\/]/,
						chunks: 'async',
						priority: 40,
					},
					// echarts
					echarts: {
						name: 'echarts',
						test: /[\\/]node_modules[\\/]echarts[\\/]/,
						chunks: 'async',
						priority: 50,
					},
					// froalaEditor
					froalaEditor: {
						name: 'froalaEditor',
						test: /[\\/]node_modules[\\/]froala-editor[\\/]/,
						chunks: 'async',
						priority: 60,
					},
					// flvJs
					flvJs: {
						name: 'flvJs',
						test: /[\\/]node_modules[\\/]flv.js[\\/]/,
						chunks: 'async',
						priority: 70,
					},
					// hlsJs
					hlsJs: {
						name: 'hlsJs',
						test: /[\\/]node_modules[\\/]hls.js[\\/]/,
						chunks: 'async',
						priority: 80,
					},
				},
			},
		},
	};
	// @ts-ignore
	config = WebpackMerge(config, prodConfig);
}

// 动态扫描入口文件 entry HtmlWebpackPlugin
const imageHomeBackground = `data:image/png;base64,${fs.readFileSync(path.join(publicPath, './images/home-background.png'), { encoding: 'base64' })}`;
const imageAmisLogo = `data:image/png;base64,${fs.readFileSync(path.join(publicPath, './images/logo.png'), {
	encoding: 'base64',
})}`;
const imageLogo = `data:image/png;base64,${fs.readFileSync(path.join(publicPath, './images/logo.png'), {
	encoding: 'base64',
})}`;
const base64Images = { imageHomeBackground, imageAmisLogo, imageLogo };
const chunks: string[] = [];
if (config.optimization?.splitChunks && config.optimization?.splitChunks?.cacheGroups) {
	const cacheGroups = (config.optimization!.splitChunks as Options.SplitChunksOptions).cacheGroups as {
		[key: string]: Options.CacheGroupsOptions;
	};
	forEach(cacheGroups, (option) => {
		chunks.push(option.name as string);
	});
	console.log('chunks -> ', JSON.stringify(chunks));
}
scanJsEntry(config, srcPath, distPath, chunks, faviconPath, base64Images);

// schema-app 支持
const options: HtmlWebpackPlugin.Options = {
	template: `${srcPath}/template.ejs`,
	filename: `${distPath}/schema-app.html`,
	minify: false,
	title: settings.defaultHTMLTitle,
	keywords: settings.metaKeywords,
	description: settings.metaDescripttion,
	favicon: faviconPath,
	appVersion: settings.appVersion,
	/** chunks 选项的作用主要是针对多入口(entry)文件。当你有多个入口文件的时候，对应就会生成多个编译后的 js 文件。那么 chunks 选项就可以决定是否都使用这些生成的 js 文件。
	chunks 默认会在生成的 html 文件中引用所有的 js 文件，当然你也可以指定引入哪些特定的文件 */
	// manifest是 runtimeChunk 分离出来的chunk
	chunks: ['manifest', ...chunks, 'schemaApp'],
	//  ''：相对于 HTML 页面（目录同级）
	urlPrefix: enableCDN ? cdnPublicPath : '',
	isDevMode,
	...base64Images,
};
if (settings.mode === 'production') {
	options.minify = {
		removeRedundantAttributes: true,
		collapseWhitespace: true,
		removeAttributeQuotes: true,
		removeComments: true,
		collapseBooleanAttributes: true,
	};
}
config.plugins!.push(new HtmlWebpackPlugin(options));

// 生成代码分析报告
if (settings.needAnalyzer) {
	config.plugins!.push(
		new BundleAnalyzerPlugin({
			analyzerPort: 9528,
			analyzerMode: 'static',
			openAnalyzer: true,
			reportFilename: `${settings.rootPath}/out/report.html`,
		})
	);
}

// CDN支持(静态资源上传到阿里OSS)
if (enableCDN) {
	const webpackAliyunOss = new WebpackAliyunOss({
		// test: true,
		timeout: 1000 * 60 * 10,
		from: ['./dist/**', '!./dist/*.html', '!./dist/**/*.map', '!./dist/pages/**/*.html'],
		dist: `${aliOssConf.appPath}/${aliOssConf.appVersion}/`,
		region: aliOssConf.region,
		accessKeyId: aliOssConf.accessKeyId,
		accessKeySecret: aliOssConf.accessKeySecret,
		bucket: aliOssConf.bucket,
		// setOssPath(filePath: string) {
		//   return filePath;
		// },
		setHeaders(filePath: string) {
			return {
				// 缓存时间
				'Cache-Control': 'max-age=31536000',
			};
		},
	});
	config.plugins!.push(webpackAliyunOss);
	config.plugins!.push(
		new CopyDistFiles({
			onBefore: () => fsExtra.rmdirSync('./server/dist', { recursive: true }),
			patterns: [
				{
					from: './dist/**/*.html',
					transformPath: (targetPath, absolutePath) => {
						return `./server/dist/${slash(absolutePath).substr(slash(distPath).length)}`;
					},
				},
			],
		})
	);
}

export default config;
```

#### 搭建脚手架

https://mp.weixin.qq.com/s/C40Izv2Q3Wlv6t81RxYrbA

https://mp.weixin.qq.com/s/q2x-EAoeQFan5y64adk4Wg

https://mp.weixin.qq.com/s/xoYQeUhNSxXhAc3_l9xRJA

- [commander](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 提供 cli 命令与参数
- [glob](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 遍历文件
- [shelljs](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 常用的 shell 命令支持
- [prompts](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 读取控制台用户输入
- [fs-extra](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 文件读写等操作
- [inquirer](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 类似于 prompts
- [chalk](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 彩色日志
- [debug](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 类似于 chalk
- [execa](https://mp.weixin.qq.com/s?__biz=MzI1ODE4NzE1Nw==&mid=2247491366&idx=1&sn=ef7d34e289489547b352c2f746331567&chksm=ea0d55dcdd7adcca9e18ea49344b2100a5e587ea33e2125331ab46988178a0fbbd929f68f632&scene=126&sessionid=1644802905&subscene=207&key=cd1943ff5cfa93f320a19d0c6e3ea103aba9677a6d703b6c2d975ebb4264ff6f0be5306ba26534c65c54bfff1fe770932be4cf04397e5de1b2bcb3e52d48d91a3fbdf597f9df611f721eb90928165ee9d6db787f952b565144a5ffd59adf180bcb2cbed500280acd1ea7547bf0a8c2e7c8ace68f7be7e842b9a1d5bdca7ff967&ascene=0&uin=NjMxODk4NDM4&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=AVR8JBMfkP52oz2%2BCjBNXpE%3D&acctmode=0&pass_ticket=8UPJ8hYIh0hvfF5WxtSBB1zrgsMVuni9ytVnaJO%2FHs%2BwB4K282MYq6GT1WzQ%2Fr5k&wx_header=0&fontgear=2) —— 执行 shell 指令
