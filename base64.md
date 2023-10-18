### Base64
使用 64 个可打印字符来表示二进制数据的方法

Base64 encoding schemes are commonly used to encode binary data for storage or transfer over media that can only deal with ASCII text 

#### 64个可打印字符

Base64 uses the following alphabet to represent the radix-64 digits, alongside "`=`" as a padding character

A-Z，a-z，0-9，+，/

```
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/
```

> 在 URL 中一些字符有特殊含义，比如`/?=#`，URL Base64 编码会对其进行优化，替换`+/`为`-_`

#### 原理
Base64的索引与对应字符的关系，在 Base64 编码中，每个字符只使用了 6 位，即 2^6 = 64

| 字符 | base64索引 |
| ---- | ---------- |
| A-Z  | 0-25       |
| a-z  | 26-51      |
| 0-9  | 52-61      |
| +    | 62         |
| /    | 63         |

一个字符的ASCII码占用存储空间为1个字节/Byte（8Bit）

在ASCII码中规定，十进制为0~31、127这33个字符属于控制字符，32~126这95个字符属于可打印字符，也就是说网络传输只能传输这95个字符，不在这个范围内的字符无法传输。那么该怎么才能传输其他字符呢？其中一种方式就是使用Base64。

一个 base64 占 6 bit，然而ASCII码 占 8 bit，那么怎么使用 6 bit 来表示 8 bit 的数据呢？

 3\*8 bit === 4\*6 bit



##### 举例1

假设我们要编码的内容为`China`

对照 ascii 码表，China 对应的二进制数据为：`01000011 01101000 01101001 01101110 01100001`

编码的方式如下：

1、将原始数据按每3个字节为一块进行编码。一共3*8=24个bit位。最后不足3个的单独一块。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5537bcd2c69744b1be94d6f34a73aea2~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

2、将24个字节分为4组，每组6个bit。不足的在后面补0

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0948ebda2e3540a38fe8e8df0f40b4e4~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

3、在每组6个bit前插入两个 0 bit，变为8 * 4 = 32 个bit

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/87bf3e08b10a4c18b910793c5fbb7505~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?) 

4、不足4个字节的部分填充“0”，如果一组全部填充了0，则用"="表示，对照Base64码表，得到最终编码后的数据：`Q2hpbmE=`

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6b56dc7097b249c5a3337eb92160f7d9~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp?)

从上面的分析可以看出，如果最后一组不足 32 bits，Base64 会将3个字节的数据最终编码成了4个字节，体积约是原来的 4/3 倍。



##### 举例2

| ASCII字符                              |    S     |    o     |    n     |          |
| -------------------------------------- | :------: | :------: | :------: | :------: |
| ASCII十进制                            |    83    |   111    |   110    |          |
| ASCII二进制                            | 01010011 | 01101111 | 01101110 |          |
| 每6个bit为一组（将上面的3\*8变成4\*6） |  010100  |  110110  |  111101  |  101110  |
| 高位补0                                | 00010100 | 00110110 | 00111101 | 00101110 |
| 对应的base64索引                       |    20    |    54    |    61    |    46    |
| 对应的base64字符                       |    U     |    2     |    9     |    u     |

可以看到“Son”通过Base64编码转换成了“U29u”。这是刚刚好的情况，3个字符刚好转换成对应的4个Base64字符。但是，当需要转换的字符数不是3的倍数的情况下该怎么办呢？Base64规定，当需要转换的字符不是3的倍数时，一律采用补0的方式凑足3的倍数，具体如下表所示：
| ASCII字符                              |    S     |          |          |          |
| -------------------------------------- | :------: | :------: | :------: | :------: |
| ASCII十进制                            |    83    |          |          |          |
| ASCII二进制                            | 01010011 |          |          |          |
| 每6个bit为一组（将上面的3\*8变成4\*6） |  010100  |  110000  |  000000  |  000000  |
| 高位补0                                | 00010100 | 00110000 | 00000000 | 00000000 |
| 对应的base64索引                       |    20    |    48    |          |          |
| 对应的base64字符                       |    U     |    w     | = | = |
|每6个Bit为一组，第一组转换后为字符“U”，第二组末尾补4个0转换后为字符“w”。标准Base64编码规定：剩下的组全部补0且使用“=”替代。即字符“S”通过Base64编码后为“Uw==”。这就是Base64的编码过程。|||||



**使用base64图片**
优点：Base64 的好处是能够减少一个图片的 HTTP 请求，没有跨域问题。
缺点： HTML 或 CSS 文件体积的增大，加载base64图片会阻塞HTML和CSS的解析。外链图片可以在页面渲染完成后继续加载，不会造成阻塞。

> HTML的img标签元素的`src`属性和CSS样式`background-image : url()` 可以指定`data URL`（因为 `data URL` 也是 `URL`），`data URL`可以指示经过`base64`编码的二进制数据等等。`img.src`和`background-image : url()` 值为二进制数据时格式为下述data url的格式
> 采用Base64的编码方式将图片直接嵌入到网页中，而不是从外部载入，这样就减少了HTTP请求。

如果图片足够小且无法被制作成雪碧图（CssSprites），在整个网站的复用性很高且基本不会被更新，即通常用于无法被制成雪碧图的小图标（十几K以下）。



#### Data URI

Data URI scheme是在RFC2397中定义的，目的是将一些小的数据，直接嵌入到网页中，从而不用再从外部文件载入。 目前，Data URI scheme支持的类型有：
```JavaScript
data:,文本数据
data:text/plain,文本数据
data:text/html,HTML代码
data:text/html;base64,base64编码的HTML代码
data:text/css,CSS代码
data:text/css;base64,base64编码的CSS代码
data:text/javascript,Javascript代码
data:text/javascript;base64,base64编码的Javascript代码
data:image/gif;base64,base64编码的gif图片数据
data:image/png;base64,base64编码的png图片数据
data:image/jpeg;base64,base64编码的jpeg图片数据
data:image/x-icon;base64,base64编码的icon图片数据
```

[data url 常见问题](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/data_URIs)

- HTML代码格式化
一个 data URL 是一个文件中的文件，相对于文档来说这个文件可能就非常的长。因为 data URL 也是 URL，所以 data 会用空白符(换行符, 制表符, 空格)来对它进行格式化。但如果数据是经过 base64 编码的，就可能会遇到一些问题。
- **不同浏览器对data URL的长度限制不同**