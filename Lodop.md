### 基本步骤

嵌入Lodop

通常情况下,在页面中嵌入Lodop如下引用代码，然后用一个全局对象变量来使用控件

在head或body中加入：

```html
<script src="LodopFuncs.js"></script>
<object  id="LODOP_OB" classid="clsid:2105C259-1E0C-4534-8141-A753534CB4CA" width=0 height=0> 
  <embed id="LODOP_EM" type="application/x-print-lodop" width=0 height=0></embed>
</object>
```

在调用Lodop功能前，先用如下JS过程获得控件对象：

```js
var LODOP=getLodop(document.getElementById('LODOP_OB'),document.getElementById('LODOP_EM'));
// 或者
var LODOP=getLodop();
```

如果仅部署C-Lodop则更简单，以上所有代码都省略，只加如下一句：

```html
<script src="http://localhost:8000/CLodopFuncs.js"></script>
```

**LodopFuncs.js** 会自动判断是否已经安装**Lodop**或**C-Lodop**控件。

最基本的打印过程至少有**初始化语句**、**添内容语句**和**打印语句**三部分组成，例如：

```js
LODOP.PRINT_INIT("打印任务名"); // 首先初始化语句，Lodop会根据该名记忆相关的打印设置、打印维护信息。
LODOP.ADD_PRINT_TEXT(0,0,100,20,"文本内容一"); //然后多个SET语句及ADD语句（先SET后ADD）
LODOP.PRINT(); //最后一个打印(或预览、维护、设计)语句
```

### API

https://blog.csdn.net/weixin_37020977/article/details/81560485

#### 实例对象

```js
var LODOP=getLodop();
```

以下方法均由 **LODOP**调用

#### 打印初始化

PRINT_INIT(strPrintTaskName)

功能：初始化运行环境，清理异常打印遗留的系统资源，设定打印任务名。

- strTaskName：string|null，

  - 打印任务名，由开发者自主设定，未限制长度，字符要求符合Windows文件起名规则，Lodop会根据该名记忆相关的打印设置、打印维护信息。

  - 若strTaskName空，控件则不保存本地化信息，打印全部由页面程序控制。

- 结果：返回逻辑值

  返回逻辑真表示初始化成功，逻辑假表示初始化失败，失败原因有：前一个打印事务没有完成；操作系统没有打印机(驱动)等。

建议或要求：

该函数与**PRINT_INITA**都有初始化功能，每个打印事务至少初始化一次，建议打印程序首先调用该函数。任务名要尽量区别于其它打印任务。

不希望最终用户更改打印布局时，则设strTaskName空。

#### 关于距离、尺寸取值为字符串类型的说明

字符型时可包含单位名：in(英寸)、cm(厘米)、mm(毫米)、pt(磅)、px(1/96英寸)、%(百分比)，如“10mm”表示10毫米。

#### 设定纸张大小

SET_PRINT_PAGESIZE(intOrient,intPageWidth,intPageHeight,strPageName)

功能：设定打印纸张为固定纸张或自适应内容高，并设定相关大小值或纸张名及打印方向。

- intOrient：number

  打印方向及纸张类型
    1---纵向打印，固定纸张； 
    2---横向打印，固定纸张； 
    3---纵向打印，宽度固定，高度按打印内容的高度自适应(见[样例18](http://www.c-lodop.com/demolist/PrintSample18.html))；
    0---方向不定，由操作者自行选择或按打印机缺省设置。

- intPageWidth：number|string
  纸张宽，数字型时单位为mm，字符型时可包含单位名：in(英寸)、cm(厘米)、mm(毫米)、pt(磅)、px(1/96英寸)，如“10mm”表示10毫米。

- intPageHeight：

  固定纸张时该参数是纸张高；高度自适应时该参数是纸张底边的空白高，计量单位与纸张宽一样。

- strPageName：
  纸张类型名， intPageWidth等于零时本参数才有效，具体名称参见操作系统打印服务属性中的格式定义。
  关键字“CreateCustomPage”会在系统内建立一个名称为“LodopCustomPage”自定义纸张类型。

结果：无

注：PageWidth、PageHeight和strPageName都无效时，本函数对纸张大小不起作用，控件则采用所选打印机的默认纸张，但intOrient仍可起作用。

如果打印程序未采用扩展方式（PRINT_INITA）初始化，本函数的固定纸张功能所定制的纸张大小，会起到PRINT_INITA中Width和Height的相同功能。

**实际打印时，控件按如下优先级顺序确定纸张大小：**

- 第1优先是打印维护里纸张属性（“本机自行定义纸张”）设置的纸张大小。

- 第2优先是SET_PRINT_PAGESIZE指定的纸张大小；

- 第3优先是上次打印时在预览界面设置里选择的纸张类型；

- 第4是按所选打印机的默认纸张；

建议或要求：

如果打印纸张不固定，希望由操作者自主选择纸张时，则不要调用本函数。

#### 设置打印项风格

SET_PRINT_STYLE(strStyleName, varStyleValue)

功能：设置打印项的输出风格，成功执行该函数，此后再增加的打印项按此风格输出。

- strStyleName：打印风格名，风格名称及其含义如下：

  - “FontName”：设定纯文本打印项的字体名称。
  - “FontName”：设定纯文本打印项的字体名称。
  - “FontSize”：设定纯文本打印项的字体大小。
  - “FontColor”：设定纯文本打印项的字体颜色。
  - “Bold”：设定纯文本打印项是否粗体。
  - “Italic”：设定纯文本打印项是否斜体。
  - “Underline”：设定纯文本打印项是否下滑线。
  - “Alignment”：设定纯文本打印项的内容左右靠齐方式。
  - “Angle”：设定纯文本打印项的旋转角度。
  - “ItemType”：设定打印项的基本属性。
  - “HOrient”：设定打印项在纸张内的水平位置锁定方式。
  - “VOrient”：设定打印项在纸张内的垂直位置锁定方式。
  - “PenWidth”：线条宽度。“PenStyle”：线条风格。
  - “Stretch”：图片截取缩放模式。
  - “PreviewOnly”:内容仅仅用来预览。
  - “ReadOnly”:纯文本内容在打印维护时，是否禁止修改。

- varStyleValue：打印风格值，相关值如下：

  - FontName的值：字符型，与操作系统字体名一致，缺省是“宋体”。

  - FontSize的值：数值型，单位是pt，缺省值是9，可以含小数，如13.5。

  - FontColor的值：整数或字符型，整数时是颜色的十进制RGB值；字符时是超文本颜色值，可以是“#”加

    色16进制值组合，也可以是英文颜色名；

  - Bold的值：数字型，1代表粗体，0代表非粗体，缺省值是0。

  - Italic的值：数字型，1代表斜体，0代表非斜体，缺省值是0。

  - Underline的值：数字型，1代表有下划线，0代表无下划线，缺省值是0。

  - Alignment的值：数字型，1—左靠齐 2—居中 3—右靠齐，缺省值是1。

  - Angle的值：数字型，逆时针旋转角度数，单位是度，0度表示不旋转。

  - ItemType的值：数字型，0—普通项 1—页眉页脚 2—页号项 3—页数项 4—多页项

    缺省（不调用本函数时）值0。普通项只打印一次；页眉页脚项则每页都在固定位置重复打印；页号项和页数项是特殊的页眉页脚项，其内容包含当前页号和全部页数；多页项每页都打印，直到把内容打印完毕，打印时在每页上的位置和区域大小固定一样（多页项只对纯文本有效）

    在页号或页数对象的文本中，有两个特殊控制字符：“#”特指“页号”，“&”特指“页数”。

  - HOrient的值：数字型，0—左边距锁定 1—右边距锁定 2—水平方向居中 3—左边距和右边距同时锁定（中间拉伸），缺省值是0。

  - VOrient的值：数字型，0—上边距锁定 1—下边距锁定 2—垂直方向居中 3—上边距和下边距同时锁定（中间拉伸），缺省值是0。

  - PenWidth的值：整数型，单位是(打印)像素，缺省值是1，非实线的线条宽也是0。

  - PenStyle的值：数字型，0—实线 1—破折线 2—点线 3—点划线 4—双点划线  缺省值是0。

  - Stretch的值：数字型，0—截取图片 1—扩展（可变形）缩放 2—按原图长和宽比例（不变形）缩放。缺省值是0。

  - PreviewOnly的值：字符或数字型，1或“true”代表仅预览，否则为正常内容。

  - ReadOnly的值：字符或数字型，1或“true”代表“是”，其它表示“否”，缺省值为“是”，即缺省情况下，纯文本内容在打印维护时是禁止修改的。

  - 结果：无

##### 设定字大小

SET_PRINT_STYLE("FontSize", fontSizeNum)

- fontSizeNum：number，单位pt

##### 设定字体粗细

SET_PRINT_STYLE("Bold", boldNum : number)

#### 设置打印风格



**注意SET_PRINT_STYLEA与SET_PRINT_STYLE的区别。**

LODOP.SET_PRINT_STYLEA(2,"FontName","隶书");

LODOP.SET_PRINT_STYLEA(2,"FontSize",15);

2是姓名栏的增加顺序号,"FontName"和"FontSize"是系统关键字,表示设置字体名和字体大小。

"隶书"是字体名值,同操作系统的字体名，15是字体大小值，单位是pt。

序号设0表示最新对象。

#### 增加超文本项

ADD_PRINT_HTM(Top,Left,intWidth,intHeight,strHtml)

- Top：number|string，number时单位px，所增打印项在纸张内的上边距
- Left：number|string，number时单位px，所增打印项在纸张内的左边距

- intWidth：number|string，number时单位px，打印项的宽度，本参数可以用RightMargin关键字转义为打印区域相对于纸张的“右边距”。

  例如：ADD_PRINT_HTM(Top,Left,**"RightMargin:0mm"**,intHeight,strHtml)

- intHeight：number|string，number时单位px，打印项的高度，本参数可以用BottomMargin关键字转义为打印区域相对于纸张的“下边距”。

- strHtml：string，超文本代码内容，未限制长度。可以是一个完整的页面超文本代码，或者是一个代码段落，也可以是URL:web地址形式的URL地址。

结果：无

**Lodop专有样式和属性有：**

- 代码中若包含**style="page-break-after:always"**或**style="page-break-before:always"**，该元素称为“强制分页元素”，控件会在该元素处分页。

- 代码中的**img**标签如果有**transcolor**属性，则可以实现透明打印图片。例如属性格式为：transcolor=”#FFFFFF”表示用白色作为透明底色，这里的颜色值可以是“#”加三色16进制值组合，也可以是英文颜色名。这个专有属性再配合img的position:absolute可以实现“先字后章”的公章打印效果。

- 代码中的元素如果包含borderthin属性，如果属性值等于true,则该元素的border在合并单元格时会采用单细线模式。

建议或要求：

要求在打印初始化后使用，建议在画线类函数之后调用。注意“强制分页元素”要符合xhtml规范，建议用跨整行的元素，内容不能空，内容可以是“ ”。强制分页符对其它Lodop函数无效，仅适用本函数。

#### 增加纯文本项

ADD_PRINT_TEXT(Top, Left, intWidth, intHeigh, strContent)

功能：增加纯文本打印项，设定该打印项在纸张内的位置和区域大小，文本内容在该区域内自动折行，当内容超出区域高度时，如果对象被设为“多页文档”则会自动分页继续打印，否则内容被截取。

- Top：number|string，number时单位px，所增打印项在纸张内的上边距
- Left：number|string，number时单位px，所增打印项在纸张内的左边距
- intWidth：number|string，number时单位px，打印项的宽度
- intHeight：number|string，number时单位px，打印项的高度
- strContent：string，纯文本内容，未限制长度。

#### 增加表格项

ADD_PRINT_TABLE(Top,Left,intWidth,intHeight,strHtml)

功能：用超文本增加一个表格打印项，设定该表格在每个纸张内的位置和区域大小。打印时只输出首个页面元素table的显示内容，当table内包含thead或tfoot时，一旦表格被分页，则每个打印页都输出表头(thead)或表尾(tfoot)。

- Top：number|string，number时单位px，所增打印项在纸张内的上边距
- Left：number|string，number时单位px，所增打印项在纸张内的左边距

- intWidth：number|string，number时单位px，打印项的宽度
- intHeight：number|string，number时单位px，打印项的高度
- strHtml：string，超文本代码内容，未限制长度。可以是一个完整的页面超文本代码，或者是一个代码段落，也可以是URL:web地址形式的URL地址。

结果：无

特别说明：

本函数能识别的超文本专有元素属性有tdata、format、tclass、tindex等四个，它们主要用来实现分页小计、分类合计等统计功能，**这四个属性可以用在table内的任何元素上**，包含tdata属性的超文本元素下面称为“统计结果元素”，被统计的超文本元素称为“数据元素”，它们的属性值及其含义如下：

- tdata：设置统计类型，其值和含义为：
  - subCount—-本页行数，即本页该数据列的单元格行数；
  - subSum—-本页合计，即本页该数据列的数值合计；
  - subAverage—-本页平均数，即本页合计除以本页行数；
  - Count—-累计行数，即从第一页到本页的该列行数的累加值；
  - Sum—-累计数，即从第一页到本页的该列数值的累加值；
  - Average—-累计平均数，即累计数除以累计行数；
  - allCount—-总行数，即该列全表的行数；
  - allSum—-总计数，即该列全表的数值总和；
  - allAverage—-总平均数，即全表总计数除以总行数；
  - pageNO—-页号，即本table页的序号（与打印纸张的页号不一定相同）；
  - pageCount—-总页数，即全（table）表被分成的总页数；
- format：设置统计结果的显示格式，其值如下样式：
  - “0”、“0.00”、“#.##”、“#,##0.00”、“0.000E+00”“#.###E-0”、“UpperMoney”(大写金额)等等,其中的“#”表示当数据为0时不显示。
- tclass：设置分类统计的“类名”，其值可任意设置，在分类统计时，“统计结果元素”和“数据元素”的tclass值必须一致。
- tindex：一般要求“统计结果元素”的位置与“数据元素”的列位置一致或内含在“数据元素”列内，如果位置无法一致，可以用tindex指定对应的数据列，该值是数字型的列序号，从1起始。
- 占位符：一个要显示统计结果的“统计结果元素”必须要包含占位符，占位符是任意个“#”组成的字符串，占位符可以与其它内容混合在一起，当统计结果值较大时，开发者需要注意占位符要占用足够多的位置，除非占位符周围有合理的空白区，避免统计结果与普通内容重叠。

#### 增加超文本打印项（URL）

ADD_PRINT_URL(Top,Left,Width,Height,strURL)

功能：按URL地址增加超文本打印项，设定该打印项在纸张内的位置和区域大小。

- Top：number|string，number时单位px，所增打印项在纸张内的上边距
- Left：number|string，number时单位px，所增打印项在纸张内的左边距

- intWidth：number|string，number时单位px，打印项的宽度
- intHeight：number|string，number时单位px，打印项的高度
- strURL：string，页面URL地址，未限制长度。

结果：无

#### 增加图片打印项

ADD_PRINT_IMAGE(Top,Left,Width,Height,strHtmlContent)

功能：增加图片打印项，设定该打印项在纸张内的位置和区域大小。

- Top：number|string，number时单位px，所增打印项在纸张内的上边距
- Left：number|string，number时单位px，所增打印项在纸张内的左边距

- intWidth：number|string，number时单位px，打印项的宽度
- intHeight：number|string，number时单位px，打印项的高度
- strHtmlContent：本参数是字符型，有三种情况：一是超文本代码内容；二是本地文件名内容；第三是WEB地址。第一种情况一般是用IMG标签组成的超文本代码段落。第二种情况是本地文件名全路径，格式如“C:/test.jpg”。第三种情况是是URL:web地址形式的URL地址。

结果：无

本函数可用ADD_PRINT_HTM代替，但区别是本函数仅取超文本第一个图片元素，边距是0，而且打印时不会因为设计区域小于图片而被分页，适合与纯文本组合打印的插图。

#### 画图形

ADD_PRINT_SHAPE(intShapeType,intTop,intLeft,intWidth,intHeight,intLineStyle,intLineWidth,intColor)

#### 增加矩形线

ADD_PRINT_RECT(Top, Left, Width, Height,intLineStyle, intLineWidth)

功能：增加矩形线打印项，设定该矩形在纸张内的位置和大小，设定线条的类型和线条宽度。

- Top：number|string，number时单位px，所增打印项在纸张内的上边距
- Left：number|string，number时单位px，所增打印项在纸张内的左边距

- intWidth：number|string，number时单位px，打印项的宽度
- intHeight：number|string，number时单位px，打印项的高度
- intLineStyle：number，默认为0，
  - 线条类型：0—实线  1—破折线  2—点线  3—点划线  4—双点划线
- intLineWidth：number，线条宽，单位是（打印）像素，缺省值是1，非实线的线条宽也是0。

结果：无

#### 增加椭圆线

ADD_PRINT_ELLIPSE(Top, Left,Width, Height, intLineStyle, intLineWidth)

功能：增加椭圆线打印项，设定该椭圆在纸张内的位置和大小，设定线条的类型和线条宽度。

- Top：number|string，number时单位px，该椭圆的外缘矩形在纸张内的上边距
- Left：number|string，number时单位px，该椭圆的外缘矩形在纸张内的左边距

- intWidth：number|string，number时单位px，该椭圆的外缘矩形的宽度
- intHeight：number|string，number时单位px，该椭圆的外缘矩形的高度
- intLineStyle：number，默认为0，
  - 线条类型：0—实线  1—破折线  2—点线  3—点划线  4—双点划线
- intLineWidth：number，线条宽，单位是（打印）像素，缺省值是1，非实线的线条宽也是0。

结果：无

#### 增加直线

ADD_PRINT_LINE(Top1,Left1, Top2, Left2,intLineStyle, intLineWidth)

功能：增加直线，设定直线的两个端点，设定直线的线条类型和线宽。

- Top1：number|string，number时单位px，端点1在纸张内的上边距。

  > 当上边距超过纸张高度时，打印项被输出在下一页(或更下页)。

- Left1：number|string，number时单位px，端点1在纸张内的左边距。

- Top2：number|string，number时单位px，端点2在纸张内的上边距。

  > 当上边距超过纸张高度时，打印项被输出在下一页(或更下页)。

- Left2：number|string，number时单位px，端点2在纸张内的左边距。

- intLineStyle：number，默认为0，

  - 线条类型：0—实线  1—破折线  2—点线  3—点划线  4—双点划线

- intLineWidth：number，线条宽，单位是（打印）像素，缺省值是1，非实线的线条宽也是0。

#### 增加条形码

ADD_PRINT_BARCODE(Top, Left,Width, Height, CodeType, CodeValue)

功能：增加条形码打印项，设定该条形码在纸张内的位置和大小，指定条形码的类型和条码值，控件在打印机上直接绘制条码图。

- Top：number|string，number时单位px，该条码图在纸张内的上边距
- Left：number|string，number时单位px，该条码图在纸张内的左边距

- intWidth：number|string，number时单位px，该条码图的宽度

- intHeight：number|string，number时单位px，该条码图的高度

- CodeType：string

  条码类型，目前支持的类型（条码规制）主要是一维条码，有如下几种：

  128A，128B，128C，EAN8，EAN13，EAN128A，EAN128B，EAN128C，Code39，39Extended，2_5interleaved，2_5industrial，2_5matrix，UPC_A，UPC_E0，UPC_E1，UPCsupp2，UPCsupp5，Code93，93Extended，MSI，PostNet，Codaba，QRCode。

  其中QRCode二维码，其它为一维码。默认情况下QRCode的版本会根据宽度和高度自动调整，页面程序也可以直接设置具体版本（有1、3、7、14四个简约版本可选），版本固定时会按宽度和高度自动缩放条码大小。

- CodeValue：条码值。

结果：无

#### 增加图表

ADD_PRINT_CHART(Top, Left,Width, Height, ChartType, strHtml)

功能：增加图表打印项，设定该图表在纸张内的位置和大小，指定图表的类型和生成图表的数据来源，一般数据来源于一个超文本的Table,本函数可以快速清晰的生成任何复杂的图表。

- Top：number|string，number时单位px，所增打印项在纸张内的上边距
- Left：number|string，number时单位px，所增打印项在纸张内的左边距

- intWidth：number|string，number时单位px，打印项的宽度

- intHeight：number|string，number时单位px，打印项的高度

- ChartType：number，图表类型代码，整数型，目前支持的图表类型有如下几种：

  0—折线图；1—柱状图；2—条形图；3—面积图；4—散点图；5—饼图；

  6—直线图；7—甘特图；8—箭头图；9—气泡图；10—几何图；

- strHtml：

  table的超文本代码，用该table的数据来生成图表，数据结构定义如下几种：

  - 一是“通用table”结构，这种结构的第一行提供图表的Label，第一列提供图表的图例名称，其它行列单元提供图表的Value，多数情况下Label作为X轴数据、Value作为Y轴数据来建立图表，本结构适合前7种图表；

  - 二是“甘特图table”结构，这种结构仅适合甘特图，该Table的第一列是甘特图的阶段名称，可任意起名，第二列是“计划开始时间”，第三列是“计划完成时间”，第四列是“实际开始时间”，第五列是“实际完成时间”。注意第一行第二列和第一行第三列的“名称相同部分”会与第一列的内容组合成图表的“开始阶段的标注”，第一行第四列和第一行第五列的“名称相同部分”会与第一列的内容组合成图表的“实际阶段的标注”。

  - 三是“箭头图table”结构，这种结构仅适合箭头图，该Table的第一列是每个箭头的名称，可任意起名，第二列是“箭头起点X值”，第三列是“箭头起点Y值”，第四列是“箭头终点X值”，第五列是“箭头终点Y值”。

  - 四是“气泡图table”结构，这种结构仅适合气泡图，该Table的第一列是每个气泡的名称，可任意起名，第二列是“气泡圆心X值”，第三列是“气泡圆心Y值”，第四列是“气泡圆的半径值”。

  - 五是“几何图table”结构，这种结构仅适合几何图，该Table的第一列是每个几何图的名称，可任意起名，第二列是“左下角位置X值”，第三列是“左下角位置Y值”，第四列是“几何图的宽”，第五列是“几何图的高”，第六列是“几何图的图形代码”，第七列是“几何图的颜色”，第八列是“几何图是否透明”，1代表透明，0代表不透明。其中图形代码如下：0-矩形；1—圆形；2—竖线；3—横线；4—三角形；5—倒三角形；6—斜线；7—菱形；8—立方体；9—十字线；10—斜十字线；11—米字线；12—三角锥；13—倒三角锥。颜色有RGB值、16进制组合值或英文名三种描述法。

  结果：无

  

#### 强制分页

NEWPAGE()

功能：强制分页。执行该函数之后所增加的内容会在前面内容的首页之后新建一页输出，前面无内容时，仍然从第一页开始。

参数：无

结果：返回逻辑值

返回逻辑真表示强制分页成功，逻辑假表示强制分页失败。

建议或要求：

打印初始化后调用本函数。

#### 打印预览

PREVIEW()

功能：打印预览输出页。

参数：无

结果：显示打印预览界面。如果预览界面没有被嵌入页面中，而是被弹出窗口，那么关闭窗口时会返回数字结果值，该数字大于0时表示被实际打印的次数。

#### 直接打印

PRINT()

功能：不经打印预览的直接打印。

参数：无

结果：打印机开始实际打印，返回逻辑结果，正确打印时返回真，打印出错时返回假。

#### 打印维护，调整

PRINT_SETUP()

功能：对整页的打印布局和打印风格进行界面维护，它与打印设计的区别是不具有打印项增删、增加打印项属性功能，目标使用者是最终用户。

参数：无

结果：显示打印维护界面。如界界面没有被嵌入页面中，而是被弹出窗口，那么关闭窗口时会返回数字结果值，该数字大于0时表示被实际打印的次数，这里的实打次数包括打印维护界面“打印”按钮的直接打印和“预览”按钮进入预览后的打印。

#### 打印设计

PRINT_DESIGN()

功能：对整页的打印布局和打印风格进行界面设计，它与打印维护的区别是具有打印项增删、增加打印项属性功能，目标使用者是软件开发者。

参数：无

结果：显示打印设计界面，设计完毕关闭窗口后，返回生成的程序代码。

**打印维护** 和 **打印设计** 有点类似，二者的区别是功能权限不同，后者是开发人员用的，前者可根据实际情况提供给最终用户。

**打印维护** 和 **打印设计**中按住SHIFT用鼠标可多选目标。

在**打印设计**时， 按住CTRL再拖拽选中的对象，可实现快速复制。 

#### 装载文档式模板

ADD_PRINT_ DATA (strDataStyle, varDataValue)

**功能：**直接装载文档式模板。 Lodop传统模板可以称为“JS语句组式模板”， 传统模板需要JS的eval方法来装载，“文档式模板”不再依赖该方法，直接用Lodop的语句ADD_PRINT_DATA就可装载复用

- strDataStyle：字符型，如下是类型名及其含义：

  **ProgramData**----加载文档式模板关键字。

varDataValue：文档式模板内容。文档式模板内容是特殊格式的BASE64字符，符合该编码集范畴，容易保存和网络传递

**结果**：无

**建议或要求：**

要求在初始化之后，打印或预览之前调用。

文档式模板使用相关语句信息：

1. 获得模板信息，语句如下：

   GET_VALUE (ValueType, ValueIndex);

   ValueType：获得文档式模板关键字是“ProgramData”; 
   ValueIndex：0-全部模板，1-不包含初始化语句的模板。（详细解释参考基本函数GET_VALUE）

2. 文档式模板执行后，给打印对象赋值，语句如下： 

   SET_PRINT_STYLEA(varItemNameID, strStyleName,varStyleValue);
   strStyleName：关键字参数值是“CONTENT”（详细解释参考基本函数SET_PRINT_STYLEA）。

**举例：**LODOP.ADD_PRINT_ DATA (**ProgramData**," @J0yHEH1QG1IBIS0APy …... GHIBES0APt==");//

(省略…..).

#### 获得打印设备个数

GET_PRINTER_COUNT()

功能：获得操作系统内打印设备的个数。

参数：无

结果：返回数字

返回数字结果表示操作系统内的打印设备个数，0表示失败或无打印设备。

#### 获得打印设备名称

GET_PRINTER_NAME(intPrinterNO)

功能：按打印设备序号获得其名称。

- intPrinterNO：number，打印设备序号，序号从0开始，最大序号是GET_PRINTER_COUNT()减1。

结果：返回字符

返回字符结果表示操作系统内的打印设备的名称，空表示失败或无该设备。

#### 指定打印设备

SET_PRINTER_INDEX(oIndexOrName)

功能：按名称或序号指定要进行打印输出的设备，指定后禁止重新选择。

- oIndexOrName：number|string

  打印机名称或序号，数字表示打印机的序号，从0开始，最大序号是GET_PRINTER_COUNT()减1。-1特指操作系统内设定的默认打印机。

  字符代表打印机的名称，与操作系统内的打印机名称一致。

注：用本函数指定打印机后，在预览界面不允许重新选择打印机，而用另外一个函数**SET_PRINTER_INDEXA**指定后则允许重新选择。

结果：返回逻辑值

返回逻辑真表示指定成功，逻辑假表示指定失败，失败原因有：该打印设备不存在。

#### 选择打印设备

SELECT_PRINTER()

功能：弹出界面选定某打印设备为固定输出设备。

参数：无

结果：返回数字

返回数字结果表示选定的设备序号，返回-1表示放弃选择，没有任何动作。

#### 设置显示模式

SET_SHOW_MODE (strModeType,varModeValue)

功能：设置打印预览、打印维护和打印设计的显示模式，设置打印预览时是否包含背景图等。

- strModeType：string，显示模式的名称，如下是类型名及其含义：
  - “PREVIEW_IN_BROWSE”：打印预览界面是否内嵌到网页内部。
  - “SETUP_IN_BROWSE”： 打印维护界面是否内嵌到网页内部。
  - “DESIGN_IN_BROWSE”：打印设计界面是否内嵌到网页内部。
  - “BKIMG_IN_PREVIEW”：打印预览时是否包含背景图。
  - “BKIMG_IN_FIRSTPAGE”：打印预览时是否仅首页包含背景图。
  - “SETUP_ENABLESS”：打印维护界面工具显示控制（权限控制字串）
  - “SKIN_TYPE”：界面皮肤类型
  - “SKIN_CUSTOM_COLOR”：界面自定义皮肤颜色
  - “HIDE_PBUTTIN_PREVIEW”：隐藏预览窗口的打印按钮
  - “HIDE_SBUTTIN_PREVIEW”：隐藏预览窗口的打印设置按钮
  - “HIDE_QBUTTIN_PREVIEW”：隐藏预览窗口的关闭按钮
  - “HIDE_PBUTTIN_SETUP”：隐藏打印维护窗口的打印按钮
  - “HIDE_VBUTTIN_SETUP”：隐藏打印维护窗口的预览按钮
  - “HIDE_ABUTTIN_SETUP”：隐藏打印维护窗口的应用按钮
  - “HIDE_RBUTTIN_SETUP”：隐藏打印维护窗口的复原按钮
  - “MESSAGE_GETING_URL”：URL对象下载时的提示信息
  - “MESSAGE_PARSING_URL”：URL对象解析时的提示信息
  - “MESSAGE_PARSING_HTM”：HTM对象解析时的提示信息
  - “MESSAGE_NOSET_PROPERTY”：打印维护界面企图进入属性设置的警示信息
  - “HIDE_PAPER_BOARD”：隐藏打印预览背景进纸版的图案
  - “LANDSCAPE_DEFROTATED”：横向打印的预览默认旋转90度（正向显示）
  - “BKIMG_LEFT”：设置背景图位置X值
  - “BKIMG_TOP”：设置背景图位置Y值
  - “BKIMG_WIDTH”：设置背景图宽度
  - “BKIMG_HEIGHT”：设置背景图高度
  - “HIDE_PAGE_PERCENT”：隐藏整页缩放(百分比)的下拉选择框
  - “LANGUAGE”：设置界面文字的语言

- varModeValue：number|string，显示模式的值，整数或字符型，相关值如下：

  - PREVIEW_IN_BROWSE的值：整数或字符型，1或“1”或“True”=是,否则不是。

  - SETUP_IN_BROWSE的值：整数或字符型，1或“1”或“True”=是,否则不是。

  - DESIGN_IN_BROWSE的值：整数或字符型，1或“1”或“True”=是,否则不是。

  - BKIMG_IN_PREVIEW的值：整数或字符型，1或“1”或“True”=是,否则不是。

  - BKIMG_IN_FIRSTPAGE的值：整数或字符型，1或“1”或“True”=是,否则不是。

  - SETUP_ENABLESS的值：字符型，由“1”和“0”组成的字符串，最多14个字符，

  - 按如下顺序控制打印维护的界面功能，“1”-允许，“0”-禁止：

    位置移动和宽高调整1+颜色选择2+字体名选择3+字大小选择4+旋角调整5+粗斜体功能条6+线型功能条7+对齐功能条8+删除功能9+页眉设置10+页脚设置11+位置锁定功能12+属性设置13+显示关闭钮（界面内嵌时）14**

    缺省的SETUP_ENABLES值：“11111111000001”

    例如：如想允许操作者“删除”对象，可以执行如下语句;

    LODOP.SET_SHOW_MODE(“SETUP_ENABLES”,”11111111100001”);

  - SKIN_TYPE的值：数字型，固定皮肤如下：

    0—银灰色(缺省)；1—经典绿; 2—熏衣草紫；3—淡钢青；4—茶色棕；5—茶色棕；6—麦色；7—紫罗兰；8—天蓝；9—镀银；10—沙滩棕；11—鲜肉色；12—粉末蓝；13—钒矿色；14—浅绿；15—浅蓝；16—卡其布； 17—秋麒麟；18—深海绿；19—深卡其布；20—番茄桔

  - SKIN_CUSTOM_COLOR的值：整数或字符型，整数时是颜色的十进制RGB值；字符时是超文本颜色值，可以是“#”加三色16进制值组合，也可以是英文颜色名。
  - HIDE_PBUTTIN_PREVIEW的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - HIDE_SBUTTIN_PREVIEW的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - HIDE_QBUTTIN_PREVIEW的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - HIDE_PBUTTIN_SETUP的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - HIDE_VBUTTIN_SETUP的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - HIDE_ABUTTIN_SETUP的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - HIDE_RBUTTIN_SETUP的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - MESSAGE_GETING_URL的值：字符型，默认值是“正打开页面下载数据(限时5分钟)…”。
  - MESSAGE_PARSING_URL的值：字符型，默认值是“下载结束,正在准备打印数据…”。
  - MESSAGE_PARSING_HTM的值：字符型，默认值是空（不提示信息），当超文本内容较多，有明显等待时间时，建议设置该值。**
  - MESSAGE_NOSET_PROPERTY的值：字符型，默认值是“只有在设计模式下才能设置属性…”。
  - HIDE_PAPER_BOARD的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - LANDSCAPE_DEFROTATED的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - BKIMG_LEFT的值：整数或字符型。整数的单位是PX，字符时可以包含具体计量单位。
  - BKIMG_TOP的值：同上；
  - BKIMG_WIDTH的值：同上；
  - BKIMG_HEIGHT的值：同上；
  - HIDE_PAGE_PERCENT的值：整数或字符型，1或“1”或“True”=是,否则不是。
  - LANGUAGE的值：数字，0-简体中文 1-英文 2-繁体 3-BIG5(繁体)

结果：返回逻辑结果，成功时返回真，失败时返回假。

建议或要求：

初始化之后，进入功能（打印预览、打印维护或打印设计）界面前调用本函数。

#### 设置打印模式

SET_PRINT_MODE (strModeType,varModeValue)

功能：设置人工双面打印模式等。

- strModeType：string，模式类型名，如下是类型名及其含义：
  - “DOUBLE_SIDED_PRINT”：设置是否人工双面打印。
  - “PRINT_START_PAGE”：指定要打印的起始页。
  - “PRINT_END_PAGE”：指定要打印的截止页。
  - “PRINT_PAGE_PERCENT”：指定整页缩放打印的比例。
  - “AUTO_CLOSE_PREWINDOW”：设置打印完毕是否自动关闭预览窗口。
  - “PRINT_SETUP_PROGRAM”：设置打印维护窗口关闭后是否返回程序代码。
  - “NOCLEAR_AFTER_PRINT”：设置打印或预览后内容不清空是否为真。
  - “CATCH_PRINT_STATUS”：设置是否进行对后台服务的打印状态进行捕获。

- varModeValue：number|string，模式类型值，相关值如下：

  - DOUBLE_SIDED_PRINT的值：整数或字符型，1或“1”或“True”=是,否则不是。

  - PRINT_START_PAGE的值：整数，不设置本参数时，控件默认从1开始打印。适用打印部分页时。

  - PRINT_END_PAGE的值：整数，不设置本参数时，控件默认打印到最后页。适用打印部分页时。

  - PRINT_PAGE_PERCENT的值：字符型，具体值有如下几种：

    “Full-Width” –宽度按纸张的整宽缩放；

    “Full-Height”–高度按纸张的整高缩放：

    “Full-Page” –按整页缩放，也就是既按整宽又按整高缩放；

    此外还可以按具体百分比例，格式为“Width:XX%;Height:XX%”或“XX%”

    比值范围是5%-800%,也就是最大缩小到原来的5%，最大放大8倍。

  - AUTO_CLOSE_PREWINDOW的值：整数或字符型，1或“1”或“True”=是,否则不是。

  - PRINT_SETUP_PROGRAM的值：整数或字符型，1或“1”或“True”=是,否则不是，打印维护窗口关闭后如果不返回程序代码，则返回打印按钮被点击的次数。

  - NOCLEAR_AFTER_PRINT的值：整数或字符型，1或“1”或“True”=是,否则不是，默认值是“否”，也就是说，默认情况下打印或预览后会清空所有内容。

  - CATCH_PRINT_STATUS的值：整数或字符型，1或“1”或“True”=是,否则不是，默认值是“否”，也就是说，默认情况下打印时不对打印状态进行捕获，该捕获动作会针对每个打印机开启一个监控线程，对页面性能有少许影响，开启后用GET_VALUE获得状态值。

  结果：返回逻辑结果，成功时返回真，失败时返回假。

#### 设置预览窗口

SET_PREVIEW_WINDOW(intDispMode, intToolMode,blDirectPrint,inWidth,intHeight, strTitleButtonCaptoin)

功能：设置预览窗口的显示模式和大小。

- intDispMode预览比例，数字型，0—适高1—正常大小2—适宽。

- intToolMode工具条和按钮，数字型 0—显示工具条1—显示按钮 2—两个都显示 3—两个都不显示

- blDirectPrint打印按钮是否“直接打印” 1-是 0-否（弹出界面“选机打印”）

- inWidth 窗口宽，整数型，单位是px

- intHeight 窗口高，整数型，单位是px

当inWidth或intHeight小于等于0时窗口最大化。

- strTitleButtonCaptoin：预览窗口和打印按钮的名称组合，字符型，用“点”分隔，譬如“预览查看.开始打印”，表示预览窗口的标题是“预览查看”，按钮名是“开始打印”。

结果：无

#### 指定背景图

ADD_PRINT_SETUP_BKIMG(strImgHtml)

功能：用程序方式指定打印维护或打印设计的背景图。

- strImgHtml：本参数是字符型，有两种情况：一是超文本代码内容；二是本地文件名内容。第一种情况一般是用IMG标签组成的超文本代码段落。第二种情况是本地文件名全路径，格式如“C:/test.jpg”，图片文件可以是jpg、jpeg、bmp、gif、ico、png、emf等格式。

结果：无

建议或要求：

初始化之后调用。

#### 发送原始数据

SEND_PRINT_RAWDATA(strRawData)

功能：向打印机发送原始数据或指令。

- strRawData：数据或指令值，字符型，未限制长度。

结果：返回逻辑结果，发送成功时返回真，发送失败时返回假。

#### 写端口数据

WRITE_PORT_DATA(strPortName,strData)

功能：直接向端口写数据或指令。

- strPortName：端口名，同操作系统的端口名，名称如下：LPT1、LPT2、LPT3、COM1、COM2、COM3…

- strData：数据或指令值，字符型，未限制长度。

  当设置端口通讯参数时strData格式如下：

  mode com1：波特率,校验,数据位,停止位,读时限,写时限

  其中mode为固定关键字，com1要和strPortName保持一直。

  校验值有：N(noparity)O(oddparity) E(evenparity)
  M(markparity)S(spaceparity)

  读时限和写时限的时间单位为毫秒，举例如下：

  WRITE_PORT_DATA(“com1”,“mode com1:2400,n,8,1”)

  或WRITE_PORT_DATA(“com2”,“mode com2:2400,n,7,2,5000,2000”)

结果：返回逻辑结果，发送成功时返回真，发送失败时返回假。

#### 读端口数据

READ_PORT_DATA(strPortName)

功能：直接从端口读数据。

- strPortName：端口名，同操作系统的端口名，名称如下：LPT1、LPT2、LPT3、COM1、COM2、COM3…

结果：返回字符数据。

#### 获得配置文件名

GET_PRINT_INIFFNAME (strPrintTask)

功能：获得某打印任务的本地配置文件全路径名。

- strPrintTask：打印任务名，字符型，即初始化时所设的任务名。

结果：返回字符

返回字符结果表示本地配置文件全路径名（并非文件内容），空表示失败。

#### 获得纸张类型名清单

GET_PAGESIZES_LIST(oPrinterName,strSplit)

功能：获得某个打印机所支持的纸张类型名清单，返回一个用分隔符链接的长字符串。

- oPrinterName：打印机名称或序号，字符型或数字，序号从0开始，-1代表默认打印机。

- strSplit：分隔符，字符型，例如可以用“\n”代表换行控制符来分隔。

结果：返回字符串。

#### 写本地文件内容

WRITE_FILE_TEXT(intWriteMode,strFileName, strText)

功能：向本地文件写入文本内容。

- intWriteMode：写入模式，数字型，0—文件覆盖模式 1—文件尾追加模式 2—文件首插入模式。

- strFileName：本地文件名，字符型，文件名包含全路径。
- strText：写入的文本内容，字符型。

结果：调用函数后控件启动安全提示，等待操作许可。

返回字符值表示写入情况：

- “ok”-写入成功

- “file not exist”-文件不存在

- “do nothing”-未写入，一般原因有：操作者禁止读写、文件只读属性等。

- 写入时如果文件不存在则自动新建。

#### 读本地文件内容

GET_FILE_TEXT(strFileName)

功能：读本地文件文本内容。

- strFileName：本地文件名，字符型，含全路径。

结果：调用函数后控件启动安全提示，等待操作许可。

返回字符值，文本内容。

返回空原因：文件不存在；内容真实空；操作者禁止读写。

