### create react app

#### 快速开始

##### 安装create react app

```sh
npm install create-react-app -g
```

##### 创建应用程序

npx来自npm5.2+或更高版本

```sh
npx create-react-app my-app
cd my-app
npm start
```

然后打开 http://localhost:3000/ 查看你的应用。

##### 文件架构

```
my-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    └── serviceWorker.js
```

对于要构建的项目，这些文件必须以确切的文件名存在：

- `public/index.html` 是页面模板;
- `src/index.js` 是 JavaScript 入口点。

你可以在 `src` 中创建子目录。 为了加快重新构建的速度，Webpack 只处理 `src` 中的文件。 你需要**将任何 JS 和 CSS 文件放在 `src` 中**，否则 Webpack 将发现不了它们。

**manifest.json（可选）**

manifest.json文件是5+移动App的配置文件，用于指定应用的显示名称、图标、入口页面等信息。你可以在该文件内对你的网页做一些配置，当用户访问网页，生成一个网页的桌面快捷方式时，会以这个文件中的内容作为图标和文字的显示内容。

> HTML5 Plus移动App，简称5+App，是一种基于HTML、JS、CSS编写的运行于手机端的App，这种App可以通过扩展的JS API任意调用手机的原生能力，实现与原生App同样强大的功能和性能。

**serviceWorker.js（可选）**

serviceWorker.js 用于移动端web开发，可以使你的react项目变成一个PWA，也就是说，在线上，只要访问过一次你的网站，下一次即使没有网络，这个应用依然可以被访问。

> Progressive Web Application，简称PWA，渐进式网页应用。简单的理解，就是网页可以通过某种方式达到离线使用，还可以拥有类似其他手机app一样拥有一个logo，名称，启动页面等等。

配置manifest.json, 使用serviceWorker.js，用户完全可以把你的网页快捷方式放到桌面上，因为你的网页此时支持离线访问，所以用起来和原生app的体验很接近。

**react-app-env.d.ts**

如果是使用 TypeScript 创建新的 Create React App 项目，则会在src文件夹下生成 react-app-env.d.ts 文件

react-app-env.d.ts是全局变量的声明文件，常用于react、react-dom的一些API类型声明，图片、样式模块类型声明等等。

> 在全局变量的声明文件中，最外层是不允许出现 `import`, `export` 关键字的。一旦出现了，那么他就会被视为一个 npm 包或 UMD 库，就不再是全局变量的声明文件了。故当我们在书写一个全局变量的声明文件时，如果需要引用另一个库的类型，那么就必须用三斜线指令了。

##### Scripts

**`npm start` 或 `yarn start`**

在开发模式下运行应用程序。 打开 [http://localhost:3000](http://localhost:3000/) 在浏览器中查看它。

如果你更改代码，页面将自动重新加载。 你将在控制台中看到构建错误和 lint 警告。

**`npm test` 或 `yarn test`**

以交互模式运行测试观察程序。 默认情况下，运行与上次提交后更改的文件相关的测试。

**`npm run build` 或 `yarn build`**

将生产环境的应用程序构建到 `build` 目录。 它能将 React 正确地打包为生产模式中并优化构建以获得最佳性能。

构建将被压缩，文件名中将包含哈希。

**`npm run eject`**

运行行`npm run eject`命令会把项目的配置文件暴露出来。

注意：这是单向操作。一旦你 `eject` ，你就不能回去了！

如果你对构建工具和配置选项不满意，可以随时 `eject` 。此命令将从项目中删除单个构建依赖项。

相反，它会将所有配置文件和传递依赖项（Webpack，Babel，ESLint等）复制到项目中，以便你可以完全控制它们。除 `eject` 之外的所有命令仍然有效，但它们将指向复制的脚本，以便你可以调整它们。接下来你只能靠自己了。

##### [react-app-rewired](https://github.com/timarney/react-app-rewired/blob/HEAD/README_zh.md)

react-app-rewired是react社区开源的一个修改CRA（ create-react-app）配置的工具。

此工具可以在不 'eject' 也不创建额外 react-scripts 的情况下修改 create-react-app 内置的 webpack 配置，然后你将拥有 create-react-app 的一切特性，且可以根据你的需要去配置 webpack 的 plugins, loaders 等。

**使用**

1.安装

```sh
npm install react-app-rewired --save-dev
```

2.在根目录中创建一个 config-overrides.js 文件

```js
/* config-overrides.js */
module.exports = function override(config, env) {
  //do stuff with the webpack config...
  return config;
}
```

3.替换 package.json 中 scripts 执行部分

```diff
  /* package.json */

  "scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test --env=jsdom",
+   "test": "react-app-rewired test --env=jsdom",
    "eject": "react-scripts eject"
}
```

> 注意：不用替换 `eject` 部分。工具中没有针对 `eject` 的配置替换，执行了 eject 命令会让工具失去作用（能找到这个插件我也相信你知道 eject 是干嘛的）。

4.启动 Dev Server

```sh
npm start
```

5.构建你的应用程序

```sh
npm run build
```

**扩展配置选项**

默认情况下，该 `config-overrides.js` 文件导出单个函数，以便在开发或生产模式下自定义 webpack 配置, 方便编译您的 react 应用程序。可以从该文件中导出一个包含最多三个字段的对象，每个字段都是一个函数。这种替代形式允许您自定义用于 Jest（在测试中）和 Webpack Dev Server 本身的配置。 此示例实现用于演示使用每个对象需要的函数。

**1) Webpack 配置 - 开发和生产**

该 `webpack` 字段用于提供与 config-overrides.js 导出的单个函数的等效项。这是使用所有常用 rewire 的地方。它无法在测试模式下配置编译，因为测试模式根本无法通过 Webpack 运行（它在 Jest 中运行）。它也不能用于自定义用于在开发模式下提供页面的 Webpack Dev Server，因为 create-react-app 生成一个单独的 Webpack 配置，以便与使用不同函数和默认配置的 dev 服务器一起使用。

**2）Jest 配置 - 测试**

Webpack 不用于在测试模式下编译应用程序 - 而是使用 Jest。这意味着您的 webpack 配置自定义函数中指定的任何重连接*都不会在*测试模式下应用于您的项目。

**3) Webpack Dev Server**

在开发模式下运行时，create-react-app 不会为 Development Server（提供应用程序页面的服务器）使用常用 Webpack 配置。这意味着您无法使用服务器的常规 `webpack` 在`config-overrides.js` 中的部分来更改 Development Server 设置，因为这些更改将不会应用。

与此相反，create-react-app 期望能够在需要时调用函数来生成 webpack dev 服务器。此函数提供了在 webpack dev 服务器中使用的 proxy 和 allowedHost 设置的参数（create-react-app 从package.json 文件中检索这些参数的值）。

##### customize-cra

`customize-cra` 是依赖于 `react-app-rewired` 的库，通过 `config-overrides.js` 来修改底层的 `webpack`，`babel`配置。

`config-overrides.js` 创建在项目根目录下，是`react-app-rewird` 包所利用的文件，结合`customize-cra`可以轻松修改底层配置，而不用运行 `npm run eject` 来暴露`webpack.config.js` 来修改配置。

**打包不产生sourcemap**

**config-overrides.js**

```js
const { override } = require('customize-cra')
const rewiredMap = () => config => {
  config.devtool =
    config.mode === 'development' ? 'cheap-module-source-map' : false
  return config
}
module.exports = override(
  rewiredMap(),
)
```

或者 

**config-overrides.js**

```js
process.env.GENERATE_SOURCEMAP = "false";
```

例如：

**设置路径别名**：addWebpackAlias

```js
const { addWebpackAlias } = require("customize-cra");
const path = require('path');
module.exports = override(    
    addWebpackAlias({        
        ["mock"]: path.resolve(__dirname, "src/mock"),        
        ["containers"]: path.resolve(__dirname, "src/containers"),        
        ["components"]: path.resolve(__dirname, "src/components")   
    })
)
```

更多：addLessLoader(loaderOptions)、addDecoratorsLegacy() 等API，请参考

https://github.com/arackaf/customize-cra/blob/master/api.md#fixbabelimportslibraryname-options

##### Craco

与customize-cra相似，Craco是一个对 create-react-app 进行自定义配置的社区解决方案，但**Craco**并不依赖 **react-app-rewired**。更推荐使用**Craco**。**Craco**用法与**vue.config.js**更加相似。

使用：

1.安装

```sh
$ yarn add @craco/craco

# OR

$ npm install @craco/craco --save
```

2.在根目录创建 `craco.config.js`文件

3.替换 package.json 中 scripts 执行部分

```diff
/* package.json */

"scripts": {
-   "start": "react-scripts start",
+   "start": "craco start",
-   "build": "react-scripts build",
+   "build": "craco build"
-   "test": "react-scripts test",
+   "test": "craco test"
}
```

4.启动 Dev Server

```sh
npm start
```

5.构建你的应用程序

```sh
npm run build
```

**配置文件craco.config.js**

https://www.jianshu.com/p/7de3d3b15ff3

**起别名**

```js
const path = require('path');
module.exports = {
    webpack: {
        // 别名
        alias: {
            "@": path.resolve(__dirname, "src"),
        },
    }
}       
```

**打包配置压缩文件**

```sh
npm install compression-webpack-plugin --save //打包build生成gizp压缩文件
```

使用Express也可以做gzip压缩—compression模块。

```tsx
const CompressionWebpackPlugin = require('compression-webpack-plugin');
const webpack = require('webpack')
module.exports = {
    webpack: {
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
            new webpack.IgnorePlugin(/^\.\/locale$/, /moment$/),
    ]
};
```

⚠️compression-webpack-plugin 打包的文件生成 .gz后缀的文件需要服务器配置支持。

> 需要确保服务器那边，开启了gzip功能。
>
> 其实前端这些配置，按理说是可以省略的，因为服务器那边直接开启gzip功能，也能有压缩效果。
>
> 那为啥还要前端这边去配置生成.gz文件呢？如果前端这边提供了.gz文件，服务器收到请求时，就直接用这个文件了，但如果前端没有提供.gz文件，服务器会自动去压缩生成，这一步压缩生成，是需要时间滴。

**查看打包分析明细**

```sh
npm i webpack-bundle-analyzer --save -dev
```

```jsx
const { BundleAnalyzerPlugin } = require("webpack-bundle-analyzer");
const webpackPlugins = []
if (process.env.NODE_ENV === 'production') {
  // 打包分析
  const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer')
  // npm run build的时候浏览器会自动打开9090端口的一个页面显示当前依赖包的各种大小拼图
  webpackPlugins.push(new BundleAnalyzerPlugin({ analyzerPort: 9090 }))
}
module.exports = {
    webpack: {
        plugins: webpackPlugins,
    }
};
```

⚠️：生产版本关闭此项

**打包忽略console,debugger**

```sh
npm install uglifyjs-webpack-plugin@1 --save-dev
```


 （⚠️ 必须为1.0版本，否则打包报错）

```js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
const webpack = require('webpack')
module.exports = {
    webpack: {
        plugins: [
             new UglifyJsPlugin({
                uglifyOptions: {
                    compress: {
                        warnings: false,
                        drop_debugger: true,
                        drop_console: true,
                    },
                },
                sourceMap: false,
                parallel: true,
            }),
                ]
};
```

**装饰器**

```sh
npm i @babel/plugin-proposal-decorators --save -dev
```

```java
module.exports = {
    babel: {
        plugins: [
            ['import', { libraryName: 'antd', libraryDirectory: 'es', style: 'css' }],
            ['@babel/plugin-proposal-decorators', { legacy: true }]
        ]
    }
};
```

**Craco-less**

> 不需再额外下载 less 和 less-loader

JS环境：[craco-less](https://github.com/DocSpring/craco-less) 插件来帮助加载 less 样式和修改变量。

```sh
$ yarn add craco-less
```

```js
const CracoLessPlugin = require('craco-less');

module.exports = {
  plugins: [
    {
      plugin: CracoLessPlugin,
      options: {
        lessLoaderOptions: {
          lessOptions: {
            modifyVars: { '@primary-color': '#1DA57A' },
            javascriptEnabled: true,
          },
        },
      },
    },
  ],
};
```

TS环境： [craco-antd](https://github.com/DocSpring/craco-antd) 插件来帮助加载 less 样式和修改变量，集成了**less**和**moment**。

```sh
$ yarn add craco-antd
```

```js
const CracoAntDesignPlugin = require('craco-antd');

module.exports = {
  plugins: [
    {
      plugin: CracoAntDesignPlugin,
      options: {
        customizeTheme: {
          '@primary-color': '#1DA57A',
        },
      },
    },
  ],
};
```

#### 支持的浏览器和JS等语言特性

##### 支持的浏览器

默认情况下，生成的项目支持所有现代浏览器。 如果你的项目想支持 Internet Explorer 9 , 10 和 11 ，那么需要 [polyfills](https://github.com/facebook/create-react-app/blob/master/packages/react-app-polyfill/README.md)。

##### 支持的语言特性

该项目支持最新 JavaScript 标准的超集。 除了 [ES6](http://www.html.cn/archives/6963) 语法功能外，它还支持：

- [指数运算符](http://www.html.cn/archives/7735) (ES2016).
- [Async/await](http://www.html.cn/archives/7980) (ES2017).
- [Object Rest(剩余)/Spread(展开) 属性](http://www.html.cn/archives/9990) (ES2018).
- [动态 import()](https://github.com/tc39/proposal-dynamic-import) (stage 3 proposal)
- [Class 字段和静态属性](https://github.com/tc39/proposal-class-public-fields) (part of stage 3 proposal).
- [JSX](https://facebook.github.io/react/docs/introducing-jsx.html), [Flow](http://www.html.cn/create-react-app/docs/supported-browsers-features/adding-flow) 和 [TypeScript](http://www.html.cn/create-react-app/docs/supported-browsers-features/adding-typescript).

#### 开发

##### 在编辑器中显示 Lint 输出

先为编辑器安装 ESLint 插件。 然后，将名为 `.eslintrc.json` 的文件添加到项目根目录：

```json
{
  "extends": "react-app"
}
```

请注意，即使你进一步编辑 `.eslintrc.json` 文件，这些更改也 **只会影响编辑器集成**。它们不会影响终端和浏览器中的 lint 输出。这是因为 Create React App 有意提供了一组最常见的错误规则。

如果要为项目强制执行编码风格，请考虑使用 [Prettier](https://github.com/jlongster/prettier) 而不是 ESLint 样式规则。

##### 生产环境打包关闭sourceMap

在根目录建立 **.env.production**文件

文件内：

```
GENERATE_SOURCEMAP=false
```

**tsconfig.json**

```json
{
  "compilerOptions": {
  	"sourceMap": false,
  	}
} 
```

##### 分析 Bundle (包) 大小

[Source map explorer](https://www.npmjs.com/package/source-map-explorer) 使用 source maps 分析 JavaScript 包。 这有助于你了解代码膨胀的来源。

要将 Source map explorer 添加到 Create React App 项目，请按照下列步骤操作：

```sh
npm install --save-dev source-map-explorer
```

然后在 `package.json` 中，将以下行添加到 `scripts` 中：

```diff
   "scripts": {
+    "analyze": "source-map-explorer build/static/js/main.*",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
```

然后分析 bundle(包) 运行生产构建然后运行分析脚本。

```sh
npm run build
npm run analyze
```

> 注意：需要开启sourceMap打包生成.map文件，才能npm run analyze。sourceMap默认为true。
>
> 建议使用 webpack-bundle-analyzer 进行分析。webpack-bundle-analyzer不需开启sourceMap。

##### 在开发环境中使用 HTTPS

你可能需要开发服务器通过 HTTPS 提供页面。 当 API 服务器本身为 HTTPS 服务时，使用 ["proxy"（代理）功能](http://www.html.cn/create-react-app/docs/proxying-api-requests-in-development) 将请求代理到 API 服务器，这可能是有用的。

为此，请将 `HTTPS` 环境变量设置为 `true` ，然后像往常一样使用 `npm start` 启动开发服务器：

**Windows (cmd.exe)**

```cmd
set HTTPS=true&&npm start
```

（注意：缺少空格是故意的。）

**Windows (Powershell)**

```Powershell
($env:HTTPS = "true") -and (npm start)
```

**Linux, macOS (Bash)**

```bash
HTTPS=true npm start
```

请注意，服务器将使用自签名证书，因此你的 Web 浏览器在访问页面时基本上会显示警告。

##### 隔离开发组件（了解）

###### Storybook

Storybook 是 React UI 组件的开发环境。 它允许你浏览组件库，查看每个组件的不同状态，以及交互式开发和测试组件。

在应用程序的目录中运行以下命令：

```sh
npx -p @storybook/cli sb init
```

之后，请按照说明进行操作。

了解有关 React Storybook 的更多信息：

- [学习 Storybook (tutorial)](https://learnstorybook.com/)
- [文档](https://storybook.js.org/basics/introduction/)
- [GitHub 仓库](https://github.com/storybooks/storybook)
- [Snapshot Testing UI](https://github.com/storybooks/storybook/tree/master/addons/storyshots) 使用 Storybook + addon/storyshot

#### 样式和资源

##### 添加一个样式表

 JavaScript 文件依赖于某个 CSS 文件，你需要 **在 JavaScript 文件中导入 CSS**：

`Button.css`

```css
.Button {
  padding: 20px;
}
```

`Button.js`

```js
import React, { Component } from 'react';
import './Button.css'; // 告诉 Webpack Button.js 使用这些样式

class Button extends Component {
  render() {
    // 你可以将它们用作常规 CSS 样式
    return <div className="Button" />;
  }
}
```

##### 添加 CSS Modules 样式表

使用 `[name].module.css` 文件命名约定支持 [CSS Modules](https://github.com/css-modules/css-modules) 和常规 CSS 。 CSS Modules 允许通过自动创建 `[filename]\_[classname]\_\_[hash]` 格式的唯一 classname 来确定 CSS 的作用域。

> 如果要使用 Sass 预处理 CSS，然后将 CSS 文件扩展名更改为：`[name].module.scss` 或 `[name].module.sass` 。

CSS Modules 允许你在不同的文件中使用相同的 CSS classname，而无需担心命名冲突。 在此处 [了解有关 CSS Modules 的更多信息](https://css-tricks.com/css-modules-part-1-need/)。

`Button.module.css`

```css
.error {
  background-color: red;
}
```

`Button.js`

```js
import React, { Component } from 'react';
import styles from './Button.module.css'; // 将 css modules 文件导入为 styles

class Button extends Component {
  render() {
    // 作为 js 对象引用
    return <button className={styles.error}>Error Button</button>;
  }
}
```

##### 添加Sass样式表

要使用Sass，首先安装 `node-sass` ：

```bash
$ npm install node-sass --save
```

> sass-loader已在脚手架集成，不需额外下载

现在你可以将 `src/App.css` 重命名为 `src/App.scss` 并更新 `src/App.js` 以导入 `src/App.scss` 。 此文件和任何其他文件将会自动编译，如果使用扩展名 `.scss` 或 `.sass` 导入的话。

要在 Sass 文件之间共享变量，可以使用 Sass 导入。 例如，`src/App.scss` 和其他组件样式文件可能包含 `@import "./shared.scss";` 中定义的变量。

App.scss 文件允许你像这样导入：

```scss
@import 'styles/_colors.scss'; // 假设 styles 目录 在 src/ 目录下
@import '~antd/dist/antd.less'; // 从模块导入 一个 less 文件到 App.less
```

> **注意：** 你必须在 `node_modules` 之前添加 `~` 前缀，如上所示。

##### PostCSS（后处理CSS）

Create React App 会压缩你的 CSS 并通过 [Autoprefixer](https://github.com/postcss/autoprefixer) 自动添加浏览器前缀，因此你无需担心它。

支持全新CSS 特性，[`all` 属性](https://developer.mozilla.org/en-US/docs/Web/CSS/all), [`break` 属性](https://www.w3.org/TR/css-break-3/#breaking-controls), [自定义属性](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables), and [媒体查询范围](https://www.w3.org/TR/mediaqueries-4/#range-context) 会自动进行 polyfill，以添加对旧版浏览器的支持。

你可以根据 [Browserslist 规范](https://github.com/browserslist/browserslist#readme) 调整 `package.json` 中的 `browserslist` 来自定义目标支持浏览器。

例如，这个样式：

```css
.App {
  display: flex;
  flex-direction: row;
  align-items: center;
}
```

将变成:

```css
.App {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  -ms-flex-direction: row;
  flex-direction: row;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}
```

如果由于某种原因需要禁用 Autoprefixer，请 [按照本节进行操作](https://github.com/postcss/autoprefixer#disabling) 。

默认情况下 禁用了 [CSS Grid(网格) 布局](http://www.html.cn/archives/8510) 前缀，但不会删除手动前缀。 如果你想选择加入 CSS Grid(网格) 前缀，请先 [熟悉一下它的局限性](https://github.com/postcss/autoprefixer#does-autoprefixer-polyfill-grid-layout-for-ie)。
要启用 CSS Grid(网格) 前缀，请将 `/* autoprefixer grid: on */` 添加到 CSS 文件的顶部。

##### 使用public文件夹（在模块系统之外添加静态资源）

**建议你改为在 JavaScript 文件中导入资源。**

这种机制提供了许多好处：

- 脚本和样式被压缩并打包在一起，以避免额外的网络请求。
- 缺少文件会导致编译错误，而不是给用户 404 错误。
- 结果文件名包含内容哈希，因此你无需担心浏览器会缓存旧版本。

但是，你可以使用 **escape hatch(逃生舱)** ，在模块系统之外添加资源。

如果将文件放入 `public` 文件夹，Webpack 将 **不会** 处理它。相反，它将被复制到构建文件夹中。要引用 `public` 文件夹中的资源，需要使用名为 `PUBLIC_URL` 的特殊变量。

在 `index.html` 中，你可以像这样使用它：

```html
<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">
```

只有 `%PUBLIC_URL%` 前缀才能访问 `public` 文件夹中的文件。如果你需要使用 `src` 或 `node_modules` 中的文件，则必须将其复制到 `public` 文件夹，以显式指定将该文件作为构建的一部分的意图。

当你运行 `npm run build` 时，Create React App 将使用正确的绝对路径替换 `%PUBLIC_URL%` 。

在 JavaScript 代码中，你可以使用 `process.env.PUBLIC_URL` 用于类似目的：

```js
render() {
  // 注意：这是一个逃生舱（escape hatch），应该谨慎使用！
  // 通常我们建议使用`import`来获取资源的 URL 
  // 如我们上一节 “添加图片和字体”中所述。
  return <img src={process.env.PUBLIC_URL + '/img/logo.png'} />;
}
```

这种方法的缺点：

- `public` 文件夹中的所有文件都不会进行后处理或压缩。
- 在编译时不会调用丢失的文件，并且会导致用户出现 404 错误。
- 结果文件名不包含内容哈希值，因此你需要添加查询参数或在每次更改时重命名它们（以便清除浏览器缓存）。

**何时使用 `public` 文件夹**

通常我们建议从 JavaScript 导入 [stylesheets](http://www.html.cn/create-react-app/docs/adding-a-stylesheet) ，[图片和字体](http://www.html.cn/create-react-app/docs/adding-images-fonts-and-files) 。 `public`文件夹可用作许多不常见情况的变通方法：

- 你需要在构建输出中具有特定名称的文件，例如 [`manifest.webmanifest`](https://developer.mozilla.org/en-US/docs/Web/Manifest) 。
- 你有数千张图片，需要动态引用它们的路径。
- 你希望在打包代码之外包含一个像 [`pace.js`](http://github.hubspot.com/pace/docs/welcome/) 这样的小脚本。
- 某些库可能与 Webpack 不兼容，你没有其他选择，只能将其包含为 `<script>` 标记。

##### 添加图片，字体和文件

使用 Webpack，添加图片和字体等静态资源的工作方式与 CSS 类似。

你可以 **直接在 JavaScript 模块中 `import` 文件**。 这会告诉 Webpack 将该文件包含在 bundle(包) 中。 与 CSS 导入不同，导入文件会为你提供字符串值。 此值是你可以在代码中引用的最终路径，例如 image 的 `src` 属性或链接到 PDF 的 `href` 属性。

要减少对服务器的请求数，导入小于 10,000 字节的图片将返回 [data URI](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URIs) 而不是路径。 这适用于以下文件扩展名：`bmp` ，`gif` ，`jpg` ，`jpeg` 和 `png` 。 由于 [#1153](https://github.com/facebook/create-react-app/issues/1153) ，SVG文件被排除在外。

例如：

```jsx
import React from 'react';
import logo from './logo.png'; // 告诉 Webpack 这个 JS 文件使用了这个图片

console.log(logo); // /logo.84287d09.png

function Header() {
  // 导入结果是图片的 URL 
  return <img src={logo} alt="Logo" />;
}
```

##### 添加 SVG

直接导入 SVG 作为 React 组件。你可以使用这两种方法中的任何一种。在你的代码中，它看起来像这样：

```jsx
import { ReactComponent as Logo } from './logo.svg';
const App = () => (
  <div>
    {/* Logo 是一个实际的 React 组件 */}
    <Logo />
  </div>
);
```

如果你不想将 SVG 作为单独的文件加载，这很方便。不要忘记导入中的花括号！ `ReactComponent` 导入名称是特殊的，它告诉 Create React App 你想要一个渲染 SVG 的 React 组件，而不是它的文件名。

#### 代码拆分

支持通过 [动态`import()`](http://2ality.com/2017/01/import-operator.html#loading-code-on-demand) 进行代码拆分。 `import()` 接受模块名作为参数，，并返回一个[`Promise`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) 

示例：

`moduleA.js`

```js
const moduleA = 'Hello';

export { moduleA };
```

`App.js`

```js
import React, { Component } from 'react';

class App extends Component {
  handleClick = () => {
    import('./moduleA')
      .then(({ moduleA }) => {
        // Use moduleA
      })
      .catch(err => {
        // Handle failure
      });
  };

  render() {
    return (
      <div>
        <button onClick={this.handleClick}>Load</button>
      </div>
    );
  }
}

export default App;
```

这将使 `moduleA.js` 及其所有唯一依赖项成为一个单独的块，仅在用户单击 'Load' 按钮后加载。 

##### 路由按需加载

如果你使用的是 React Router ，请查看 [这个教程](http://serverless-stack.com/chapters/code-splitting-in-create-react-app.html)，或查看自己写的 React Router文档笔记。

#### 构建App

##### 安装依赖项

使用 `npm` 安装其他依赖项（例如，React Router）：

```sh
npm install --save react-router-dom
```

或者你可以使用 `yarn`:

```sh
yarn add react-router-dom
```

这适用于任何库，而不仅仅是 `react-router-dom` 。

##### 导入组件

们建议你使用 [`import` 和 `export`](http://exploringjs.com/es6/ch_modules.html)，虽然你仍然可以使用 `require()` 和 `module.exports` 。

请注意 [默认导出和命名导出之间的区别](http://stackoverflow.com/questions/36795819/react-native-es-6-when-should-i-use-curly-braces-for-import/36796281#36796281)。 这是一个常见的错误来源。

我们建议你在模块仅导出单个内容（例如，组件）时坚持使用默认`import` 和 `export`。 当你使用 `export default Button` 时，可以使用 `import Button from './Button'`导入。

命名导出对于导出多个函数的实用程序模块很有用。 一个模块最多可以有一个默认导出和任意多个命名导出。

##### 全局变量

```js
const $ = window.$;
```

##### 添加 Typescript

将 [TypeScript](https://www.typescriptlang.org/) 添加到 Create React App 项目，请先安装它：

```bash
$ npm install --save typescript @types/node @types/react @types/react-dom @types/jest
$ # 或者
$ yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

使用 [TypeScript](https://www.typescriptlang.org/) 创建新的 Create React App 项目，你可以运行：

```bash
$ npx create-react-app my-app --typescript
$ # 或者
$ yarn create react-app my-app --typescript
```

接下来，将任何文件重命名为 TypeScript 文件（例如 `src/index.js` 重命名为 `src/index.tsx` ）并 **重新启动开发服务器**！

类型错误将显示在与构建错误相同的控制台中。

> **注意：** 你不需要制作 [`tsconfig.json` 文件](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)，我们将为你制作一个文件。 你可以编辑生成 TypeScript 配置。
>
> **注意：** 不支持常量枚举和命名空间。

##### 添加自定义环境变量（端口等）

你的项目可以使用在你的环境中声明的变量，就好像它们是在 JS 文件中本地声明的变量一样。默认情况下，你将定义以 `REACT_APP_` 开头的任何其他环境变量。

除了一些内置变量（ `NODE_ENV` 、`PORT`、 `GENERATE_SOURCEMAP`和 `PUBLIC_URL`）之外，变量名必须以 `REACT_APP_` 开头才能工作。

**环境变量在构建期间嵌入**。由于 Create React App 生成静态的 HTML / CSS / JS 包，因此无法在 runtime(运行时) 读取它们。要在运行时读取它们，你需要将HTML 加载到服务器上的内存中，并在运行时替换占位符，就像 [此处所述](http://www.html.cn/create-react-app/docs/title-and-meta-tags#injecting-data-from-the-server-into-the-page) 。或者，你可以随时在服务器上重建应用程序。

在 `process.env` 上定义环境变量。例如，名为 `REACT_APP_SECRET_CODE` 的环境变量将在你的JS中公开为 `process.env.REACT_APP_SECRET_CODE` 。

**在 HTML 中引用环境变量**

你还可以在 `public/index.html` 中以 `REACT_APP_` 开头访问环境变量。 例如：

```html
<title>%REACT_APP_WEBSITE_NAME%</title>
```

- 除了一些内置变量（ `NODE_ENV` 、`PORT`、`GENERATE_SOURCEMAP`和 `PUBLIC_URL`）之外，变量名必须以 `REACT_APP_` 开头才能工作。
- 环境变量在构建时注入。 如果需要在运行时注入它们，[请改为使用此方法](http://www.html.cn/create-react-app/docs/title-and-meta-tags#generating-dynamic-meta-tags-on-the-server) 

 **process.env.NODE_ENV** 

还有一个名为 `NODE_ENV` 的特殊内置环境变量，有三个值：'development' | 'production' | 'test'。你可以从 `process.env.NODE_ENV` 中读取它。当你运行 `npm start` 时，它总是等于 `'development'` ，当你运行 `npm test` 它总是等于 `'test'` ，当你运行 `npm run build` 来生成一个生产 bundle(包) 时，它总是等于 `'production'` 。**你无法手动覆盖`NODE_ENV`。** 这可以防止开发人员意外地将慢速开发构建部署到生产环境中。

```js
if (process.env.NODE_ENV === 'production') {
  // ...
}
```

**定义环境变量**

这可以使用两种方式完成：在shell中 或`.env` 文件  。

**在 Shell 中添加临时环境变量**

在shell中定义环境变量可能因操作系统而异。

Windows (cmd.exe)

```cmd
set "REACT_APP_SECRET_CODE=abcdef" && npm start
```

（注意：变量赋值需要用引号包裹，以避免尾随空格。）

设置打开端口

```cmd
set PORT=5000
```

或将package.json的"script"的"start"改成：

```js
"set PORT=5000 && npm start"
// 如果你是使用craco
"set PORT=5000 && craco start"
```

Windows (Powershell)

```Powershell
($env:REACT_APP_SECRET_CODE = "abcdef") -and (npm start)
```

Linux, macOS (Bash)

```bash
REACT_APP_SECRET_CODE=abcdef npm start
```

**在 `.env` 中添加开发环境变量**

在项目的根目录中创建名为 `.env` 的文件：

```
REACT_APP_SECRET_CODE=abcdef
```

> 注意：你必须以 `REACT_APP_` 开头创建自定义环境变量。除了 `NODE_ENV` 之外的任何其他变量都将被忽略，以避免[意外地在可能具有相同名称的计算机上公开私钥](https://github.com/facebook/create-react-app/issues/865#issuecomment-252199527)。更改任何环境变量都需要重新启动正在运行的开发服务器。

**在 HTML 中引用环境变量**

```html
<title>%REACT_APP_SECRET_CODE%</title>
```

**在JS引用环境变量**

```js
process.env.REACT_APP_SECRET_CODE
```

**还可以使用哪些其他 `.env` 文件？**（了解）

- `.env`：默认。
- `.env.local`：本地覆盖。**除 test 之外的所有环境都加载此文件**。
- `.env.development`, `.env.test`, `.env.production`：设置特定环境。
- `.env.development.local`, `.env.test.local`, `.env.production.local`：设置特定环境的本地覆盖。

左侧的文件比右侧的文件具有更高的优先级：

- `npm start`: `.env.development.local`, `.env.development`, `.env.local`, `.env`
- `npm run build`: `.env.production.local`, `.env.production`, `.env.local`, `.env`
- `npm test`: `.env.test.local`, `.env.test`, `.env` (注意没有 `.env.local` )

**在 `.env` 中展开环境变量**（了解）

展开计算机上已有的变量，以便在 `.env` 文件中使用（使用 [dotenv-expand](https://github.com/motdotla/dotenv-expand) ）。

例如，要获取环境变量 `npm_package_version` ：

```
REACT_APP_VERSION=$npm_package_version
＃也有效：
# REACT_APP_VERSION=${npm_package_version}
```

或者展开当前 `.env` 文件的本地变量：

```
DOMAIN=www.example.com
REACT_APP_FOO=$DOMAIN/foo
REACT_APP_BAR=$DOMAIN/bar
```

##### 制作渐进式 Web 应用程序(PWA)

生产版本具有生成一流的 [Progressive Web App](https://developers.google.com/web/progressive-web-apps/) 所需的所有工具，但 **离线/缓存优先行为选择性加入的**。 默认情况下，构建过程将生成一个 service worker 文件，但不会注册，因此它不会控制你的生产 Web 应用程序。

为了选择加入离线优先行为，开发人员应在其 [`src/index.js`](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/src/index.js) 文件中查找以下内容：

```js
// 如果你希望应用程序能脱机工作并加载更快，
// 那么可以将下面的 unregister() 改为 register()。 请注意，这带来了一些陷阱。
// 了解有关 service workers 的更多信息：http://bit.ly/CRA-PWA

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: http://bit.ly/CRA-PWA
serviceWorker.unregister();
```

正如注释所述，将 `serviceWorker.unregister()` 更改为 `serviceWorker.register()` 将选择使用 service worker 。

**为什么选择性加入?**

离线优先的 Progressive Web Apps（渐进式 Web 应用程序）比传统网页更快，更可靠，并提供了很好的移动体验：

- 无论网络连接如何（例如2G或3G），所有静态站点资源都会被缓存，以便你的页面在后续访问中快速加载。更新将在后台下载。
- 无论网络状态如何，即使离线，你的应用也能正常运行。这意味着你的用户将能够在 1万英尺的高空和地铁上使用你的应用程序。
- 在移动设备上，你的应用可以直接添加到用户的主屏幕，应用图标和所有内容。这消除了对 app 商店的需求。

但是，它们[使调试部署更具挑战性](https://github.com/facebook/create-react-app/issues/2398) ，因此，从 Create React App 2 开始，service workers 被设置成了可以选择加入。

[`workbox-webpack-plugin`](https://developers.google.com/web/tools/workbox/modules/workbox-webpack-plugin) 已集成到生产配置中，它将负责生成 service worker 文件，该文件将自动预先缓存所有本地资源，并在部署更新时使其保持最新。 service worker 将使用 [缓存优先策略](https://developers.google.com/web/fundamentals/instant-and-offline/offline-cookbook/#cache-falling-back-to-network) 来处理对本地资源的所有请求，包括 HTML 的 [导航请求](https://developers.google.com/web/fundamentals/primers/service-workers/high-performance-loading#first_what_are_navigation_requests)，确保你的 Web 应用程序始终保持快速，即使在缓慢或不可靠的网络上也是如此。

**离线优先的注意事项**

如果你决定选择加入 service worker 注册，请考虑以下因素：

1. 初始缓存完成后，[service worker 生命周期](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle) 控制何时更新的内容最终显示给用户。为了 [防止延迟加载内容的竞争条件](https://github.com/facebook/create-react-app/issues/3613#issuecomment-353467430) ，默认行为是保守地使更新的 service worker 保持 "[waiting](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#waiting)" 状态。这意味着用户最终会看到旧内容，直到他们关闭（重新加载）现有的打开标签。有关此行为的更多详细信息，请参阅 [这篇博客文章](https://jeffy.info/2018/10/10/sw-in-c-r-a.html)。
2. 用户并不总是熟悉离线优先Web应用程序。[让用户知道](https://developers.google.com/web/fundamentals/instant-and-offline/offline-ux#inform_the_user_when_the_app_is_ready_for_offline_consumption) service worker 何时完成填充缓存（显示 "This web app works offline!（此 Web 应用程序脱机工作！）"消息），并让他们知道 service worker 何时获取可用的最新更新可能很有用。下次他们加载页面时（显示“New content is available once existing tabs are closed.(一旦现有选项卡关闭，新内容可用。)”消息）。显示这些消息目前留给开发人员，但作为一个起点，你可以使用 [`src/serviceWorker.js`](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/src/serviceWorker.js) 中包含的逻辑，该逻辑演示了要监听哪些 service worker 生命周期事件以检测每个方案，以及默认情况下，只需将适当的消息记录到 JavaScript 控制台。
3. service worker [需要 HTTPS](https://developers.google.com/web/fundamentals/getting-started/primers/service-workers#you_need_https)，但为了便于本地测试，该策略[不适用于 `localhost`](http://stackoverflow.com/questions/34160509/options-for-testing-service-workers-via-http/34161385#34161385) 。如果你的生产 Web 服务器不支持 HTTPS ，则服务工作者注册将失败，但你的 Web 应用程序的其余部分仍将保持正常运行。
4. service worker 仅在 [生产环境](http://www.html.cn/create-react-app/docs/deployment) 中启用，例如，`npm run build` 的输出。建议你不要在开发环境中启用离线优先 service worker 程序，因为它可能会导致使用以前缓存的资源时感到沮丧，并且不包括你在本地进行的最新更改。
5. 如果 *需要* 在本地测试离线优先 service worker ，请构建应用程序（使用 `npm run build` ）并从构建目录运行简单的 http 服务器。运行构建脚本后，`create-react-app` 将提供一种在本地测试生产构建的方法的说明，并且 [部署说明](http://www.html.cn/create-react-app/docs/deployment) 包含使用其他方法的说明。 _请务必始终使用隐身窗口，以避免浏览器缓存出现问题 _。
6. 默认情况下，生成的 service worker 文件不会拦截或缓存任何跨源资源，如 HTTP [API请求](http://www.html.cn/create-react-app/docs/integrating-with-an-api-backend)，图片或从其他域名加载的嵌入。

**渐进式 Web 应用程序元 Metadata**

默认配置包含的 Web 应用程序 manifest 位于 [`public/manifest.json`](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/public/manifest.json) ，你可以使用特定于 Web 应用程序的详细信息进行自定义。

当用户在 Android 上使用 Chrome 或 Firefox 将网络应用添加到其主屏幕时，[`manifest.json`](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/public/manifest.json) 中的元数据可以设置显示网络应用时需要使用的图标，名称和品牌颜色（branding colors）。[ Web App Manifest 指南](https://developers.google.com/web/fundamentals/engage-and-retain/web-app-manifest/) 提供了有关每个字段的含义以及你的自定义将如何影响用户体验的更多背景信息。

已添加到主屏幕的渐进式Web应用程序将加载更快，并在激活 service worker 时脱机工作。话虽如此，无论你是否选择性加入 service worker 注册，Web 应用程序清单中的元数据仍将被使用。

##### 创建生产构建

`npm run build` 会创建一个 `build` 目录，用于存放生产构建。`build/static` 目录中将是你的 JavaScript 和 CSS 文件。 `build/static` 中的每个文件名都包含文件内容的唯一哈希值。文件名中的此哈希启用 [长期缓存技术](http://www.html.cn/create-react-app/docs/production-build/#static-file-caching) 。

运行新创建的 Create React App 应用程序的生产版本时，会生成3个 `.js` 文件（称为 *chunks* ，理解为“块”），并将其放置在 `build/static/js` 目录中：

```
main.[hash].chunk.js
```

- 这是你的 *应用程序* 代码。 `App.js` 等。

```
1.[hash].chunk.js
```

- 这是 *vendor(第三方库)* 的代码，其中包含你在 `node_modules` 中导入的模块。拆分 *vendor(第三方库)* 和 *应用程序* 代码的潜在优势之一是启用[长期缓存技术](http://www.html.cn/create-react-app/docs/production-build/#static-file-caching) 以提高应用程序加载性能。由于 *vendor(第三方库)* 代码的更改频率低于实际应用程序代码，因此浏览器将能够单独缓存它们，并且每次应用程序代码更改时都不会重新下载它们。

```
runtime~main.[hash].js
```

- 这是 [webpack runtime(运行时)](https://webpack.js.org/configuration/optimization/#optimization-runtimechunk) 逻辑 chunk ，用于加载和运行你的应用程序。默认情况下，此内容将嵌入 `build/index.html` 文件中以保存其他网络请求。你可以通过指定我们的 [高级配置](http://www.html.cn/create-react-app/docs/advanced-configuration) 中记录的`INLINE_RUNTIME_CHUNK=false` 来选择不这样做，这将加载此 chunk 而不是将其嵌入 `index.html` 。

如果你使用 [code splitting(代码拆分)](http://www.html.cn/create-react-app/docs/code-splitting) 来拆分应用程序，这将在 `build/static` 文件夹中创建额外的 chunks 。

**静态文件缓存**

`build/static` 目录中的每个文件都会有一个基于文件内容生成的唯一哈希 附加到文件名，这允许你使用 [积极的缓存技术](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching#invalidating_and_updating_cached_responses) 来避免浏览器重新下载那些文件内容没有改变的资源。如果文件的内容在后续构建中发生更改，则生成的文件名哈希将会不同。

为了向用户提供最佳性能，最佳做法是为 `index.html` 以及 `build/static` 中的文件指定 `Cache-Control` 标头。此标头允许你控制浏览器和 CDN 将缓存静态资产的时间长度。如果你不熟悉 `Cache-Control` 的功能，请参阅 [这篇文章](https://jakearchibald.com/2016/caching-best-practices/) 以获得精彩的介绍。

使用 `Cache-Control: max-age=31536000` 用于 `build/static` 资源，而 `Cache-Control: no-cache` 用于其他所有内容是一个安全有效的起点，可确保用户的浏览器始终检查更新的 `index.html` 文件，并将缓存所有 `build/static` 文件一年。请注意，你可以安全地在 `build/static` 上使用一年到期，因为文件内容哈希嵌入到文件名中。

#### [测试](http://www.html.cn/create-react-app/docs/running-tests/)

#### 后端集成

##### 在开发环境中代理API请求

开发服务器在开发过程中向 API 服务器代理任何未知请求，请在 `package.json` 中添加 `proxy` 字段，例如：

```json
 "proxy": "http://localhost:4000",
```

这样，当你在开发中使用 `fetch('/api/todos')` 时，开发服务器将识别出它不是静态资源，并将你的请求代理到`http://localhost:4000/api/todos` 作为后备。开发服务器将 **仅仅** 尝试将 `Accept` 头中没有 `text/html` 的请求发送到代理。

请记住，`proxy` 只在开发环境中有效（使用 `npm start` ），并且你应该确保像 `/api/todos` 这样的 URL 在生产环境中指向正确的地址。你不必使用 `/api` 前缀。没有 `text/html` accept 标头的任何无法识别的请求都将被重定向到指定的 `proxy`（代理服务器）。

**配置`proxy`多个代理**

```sh
npm install http-proxy-middleware --save
# 或
yarn add http-proxy-middleware
```

在 `src`文件夹 新建 `setupProxy.js`：

```js
const proxy = require('http-proxy-middleware');

module.exports = function(app) {
  app.use(
    proxy('/api1', { 
      target: 'http://localhost:5000/', // API服务器的地址,将本地请求代理到该地址 
      ws:ture, // 代理websockets  
      changeOrign:true, // 是否跨域，虚拟的站点需要更换origin
      pathRewrite:{'/^api1':''} // '^/api1'是一个正则表达式，表示要匹配请求的url中，全部'http://localhost:8080/api1' 转接为 http://localhost:5000/
    }),
    proxy('/api2', { 
      target: 'http://localhost:5001/',
      ws:ture, 
      changeOrign:true,
      pathRewrite:{'/^api2':''}
    }),      
  );
};
```

> **注意：** 你无需在任何位置导入此文件。 它在启动开发服务器时会自动注册。

#### 部署

##### 部署

###### 静态服务器

对于使用 [Node](https://nodejs.org/) 环境，最简单的方法是安装 [serve](https://github.com/zeit/serve) 并让它处理剩下的事情：

```sh
npm install -g serve
serve -s build
```

上面显示的最后一个命令将为端口 **5000** 上的静态站点提供服务。与许多 [serve](https://github.com/zeit/serve) 的内部设置一样，可以使用 `-p` 或 `--port` 标志调整端口。

###### 其他方案

你不一定需要静态服务器才能在生产中运行 Create React App 项目。 它可以很好地集成到现有的动态服务器中。

这是使用 [Node](https://nodejs.org/) 和 [Express](http://expressjs.com/) 的程序化示例：

```javascript
const express = require('express');
const path = require('path');
// 先安装compression：npm install compression --save
const compression = require('compression')
const app = express();

// 一定要写到静态资源托管之前
app.use(compression())
app.use(express.static(path.join(__dirname, 'build')));

app.get('/', function(req, res) {
  res.sendFile(path.join(__dirname, 'build', 'index.html'));
});

app.listen(9000);
```

###### 使用客户端路由服务于应用程序

如果你使用在底层使用 HTML5 [`pushState` history API](https://developer.mozilla.org/en-US/docs/Web/API/History_API#Adding_and_modifying_history_entries) 的路由（例如，[React Router](https://github.com/ReactTraining/react-router) 使用 `browserHistory` ），许多静态文件服务器将失败。例如，如果你使用带有 `/todos/42` 路由的 React 路由器，开发服务器将正确响应 `localhost:3000/todos/42` ，但是服务于上述生产构建的 Express 不会正确响应。

这是因为当 `/todos/42` 有新的页面加载时，服务器会查找文件 `build/todos/42` 并且找不到它。需要配置服务器以通过提供`index.html` 来响应对 `/todos/42` 的请求。例如，我们可以修改上面的 Express 示例，为任何未知路径提供 `index.html` ：

知路径提供 `index.html` ：

```diff
 app.use(express.static(path.join(__dirname, 'build')));

-app.get('/', function (req, res) {
+app.get('/*', function (req, res) {
   res.sendFile(path.join(__dirname, 'build', 'index.html'));
 });
```



###### 建立相对路径

默认情况下，Create React App 会生成一个构建，**假设你的应用程序托管在服务器根目录下**。
要覆盖它，请在 `package.json` 中指定 `homepage` ，例如：

```js
  "homepage": "http://mywebsite.com/relativepath",
```

Create React App 使用 `homepage` 字段确定构建的 HTML 文件中的根 URL 。

**注意：** 如果你使用的是 `react-router@^4` ，则可以使用任何 `<Router>` 上的 `basename` 属性来根 `<Link>`。 更多信息在 [这里](https://reacttraining.com/react-router/web/api/BrowserRouter/basename-string)。

例如：

```js
<BrowserRouter basename="/calendar"/>
<Link to="/today"/> // renders <a href="/calendar/today">
```

> BrowserRouter的basenname属性：所有locations的基本URL。如果应用程序是从服务器上的子目录提供的，则需要将其设置为子目录。格式正确的基名称应该有一个前导斜杠，但不能有尾随斜杠。

###### 不同的路径服务相同的构建（了解）

如果你没有使用 HTML5 `pushState` history API 或根本不使用客户端路由，则无需指定应用程序的 URL 。相反，你可以把它放在你的 `package.json` 中：

```js
  "homepage": ".",
```

这将确保所有资产路径都与 `index.html` 相关。然后，你就可以将应用程序从 `http://mywebsite.com` 移动到`http://mywebsite.com/relativepath` 甚至 `http://mywebsite.com/relative/path` ，而无需重新构建。

###### 为任意构建环境定制环境变量（了解）

你可以通过创建自定义 `.env` 文件并使用 [env-cmd](https://www.npmjs.com/package/env-cmd) 加载它来创建任意构建环境。

例如，要为演示环境创建构建环境：

1. 创建一个名为 `.env.staging` 的文件

2. 在`.env.staging` 中像设置任何其他 `.env` 文件一样设置环境变量（例如 `REACT_APP_API_URL=http://api-staging.example.com` ）

3. 安装env-cmd

   ```sh
   $ npm install env-cmd --save
   $ # or
   $ yarn add env-cmd
   ```

4. 在package.json中添加一个新脚本，使用新环境构建：

   ```json
   {
     "scripts": {
       "build:staging": "env-cmd .env.staging npm run build"
     }
   }
   ```

现在，你可以运行 `npm run build:staging` 来使用演示环境配置进行构建。你可以以相同的方式指定其他环境。

`.env.production` 中的变量将用作后备，因为 `NODE_ENV` 将始终设置为 `production` 以进行构建。



###### Github Pages

**第1步：将 `homepage` 添加到 `package.json`**

开你的 `package.json` 并为你的项目添加一个 `homepage` 字段：

```json
  "homepage": "https://myusername.github.io/my-app",
```

> my-app是项目文件夹名称

或者对于 GitHub 用户页面（个人页面）：

```json
  "homepage": "https://myusername.github.io",
```

Create React App 使用 `homepage` 字段确定构建的 HTML 文件中的根 URL 。

**第2步：安装 `gh-pages` 并将 `deploy` 添加到 `package.json` 的 `scripts` 中**

现在，无论何时运行 `npm run build` ，你都会看到一个备忘单，其中包含有关如何部署到 GitHub 页面的说明。

要在 https://myusername.github.io/my-app 上发布它，请运行：

```sh
npm install --save gh-pages
```

或者你可以使用 `yarn`:

```sh
yarn add gh-pages
```

在 `package.json` 的 `"scripts"` 中添加以下内容：

```diff
  "scripts": {
+   "predeploy": "npm run build",
+   "deploy": "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

`predeploy` 脚本将在 `deploy` 运行之前自动运行。

`predeploy` 脚本将在 `deploy` 运行之前自动运行。

如果要部署到 GitHub 用户页面而不是项目页面，则需要进行两项额外修改：

1. 首先，将仓库的源分支更改为除 **master** 之外的任何分支。
2. 另外，调整 `package.json` 脚本以将部署推送到 **master** ：

```diff
  "scripts": {
    "predeploy": "npm run build",
-   "deploy": "gh-pages -d build",
+   "deploy": "gh-pages -b master -d build",
```

**第3步：通过运行 `npm run deploy` 来部署站点**

然后运行：

```sh
npm run deploy
```

**第4步：确保项目的设置使用 `gh-pages`**

最后，确保 GitHub 项目设置中的 **GitHub Pages** 选项设置为使用 `gh-pages` 分支。

**第5步：（可选）配置域名**

你可以通过向 `public/` 文件夹添加 `CNAME` 文件来使用 GitHub 页面配置自定义域名。

你的 CNAME 文件应如下所示：

```
mywebsite.com
```

**客户端路由的注意事项**

GitHub Pages 不支持底层使用 HTML5 `pushState` history API 的路由（例如，使用 `browserHistory` 的 React Router ）。这是因为当像 `http://user.github.io/todomvc/todos/42` 这样的网址有新的页面加载时，其中 `/todos/42` 是前端路由，GitHub Pages 服务器返回 404，因为它不知道 `/todos/42` 。如果要将路由添加到 GitHub 页面上托管的项目中，可以使用以下几种解决方案：

- 你可以从使用 HTML5 history API 切换到使用哈希值进行路由。如果你使用 React Router，你可以切换到 `hashHistory` 以获得此效果，但 URL 会更长且更冗长（例如，`http://user.github.io/todomvc/#/todos/42?_k=yknaj` ） 。阅读有关 [React Router 中不同历史实现的更多信息](https://reacttraining.com/react-router/web/api/Router) 。
- 或者，你可以在 GitHub Pages 使用一些小技巧，通过使用特殊的重定向参数重定向到你的 `index.html` 页面来处理 404 。在部署项目之前，你需要在构建文件夹中添加带有重定向代码的 `404.html` 文件，并且需要将处理重定向参数的代码添加到 `index.html` 。你可以在 [本指南](https://github.com/rafrex/spa-github-pages) 中找到有关此技术的详细说明。

**故障排除**

**"/dev/tty: No such a device or address"**

如果在部署时，你得到 `/dev/tty: No such a device or address` 或类似的错误，请尝试以下操作：

1. 创建一个新的 [Personal Access Token](https://github.com/settings/tokens)
2. `git remote set-url origin https://<user>:<token>@github.com/<user>/<repo>` .
3. 再次尝试 `npm run deploy`

**"Cannot read property 'email' of null"**

如果在部署时，你得到 `Cannot read property 'email' of null`，请尝试以下操作：

1. `git config --global user.name '<your_name>'`
2. `git config --global user.email '<your_email>'`
3. 再次尝试 `npm run deploy`