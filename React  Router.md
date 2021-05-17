### React  Router

[官方文档 - 最新](https://reactrouter.com/web/guides/quick-start)

[官方文档 - v5中文翻译](https://zhuanlan.zhihu.com/p/68838182)

[官方文档](http://react-guide.github.io/react-router-cn/docs/API.html)

```sh
npm install react-router-dom
```

#### 路径语法

路由路径是匹配一个（或一部分）URL 的 [一个字符串模式](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#routepattern)。大部分的路由路径都可以直接按照字面量理解，除了以下几个特殊的符号：

- `:paramName` – 匹配一段位于 `/`、`?` 或 `#` 之后的 URL。 命中的部分将被作为一个[参数](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#params)
- `()` – 在它内部的内容被认为是可选的
- `*` – 匹配任意字符（非贪婪的）直到命中下一个字符或者整个 URL 的末尾，并创建一个 `splat` [参数](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#params)

```jsx
<Route path="/hello/:name">         // 匹配 /hello/michael 和 /hello/ryan
<Route path="/hello(/:name)">       // 匹配 /hello, /hello/michael 和 /hello/ryan
<Route path="/files/*.*">           // 匹配 /files/hello.jpg 和 /files/path/to/hello.jpg
```

如果一个路由使用了相对路径，那么完整的路径将由它的所有祖先节点的路径和自身指定的相对路径拼接而成。[使用绝对路径](http://react-guide.github.io/react-router-cn/docs/guides/basics/RouteConfiguration.html#decoupling-the-ui-from-the-url)可以使路由匹配行为忽略嵌套关系。

#### history 模式

React Router 是建立在 [history](https://github.com/rackt/history) 之上的。 简而言之，一个 history 知道如何去监听浏览器地址栏的变化， 并解析这个 URL 转化为 `location` 对象， 然后 router 使用它匹配到路由，最后正确地渲染对应的组件。

常用的 history 有三种形式， 但是你也可以使用 React Router 实现自定义的 history。

- `browserHistory`
- `hashHistory`
- `createMemoryHistory`

你可以从 React Router 中引入它们：

```js
// JavaScript 模块导入（译者注：ES6 形式）
import { browserHistory } from 'react-router'
```

然后将它们传递给`<Router>`:

```js
render(
  <Router history={browserHistory} routes={routes} />,
  document.getElementById('app')
)
```

##### **browserHistory**

Browser history 是使用 React Router 的应用推荐的 history。它使用浏览器中的 [History](https://developer.mozilla.org/en-US/docs/Web/API/History) API （HTML5 history 模式）用于处理 URL，创建一个像`example.com/some/path`这样真实的 URL 。

**需要服务器配置（了解）**

这种模式下，服务器需要做好处理 URL 的准备。处理应用启动最初的 `/` 这样的请求应该没问题，但当用户来回跳转并在 `/accounts/123` 刷新时，服务器就会收到来自 `/accounts/123` 的请求，这时你需要处理这个 URL 并在响应中包含 JavaScript 应用代码。

一个 express 的应用可能看起来像这样的：

```js
const express = require('express')
const path = require('path')
const port = process.env.PORT || 8080
const app = express()

// 通常用于加载静态资源
app.use(express.static(__dirname + '/public'))

// 在你应用 JavaScript 文件中包含了一个 script 标签
// 的 index.html 中处理任何一个 route
app.get('*', function (request, response){
  response.sendFile(path.resolve(__dirname, 'public', 'index.html'))
})

app.listen(port)
console.log("server started on port " + port)
```

如果你的服务器是 nginx，请使用 [`try_files` 指令](http://nginx.org/en/docs/http/ngx_http_core_module.html#try_files)：

```
server {
  ...
  location / {
    try_files $uri /index.html
  }
}
```

当在服务器上找不到其他文件时，这可以让 nginx 服务器提供静态文件服务并指向`index.html` 文件。

对于Apache服务器也有类似的方式，创建一个`.htaccess`文件在你的文件根目录下：

```
RewriteBase /
RewriteRule ^index\.html$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.html [L]
```

**IE8, IE9 支持情况（了解）**

如果我们能使用浏览器自带的 `window.history` API，那么我们的特性就可以被浏览器所检测到。如果不能，那么任何调用跳转的应用就会导致 **全页面刷新**，它允许在构建应用和更新浏览器时会有一个更好的用户体验，但仍然支持的是旧版的。

你可能会想为什么我们不后退到 hash history，问题是这些 URL 是不确定的。如果一个访客在 hash history 和 browser history 上共享一个 URL，然后他们也共享同一个后退功能，最后我们会以产生笛卡尔积数量级的、无限多的 URL 而崩溃。

##### hashHistory

Hash history 使用 URL 中的 hash（`#`）部分去创建形如 `example.com/#/some/path` 的路由。

```js
window.location.hash = 'product' // 设置 url 的 hash，会在当前url后加上 '#product'

console.log(window.location.hash) // '#product'  

// 监听hash变化，点击浏览器的前进后退会触发
window.addEventListener('hashchange', function(){ 
    
})

// 或者在body上加上onhashchange
<body onhashchange="myFunction()">
```

**我应该使用 `createHashHistory`吗？**

Hash history 不需要服务器任何配置就可以运行，如果你刚刚入门，那就使用它吧。但是我们不推荐在实际线上环境中用到它，因为每一个 web 应用都应该渴望使用 `browserHistory`。

**像这样 `?_k=ckuvup` 没用的在 URL 中是什么？**

当一个 history 通过应用程序的 `push` 或 `replace` 跳转时，它可以在新的 location 中存储 “location state” 而不显示在 URL 中，这就像是在一个 HTML 中 post 的表单数据。

在 DOM API 中，这些 hash history 通过 `window.location.hash = newHash` 很简单地被用于跳转，且不用存储它们的location state。但我们想全部的 history 都能够使用location state，因此我们要为每一个 location 创建一个唯一的 key，并把它们的状态存储在 session storage 中。当访客点击“后退”和“前进”时，我们就会有一个机制去恢复这些 location state。

##### **createMemoryHistory**

Memory history 不会在地址栏被操作或读取。这就解释了我们是如何实现服务器渲染的。同时它也非常适合测试和其他的渲染环境（像 React Native ）。

和另外两种history的一点不同是你必须创建它，这种方式便于测试。

```js
const history = createMemoryHistory(location)
```

##### 示例

```jsx
import React from 'react'
import { render } from 'react-dom'
import { browserHistory, Router, Route, IndexRoute } from 'react-router'

import App from '../components/App'
import Home from '../components/Home'
import About from '../components/About'
import Features from '../components/Features'

render(
  <Router history={browserHistory}>
    <Route path='/' component={App}>
      <IndexRoute component={Home} />
      <Route path='about' component={About} />
      <Route path='features' component={Features} />
    </Route>
  </Router>,
  document.getElementById('app')
)
```

#### 动态路由（代码路由）

https://blog.csdn.net/u010977147/article/details/53489932?utm_medium=distribute.pc_relevant_download.none-task-blog-baidujs-1.nonecase&depth_1-utm_source=distribute.pc_relevant_download.none-task-blog-baidujs-1.nonecase

https://www.jianshu.com/p/03840824ec70

https://www.cnblogs.com/bydzhangxiaowei/p/12238776.html

https://github.com/wwlh200/react-router-demo

https://react.docschina.org/docs/code-splitting.html#import

https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html

create-react-app文档



##### 路由的动态加载模块

React Router 里的[路径匹配](http://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/guides/basics/RouteMatching.md)以及组件加载都是异步完成的，不仅允许你延迟加载组件，**并且可以延迟加载路由配置**。

- **React.lazy（推荐使用，首选）**

只能导入 ES6 export default 导出的模块，不支持服务端渲染。

`React.lazy` 和 Suspense 技术还不支持服务端渲染。如果你想要在使用服务端渲染的应用中使用，我们推荐 [Loadable Components](https://github.com/gregberge/loadable-components) 这个库。

```js
const Component = React.lazy(() => import('./Component'));
```

此代码将会在组件首次渲染时，自动导入包含 `Component` 组件的包。

`React.lazy` 接受一个函数，这个函数需要动态调用 `import()`。它必须返回一个 `Promise`，该 Promise 需要 resolve 一个 `default` export 的 React 组件。

然后应在 `Suspense` 组件中渲染 lazy 组件，如此使得我们可以使用在等待加载 lazy 组件时做优雅降级（如 loading 指示器等）。

```jsx
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```

`fallback` 属性接受任何在组件加载过程中你想展示的 React 元素。你可以将 `Suspense` 组件置于懒加载组件之上的任何位置。你甚至可以用一个 `Suspense` 组件包裹多个懒加载组件。

```jsx
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));
const AnotherComponent = React.lazy(() => import('./AnotherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
          <AnotherComponent />
        </section>
      </Suspense>
    </div>
  );
}
```

**使用示例：**

```jsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```

> 通常需要搭配错误边界使用，当加载失败使用降级后的UI

- **import() — 推荐使用**

https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html

只能导入 ES6 export default 导出的模块

**使用之前：**

```js
import { add } from './math';

console.log(add(16, 26));
```

**使用之后：**

```js
// math.js 导出 math对象
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

当 Webpack 解析到该语法时，会自动进行代码分割。如果你使用 Create React App，该功能已**开箱即用**，你可以[立刻使用](https://create-react-app.dev/docs/code-splitting/)该特性。[Next.js](https://nextjs.org/docs/advanced-features/dynamic-import) 也已支持该特性而无需进行配置。

```js
const Foo = () => import('./Foo')
```

 **把组件按组分块**

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk](https://webpack.js.org/guides/code-splitting-require/#chunkname)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

如果你自己配置 Webpack，你可能要阅读下 Webpack 关于[代码分割](https://webpack.docschina.org/guides/code-splitting/)的指南。你的 Webpack 配置应该[类似于此](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269)。

当使用 [Babel](https://babeljs.io/) 时，你要确保 Babel 能够解析动态 import 语法而不是将其进行转换。对于这一要求你需要 [@babel/plugin-syntax-dynamic-import](https://classic.yarnpkg.com/en/package/@babel/plugin-syntax-dynamic-import) 插件。

```
npm install --save-dev @babel/plugin-syntax-dynamic-import
```

package.json

```json
{
  "presets": ["@babel/preset-react"],
  "plugins": ["@babel/plugin-syntax-dynamic-import"]
}
```



- **loadable-components（不建议使用）**

```js
// 第一步
npm install @loadable/component
// 第二步
const Loading = () => <div>Loading...</div>
const Home = loadable(
  () => import('./Home'), 
  {LoadingComponent: Loading}
)
```

缺点：需要引入第三方包，不建议使用，已经弃用

- **react-loadable（不建议使用）**

Loadable( )

接收一个配置对象为参数,第一个属性名为`loader`，是一个方法，用于动态加载我们所需要的模块，第二个参数就是我们的`Loading`组件咯，在动态加载还未完成的过程中会有该组件占位。

```js
{
  loader: () => import('../containers/Home'),
  loading: MyLoadingComponent
}
```

这个方法的返回值是一个react component，我们`Route`组件和url香匹配时，加载的就是这个component，该component通过loader方法进行异步加载以及错误处理：`() => import('../containers/Home').catch(err=>err)`

基本用法：

```js
import Loadable from 'react-loadable';
import Loading from './my-loading-component';

// react-loadable便是利用了import()来进行动态加载
const LoadableComponent = Loadable({
  loader: () => import('./my-component'),
  loading: Loading,
});
 
export default class App extends React.Component {
  render() {
    return <LoadableComponent/>;
  }
}
```

封装（高阶组件）：

```js
// utils.js
import Loadable from "react-loadable";
export default function asyncComponent(comp) {
  return Loadable({
    loader: comp, // () => import('./my-component') 的形式
    loading: (props) => {
       return "加载中...";
    },
  });
}

// 其它js文件
import asyncComponent from "../utils/utils";
const Component = asyncComponent(() => import('./Component'));
```

缺点：需要引入第三方包，该方法不建议使用，StrictMode下回报如下警告:　　 

The old API will be supported in all 16.x releases, but applications using it should migrate to the new version.

- **require.ensure（不建议使用）**

该方法不建议使用

```js
require.ensure(
  dependencies: String[],
  callback: function(require),
  errorCallback: function(error),
  chunkName: String
)
```

给定 `dependencies` 参数，将其对应的文件拆分到一个单独的 bundle 中，此 bundle 会被异步加载。**当使用 CommonJS 模块语法时，这是动态加载依赖项的唯一方法。**这意味着，可以在模块执行时才允许代码，只有在满足特定条件时才会加载 `dependencies`。

- `dependencies`：字符串数组，声明 `callback` 回调函数中所需要的所有模块。
- `callback`：当依赖项加载完成后，webpack 将会执行此函数，`require` 函数作为参数传入此函数中。当程序运行需要依赖时，可以使用 `require()` 来加载依赖。函数体可以使用此参数，来进一步执行 `require()` 模块。
- `errorCallback`：当 webpack 加载依赖失败时会执行此函数。
- `chunkName`：由 `require.ensure` 创建的 chunk 的名称。通过将相同 `chunkName` 传递给不同的 `require.ensure` 调用，我们可以将其代码合并到一个单独的 chunk 中，从而只产生一个浏览器必须加载的 bundle。



Route 可以定义 [`getChildRoutes`](http://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/API.md#getchildrouteslocation-callback)，[`getIndexRoute`](http://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/API.md#getindexroutelocation-callback) 和 [`getComponents`](http://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/API.md#getcomponentslocation-callback) 这几个函数。它们都是异步执行，并且只有在需要时才被调用。我们将这种方式称之为 “逐渐匹配”。 React Router 会逐渐的匹配 URL 并只加载该 URL 对应页面所需的路径配置和组件。

如果配合 [webpack](http://webpack.github.io/) 这类的代码分拆工具使用的话，一个原本繁琐的构架就会变得更简洁明了。

```js
const CourseRoute = {
  path: 'course/:courseId',

  getChildRoutes(location, callback) {
    require.ensure([], function (require) {
      // 如果你是使用 es6 的写法，也就是你的组件都是通过 export default 导出的，那么需要加入.default。如果你是使用 CommonJS 的写法，也就是通过 module.exports 导出的，那就无须加 .default 了。
      // 例如 require('./routes/Announcements').default
      callback(null, [
        require('./routes/Announcements'), 
        require('./routes/Assignments'),
        require('./routes/Grades'),
      ])
    })
  },

  getIndexRoute(location, callback) {
    require.ensure([], function (require) {
      callback(null, require('./components/Index'))
    })
  },

  getComponents(location, callback) {
    require.ensure([], function (require) {
      callback(null, require('./components/Course'))
    })
  }
}
```



#### API

##### Router

所有路由器组件的通用底层接口。通常应用程序将使用一个高级路由器代替：

- <BrowserRouter>
- <HashRouter>
- <MemoryRouter>
- <NativeRouter>
- <StaticRouter>

使用底层<Router>的最常见用例是将自定义历史记录与状态管理库（例如Redux或Mobx）进行同步。 请注意，不需要将状态管理库与React Router一起使用，它仅用于深度集成。

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { Router } from "react-router";
import { createBrowserHistory } from "history";

const history = createBrowserHistory();

ReactDOM.render(
  <Router history={history}>
    <App />
  </Router>,
  node
);
```

###### children

要渲染的子元素

```jsx
<Router>
  <App />
</Router>
```

###### history

用于导航的 history 对象，由 `history` 包提供。

```jsx
import React from "react";
import ReactDOM from "react-dom";
import { createBrowserHistory } from "history";

const customHistory = createBrowserHistory();

ReactDOM.render(<Router history={customHistory} />, node);
```

###### onError(error)

当路由匹配到时，也有可能会抛出错误，此时你就可以捕获和处理这些错误。通常，它们会来自那些异步的特性，如 [`route.getComponents`](http://react-guide.github.io/react-router-cn/docs/API.html#getcomponentslocation-callback)，[`route.getIndexRoute`](http://react-guide.github.io/react-router-cn/docs/API.html#getindexroutelocation-callback)，和 [`route.getChildRoutes`](http://react-guide.github.io/react-router-cn/docs/API.html#getchildrouteslocation-callback)。

###### onUpdate()

当 URL 改变时，需要更新路由的 state 时会被调用。



##### BrowserRouter

它使用浏览器中的 [History](https://developer.mozilla.org/en-US/docs/Web/API/History) API （HTML5 history 模式）用于处理 URL，创建一个像`example.com/some/path`这样真实的 URL 。

```js
// 引入BrowserRouter这个组件的类型（接口）
import { BrowserRouterProps} from 'react-router-dom'
```

```jsx
import {BrowserRouter as Router} from "react-router-dom";
<BrowserRouter
  basename={optionalString}
  forceRefresh={optionalBool}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App />
</BrowserRouter>
```

###### basename：string

所有locations的基本URL。如果应用程序是从服务器上的子目录提供的，则需要将其设置为子目录。格式正确的基名称应该有一个前导斜杠，但不能有尾随斜杠。

```jsx
<BrowserRouter basename="/calendar">
    <Link to="/today"/> // renders <a href="/calendar/today">
    <Link to="/tomorrow"/> // renders <a href="/calendar/tomorrow">
    ...
</BrowserRouter>
```

###### getUserConfirmation：function

用于确认导航的函数。默认为使用window.confirm。

`Window.confirm()` 方法显示一个具有一个可选消息和两个按钮(确定和取消)的模态对话框 。

```js
let result = window.confirm(message);
```

- message 是要在对话框中显示的可选字符串。
- result 是一个布尔值，表示是选择确定还是取消 (true表示OK)。

```jsx
<BrowserRouter
  getUserConfirmation={(message, callback) => {
    // this is the default behavior
    const allowTransition = window.confirm(message);
    callback(allowTransition);
  }}
/>
```

###### forceRefresh：bool

如果为真，路由器将在页面导航上使用整页刷新。您可能希望使用它来模拟传统服务器呈现的应用程序在页面导航之间进行整页刷新的方式。

```jsx
<BrowserRouter forceRefresh={true} />
```

###### keyLength：number

location.key的长度，默认为6。

```jsx
<BrowserRouter keyLength={12} />
```

###### children：node

渲染的子元素。若 React 版本小于16：渲染多个子元素时必须用一个根元素包裹。

##### HashRouter

Hash history 使用 URL 中的 hash（`#`）部分去创建形如 `example.com/#/some/path` 的路由。

Hash history 不支持 `location.key` or `location.state`，但它的兼容性更好，不需要配置服务器，通常适用于老版本的浏览器。

###### basename：string

所有locations的基本URL。如果应用程序是从服务器上的子目录提供的，则需要将其设置为子目录。格式正确的基名称应该有一个前导斜杠，但不能有尾随斜杠。

###### getUserConfirmation：function

用于确认导航的函数。默认为使用window.confirm。

`Window.confirm()` 方法显示一个具有一个可选消息和两个按钮(确定和取消)的模态对话框 。

###### hashType：string

用于window.location.hash的编码类型，有效值为：

- `"slash"` - Creates hashes like `#/` and `#/sunshine/lollipops` ，默认值为 slash
- `"noslash"` - Creates hashes like `#` and `#sunshine/lollipops`

- `"hashbang"` - Creates [“ajax crawlable”](https://developers.google.com/webmasters/ajax-crawling/docs/learn-more) (被谷歌反对) hashes like `#!/` and `#!/sunshine/lollipops`

> slash—斜线

###### children：node

单个被渲染的子元素。



##### MemoryRouter

它将您的“URL”的历史记录保存在内存中（不读取、不写入地址栏）。在测试和非浏览器环境（如React Native）中非常有用。

```jsx
<MemoryRouter
  initialEntries={optionalArray}
  initialIndex={optionalNumber}
  getUserConfirmation={optionalFunc}
  keyLength={optionalNumber}
>
  <App />
</MemoryRouter>
```

###### initialEntries：array

元素是history stack中location的数组。这些元素可能是具有{pathname，search，hash，state}对象或者是字符串url。

```jsx
<MemoryRouter
  initialEntries={["/one", "/two", { pathname: "/three" }]}
  initialIndex={1}
>
  <App />
</MemoryRouter>
```

###### initialIndex：number

initialEntries数组中初始位置的索引。

###### keyLength：number

loaction.key 的长度，默认为6。

###### children：node

渲染的子元素。若 React 版本小于16：渲染多个子元素时必须用一个根元素包裹。



##### Link

在应用程序周围提供声明性的、可访问的导航。`<Link>` 以适当的 href 去渲染一个可访问的锚标签。

`<Link>` 可以知道哪个 route 的链接是激活状态的，并可以自动为该链接添加 `activeClassName` 或 `activeStyle`。

###### to：string

跳转链接的路径，字符串形式，如 `/users/123`或`/users?userId=123&age=18`。

###### to：object

跳转链接的对象，可以是具有以下任何属性的对象：

- `pathname`: A string representing the path to link to.
- `search`: A string representation of query parameters.
- `hash`: A hash to put in the URL, e.g. `#a-hash`.
- `state`: State to persist to the `location`.

```jsx
<Link
  to={{
    pathname: "/courses",
    search: "?sort=name",
    hash: "#the-hash",
    state: { /* some key/value */ }
  }}
/>
```

###### to：function

loaction 作为参数，应当返回以字符串形式或对象形式代表的 location。

```jsx
<Link to={location => ({ ...location, pathname: "/courses" })} />
<Link to={location => `${location.pathname}?sort=name`} />
```

###### replace：bool

如果为true，则单击链接将替换 history stack中的当前location，而不是添加新的location。

```jsx
<Link to="/courses" replace />
```

###### component：React.component

```jsx
<Link to="/" component={myComponent} />
```

**注意：React Router 目前还不能管理滚动条的位置，并且不会自动滚动到 hash 对应的元素上。如果需要管理滚动条位置，可以使用 [scroll-behavior](https://github.com/rackt/scroll-behavior) 这个库。**

###### activeClassName

当某个 route 是激活状态时，`<Link>` 可以接收传入的 className。当元素处于活动状态时提供（增加）该元素的类名，默认值是`active`。

###### activeStyle

当元素处于活动状态时提供（增加）给该元素的样式对象（采用小驼峰命名属性的 JavaScript 对象）。

###### onClick(e)

自定义点击事件的处理方法。如处理 `<a>` 标签一样 - 调用 `e.preventDefault()` 来防止过度的点击，同时 `e.stopPropagation()` 可以阻止冒泡的事件。

###### 示例

如 `<Route path="/users/:userId" />` 这样的 route：

```jsx
<Link to={`/users/${user.id}`} activeClassName="active">{user.name}</Link>
// 变成它们其中一个依赖在 History 上，当这个 route 是
// 激活状态的
<a href="/users/123" class="active">Michael</a> // browserHistory模式
<a href="#/users/123">Michael</a> // hashHistory模式

// 修改 activeClassName
<Link to={`/users/${user.id}`} activeClassName="current">{user.name}</Link>

// 当链接激活时，修改它的样式
<Link to="/users" style={{color: 'white'}} activeStyle={{color: 'red'}}>Users</Link>
```



##### NavLink

<Link>的一个特殊版本，当呈现元素与当前URL匹配时，它将向渲染的元素添加样式属性。

###### activeClassName：string

当元素处于活动状态时提供（增加）给该元素的类名，默认值是`active`。

###### activeStyle：object

当元素处于活动状态时提供（增加）给该元素的样式对象（采用小驼峰命名属性的 JavaScript 对象）。

###### excat：bool

如果为true，则仅当location完全匹配时才应用活动类/样式。

```jsx
<NavLink exact to="/profile">
  Profile
</NavLink>
```

###### strict：bool

如果为true，则在确定location是否与当前URL匹配时，将考虑 location 路径名上的尾部斜杠。

###### isActive：function

添加额外逻辑决定链接能够处于活动状态的函数，需要返回一个布尔值。

```jsx
<NavLink
  to="/events/123"
  isActive={(match, location) => {
    if (!match) {
      return false;
    }

    // 奇数返回true，偶数返回false
    const eventID = parseInt(match.params.eventID);
    return !isNaN(eventID) && eventID % 2 === 1;
  }}
>
  Event 123
</NavLink>
```

###### loaction：object

location 对象包含有关当前 URL 的信息。形式大概就像这样：

```js
{
  key: 'ac3df4', // 在使用 hashHistory 时，没有 key
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: 'something'
  } // 仅在 browser history 和 memory history中有效
}
```

你使用以下几种方式来获取 location 对象：

- 在 [Route component]() 中，以 `this.props.location` 的方式获取，
- 在 [Route render]() 属性中，以 `({ location }) => ()` 的方式获取，
- 在 [Route children]() 属性中，以 `({ location }) => ()` 的方式获取，
- 在 [withRouter]() 中，以 `this.props.location` 的方式获取。

###### aria-current：string（了解）

aria-current属性应用在处于激活状态的链接，有效值为：

- `"page"` - 用于指示一组分页链接中的链接，默认值
- `"step"` - 用于指示基于步骤的流程的步骤指示器内的链接
- `"location"` - 用于指示可视高亮显示为流程图当前组件的图像
- `"date"` - 用于指示日历中的当前日期
- `"time"` - 用于指示时间表中的当前时间
- `"true"` - 用于指示导航链接是否处于活动状态
- `"false"` - 用于防止辅助技术对当前链接作出反应（一个用例是防止单个页面上出现多个aria当前标记）

##### Prompt

用于在离开页面之前提示用户。例如表单已填写一半，用户点击跳转到其它页面时，应该向用户呈现一个<Prompt>。

###### message：string

当用户试图离开时提示用户的消息

<Prompt message="Are you sure you want to leave?" />

###### message：function

参数是将要跳转到的location和action（字符串：PUSH、REPLACE、POP），根据判断条件需要返回一个true或者提示信息。true则直接跳转，字符串则是用户离开时的提示信息。

```jsx
<Prompt
  message={(location, action) => {
    if (action === 'POP') {
      console.log("Backing up...")
    }

    return location.pathname.startsWith("/app")
      ? true
      : `Are you sure you want to go to ${location.pathname}?`
  }}
/>
```

###### when：bool

在when={true}或when={false}时以相应地阻止（true）或允许（false）导航。



##### Redirect

在应用中 `<Redirect>` 可以设置重定向到其他 route 而不改变旧的 URL。

新的location将覆盖history stack中的当前location。

```jsx
<Route exact path="/">
  {loggedIn ? <Redirect to="/dashboard" /> : <PublicHomePage />}
</Route>
```

###### to：string

重定向模板URL，from中所有URL参数必须在to中体现

```jsx
<Redirect to="/somewhere/else"/>
```

###### to：object

重定向目标对象

```jsx
<Redirect
  to={{
    pathname: "/login",
    search: "?utm=your+face",
    state: { }
  }}
/>
```

###### from：string

你想由哪个路径进行重定向，包括动态段。

确保from中所有URL参数都在to中有使用。

```jsx
<Switch>
  <Redirect from="/old-path" to="/new-path" />
  <Route path="/new-path">
    <Place />
  </Route>
</Switch>

// Redirect with matched parameters
<Switch>
  <Redirect from="/users/:id" to="/users/profile/:id" />
  <Route path="/users/profile/:id">
    <Profile />
  </Route>
</Switch>
```

没有 path 属性的< Route > 或者 没有 from 属性的 < Redirect > 将总是匹配到当前的地址(location)，然后进行渲染。

###### push：bool

设置为true时，重定向会将新 location 推送到 history stack 中，而不是替换当前 location

```jsx
<Redirect push to="/somewhere/else" />
```

###### excat：bool

精确匹配路径。

当在<Switch>内部渲染<Redirect>时，只能与from结合使用以精确匹配location。

|  path  | location.pathname |  exact  | matches? |
| :----: | :---------------: | :-----: | :------: |
| `/one` |    `/one/two`     | `true`  |    no    |
| `/one` |    `/one/two`     | `false` |   yes    |

###### strict：bool

如果为true，则在确定location是否与当前URL匹配时，将考虑 location 路径名上的尾部斜杠。

当在<Switch>内渲染<Redirect>时，只能与from结合使用以严格匹配location。

###### sensitve：bool

如果为true，则匹配路径是否区分大小写。

|  path  | location.pathname | sensitive | matches? |
| :----: | :---------------: | :-------: | :------: |
| `/one` |      `/one`       |  `true`   |   yes    |
| `/One` |      `/one`       |  `true`   |    no    |
| `/One` |      `/one`       |  `false`  |   yes    |

###### 其他

注意，在 route 层 `<Redirect>` 可以被放在任何地方，尽管[正常的优先](http://react-guide.github.io/react-router-cn/docs/docs/guides/basics/RouteMatching.md#precedence) 规则仍适用。

```js
<Route path="course/:courseId">
  <Route path="dashboard" />
  {/* /course/123/home -> /course/123/dashboard */}
  <Redirect from="home" to="dashboard" />
</Route>
```

**优先级**

路由算法会根据定义的顺序自顶向下匹配路由。因此，当你拥有两个兄弟路由节点配置时，你必须确认前一个路由不会匹配后一个路由中的路径。例如，千万**不要**这么做：

```jsx
<Route path="/comments" ... />
<Redirect from="/comments" to="/others" />
```

##### Route

它最基本的职责是在某个UI的路径与当前URL匹配时呈现该UI。具有占位作用。

如果同一个组件被用作组件树中同一点上多个<Route>的子组件，React会将其视为同一个组件实例，并且组件的状态将在路由更改之间保留。如果不需要这样做，则添加到每个路由组件的唯一`key`将导致React在路由更改时重新创建组件实例。

###### Route render methods

- <Route component>
- <Route render>
- <Route children>

###### Route props

以上三种渲染方法都是传递一个包含以下三个路由参数的对象

- match
- location
- history

###### component

当匹配到 URL 时，单个组件会被渲染。它可以被父 route 组件的 `this.props.children` 渲染。

```jsx
const routes = (
  <Route component={App}>
    <Route path="groups" component={Groups}/>
    <Route path="users" component={Users}/>
  </Route>
)

class App extends React.Component {
  render () {
    return (
      <div>
        {/* 这会是 <Users> 或 <Groups> */}
        {this.props.children}
      </div>
    )
  }
}
```

可以使用 `this.props.location`等获取路由参数。

当您使用组件（而不是下面的 render 或 children）时，路由器会使用React.createElement从给定的组件中创建一个新的React元素。 这意味着，如果您向组件prop提供内联函数，则将在每个渲染中创建一个新组件。 因为在每次渲染时，都会重新将一个新的函数赋值给组件，所以将导致现有组件的卸载和新组件的安装，而不仅仅是更新现有组件。 使用内联函数进行内联渲染时，请使用render或children。

###### components

Route 可以定义一个或多个已命名的组件，当路径匹配到 URL 时， 它们可以被 父 route 组件的 `this.props[name]` 渲染。

```jsx
// 想想路由外部的 context — 如果你可拔插
// `render` 的部分，你可能需要这么做：
// <App main={<Users />} sidebar={<UsersSidebar />} />

const routes = (
  <Route component={App}>
    <Route path="groups" components={{main: Groups, sidebar: GroupsSidebar}}/>
    <Route path="users" components={{main: Users, sidebar: UsersSidebar}}>
      <Route path="users/:userId" component={Profile}/>
    </Route>
  </Route>
)

class App extends React.Component {
  render () {
    const { main, sidebar } = this.props // 当路径匹配到 URL 时，可以被父route组件的 `this.props[name]` 访问
    return (
      <div>
        <div className="Main">
          {main}
        </div>
        <div className="Sidebar">
          {sidebar}
        </div>
      </div>
    )
  }
}

class Users extends React.Component {
  render () {
    return (
      <div>
        {/* 当路径是 "/users/123" 是 `children` 会是 <Profile> */}
        {/* UsersSidebar 也可以获取作为 this.props.children 的 <Profile> ，
            所以这有点奇怪，但你可以决定哪一个可以
            继续这种嵌套 */}
        {this.props.children}
      </div>
    )
  }
}
```

###### render：function

render可以方便地进行内联渲染和包装，而无需进行上述不必要的重新安装。

render函数可以传递路由参数routeProps：match、loaction、history。

```jsx
<Route path="/home" render={(routeProps) => <div>Home</div>} />
```

>  与location匹配才渲染。

<Route component>优先于<Route render>，因此不要在同一个<Route>中同时使用这两个组件。

###### children：function

有时无论路径是否与location匹配都需要渲染。在这些情况下，可以使用函数children属性。它的工作原理与render完全相同，只是无论是否匹配都会调用它。

children函数可以传递路由参数routeProps：match、loaction、history。

这对于动画也很有用：

```jsx
<Route
  children={({ match, ...rest }) => (
    {/* 动画将始终渲染 */}
    <Animate>
      {match && <Something {...rest}/>}
    </Animate>
  )}
/>
```

<Route children>优先于<Route component>和<Route render>，因此不要在同一个<Route>中同时使用。

###### path：string | string [ ]

任何有效的URL路径（string）或路径数组（string [ ]）

它会与父 route 的路径连接，除非它是从 `/` 开始的， 将它变成一个绝对路径。

**注意**：在[动态路由](http://react-guide.github.io/react-router-cn/docs/docs/guides/advanced/DynamicRouting.md)中，绝对路径可能不适用于 route 配置中。

如果它是 undefined，路由会去匹配子 route。

```jsx
<Route path="/users/:id">
  <User />
</Route>

<Route path={["/users/:id", "/profile/:id"]}>
  <User />
</Route>
```

没有 path 属性的< Route > 或者 没有 from 属性的 < Redirect > 将总是匹配到当前的地址(location)，然后进行渲染。

###### excat：bool

精确匹配路径。

|  path  | location.pathname |  exact  | matches? |
| :----: | :---------------: | :-----: | :------: |
| `/one` |    `/one/two`     | `true`  |    no    |
| `/one` |    `/one/two`     | `false` |   yes    |

###### strict：bool

如果为true，则在确定location是否与当前URL匹配时，将考虑 location 路径名上的尾部斜杠。

当location.pathname中有其他URL段时，strict无效。

|  path   | location.pathname | strict | matches? |
| :-----: | :---------------: | :----: | -------- |
| `/one/` |      `/one`       |  true  | no       |
| `/one/` |      `/one/`      |  true  | yes      |
| `/one/` |    `/one/two`     |  true  | yes      |

strict 可以用来强制 location.pathname 不能有尾部斜杠，但要做到这一点，strict 和 excat 都必须是真的。

###### location：object

location 对象包含有关当前 URL 的信息。形式大概就像这样：

```js
{
  key: 'ac3df4', // 在使用 hashHistory 时，没有 key
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: 'something'
  } // 仅在 browser history 和 memory history中有效
}
```

你使用以下几种方式来获取 location 对象：

- 在 [Route component]() 中，以 `this.props.location` 的方式获取，
- 在 [Route render]() 属性中，以 `({ location }) => ()` 的方式获取，
- 在 [Route children]() 属性中，以 `({ location }) => ()` 的方式获取，
- 在 [withRouter]() 中，以 `this.props.location` 的方式获取。

如果<Route>元素包裹在<Switch>中并与传递给<Switch>的location（或当前history location）匹配，则传递给<Route>的 location 将被<Switch>的 location 覆盖。

###### sensitive：bool

区分大小写。

|  path  | location.pathname | sensitive | matches? |
| :----: | :---------------: | :-------: | :------: |
| `/one` |      `/one`       |  `true`   |   yes    |
| `/One` |      `/one`       |  `true`   |    no    |
| `/One` |      `/one`       |  `false`  |   yes    |

###### getComponent(location, callback)

与`component`属性 相比，它是异步的，能够实现按需加载，对于 code-splitting（代码分割）很有用。

- ###### callback 

  cb(err, component)

  ```jsx
  <Route path="courses/:courseId" getComponent={(location, cb) => {
    // 做一些异步操作去查找组件
    cb(null, Course)
  }}/>
  ```

###### onEnter(nextState, replaceState, callback?)

当 route 即将进入时调用。它有三个参数：下一个路由的 state，重定向到另一个路径的方法replaceState，回调函数。`this` 会触发钩子去创建 route 实例。

`replaceState(state,replacePath)`有两个参数：第一个参数用于更新 state 的 state 对象，第二个参数是重定向的路径。

当 `callback` 作为函数的第三个参数传入时，这个钩子将是异步执行的，并且跳转会阻塞直到 `callback` 被调用。

###### onLeave()

当 route 即将退出时调用。

> 在路由跳转过程中，[`onLeave` hook](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#leavehook) 会在所有将离开的路由中触发，从最下层的子路由开始直到最外层父路由结束。然后[`onEnter` hook](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#enterhook)会从最外层的父路由开始直到最下层子路由结束。

##### StaticRouter

从不改变位置的<Router>。

这在服务器端渲染场景中非常有用，因为当用户没有实际单击时，所以location不会实际更改。它在简单的测试中也很有用：当您只需要插入一个位置并对输出进行断言时。

###### basename：string

所有location的基本URL。格式正确的基名称应该有一个前导斜杠，但不能有尾随斜杠。

###### location：string

服务器收到的URL，可能是节点服务器上的req.url。

###### location：object

location对象：{pathname, search, hash, state}

###### context：object

纯JavaScript对象。在渲染期间，组件可以向对象添加属性以存储有关渲染的信息。

```jsx
const context = {}
<StaticRouter context={context}>
  <App />
</StaticRouter>
```

当<Route>匹配时，它将context对象传递给组件作为staticContext属性。有关如何自己执行此操作的详细信息，请参阅 [Server Rendering guide](https://reactrouter.com/web/guides/server-rendering) 。

渲染后，这些属性可用于配置服务器的响应。

```js
if (context.status === "404") {
  // ...
}
```

###### children：node

渲染的子元素。若 React 版本小于16：渲染多个子元素时必须用一个根元素包裹。

##### Switch

渲染与location匹配的第一个子级<Route>或<Redirect>。

< Switch >的独特之处是独它仅仅渲染一个路由。嵌套路由也需要加<Switch>。如果没有被<Switch>包裹，每一个包含匹配地址(location)的< Route >都会被渲染。思考下面的代码：

```jsx
import { Route } from "react-router";

let routes = (
  <div>
    <Route path="/about">
      <About />
    </Route>
    <Route path="/:user">
      <User />
    </Route>
    <Route>
      <NoMatch />
    </Route>
  </div>
);
```

如果现在的URL是 /about ，那么 < About >, < User >, 还有 < NoMatch > 都会被渲染，因为它们都与路径(path)匹配。这种设计，允许我们以多种方式将多个 < Route > 组合到我们的应用程序中，例如侧栏(sidebars)，面包屑(breadcrumbs)，bootstrap tabs等等。 然而，偶尔我们只想选择一个< Route > 来渲染。如果我们现在处于 /about ，我们也不希望匹配 /:user （或者显示我们的 “404” 页面 ）。以下是使用 Switch 的方法来实现：

```jsx
import { Route, Switch } from "react-router";

<Switch>
  <Route exact path="/" component={Home}/>
  <Route path="/about" component={About}/>
  <Route path="/:user" component={User}/>
  <Route component={NoMatch}/>
</Switch>
```

现在，如果我们处于 /about, 将开始寻找匹配的 。<About>将被匹配， 将停止寻找匹配并渲染。 同样，如果我们处于 /michael ， <User>将被渲染。

这对于动画过渡也很有用，因为匹配的<Route>渲染位置与上一个相同：

```jsx
let routes = (
  <Fade>
    <Switch>
      {/* there will only ever be one child here */}
      {/* 这里只会有一个子节点 */}
      <Route />
      <Route />
    </Switch>
  </Fade>
);
```

###### location：object

用于覆盖匹配子元素的location的location对象而不是当前history location（通常是当前浏览器URL）。

###### children：node

< Switch > 的所有子节点应为 < Route > 或 < Redirect > 元素。只有匹配当前地址(location)的第一个子节点才会被渲染。

< Route > 元素使用它们的 path 属性匹配，< Redirect > 元素使用它们的 from 属性匹配。没有 path 属性的< Route > 或者 没有 from 属性的 < Redirect > 将总是匹配到当前的地址(location)，然后进行渲染。

如果<Switch>有一个location属性，它将覆盖匹配到的子元素上的location。

##### generatePath

generatePath函数可用于生成路由的URL。

内部使用regexp库的路径。

```js
import { generatePath } from "react-router";

generatePath("/user/:id/:entity(posts|comments)", {
  id: 1,
  entity: "posts"
});
// Will return /user/1/posts
```

将路径编译为正则表达式的结果会被缓存。

generatePath接受2个参数pattern和params。

###### pattern：string

作为路径属性的路径模板。

###### params：object

该对象具有pattern（路径模板）中要使用的相应参数。

如果提供的参数和路径不匹配，将抛出错误：

```js
generatePath("/user/:id/:entity(posts|comments)", { id: 1 });
// TypeError: Expected "entity" to be defined
```

##### history / 编程式路由导航

本文档中的术语`history`和`history object`指的是`history`包，它是React Router（包括React本身）仅有的两个主要依赖项之一。在不同的 Javascript 环境中，history 以多种形式实现了对于 session 历史的管理。

也使用以下术语：

- “browser history” - 特定于DOM的实现，在支持HTML5 history API的Web浏览器中很有用
- “hash history” - 传统web浏览器的特定于DOM的实现
- “memory history” - 缓存history的实现，在测试和非DOM环境（如React Native）中非常有用

>本文档中的 `history`是React Router的`history`包，不是 [window.history](https://developer.mozilla.org/zh-CN/docs/Web/API/History)

 history对象通常具有以下属性和方法：

###### length：number

history stack中条目的数量

###### action：string

表示history stack中发生的更改类型

###### location：obejct

location 对象包含有关当前 URL 的信息。形式大概就像这样：

```js
{
  key: 'ac3df4', // 在使用 hashHistory 时，没有 key
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: 'something'
  } // 仅在 browser history 和 memory history中有效
}
```

你使用以下几种方式来获取 location 对象：

- 在 [Route component]() 中，以 `this.props.history` 的方式获取，
- 在 [Route render]() 属性中，以 `({ history }) => ()` 的方式获取，
- 在 [Route children]() 属性中，以 `({ history }) => ()` 的方式获取，
- 在 [withRouter]() 中，以 `this.props.history` 的方式获取。

###### push（path, [state]）

将新的条目推送到history stack顶。

###### replace（path, [state]）

替换history stack顶部的条目。

###### go(n)

在 history 中使用 `n` 或 `-n` 进行前进或后退

###### goBack()

在 history 中后退，等于go(-1)

###### goForward()

在 history 中前进，等于go(1)

###### block(prompt)

离开时阻止页面跳转

###### history is mutable

history 对象是可变的（随着地址改变而改变），因此需要以`this.props.location` 和 `({ location }) => ()` 的方式获取location，而不是通过 history.location获取。这能确保在生命周期内正确使用location对象。

```jsx
class Comp extends React.Component {
  componentDidUpdate(prevProps) {
    // will be true
    const locationChanged =
      this.props.location !== prevProps.location;

    // INCORRECT, will *always* be false because history is mutable.
    const locationChanged =
      this.props.history.location !== prevProps.history.location;
  }
}

<Route component={Comp} />;
```

##### location

location 对象包含有关当前 URL 的信息。形式大概就像这样：

```js
{
  key: 'ac3df4', // 在使用 hashHistory 时，没有 key
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: 'something'
  } // 仅在 browser history 和 memory history中有效
}
```

你使用以下几种方式来获取 location 对象：

- 在 [Route component]() 中，以 `this.props.location` 的方式获取，
- 在 [Route render]() 属性中，以 `({ location }) => ()` 的方式获取，
- 在 [Route children]() 属性中，以 `({ location }) => ()` 的方式获取，
- 在 [withRouter]() 中，以 `this.props.location` 的方式获取。

location 对象不会发生改变，因此你可以在生命周期的钩子函数中使用 location 对象来查看当前页面的位置是否发生改变，这种技巧在获取远程数据以及使用动画时非常有用。

```js
componentWillReceiveProps(nextProps) {
  if (nextProps.location !== this.props.location) {
    // navigated!
  }
}
```

你可以在不同环境中使用 location ： 

- Web [Link to](https://reactrouter.com/web/api/Link/to)
- Native [Link to](https://reactrouter.com/native/api/Link/to)
- [Redirect to](https://reactrouter.com/web/api/Redirect/to)
- [history.push](https://reactrouter.com/web/api/history/push)
- [history.replace](https://reactrouter.com/web/api/history/push)

通常情况下，你只需要给一个字符串当做 location ，但是，当你需要添加一些 location 的state时，你可以使用 location 对象。并且当你需要多个 UI ，而这些 UI 取决于浏览历史时，例如弹出框（modal），使用location 对象会有很大帮助。

```jsx
// usually all you need
<Link to="/somewhere"/>

// but you can use a location instead
const location = {
  pathname: '/somewhere',
  state: { fromDashboard: true }
}

<Link to={location}/>
<Redirect to={location}/>
history.push(location)
history.replace(location)
```

最后，你可以把 location 传入以下组件：

- Switch
- Route

这样做可以让组件不使用路由状态（router state）中的真实 location，因为我们有时候需要组件去渲染一个其他的 location 而不是本身所处的真实 location，比如使用动画或是等待跳转时。

##### match

match 对象包含了 如何与URL匹配的信息。

match 对象包含以下属性：

- `params` - (object) 路径参数对象，通过解析URL中动态的部分获得的键值对。
- `isExact` - (boolean) 当为 true 时，整个URL都需要精确匹配（无尾随字符）。
- `path` - (string) 用来做匹配的路径格式。用于构建嵌套的<Route>。 
- `url` - (string) URL匹配的部分，与`location.pathname`相等。用于构建嵌套的<Link>。

你可以在以下地方获取 match 对象：

- [Route component](https://reactrouter.com/web/api/Route/component) ：`this.props.match`
- [Route render](https://reactrouter.com/web/api/Route/render-func) 属性： `({ match }) => ()`
- [Route children](https://reactrouter.com/web/api/Route/children-func) 属性：`({ match }) => ()`
- [withRouter](https://reactrouter.com/web/api/withRouter) ： `this.props.match`
- [matchPath](https://reactrouter.com/web/api/matchPath) ：the return value（函数返回值）
- [useRouteMatch](https://reactrouter.com/web/api/hooks/useroutematch) ：the return value（函数返回值）

如果<Route>没有path，则会与最接近的父级<Route>匹配。<withRouter>也是如此。

###### null matches

使用children属性的<Route>将调用其children函数，即使路由的路径与当前location不匹配，此时match为null。

“解析”URL的默认方法是将match.url字符串拼接到“相对”路径。

```js
`${match.url}/relative-path`
```

如果在match为null时尝试执行此操作，最终会出现TypeError。 这意味着在使用children属性尝试连接< Route >内的“relative”路径是不安全的。

在match为null的<Route>内部使用无path<Route>时，会出现类似但更微妙的情况。

```jsx
// location.pathname = '/matches'
<Route path="/does-not-match"
  children={({ match }) => (
    // match === null
    // 无path
    <Route
      render={({ match: pathlessMatch }) => (
        // pathlessMatch === ???
      )}
    />
  )}
/>
```

没有path属性的`< Route >`从其父级继承匹配对象。 如果他们的父级路由的match为null，那么他们的match也将为null。 这意味着
a）任何子路由/链接必须是绝对路径，因为没有要解析的父级。
b）父级路由match为null时，无path的子路由将需要使用children属性来渲染。

##### matchPath

这允许您使用与<Route>中相同的match，除了在正常生命周期之外，例如在服务器上渲染之前收集数据依赖关系。

```js
import { matchPath } from "react-router";

const match = matchPath("/users/123", {
  path: "/users/:id",
  exact: true,
  strict: false
});
```

###### pathname

第一参数是你想要匹配的 pathname, 如果你正在使用服务端的 nodos.js 下使用, 将会是 req.url。

###### props

第二个参数是要用于与match匹配的对象，它们与Route接受的属性相同。 其中path可以是字符串或字符串数组：

```js
{
  path, // like /users/:id; either a single string or an array of strings
  strict, // optional, defaults to false
  exact, // optional, defaults to false
  sensitive, // optional, defaults to false
}
```

当props的path属性与match对象的path属性匹配时，它将返回一个对象。

```js
matchPath("/users/2", {
  path: "/users/:id",
  exact: true,
  strict: true
});

//  {
//    isExact: true
//    params: {
//        id: "2"
//    }
//    path: "/users/:id"
//    url: "/users/2"
//  }
```

当props的path属性与match对象的path属性不匹配时，它将返回null。

##### withRouter

把不是通过路由切换过来的组件中，用 **withRouter** 包裹，将react-router 的 history、location、match 三个对象传入props对象上

默认情况下必须是经过路由匹配渲染的组件才存在this.props，才拥有路由参数，才能使用编程式导航的写法，执行this.props.history.push('/detail')跳转到对应路由的页面

然而不是所有组件都直接与路由相连（通过路由跳转到此组件）的，当这些组件需要路由参数时，使用withRouter就可以给此组件传入路由参数，此时就可以使用this.props

```jsx
import React from "react";
import PropTypes from "prop-types";
import { withRouter } from "react-router";

// A simple component that shows the pathname of the current location
class ShowTheLocation extends React.Component {
  static propTypes = {
    match: PropTypes.object.isRequired,
    location: PropTypes.object.isRequired,
    history: PropTypes.object.isRequired
  };

  render() {
    const { match, location, history } = this.props;

    return <div>You are now at {location.pathname}</div>;
  }
}

// Create a new component that is "connected" to the router.
const ShowTheLocationWithRouter = withRouter(ShowTheLocation);
```

使用场景：

比如app.js这个组件，一般是首页，不是通过路由跳转过来的，而是直接从浏览器中输入地址打开的，如果不使用withRouter此组件的this.props为空，没法执行props中的history、location、match等方法。

withRouter不追踪location的更改，而是在从< Router >组件传递出去的location改变后重新渲染，这意味着withRouter不会在路由改变时重新渲染，除非他的父组件重新渲染。

组件（ShowTheLocation）的所有非React的静态方法和属性都会被自动的复制到已连接的组件（withRouter(ShowTheLocation)）。

###### Component.WrappedComponent（了解）

封装的组件作为组件上的静态属性WrappedComponent的值，这个组件可以用于在隔离环境中测试。

```jsx
// MyComponent.js
export default withRouter(MyComponent)

// MyComponent.test.js
import MyComponent from './MyComponent'
render(<MyComponent.WrappedComponent location={{...}} ... />)
```

###### wrapperdComponentRef：function

作为函数形式的ref传递给包装组件，参数是封装组件（Container）。

```jsx
class Container extends React.Component {
  componentDidMount() {
    this.component.doSomething();
  }

  render() {
    return (
      <MyComponent wrappedComponentRef={c => (this.component = c)} />
    );
  }
}
```

##### IndexLink

`<IndexLink>`写在组件内。

如果你使用 `<Link to="/">Home</Link>` , 它会一直处于激活状态，因为所有的 URL 的开头都是 `/` 。

如果链接到根路由/，则不能使用Link组件，而要使用IndexLink组件。因为根路由的特殊性，`/`会匹配任何子路由。**使用IndexLink组件可以对路径进行精确匹配**。

```jsx
// 在上面的代码中，根路由只会在精确匹配时才具有activeClassName属性
<IndexLink to="/" activeClassName="active" onlyActiveOnIndex={true}>
	Home
</IndexLink>
```

如果不想使用IndexLink组件，也可以使用Link组件中的onlyActiveOnIndex属性达到同样的效果。

```jsx
<Link to="/" activeClassName="active" onlyActiveOnIndex={true}>
	Home
</Link>
```

实际上，IndexLink组件就是对Link组件的onlyActiveOnIndex属性封装后的高阶组件。

##### IndexRoute（默认child，已废弃）

当用户在父 route 的 URL 时， Index Routes 允许你**为父 route 提供一个默认的 "child"**， 并且为使`<IndexLink>` 能用提供了约定。

与 [Route](http://react-guide.github.io/react-router-cn/docs/API.html#route) 的 props 一样，除了 `path`。

想象一下当 URL 为 `/` 时，我们想渲染一个在 `App` 中的组件。不过在此时，`App` 的 `render` 中的 `this.props.children` 还是 `undefined`。这种情况我们可以使用 [`IndexRoute`](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/API.md#indexroute) 来设置一个默认页面。

```jsx
import React from 'react'
import { Router, Route, Link } from 'react-router'
import { IndexRoute } from 'react-router'

const Dashboard = React.createClass({
  render() {
    return <div>Welcome to the app!</div>
  }
})

const App = React.createClass({
  render() {
    return (
      <div>
        <h1>App</h1>
        <ul>
          <li><Link to="/about">About</Link></li>
          <li><Link to="/inbox">Inbox</Link></li>
        </ul>
        {this.props.children}
      </div>
    )
  }
})

React.render((
  <Router>
    <Route path="/" component={App}>
      {/* 当 url 为/时渲染 Dashboard */}
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="messages/:id" component={Message} />
      </Route>
    </Route>
  </Router>
), document.body)
```

现在看起来如下：

| URL  | 组件               |
| ---- | ------------------ |
| `/`  | `App -> Dashboard` |

##### IndexRedirect

Index Redirects 允许你**从一个父 route 的 URL 重定向到其他 route**。 它们被用于允许子 route 作为父 route 的默认 route， 同时保持着不同的 URL。与 [Redirect](http://react-guide.github.io/react-router-cn/docs/API.html#redirect) 的 props 一样，除了 `from`。

##### 相对路径

路径前没有 `/`

##### 绝对路径

如果我们可以将 `/inbox` 从 `/inbox/messages/:id` 中去除，并且还能够让 `Message` 嵌套在 `App -> Inbox` 中渲染，那会非常赞。绝对路径可以让我们做到这一点。

```jsx
React.render((
  <Router>
    <Route path="/" component={App}>
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        {/* 使用绝对路径 /messages/:id 替换 messages/:id */}
        <Route path="/messages/:id" component={Message} />
      </Route>
    </Route>
  </Router>
), document.body)
```

在多层嵌套路由中使用绝对路径的能力让我们对 URL 拥有绝对的掌控。我们无需在 URL 中添加更多的层级，从而可以使用更简洁的 URL。

我们现在的 URL 对应关系如下：

| URL             | 组件                      |
| --------------- | ------------------------- |
| `/`             | `App -> Dashboard`        |
| `/about`        | `App -> About`            |
| `/inbox`        | `App -> Inbox`            |
| `/messages/:id` | `App -> Inbox -> Message` |

**提醒**：绝对路径可能在[动态路由](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/guides/advanced/DynamicRouting.md)中无法使用。



##### RoutingContext

在 context 中给定路由的 state、设置 history 对象和当前的 location，`<RoutingContext>` 就会去渲染组件树。

#### 配置

##### 使用

目前我们是基于create-react-app脚手架搭建起来的项目简单配置的，直接上代码

- 1.搭建项目

```sh
npx create-react-app my-app --typescript
npm install --save react-router-dom @types/react-router-dom 
```

- 2.在react-app-env.d.ts里面声明react-router-dom包或者安装@types/react-router-dom解决找不到包的问题

```ts
declare module "react-router-dom";
```

- 3.在src下面建立pages文件夹，创建Layout.tsx、Page1.tsx、Page2.tsx、Page3.tsx

```tsx
// Layout.tsx
import * as React from "react";
import RouteView, { IRouteViewProps } from "../routes/RouteView";
import { History } from "history";

interface ILayoutProps extends IRouteViewProps {
  history: History;
}

const Layout = (props: ILayoutProps) => {
  const handleClick = React.useCallback((e) => {
    const { name } = e.target;
    props.history.push(name);
  }, [props.history]);

  return (
    <div>
      <div>
        <button name="/basic/page1" onClick={handleClick}>
          Page1
        </button>
        <button name="/basic/page2" onClick={handleClick}>
          Page2
        </button>
        <button name="/basic/page3" onClick={handleClick}>
          Page3
        </button>
      </div>
      <RouteView {...props} />
    </div>
  );
};

export default Layout;

// Page1.tsx
import * as React from "react";

const Page1 = () => {
    return (
        <div>我是Page1</div>
    )
};

export default Page1;

// Page2.tsx
import * as React from "react";

const Page2 = () => {
    return (
        <div>我是Page2</div>
    )
};

export default Page2;

// Page3.tsx
import * as React from "react";

const Page3 = () => {
    return (
        <div>我是Page3</div>
    )
};

export default Page3;
```

- 4.在src下面建立routes文件夹，创建router.config.ts和RouteView.tsx

```tsx
// router.config.ts
import Layout from "../pages/Layout";
import { lazy } from "react";
const routesConfig = [
  {
    path: "/basic",
    component: Layout,
    childrenRoutes: [
      {
        path: "/basic/page1",
        component: lazy(() => import("../pages/Page1")),
      },
      {
        path: "/basic/page2",
        component: lazy(() => import("../pages/Page2")),
      },
      {
        path: "/basic/page3",
        component: lazy(() => import("../pages/Page3")),
      },
      { path: "/basic", redirect: "/basic/page1" },
    ],
  },
  // {
  //   path: "/login",
  //   component: lazy(() => import("../pages/Login")),
  // },
  {
    path: "/",
    redirect: "/basic",
  },
];

export default routesConfig;


// RouteView.tsx
import React from 'react'
import { Redirect, Route, Switch } from 'react-router-dom'
import {connect} from 'react-redux'

export interface IRouteViewProps {
  path?: string
  redirect?: string
  component?: any
  childrenRoutes?: IRouteViewProps[]
}
// RouteView.tsx
import React from 'react'
import { Redirect, Route, Switch, RedirectProps } from 'react-router-dom'

export interface IRouteViewProps {
  path?: string
  redirect?: RedirectProps
  component?: any
  childrenRoutes?: IRouteViewProps[]
}

const RouteView = (props: IRouteViewProps) => {
  return (
    <Switch>
    	{redirect && <Redirect {...redirect} />}
      <Route
        path={props.path}
        render={routeProps => {
          return  (
            <props.component {...routeProps}>
              {childrenRoutes && childrenRoutes.length > 0 && (
                <Switch>
                  {childrenRoutes.map((route, index) => (
                    // 因为是RouteViewContainer，因此会自动传入isLogin
                    <RouteViewContainer {...route} key={index} />
                  ))}
                </Switch>
            	 )}
            </props.component>
          ) : null
      	}}
      ></Route>
    <Switch/>
  )
}
// 这里我们使用了redux给RouteView生成一个容器组件
// 如果不使用redux，也可以直接使用RouteView组件
const RouteViewContainer = connect()(withRouter(RouteViewContainer))
export default RouteViewContainer
```

- 5.修改App.tsx

```tsx
// App.tsx
import React, { Suspense } from "react";
import routesConfig from "./routes/router.config";
import RouteViewContainer from "./routes/RouteView";
import { BrowserRouter } from "react-router-dom";

const App = () => {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>loading...</div>}>
        <Switch>
          <Redirect exact from="/" to="/login" />
          {routesConfig.map((route,index)=>(
            <RouteViewContainer {...route} key={index} />
          ))}
        <Switch />
      </Suspense>
    </BrowserRouter>
  );
};

export default App;
```

一个具备路由嵌套，路由懒加载，可配置化的的React-Router就配置好了。

- 6.index.tsx

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
```

> 

##### 集中式路由配置（JS对象）

route 定义的一个普通的 JavaScript 对象。 `Router` 把 JSX 的 `<Route>` 转化到这个对象中。 所有的 props 都和 `<Route>` 的 props 一样，除了以下属性。

示例：

因为 [route](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#route) 一般被嵌套使用，所以使用 [JSX](https://facebook.github.io/jsx/) 这种天然具有简洁嵌套型语法的结构来描述它们的关系非常方便。然而，如果你不想使用 JSX，也可以直接使用原生 [route](http://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#route) 数组对象。

上面我们讨论的路由配置可以被写成下面这个样子：

```js
const routeConfig = [
  { path: '/', // path: stirng | string[]
    component: App,
    // indexRedirect:'/about', // 重定向路由。不过一般使用 redirect组件进行重定向
    childRoutes: [
      { path: 'about', component: About },
      { path: 'inbox',
        component: Inbox,
        childRoutes: [
          { path: '/messages/:id', component: Message },
          { path: 'messages/:id',
            onEnter: function (nextState, replaceState) {
              replaceState(null, '/messages/' + nextState.params.id)
            }
          }
        ]
      }
    ]
  }
]

React.render(<Router routes={routeConfig} />, document.body)
```

###### childRoutes

子 route 的一个数组，与在 JSX route 配置中的 `children` 一样。

###### getChildRoutes(location, callback)

与 `childRoutes` 一样，但是是异步的，并且可以接收 `location`。对于 code-splitting 和动态路由匹配很有用（给定一些 state 或 session 数据会返回不同的子 route）。

**callback**

cb(err, routesArray)

```jsx
let myRoute = {
  path: 'course/:courseId',
  childRoutes: [
    announcementsRoute,
    gradesRoute,
    assignmentsRoute
  ]
}

// 异步的子 route
let myRoute = {
  path: 'course/:courseId',
  getChildRoutes(location, cb) {
    // 做一些异步操作去查找子 route
    cb(null, [ announcementsRoute, gradesRoute, assignmentsRoute ])
  }
}

// 可以根据一些 state
// 跳转到依赖的子 route
<Link to="/picture/123" state={{ fromDashboard: true }}/>

let myRoute = {
  path: 'picture/:id',
  getChildRoutes(location, cb) {
    let { state } = location

    if (state && state.fromDashboard) {
      cb(null, [dashboardPictureRoute])
    } else {
      cb(null, [pictureRoute])
    }
  }
}
```

###### getIndexRoute(location, callback)

与 `indexRoute` 一样，但是是异步的，并且可以接收 `location`。与 `getChildRoutes` 一样，对于 code-splitting 和动态路由匹配很有用

**callback** 

cb(err, route)

```js
// 例如：
let myIndexRoute = {
  component: MyIndex
}

let myRoute = {
  path: 'courses',
  indexRoute: myIndexRoute
}

// 异步的 index route
let myRoute = {
  path: 'courses',
  getIndexRoute(location, cb) {
    // 做一些异步操作
    cb(null, myIndexRoute)
  }
}
```



#### Route Components（路由配对时渲染的组件）

当 route 匹配到 URL 时会渲染一个 route 的组件。路由会在渲染时将以下属性注入组件中：

##### history

Router 的  [history](https://github.com/rackt/history/blob/master/docs)。

[JS history API](https://github.com/ReactTraining/history/blob/master/docs/api-reference.md)

路由 history 对象的方法：

**pushState(state, pathname, query)**

跳转至一个新的 URL。

参数

- `state` - location 的 state。
- `pathname` - 没有 query 完整的 URL。
- `query` - 通过路由字符串化的一个对象。

**replaceState(state, pathname, query)**

在不影响 history 长度的情况下（如一个重定向），用新的 URL 替换当前这个。

**参数**

- `state` - location 的 state。
- `pathname` - 没有 query 完整的 URL。
- `query` - 通过路由字符串化的一个对象。

**go(n)**

在 history 中使用 `n` 或 `-n` 进行前进或后退

**goBack()**

在 history 中后退。

**goForward()**

在 history 中前进。

**createPath(pathname, query)**

使用路由配置，将 query 字符串化加到路径名中。

**createHref(pathname, query)**

使用路由配置，创建一个 URL。例如，它会在 `pathname` 的前面加上 `#/` 给 hash history。

**isActive(pathname, query, indexOnly)**

根据当前路径是否激活返回 `true` 或 `false`。通过 `pathname` 匹配到 route 分支下的每个 route 将会是 true（子 route 是激活的情况下，父 route 也是激活的），除非 `indexOnly` 已经指定了，在这种情况下，它只会匹配到具体的路径。

**参数**

- `pathname` - 没有 query 完整的 URL。
- `query` - 如果没有指定，那会是一个包含键值对的对象，并且在当前的 query 中是激活状态的 - 在当前的 query 中明确是 `undefined` 的值会丢失相应的键
- `indexOnly` - 一个 boolean（默认：`false`）。

##### location

location 对象包含有关当前 URL 的信息。形式大概就像这样：

```js
{
  key: 'ac3df4', // 在使用 hashHistory 时，没有 key
  pathname: '/somewhere'
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: 'something'
  }  // 仅在 browser history 和 memory history中有效
}
```

你使用以下几种方式来获取 location 对象：

- 在 [Route component]() 中，以 `this.props.location` 的方式获取，
- 在 [Route render]() 中，以 `({ location }) => ()` 的方式获取，
- 在 [Route children]() 中，以 `({ location }) => ()` 的方式获取，
- 在 [withRouter]() 中，以 `this.props.location` 的方式获取。

##### params

URL 的动态段。

##### route

渲染组件的 route。

##### routeParams

`this.props.params` 是直接在组件中指定 route 的一个子集。例如，如果 route 的路径是 `users/:userId` 而 URL 是 `/users/123/portfolios/345`，那么 `this.props.routeParams` 会是 `{userId: '123'}`，并且 `this.props.params` 会是 `{userId: '123', portfolioId: 345}`。

##### children

匹配到子 route 的元素将被渲染。如果 route 有已命名的组件，那么此属性会是 undefined，并且可用的组件会被直接替换到 `this.props` 上。

###### 示例

```jsx
render((
  <Router>
    <Route path="/" component={App}>
      <Route path="groups" component={Groups} />
      <Route path="users" component={Users} />
    </Route>
  </Router>
), node)

class App extends React.Component {
  render() {
    return (
      <div>
        {/* 这可能是 <Users> 或 <Groups> */}
        {this.props.children}
      </div>
    )
  }
}
```

##### 已命名的组件

当一个 route 有一个或多个已命名的组件时，其子元素的可用性是通过 `this.props` 命名的。因此 `this.props.children` 将会是 undefined。那么所有的 route 组件都可以参与嵌套。

###### 示例

```jsx
render((
  <Router>
    <Route path="/" component={App}>
      <Route path="groups" components={{main: Groups, sidebar: GroupsSidebar}} />
      <Route path="users" components={{main: Users, sidebar: UsersSidebar}}>
        <Route path="users/:userId" component={Profile} />
      </Route>
    </Route>
  </Router>
), node)

class App extends React.Component {
  render() {
    // 在父 route 中，被匹配的子 route 变成 props
    return (
      <div>
        <div className="Main">
          {/* 这可能是 <Groups> 或 <Users> */}
          {this.props.main}
        </div>
        <div className="Sidebar">
          {/* 这可能是 <GroupsSidebar> 或 <UsersSidebar> */}
          {this.props.sidebar}
        </div>
      </div>
    )
  }
}

class Users extends React.Component {
  render() {
    return (
      <div>
        {/* 如果在 "/users/123" 路径上这会是 <Profile> */}
        {/* UsersSidebar 也会获取到作为 this.props.children 的 <Profile> 。
            你可以把它放这渲染 */}
        {this.props.children}
      </div>
    )
  }
}
```



#### Mixins和生命周期

##### 生命周期 Mixin

在组件中添加一个钩子，当路由要从 route 组件的配置中跳转出来时被调用，并且有机会去取消这次跳转。主要用于表单的部分填写。

在常规的跳转中， `routerWillLeave` 会接收到一个单一的参数：我们正要跳转的 `location`。去取消此次跳转，返回 false。

提示用户确认，返回一个提示信息（字符串）。在 web 浏览器 beforeunload 事件发生时，`routerWillLeave` 不会接收到一个 location 的对象（假设你正在使用 `useBeforeUnload` history 的增强方法）。在此之上，我们是不可能知道要跳转的 location，因此 `routerWillLeave` 必须在用户关闭标签之前返回一个提示信息。

> beforeunload事件在当页面卸载（关闭）或刷新时调用，事件触发的时候弹出一个有确定和取消的对话框，确定则离开页面，取消则继续待在本页

##### 生命周期方法

###### routerWillLeave(nextLocation)

当路由尝试从一个 route 跳转到另一个并且渲染这个组件时被调用。

`nextLocation` ： 下一个 location

#### History Mixin

在组件中添加路由的 history 对象。

**注意**：你的 route components 不需要这个 mixin，它在 `this.props.history` 中已经是可用的了。这是为了组件更深次的渲染树中需要访问路由的 `history` 对象。

##### pushState(state, pathname, query)

跳转至一个新的 URL。

**参数**

- `state` - location 的 state。
- `pathname` - 没有 query 完整的 URL。
- `query` - 通过路由字符串化的一个对象。

##### replaceState(state, pathname, query)

在不影响 history 长度的情况下（如一个重定向），用新的 URL 替换当前这个。

**参数**

- `state` - location 的 state。
- `pathname` - 没有 query 完整的 URL。
- `query` - 通过路由字符串化的一个对象。

##### **go(n)**

在 history 中使用 `n` 或 `-n` 进行前进或后退

##### **goBack()**

在 history 中后退。

##### **goForward()**

在 history 中前进。

##### **createPath(pathname, query)**

使用路由配置，将 query 字符串化加到路径名中。

##### **createHref(pathname, query)**

使用路由配置，创建一个 URL。例如，它会在 `pathname` 的前面加上 `#/` 给 hash history。

##### **isActive(pathname, query, indexOnly)**

根据当前路径是否激活，返回 `true` 或 `false`。通过 `pathname` 匹配到 route 分支下的每个 route 都会是 true（子 route 是激活的情况下，父 route 也是激活的），除非 `indexOnly` 已经指定了，在这种情况下，它只会匹配到具体的路径。

**参数**

- `pathname` - 没有 query 完整的 URL。
- `query` - 如果没有指定，那会是一个包含键值对的对象，并且在当前的 query 中是激活状态的 - 在当前的 query 中明确是 `undefined` 的值会丢失相应的键
- `indexOnly` - 一个 boolean（默认：`false`）。

##### **示例**

```jsx
import { History } from 'react-router'

React.createClass({
  mixins: [ History ],
  render() {
    return (
      <div>
        <div onClick={() => this.history.pushState(null, '/foo')}>Go to foo</div>
        <div onClick={() => this.history.replaceState(null, 'bar')}>Go to bar without creating a new history entry</div>
        <div onClick={() => this.history.goBack()}>Go back</div>
     </div>
   )
 }
})
```

假设你正在使用 bootstrap，并且在 Tab 中想让那些 `li` 获得 `active`：

```jsx
import { Link, History } from 'react-router'

const Tab = React.createClass({
  mixins: [ History ],
  render() {
    let isActive = this.history.isActive(this.props.to, this.props.query)
    let className = isActive ? 'active' : ''
    return <li className={className}><Link {...this.props}/></li>
  }
})

<Tab href="foo">Foo</Tab>
```

在应用中少数组件由于 `History` mixin 的缘故而用得不爽，此时有以下几个选项：

1、让 `this.props.history` 通过 route 组件到达需要它的组件中。

2、使用 context

```js
import { PropTypes } from 'react-router'

class MyComponent extends React.Component {
  doStuff() {
    this.context.history.pushState(null, '/some/path')
  }
}

MyComponent.contextTypes = { history: PropTypes.history }
```

3、确保你的 history 是一个 module

4、创建一个高阶的组件，我们可能用它来结束跳转和阻止 history，只是没有时间去思考所有的方法。

```jsx
// 创建一个高阶的组件
function connectHistory(Component) {
  return React.createClass({
    mixins: [ History ],
    render() {
      return <Component {...this.props} history={this.history} />
    }
  })
}

// 其他文件
// 确保你的 history 是一个 module
import connectHistory from './connectHistory'

class MyComponent extends React.Component {
  doStuff() {
    this.props.history.pushState(null, '/some/where')
  }
}

export default connectHistory(MyComponent)
```

#### RouteContext Mixin

RouteContext mixin 提供了一个将 route 组件设置到 context 中的便捷方法。这对于 route 渲染元素并且希望用 [生命周期 mixin](http://react-guide.github.io/react-router-cn/docs/API.html#lifecycle) 来阻止跳转是很有必要的。

简单地将 `this.context.route` 添加到组件中。

#### Utilities（公共方法）

##### useRoutes(createHistory)

返回一个新的 `createHistory` 函数，它可以用来创建读取 route 的 history 对象。

- listen((error, nextState) => {})
- listenBeforeLeavingRoute(route, (nextLocation) => {})
- match(location, (error, redirectLocation, nextState) => {})
- isActive(pathname, query, indexOnly=false)

##### match(location, cb)

这个函数被用于服务端渲染。它在渲染之前会匹配一组 route 到一个 location，并且在完成时调用 `callback(error, redirectLocation, renderProps)`。

传给回调函数去 `match` 的三个参数如下：

- `error`：如果报错时会出现一个 Javascript 的 `Error` 对象，否则是 `undefined`。
- `redirectLocation`：如果 route 重定向时会有一个 [Location](http://react-guide.github.io/react-router-cn/docs/docs/Glossary.md#location) 对象，否则是 `undefined`。
- `renderProps`：当匹配到 route 时 props 应该通过路由的 context，否则是 `undefined`。

如果这三个参数都是 `undefined`，这就意味着在给定的 location 中没有 route 被匹配到。

**注意：你可能不想在浏览器中用它，除非你做的是异步 route 的服务端渲染。**

##### createRoutes(routes)

创建并返回一个从给定对象 route 的数组，它可能是 JSX 的 route，一个普通对象的 route，或是其他的数组。

##### params

`routes`

一个或多个的 [`Route`](http://react-guide.github.io/react-router-cn/docs/API.html#route) 或 [`PlainRoute`](http://react-guide.github.io/react-router-cn/docs/API.html#plainRoute)。

#### 滚动到顶部

react 版本是16.8及以上：

```jsx
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

export default function ScrollToTop() {
  const { pathname } = useLocation();

  useEffect(() => {
    window.scrollTo(0, 0);
  }, [pathname]);

  return null;
}
```

16.8以下：

```jsx
import React from "react";
import { withRouter } from "react-router-dom";

class ScrollToTop extends React.Component {
  componentDidUpdate(prevProps) {
    if (
      this.props.location.pathname !== prevProps.location.pathname
    ) {
      window.scrollTo(0, 0);
    }
  }

  render() {
    return null;
  }
}
// 确保使用withRouter封装该组件以使其能够访问路由器的props
export default withRouter(ScrollToTop);
```

然后将其渲染在应用的顶部（整个应用跳转都是滚动到顶部）

```jsx
function App() {
  return (
    <Router>
      <ScrollToTop>
        <App />
      </ScrollToTop>
    </Router>
  );
}

// or just render it bare anywhere you want, but just one :)
<ScrollToTop />;
```

对于标签按钮跳转（部分页面跳转滚动到顶部）

```jsx
// 16.8版本及以上
function ScrollToTopOnMount() {
  useEffect(() => {
    window.scrollTo(0, 0);
  }, []);

  return null;
}

// Render this somewhere using:
// <Route path="..." children={<LongContent />} />
function LongContent() {
  return (
    <div>
      <ScrollToTopOnMount />

      <h1>Here is my long content page</h1>
      <p>...</p>
    </div>
  );
}

// 16.8版本以下

class ScrollToTopOnMount extends React.Component {
  componentDidMount() {
    window.scrollTo(0, 0);
  }

  render() {
    return null;
  }
}

// Render this somewhere using:
// <Route path="..." children={<LongContent />} />
class LongContent extends React.Component {
  render() {
    return (
      <div>
        <ScrollToTopOnMount />

        <h1>Here is my long content page</h1>
        <p>...</p>
      </div>
    );
  }
}
```

#### Hooks

##### useHistory

允许您访问用于导航的历史实例

```jsx
import { useHistory } from "react-router-dom";

function HomeButton() {
  let history = useHistory();

  function handleClick() {
    history.push("/home");
  }

  return (
    <button type="button" onClick={handleClick}>
      Go home
    </button>
  );
}
```

##### useLoaction

返回表示当前URL的location对象。你可以把它想象成一个useState，每当URL改变时，它就会返回一个新的位置。

##### useParams

useParams返回URL参数的键/值对的对象。

> 上面三个一般都是通过props.history，props.location，props.match.params或props.location.search来获取

##### useRouteMatch

useRouteMatch 尝试以与 <Route> 相同的方式匹配当前URL。它主要用于访问匹配数据，而无需实际渲染<Route>。

1、不接受任何参数并返回与当前<Route>匹配的match对象

2、接受一个参数，与matchPath的props参数相同。 它可以是字符串的路径名（绝对或相对路径），也可以是带有Route接受的匹配参数的对象。

#### 向路由传递参数

##### params参数

路由链接（携带参数）：

```jsx
< Link to='/demo/test/tom/18'}>详情</Link>
```

注册路由（声明接收）：

```jsx
< Route path="/demo/test/:name/:age" component=(Test} />
```

接收参数：

```js
const {name,age}=this.props.match.params
```

**需要服务器配合**

例如，如果你使用带有 `/todos/42` 路由的 React 路由器，开发服务器将正确响应 `localhost:3000/todos/42` ，但是服务于上述生产构建的 Express 不会正确响应。

这是因为当 `/todos/42` 有新的页面加载时，服务器会查找文件 `build/todos/42` 并且找不到它。需要配置服务器以通过提供`index.html` 来响应对 `/todos/42` 的请求。例如，我们可以修改上面的 Express 示例，为任何未知路径提供 `index.html` ：

```diff
 app.use(express.static(path.join(__dirname, 'build')));

-app.get('/', function (req, res) {
+app.get('/*', function (req, res) {
   res.sendFile(path.join(__dirname, 'build', 'index.html'));
 });
```

##### Search参数

将{ name: 'Tom' , age : 18 } 转换为 'name=tom&age=18'

可以使用`qs.stringfy`转换：

```js
import qs from 'querystring'
qs.stringfy({ name: 'Tom' , age : 18 })  // 'name=tom&age=18'
```

解析：

```js
qs.parse('name=tom&age=18')  // { name: 'Tom' , age : 18 }
```

路由链接（携带参数）：

```jsx
< Link to='/demo/test?name=tom&age=18'}>详情</Link>
```

注册路由（无需声明,正常注册即可）：

```jsx
< Route path="/demo/test" component={Test} />
```

接收参数：

```js
const {search}=this.props.location
```

备注：获取到的 search是  `'name=tom&age=18'` 这样的字符串，需要借助 querystring 模块的qs.parse解析

##### State参数

State参数在url中不可见

路由链接（携带参数）：

```jsx
< Link to={{pathname:'/demo/test', search: "?sort=name", state:{name:'tom',age:18}}>详情</Link>
```

注册路由（无需声明,正常注册即可）：

```jsx
< Route path="/demo/test" component={Test} />
```

接收参数：

```js
const {state}=this.props.location
```

备注：刷新会保留参数



#### 404

```jsx
<Route path="*">
	<NoMatch />
</Route>
```

#### 解决路径刷新页面样式丢失问题
1. pub1ic/ index.html 中引入样式时不使用相对路径，使用绝对路径，即将 `./`改为 `/`
2.  pub1ic/ index.html 中引入样式时不使用相对路径，使用绝对路径，即将 `./`改为 `%PUBLIC_URL%`，只适用于react脚手架， `%PUBLIC_URL%`是 public 文件夹的路径
3. 使用 HashRouter

#### TS 手动引入路由组件属性类型

```js
import { RouteComponentProps } from 'react-router-dom'
```

当我们使用`Route`组件或者使用`withRouter`的时候,都会给组件绑定`history,location,match`三个属性：

```ts
import React from 'react'
import { withRouter,RouteComponentProps } from 'react-router-dom'

interface Iprops extends RouteComponentProps{}

const App:React.FC<Iprops> = props => {
    console.log(props.pathname) // 有提示,并且props有他的类型规定
    return (<div></div>)
}

export default withRouter(App)
```

