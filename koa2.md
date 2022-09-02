### koa2

#### express、koa、koa2区别

三者都是web框架

| 框架名  | 异步处理          |
| ------- | ----------------- |
| exprees | 回调函数          |
| koa     | generator / yield |
| koa2    | async / await     |

 **在koa / koa2中，一切的流程都是中间件，数据流向遵循洋葱模型，先入后出，是按照类似栈的方式组织和执行的** 

洋葱内的每一层都表示一个独立的中间件，用于实现不同的功能，比如异常处理、缓存处理等。每次请求和响应都会从左侧开始一层层地经过每层的中间件，当进入到最里层的中间件之后，就会从最里层的中间件开始逐层返回。因此对于每层的中间件来说，在一个请求和响应周期中，都有两个时机点来添加不同的处理逻辑。

#### 快速上手

- 安装Koa

  ```js
  npm init -y // 在项目中快速生成package.json
  npm install koa
  ```

- 创建并编写 app. s文件
  1.创建Koa对象
  2.编写响应函数(中间件)
  3.监听端口

- 启动服务器

  ```js
  node app.js     
  // or
  nodemon app.js // 利用第三方工具 nodemon 修改保存后自动重启
  ```

#### 中间件特点

- Koa对象通过use方法加入一个中间件
- 一个中间件就是一个函数
- 中间件的执行顺序符合洋葱模型，先入后出
- 内层中间件能否执行取決于外层中间件的next函数是否调用

- next函数返回promise对象

```js
app.use(async(ctx, next)=>{
  //刚进入中间件想做的事情
	await next()
	//内层所有中间件结東之后想做的事情
}
```

基本用法

```js
/**
 * 1 创建koa对象
 * 2 编写响应函数(中间件)
 * 3 绑定端口号 3000
 */
const koa = require('koa')
const app = new koa()

// ctx:上下文，web容器，ctx.request，ctx.response
/*
next:调用下一个中间件，下一层中间件能否得到执行，取决于next这个函数有没有被调用
next()返回Promise对象，该Promise对象的resolve参数就是next()所调用的中间件return的值
*/
app.use((ctx, next) => {
  console.log('第一层中间件-a')
  ctx.response.body = 'hello world'
  const res = next() // Promise { '第二层中间件的返回值' }
  console.log('第一层中间件-b')
})
// 第二层中间件
app.use((ctx, next) => {
  console.log('第二层中间件-a')
  next()
  console.log('第二层中间件-b')
})
// 第三层中间件
app.use((ctx, next) => {
  console.log('最后一层中间件')
  return
  next()
})
/*
打印结果
request阶段：
第一层中间件-a
第二层中间件-a
最后一层中间件
第二层中间件-b
第一层中间件-b
response阶段：
第一层中间件-a
第二层中间件-a
最后一层中间件
第二层中间件-b
第一层中间件-b
*/

app.listen(3000, () => {
  console.log('web server running at http://127.0.0.1:3000')
})

```

#### 设置跨域

在响应头中间件中

```js
app. use(async(ctx, next)=>{
  ctx.set("Access-Control-Allow-Origin","*")
  ctx.set("Access-Control-Allow-Methods","OPTIONS, GET, PUT, POST, DELETE")
  await next()
}
```

