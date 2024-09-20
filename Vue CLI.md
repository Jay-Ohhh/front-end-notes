### Vue CLI

#### vue.config.js

##### configureWebpack

如果你需要基于环境有条件地配置行为，或者想要直接修改配置，那就换成一个函数 (该函数会在环境变量被设置之后懒执行)。该方法的第一个参数会收到已经解析好的配置。在函数内，你可以直接修改配置，或者返回一个将会被合并的对象：

```js
// vue.config.js
module.exports = {
  configureWebpack: config => {
    if (process.env.NODE_ENV === 'production') {
      // 为生产环境修改配置...
      config.externals = {
        jquery: 'jQuery',
      }
    } else {
      // 为开发环境修改配置...
      config.devtool = "eval-source-map"
    }
  }
  chainWebpack: config => {
    // tap 动态修改插件的传参
    // config.plugin('html')为什么指的是 html-webpack-plugin呢？ vue-cli 源码设置的
		config.plugin('html').tap(args => {
        args[0].isProd = true
        return args
      })
	}
}
```

> htmlWebpackPlugin.options：传递给插件的options。 除了此插件实际使用的选项之外，您还可以使用此选项将任意数据传递到模板。
>
> ```html
>  <title><%= htmlWebpackPlugin.options.title %></title>
> ```
>
> ```javascript
> module.exports = {
>   // ...
>   plugins: [new HtmlWebpackPlugin({
>     title: 'Custom template using Handlebars',
>   })],
> };
> // { title: 'Custom template using Handlebars' } 即是 htmlWebpackPlugin.options
> ```

`configureWebpack` 与 `chainWebpack` 本质上没有什么区别，只是前者配置 **简单方便**，后者可以 **更为细粒度地控制配置**。

##### chainWebpack

- Type: `Function`

  是一个函数，会接收一个基于 [webpack-chain](https://github.com/mozilla-neutrino/webpack-chain) 的 `ChainableConfig` 实例。允许对内部的 webpack 配置进行更细粒度的修改。

  更多细节可查阅：[配合 webpack > 链式操作](https://cli.vuejs.org/zh/guide/webpack.html#链式操作-高级)

使用方式

https://juejin.cn/post/6950076780669566983

常用配置

https://juejin.cn/post/6844904138954801166

##### productionSourceMap

- Type: `boolean`

- Default: `true`

  如果你不需要生产环境的 source map，可以将其设置为 `false` 以加速生产环境构建。

##### css.sourceMap

- Type: `boolean`

- Default: `false`

  是否为 CSS 开启 source map。

##### devServer.proxy

- Type: `string | Object`

  如果你的前端应用和后端 API 服务器没有运行在同一个主机上，你需要在开发环境下将 API 请求代理到 API 服务器。这个问题可以通过 `vue.config.js` 中的 `devServer.proxy` 选项来配置。

  `devServer.proxy` 可以是一个指向开发环境 API 服务器的字符串：

  ```js
  module.exports = {
    devServer: {
      proxy: 'http://localhost:4000'
    }
  }
  ```

  这会告诉开发服务器将任何未知请求 (没有匹配到静态文件的请求) 代理到`http://localhost:4000`。

  如果你想要更多的代理控制行为，也可以使用一个 `path: options` 成对的对象。完整的选项可以查阅 [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware#proxycontext-config) 。

  ```js
  module.exports = {
    devServer: {
      proxy: {
        '/api': {
          target: '<url>',
          ws: true,
          changeOrigin: true
        },
        '/foo': {
          target: '<other_url>'
        }
      }
    }
  }
  ```

