### axios

[axios官方中文文档](http://www.axios-js.com/docs/)

#### 请求方法

**axios.request(config)**

**axios.get(url[, config])**

**axios.delete(url[, config])**

**axios.head(url[, config])**

**axios.options(url[, config])**

**axios.post(url[, data[, config]])**

**axios.put(url[, data[, config]])**

**axios.patch(url[, data[, config]])**

#### get请求、delete请求

**GET 请求不存在请求实体部分，键值对参数放置在 URL 尾部，**浏览器把form数据转换成一个字串（name1=value1&name2=value2...），然后把这个字串追加到url后面，用`?`分割，加载这个新的url。因此请求头不需要设置 Content-Type 字段。

```js
 // Add headers to the request
    if ('setRequestHeader' in request) {
      utils.forEach(requestHeaders, function setRequestHeader(val, key) {
        if (typeof requestData === 'undefined' && key.toLowerCase() === 'content-type') {
          // Remove Content-Type if data is undefined  如果没有data，则去掉content-type，意味着get或delete请求会自动去掉content-type
          delete requestHeaders[key];
        } else {
          // Otherwise add header to the request
          request.setRequestHeader(key, val);
        }
      });
    }
```

##### get、delete请求添加Content-Type

```js
var $http
// 添加一个新的axios实例
$http = axios.create({
  baseURL: url,
})
// 添加请求拦截器
$http.interceptors.request.use(function (config) {
  if (config.method == 'get') {
    config.data = true  // 绕过源码的if判断
  }
  config.headers['H-TOKEN'] = '111'
  return config;
}, function (error) {
  // 对请求错误做些什么
  return Promise.reject(error);
});
```

##### get、delete请求添加body传参

**axios 0.22.0 版本以下的**

```js
axios.delete("/xxx/xxx/delete", {data:ids})
```

**axios 0.22.0 版本以上的**

```js
axios.request({
        url:"/xxx/xxx/delete",
        method:"delete",
        data:ids
      })
```

**axios 0.22.0的版本在delete方法中，在config中配置data属性并不能上传payload数据，可采用axios.request()方法替代**

#### post请求、put请求

当 data 是对象时，自动会把 Content-Type设置为 application/json;charset=UTF-8

#### 响应数据

axios对响应的数据会做了一层封装，response.data才是后端发过来的整个响应数据体，例如：

```js
// response.data 
{
	data:{},
	msg:"",
	code:200
}
```

axios封装的response数据结构：

```js
config: {url: "/banner", method: "get", headers: {…}, baseURL: "http://localhost:3000", transformRequest: Array(1), …}

data: {banners: Array(10), code: 200}  // 这个是后端发来的响应数据

headers: {cache-control: "max-age=120", content-length: "5943", content-type: "application/json; charset=utf-8"}

request: XMLHttpRequest {readyState: 4, timeout: 5000, withCredentials: true, upload: XMLHttpRequestUpload, onreadystatechange: *ƒ*, …}

status: 200

statusText: "OK"

__proto__: Object
```



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

[如何在请求拦截器中取消请求-how to cancel request inside request interceptor properly](https://stackoverflow.com/questions/50461746/axios-how-to-cancel-request-inside-request-interceptor-properly)

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
    removePendingRequest(config); // 在请求开始前，取消之前的相同请求
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

##### 完整代码

```ts
// axios取消重复请求：
// 两个版本重复则取消之前、重复则取消之后

// 重复则取消之前:
import qs from 'qs';
import axios from 'axios';
import type { AxiosRequestConfig, Canceler } from 'axios';

// 用于根据当前请求的信息，生成请求 Key
function generateReqKey(config: AxiosRequestConfig) {
	const { method, url, params, data } = config;
	return [method, url, qs.stringify(params), qs.stringify(data)].join('&');
}

// 用于把当前请求信息添加到pendingRequest对象中
const pendingRequest = new Map<string, Canceler>();
function addPendingRequest(config) {
	const requestKey = generateReqKey(config);
  // 如果请求config含有cancelToken，则意味是开发者故意传进来的，因此取消请求则应该由开发者在适当时机去调用
	config.cancelToken =
		config.cancelToken ||
		new axios.CancelToken((cancel) => {
			if (!pendingRequest.has(requestKey)) {
				pendingRequest.set(requestKey, cancel);
			}
		});
}

// 检查是否存在重复请求，若存在则取消已发的请求
function removePendingRequest(config) {
	const requestKey = generateReqKey(config);
	if (pendingRequest.has(requestKey)) {
		const cancelToken = pendingRequest.get(requestKey);
		cancelToken!(requestKey);
		pendingRequest.delete(requestKey);
	}
}

// 设置请求拦截器
axios.interceptors.request.use(
	(config) => {
		removePendingRequest(config); // 检查是否存在重复请求，若存在则取消已发的请求
		addPendingRequest(config); // 把当前请求信息添加到pendingRequest对象中
		return config;
	},
	(error) => {
		return Promise.reject(error);
	}
);

// 设置响应拦截器
axios.interceptors.response.use(
	(response) => {
		removePendingRequest(response.config); // 从pendingRequest对象中移除请求
		return response;
	},
	(error) => {
		removePendingRequest(error.config || {}); // 从pendingRequest对象中移除请求
		if (axios.isCancel(error)) {
			console.log('已取消的重复请求：' + error.message);
		} else {
			// 添加异常处理
		}
		return Promise.reject(error);
	}
);
```



```ts
// 重复则取消之后:
import qs from 'qs';
import axios from 'axios';
import type { AxiosRequestConfig } from 'axios';

// 用于根据当前请求的信息，生成请求 Key
function generateReqKey(config: AxiosRequestConfig) {
	const { method, url, params, data } = config;
	return [method, url, qs.stringify(params), qs.stringify(data)].join('&');
}

// 用于把当前请求信息添加到pendingRequest对象中
const pendingRequest = new Set<string>();
function addPendingRequest(config) {
	const requestKey = generateReqKey(config);
	pendingRequest.add(requestKey);
}

// 检查是否存在重复请求，若存在则取消本次的请求
function needCancelRepeatCancel(config) {
	const requestKey = generateReqKey(config);
	if (pendingRequest.has(requestKey)) {
		return requestKey || 'Cancel repeated request';
	}
	return false;
}

// 移除已请求的key
function removePendingRequest(config) {
	const requestKey = generateReqKey(config);
	if (pendingRequest.has(requestKey)) {
		pendingRequest.delete(requestKey);
	}
}

// 设置请求拦截器
axios.interceptors.request.use(
	(config) => {
		const cancelMsg = needCancelRepeatCancel(config);
		if (cancelMsg) {
			return {
				...config,
				// 如果请求config含有cancelToken，则意味是开发者故意传进来的，因此取消请求则应该由开发者在适当时机去调用
        // https://stackoverflow.com/questions/50461746/axios-how-to-cancel-request-inside-request-interceptor-properly/67266644#67266644
				cancelToken: config.cancelToken || new axios.CancelToken((cancel) => cancel(cancelMsg)),
			};
		}
		addPendingRequest(config); // 把当前请求信息添加到pendingRequest对象中
		return config;
	},
	(error) => {
		return Promise.reject(error);
	}
);

// 设置响应拦截器
axios.interceptors.response.use(
	(response) => {
		removePendingRequest(response.config); // 从pendingRequest对象中移除请求
		return response;
	},
	(error) => {
		removePendingRequest(error.config || {}); // 从pendingRequest对象中移除请求
		if (axios.isCancel(error)) {
			console.log('已取消的重复请求：' + error.message);
		} else {
			// 添加异常处理
		}
		return Promise.reject(error);
	}
);

```



#### 请求重试

对于请求重试的功能来说，我们希望让用户不仅能够设置**重试次数**，而且可以设置**重试延时时间**。

适配器是一个函数，它接收一个 `config` 参数并返回一个 `Promise` 对象。

##### 定义 retryAdapterEnhancer 函数

为了让用户能够更灵活地控制请求重试的功能，我们定义了一个 `retryAdapterEnhancer` 函数，该函数支持两个参数：

- adapter：预增强的 Axios 适配器对象；

- options：默认的缓存配置对象，该对象支持  2 个属性，分别用于配置不同的功能：

- - times：全局设置请求重试的次数；
  - delay：全局设置请求延迟的时间，单位是 ms

```ts
// axios请求重试
import axios from 'axios';
import type { AxiosRequestConfig, AxiosAdapter } from 'axios';

function retryAdapterEnhancer(adapter: AxiosAdapter, options) {
	const { times = 0, delay = 300 } = options;

	return async (config: AxiosRequestConfig & { retryTimes?: number; retryDelay?: number }) => {
		const { retryTimes = times, retryDelay = delay } = config;
		let __retryCount = 0;
		const request = async () => {
			try {
				return await adapter(config);
			} catch (err) {
				// 判断是否进行重试
				if (!retryTimes || __retryCount >= retryTimes) {
					return Promise.reject(err);
				}
				__retryCount++; // 增加重试次数
				// 延时处理
				const delay = new Promise((resolve) => {
					setTimeout(() => {
						resolve(null);
					}, retryDelay);
				});
				// 重新发起请求
				return delay.then(() => {
					return request();
				});
			}
		};
		return request();
	};
}

const http = axios.create({
	baseURL: 'http://localhost:3000/',
	adapter: retryAdapterEnhancer(axios.defaults.adapter!, {
		retryDelay: 1000,
	}),
});
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

#### 跨域请求携带凭证 withCredentials

withCredentials : boolean，表示跨域请求时是否需要使用凭证

对于附带身份凭证的请求（通常是 `Cookie`），服务器不得设置 `Access-Control-Allow-Origin` 的值为“`*`”。

这是因为请求的首部中携带了 `Cookie` 信息，如果 `Access-Control-Allow-Origin` 的值为“`*`”，请求将会失败。而将 `Access-Control-Allow-Origin` 的值设置为具体域名，例如 `https://example.com`，则请求将成功执行。



#### refreshToken

思路是这样：token失效的话401，用refreshToken请求刷新token，此时正在刷新token，把之后的请求用一个队列数组缓存起来处于等待状态，等到刷新token后再逐个重试清空请求队列。那么如何做到让这个请求处于等待中呢？为了解决这个问题，我们得借助Promise。将请求存进队列中后，同时返回一个Promise，让这个Promise一直处于Pending状态（即不调用resolve），此时这个请求就会一直等啊等，只要我们不执行resolve，这个请求就会一直在等待。当刷新请求的接口返回来后，我们再调用resolve，逐个重试。

```js
let isReflesh = false
let reTryReqeustList = []
let token = ''

axios.interceptors.response.use((response) => {
  // ...
  return response
}, async (error) => {
  if (error.response && error.response.status === 401) {
    if (!isReflash) {
      isReflash = true
      try {
        reflashTokenConfig && await _reflashToken() // 记得更改token
        isReflash = false
        while (reTryReqeustList.length > 0) {
          const cb = reTryReqeustList.shift()
          cb()
        }
        return axios.request(error.response.config)
      } catch (err) {
        return Promise.reject(err)
      }
    } else {
      return new Promise((resolve) => {
        reTryReqeustList.push(
          () => resolve(axios.request({...error.response.config, token})) // 要使用最新的token
        )
      })
    }
  }
  return Promise.reject(error)
})
```



#### 单点登录

iframe + postMessage + localStorage

处理步骤
1、登录主系统，获取各个子系统的token；
2、点击项目地址链接后，创建一个隐藏iframe并使用postMessage将token传递过去，iframe接收后将token存到iframe所对应的域名下的localStorage；
3、通过window.open(url, ‘_blank’)打开一个新地址；
4、移除该iframe。

```js
// 创建iframe存储localStorage
var iframe = document.createElement('iframe')
iframe.setAttribute(
  'style',
  'position:absolute;width:0px;height:0px;left:-500px;top:-500px;'
)
iframe.src = addr
document.body.append(iframe)
// 传递token给加载完成后的iframe
iframe.onload = () => {
  iframe.contentWindow.postMessage(
    {
      token: this.tokenMap[key]
    },
    `${addr}`
  )
  setTimeout(function () {
    iframe.remove()
    iframe = null
  }, 5000)
  setTimeout(function () {
    window.open(addr, '_blank')
  }, 0)
}

```

