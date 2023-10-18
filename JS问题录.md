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

https://gitee.com/liuloser/output_pdf_demo



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
  iframe = null
  URL.revokeObjectURL(url);
}
iframe.onerror = () => {
  iframe.contentWindow.print()
  iframe.onerror = null
  iframe = null
  URL.revokeObjectURL(url);
}
```



# keyup 、keydown

在触屏设备上：

event.keyCode always is 229

event.key always is "Unidentified"

可以通过 requestAnimationFrame 、 window.getSelection 、event.target 判断输入值



# 光标移动到行后

```javascript
requestAnimationFrame(()=>{
  const selection = window.getSelection();
  const range = document.createRange();
  // childNodes[0]: 文本
  range.setStart(event.target as HTMLSpanElement).childNodes[0], text.length);
  range.collapse(true);
  selection.removeAllRanges();
  selection.addRange(range);
})
```

