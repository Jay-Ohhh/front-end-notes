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

filename是主入口的文件名，chunkFilename是非主入口的文件名。

###### 占位符

| **模板**    | **描述**                                    |
| ----------- | ------------------------------------------- |
| [hash]      | 模块标识符(module identifier)的 hash        |
| [chunkhash] | chunk 内容的 hash                           |
| [name]      | 模块名称                                    |
| [id]        | 模块标识符(module identifier)               |
| [query]     | 模块的 query，例如，文件名 `?` 后面的字符串 |

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

###### splitChunks

使用webpack打包项目时一方面我们要防止单个文件太大，另一方面要防止文件碎片化，即打包文件太多，导致网络请求过多。所以合理的配置应该是兼顾打包文件的数量以及打包文件的个数。

**splitChunks.chunks**

Chunks 有三个提供的值，分别是 async、initial、all

- async 只对动态（异步）导入的模块进行分离
- initial 对所有模块进行分离，如果一个模块既被异步引用，也被同步引用，那么会生成两个包
- all 对所有模块进行分离，如果一个模块既被异步引用，也被同步引用，那么只会生成一个共享包

具体可以参考这篇文章: [Webpack 4 Mysterious SplitChunks Plugin](https://link.juejin.cn/?target=https%3A%2F%2Fmedium.com%2Fdailyjs%2Fwebpack-4-splitchunks-plugin-d9fbbe091fd0)

**splitChunks.minChunk**

某个包的引用次数必须大于等于设置的数值，该模包才能被拆分出来；

**splitChunks.minSize**

字面意思就可以看出来是满足相应大小的包才会被提取出来，单位是字节，minSize的值为30000，也就是说小于这个数的包不会被提取而是会被打到引用的文件中去，maxSize与之相反，但优先级小于minSize，另外值得注意的是，这两个值只对静态引入的公共包有影响，对于异步引入的包，不管大小多少哪怕minSize设置的很大，也同样是会被提取出来的。

maxSize如果为非0值时，切忌小于minSize；

**splitChunks.cacheGroups**

缓存组可以继承和/或覆盖来自 `splitChunks.*` 的任何选项。但是 `test`、`priority` 和 `reuseExistingChunk` 只能在缓存组级别上进行配置。将它们设置为 `false`以禁用任何默认缓存组。

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

- **root**：可以通过一个全局变量访问 library（例如，通过 script 标签）。

  > 通过 script 标签引入的库，应该为 umd 或 iife 格式。

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

- mini-css-extract-plugin：把js中import导入的样式文件，单独打包成一个css文件，结合html-webpack-plugin，以link的形式插入到html文件中。

  > 此插件不支持HMR，若修改了样式文件，是不能即时在浏览器中显示出来的，需要手动刷新页面。

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

- `optimize-css-assets-webpack-plugin` ：压缩css

- `clean-webpack-plugin`: build之前，将打包目录清理

- `compression-webpack-analyzer`：gzip压缩

- `webpack-bundle-analyzer`: 可视化分析包大小 (业务组件、依赖第三方模块)

  > vue cli 已经集成了该功能

- `webpackbar`：打包进度条

- `open-browser-webpack-plugin`：自动打开浏览器

- `copy-webpack-plugin`：将已存在的单个文件或整个目录复制到构建目录中

- `webpack.DefinePlugin`：这是一个简单的字符串替换插件，将我们所有经过 webpack 打包的 js 文件中代码对应的变量都替换为我们在这个插件中指定的其他值或表达式。DefinePlugin 允许创建一个在编译时可以配置的全局常量。

- `terser-webpack-plugin`：该插件使用 [terser](https://github.com/terser-js/terser) 来压缩 JavaScript，另外可以去除注释、console、debugger。

  webpack v5 开箱即带有最新版本的 `terser-webpack-plugin`。如果你使用的是 webpack v5 或更高版本，同时希望自定义配置，那么仍需要安装 `terser-webpack-plugin`。如果使用 webpack v4，则必须安装 `terser-webpack-plugin` v4 的版本。

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
-  babel-loader  options: {cacheDirectory :true} 指定的目录将用来缓存 loader 的执行结果。
- TerserWebpackPlugin：{parallel:true}  开启多线程

####  优化打包体积

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

- 确保没有编译器将您的 ES2015 模块语法转换为 CommonJS 的（顺带一提，这是现在常用的 @babel/preset-env 的默认行为，详细信息请参阅[文档](https://babeljs.io/docs/en/babel-preset-env#modules)）。

- 在项目的 `package.json` 文件中，添加 `"sideEffects"` 属性。

- 使用 `mode` 为 `"production"` 的配置项以启用[更多优化项](https://webpack.docschina.org/concepts/mode/#usage)，包括压缩代码与 tree shaking。

你可以将应用程序想象成一棵树。绿色表示实际用到的 source code(源码) 和 library(库)，是树上活的树叶。灰色表示未引用代码，是秋天树上枯萎的树叶。为了除去死去的树叶，你必须摇动这棵树，使它们落下。

如果你对优化输出很感兴趣，请进入到下个指南，来了解 [生产环境](https://webpack.docschina.org/guides/production) 构建的详细细节。



但是要想使其生效，生成的代码必须是ES6模块。不能使用其它类型的模块如`CommonJS`之流。如果使用`Babel`的话，这里有一个小问题，因为`Babel`的预案（preset）默认会将任何模块类型都转译成`CommonJS`类型，这样会导致`tree-shaking`失效。修正这个问题也很简单，在`.babelrc`文件或在`webpack.config.js`文件中设置`modules： false`就好了

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

看这个图就很明白了：

1. 对于一份同逻辑的代码，当我们手写下一个一个的文件，它们无论是 ESM 还是 commonJS 或是 AMD，他们都是 **module** ；
2. 当我们写的 module 源文件传到 webpack 进行打包时，webpack 会根据文件引用关系生成 **chunk** 文件，webpack 会对这个 chunk 文件进行一些操作；
3. webpack 处理好 chunk 文件后，最后会输出 **bundle** 文件，这个 bundle 文件包含了经过加载和编译的最终源文件，所以它可以直接在浏览器中运行。

一般来说一个 chunk 对应一个 bundle，比如上图中的 `utils.js -> chunks 1 -> utils.bundle.js`；但也有例外，比如说上图中，我就用 `MiniCssExtractPlugin` 从 chunks 0 中抽离出了 `index.bundle.css` 文件。

 

**总结：**

`module`，`chunk` 和 `bundle` 其实就是同一份逻辑代码在不同转换场景下的取了三个名字：

我们直接写出来的是 module，webpack 处理时是 chunk，最后生成浏览器可以直接运行的 bundle。

#### runtime和manifest

https://webpack.docschina.org/concepts/manifest/#root

webpack manifest用来引导所有模块的交互。manifest包含了加载和处理模块的逻辑。

当webpack编译器处理和映射应用代码时，它把模块的详细的信息都记录到了manifest中。当模块被打包并运输到浏览器上时，runtime就会根据manifest来处理和加载模块。利用manifest就知道从哪里去获取模块代码。

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

```sh
npm install --save-dev typescript ts-node @types/node @types/webpack
# 如果使用版本低于 v4.7.0 的 webpack-dev-server，还需要安装以下依赖
npm install --save-dev @types/webpack-dev-server
```

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

值得注意的是你需要确保 `tsconfig.json` 的 `compilerOptions` 中 `module` 选项的值为 `commonjs`,否则 webpack 的运行会失败报错，因为 `ts-node` 不支持 `commonjs` 以外的其他模块规范。

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

需安装 typescript 、ts-node

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

#### webpack、babel编译 ts 、tsx、jsx，搭建react环境

https://juejin.cn/post/7020972849649156110

ts-loader 可以转换 ts、tsx 文件

babel-loader 可以转换 js、jsx、ts、tsx 文件 （推荐）

这里我们选择使用 babel-loader 

```shell
npm i -D @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript @babel/runtime-corejs3 @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators core-js@3 babel-loader
```

```sh
npm i react react-dom
npm i -D @types/react @types/react-dom typescript 
```

- @babel/core——babel核心
- core-js——polyfill的类库

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

https://zhuanlan.zhihu.com/p/394782898 babel插件详解

```json

module.exports = {
  // 由于@babel/polyfill在7.4.0中被弃用，我们建议直接添加core js并通过corejs选项设置版本
  // @babel/env===@babel/preset-env  @babel/react === @babel/preset-react ...
  // 当在presets中使用@babel/env，babel会自动加上preset-
	presets: [
		[
			"@babel/preset-env",
			{
        // esm转换成其他模块语法，cjs、amd、umd等
        // Tree Shaking需要设置为false
				modules: false, 
				targets: { browsers: ["> 1%", "last 2 versions", "not ie <= 8"] },
        // when using useBuiltIns: "usage", set the proposals option to true. This will enable polyfilling of every proposal supported by core-js@xxx
        // 按需加载，将 useBuiltIns 改为 "usage"，babel 就可以按需加载 polyfill，并且不需要手动引入 @babel/polyfill，不过@babel/polyfill已被废弃，请使用安装core-js，并使用corejs选项
				useBuiltIns: "usage", 
				corejs: { 
          version: 3, // 需安装 core-js3.x的版本
          proposals: true, // 支持js提案语法
        }
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
    }], // 用于babel的编译(必须)，将重复的辅助函数自动替换，节省大量体积
		["@babel/plugin-proposal-decorators", { legacy: true }], // 需要放在@babel/plugin-proposal-class-propertie之前
		["@babel/plugin-proposal-class-properties", { loose: true }], // 用于解析class语法(react必选)
	]
}
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
