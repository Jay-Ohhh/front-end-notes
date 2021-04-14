### WebSocket
[WebSocket-阮一峰](http://www.ruanyifeng.com/blog/2017/05/websocket.html)
[WebSocket-菜鸟教程](https://www.runoob.com/html/html5-websocket.html)
[轮询](https://www.cnblogs.com/huchong/p/8595644.html)

#### 简介
- WebSocket和http都是基于tcp协议的应用层协议，tcp是传输层协议。
  tcp传输层协议：连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。
- http和https协议的缺陷：
  通信只能由客户端发起，服务器不能主动向客户端推送信息。
- WebSocket的最大特点：
  服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话（
  全双工通讯协议）。
- 其他特点包括：
  （1）建立在 TCP 协议之上，同http一样通过TCP来传输数据，服务器端的实现比较容易。
  （2）与 HTTP 协议有着良好的兼容性。默认端口也是80（ws）和443（wss），并且<u>**握手阶段**</u>采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。
  （3）数据格式比较轻量，性能开销小，通信高效。
  （4）可以发送文本，也可以发送二进制数据。
  （5）没有同源限制，客户端可以与任意服务器通信。
  （6）协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。
  ![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017051503.jpg)

#### 握手过程

1. 浏览器、服务器建立TCP连接，三次握手。这是通信的基础，传输控制层，若失败后续都不执行。
2. TCP连接成功后，浏览器通过HTTP协议向服务器传送WebSocket握手数据。（开始前的HTTP握手）
3. 服务器收到客户端的握手请求后，同样采用HTTP协议返回同意握手数据。
4. 当收到了连接成功的消息后，通过TCP通道进行传输通信。（TCP协议，非HTTP协议）
5. 断开连接，TCP四次挥手。



#### WebSocket API

WebSocket 的用法相当简单。

```JavaScript
var ws = new WebSocket("wss://echo.websocket.org")

ws.onopen = function(evt) { 
  console.log("Connection open ...");
  ws.send("Hello WebSockets!")
 }

ws.onmessage = function(evt) {
  console.log( "Received Message: " + evt.data)
  ws.close()
 }

ws.onclose = function(evt) {
  console.log("Connection closed.")
 }
```
##### 构造函数
构造函数 WebSocket 用于新建 WebSocket 实例 ws
```JavaScript
var ws = new WebSocket('ws://localhost:8080');
```
- 静态成员
CONNECTING：值为0，表示正在连接。
OPEN：值为1，表示连接成功，可以通信了。
CLOSING：值为2，表示连接正在关闭。
CLOSED：值为3，表示连接已经关闭，或者打开连接失败。



##### 属性
- ws.readyState
返回实例对象的当前状态
0 - 表示连接尚未建立。
1 - 表示连接已建立，可以进行通信。
2 - 表示连接正在进行关闭。
3 - 表示连接已经关闭或者连接不能打开。

- ws.bufferedAmount
  是一个只读属性，用于返回已经被send()方法放入队列中但还没有被发送到网络中的数据的字节数。一旦队列中的所有数据被发送至网络，则该属性值将被重置为0。但是，若在发送过程中连接被关闭，则属性值不会重置为0。如果你不断地调用send()，则该属性值会持续增长。

  有时候需要检查传输数据的大小，尤其是客户端传输大量数据的时候。虽然send()方法会马上执行，但数据并不是马上传输。浏览器会缓存应用流出的数据，你可以使用bufferedAmount属性检查已经进入队列但还未被传输的数据大小，也可以用来判断发送是否结束。这个值不包含协议框架、操作系统缓存和网络软件的开销。

  

##### 事件
- open  用于指定连接成功后的回调函数
```JavaScript
ws.onopen = function (event) {
  ws.send('Hello Server!')
}
```
- close  用于指定连接关闭或连接失败的回调函数
```JavaScript
ws.onclose = function(event) {
  var code = event.code;
  var reason = event.reason;
  var wasClean = event.wasClean;
  // handle close event
};
```
- message  
用于指定收到服务器数据后的回调函数，WebSocket消息机制只支持字符串（String）和二进制(blob和ArrayBuffer)
```JavaScript
ws.onmessage = function(event) {
  var data = event.data;
  // 处理数据
};

ws.onmessage = function(event){
  if(typeof event.data === String) {
    console.log("Received data string");
  }

  if(event.data instanceof ArrayBuffer){
    var buffer = event.data;
    console.log("Received arraybuffer");
  }}
```
除了动态判断收到的数据类型，也可以使用binaryType属性，显式指定收到的二进制数据类型。
```JavaScript
// 收到的是 blob 数据
ws.binaryType = "blob";
ws.onmessage = function(e) {
  console.log(e.data.size);};
// 收到的是 ArrayBuffer 数据
ws.binaryType = "arraybuffer";
ws.onmessage = function(e) {
  console.log(e.data.byteLength);};
```

- error
用于指定连接失败后的回调函数 
```JavaScript
ws.onerror = function(event) {
  // handle error event
};
```
> 如果要指定多个回调函数，可以使用addEventListener方法

##### 方法
- send()
向服务器发送数据

发送文本
```JavaScript
ws.send('your message')
```

发送Blob对象
```JavaScript
var file = document
  .querySelector('input[type="file"]')
  .files[0];
ws.send(file);
```

发送 ArrayBuffer 对象
```JavaScript
// Sending canvas ImageData as ArrayBuffer
var img = canvas_context.getImageData(0, 0, 400, 320);
var binary = new Uint8Array(img.data.length);
for (var i = 0; i < img.data.length; i++) {
  binary[i] = img.data[i];
}
ws.send(binary.buffer);
```

### [长连接和短连接](https://www.cnblogs.com/gotodsp/p/6366163.html)

HTTP的长连接和短连接本质上是TCP长连接和短连接。HTTP属于应用层协议，在传输层使用TCP协议，在网络层使用IP协议。 IP协议主要解决网络路由和寻址问题，TCP协议主要解决如何在IP层之上可靠地传递数据包。

长连接和短连接的产生在于client和server采取的关闭策略。不同的应用场景适合采用不同的策略。

#### 短连接

建立连接——数据传输——关闭连接...建立连接——数据传输——关闭连接
连接只保持在数据传输过程，请求发起，连接建立，数据返回，连接关闭。它适用于一些实时数据请求，配合轮询来进行新旧数据的更替。

#### 长连接

建立连接——数据传输...（保持连接）...数据传输——关闭连接
长连接是在连接发起后，在请求关闭连接前客户端和服务端都保持连接，实质是保持这个通信管道，之后便可以对其进行复用。它适用于请求频繁的场景（直播，流媒体）。连接建立后，在该连接下的所有请求都可以重用这个长连接通道，避免了频繁连接请求，提升了效率。

> 每个TCP连接都需要三步握手，这需要时间，如果每个操作都是先连接，再操作的话那么处理速度会降低很多，所以每个操作完后都不断开，每次处理时直接发送数据包就OK了，不用建立TCP连接。例如：数据库的连接用长连接， 如果用短连接频繁的通信会造成socket错误，而且频繁的socket 创建也是对资源的浪费。 

在HTTP/1.0中默认使用短连接。
也就是说，客户端和服务器每进行一次HTTP操作，就建立一次连接，任务结束就中断连接。

从HTTP/1.1起，默认使用长连接，用以保持连接特性。使用长连接的HTTP协议，会在响应头加入这行代码：
`Connection:keep-alive`


长连接不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。实现长连接需要客户端和服务端都支持长连接。

长连接中TCP的保活功能主要为服务器应用提供。如果客户端已经消失而连接未断开，则会使得服务器上保留一个半开放的连接，而服务器又在等待来自客户端的数据，此时服务器将永远等待客户端的数据。保活功能就是试图在服务端器端检测到这种半开放的连接。

如果一个给定的连接在两小时内没有任何动作，服务器就向客户发送一个探测报文段，根据客户端主机响应探测4个客户端状态：
- 客户主机依然正常运行，且服务器可达。此时客户的TCP响应正常，服务器将保活定时器复位。
- 客户主机已经崩溃，并且关闭或者正在重新启动。上述情况下客户端都不能响应TCP。服务端将无法收到客户端对探测的响应。服务器总共发送10个这样的探测，每个间隔75秒。若服务器没有收到任何一个响应，它就认为客户端已经关闭并终止连接。
- 客户端崩溃并已经重新启动。服务器将收到一个对其保活探测的响应，这个响应是一个复位，使得服务器终止这个连接。
- 客户机正常运行，但是服务器不可达。这种情况与第二种状态类似。

#### 两者优缺点

- 长连接可以省去较多的TCP建立和关闭的操作，减少浪费，节约时间。对于频繁请求资源的客户来说，较适用长连接。不过这里存在一个问题，存活功能的探测周期太长，还有就是它只是探测TCP连接的存活，属于比较斯文的做法，遇到恶意的连接时，保活功能就不够使了。在长连接的应用场景下，client端一般不会主动关闭它们之间的连接，Client与server之间的连接如果一直不关闭的话，会存在一个问题，随着客户端连接越来越多，server早晚有扛不住的时候，这时候server端需要采取一些策略，如关闭一些长时间没有读写事件发生的连接，这样可 以避免一些恶意连接导致server端服务受损；如果条件再允许就可以以客户端机器为颗粒度，限制每个客户端的最大长连接数，这样可以完全避免某个客户端连累后端服务。
- 短连接对于服务器来说管理较为简单，存在的连接都是有用的连接，不需要额外的控制手段。但如果客户请求频繁，将在TCP的建立和关闭操作上消耗大量的时间和带宽。



### 短轮询和长轮询

#### 短轮询/轮询

客户端定时向服务器发送Ajax请求，服务器接到请求后马上返回响应信息并关闭连接。
短轮询：客户端定时向服务器发送Ajax请求，服务器接到请求后马上返回响应信息并关闭连接。
优点：后端程序编写比较容易。
缺点：请求中有大半是无用，浪费带宽和服务器资源。
实例：适于小型应用，不适用于那些同时在线用户数量比较大，并且很注重性能的Web应用。

```JavaScript
var xhr = new XMLHttpRequest(); 
setInterval(function(){ 
    xhr.open('GET','/user'); 
    xhr.onreadystatechange = function(){
    // do something...
    }; 
    xhr.send(); 
},10000)
```
上面的程序存在着一个的缺陷：在网络情况不稳定的情况下，服务器从接收请求、发送请求到客户端接收请求的总时间有可能超过10秒，而请求是以10秒间隔发送的，这样会导致接收的数据到达先后顺序与发送顺序不一致。于是出现了采用`setTimeout`的轮询方式：
```JavaScript
function ajax() {
    setTimeout(function() {
        var xhr = new XMLHttpRequest();
        xhr.open('GET','/user'); 
        xhr.onreadystatechange = function(){ 
            ajax(); 
        }; 
        xhr.send();
    }, 10000);}
```
程序首先设置10s后发起请求，当数据返回后，调用请求函数再隔10s发起第二次请求，以此类推。这样的话虽然无法保证两次请求之间的时间间隔为固定值，但是可以保证到达数据的顺序。

#### 长轮询

客户端请求1——服务端hold——数据更新，服务端返回——客户端请求2
客户端向服务器发送Ajax请求，服务器接到请求后hold住连接，直到有新消息才返回响应信息并关闭连接，客户端处理完响应信息后再向服务器发送新的请求。
优点：在无消息的情况下不会频繁的请求。
缺点：服务器hold连接会消耗资源。

```JavaScript
function ajax(){ 
    var xhr = new XMLHttpRequest();
    xhr.open('GET','/user'); 
    xhr.onreadystatechange = function(){ 
       ajax(); 
    };
    xhr.send(); 
}
```
> 长轮询和短轮询比起来，减少了很多不必要的http请求次数。

