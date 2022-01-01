#### 文件结构

YAML 文件可以由一或多个文档组成（也即相对独立的组织结构组成），**文档间使用“---”（三个横线）在每文档开始作为分隔符。同时，文档也可以使用“...”（三个点号）作为结束符（可选）**。

**如果只是单个文档，分隔符“---”可省略。**

每个文档并不需要使用结束符“...”来表示结束，但是对于网络传输或者流来说，作为明确结束的符号，有利于软件处理。（例如不需要知道流关闭就能知道文档结束） 

#### 语法规则和数据结构

以 [JS-YAML](https://github.com/nodeca/js-yaml) 的实现为例。你可以去[在线 Demo](https://nodeca.github.io/js-yaml/) 验证下面的例子（可校验YAML）。

基本语法规则如下。

> - 大小写敏感
> - 使用缩进表示层级关系
> - **缩进时不允许使用Tab键，只允许使用空格**。
> - 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
> - key: value  冒号后面有一个空格

`#` 表示注释，从这个字符一直到行尾，都会被解析器忽略。

YAML 支持的数据结构有三种。

> - 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）
> - 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）
> - 纯量（scalars）：单个的、不可再分的值

#### 对象（键值表）

对象的一组键值对，使用冒号结构表示。

> ```yaml
> animal: pets
> ```

转为 JavaScript 如下。

> ```javascript
> { animal: 'pets' }
> ```

Yaml 也允许另一种写法，将所有键值对写成一个行内对象。

> ```yaml
> hash: { name: Steve, foo: bar } 
> ```

转为 JavaScript 如下。

> ```javascript
> { hash: { name: 'Steve', foo: 'bar' } }
> ```

#### 数组（列表）

一组连词线开头的行，构成一个数组。

> ```yaml
> - Cat
> - Dog
> - Goldfish
> ```

转为 JavaScript 如下。

> ```javascript
> [ 'Cat', 'Dog', 'Goldfish' ]
> ```

数据结构的子成员是一个数组，则可以在该项下面缩进一个空格。

> ```yaml
> -
>  - Cat
>  - Dog
>  - Goldfish
> ```

转为 JavaScript 如下。

> ```javascript
> [ [ 'Cat', 'Dog', 'Goldfish' ] ]
> ```

数组也可以采用行内表示法。

> ```yaml
> animal: [Cat, Dog]
> ```

转为 JavaScript 如下。

> ```javascript
> { animal: [ 'Cat', 'Dog' ] }
> ```

#### 复合结构

对象和数组可以结合使用，形成复合结构。

> ```yaml
> languages:
>  - Ruby
>  - Perl
>  - Python 
> websites:
>  YAML: yaml.org 
>  Ruby: ruby-lang.org 
>  Python: python.org 
>  Perl: use.perl.org 
> ```

转为 JavaScript 如下。

> ```javascript
> { languages: [ 'Ruby', 'Perl', 'Python' ],
>   websites: 
>    { YAML: 'yaml.org',
>      Ruby: 'ruby-lang.org',
>      Python: 'python.org',
>      Perl: 'use.perl.org' } }
> ```

#### 纯量

纯量是最基本的、不可再分的值。以下数据类型都属于 JavaScript 的纯量。

> - 字符串
> - 布尔值
> - 整数
> - 浮点数
> - Null
> - 时间
> - 日期

数值直接以字面量的形式表示。

> ```yaml
> number: 12.30
> ```

转为 JavaScript 如下。

> ```javascript
> { number: 12.30 }
> ```

布尔值用`true`和`false`表示。

> ```yaml
> isSet: true
> ```

转为 JavaScript 如下。

> ```javascript
> { isSet: true }
> ```

`null`用`~`表示。

> ```yaml
> parent: ~ 
> ```

转为 JavaScript 如下。

> ```javascript
> { parent: null }
> ```

时间采用 ISO8601 格式。

> ```yaml
> iso8601: 2001-12-14t21:59:43.10-05:00 
> ```

转为 JavaScript 如下。

> ```javascript
> { iso8601: new Date('2001-12-14t21:59:43.10-05:00') }
> ```

日期采用复合 iso8601 格式的年、月、日表示。

> ```yaml
> date: 1976-07-31
> ```

转为 JavaScript 如下。

> ```javascript
> { date: new Date('1976-07-31') }
> ```

#### 转换类型

双叹号是内置类型

```
#下面是内置类型
!!int               # 整数类型
!!float             # 浮点类型
!!bool              # 布尔类型
!!str               # 字符串类型
!!binary            # 也是字符串类型
!!timestamp         # 日期时间类型
!!null              # 空值
!!set               # Set 
!!omap              # 键值列表 
!!pairs             # 对象列表 
!!seq               # 序列，也是列表
!!map               # Map
```

> ```javascript
> e: !!str 123
> f: !!str true
> ```

转为 JavaScript 如下。

> ```javascript
> { e: '123', f: 'true' }
> ```

#### 字符串

字符串是最常见，也是最复杂的一种数据类型。

字符串默认不使用引号表示。

> ```yaml
> str: 这是一行字符串
> ```

转为 JavaScript 如下。

> ```javascript
> { str: '这是一行字符串' }
> ```

如果字符串之中包含空格或特殊字符，需要放在引号之中。

> ```javascript
> str: '内容： 字符串'
> ```

转为 JavaScript 如下。

> ```javascript
> { str: '内容: 字符串' }
> ```

单引号和双引号都可以使用，单引号会对特殊字符转义。

> ```javascript
> s1: '内容\n字符串'
> s2: "内容\n字符串"
> ```

转为 JavaScript 如下。

> ```javascript
> { s1: '内容\\n字符串', s2: '内容\n字符串' }
> ```

单引号之中如果还有单引号，必须连续使用两个单引号转义。

> ```javascript
> str: 'labor''s day' 
> ```

转为 JavaScript 如下。

> ```javascript
> { str: 'labor\'s day' }
> ```

字符串可以写成多行，从第二行开始，必须有一个单空格缩进。换行符会被转为空格。

```yaml
str: 这是一段
 多行
 字符串
```

多行字符串可以使用`|`保留换行符，使用`>`折叠换行（连接后再换行）。

#### | > + -

使用 “|” 和文本内容缩进表示的块：保留块中已有的换行符，相当于段落块

使用 “>” 和文本内容缩进表示的块：将块中回车替换为空格，最终连接成一行，并在行的最后添加换行符

```yaml
this: |
 Foo
 Bar
that: >
 Foo
 Bar
```

转为 JavaScript 代码如下。

> ```javascript
> { this: 'Foo\nBar\n', that: 'Foo Bar\n' }
> ```

使用定界符双引号、单引号或回车表示的块：最终表示成一行

```yaml
# 使用回车的多行，最终连接成一行。
a:     
   JSON的语法其实是YAML的子集，
   大部分的JSON文件都可以被YAML的解释器解释。

# 使用了双引号，双引号的好处是可以转义，即在里面可以使用特殊符号
b:      
   "JSON的语法其实是YAML的子集，
   大部分的JSON文件都可以被YAML的解释器解释。"
```

转为 JavaScript 代码如下

```js
{ a: 'JSON的语法其实是YAML的子集， 大部分的JSON文件都可以被YAML的解释器解释。',
  b: 'JSON的语法其实是YAML的子集， 大部分的JSON文件都可以被YAML的解释器解释。' }
```

`+`表示保留文字块末尾的换行，`-`表示删除字符串末尾的换行。

```yaml
s1: |
 Foo

s2: |+
 Foo


s3: |-
 Foo
```

转为 JavaScript 代码如下。

> ```javascript
> { s1: 'Foo\n', s2: 'Foo\n\n\n', s3: 'Foo' }
> ```

字符串之中可以插入 HTML 标记。

> ```yaml
> message: |
> 
>   <p style="color: red">
>     段落
>   </p>
> ```

转为 JavaScript 如下。

> ```javascript
> { message: '\n<p style="color: red">\n  段落\n</p>\n' }
> ```

#### 引用

锚点`&`和别名`*`，可以用来引用。

> ```yaml
> defaults: &defaults
>   adapter:  postgres
>   host:     localhost
> 
> development:
>   database: myapp_development
>   <<: *defaults
> ```

等同于下面的代码。

> ```yaml
> defaults:
>   adapter:  postgres
>   host:     localhost
> 
> development:
>   database: myapp_development
>   adapter:  postgres
>   host:     localhost
> ```

`&`用来建立锚点（`defaults`），`<<`表示合并到当前数据，`*`用来引用锚点。

下面是另一个例子。

> ```yaml
> - &showell Steve 
> - Clark 
> - Brian 
> - Oren 
> - *showell 
> ```

转为 JavaScript 代码如下。

> ```javascript
> [ 'Steve', 'Clark', 'Brian', 'Oren', 'Steve' ]
> ```



#### JS-YAML

##### 函数和正则表达式的转换

这是 [JS-YAML](https://github.com/nodeca/js-yaml) 库特有的功能，可以把函数和正则表达式转为字符串。

> ```yaml
> # example.yml
> fn: function () { return 1 }
> reg: /test/
> ```

解析上面的 yml 文件的代码如下。

> ```javascript
> var yaml = require('js-yaml');
> var fs   = require('fs');
> 
> try {
>   var doc = yaml.load(
>     fs.readFileSync('./example.yml', 'utf8')
>   );
>   console.log(doc);
> } catch (e) {
>   console.log(e);
> }
> ```

从 JavaScript 对象还原到 yaml 文件的代码如下。

> ```javascript
> var yaml = require('js-yaml');
> var fs   = require('fs');
> 
> var obj = {
>   	fn: function () { return 1 },
>   	reg: /test/
> };
> 
> try {
>   fs.writeFileSync(
>      './example.yml',
>      yaml.dump(obj),
>      'utf8'
>   );
> } catch (e) {
>   	console.log(e);
> }
> ```