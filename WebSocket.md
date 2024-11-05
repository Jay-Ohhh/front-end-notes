### WebSocket

[WebSocket-阮一峰](http://www.ruanyifeng.com/blog/2017/05/websocket.html)

[WebSocket-菜鸟教程](https://www.runoob.com/html/html5-websocket.html)

[轮询](https://www.cnblogs.com/huchong/p/8595644.html)

[短连接轮询、长连接、Websocket 横向对比](https://mp.weixin.qq.com/s/HXF_AO24WeiOoq4Q5G_pBw)

#### 简介

-   WebSocket 和 http 都是基于 tcp 协议的应用层协议，tcp 是传输层协议。
    tcp 传输层协议：连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。
-   http 和 https 协议的缺陷：
    通信只能由客户端发起，服务器不能主动向客户端推送信息。
-   WebSocket 的最大特点：
    服务器可以主动向客户端推送信息，客户端也可以主动向服务器发送信息，是真正的双向平等对话（全双工通讯协议）。
-   其他特点包括：
    （1）建立在 TCP 协议之上，同 http 一样通过 TCP 来传输数据，服务器端的实现比较容易。TCP 需要三次握手。
    （2）与 HTTP 协议有着良好的兼容性。默认端口也是 80（ws）和 443（wss），并且<u>**握手阶段**</u>采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。在 WebSocket API 中，浏览器和服务器通过 HTTP 一次握手完成升级协议，两者之间就直接可以创建持久性的连接，并进行双向数据传输。
    （3）数据格式比较轻量，性能开销小，通信高效。
    （4）可以发送文本，也可以发送二进制数据。
    （5）没有同源限制，客户端可以与任意服务器通信。
    （6）协议标识符是 ws（如果加密，则为 wss），服务器网址就是 URL。
    ![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017051503.jpg)

#### 握手过程

1. 浏览器、服务器建立 TCP 连接，三次握手。这是通信的基础，传输控制层，若失败后续都不执行。
2. TCP 连接成功后，浏览器通过 HTTP 协议向服务器传送 WebSocket 握手数据。（开始前的 HTTP 握手）
3. 服务器收到客户端的握手请求后，确认升级到 WebSocket 协议，返回同意握手数据。
4. 当收到了连接成功的消息后，通过 TCP 通道进行传输通信。（TCP 协议）
5. 断开连接，TCP 四次挥手。

WebSocket 协议属于应用层协议，它依赖于传输层的 TCP 协议。WebSocket 通过 HTTP/1.1 协议的 **101** 状态码（切换协议）进行握手。为了创建 WebSocket 连接，需要通过浏览器发出请求，之后服务器进行回应，这个过程通常称为 “握手”（Handshaking）。

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

-   静态成员
    CONNECTING：值为 0，表示正在连接。
    OPEN：值为 1，表示连接成功，可以通信了。
    CLOSING：值为 2，表示连接正在关闭。
    CLOSED：值为 3，表示连接已经关闭，或者打开连接失败。

##### 属性

-   ws.readyState
    返回实例对象的当前状态
    0 - 表示连接尚未建立。
    1 - 表示连接已建立，可以进行通信。
    2 - 表示连接正在进行关闭。
    3 - 表示连接已经关闭或者连接不能打开。

-   ws.bufferedAmount
    是一个只读属性，用于返回已经被 send()方法放入队列中但还没有被发送到网络中的数据的字节数。一旦队列中的所有数据被发送至网络，则该属性值将被重置为 0。但是，若在发送过程中连接被关闭，则属性值不会重置为 0。如果你不断地调用 send()，则该属性值会持续增长。

    有时候需要检查传输数据的大小，尤其是客户端传输大量数据的时候。虽然 send()方法会马上执行，但数据并不是马上传输。浏览器会缓存应用流出的数据，你可以使用 bufferedAmount 属性检查已经进入队列但还未被传输的数据大小，也可以用来判断发送是否结束。这个值不包含协议框架、操作系统缓存和网络软件的开销。

##### 事件

-   open 用于指定连接成功后的回调函数

    ```JavaScript
    ws.onopen = function (event) {
    ws.send('Hello Server!')
    }
    ```

-   close 用于指定连接关闭或连接失败的回调函数

    ```JavaScript
    ws.onclose = function(event) {
    var code = event.code;
    var reason = event.reason;
    var wasClean = event.wasClean;
    // handle close event
    };
    ```

-   message
    用于指定收到服务器数据后的回调函数，WebSocket 消息机制只支持字符串（String）和二进制(blob 和 ArrayBuffer)

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
      }
    }
    ```

除了动态判断收到的数据类型，也可以使用 binaryType 属性，显式指定收到的二进制数据类型。

```js
// 收到的是 blob 数据
ws.binaryType = "blob";
ws.onmessage = function (e) {
    console.log(e.data.size);
};
// 收到的是 ArrayBuffer 数据
ws.binaryType = "arraybuffer";
ws.onmessage = function (e) {
    console.log(e.data.byteLength);
};
```

-   error
    用于指定连接失败后的回调函数

    ```JavaScript
    ws.onerror = function(event) {
    // handle error event
    };
    ```

    > 如果要指定多个回调函数，可以使用 addEventListener 方法

##### 方法

-   send()
    向服务器发送数据

发送文本

```JavaScript
ws.send('your message')
```

发送 Blob 对象

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

#### WebSocket 客户端 单例模式

需要注意处理连接、发送不成功的情况：

-   如果连接不成功，重新连接
-   重试的次数越多，重新连接的时间越大，避免浪费资源，因为服务器可能关机了
-   如果发送不成功，重新发送
    重试的次数越多，重新发送的时间越大，避免浪费资源，因为服务器可能关机了

```js
class SocketService {
    // 单例设计模式
    static instance = null;
    static get Instance() {
        if (!this.instance) {
            this.instance = new SocketService();
        }
        return this.instance;
    }
    // 和服务器连接的socket对象
    ws = null;
    // 存储回调函数
    callBackMapping = {};
    // 连接状态标识
    connected = false;
    // 重新发送次数
    sendRecord = 0;
    // 重新连接次数
    connectRecord = 0;
    // 定时器
    sendTimer = null;
    connectTimer = null;

    // 连接服务器的方法
    connect() {
        if (!window.WebSocket) {
            return console.log("您的浏览器不支持WebSocket");
        }
        this.ws = new WebSocket("ws://127.0.0.1:8080");
        // 连接成功
        this.ws.onopen = () => {
            console.log("连接服务端成功");
            this.connected = true;
            this.connectRecord = 0;
        };
        // 连接失败
        this.ws.onclose = () => {
            console.log("连接服务端失败");
            this.connected = false;
            // 如果连接不成功，重新连接
            // 重试的次数越多，重新连接的时间越大，避免浪费资源，因为服务器可能关机了
            this.connectRecord++;
            if (this.connectTimer) clearTimeout(this.connectTimer);
            if (this.connectRecord * 500 >= Number.MAX_SAFE_INTEGER) {
                this.connectRecord = 0;
                return;
            }
            this.connectTimer = setTimeout(() => {
                this.connect();
            }, this.connectRecord * 500);
        };
        // 得到服务端传过来的数据
        this.ws.onmessage = msg => {
            console.log("从服务端获取到了数据");
            // 真正从服务器发送过来的数据在msg中的data字段
            // console.log(msg.data)
            const receive = JSON.parse(msg.data);
            const socketType = receive.socketType;
            if (this.callBackMapping[socketType]) {
                const action = receive.action;
                if (action === "getData") {
                    const realData = JSON.parse(receive.data);
                    this.callBackMapping[socketType].call(this, realData);
                } else if (action === "fullScreen") {
                    this.callBackMapping[socketType].call(this, receive);
                } else if (action === "themeChange") {
                    this.callBackMapping[socketType].call(this);
                }
            }
        };
    }

    // 注册回调函数
    registerCallback(socketType, callback) {
        this.callBackMapping[socketType] = callback;
    }
    // 注销回调函数
    unRegisterCallback(socketType) {
        this.callBackMapping[socketType] = null;
    }
    // 发送数据
    send(data) {
        if (this.connected) {
            this.sendRecord = 0;
            this.ws.send(JSON.stringify(data));
        } else {
            // 如果发送不成功，重新发送
            // 重试的次数越多，重新发送的时间越大，避免浪费资源，因为服务器可能关机了
            this.sendRecord++;
            if (this.sendTimer) clearTimeout(this.sendTimer);
            if (this.sendRecord * 500 >= Number.MAX_SAFE_INTEGER) {
                this.sendRecord = 0;
                return;
            }
            this.sendTimer = setTimeout(() => {
                this.send(data);
            }, this.sendRecord * 500);
        }
    }
}
export default SocketService;
```

##### 心跳

实现心跳检测的思路是：每隔一段固定的时间，向服务器端发送一个`ping`数据，如果在正常的情况下，服务器会返回一个`pong`给客户端，如果客户端通过`onMessage`事件能监听到的话，说明请求正常，数据为 pong 时，清除心跳定时器重新定时发送一个心跳信息；如果是网络断开的情况下，在指定的时间内服务器端并没有返回心跳响应消息，因此服务器端断开了，因此这个时通过`onClose`事件监听到。因此在`onClose`事件内，我们可以调用`reconnect`事件进行重连操作。

```javascript
interface MySocketProps {
	url: string;
	onMessage?: (data: object) => void;
	onError?: (data: object) => void;
	heartCheckTimeout?: number;
	reconnectTimeout?: number;
	serverTimeout?: number;
}

class MySocket {
	protected socket: WebSocket | null;
	protected url: string;
	protected heartCheckTimer: NodeJS.Timeout | null; // 心跳检测定时器
	protected serverTimer: NodeJS.Timeout | null;
	protected reconnectTimer: NodeJS.Timeout | null; // 重连定时器
	protected lockReconnect: boolean; // 重连锁，避免多个重连
	protected heartCheckTimeout: number; // 心跳检测间隔
	protected reconnectTimeout: number; // 心跳检测间隔
	protected serverTimeout: number; // 判断服务器没连上的时间
	protected onMessage: ((data: object) => void) | undefined;
	protected onError: ((data: object) => void) | undefined;
	protected reconnectCount: number; // 重连次数

	// 单例模式
	static instanceMap = new Map<string, MySocket>();
	static getInstance(props: MySocketProps) {
		if (!this.instanceMap.has(props.url)) {
			this.instanceMap.set(props.url, new MySocket(props));
		}
		return this.instanceMap.get(props.url)!;
	}

	constructor(props: MySocketProps) {
		this.socket = null;
		this.url = props.url;
		this.heartCheckTimer = null;
		this.serverTimer = null;
		this.reconnectTimer = null;
		this.lockReconnect = false;
		this.heartCheckTimeout = props.heartCheckTimeout || 5000;
		this.reconnectTimeout = props.heartCheckTimeout || 3000;
		this.serverTimeout = props.serverTimeout || 4000;
		this.onMessage = props.onMessage;
		this.onError = props.onError;
		this.reconnectCount = 0;
	}

	init() {
		try {
			if ('WebSocket' in window) {
				this.socket = new WebSocket(this.url);
				this.socket.onopen = this.websocketOnOpen;
				this.socket.onerror = this.websocketOnError;
				this.socket.onmessage = this.websocketOnMessage;
				this.socket.onclose = this.websocketOnClose;
			} else {
				console.log('您的浏览器不支持websocket');
			}
		} catch (e) {
			this.reconnect();
		}
	}

	heartCheck() {
		this.clearAllTimer();
		this.heartCheckTimer = setTimeout(() => {
			this.socket?.send('ping');

			this.serverTimer = setTimeout(() => {
				// 如果超过一定时间还没响应(响应后触发重置)，说明后端断开了
				this.socket?.close(); // 关闭会触发重连方法，重连有次数限制
			}, this.serverTimeout);
		}, this.heartCheckTimeout);
	}

	clearAllTimer() {
		this.serverTimer && clearTimeout(this.serverTimer);
		this.heartCheckTimer && clearTimeout(this.heartCheckTimer);
		this.reconnectTimer && clearTimeout(this.reconnectTimer);
	}

	reconnect() {
		if (this.lockReconnect) {
			return;
		}
		console.log('尝试重连');
		this.reconnectCount++;
		if (this.reconnectCount > 20) {
			this.reconnectCount = 0;
			this.destroy();
			return;
		}

		this.lockReconnect = true;
		this.reconnectTimer && clearTimeout(this.reconnectTimer);
		this.reconnectTimer = setTimeout(() => {
			this.lockReconnect = false;
			this.init();
		}, this.reconnectTimeout);
	}

	websocketOnOpen() {
		console.log('WebSocket连接成功', this.socket!.readyState);
    this.reconnectCount = 0;
    this.lockReconnect = false;
		this.heartCheck();
	}

	websocketOnMessage(e: MessageEvent) {
		const res = JSON.parse(e.data);
		this.onMessage?.(res);
		if (e.data === 'pong') {
			// 数据体因后端而异
			// 消息获取成功，重置心跳
			this.heartCheck();
		}
	}

	websocketOnClose(e) {
		console.log(e);
		this.reconnect();
	}

	websocketOnError(e) {
		console.log('WebSocket连接发生错误', e);
		this.reconnect();
	}

	send(data: object) {
		if (this.socket && this.socket.readyState === WebSocket.OPEN) {
			this.socket.send(JSON.stringify(data));
		}
	}

	destroy() {
		console.log('destroy!');
		this.lockReconnect = true;
		this.socket && this.socket.close();
		this.clearAllTimer();
		this.socket = null;
		MySocket.instanceMap.delete(this.url);
	}
}

// 不要暴露 MyScoket，而是暴露 SingleSocket 给使用者
export class SingleSocket {
	instance: MySocket;
	constructor(props: MySocketProps) {
		this.instance = MySocket.getInstance(props);
	}
}

// 使用
const socketInstance = new SingleSocket({ url: '123' }).instance;
socketInstance.init();

```

#### WebSocket 服务器端

```js
const WebSocket = require("ws");
const ws = new WebSocket.Server({ port: 8080 });
// 对客户端的连接事件进行监听
ws.on("connection", client => {
    // 对客户端的连接对象进行message事件监听
    // msg是客户端发给服务器端的数据
    client.on("message", message => {
        console.log("received: %s", message);
    });
    client.send("something");
});
```

### [长连接和短连接](https://www.cnblogs.com/gotodsp/p/6366163.html)

HTTP 的长连接和短连接本质上是 TCP 长连接和短连接。HTTP 属于应用层协议，在传输层使用 TCP 协议，在网络层使用 IP 协议。 IP 协议主要解决网络路由和寻址问题，TCP 协议主要解决如何在 IP 层之上可靠地传递数据包。

长连接和短连接的产生在于 client 和 server 采取的关闭策略。不同的应用场景适合采用不同的策略。

#### 短连接

建立连接——数据传输——关闭连接...建立连接——数据传输——关闭连接
连接只保持在数据传输过程，请求发起，连接建立，数据返回，连接关闭。它适用于一些实时数据请求，配合轮询来进行新旧数据的更替。

#### 长连接

建立连接——数据传输...（保持连接）...数据传输——关闭连接
长连接是在连接发起后，在请求关闭连接前客户端和服务端都保持连接，实质是保持这个通信管道，之后便可以对其进行复用。它适用于请求频繁的场景（直播，流媒体）。连接建立后，在该连接下的所有请求都可以重用这个长连接通道，避免了频繁连接请求，提升了效率。

> 每个 TCP 连接都需要三步握手，这需要时间，如果每个操作都是先连接，再操作的话那么处理速度会降低很多，所以每个操作完后都不断开，每次处理时直接发送数据包就 OK 了，不用建立 TCP 连接。例如：数据库的连接用长连接， 如果用短连接频繁的通信会造成 socket 错误，而且频繁的 socket 创建也是对资源的浪费。

在 HTTP/1.0 中默认使用短连接。
也就是说，客户端和服务器每进行一次 HTTP 操作，就建立一次连接，任务结束就中断连接。

从 HTTP/1.1 起，默认使用长连接，用以保持连接特性。使用长连接的 HTTP 协议，会在响应头加入这行代码：
`Connection:keep-alive`

长连接不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如 Apache）中设定这个时间。实现长连接需要客户端和服务端都支持长连接。

长连接中 TCP 的保活功能主要为服务器应用提供。如果客户端已经消失而连接未断开，则会使得服务器上保留一个半开放的连接，而服务器又在等待来自客户端的数据，此时服务器将永远等待客户端的数据。保活功能就是试图在服务端器端检测到这种半开放的连接。

如果一个给定的连接在两小时内没有任何动作，服务器就向客户发送一个探测报文段，根据客户端主机响应探测 4 个客户端状态：

-   客户主机依然正常运行，且服务器可达。此时客户的 TCP 响应正常，服务器将保活定时器复位。
-   客户主机已经崩溃，并且关闭或者正在重新启动。上述情况下客户端都不能响应 TCP。服务端将无法收到客户端对探测的响应。服务器总共发送 10 个这样的探测，每个间隔 75 秒。若服务器没有收到任何一个响应，它就认为客户端已经关闭并终止连接。
-   客户端崩溃并已经重新启动。服务器将收到一个对其保活探测的响应，这个响应是一个复位，使得服务器终止这个连接。
-   客户机正常运行，但是服务器不可达。这种情况与第二种状态类似。

#### 两者优缺点

-   长连接可以省去较多的 TCP 建立和关闭的操作，减少浪费，节约时间。对于频繁请求资源的客户来说，较适用长连接。不过这里存在一个问题，存活功能的探测周期太长，还有就是它只是探测 TCP 连接的存活，属于比较斯文的做法，遇到恶意的连接时，保活功能就不够使了。在长连接的应用场景下，client 端一般不会主动关闭它们之间的连接，Client 与 server 之间的连接如果一直不关闭的话，会存在一个问题，随着客户端连接越来越多，server 早晚有扛不住的时候，这时候 server 端需要采取一些策略，如关闭一些长时间没有读写事件发生的连接，这样可 以避免一些恶意连接导致 server 端服务受损；如果条件再允许就可以以客户端机器为颗粒度，限制每个客户端的最大长连接数，这样可以完全避免某个客户端连累后端服务。
-   短连接对于服务器来说管理较为简单，存在的连接都是有用的连接，不需要额外的控制手段。但如果客户请求频繁，将在 TCP 的建立和关闭操作上消耗大量的时间和带宽。

### 短轮询和长轮询

#### 短轮询/轮询

客户端定时向服务器发送 Ajax 请求，服务器接到请求后马上返回响应信息并关闭连接。
短轮询：客户端定时向服务器发送 Ajax 请求，服务器接到请求后马上返回响应信息并关闭连接。
优点：后端程序编写比较容易。
缺点：请求中有大半是无用，浪费带宽和服务器资源。
实例：适于小型应用，不适用于那些同时在线用户数量比较大，并且很注重性能的 Web 应用。

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

上面的程序存在着一个的缺陷：在网络情况不稳定的情况下，服务器从接收请求、发送请求到客户端接收请求的总时间有可能超过 10 秒，而请求是以 10 秒间隔发送的，这样会导致接收的数据到达先后顺序与发送顺序不一致。于是出现了采用`setTimeout`的轮询方式：

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

程序首先设置 10s 后发起请求，当数据返回后，调用请求函数再隔 10s 发起第二次请求，以此类推。这样的话虽然无法保证两次请求之间的时间间隔为固定值，但是可以保证到达数据的顺序。

#### 长轮询

客户端请求 1——服务端 hold——数据更新，服务端返回——客户端请求 2
客户端向服务器发送 Ajax 请求，服务器接到请求后 hold 住连接，直到有新消息才返回响应信息并关闭连接，客户端处理完响应信息后再向服务器发送新的请求。
优点：在无消息的情况下不会频繁的请求。
缺点：服务器 hold 连接会消耗资源。

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

> 长轮询和短轮询比起来，减少了很多不必要的 http 请求次数。

### Server-Sent Events

在线 demo：https://www.esegece.com:2053/sse

http://www.ruanyifeng.com/blog/2017/05/server-sent_events.html
https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events/Using_server-sent_events
**`EventSource`** 是服务器推送的一个网络事件接口。一个 EventSource 实例会对 HTTP 服务开启一个持久化的连接，以`text/event-stream` 格式发送事件，会一直保持开启直到被要求关闭（EventSource.close() 或 网页关闭）。

SSE 与 WebSocket 作用相似，都是建立浏览器与服务器之间的通信渠道，然后服务器向浏览器推送信息。

总体来说，WebSocket 更强大和灵活。因为它是全双工通道，可以双向通信；SSE 是单向通道，只能服务器向浏览器发送，因为流信息本质上就是下载。

![img](https://www.ruanyifeng.com/blogimg/asset/2017/bg2017052702.jpg)

WebSocket 和 SSE 区别

-   WebSocket 全双工，SSE 单工
-   SSE 默认支持断线重连，WebSocket 需要自己实现。
-   SSE 只能用来传送文本，因此二进制数据需要编码成二进制字符串（utf-8）后传送，WebSocket 默认支持传送二进制数据。
-   WebSocket 没有同源限制
-   SSE 支持自定义发送的消息类型。

#### 服务器实现

##### 数据格式

The event stream（text/event-stream） is a simple stream of text data which must be encoded using UTF-8.
因此，服务器向浏览器发送的 SSE 数据，必须使用 UTF-8 编码的文本，具有如下的 HTTP 头信息。

> ```markup
> Content-Type: text/event-stream
> Cache-Control: no-cache
> Connection: keep-alive
> ```

上面三行之中，第一行的`Content-Type`必须指定 MIME 类型为`event-steam`。

每一次发送的信息，由若干个`message`组成，每个`message`之间用`\n\n`分隔。每个`message`内部由若干行组成，每一行都是如下格式。

> ```markup
> [field]: value\n
> ```

上面的`field`可以取四个值。

> -   data
> -   event
> -   id
> -   retry

**注释**

此外，还可以有冒号开头的行，表示注释。通常，服务器每隔一段时间就会向浏览器发送一个注释，保持连接不中断。

> ```markup
> : This is a comment
> ```

下面是一个例子。

> ```markup
> : this is a test stream\n\n
>
> data: some text\n\n
>
> data: another message\n
> data: with two lines \n\n
> ```

**data 字段**

数据内容用`data`字段表示。

> ```markup
> data:  message\n\n
> ```

如果数据很长，可以分成多行，最后一行用`\n\n`结尾，前面行都用`\n`结尾。

> ```markup
> data: begin message\n
> data: continue message\n\n
> ```

下面是一个发送 JSON 数据的例子。

> ```markup
> data: {\n
> data: "foo": "bar",\n
> data: "baz", 555\n
> data: }\n\n
> ```

**id 字段**

数据标识符用`id`字段表示，相当于每一条数据的编号。

> ```markup
> id: msg1\n
> data: message\n\n
> ```

浏览器用`lastEventId`属性读取这个值。一旦连接断线，浏览器会发送一个 HTTP 头，里面包含一个特殊的`Last-Event-ID`头信息，将这个值发送回来，用来帮助服务器端重建连接。因此，这个头信息可以被视为一种同步机制。

**event 字段**

`event`字段表示自定义的事件类型，默认是`message`事件。浏览器可以用`addEventListener()`监听该事件。

> ```markup
> event: foo\n
> data: a foo event\n\n
>
> data: an unnamed event\n\n
>
> event: bar\n
> data: a bar event\n\n
>
>
> // client
> const sse = new EventSource(url);
> sse.addEventListener("bar", (e)=>{
> 	console.log(e);
> })
> ```

**retry 字段**

服务器可以用`retry`字段，指定浏览器重新发起连接的时间间隔。

> ```markup
> retry: 10000\n
> ```

两种情况会导致浏览器重新发起连接：一种是时间间隔到期，二是由于网络错误等原因，导致连接出错。

#### express-sse

https://github.com/dpskvn/express-sse/tree/master

```javascript
"use strict";

/**
 * Require the module dependencies
 */

const EventEmitter = require("events").EventEmitter;

/**
 * Server-Sent Event instance class
 * @extends EventEmitter
 */
class SSE extends EventEmitter {
    /**
     * Creates a new Server-Sent Event instance
     * @param [array] initial Initial value(s) to be served through SSE
     * @param [object] options SSE options
     */
    constructor(initial, options) {
        super();

        if (initial) {
            this.initial = Array.isArray(initial) ? initial : [initial];
        } else {
            this.initial = [];
        }

        if (options) {
            this.options = options;
        } else {
            this.options = { isSerialized: true };
        }

        this.init = this.init.bind(this);
    }

    /**
     * The SSE route handler
     */
    init(req, res) {
        let id = 0;
        // 设置为 0，禁用超时
        req.socket.setTimeout(0);
        // 创建 TCP 连接时，是否启用 Nagle 算法。Nagle 的算法会在数据通过网络发送之前延迟数据。 它试图以延迟为代价来优化吞吐量。
        req.socket.setNoDelay(true);
        // 启用/禁用保持活动功能
        req.socket.setKeepAlive(true);
        res.statusCode = 200;
        res.setHeader("Content-Type", "text/event-stream");
        res.setHeader("Cache-Control", "no-cache");
        res.setHeader("X-Accel-Buffering", "no");
        if (req.httpVersion !== "2.0") {
            res.setHeader("Connection", "keep-alive");
        }
        if (this.options.isCompressed) {
            res.setHeader("Content-Encoding", "deflate");
        }

        // Increase number of event listeners on init
        this.setMaxListeners(this.getMaxListeners() + 2);

        const dataListener = data => {
            if (data.id) {
                res.write(`id: ${data.id}\n`);
            } else {
                res.write(`id: ${id}\n`);
                id += 1;
            }
            if (data.event) {
                res.write(`event: ${data.event}\n`);
            }
            res.write(`data: ${JSON.stringify(data.data)}\n\n`);
            res.flush();
        };

        const serializeListener = data => {
            const serializeSend = data.reduce((all, msg) => {
                all += `id: ${id}\ndata: ${JSON.stringify(msg)}\n\n`;
                id += 1;
                return all;
            }, "");
            res.write(serializeSend);
        };

        this.on("data", dataListener);

        this.on("serialize", serializeListener);

        if (this.initial) {
            if (this.options.isSerialized) {
                this.serialize(this.initial);
            } else if (this.initial.length > 0) {
                this.send(this.initial, this.options.initialEvent || false);
            }
        }

        // Remove listeners and reduce the number of max listeners on client disconnect
        req.on("close", () => {
            this.removeListener("data", dataListener);
            this.removeListener("serialize", serializeListener);
            this.setMaxListeners(this.getMaxListeners() - 2);
        });
    }

    /**
     * Update the data initially served by the SSE stream
     * @param {array} data array containing data to be served on new connections
     */
    updateInit(data) {
        this.initial = Array.isArray(data) ? data : [data];
    }

    /**
     * Empty the data initially served by the SSE stream
     */
    dropInit() {
        this.initial = [];
    }

    /**
     * Send data to the SSE
     * @param {(object|string)} data Data to send into the stream
     * @param [string] event Event name
     * @param [(string|number)] id Custom event ID
     */
    send(data, event, id) {
        this.emit("data", { data, event, id });
    }

    /**
     * Send serialized data to the SSE
     * @param {array} data Data to be serialized as a series of events
     */
    serialize(data) {
        if (Array.isArray(data)) {
            this.emit("serialize", data);
        } else {
            this.send(data);
        }
    }
}

module.exports = SSE;
```

"X-Accel-Buffering" 是一个 HTTP 响应头，通常用于 Nginx 配置中。它的作用是控制 Nginx 是否启用响应缓存，以及缓存的大小。

当 "X-Accel-Buffering" 的值为 "no" 时，Nginx 将禁用缓存，即使响应体中包含可缓存的内容。这通常用于实时流媒体等需要立即响应的场景，以避免将数据缓存起来导致延迟增加。

当 "X-Accel-Buffering" 的值为 "yes" 或未设置时，Nginx 将启用缓存，以提高响应速度。在这种情况下，Nginx 会将响应体缓存到内存中，直到缓存达到一定大小或响应体完全被接收为止，然后再将缓存的响应发送给客户端。

需要注意的是，"X-Accel-Buffering" 只影响 Nginx 对响应的处理方式，而不会直接影响客户端的行为。客户端仍然可以根据响应中的其他头部信息来决定是否缓存响应。

#### 使用

```javascript
// Server:
var SSE = require('express-sse');
var sse = new SSE(["array", "containing", "initial", "content", "(optional)"]);

const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const PORT = 3000;

app.use(cors());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}));


app.get('/sse', sse.init);

sse.send(content);
sse.send(content, eventName);
sse.send(content, eventName, customID);
sse.updateInit(["array", "containing", "new", "content"]);
sse.serialize(["array", "to", "be", "sent", "as", "serialized", "events"]);


app.listen(PORT, () => {
  console.log(`Facts Events service listening at http://localhost:${PORT}`)
});


// Client:
var es = new EventSource('/stream');

es.onmessage = function (event) {
  ...
};

es.addEventListener(eventName, function (event) {
  ...
});
```

### WebSocket 和 SSE 的使用场景

SSE 通常用于低频率的数据更新，例如推送新消息。

WebSocket 通常用于高频率的数据传输，例如在线游戏或实时协作。

### 全双工 半双工 单工

全双工（full-duplex）：允许数据在两个方向上同时进行传输。在全双工通信中，发送方和接收方可以同时发送和接收数据，互不干扰。

半双工通信：数据只能在一个方向上传输，但每个方向的数据传输是双向的。例如，当一方向的数据发送时，另一方向必须先停止发送，等待数据接收完成后，才能开始发送数据。

单工通信：数据只能在一个方向上传输，而另一个方向无法传输数据。
