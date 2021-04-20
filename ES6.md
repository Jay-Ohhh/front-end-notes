#### var、let、const

如果不用var、let、const声明变量，则会声明为全局变量，在浏览器环境下，相当于在window上增加一个属性。

##### var

如果在方法中声明，则为局部变量（local variable）；如果是在全局域中声明，则为全局变量。

var不支持块级作用域。

##### let

通过 `let` 声明的变量直到它们的定义被执行时才初始化。在初始化之前，该变量处在一个自块顶部到初始化处理的“暂存死区”中（temporal dead zone，简称 TDZ）。

不能在同一作用域下用 let 重复声明变量。

> let会不会变量提升，官方是没有明确说明的，也许底层机制上是提升了，但是形式是没有提升。无需纠结。

##### const

const和let唯一不同的是，const声明的时候必须进行初始化赋值，而且只能赋值一次。

若存储的值不需要变化则建议用const，因为const常量是不变的，不需要引擎实时监控，效率更高。

#### 变量提升和函数提升

ES6新增了块级作用域。块级作用域由 { } 包括，if 语句和 for 语句里面的{ }也属于块作用域。

变量提升是针对**var**而言。

var不支持块级作用域。

class中不能存在变量提升和函数提升。

```js
if(true){
  var a = 1
}
console.log(a) // 1
```

##### 变量提升

变量提升就是变量声明会被提升到作用域的最顶层。

```js
console.log(a) // undefined
var a = 1
console.log(a) // 1
```

等价于

```js
var a
console.log(a)
a = 1
console.log(a)
```

**eg1**

```js
// 全局作用域
var a = 10 // 全局变量，函数内部也能访问
function fn(){
	console.log(a) // undefined
	var a = 20; // 在函数作用域中变量提升
	console.log(a) // 20
}
fn()
console.log(a) // 10
```

等价于

```js
// 全局作用域
var a = 10 // 全局变量，函数内部也能访问
function fn(){
	var a 
	console.log(a) // undefined
	a = 20  // 在函数作用域中变量提升
	console.log(a) // 20
}
fn()
console.log(a) // 10
```

**eg2**

```js
// 全局作用域
var a = 10 // 全局变量，函数内部也能访问
function fn(){
	console.log(a) // 10
	a = 20; // 全局变量a重新赋值
	console.log(a) // 20
}
fn()
console.log(a) // 10
```

**eg3**

var不支持块级作用域，所以在全局作用域重复声明了两次，第二次声明会被忽略，仅用于赋值。

```js
// 全局作用域
var a = 1;
if(true){
	console.log(a); // 1
	var a = 2;
	console.log(a); // 2
}
console.log(a); // 2
```

##### 函数提升

函数声明式，会将函数的声明和定义一起提升到作用域的最顶上去。

```js
fn() // 1，此处可以正常调用
function fn(){
	console.log(1)
}
```

函数表达式

```js
console.log(fn) // undefined
var fn = function(){
	console.log(1)
}
console.log(fn) // function(){//...}
```

等价于

```js
var fn
console.log(fn) // undefined
fn = function(){
	console.log(1)
}
console.log(fn) // function fn(){}
```

如果函数声明和变量声明使用的是同一变量名称，函数声明的优先级高于变量声明。

```js
console.log(fn) // function fn(){}
function(){}
var fn = 1 // 第二次声明会被忽略，仅用于赋值
console.log(fn) // 1
```

##### 总结

1、所有的声明都会提升到作用域的最顶上去。

2、同一个变量只会声明一次，其他的会被忽略掉。

3、函数声明的优先级高于变量申明的优先级。

#### this指向

> 对象没有this，this是在函数里指向对象的。

1、非严格模式，普通函数的this指向全局对象，普通函数是在全局对象里的函数，调用普通函数，其实就是全局对象在调用。

严格模式下的全局作用域中普通函数的this是undefined。

不管是不是严格模式，全局作用域的this都是指向全局对象。全局作用域实际上是在一个全局函数内的作用域。

2、对于方法调用：谁调用方法，this指向谁（方法是写在对象里的函数）

3、构造函数里的this指向它的实例对象。

4、类的constructor的this指向实例对象，类的方法的this指向这个方法的调用者。

5、闭包内部函数和外层函数的this，决定于代码执行时，它们的函数体被引用在哪个作用域（函数体的地址被哪个作用域的变量所引用），即this会指向该函数运行时所在的环境，普通函数和方法被调用时亦如此。

**eg1**

fun访问的是对象obj的b属性，即var fun=function(){console.log(this.a)}。this值要等到代码执行的时候才能确定，上述代码执行时，函数体被引用在全局作用域。

```js
// 全局作用域
var obj={
    a:1,
    b:function(){console.log(this.a)}
}
var fun=obj.b
fun() // undefined
```

**eg2**

作为方法调用时，函数体是在obj里被引用。

```js
// 全局作用域
var obj={
    a:1,
    b:function(){console.log(this.a)}
}
obj.b()  // 打印1
```

6、定时器setTimeout(function(){})里回调函数的this指向window，这是因为function( ){}调用的代码运行在window上，而不是setTimeout所在的执行环境里。

如果定时器setTimeout(( )=>{})里是箭头函数，则箭头函数里的this指向setTimeout的外层函数的this所指向的对象。那么如何找这个setTimeout的外层函数呢？可以找( ){ }这样的结构就可以了

即使是在严格模式下，setTimeout()的function里面的this仍然默认指向window对象， 并不是undefined。

7、箭头函数本身是没有this的，它的this是根据外层函数的this来决定的，它的外层函数可以这样找，向上找到( ){ }这样的函数体结构。

8、嵌套函数的this指向，嵌套的函数不会从调用它的函数中继承this。

（1）如果嵌套函数作为方法调用，其this的值指向调用它的对象。

（2）如果嵌套函数作为函数调用（只要不是方法调用，例如在方法内作为函数调用、在函数内作为函数调用），其this值不是全局对象（非严格模式下）就是undefined（严格模式下）。为什么指向全局对象或者undefined？因为this是指向函数所在的对象，this不能指向函数。

将嵌套函数作为方法调用：

1、例如使用call或apply：

```js
f.call(o); // f是个嵌套函数
```

这句相当于在o对象上调用 f 方法。

2、另外一种方法是将函数设置为某个对象的属性，使其成为该对象的方法，例如：

```js
var o = {};
function outer() {
    var inner = function() {
        console.log(this === o);
    };
    o.m = inner;
}
outer();
o.m(); // true
```



#### 闭包

##### 定义

**「函数」和「函数内部能访问到的变量」（也叫环境）的总和，就是一个闭包。**

eg

```js
function foo(){
  var local = 1
 	return function (){
    console.log(local)
  }
}
// or
function foo(local){
	return function (){
    console.log(local)
  }
}
```

##### 作用

闭包可以用来封装一个需要持久保存的变量，也可以模拟命名空间，避免变量名的污染。

##### 为什么要函数套函数呢？（不是必须的）

是因为需要局部变量，所以才把 local 放在一个函数里，如果不把 local 放在一个函数里，local 就是一个全局变量了，达不到使用闭包的目的——隐藏变量。

所以函数套函数只是为了造出一个局部变量，跟闭包无关。

##### 为什么要return函数呢？

```js
function foo(){
  var local = 1
  function bar(){
    local++
    return local
  }
  return bar
}
```

因为如果不 return，你就无法使用这个闭包。把 return bar 改成 obj.bar = bar 也是一样的，只要让外面可以访问到这个 bar 函数就行了。

所以 return bar 只是为了 bar 能被使用，也跟闭包无关。

##### 闭包会造成内存泄漏？

内存泄露是指用不到（访问不到）的变量，依然占居着内存空间，不能被再次利用起来。

存泄漏在于“浪费”二字，其实闭包会不会发生内存泄漏主要是看你以后会不会对这个闭包里的变量继续进行引用，如果你以后继续引用，这个闭包里的变量跟定义在全局作用域里的作用差不多，只是为了封装成私有不被污染。

如果你以后不再引用则造成内存浪费，需要手动null释放对闭包的引用，即将引用（指向）包含闭包的函数的变量赋值为null。

```js
function foo(){
  var local = 1
  function bar(){
    local++
    return local
  }
  return bar
}
let a = foo()
// ...
a = null // 不再引用时
```

之所以有闭包会造成内存泄漏的谣言，是因为在IE中因为循环引用可能会导致内存泄露。

#### 立即执行函数

```js
(function foo(){console.log("Hello World!")}())//用括号把整个表达式包起来,正常执行
(function foo(){console.log("Hello World!")})()//用括号把函数包起来，正常执行
!function foo(){console.log("Hello World!")}()//使用！，求反，这里只想通过语法检查。
+function foo(){console.log("Hello World!")}()//使用+，正常执行
-function foo(){console.log("Hello World!")}()//使用-，正常执行
~function foo(){console.log("Hello World!")}()//使用~，正常执行
void function foo(){console.log("Hello World!")}()//使用void，正常执行
new function foo(){console.log("Hello World!")}()//使用new，正常执行
```

> 原理：让 JS 引擎认为这不是函数声明式
>
> 可以把foo省略，写成匿名函数

#### 箭头函数

ES6标准新增了一种新的函数：Arrow Function（箭头函数）

箭头函数没有`this`，`arguments`，`super`、`new.target`、`prototype`。

> **super**关键字用于访问和调用一个对象的父对象上的函数。
>
> **new.target**属性允许你检测函数或构造方法是否是通过[new](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/new)运算符被调用的。即箭头函数不能用作构造器，和 new一起用会抛出错误。
>
> 箭头函数不能用作函数生成器Generator 。

```js
x => x * x
// 等价于
function (x) {
  return x * x;
}
```

箭头函数相当于匿名函数，并且简化了函数定义。箭头函数有两种格式，一种像上面的，只包含一个表达式，连{ ... }和return都省略掉了。还有一种可以包含多条语句，这时候就不能省略{ ... }和return：

```js
x => {
  if (x > 0) {
 		return x * x;
  }
  else {
		return - x * x;
  }
}
```

如果参数不是一个，就需要用括号()括起来：

```js
// 两个参数:
(x, y) => x * x + y * y

// 无参数:
( ) => 3.14

// 可变参数:
(x, y, ...rest) => {
  var i, sum = x + y;
  for (i=0; i<rest.length; i++) {
		sum += rest[i];
  }
  return sum;
}
```

如果要返回一个对象，就要注意，如果是单表达式，这么写的话会报错：

```js
// SyntaxError:
x => { foo: x }
// 因为和函数体的{ ... }有语法冲突，所以要改为：
// ok:
x => ({ foo: x })
```

##### **词法作用域**

由书写代码时函数声明的位置来决定的。

箭头函数看上去是匿名函数的一种简写，但实际上，箭头函数和匿名函数有个明显的区别：箭头函数内部的this是词法作用域，由上下文确定。

所以，用bind()、call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略。

箭头函数没有prototype属性。

箭头函数不能用作生成器。

##### 适用场景

1. 箭头函数适合于无复杂逻辑或者无副作用的纯函数场景下，例如用在 map 、 reduce 、 filter 的回调函数定义中；

2. 不要在最外层定义箭头函数，因为在函数内部操作 this 会很容易污染全局作用域。最起码在箭头函数外部包一层普通函数，将 this 控制在可见的范围内；

3. 如开头所述，箭头函数最吸引人的地方是简洁。在有多层函数嵌套的情况下，箭头函数的简洁性并没有很大的提升，反而影响了函数的作用范围的识别度，这种情况不建议使用箭头函数。

#### 高阶函数

高阶函数英文叫 `Higher-order function`。高阶函数是对其他函数进行操作的函数，操作可以是将函数作为参数，或者返回函数。简单总结为高阶函数是一个接收函数作为参数或者将函数作为返回输出的函数。

在JS中，函数是一等公民
1. 可赋值给变量
2. 可作为参数传递
3. 可作为结果return
4. 可包含在数据结构里

##### Array对象的高阶函数

- forEach 只遍历数组，不返回新数组
- map  对每项元素做改变后，返回新数组，遍历需return
- reduce  对每项元素做叠加，返回叠加后的值，遍历需return
- some  判断数组中某些项是否符合条件（内部return true跳出循环），返回布尔值。遍历的是空数组，直接返回false。
- every  判断数组中每一项是否符合条件（内部return false跳出循环），返回布尔值。遍历的是空数组，直接返回true。
- filter  筛选出符合条件的元素，返回新数组，遍历需return布尔值
- find 返回数组中满足条件的第一个元素的值，否则返回undefined，遍历需return布尔值
- findIndex  返回数组中满足条件的第一个元素的索引，否则返回-1，遍历需return布尔值
- includes  判断一个数组是否包含一个指定的值，返回布尔值

> forEach、map、reduce无法break跳出循环，不能通过return操作退出循环

[15个必须知道的数组方法](https://mp.weixin.qq.com/s?__biz=MzIyMDkwODczNw==&mid=2247489785&idx=2&sn=081bfa65dde2a7afd456446ab584962c&chksm=97c58557a0b20c41a10a5e9f8f703b234587c446ff3fe2ba1f4e9b00d69218725033590c06fd&mpshare=1&scene=1&srcid=0909JGL7noZ12WJ23eDNQntq&sharer_sharetime=1600417803716&sharer_shareid=27dcefb05bb42e9bc0099d713541b753&key=289ce58814d2cb9f99707c59176d8414168f707ab7787f2762e760713a7e8356a074aa25d68df39184a39832930509aa54cb71e933313904df1505cae8c02a663c3d9a86f320a5b68d22d5fc7d55fc9659b47eee3b65630d8ccd67f2074992908819170152fdcec863c4064e8b5a7374d99bce2936732de895d7afe01ecd3c64&ascene=1&uin=NjMxODk4NDM4&devicetype=Windows+7+x64&version=62090070&lang=zh_CN&exportkey=AZjpDr8XQeyG2MyOkDGStFw=&pass_ticket=ueinQ2L4MqKNmXVpmfiBK9MosLQpuVnoM3G8ruA/olLkhQM/VzO5XcmlT9P8O6a/&wx_header=0)

#### 函数柯里化

柯里化，英语：Currying

是把多个参数的函数变换成单一参数（最初函数的第一个参数）的函数，并且返回接受余下参数且返回结果的新函数。

```js
// 普通的add函数
function add(x, y) {
    return x + y
}

// Currying后
function curryingAdd(x) {
    return function (y) {
        return x + y
    }
}

add(1, 2)           // 3
curryingAdd(1)(2)   // 3
```

#### 立即执行函数

要让 JS 引擎认为这不是一个函数声明：

```js
(function(a){
    console.log(a);   //firebug输出123,使用（）运算符
})(123);
 
(function(a){
    console.log(a);   //firebug输出1234，使用（）运算符
}(1234));
 
!function(a){
    console.log(a);   //firebug输出12345,使用！运算符
}(12345);
 
+function(a){
    console.log(a);   //firebug输出123456,使用+运算符
}(123456);
 
-function(a){
    console.log(a);   //firebug输出1234567,使用-运算符
}(1234567);
 
var fn=function(a){
    console.log(a);   //firebug输出12345678，使用=运算符
}(12345678)
```

#### Set和Map数据结构

##### Set

###### 基本用法

`Set`对象是值的集合，你可以按照插入的顺序迭代它的元素（元素是有序的）。 Set中的元素只会**出现一次**，即 Set 中的元素是唯一的。

`Set`本身是一个构造函数，用来生成 Set 数据结构。

```javascript
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

上面代码通过`add()`方法向 Set 结构加入成员，结果表明 Set 结构不会添加重复的值。

`Set`函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

`et`函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化。

```javascript
// 例一
const set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
const set = new Set(document.querySelectorAll('div'));
set.size // 56
```

去除数组的重复成员：

```javascript
[...new Set(array)]
```

上面的方法也可以用于，去除字符串里面的重复字符。

```javascript
[...new Set('ababbc')].join('')
// "abc"
```

`Array.from`方法可以将 Set 结构转为数组。

```javascript
const items = new Set([1, 2, 3, 4, 5]);
const array = Array.from(items);
```

这就提供了去除数组重复成员的另一种方法。

向 Set 加入值的时候，不会发生类型转换，所以`5`和`"5"`是两个不同的值。Set 内部判断两个值是否不同，使用的算法叫做“Same-value-zero equality”，它类似于精确相等运算符（`===`），主要的区别是向 Set 加入值时认为`NaN`等于自身，而精确相等运算符===认为`NaN`不等于自身。

```javascript
let set = new Set();
let a = NaN;
let b = NaN;
set.add(a);
set.add(b);
set // Set {NaN}
```

这表明，在 Set 内部，两个`NaN`是相等的。

另外，两个对象总是不相等的。

```javascript
let set = new Set();

set.add({});
set.size // 1

set.add({});
set.size // 2
```

上面代码表示，由于两个空对象不相等，所以它们被视为两个值。

###### Set 实例的属性和方法 

Set 结构的实例有以下属性。

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。下面先介绍四个操作方法。

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。



`Object`结构和`Set`结构的不同写法：

```javascript
// 对象的写法
const properties = {
  'width': 1,
  'height': 1
};

if (properties[someName]) {
  // do something
}

// Set的写法
const properties = new Set();

properties.add('width');
properties.add('height');

if (properties.has(someName)) {
  // do something
}
```

###### 遍历方法

Set 结构的实例有四个遍历方法，可以用于遍历成员。**`Set`的遍历顺序就是插入顺序。**

- `Set.prototype.keys()`：返回键名的遍历器
- `Set.prototype.values()`：返回键值的遍历器
- `Set.prototype.entries()`：返回键值对的遍历器
- `Set.prototype.forEach()`：使用回调函数遍历每个成员

**（1）`keys()`，`values()`，`entries()`**

`keys`方法、`values`方法、`entries`方法返回的都是遍历器对象（详见《Iterator 对象》一章）。由于 Set 结构没有键名，只有值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let item of set.keys()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.values()) {
  console.log(item);
}
// red
// green
// blue

for (let item of set.entries()) {
  console.log(item);
}
// ["red", "red"]
// ["green", "green"]
// ["blue", "blue"]
```

上面代码中，`entries`方法返回的遍历器，同时包括键名和键值

可以省略`values`方法，直接用`for...of`循环遍历 Set。

```javascript
let set = new Set(['red', 'green', 'blue']);

for (let x of set) {
  console.log(x);
}
// red
// green
// blue
```

**（2）`forEach()`**

Set 结构的实例与数组一样，也拥有`forEach`方法，用于对每个成员执行某种操作，没有返回值。

```javascript
let set = new Set([1, 4, 9]);
set.forEach((value, key) => console.log(key + ' : ' + value))
// 1 : 1
// 4 : 4
// 9 : 9
```

**（3）扩展运算符**

扩展运算符（`...`）内部使用`for...of`循环，所以也可以用于 Set 结构。

```javascript
let set = new Set(['red', 'green', 'blue']);
let arr = [...set];
// ['red', 'green', 'blue']
```

扩展运算符和 Set 结构相结合，就可以去除数组的重复成员。

```javascript
let arr = [3, 5, 2, 2, 5, 5];
let unique = [...new Set(arr)];
// [3, 5, 2]
```

Set 可以很容易地实现并集（Union）、交集（Intersect）和差集（Difference）。

```javascript
let a = new Set([1, 2, 3]);
let b = new Set([4, 3, 2]);

// 并集
let union = new Set([...a, ...b]);
// Set {1, 2, 3, 4}

// 交集
let intersect = new Set([...a].filter(x => b.has(x)));
// set {2, 3}

// （a 相对于 b 的）差集
let difference = new Set([...a].filter(x => !b.has(x)));
// Set {1}
```

如果想在遍历操作中，同步改变原来的 Set 结构，目前没有直接的方法，但有两种变通方法。一种是利用原 Set 结构映射出一个新的结构，然后赋值给原来的 Set 结构；另一种是利用`Array.from`方法。Set本身没有 `map`和 `filter`方法。

```javascript
// 方法一
let set = new Set([1, 2, 3]);
set = new Set([...set].map(val => val * 2));
// set的值是2, 4, 6

// 方法二
let set = new Set([1, 2, 3]);
set = new Set(Array.from(set, val => val * 2));
// set的值是2, 4, 6
```

上面代码提供了两种方法，直接在遍历操作中改变原来的 Set 结构。

##### WeakSet

###### 基本用法

`WeakSet` 对象是一些对象值的集合, 并且其中的每个对象值都只能出现一次。

`WeakSet` 对象允许你将**弱保持对象**存储在一个集合中。

它和 [`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set) 对象的区别有两点:

- 与`Set`相比，`WeakSet` 只能是**对象或数组的集合**，而不能是任何类型的任意值。
- `WeakSet`持弱引用：集合中对象的引用为弱引用。 如果没有其他的对`WeakSet`中对象的引用，那么这些对象会被当成垃圾回收掉。 这也意味着WeakSet中没有存储当前对象的列表。 正因为这样，`WeakSet` 是不可枚举的，没有办法拿到它包含的所有元素（所以没有遍历方法）。

因为`WeakSet` 对象不可遍历，因此`WeakSet` 对象内属性的顺序不重要，无遍历方法，无size属性，无clear方法。

因为垃圾回收机制依赖引用计数，如果一个值的引用次数不为`0`，垃圾回收机制就不会释放这块内存。当结束使用该值之后，有时会忘记取消引用，导致内存无法释放，进而可能会引发内存泄漏。

添加进WeakSet的元素对象，WeakSet不会对元素对像的引用计数加1，对于被添加进WeakSet的元素对象，只要该元素对象没有被除WeakSet以外的其他对象引用，就会被垃圾回收释放，在WeakSet中的该元素对象自动被释放，不会出现内存泄漏。

因此，WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。

由于上面这个特点，WeakSet 的成员是不适合引用的，因为它会随时消失。另外，由于 WeakSet 内部有多少个成员，取决于这些成员有没有在外部被垃圾回收机制回收，垃圾回收机制运行前后很可能成员个数是不一样的，而垃圾回收机制何时运行是不可预测的，因此 ES6 规定 WeakSet 不可遍历。

WeakSet 是一个构造函数，可以使用`new`命令，创建 WeakSet 数据结构。

```javascript
const ws = new WeakSet();
```

作为构造函数，WeakSet 可以接受一个[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)或空数组作为参数，且该可迭代对象的所有成员需要是对象或数组类型。该数组的所有成员，都会自动成为 WeakSet 实例对象的成员。

```javascript
const a = [[1, 2, 3], [3, 4]];
const ws = new WeakSet(a);
// WeakSet {[1, 2, 3], [3, 4]}
```

```javascript
const b = [3, 4];
const ws1 = new WeakSet(b); // Uncaught TypeError: Invalid value used in weak set(…)

const ws2 = new WeakSet({}); // object is not iterable (cannot read property Symbol(Symbol.iterator)) 
```

虽然不能接受非可迭代对象作为WeakSet的参数，但是可以为其实例添加对象

```js
var ws = new WeakSet();
var foo = {};
var bar = {};

ws.add(foo);
ws.add(bar);

ws.has(foo);    // true
ws.has(bar);   // true
```

###### 方法

WeakSet 有以下三个方法。

- **WeakSet.prototype.add(value)**：向 WeakSet 实例添加一个新成员。
- **WeakSet.prototype.delete(value)**：清除 WeakSet 实例的指定成员。
- **WeakSet.prototype.has(value)**：返回一个布尔值，表示某个值是否在 WeakSet 实例之中。

WeakSet 没有`size`属性，没有办法遍历它的成员。

```javascript
ws.size // undefined
ws.forEach // undefined

ws.forEach(function(item){ console.log('WeakSet has ' + item)})
// TypeError: undefined is not a function
```

WeakSet 不能遍历，是因为成员都是弱引用，随时可能消失，遍历机制无法保证成员的存在，很可能刚刚遍历结束，成员就取不到了。WeakSet 的一个用处，是储存 DOM 节点，而不用担心这些节点从文档移除时，会引发内存泄漏。

下面是 WeakSet 的另一个例子。

```javascript
const foos = new WeakSet()
class Foo {
  constructor() {
    foos.add(this)
  }
  method () {
    if (!foos.has(this)) {
      throw new TypeError('Foo.prototype.method 只能在Foo的实例上调用！');
    }
  }
}
```

##### Map

###### 基本用法

**`Map`** 对象保存键值对，并且能够记住键的原始插入顺序（键是有序的）。任何值(对象或者[原始值/基本类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive)) 都可以作为一个键或一个值。**`Map`** 对象中，键是唯一的，相同的键名，后面的键值会覆盖前面的键值。

JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。

Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

上面的例子展示了如何向 Map 添加成员。作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

`Map` 可以接受一个[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)或空数组作为参数，且该可迭代对象的所有成员需要是对象或数组类型。

```javascript
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"

let map = new Map([1,2]); // Iterator value 1 is not an entry object
```

```js
// 数组元素中的数组的第三个值及之后的值都是无效的
let map = new Map([
  ['name', '张三', 111],
  ['title', 'Author', 222],
]);
console.log(map); // Map { 'name' => '张三', 'title' => 'Author' }


let map = new Map([{ a: 1 }]);
console.log(map); // Map { undefined => undefined }
```

虽然不能接受非可迭代对象作为WeakSet的参数，但是可以为其实例添加对象

```javascript
const m = new Map();
m.set(1, 'content')
m.get(1) // "content"
```

**键的相等**

- 键的比较是基于 `sameValueZero` 算法：
- [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) 是与 `NaN` 相等的（虽然 `NaN !== NaN`），剩下所有其它的值是根据 `===` 运算符的结果判断是否相等（例如`-0`和`+0`被认为是相等的）。

###### Map 实例的属性和方法

**（1）size 属性**

`size`属性返回 Map 结构的成员总数。

```javascript
const map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
```

**（2）Map.prototype.set(key, value)**

`set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。

```javascript
const m = new Map();

m.set('edition', 6)        // 键是字符串
m.set(262, 'standard')     // 键是数值
m.set(undefined, 'nah')    // 键是 undefined
```

`set`方法返回的是当前的`Map`对象，因此可以采用链式写法。

```javascript
let map = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');
```

**（3）Map.prototype.get(key)**

`get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。

```javascript
const m = new Map();

const hello = function() {};
m.set(hello, 'Hello ES6!') // 键是函数

m.get(hello)  // Hello ES6!
```

**（4）Map.prototype.has(key)**

`has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

```javascript
const m = new Map();

m.set('edition', 6);
m.set(262, 'standard');
m.set(undefined, 'nah');

m.has('edition')     // true
m.has('years')       // false
m.has(262)           // true
m.has(undefined)     // true
```

**（5）Map.prototype.delete(key)**

`delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。

```javascript
const m = new Map();
m.set(undefined, 'nah');
m.has(undefined)     // true

m.delete(undefined)
m.has(undefined)       // false
```

**（6）Map.prototype.clear()**

`clear`方法清除所有成员，没有返回值。

```javascript
let map = new Map();
map.set('foo', true);
map.set('bar', false);

map.size // 2
map.clear()
map.size // 0
```

###### 遍历方法

Map 结构原生提供三个遍历器生成函数和一个遍历方法。

- `Map.prototype.keys()`：返回键名的遍历器。
- `Map.prototype.values()`：返回键值的遍历器。
- `Map.prototype.entries()`：返回所有成员的遍历器。
- `Map.prototype.forEach()`：遍历 Map 的所有成员。

需要特别注意的是，Map 的遍历顺序就是插入顺序。

```javascript
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

上面代码最后的那个例子，表示 Map 结构的默认遍历器接口（`Symbol.iterator`属性），就是`entries`方法。

```javascript
map[Symbol.iterator] === map.entries
// true
```

Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。

```javascript
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```

结合数组的`map`方法、`filter`方法，可以实现 Map 的遍历和过滤（Map 本身没有`map`和`filter`方法）。

```javascript
const map0 = new Map()
  .set(1, 'a')
  .set(2, 'b')
  .set(3, 'c');

const map1 = new Map(
  [...map0].filter(([k, v]) => k < 3)
);
// 产生 Map 结构 {1 => 'a', 2 => 'b'}
```

此外，Map 还有一个`forEach`方法，与数组的`forEach`方法类似，也可以实现遍历。

```javascript
map.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```

`forEach`方法还可以接受第二个参数，用来绑定`this`。

###### 与其他数据结构的互相转换

**（1）Map 转为数组**

前面已经提过，Map 转为数组最方便的方法，就是使用扩展运算符（`...`）。

```javascript
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap]
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

**（2）数组 转为 Map**

将数组传入 Map 构造函数，就可以转为 Map。

```javascript
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```

**（3）Map 转为对象**

如果所有 Map 的键都是字符串，它可以无损地转为对象。

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

如果有非字符串的键名，那么这个键名会被转成字符串（如果该键名是对象，则转化为 '[object Object]' ），再作为对象的键名。

**（4）对象转为 Map**

对象转为 Map 可以通过`Object.entries()`。

```javascript
let obj = {"a":1, "b":2};
let map = new Map(Object.entries(obj));
```

> `Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环还会枚举原型链中的属性）。

此外，也可以自己实现一个转换函数。

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
}

objToStrMap({yes: true, no: false})
// Map {"yes" => true, "no" => false}
```

**（5）Map 转为 JSON**

Map 转为 JSON 要区分两种情况。一种情况是，Map 的键名都是字符串，这时可以选择转为对象 JSON。

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```

另一种情况是，Map 的键名有非字符串，这时可以选择转为数组 JSON。

```javascript
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

**（6）JSON 转为 Map**

JSON 转为 Map，正常情况下，所有键名都是字符串。

```javascript
function objToStrMap(obj) {
  let strMap = new Map();
  for (let k of Object.keys(obj)) {
    strMap.set(k, obj[k]);
  }
  return strMap;
  // or return new Map(Object.entries(obj))
}

function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
```

但是，有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 Map 转为数组 JSON 的逆操作。

```javascript
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```

##### WeakMap

###### 基本用法

**`WeakMap`** 对象是一组键/值对的集合，其中的键是弱引用的。其键必须是对象，而值可以是任意的。键是唯一的，相同键名，后面的键值会覆盖前面的键值。

语法

```js
new WeakMap([iterable])
```

iterable 是一个数组（二元数组）或者其他可迭代的且其元素是键值对的对象。每个键值对会被加到新的 WeakMap 里。null 会被当做 undefined。

因为`WeakSet` 对象不可遍历，因此`WeakSet` 对象内属性的顺序不重要，无遍历方法，无size属性，无clear方法。

###### 方法

WeakMap 有以下三个方法。

- **WeakMap.prototype.set(key,value)**：向 WeakMap 实例添加一个键值对。
- **WeakMap.prototype.get(key)**：返回 WeakMap 实例的指定成员。
- **WeakMap.prototype.delete(key)**：清除 WeakMap 实例的指定成员。
- **WeakMap.prototype.has(value)**：返回一个布尔值，表示某个值是否在 WeakMap 实例之中。

###### 使用场景

WeakMap 应用的典型场合就是 DOM 节点作为键名。下面是一个例子。

```javascript
let myWeakmap = new WeakMap();

myWeakmap.set(
  document.getElementById('logo'),
  {timesClicked: 0})
;

document.getElementById('logo').addEventListener('click', function() {
  let logoData = myWeakmap.get(document.getElementById('logo'));
  logoData.timesClicked++;
}, false);
```

#### Promise

Promise 对象代表了未来将要发生的事件，用来传递异步操作的消息。

Promise对象里的函数会立即执行

```js
new Promise((resolve, reject) => {
  console.log(1)  // 立即执行
  resolve()  // 立即执行
}).then(res=>{})
```

当我们调用resolve()或reject()的时候，触发promise.then(...)实际上是一个异步操作，这个promise.then(...)并不是在resolve()或reject()的时候就立刻执行的，而也是要重新进入任务队列排队的，不过能直接在当前的事件循环新的执行栈中被取出执行（不用等下次事件循环）。

##### Promise.resolve() 

返回一个状态由给定value决定的Promise对象。

```js
Promise.resolve() 
// 等价于
new Promise(resolve =>{
	resolve()
})
```

##### Promise.reject()

返回一个状态为失败的Promise对象，并将给定的失败信息传递给对应的处理方法—then或catch。

```js
Promise.reject() 
// 等价于
new Promise((resolve,reject) =>{
	reject()
})
```

##### Promise.prototype.then(onFulfilled [, onRejected])

最多需要有两个参数：Promise 的成功和失败情况的回调函数。返回一个新的 promise。

成功状态的回调函数接受前一个promise resolve的值作为参数。

失败状态的回调函数接受前一个promise reject的值作为参数。

##### Promise.prototype.catch(onRejected)

catch内的回调函数接受前一个promise reject的值作为参数。返回一个新的 promise。

##### Promise.all([ promise1,promise2, . . . ])

接收一个Promise对象的集合，返回一个新的promise对象。该promise对象在数组里所有的promise对象都成功的时候才会触发成功，一旦有任何一个promise对象失败则立即触发该promise对象的失败。这个新的promise对象在触发成功状态以后，会把一个包含所有promise返回值的数组作为成功回调的返回值，顺序跟数组元素的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把数组里第一个触发失败的promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。

##### Promise.allSettked([ promise1,promise2, . . . ])

接收一个Promise对象的集合，等到所有promises都已敲定（settled）（每个promise都已兑现（fulfilled）或已拒绝（rejected））。
返回一个新promise对象，该promise在所有promise完成后完成。并带有一个对象数组（可以在then中获取），每个对象对应每个promise的结果。

##### Promise.any([ promise1,promise2, . . . ])

接收一个Promise对象的集合，当其中的一个 promise 成功，就返回那个成功的promise对象。

##### Promise.race([ promise1,promise2, . . . ])

接收一个Promise对象的集合。race就是赛跑的意思，意思就是说，Promise.race([p1, p2, p3])里面哪个结果获取得快，就返回那个结果，不管结果本身是成功状态还是失败状态。返回最快的那个promise对象。

##### 使用例子

```js
let temp = 'aaa'
new Promise((resolve, reject) => {
  console.log(temp)
  setTimeout(() => {
    resolve(temp + '111')
  }, 1000)
}).then(data => {
  console.log(data)
  return new Promise(resolve => {
    resolve(data + '222')
  })
    .then(data => {
      console.log(data)
      return Promise.resolve(data + '333') // new Promise((resolve) => {resolve()})的简写
    })
    .then(data => {
      console.log(data)
      return 'Promise' // 可以直接返回数据，实际上底层还是进行了new Promise
    })
    .then(data => {
      console.log(data)
      return Promise.reject('err')
    })
    .then(data => {
      console.log('。。。') // 因为是reject，所以不会执行then的第一个回调函数
      // 下面写了catch，这里就不写then的第二个回调函数了
    })
    .catch(err => {
      console.log(err)
      return Promise.resolve('Promise')
    })
    .then(data => {
      console.log(data) // 'Promise'
      console.log(qqq) // qqq未定义
      // throw 'err' 将异常信息定义为err
      console.log('-------') // 同一个作用域下，异常后面的代码不执行
    })
    .catch(err => {
      console.log(err) // catch还能捕获then中第一个回调函数抛出的异常,防止阻塞
      return 'Promise'
    })
    .then(data => {
      console.log(data) // 'Promise'
    })
})

// ----------------------------------------------

let temp1 = 'aaa'
new Promise((resolve, reject) => {
  console.log(temp1)
  setTimeout(() => {
    resolve(temp1 + '111')
  }, 1000)
})
  .then(res => {
    console.log(res)
    // 可以直接return data，等于 return Promise.resolve(结果)
    return res + '888'
  })
  .then(res => {
    console.log(res) // aaa111888
    throw 'err'
  })
  .catch(err => {
    console.log(err)
    // 如果是 return 结果 或者return Promise.resolve(结果)，则可以被后面的then接受
    return Promise.resolve('catch里面的resolve可以被之后的then接收')
    // 如果是return Promise.reject，catch后面必须有一个catch，但不一定是紧跟
    // return Promise.reject('catch里面的reject可以被之后的catch接收')
  })
  .then(res => {
    console.log(res)
  })
  // finally是获取不到then和catch的return
  .finally(res => {
    console.log(res)
  })

```

#### async/await

`async`和`await`关键字让我们可以用一种更简洁的方式写出基于Promise的异步行为，而无需刻意地链式调用`promise`。

1、await  只能被用在 async 函数中，await后面一般是async函数（返回promise对象的函数）或者后面接new Promise() 或者任何要等待的值。await就是把promise的resolve的数据拿出来直接使用。

2、await会等待结果返回，再执行之后的代码

> 如果await后面的promise没有resolve一个值，对await来说是失败了，后面的代码不会执行

3、async 修饰的函数  隐式返回一个 promise（即使不写return）。箭头函数也能用async修饰。

async函数内return  xxx ，底层内部实际上进行了new Promise (resolve => resolve(xxx))，跟Promise的用法一样。

4、在 await 外层  包一层  try{} catch(e){} 使用 可以获取异常

5、并发要使用await Promise.all([ promise1,promise2, . . . ])

6、async函数外部如果要到用then(res=>{})，要在async函数内return res。async函数内部就是promise 和 then 的语法糖。

7、async函数本身不是异步的，是它里面的代码可能是异步的，如果想要执行完async函数再执行其他操作（而这个操作不想写在async函数内，因为不想每次执行async都会执行这个操作），这时候要在async函数后使用then。

> 实际上，async / await 在底层转换成了 promise 和 then 回调函数。是基于promises的语法糖。每次我们使用 await, 解释器都创建一个 promise 对象，然后把剩下的 async 函数中的操作放到 then 回调函数中。

- await会阻塞后面的代码

```js
async function fn(){
	await console.log('a')
}
fn()
console.log('b')
// a
// b
```

- 如果await后面的promise没有resolve一个值，对await来说是失败了，后面的代码不会执行

```js
function fn(){
  return new Promise(resolve=>{
      console.log(1)
  })
}
async function f1(){
  await fn()
  console.log(2)
}
f1()
console.log(3)
//1
//3
```

```js
function fn(){
  return new Promise(resolve=>{
    console.log(1)
    resolve()
  })
}
async function f1(){
  await fn()
  console.log(2)
}
f1()
console.log(3)
//1
//3
//2
```



#### Generator函数

##### Generator函数

Generator 函数有多种理解角度。语法上，首先可以把它理解成，Generator 函数是一个状态机，封装了多个内部状态。

Generator 函数还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

形式上，Generator 函数是一个普通函数，但是有两个特征。一是，`function`关键字与函数名之间有一个星号；二是，函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）。

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
```

调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象——遍历器对象（Iterator Object）。

```javascript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```

调用遍历器对象的`next`方法，使得指针移向下一个状态。也就是说，每次调用`next`方法，内部指针就从函数头部或上一次停下来的地方开始执行，直到遇到下一个`yield`表达式（或`return`语句）为止。换言之，Generator 函数是分段执行的，`yield`表达式是暂停执行的标记，而`next`方法可以恢复执行。

总结一下，调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的`next`方法，就会返回一个有着`value`和`done`两个属性的对象。`value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束。

ES6 没有规定，`function`关键字与函数名之间的星号，写在哪个位置。这导致下面的写法都能通过。

```javascript
function * foo(x, y) { ··· }
function *foo(x, y) { ··· }
function* foo(x, y) { ··· }
function*foo(x, y) { ··· }
```

###### 实例方法

[`Generator.prototype.next()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator/next)

`next`方法可以带一个参数，该参数会被当作上一个`yield`表达式的返回值。

[`Generator.prototype.return()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator/return)

返回给定的值并结束生成器。

[`Generator.prototype.throw()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Generator/throw)

向生成器抛出一个错误。

##### yield 表达式

由于 Generator 函数返回的遍历器对象，只有调用`next`方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。`yield`表达式就是暂停标志。

`yield`表达式本身没有返回值，或者说总是返回`undefined`。

```js
yield x+2 // x+2 是表达式
yield 1+2*3 // 1+2*3 是表达式
```

**`yield`和 `return`的区别**

每次遇到`yield`，函数暂停执行，下一次再从该位置继续向后执行，而`return`语句不具备位置记忆的功能。一个函数里面，只能执行一次（或者说一个）`return`语句，但是可以执行多次（或者说多个）`yield`表达式。正常函数只能返回一个值，因为只能执行一次`return`；Generator 函数可以返回一系列的值，因为可以有任意多个`yield`。

**`yield`的使用条件**

`yield`表达式只能用在 Generator 函数里面，用在其他地方都会报错。

`yield`表达式如果用在另一个表达式之中，必须放在圆括号里面。

```javascript
function* demo() {
  console.log('Hello' + yield); // SyntaxError
  console.log('Hello' + yield 123); // SyntaxError

  console.log(yield 123); // OK
  console.log('Hello' + (yield)); // OK
  console.log('Hello' + (yield 123)); // OK
}
```

`yield`表达式用作函数参数或放在赋值表达式的右边，可以不加括号。

```javascript
function* demo() {
  foo(yield 'a', yield 'b'); // OK
  let input = yield; // OK
}
```

##### Generator 与 Iterator 接口的关系

任意一个对象的`Symbol.iterator`方法，等于该对象的遍历器生成函数，调用该函数会返回该对象的一个遍历器对象。

由于 Generator 函数就是遍历器生成函数，因此可以把 Generator 赋值给对象的`Symbol.iterator`属性，从而使得该对象具有 Iterator 接口。

```javascript
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]
```

上面代码中，Generator 函数赋值给`Symbol.iterator`属性，从而使得`myIterable`对象具有了 Iterator 接口，可以被`...`运算符遍历了。

##### for...of 循环

`for...of`循环可以自动遍历 Generator 函数运行时生成的`Iterator`对象，且此时不再需要调用`next`方法。

```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
```

上面代码使用`for...of`循环，依次显示 5 个`yield`表达式的值。这里需要注意，一旦`next`方法的返回对象的`done`属性为`true`，`for...of`循环就会中止，且不包含该返回对象，所以上面代码的`return`语句返回的`6`，不包括在`for...of`循环之中。

除了`for...of`循环以外，扩展运算符（`...`）、解构赋值和`Array.from`方法内部调用的，都是遍历器接口。这意味着，它们都可以将 Generator 函数返回的 Iterator 对象，作为参数。

```javascript
function* numbers () {
  yield 1
  yield 2
  return 3
  yield 4
}

// 扩展运算符
[...numbers()] // [1, 2]

// Array.from 方法
Array.from(numbers()) // [1, 2]

// 解构赋值
let [x, y] = numbers();
x // 1
y // 2

// for...of 循环
for (let n of numbers()) {
  console.log(n)
}
// 1
// 2
```

##### next 方法

```js
gen.next(value)
```

`next`方法可以带一个参数，该参数会被当作上一个`yield`表达式的返回值。

`next`方法返回一个有着`value`和`done`两个属性的对象。`value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束。

**遍历器对象的`next`方法的运行逻辑如下：**

（1）遇到`yield`表达式，就暂停执行后面的操作，并将紧跟在`yield`后面的那个表达式的值，作为返回的对象的`value`属性值。

（2）下一次调用`next`方法时，再继续往下执行，直到遇到下一个`yield`表达式。

（3）如果没有再遇到新的`yield`表达式，就一直运行到函数结束，直到`return`语句为止，并将`return`语句后面的表达式的值，作为返回的对象的`value`属性值，`done`属性值为`true`。

（4）如果该函数没有`return`语句或者已执行过 `return`语句，则返回的对象的`value`属性值为`undefined`，`done`属性值为`true`。

```js
function* generator() {
  console.log(yield 123)
  var a = yield 'world'
  console.log(a)
  return 'ending'
}
var a = generator()
console.log(a.next()) // 启动，打印 { value: 123, done: false }
console.log(a.next()) // 打印 undefined，打印 { value: 'world', done: false }
console.log(a.next(1)) // 打印 1，打印 { value: 'ending', done: true }
console.log(a.next()) // 打印 { value: undefined, done: true }
```

```javascript
function* foo(x) {
  var y = 2 * (yield (x + 1));
  var z = yield (y / 3);
  return (x + y + z);
}

var a = foo(5);
a.next() // Object{value:6, done:false}
a.next() // Object{value:NaN, done:false}
a.next() // Object{value:NaN, done:true}

var b = foo(5);
b.next() // { value:6, done:false }
b.next(12) // { value:8, done:false }
b.next(13) // { value:42, done:true }
```

上面代码中，第二次运行`next`方法的时候不带参数，导致 y 的值等于`2 * undefined`（即`NaN`），除以 3 以后还是`NaN`，因此返回对象的`value`属性也等于`NaN`。第三次运行`Next`方法的时候不带参数，所以`z`等于`NaN`，返回对象的`value`属性等于`5 + NaN + NaN`，即`NaN`。

如果向`next`方法提供参数，返回结果就完全不一样了。上面代码第一次调用`b`的`next`方法时，返回`x+1`的值`6`；第二次调用`next`方法，将上一次`yield`表达式的值设为`12`，因此`y`等于`24`，返回`y / 3`的值`8`；第三次调用`next`方法，将上一次`yield`表达式的值设为`13`，因此`z`等于`13`，这时`x`等于`5`，`y`等于`24`，所以`return`语句的值等于`42`。

注意，由于`next`方法的参数表示上一个`yield`表达式的返回值，所以在第一次使用`next`方法时，传递参数是无效的。V8 引擎直接忽略第一次使用`next`方法时的参数，只有从第二次使用`next`方法开始，参数才是有效的。

如果想要第一次调用`next`方法时，就能够输入值，可以在 Generator 函数外面再包一层，原理是自动先调用了一次next。

```js
function wrapper(generatorFunction) {
  return function (...args) {
    let generatorObject = generatorFunction(...args)
    generatorObject.next()
    return generatorObject
  }
}

let wrapped = wrapper(function* () {
  console.log(`First input: ${yield}`)
  return 'DONE'
})

let g = wrapped()
console.log(g.next('hello')) // 打印 First input: hellow，打印 { value: 'DONE', done: true }
console.log(g.next()) // 打印{ value: undefined, done: true }
```

##### return方法

```js
gen.return(value)
```

返回给定的值并结束生成器。

```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next();        // { value: 1, done: false }
g.return("foo"); // { value: "foo", done: true }
g.next();        // { value: undefined, done: true }
```

如果对已经处于“完成”状态的生成器调用`return(value)`，则生成器将保持在“完成”状态。如果不提供参数，则返回值的`value`属性为`undefined`， `done`为 `true`。如果提供了参数，则参数将被设置为返回对象的`value`属性的值， `done`为 `true`：

```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();
g.next(); // { value: 1, done: false }
g.next(); // { value: 2, done: false }
g.next(); // { value: 3, done: false }
g.next(); // { value: undefined, done: true }
g.return(); // { value: undefined, done: true }
g.return(1); // { value: 1, done: true }
```

如果 Generator 函数内部有`try...finally`代码块，且正在执行`try`代码块，那么`return()`方法会导致立刻进入`finally`代码块，执行完以后，整个函数才会结束。

```javascript
function* numbers () {
  yield 1;
  try {
    yield 2;
    yield 3;
  } finally {
    yield 4;
    yield 5;
  }
  yield 6;
}
var g = numbers();
g.next() // { value: 1, done: false }
g.next() // { value: 2, done: false }
g.return(7) // { value: 4, done: false }
g.next() // { value: 5, done: false }
g.next() // { value: 7, done: true }
```

上面代码中，调用`return()`方法后，就开始执行`finally`代码块，不执行`try`里面剩下的代码了，然后等到`finally`代码块执行完，再返回`return()`方法指定的返回值。



##### throw方法

```js
gen.throw(exception)
```

exception：用于抛出的异常。 使用 [`Error`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error) 的实例对调试非常有帮助。

`throw` 方法用来向生成器抛出异常，并恢复生成器的执行，返回带有 `done` 及 `value` 两个属性的对象。

`value`属性表示当前的内部状态的值，是`yield`表达式后面那个表达式的值；`done`属性是一个布尔值，表示是否遍历结束。

下面的例子展示了一个简单的生成器并使用 throw方法向该生成器抛出一个异常，该异常通常可以通过 `try...catch` 块进行捕获.

```js
function* gen() {
  while(true) {
    try {
       yield 42;
    } catch(e) {
      console.log("Error caught!");
    }
  }
}

var g = gen();
g.next(); // { value: 42, done: false }
g.throw(new Error("Something went wrong")); // "Error caught!"
```

```javascript
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
```

上面代码中，遍历器对象`i`连续抛出两个错误。第一个错误被 Generator 函数体内的`catch`语句捕获。`i`第二次抛出错误，由于 Generator 函数内部的`catch`语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的`catch`语句捕获。

如果 Generator 函数内部没有部署`try...catch`代码块，那么`throw`方法抛出的错误，将被外部`try...catch`代码块捕获。

`throw`方法抛出的错误要被内部捕获，前提是必须至少执行过一次`next`方法。

```javascript
function* gen() {
  try {
    yield 1;
  } catch (e) {
    console.log('内部捕获');
  }
}

var g = gen();
g.throw(1);
// Uncaught 1
```

这种行为其实很好理解，因为第一次执行`next`方法，等同于启动执行 Generator 函数的内部代码，否则 Generator 函数还没有开始执行，这时`throw`方法抛错只可能抛出在函数外部。

`throw`方法被捕获以后，会附带执行下一条`yield`表达式。也就是说，会附带执行一次`next`方法。只要 Generator 函数内部部署了`try...catch`代码块，那么遍历器的`throw`方法抛出的错误，不影响下一次遍历。

```javascript
var gen = function* gen(){
  try {
    yield console.log('a');
  } catch (e) {
    // ...
  }
  yield console.log('b');
  yield console.log('c');
}

var g = gen();
g.next() // a
g.throw() // b
g.next() // c
```

一旦 Generator 执行过程中抛出错误，且没有被内部捕获，就不会再执行下去了。如果此后还调用`next`方法，将返回一个`value`属性等于`undefined`、`done`属性等于`true`的对象，即 JavaScript 引擎认为这个 Generator 已经运行结束了



##### next()、throw()、return() 的共同点

`next()`、`throw()`、`return()`这三个方法本质上是同一件事，可以放在一起理解。它们的作用都是让 Generator 函数恢复执行，并且使用不同的语句替换`yield`表达式。

`next()`是将`yield`表达式替换成一个值。

```javascript
const g = function* (x, y) {
  let result = yield x + y;
  return result;
};

const gen = g(1, 2);
gen.next(); // Object {value: 3, done: false}

gen.next(1); // Object {value: 1, done: true}
// 相当于将 let result = yield x + y
// 替换成 let result = 1;
```

上面代码中，第二个`next(1)`方法就相当于将`yield`表达式替换成一个值`1`。如果`next`方法没有参数，就相当于替换成`undefined`。

`throw()`是将`yield`表达式替换成一个`throw`语句。

```javascript
gen.throw(new Error('出错了')); // Uncaught Error: 出错了
// 相当于将 let result = yield x + y
// 替换成 let result = throw(new Error('出错了'));
```

`return()`是将`yield`表达式替换成一个`return`语句。

```javascript
gen.return(2); // Object {value: 2, done: true}
// 相当于将 let result = yield x + y
// 替换成 let result = return 2;
```

##### yield* 表达式

`yield* 表达式` 用来在一个 Generator 函数里面执行另一个 Generator 函数。

如果在 Generator 函数内部，调用另一个 Generator 函数。需要在前者的函数体内部，自己手动完成遍历。

```javascript
function* foo() {
  yield 'a';
  yield 'b';
}

function* bar() {
  yield 'x';
  // 手动遍历 foo()
  for (let i of foo()) {
    console.log(i);
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// x
// a
// b
// y
```

上面代码中，`foo`和`bar`都是 Generator 函数，在`bar`里面调用`foo`，就需要手动遍历`foo`。如果有多个 Generator 函数嵌套，写起来就非常麻烦。

ES6 提供了`yield*`表达式，作为解决办法，用来在一个 Generator 函数里面执行另一个 Generator 函数。

```javascript
function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  for (let v of foo()) {
    yield v;
  }
  yield 'y';
}

for (let v of bar()){
  console.log(v);
}
// "x"
// "a"
// "b"
// "y"
```

`yield*`后面的 Generator 函数（没有`return`语句时），等同于在 Generator 函数内部，部署一个`for...of`循环。

```javascript
function* concat(iter1, iter2) {
  yield* iter1;
  yield* iter2;
}

// 等同于

function* concat(iter1, iter2) {
  for (var value of iter1) {
    yield value;
  }
  for (var value of iter2) {
    yield value;
  }
}
```

上面代码说明，`yield*`后面的 Generator 函数（没有`return`语句时），不过是`for...of`的一种简写形式，完全可以用后者替代前者。反之，在有`return`语句时，则需要用`var value = yield* iterator`的形式获取`return`语句的值。

如果`yield*`后面跟着遍历器对象，会遍历其成员。

如果`yield*`后面跟着一个数组，由于数组原生支持遍历器，因此就会遍历数组成员。

```javascript
function* gen(){
  yield* ["a", "b", "c"];
}

gen().next() // { value:"a", done:false }
```

上面代码中，`yield`命令后面如果不加星号（相当于普通的yield），返回的是整个数组，加了星号就表示返回的是数组的遍历器对象。

实际上，任何数据结构只要有 Iterator 接口，就可以被`yield*`遍历。

##### 作为对象属性的 Generator 函数

如果一个对象的属性是 Generator 函数，可以简写成下面的形式。

```javascript
let obj = {
  * myGeneratorMethod() {
    ···
  }
};
```

它的完整形式如下，与上面的写法是等价的。

```javascript
let obj = {
  myGeneratorMethod: function* () {
    // ···
  }
};
```

##### Generator 函数的this

Generator 函数总是返回一个遍历器对象，ES6 规定这个遍历器对象是 Generator 函数的实例，也继承了 Generator 函数的`prototype`对象上的方法。

Generator 函数也不能跟`new`命令一起用，会报错。

```javascript
function* g() {}

g.prototype.hello = function () {
  return 'hi!';
};

let obj = g();

obj instanceof g // true
obj.hello() // 'hi!'
```

上面代码表明，Generator 函数`g`返回的遍历器`obj`，是`g`的实例，而且继承了`g.prototype`。但是，如果把`g`当作普通的构造函数，并不会生效，因为`g`返回的总是遍历器对象，而不是`this`对象。

```javascript
function* g() {
  this.a = 11;
}

let obj = g();
obj.next();
obj.a // undefined
```

上面代码中，Generator 函数`g`在`this`对象上面添加了一个属性`a`，但是`obj`对象拿不到这个属性。

那么，有没有办法让 Generator 函数返回一个正常的对象实例，既可以用`next`方法，又可以获得正常的`this`？

下面是一个变通方法。首先，生成一个空对象，使用`call`方法绑定 Generator 函数内部的`this`。这样，构造函数调用以后，这个空对象就是 Generator 函数的实例对象了。

```javascript
function* F() {
  this.a = 1;
  yield this.b = 2;
  yield this.c = 3;
}
var obj = {};
var f = F.call(obj);

f.next();  // Object {value: 2, done: false}
f.next();  // Object {value: 3, done: false}
f.next();  // Object {value: undefined, done: true}

obj.a // 1
obj.b // 2
obj.c // 3
```

上面代码中，执行的是遍历器对象`f`，但是生成的对象实例是`obj`，有没有办法将这两个对象统一呢？

一个办法就是将`obj`换成`F.prototype`。

```javascript
function* F() {
  this.a = 1;
  yield this.b = 2;
  yield this.c = 3;
}
var f = F.call(F.prototype);

f.next();  // Object {value: 2, done: false}
f.next();  // Object {value: 3, done: false}
f.next();  // Object {value: undefined, done: true}

f.a // 1
f.b // 2
f.c // 3
```

再将`F`改成构造函数，就可以对它执行`new`命令了。

```javascript
function* gen() {
  this.a = 1;
  yield this.b = 2;
  yield this.c = 3;
}

function F(g) {
  return g.call(g.prototype);
}

var f = new F(gen);

f.next();  // Object {value: 2, done: false}
f.next();  // Object {value: 3, done: false}
f.next();  // Object {value: undefined, done: true}

f.a // 1
f.b // 2
f.c // 3
```

##### Generator 与上下文

JavaScript 代码运行时，会产生一个全局的上下文环境（context，又称运行环境），包含了当前所有的变量和对象。然后，执行函数（或块级代码）的时候，又会在当前上下文环境的上层，产生一个函数运行的上下文，变成当前（active）的上下文，由此形成一个上下文环境的堆栈（context stack）。

这个堆栈是“后进先出”的数据结构，最后产生的上下文环境首先执行完成，退出堆栈，然后再执行完成它下层的上下文，直至所有代码执行完成，堆栈清空。

Generator 函数不是这样，它执行产生的上下文环境，一旦遇到`yield`命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态。等到对它执行`next`命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。

```javascript
function* gen() {
  yield 1;
  return 2;
}

let g = gen();

console.log(
  g.next().value,
  g.next().value,
);
```

上面代码中，第一次执行`g.next()`时，Generator 函数`gen`的上下文会加入堆栈，即开始运行`gen`内部的代码。等遇到`yield 1`时，`gen`上下文退出堆栈，内部状态冻结。第二次执行`g.next()`时，`gen`上下文重新加入堆栈，变成当前的上下文，重新恢复执行。

##### Generator 应用场景

###### （1）异步操作的同步化表达

Generator 函数的暂停执行的效果，意味着可以把异步操作写在`yield`表达式里面，等到调用`next`方法时再往后执行。这实际上等同于不需要写回调函数了，因为异步操作的后续操作可以放在`yield`表达式下面，反正要等到调用`next`方法时再执行。所以，Generator 函数的一个重要实际意义就是用来处理异步操作，改写回调函数。

```javascript
function* loadUI() {
  showLoadingScreen();
  yield loadUIDataAsynchronously();
  hideLoadingScreen();
}
var loader = loadUI();
// 加载UI（showLoadingScreen），加载数据（loadUIDataAsynchronously）
loader.next()

// 卸载UI（ hideLoadingScreen()）
loader.next()
```

Ajax 是典型的异步操作，通过 Generator 函数部署 Ajax 操作，可以用同步的方式表达。

```javascript
function* main() {
  var result = yield request("http://some.url");
  var resp = JSON.parse(result);
    console.log(resp.value);
}

var it = main();
it.next();
```

###### （2）给任意对象部署 Iterator 接口

利用 Generator 函数，可以在任意对象上部署 Iterator 接口。

```javascript
function* iterEntries(obj) {
  let keys = Object.keys(obj);
  for (let i=0; i < keys.length; i++) {
    let key = keys[i];
    yield [key, obj[key]];
  }
}

let myObj = { foo: 3, bar: 7 };

for (let [key, value] of iterEntries(myObj)) {
  console.log(key, value);
}

// foo 3
// bar 7
```

上述代码中，`myObj`是一个普通对象，通过`iterEntries`函数，就有了 Iterator 接口。

#### Generator 函数的异步应用

##### 协程

传统的编程语言，早有异步编程的解决方案（其实是多任务的解决方案）。其中有一种叫做"协程"（coroutine），意思是多个线程互相协作，完成异步任务。

协程有点像函数，又有点像线程。它的运行流程大致如下。

- 第一步，协程`A`开始执行。
- 第二步，协程`A`执行到一半，进入暂停，执行权转移到协程`B`。
- 第三步，（一段时间后）协程`B`交还执行权。
- 第四步，协程`A`恢复执行。

上面流程的协程`A`，就是异步任务，因为它分成两段（或多段）执行。

##### 协程的 Generator 函数实现

Generator 函数是 ES6 对协程的实现，最大特点就是可以交出函数的执行权（即暂停执行），但属于不完全实现。Generator 函数被称为“半协程”（semi-coroutine），意思是只有 Generator 函数的调用者，才能将程序的执行权还给 Generator 函数。如果是完全执行的协程，任何函数都可以让暂停的协程继续执行。

##### Generator 函数的数据交换和错误处理

Generator 函数可以暂停执行和恢复执行，这是它能封装异步任务的根本原因。除此之外，它还有两个特性，使它可以作为异步编程的完整解决方案：函数体内外的数据交换和错误处理机制。

`next`返回值的 value 属性，是 Generator 函数向外输出数据；`next`方法还可以接受参数，向 Generator 函数体内输入数据。

```javascript
function* gen(x){
  var y = yield x + 2;
  return y;
}

var g = gen(1);
g.next() // { value: 3, done: false }
g.next(2) // { value: 2, done: true }
```

Generator 函数内部还可以部署错误处理代码，捕获函数体外抛出的错误。

```javascript
function* gen(x){
  try {
    var y = yield x + 2;
  } catch (e){
    console.log(e);
  }
  return y;
}

var g = gen(1);
g.next();
g.throw('出错了');
// 出错了
```

##### 异步任务的封装 

下面看看如何使用 Generator 函数，执行一个真实的异步任务。

```javascript
var fetch = require('node-fetch');

function* gen(){
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);
  console.log(result.bio);
}
```

上面代码中，Generator 函数封装了一个异步操作，该操作先读取一个远程接口，然后从 JSON 格式的数据解析信息。就像前面说过的，这段代码非常像同步操作，除了加上了`yield`命令。

执行这段代码的方法如下。

```javascript
var g = gen();
var result = g.next();

result.value.then(function(data){
  return data.json();
}).then(function(data){
  g.next(data);
});
```

上面代码中，首先执行 Generator 函数，获取遍历器对象，然后使用`next`方法（第二行），执行异步任务的第一阶段。由于`Fetch`模块返回的是一个 Promise 对象，因此要用`then`方法调用下一个`next`方法。

可以看到，虽然 Generator 函数将异步操作表示得很简洁，但是流程管理却不方便（即何时执行第一阶段、何时执行第二阶段）。

#### try-catch-finally

##### 三者执行顺序

**1、如果try和catch模块中不存在return语句**，那么运行完try和catch模块中的代码后再运行finally中的代码。

**2、如果try和catch模块中存在return语句**，那么在运行return之前会运行finally中的代码，
 (1). 如果finally中存在return语句，则返回finally的return结果，代码运行结束。
 (2). 如果finally不存在return语句，则返回try或catch中的return结果，代码运行结束。

**3、如果try和catch模块中存在throw语句**，那么在catch运行throw之前会运行finally中的代码。
 (1). 如果finally中存在return语句，则返回finally的return结果，代码运行结束。
 (2). 如果finally不存在return语句，则运行catch中的throw语句，代码运行结束。

#### 事件循环

##### 执行机制

![执行机制](http://jay_ohhh.gitee.io/imagehosting/JS/执行机制.png)



> Event Loop中，每一次循环称为tick

翻开规范《ECMAScript® 2015 Language Specification》，找到事件循环 8.1.6 [Event loops](https://html.spec.whatwg.org/multipage/webappapis.html#event-loops)。

规范中中提到，一个浏览器环境，只能有一个事件循环，而一个事件循环可以多个任务队列，每个任务都有一个任务源（Task source）。

相同任务源的宏任务，只能放到同一个宏任务队列中。

如 setTimeout setInterval setimmediate 会在一个task queue，网络IO会在一个task queue，用户的事件会在一个task queue。

又举了一个例子说，客户端可能实现了一个包含鼠标键盘事件的任务队列，还有其他的任务队列，而给鼠标键盘事件的任务队列更高优先级，例如75%的可能性执行它。这样就能保证流畅的交互性，而且别的任务也能执行到了。同一个任务队列中的任务必须按先进先出的顺序执行，但是不保证多个任务队列中的任务优先级，具体实现可能会交叉执行。

**结论**

一个事件循环可以有多个宏任务队列，队列之间可有不同的优先级，同一队列中的任务按先进先出的顺序执行，但是不保证多个宏任务队列中的任务优先级，具体实现可能会交叉执行。

##### 宏任务、微任务、不同任务队列的优先级

![宏任务和微任务的关系](http://jay_ohhh.gitee.io/imagehosting/JS/宏任务和微任务的关系.png)

> 当我们调用resolve()或reject()的时候，触发promise.then(...)实际上是一个异步操作，这个promise.then(...)并不是在resolve()或reject()的时候就立刻执行的，而也是要重新进入任务队列排队的，不过能直接在当前的事件循环新的执行栈中被取出执行（不用等下次事件循环）。

在JS引擎中，我们可以按性质把任务分为两类，**macrotask**（宏任务）和 **microtask**（微任务）。

**macrotask queue**可以有多个，相同任务源的宏任务，只能放到同一个宏任务队列中。

如 setTimeout setInterval setimmediate 会在一个task queue，网络IO会在一个task queue，用户的事件会在一个task queue。

**microtask queue** 只有一个，因为microtask queue是在当前task queue执行时生成的，在下一个task queue执行时会清空并重新生成。

**优先级**

1、macrotask（按优先级顺序排列）：script(你的全部JS代码，“同步代码”）、 setTimeout、setInterval、setImmediate、 I/O、UI rendering

2、microtask（按优先级顺序排列）：process.nextTick、Promise（这里指浏览器原生实现的 Promise）, Object.observe、 MutationObserver

> microtask任务 也可以生成新的 microtask任务 并且插入到同样的队列中（插入当前microtask）并且在同一个 tick 里执行

3、JS引擎首先从macrotask queue中取出第一个任务，执行完毕后，将microtask queue中的所有任务取出，按顺序全部执行；

4、然后再从macrotask queue（宏任务队列）中取下一个，执行完毕后，再次将microtask queue（微任务队列）中的全部取出；

5、循环往复，直到两个queue中的任务都取完。



所以，**浏览器环境中**，js执行任务的流程是这样的：

1、第一个事件循环，先执行script中的所有同步代码（即 macrotask 中的第一项任务）

2、再取出 microtask 中的全部任务执行（先清空process.nextTick队列，再清空promise.then队列）

3、下一个事件循环，再回到 macrotask 取其中的下一项任务

4、再重复2

5、反复执行事件循环…



**宏任务**

|                       | 浏览器 | Node |
| --------------------- | ------ | ---- |
| setTimeout            | √      | √    |
| setInterval           | √      | √    |
| setImmediate          | x      | √    |
| requestAnimationFrame | √      | x    |

**微任务**

|                            | 浏览器 | Node |
| -------------------------- | ------ | ---- |
| process.nextTick           | x      | √    |
| MutationObserver           | √      | x    |
| Promise.then catch finally | √      | √    |



**eg1**

```js
setTimeout(() => {
  console.log('内层宏事件3')
}, 0)
console.log('外层宏事件1')

new Promise(resolve => {
  console.log('外层宏事件2')
  resolve()
})
  .then(() => {
    setTimeout(() => {
      console.log('then-setTimeout-1')
    })
    console.log('微事件1')
  })
  .then(() => {
    setTimeout(() => {
      console.log('then-setTimeout-2')
    })
    console.log('微事件2')
  })

```

我们看看打印结果

```
外层宏事件1
外层宏事件2
微事件1
微事件2
内层宏事件3
then-setTimeout-1
then-setTimeout-2
```

• 首先浏览器执行js进入第一个宏任务进入主线程, 遇到 setTimeout 分发到宏任务Event Queue中（setTimeout优先级比script低）

• 遇到 console.log() 直接执行 输出 外层宏事件1

• 遇到 Promise， new Promise 直接执行 输出 外层宏事件2

• 执行then 被分发到微任务Event Queue中

• 第一轮宏任务执行结束，开始执行第一个微任务，遇到setTimeoutout，分发到宏任务Event Queue中，然后打印 '微事件1' 

• 开始执行第二个微任务，遇到setTimeoutout，分发到宏任务Event Queue中，然后打印 '微事件2' 

• 第一轮微任务执行完毕，执行第二轮宏事件，在同一宏任务队列中按照顺序打印 '内层宏事件3'、'then-setTimeout-1'、'then-setTimeout-2'

**NodeJS引擎中**：

1、先执行script中的所有同步代码，过程中把所有异步任务压进它们各自的队列（假设维护有process.nextTick队列、promise.then队列、setTimeout队列、setImmediate队列等4个队列）

2、按照优先级（process.nextTick > promise.then > setTimeout > setImmediate），选定一个  不为空 的任务队列，按先进先出的顺序，依次执行所有任务，执行过程中新产生的异步任务继续压进各自的队列尾，直到被选定的任务队列清空

3、重复2...



#### Object的方法

##### Object.keys()

**for...in** 循环和**Object.keys**方法都是获取对象可枚举属性

区别：前者包括对象继承自原型对象的属性，而后者只包括对象本身的属性。如果需要获取对象自身的所有属性，不管enumerable的值，可以使用**Object.getOwnPropertyNames**方法。

##### Object.assign()

**Object.assign()** 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```js
Object.assign(target, source1, source2, ...)
```

参数

- target：目标对象
- sources：源对象

返回值

- 目标对象

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

##### Object.setPrototype

设置一个指定的对象的原型 到另一个对象或 null。

```
Object.setPrototypeOf(obj, prototype)
```

参数

- obj ：指定对象
- prototype：原型(一个对象 或 null)

##### Object.freeze()

**冻结**一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。`freeze()` 返回和传入的参数相同的对象。

```js
Object.freeze(obj)
```

参数

- obj：要被冻结的对象

返回值

- 被冻结的对象（即返回原先的对象，但是被冻结了）

##### Object.defineProperty(obj,prop,desc)

- obj：需要定义属性的当前对象

- prop：当前需要定义的属性名
- desc：属性描述符

desc属性描述符以对象形式属性

- value：设置属性的值，默认为undefined
- writable：值是否可以重写，默认为false
- enumerable：属性是否可以被枚举，默认为false
- configurable：属性是否可以被删除或是否可以被再次修改特性（再次使用Object.defineProperty修改配置属性描述符），默认为false
- set：作为该属性的 是 setter 函数，默认为undefined
- get：作为该属性的 getter 函数，默认为undefined

> 定义对象时就存在的属性，或者通过 **对象.属性** 这种形式的增加的属性，writable、enumerable、configurable默认都是true。

> 属性描述符： value、writable
>
> 存取描述符：set、get
>
> 属性描述符和存取描述符不能同时使用

> get：当访问该属性时，会调用此函数。执行时不传入任何参数，但是会传入 this 对象（由于继承关系，这里的this并不一定是定义该属性的对象）。继承关系是指，我们有可能get的是原型对象或父类的属性。get属性（函数）的返回值会被用作属性的值，但并不是属性的value值。



#### 解构赋值

通过**解构赋值,** 可以将属性/值从对象/数组中取出,赋值给其他变量。

对象解构时：将“=”右边的属性解构出来，让将属性值赋值给“=”的左边的变量，按照属性名配对赋值的，所以左边的变量名要等于右边的属性名，当然左边的变量名也可以取别名，例如name：Name，将name取别名为Name。（先在右边找相应的属性，然后再根据需要取别名）。

对数组元素（数组是对象，数组元素是属性）进行解构赋值时，多个变量要用[...]括起来，对对象属性进行解构赋值时，多个变量要用{...}括起来。

```js
let { name:Name,age } = { name:'swr',age:28 }
let [x, y, z] = ['hello', 'JavaScript', 'ES6']; 
```

注意嵌套层次和位置要保持一致：

```js
let person = {
    name: '小明',
    address: {
        city: 'Beijing',
        street: 'No.1 Road',
        zipcode: '100001'
    }
};
var {name, address: {city, zipcode:zip}} = person;
console.log(city); // 'Beijing'
console.log(zip); // '100001'
```

如果数组本身还有嵌套（多维数组），也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致：

```js
let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
x; // 'hello'
y; // 'JavaScript'
z; // 'ES6'
```

解构赋值还可以忽略某些元素：

```js
let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对z赋值第三个元素
z; // 'ES6'
```

解构赋值还可以使用默认值：

```js
let { name,age=18 } = { name:'swr'}
```

有些时候，如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误：

```js
// 声明变量
var x, y;
// 解构赋值
{x, y} = { name: '小明', x: 100, y: 200};// 语法错误
```

这是因为JavaScript引擎把 { 开头的语句当作了块处理，解决方法是用小括号括起来：

```js
// 声明变量
var x, y;
({x, y} = { name: '小明', x: 100, y: 200});//相当于匿名对象解构赋值
name; //小明
x; //100
y; //200
```

#### Proxy

**Proxy** 对象用于创建一个对象的代理，从而实现基本操作的拦截和自定义（如属性查找、赋值、枚举、函数调用等）。

```js
const p = new Proxy(target, handler);
```

**参数**

- target：需要使用`Proxy`包装的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）
- handler：处理器对象，包含各种行为的捕获器（属性），捕获器中的函数定义执行一个操作时代理的行为

**返回值**

返回目标对象的代理对象

##### [handler对象的方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)

https://segmentfault.com/a/1190000019675719?utm_source=tag-newest

在TypeScript中定义了可被代理得基本操作，一共14个（Reflect对象也是具有这14个方法），如下：

```ts
interface ProxyHandler<T extends object> {
    getPrototypeOf? (target: T): object | null;
    setPrototypeOf? (target: T, v: any): boolean;
    isExtensible? (target: T): boolean;
    preventExtensions? (target: T): boolean;
    getOwnPropertyDescriptor? (target: T, p: PropertyKey): PropertyDescriptor | undefined;
    has? (target: T, p: PropertyKey): boolean;
    get? (target: T, p: PropertyKey, receiver: any): any;
    set? (target: T, p: PropertyKey, value: any, receiver: any): boolean;
    deleteProperty? (target: T, p: PropertyKey): boolean;
    defineProperty? (target: T, p: PropertyKey, attributes: PropertyDescriptor): boolean;
    enumerate? (target: T): PropertyKey[]; //废弃
    ownKeys? (target: T): PropertyKey[];
    apply? (target: T, thisArg: any, argArray?: any): any;
    construct? (target: T, argArray: any, newTarget?: any): object;
}
```

需要注意的是，如果一个属性不可配置（configurable）且不可写（writable），则 Proxy 不能修改该属性。

Proxy只代理对象外层属性。

##### 递归代理对象内部对象

```js
let obj={a:1,b:{c:2}};
let handler={
  get:function(obj,prop){
    const v = Reflect.get(obj,prop);
    if(v !== null && typeof v === 'object'){
      return new Proxy(v,handler);//代理内层
    }else{
      return v; // 返回obj[prop]
    }
  },
  set(obj,prop,value){
    obj[prop] = value
    return true;
    // 或 return Reflect.set(obj,prop,value) // 设置成功返回true
  }
};
let p=new Proxy(obj,handler);

console.log(p.a)//会触发get方法
console.log(p.b.c)//会先触发get方法获取p.b，然后触发返回的新代理对象的.c的set。

p.b.c = 3
console.log(p.b.c)
```

##### Proxy对象和原始对象

改变了proxy实例的属性的值，被代理的原对象的属性的值也会改变。

```js
let target = {};
let p = new Proxy(target, {});

p.a = 1;   // 操作转发到目标

console.log(target.a);    // 1  操作已经被正确地转发

target.a=2;
console.log(p.a)//2

console.log(target===p)//false
```

##### Reflect

**Reflect**是一个内置的对象，它提供拦截 JavaScript 操作的方法。这些方法与[proxy handlers](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)的方法相同。Reflect不是一个函数对象，因此它是不可构造的。

**eg**

```js
const duck = {
  name: 'Maurice',
  color: 'white',
  greeting: function() {
    console.log(`Quaaaack! My name is ${this.name}`);
  }
}

Reflect.has(duck, 'color');
// true
Reflect.has(duck, 'haircut');
// false
```

##### Proxy和Object.defineProperty

1、Proxy使用上比Object.defineProperty方便的多。

2、Proxy代理整个对象，Object.defineProperty只代理对象上的某个属性。

3、对象上定义新属性时，Proxy可以监听到，Object.defineProperty监听不到。

4、数组新增删除修改时，Proxy可以监听到，Object.defineProperty监听不到。

5、Proxy不兼容IE，Object.defineProperty不兼容IE8及以下。

#### 运算符 & 操作符

##### [运算符优先级](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

##### void

void 是一元运算符，它可以出现在任意类型的操作数（表达式）之前执行操作数（表达式），却忽略操作数的返回值，返回undefined。用法：

```js
var a = b = c = 2;  //定义并初始化变量的值
var d = void (a -= (b *= (c += 5)));  // 执行void运算符，并把返回值赋予变量
console.log(a);  //返回-12
console.log(b);  //返回14
console.log(c);  //返回7
console.log(d);  //返回undefined
```

#### window

##### window.requestAnimationFrame & window.cancelAnimationFrame

**window.requestAnimationFrame()** 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。

> 若你想在浏览器下次重绘之前继续更新下一帧动画，那么回调函数自身必须再次调用window.requestAnimationFrame()

```js
window.requestAnimationFrame(callback);
```

**参数**

callback：下一次重绘之前更新动画帧所调用的函数

**返回值**

一个 `long` 整数，请求 ID，可以传这个值给 [`window.cancelAnimationFrame()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/cancelAnimationFrame) 以取消回调函数。

###### 优点

（1）requestAnimationFrame会把每一帧中的所有DOM操作集中起来，在一次重绘或回流中就完成，并且重绘或回流的时间间隔紧紧跟随浏览器的刷新频率

（2）在隐藏或不可见的元素中，requestAnimationFrame将不会进行重绘或回流，这当然就意味着更少的CPU、GPU和内存使用量

（3）requestAnimationFrame是由浏览器专门为动画提供的API，在运行时浏览器会自动优化方法的调用，并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了CPU开销。

（4）回调的次数通常是每秒60次，但大多数浏览器通常匹配 W3C 所建议的刷新频率，避免刷新率过高，浪费性能：

- 浏览器会努力确保每秒 60 帧（60fps）。然而，如果浏览器无法确保，那么自然会限制每秒的帧数。例如，某个设备可能只能处理每秒 30 帧，所以每秒只能得到 30 帧。使用 requestAnimationFrame 来节流是一种有用的技术，它可以防止在一秒中进行 60 帧以上的更新。如果一秒钟内完成 100 次更新，则会为浏览器带来额外的负担，而用却户无法感知到这些工作。

- 采用系统时间间隔(大多浏览器刷新频率是 60Hz，相当于 1000ms/60≈16.6ms)，保持最佳绘制效率，不会因为间隔时间过短，造成过度绘制，增加开销；也不会因为间隔时间太长，使用动画卡顿不流畅，让各种网页动画效果能够有一个统一的刷新机制。

###### requestAnimationFrame 和 setTimeout

与setTimeout相比，requestAnimationFrame最大的优势是由系统来决定回调函数的执行时机。

换句话来说，requestAnimationFrame的步伐跟着系统的刷新步伐走。它能保证回调函数在屏幕每一次的刷新间隔中只被执行一次，这样就不会引起丢帧现象，也不会导致动画出现卡顿的问题。 

###### 兼容性封装

```js
function() {
  let lastTime = 0
  if (!window.requestAnimationFrame) {
    window.requestAnimationFrame =
      window.webkitRequestAnimationFrame ||
      window.msRequestAnimationFrame ||
      window.mozRequestAnimationFrame ||
      window.oRequestAnimationFrame ||
      function (callback) {
        let currentTime = new Date().getTime()
        // 16.7ms是1000/60,每一帧需要的毫秒数  
        // x = (currentTime - lastTime) 是当前时间 - 上次时间，意味着已经过了 x ms
        // 16.7 - x 是想让setTimeout的回调函数callback在16.7ms之后执行 
        let timeToCall = Math.max(0, 16.7 - (currentTime - lastTime))
        let timer = window.setTimeout(function () {
          callback(currentTime + timeTocall)
        }, timeToCall)
        lastTime = currentTime + timeToCall
        return timer
      }
  }
  if (!window.cancelAnimationFrame) {
    window.cancelAnimationFrame =
      window.webkitCancelAnimationFrame ||
      window.webkitCancelRequestAnimationFrame ||
      window.mozCancelAnimationFrame ||
      window.mozCancelRequestAnimationFrame ||
      window.msCancelAnimationFrame ||
      window.msCancelRequestAnimationFrame ||
      window.oCancelAnimationFrame ||
      window.oCancelRequestAnimationFrame ||
      function (id) {
        window.clearTimeout(id)
      }
  }
} 
```



#### 作用域

作用域即执行环境，每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。在 web 浏览器中，全局执行环境被认为是 window 对象。且每个函数都有自己的执行环境，在进入函数时入栈，在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。

当代码在一个环境中执行时，会创建变量对象的一个作用域链，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终是当前执行的代码所在环境的变量对象，最后端，始终是全局执行环境的变量对象。

JavaScript 代码运行时，会产生一个全局的上下文环境（context，又称运行环境），包含了当前所有的变量和对象。然后，执行函数（或块级代码）的时候，又会在当前上下文环境的上层，产生一个函数运行的上下文，变成当前（active）的上下文，由此形成一个上下文环境的堆栈（context stack）。

这个堆栈是“后进先出”的数据结构，最后产生的上下文环境首先执行完成，退出堆栈，然后再执行完成它下层的上下文，直至所有代码执行完成，堆栈清空。

但是Generator函数是例外的：

Generator 函数不是这样，它执行产生的上下文环境，一旦遇到`yield`命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态。等到对它执行`next`命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。

#### 垃圾回收机制

JS 中最常用的跟踪方法是标记清除，当函数执行完后，就会给局部变量打上“离开环境”的标记（除了闭包），在下一次垃圾回收时间到来时就会清除这一块内存，手动将一个有值的变量赋值为 null，也是让这个值离开环境，也可以释放内存。

还有一种跟踪方法是引用计数，这会引起循环引用的问题，但现在所有的浏览器都使用了标记清除式的垃圾回收机制，所以不用考虑这个问题了。

#### console

console.*方法是没有相关规定是同步还是异步的，因此，不同的浏览器和JavaScript 环境可以按照自己的意愿来实现。

在webkit内核浏览器中：

```js
var a = {
  index: 1
};
console.log( a ); //  输出 { index: 2 }
a.index++;
```

在调试的过程中遇到对象在console.log(..) 语句之后被修改，可你却看到了意料之外的输出结果，要意识到这可能是这种 I/O 的异步化造成的。

如果遇到这种少见的情况，最好的选择是在JavaScript 调试器中使用断点，而不要依赖控制台输出。次优的方案是把对象序列化到一个字符串中，以强制执行一次“快照”，比如通过JSON.stringify()。

一般对于基本类型number、string、boolean、null、undefined的输出是可信的。但对于Object等引用类型来说，则就会出现上述异常打印输出。

 

在Node环境中，console.*方法是严格同步的，不会出现上述Object等引用类型异常打印输出的情况。

#### 术语

##### polyfill

polyfill或polyfiller是一段代码（或插件），用于抹平浏览器之间的 API 差异，在旧的浏览器上支持新的特性。

###### 动态polyfill

具体的使用方法非常简单，只需要外链一个 js：

```xml
<script src="https://cdn.polyfill.io/v2/polyfill.min.js"></script>
```

当然这样是加载全部的 polyfill，实际上你可能并不需要这么多，比如你只需要 Map/Set 的话：

```xml
<script src="https://cdn.polyfill.io/v2/polyfill.min.js?features=Map,Set"></script>
```

polyfill.io 的原理，它会根据你的浏览器 UA 头，判断你是否支持某些特性，从而返回给你一个合适的 polyfill。对于最新的 Chrome 浏览器来说，不需要任何 polyfill，所以返回的内容为空。对于 iOS Safari 来说，需要 URL 对象的 polyfill，所以返回了对应的资源。

##### DOMString

是一个UTF-16字符串。由于JavaScript已经使用了这样的字符串，所以DOMString 直接映射到 JavaScript String。

##### 函数签名

一个函数签名 (或类型签名，或方法签名) 定义了 函数 或 方法 的输入与输出。

一个签名可以包括：

- 参数 及参数的 类型

- 一个返回值及其类型

- 可能会抛出或传回的 异常

- 有关 面向对象 程序中方法可用性的信息 (例如关键字 public、static 或 prototype)。

##### falsy

在 JavaScript 中只有 8 **个** **falsy** 值：

| `false`                                                      | [false](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Future_reserved_keywords_in_older_standards) 关键字 |      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| 0                                                            | 数值 [zero](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type) |      |
| -0                                                           | 数值 负 [zero](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type) |      |
| 0n                                                           | 当 [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) 作为布尔值使用时, 遵从其作为数值的规则. `0n` 是 *falsy* 值. |      |
| "", '', ``                                                   | 这是一个空字符串 (字符串的长度为零). JavaScript 中的字符串可用双引号 `**""**`, 单引号 `''`, 或 [模板字面量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) `` 定义。 |      |
| [null](https://developer.mozilla.org/zh-CN/docs/Glossary/Null) | [null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null) - 缺少值 |      |
| [undefined](https://developer.mozilla.org/zh-CN/docs/Glossary/undefined) | [undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) - 原始值 |      |
| [NaN](https://developer.mozilla.org/zh-CN/docs/Glossary/NaN) | [NaN ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)- 非数值 |      |