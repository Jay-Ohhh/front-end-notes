### axios

[axios官方中文文档](http://www.axios-js.com/docs/)

#### 二次封装

```js
import axios from 'axios'

// 在node中，有全局变量process表示的是当前的node进程
// process.env包含着关于系统环境的信息
// 但是process.env中并不存在NODE_ENV这个东西
// NODE_ENV是用户一个自定义的变量，vue-cli和react脚手架帮我们把process.env.NODE_ENV配置好了，我们不需要再自己去配置。
// 在webpack中它的用途是判断生产环境或开发环境的依据的
let api_base_url
if (process.env.NODE_ENV === 'development') {
  api_base_url = 'http://localhost:3000'
} else if (process.env.NODE_ENV === 'production') {
  api_base_url = 'https://nicemusic-api.lxhcool.cn'
}

let request = axios.create({
  timeout: 5000,
  baseURL: api_base_url,
  // 接口文档：跨域请求需要添加这行代码
  withCredentials: true,
})

// 添加请求拦截器
request.interceptors.request.use(
  function(config) {
    return config
  },
  function(error) {
    return Promise.reject(error)
  },
)

// 添加响应拦截器
request.interceptors.response.use(
  function(response) {
    let data = response.data
    let status = response.status
    if (status === 200) {
      return Promise.resolve(data)
    } else {
      return Promise.reject(response)
    }
  },
  function(error) {
    return Promise.reject(error)
  },
)
export { request }

```

#### 微信小程序发送异步请求二次封装

```js
// 同时发送异步请求的次数
let ajaxTimes = 0
export const request = function (params) {
  // 判断url中是否带有 /my/ 请求的是私有的路径，如果含有 /my/ ，则带上header token
  let header = { ...params.header } // params也许会带上其他的请求头参数
  if (params.url.includes('/my/')) {
    // 拼接header 带上token
    header['Authorization'] = wx.getStorageSync('token')
  }

  // 每请求一次，+1
  ajaxTimes++
  // 显示加载中效果
  wx.showLoading({
    title: '加载中',
    mask: true, // 添加页面蒙版，用户不能进行其他操作
  })

  // 定义公共的url
  const baseUrl = 'https://api-hmugo-web.itheima.net/api/public/v1'
  // 发送请求
  return new Promise((resolve, reject) => {
    wx.request({
      ...params,
      header,
      url: baseUrl + params.url,
      success: res => {
        resolve(res)
      },
      fail: err => {
        reject(err)
      },
      complete: () => {
        // 请求结束后，-1
        ajaxTimes--
        if (ajaxTimes === 0) {
          // 关闭加载图标，利用ajaxTimes可以控制所有请求发送完了再关闭
          setTimeout(() => {
            wx.hideLoading()
          }, 500)
        }
      },
    })
  })
}
```

#### 配置请求头headers

##### hearders配置选项

https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers

https://segmentfault.com/a/1190000018234763

**1、Genaral headers**

> Cache-Control——控制缓存的行为； [详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)
> Connection——决定当前的事务完成后，是否会关闭网络连接； [详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection)
> Date——创建报文的日期时间； [详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Date)
> Keep-Alive——用来设置超时时长和最大请求数；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Keep-Alive)
> Via——代理服务器的相关信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Via)
> Warning——错误通知；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Warning)
> Trailer——允许发送方在分块发送的消息后面添加额外的元信息； [详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Trailer)
> Transfer-Encoding——指定报文主体的传输编码方式；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Transfer-Encoding)
> Upgrade——升级为其他协议；

**2、Request headers**

> Accept——客户端可以处理的内容类型；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept)
> Accept-Charset——客户端可以处理的字符集类型；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Charset)
> Accept-Encoding——客户端能够理解的内容编码方式；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Encoding)
> Accept-Language——客户端可以理解的自然语言；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language)
> Authorization——Web 认证信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Authorization)
> Cookie——通过Set-Cookie设置的值；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie)
> DNT——表明用户对于网站追踪的偏好；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/DNT)
> From——用户的电子邮箱地址；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/From)
> Host——请求资源所在服务器；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Host)
> If-Match——比较实体标记（ETag）；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Match)
> If-Modified-Since——比较资源的更新时间；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Modified-Since)
> If-None-Match——比较实体标记（与 If-Match 相反）；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-None-Match)
> If-Range——资源未更新时发送实体 Byte 的范围请求；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Range)
> If-Unmodified-Since——比较资源的更新时间（与 If-Modified-Since 相反）；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Unmodified-Since)
> Origin——表明了请求来自于哪个站点；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Origin)
> Proxy-Authorization——代理服务器要求客户端的认证信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Proxy-Authorization)
> Range——实体的字节范围请求；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Range)
> Referer——对请求中 URI 的原始获取方；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referer)
> TE——指定用户代理希望使用的传输编码类型；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/TE)
> Upgrade-Insecure-Requests——表示客户端优先选择加密及带有身份验证的响应；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Upgrade-Insecure-Requests)
> User-Agent——浏览器信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent)

**3、Response Headers**

> Accept-Ranges——是否接受字节范围请求；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Ranges)
> Age——消息对象在缓存代理中存贮的时长，以秒为单位；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Age)
> Clear-Site-Data——表示清除当前请求网站有关的浏览器数据（cookie，存储，缓存）；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Clear-Site-Data)
> Content-Security-Policy——允许站点管理者在指定的页面控制用户代理的资源；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy__by_cnvoid)
> Content-Security-Policy-Report-Only—— [详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only)
> ETag——资源的匹配信息；[链接描述](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/ETag)
> Location——令客户端重定向至指定 URI；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Location)
> Proxy-Authenticate——代理服务器对客户端的认证信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Proxy-Authenticate)
> Public-Key-Pins——包含该Web 服务器用来进行加密的 public key （公钥）信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Public-Key-Pins)
> Public-Key-Pins-Report-Only——设置在公钥固定不匹配时，发送错误信息到report-uri；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Public-Key-Pins-Report-Only)
> Referrer-Policy——用来监管哪些访问来源信息——会在 Referer 中发送；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referrer-Policy)
> Server——HTTP 服务器的安装信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Server)
> Set-Cookie——服务器端向客户端发送 cookie；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie)
> Strict-Transport-Security——它告诉浏览器只能通过HTTPS访问当前资源；[详情](https://developer.mozilla.org/zh-CN/docs/Security/HTTP_Strict_Transport_Security)
> Timing-Allow-Origin——用于指定特定站点，以允许其访问Resource Timing API提供的相关信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Timing-Allow-Origin)
> Tk——显示了对相应请求的跟踪情况；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Tk)
> Vary——服务器缓存的管理信息；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Vary)
> WWW-Authenticate——定义了使用何种验证方式去获取对资源的连接；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/WWW-Authenticate)
> X-XSS-Protection——当检测到跨站脚本攻击 (XSS)时，浏览器将停止加载页面；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-XSS-Protection)

**4、Entity Headers**

> Allow——客户端可以处理的内容类型，这种内容类型用MIME类型来表示；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Allow)
> Content-Encoding——用于对特定媒体类型的数据进行压缩；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Encoding)
> Content-Language——访问者希望采用的语言或语言组合；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)
> Content-Length——发送给接收方的消息主体的大小；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length)
> Content-Location——替代对应资源的 URI；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Location)
> Content-Range——实体主体的位置范围；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Range)
> Content-Type——告诉客户端实际返回的内容的内容类型；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type)
> Expires——包含日期/时间， 即在此时候之后，响应过期；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Expires)
> Last-Modified——资源的最后修改日期时间；[详情](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified)

##### 如何配置

**全局配置或实例配置默认项**

```js
// axios 或 axiosIntance
const a = axios | axiosIntance
// 实际上是在config.headers添加'Authorization'，config是指例如axios.get(url[, config])等方法中的config
a.defaults.headers.common['Authorization'] = 'sss';
// 可以配置自定义属性
a.defaults.headers.common['something'] = 'aaa';
```

**如果没按照上述配置，也可以直接在请求中发送，config对象内：**

> 在请求中发送config.headers会覆盖上述全局/实例的默认配置

```js
{headers:{'something':'aaa'}}
```

##### Content-Type

上述中的配置有个**Content-Type**选项是例外的，全局或实例配置需如下：

```js
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
// 或
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded;charset=utf-8';
```

在请求中发送与上述是一样的：

```js
{headers:{'Content-Type':'application/x-www-form-urlencoded'}}
```

axios 使用 post 发送数据时，默认是直接把 json 放到请求体中提交到后端的。也就是说，我们的 Content-Type 变成了 application/json;charset=utf-8 ,这是axios默认的请求头content-type类型。但是实际我们后端要求的 'Content-Type': 'application/x-www-form-urlencoded' 为多见，这就与我们不符合。所以很多同学会在这里犯错误，导致请求数据获取不到。明明自己的请求地址和参数都对了却得不到数据。

**我们现在来说说post请求常见的数据格式（content-type）：**

1. Content-Type: application/json ： 请求体中的数据会以json字符串的形式发送到后端
2. Content-Type: application/x-www-form-urlencoded：请求体中的数据会以普通表单形式（键值对）发送到后端
3. Content-Type: multipart/form-data： 它会将请求体的数据处理为一条消息，以标签为单元，用分隔符分开。既可以上传键值对，也可以上传文件。

#### 取消重复请求

假设页面中有一个按钮，用户点击按钮后会发起一个 AJAX 请求。如果未对该按钮进行控制，当用户快速点击按钮时，则会发出重复请求。

##### 一、取消请求

axios需要取消令牌`cancelToken`才能取消请求，而取消令牌`cancelToken`是保存在config中。

> 例如：axios.get(url[, config]) 的 config 参数。

通过`axios.CancelToken.source`生成取消令牌`cancelToken`和取消方法`cancel`

> 您可以使用相同的取消令牌取消多个请求

```js
const CancelToken = axios.CancelToken;
const source = CancelToken.source(); //  每次执行CancelToken.source()都会生成不同的source（地址不同），因此不同的source的token也是不同

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function (err) {
  // 取消请求、请求失败都会有err
  // isCancel函数，该函数用来判断异常对象是不是取消原因对象(Cancel的实例)，即判断是否通过cancel方法取消请求，返回true或false
  if (axios.isCancel(err)) {
    console.log('Request canceled', err.message);
  } else {
    // handle error
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// cancel the request (the message parameter is optional)
source.cancel('Operation canceled by the user.');
```

你也可以通过传递一个函数到`CancelToken`构造函数创建cancel函数：

```js
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    // An executor function receives a cancel function as a parameter
    cancel = c; // 每次执行new CancelToken生成的c是不同的，因此可以声明一个变量保存c
  })
});

// cancel the request
cancel();
```

> cancel(message) 可传入提示信息message。

##### 二、如何判断重复请求

当请求方式、请求 URL 地址和请求参数都一样时，我们就可以认为请求是一样的。因此在每次发起请求时，我们就可以根据当前请求的请求方式、请求 URL 地址和请求参数来生成一个唯一的 key，同时为每个请求创建一个专属的 CancelToken，然后把 key 和 cancel 函数以键值对的形式保存到 Map 对象中，使用 Map 的好处是可以快速的判断是否有重复的请求：

```js
import qs from 'querystring' 

const pendingRequest = new Map();
// GET -> params；POST -> data
const requestKey = [method, url, qs.stringify(params), qs.stringify(data)].join('&'); 
const cancelToken = new CancelToken(function executor(cancel) {
  if(!pendingRequest.has(requestKey)){
    pendingRequest.set(requestKey, cancel);
  }
})
```

当出现重复请求的时候，我们就可以使用 cancel 函数来取消前面已经发出的请求，在取消请求之后，我们还需要把取消的请求从 `pendingRequest` 中移除。现在我们已经知道如何取消请求和如何判断重复请求，下面我们来介绍如何取消重复请求。

##### 三、如何取消重复请求

###### 3.1 定义辅助函数

在配置请求拦截器和响应拦截器前，先来定义 3 个辅助函数：

- `enerateReqKey`：用于根据当前请求的信息，生成请求 Key；

```js
function generateReqKey(config) {
  const { method, url, params, data } = config;
  return [method, url, qs.stringify(params), qs.stringify(data)].join("&");
}
```

- `addPendingRequest`：用于把当前请求信息添加到pendingRequest对象中；

```js
const pendingRequest = new Map();
function addPendingRequest(config) {
  const requestKey = generateReqKey(config);
  // config.cancelToken = config.cancelToken 是指我们发送某一请求时自己在config加的cancelToken，用于我们自己手动取消请求
  config.cancelToken = config.cancelToken || new axios.CancelToken((cancel) => {
    if (!pendingRequest.has(requestKey)) {
       pendingRequest.set(requestKey, cancel);
    }
  });
}
```

- `removePendingRequest`：检查是否存在重复请求，若存在则取消已发的请求。

```js
function removePendingRequest(config) {
  const requestKey = generateReqKey(config);
  if (pendingRequest.has(requestKey)) {
     const cancel = pendingRequest.get(requestKey);
     // 取消请求
     cancel(requestKey);
     pendingRequest.delete(requestKey);
  }
}
```

创建好 `generateReqKey`、`addPendingRequest` 和 `removePendingRequest` 函数之后，我们就可以设置请求拦截器和响应拦截器了。

###### 3.2 设置请求拦截器

```js
axios.interceptors.request.use(
  function (config) {
    removePendingRequest(config); // 检查是否存在重复请求，若存在则取消已发的请求
    addPendingRequest(config); // 把当前请求信息添加到pendingRequest对象中
    return config;
  },
  (error) => {
     return Promise.reject(error);
  }
);
```

###### 3.3 设置响应拦截器

```js
axios.interceptors.response.use(
  (response) => {
     removePendingRequest(response.config); // 从pendingRequest对象中移除请求
     return response;
   },
   (error) => {
      removePendingRequest(error.config || {}); // 从pendingRequest对象中移除请求
      if (axios.isCancel(error)) {
        console.log("已取消的重复请求：" + error.message);
      } else {
        // 添加异常处理
      }
      return Promise.reject(error);
   }
);
```

#### 在开发环境中代理API请求

原理：

通过一些方法设置代理，在请求发送(接收)之前加入中间层，

将不同的域名转换成相同的，解决了跨域的问题

客户端发送请求时

不直接到服务器

而是先到代理的中间层

在这里将http://localhost:8081/api这个域名装换为http://ahbcht.com，

再将请求发送到服务器。

> proxy只在开发环境下有效

##### Vue

**axiosIntance.js**

```js
axios.defaults.baseURL = '/api'
```

**vue.config.js**

```js
module.exports = {  
devServer: {  
    port: '8081', // 设置端口号  
    proxy: {  
        '/api': {  
          target: 'http://ahbcht.com', //API服务器的地址,将本地请求代理到该地址 
          ws: true, //代理websockets  
          changeOrigin: true, // 是否跨域，虚拟的站点需要更换origin  
          pathRewrite: {  
             // '^/api'是一个正则表达式，表示要匹配请求的url中，全部'http://localhost:8081/api' 转接为 http://ahbcht.com/  
             '^/api': '',  
           }  
         }, 
 		// ...其他代理
     },  
 }  
```



##### React

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

#### 请求事件

**request config**

```js
onUploadProgress: function (event) {
  // Do whatever you want with the native progress event
},
  
onDownloadProgress: function (event) {
  // Do whatever you want with the native progress event
},
```

