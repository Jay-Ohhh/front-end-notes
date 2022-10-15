# PDF

## jsPDF、html2canvas

```typescript
/**
 * If content that can show totally on one page with a stable position, use html() method.
 * Other cases, use the imperative APIs of jsPDF.
 * @see https://github.com/parallax/jsPDF#optional-dependencies
 * If only import `jspdf.umd.min.js` by <script>, you should also import optional dependencies when use html().
 * E.g. the html method, which depends on `html2canvas` 
 * and, when supplied with a string HTML document, `dompurify`. 
 */
```

[jsPDF + html2canvas A4分页截断 完美解决方案](https://juejin.cn/post/7138370283739545613)



## iframe print

```typescript
let iframe = document.createElement('iframe')
// MIME类型 or 非同源 url
iframe.src = "data:application/pdf..." || url 
iframe.style.display = "none"
iframe.onload = () => {
  // 同一源站策略阻止脚本访问不同源站的内容
  // throw error:  Blocked a frame with origin "http://localhost:4000" from accessing a cross-origin frame.
  iframe.contentWindow.print()
}
```

可以使用 **URL.createObjectURL** 构建本地 blob url

```typescript
let iframe = document.createElement('iframe');

const url = URL.createObjectURL(new Blob([doc.output("blob")], { type: "application/pdf" }));

iframe.src = url
iframe.style.display = "none"

// 需要等 iframe 加载完成才打印
// 不能 remove iframe，否则打印不了
iframe.onload = () => {
  iframe.contentWindow.print()
  iframe.onload = null
  URL.revokeObjectURL(url);
}
iframe.onerror = () => {
  iframe.contentWindow.print()
  iframe.onerror = null
  URL.revokeObjectURL(url);
}
```



# Authorization popup window

https://help.aliyun.com/document_detail/175896.html

https://github.com/XD2Sketch/react-oauth-popup/blob/master/src/index.tsx

**setInterval**

```typescript
//Authorization popup window code
function ShowAuthWindow(options:{
  path: string;
  callback: ({code:string}) => void;
  windowName = 'ConnectWithOAuth';
  windowFeatures = 'location=0,status=0,width=800,height=400,centerscreen=yes'
}) {
  	const { path , callback, windowName, windowFeatures } = options
    const start = +new Date();
    const oauthWindow = window.open(path, windowName, windowOptions);
    const timer = setInterval(() => {
        // expire after 5 min
        const isOver = (+new Date() - start) > 5 * 60 * 1000;
        const { search } =  oauthWindow.window.location
        const sch = new URLSearchParams(search.replace(/^\?/g, ''))
        // or const sch = new URL(location.href).searchParams
        const code = sch.get('code')
        if (code || that._oauthWindow.closed || isOver) {
            window.clearInterval(that._oauthInterval);
            options.callback({code});
            oauthWindow.window.close();
        }
    }, 700);
  
    return timer
}

//create new oAuth popup window and monitor it
ShowAuthWindow({
    path: "",
    callback: function (params) {
        if(params){
          console.log('callback');
        }
    }
});
```

**postMessage**

```typescript
//Authorization popup window code
function ShowAuthWindow(options:{
  path: string;
  callback: ({code:string}) => void;
  windowName = 'ConnectWithOAuth';
  windowFeatures = 'location=0,status=0,width=800,height=400,centerscreen=yes'
}) {
  	const { path , callback, windowName, windowFeatures } = options
    const start = +new Date();
    let oauthWindow = window.open(path, windowName, windowOptions);
  	const handleMessage = e => {
      	console.log(e.data)
      	// get access_token via e.data.code
        oauthWindow.window.close()
        oauthWindow = null
    }
    oauthWindow.addEventListner('message', handleMessage)
}

// html
<script>
  	const sch = new URL(location.href).searchParams
    const code = sch.get('code')
    if(sch.get('code')){
     	top.opener.postMessage({code}, location.origin)
    }
</script>
```

