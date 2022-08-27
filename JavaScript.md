https://wangdoc.com/es6/index.html

#### 数据类型

原始类型：undefined、null、string、number、boolean、symbol（ES6）、bigInt（ES2020）

引用类型（复杂数据类型）：对象(Object)、数组(Array)、函数(Function)等



##### 判断

###### typeof

可以判断出string、number、boolean、symbol 、bigInt

但判断 typeof(null) 时值为 'object'; 判断数组和对象时值均为 'object'，判断函数为'function'

`a=== undefined` vs. `typeof a === 'undefined'` 这两种判断中，谁更好？先说结论，使用 typeof 的方法更好。最主要的原因有两点：

- `a === undefined` 的形式，你不能确保 a 被声明过，当 a 没声明过时，程序直接报错，而使用 typeof 可以用来判定一个变量是否声明过，这也是我们常用的 typeof window ... typeof global ... typeof self ... this 这个办法来搞定不同运行时环境下的处理。
- undefined 竟然是 window 的属性，按理来说作为 js 语言的基础类型，提供和 null 一样的关键字应该由语言解释器来做吧，但是在运行时中（浏览器），undefined 和 null 完全是两个层面的东西，null 是内置于解析器的空指针符号，而 undefined 是挂在 window 上的全局变量，竟然是挂在 window 上的变量，那么每次使用 var === undefined 时，实际上会去 window 上读取变量，读取的多了，也就让我们开始遐想有没有办法通过不断调用 undefined 变量使系统崩溃。不过值得庆幸的是，undefined 是不能重新赋值的，undefined = 1 虽然不会报错，但是没效果。而执行 null = 1 则会直接报错。就是这么奇妙。

> undefined in window === true



###### Object.prototype.toString.call()

```js
Object.prototype.toString.call('') // '[object String]'
Object.prototype.toString.call(1) // '[object Number]'
Object.prototype.toString.call([]) // '[object Array]'
Object.prototype.toString.call(undefined) //'[object Undefined]'
Object.prototype.toString.call(null) // '[object Null]'
Object.prototype.toString.call(()=>{}) // '[object Function]'
```



###### NaN 如何判断

NaN是一个特殊的数字值(typeof *NaN*的结果为number)，是not a number的缩写，表示非数字的值。

利用`NaN`自身永不相等于自身这一特征

```js
var myIsNaN = function(value) {
    var n = Number(value);
    return n !== n;
};
```



#### void 0和undefined的区别

void其实是javascript中的一个函数，接受一个参数，返回值永远是undefined。可以说，使用void目的就是为了得到javascript中的undefined。So `void 0` is a correct and standard way to produce `undefined`.

使用 void 0 的原因

- 使用void 0比使用undefined能够减少3个字节。

  ```js
  "undefined".length"; //9  
  "void 0".length"; //6  
  ```

- undefined并不是javascript中的保留字，我们可以使用undefined作为变量名字，然后给它赋值。这样就会导致不一定会返回undefined，而void 0却永远可以保证。



#### var、let、const

如果不用var、let、const声明变量，则会声明为全局变量，在浏览器环境下，相当于在window上增加一个属性。

##### var

如果在方法中声明，则为局部变量（local variable）；如果是在全局域中声明，则为全局变量。

var不支持块级作用域。

##### let

通过 `let` 声明的变量直到它们的定义被执行时才初始化。在初始化之前，该变量处在一个自块顶部到初始化处理的“暂存死区”中（temporal dead zone，简称 TDZ）。

不能在同一作用域下用 let 重复声明变量。

> let会不会变量提升，官方是没有明确说明的，也许底层机制上是提升了，但是形式是没有提升，无需纠结。

##### const

const和let唯一不同的是，const声明的时候必须进行初始化赋值，而且只能赋值一次。

若存储的值不需要变化则建议用const，因为const常量是不变的，不需要引擎实时监控，效率更高。



#### 值引用和地址引用

实际上函数参数（s）也是一个变量

如果是值则直接拷贝，函数内外对值的改变都不会收到彼此的影响

如果是地址则拷贝地址如果直接修改地址所指向的对象，则原对象会受到影响

参数是值引用，函数内的改变不会影响到函数外的

```js
function a(s){
    s=2
}
let b = 1
a(b)
console.log(b) // 1
```

参数是值引用，则函数外的改变不会影响到函数内的

```js
// 闭包
function a(s){
    return function(){
        console.log(s)
    }
}
let b = 1
let c = a(b)
b = 2
c() // 1
```



#### 变量提升和函数提升

ES6新增了块级作用域。块级作用域由 { } 包括，if 语句和 for 语句里面的{ }也属于块作用域。

变量提升是针对**var**和 **function**而言。

var不支持块级作用域。

class中不存在变量提升和函数提升。

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

var不支持块级作用域，同一作用域重复声明了两次，提升的第二次声明会被忽略，仅用于赋值。

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

等价于

```js
// 全局作用域
var a = 1;
var a; // 第二次声明会被忽略
if(true){
    console.log(a); // 1
    a = 2;
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
function fn(){}
var fn = 1 // 第二次声明会被忽略，仅用于赋值
console.log(fn) // 1
```

等价于

```js
function fn() {}
var fn // 第二次声明会被忽略，
console.log(fn) // function fn(){}
fn = 1 // 赋值
console.log(fn) // 1
```

##### 总结

1、所有的声明都会提升到作用域的最顶上去。

2、同一个变量只会声明一次，其他的会被忽略掉。

3、函数声明的优先级高于变量声明的优先级，同名称则var被忽略。

#### this指向

> 作用域的this跟对象相关联
>
> 对象没有this，this是在函数里指向对象的，this是函数的隐式参数。

1、非严格模式，普通函数的this指向全局对象，普通函数是在全局对象里的函数，调用普通函数，其实就是全局对象在调用。

严格模式下的全局作用域中普通函数的this是undefined。

不管是不是严格模式，全局作用域的this都是指向全局对象。全局作用域实际上是在一个全局函数内的作用域。

2、对于方法调用：谁调用方法，this指向谁（方法是写在对象里的函数）

3、构造函数里的this指向它的实例对象。

4、类的constructor的this指向实例对象，类的方法的this指向这个方法的调用者。

5、闭包内层函数和外层函数的this，决定于代码执行时，它们的函数体被引用在哪个作用域，即this会指向该函数运行时所在的作用域所关联的对象，普通函数和方法被调用时亦如此。

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

6、定时器setTimeout(function(){})里回调函数的this指向全局对象，这是因为function( ){}调用的代码运行在全局作用域上，而不是setTimeout所在的执行环境里。

如果定时器setTimeout(( )=>{})里是箭头函数，则箭头函数里的this指向setTimeout的外层函数的this所指向的对象。那么如何找这个setTimeout的外层函数呢？可以找( ){ }这样的结构就可以了

即使是在严格模式下，setTimeout()的function里面的this仍然默认指向window对象， 并不是undefined。

7、箭头函数本身是没有this的，它的this是根据外层非箭头函数的this来决定的，它的外层函数可以这样找，向上找到( ){ }这样的函数体结构。

8、嵌套函数的this指向，嵌套的函数不会从调用它的函数中继承this。

（1）如果嵌套函数作为方法调用，其this的值指向调用它的对象。

（2）如果嵌套函数作为函数调用（只要不是方法调用，例如在方法内作为函数调用、在函数内作为函数调用），其this值不是全局对象（非严格模式下）就是undefined（严格模式下）。为什么指向全局对象或者undefined？因为this是指向函数所在的作用域对象，this不能指向函数。

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

#### 原型对象 prototype

prototype：构造函数的原型对象

`__proto__`：对象原型。

实例对象的对象原型指向构造函数的原型对象：

```js
new Star().__proto__ === Star.prototype
```

为节省内存和时间，一般情况下，公共属性定义到构造函数里面，公共方法定义到原型对象里面。

访问一个对象的属性或方法时是通过原型链进行查找，原型链的尽头是null。

任何对象（包括函数、构造函数、数组等等）都有`__proto__`属性，是个非标准属性，只是为查找机制提供一个方向，实际开发中不应该使用这个属性。

![原型链](http://jay_ohhh.gitee.io/imagehosting/JS/原型链.png)

##### ES5实现继承

```js
function Parent(value) {
  this.value = value
}
function Child(value) {
  Parent.call(this, value)
}
Child.prototype = new Parent()
Child.prototype.constructor = Child

const c = new Child(1)
console.log(c.value) // 1
```

#### 块级作用域

```js
let a = 1;
switch (a) {
  case 1:
    const b = 10; // error: 无法重新声明块范围变量“b”
    break;
  case 2:
    const b = 11; // error: 无法重新声明块范围变量“b”
    break;
  default:
    break;
}

// correct
let a = 1;
switch (a) {
  case 1: {
    const b = 10;
    break;
  }
  case 2: {
    const b = 11;
    break;
  }
  default:
    break;
}
```



#### 词法作用域

JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。

因为 JavaScript 采用的是词法作用域，函数的作用域在函数定义的时候就决定了。

词法作用域和this指向是两回事。

词法作用域是自身的作用域

this指向强调函数运行时所在的作用域关联的对象。

```js
var value = 1;

function foo() {
    console.log(value);
}

function bar() {
    var value = 2;
    foo();
}

bar(); // 结果是 1
```

```js
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f();
}
checkscope(); // 'local scope'
```

```js
var scope = "global scope";
function checkscope(){
    var scope = "local scope";
    function f(){
        return scope;
    }
    return f;
}
checkscope()(); // 'local scope'
```

原因也很简单，因为JavaScript采用的是词法作用域，函数的作用域基于函数创建的位置。

而引用《JavaScript权威指南》的回答就是：

JavaScript 函数的执行用到了作用域链，这个作用域链是在函数定义的时候创建的。嵌套的函数 f() 定义在这个作用域链里，其中的变量 scope 一定是局部变量，不管何时何地执行函数 f()，这种绑定在执行 f() 时依然有效。

#### 作用域链

如果函数中还有函数，那么在这个作用域中又可以诞生一个作用域，压入当前执行栈的顶部；根据在**[内部函数可以访问外部函数变量]**的这种机制，用链式查找决定哪些数据能被内部函数访问，称为作用域链。



#### 构造函数

```js
function Star(name,age){
	this.name = name
	this.age = age
	this.sing = ()=>{}
}
// 
// 我们可以把那些不变的方法定义在原型上，节省开销
Star.prototype.dance = ()=>{}
```

实际上

- 在创建构造函数Start的时候就会创建对应的原型对象 Star.prototype

- 构造函数Start本身也是个实例对象，同理，我们也可以把es6的类看成构造函数，也是个实例对象

  ```js
  function Star(){}
  Star instanceof Function // true
  // isPrototypeOf: 判断调用对象是否在参数对象的原型链上
  Function.prototype.isPrototypeOf(Star) // true
  
  class A{}
  class B extends A{}
  A instanceof Function // true
  Function.prototype.isPrototypeOf(A) // true
  A.isPrototypeOf(B) // true
  ```





如果我们修改了原来的原型对象，给原型对象赋值的是一个对象，则必须手动利用constructor指回原来的构造函数

```js
// error：将另一个对象赋值给Star.prototype这个指针，导致Star的原型对象发生改变
Star.prototype = { sing: function(){} }
```

```js
// correct
Star.prototype = { 
  constructor: Start,
  sing: function(){} 
} 
```



任何对象（包括函数、构造函数、数组等等）都有`__proto__`属性，是个非标准属性，只是为查找机制提供一个方向，实际开发中不应该使用这个属性。



##### new 在执行时会做四件事

- 在内存中新建一个对象
- 让this执行该对象
- 执行构造函数里的代码，给这个对象添加属性和方法
- 返回这个新对象（构造函数不需要显式return）



##### 静态成员、实例成员

静态成员是挂载在函数或类上，并非挂载在原型上。

实例成员：构造函数或类中this上添加的成员，只能由实例化的对象来访问

静态成员：构造函数本身上添加的成员 或 类中`static`修饰的成员，只能由构造函数或类本身来访问

 

构造函数定义时内部没有静态成员。

只能通过以下的方式给构造函数添加静态属性或静态方法。

```js
function a() {}
a.Sex = 1
```



##### 继承

```js
function Parent(value) {
  this.value = value
}
function Child(value) {
  Parent.call(this, value)
}
Child.prototype = new Parent()
Child.prototype.constructor = Child
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

> 闭包可以让你在一个内层函数中访问到其外层函数的作用域的状态

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

// 这样也是闭包
function foo(){
  var local = 1
  function bar(){
    local++
    return local
  }
  obj.bar = bar
}
let obj = {}
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

箭头函数没有`this`，`super`、`arguments`，`new.target`、`prototype`。

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

**词法作用域**

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

柯里化是一种将使用多个参数的一个函数转换成一系列使用一个参数的函数的技术

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

**实现**

```js
function curry(fun) {
  return function curried(...args) {
    // 关键知识点：function.length 用来获取函数的形参个数
    // 补充：arguments.length 获取的是实参个数
    if (args.length >= fun.length) {
      return func.apply(this, args)
    }
    return function (...args2) {
      // 实参个数小于获取函数的形参个数时，实参不断向后拼接实参，直至实参个数等于形参个数
      return curried.apply(this, args.concat(args2))
    }
  }
}

// 测试
function sum(a, b, c) {
  return a + b + c
}
const curriedSum = curry(sum)
console.log(curriedSum(1, 2, 3))
console.log(curriedSum(1)(2, 3))
console.log(curriedSum(1)(2)(3))
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

通常需要在立即执行函数的最前面加上分号 `;`

```js
;(function(a){
    console.log(a)
})(123)
```

#### Symbol

ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入`Symbol`的原因。

ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。Symbol 值通过`Symbol`函数生成。

注意，`Symbol`函数前不能使用`new`命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。

`Symbol`函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

```javascript
let s1 = Symbol('foo');
let s2 = Symbol('bar');

s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
```

生成的Symbol值不是字符串，点运算符后面总是字符串，因此作为属性名时，不能用点运算符。

Symbol 值作为属性名时，该属性是公有属性不是私有属性，可以在类的外部访问。

但是不会出现在 for...in 、 for...of 的循环中，也不会被 Object.keys() 、 Object.getOwnPropertyNames() 返回。如果要读取到一个对象的 Symbol 属性，可以通过 Object.getOwnPropertySymbols() 和 Reflect.ownKeys() 取到。

#### for in 和 for of

1、for in遍历会遍历到自身和原型链上的可枚举的索引、属性、方法，而且不按实际顺序，更适合遍历对象。

2、for in遍历对象的顺序不一定按照实际对象的内部顺序，遍历数组是按照数组的索引遍历，遍历数组的索引为字符串型数字，但是通过一些算术运算符实现隐式转换进行运算。

3、for in如果不想遍历原型方法和属性的话，可以在循环内部判断一下,hasOwnProperty方法可以判断某属性是否是该对象的实例属性。

4、for of可以成功遍历数组所有**索引**的值,而不是索引，不会遍历原型。for of只能用于可迭代对象上。for of是迭代器，只能用来遍历可迭代对象。可迭代对象有：String, Array, array-like objects（类数组）(e.g., arguments or NodeList), TypedArray, Map, Set, and user-defined iterables

5、forEach不支持break, continue, return等。forEach是用来遍历数组等iterable（可迭代）对象，遍历数组的索引为数值型。

for ... in循环由于历史遗留问题，它遍历的实际上是对象的属性名称。一个Array数组实际上也是一个对象，它的每个元素的索引被视为一个属性。

当我们手动给Array对象添加了额外的属性后，for ... in循环将带来意想不到的意外效果：

```js
var a = ['A', 'B', 'C'];
a.name = 'Hello'; //对象.属性操作后，enumerable是true
for (let x in a) {
  console.log(x); // '0', '1', '2', 'name'
}
console.log(a.length); // 3
```

for ... in循环将把name包括在内，但Array的length属性却不不变。

for ... of循环则完全修复了这些问题（循环不包含额外增加的属性name），而且Array的length属性也不变，它只循环集合本身的元素：

```js
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (let x of a) {
	console.log(x); // 'A', 'B', 'C'
}
```

#### 标签函数

标签函数是一种语法糖，后面直接跟模板字符串，可以对模板字符串中的字符和变量进行拆分，不需要通过 () 调用。

```javascript
let a = 6
let b = 9
let sum = 15
function tagTemplate(strings, aVariable, bVariable, sumVariable) {
  console.log(strings)        // ‘ 67、，遍历strings，不会遍历raw
  console.log(aVariable)      // 6
  console.log(bVariable)      // 9
  console.log(sumVariable)    // 15
}

tagTemplate`${a} + ${b} = ${sum}`

let c = '张三'
let d = 24
function tagTemplate(strings, cVariable, dVariable, notVariable) {
  console.log(strings)        // ['我叫', '，今年', '岁了', raw: Array(3)]
  console.log(cVariable)      // '张三'
  console.log(dVariable)      // 24
  console.log(notVariable)    // undefined
}

tagTemplate`我叫${c}，今年${d}岁了`

```

使用标签函数可以看到，参数里面的第一个参数是数组，是由字符串模板被插入变量分割之后剩余字符串组成的数组，之后的参数依次就是按顺序插入变量的值了。

知道参数的形式后我们就可以用ES6的结构对插入变量进行一个遍历了

```javascript

let c = '张三'
let d = 24
function tagTemplate(strings, ...variable) {
  console.log(strings)        // ['我叫', '，今年', '岁了']
  for(let i = 0; i < variable.length; i++) {
    console.log(variable[i])
  }
  // '张三'  24
}

tagTemplate`我叫${c}，今年${d}岁了`

```

标签函数下的raw数组

- 采用模板标记函数后，第一个数组返回的是我们的非变量插入的字符串片段，这里的字符串片段是会转换成我们的实际展示形式的，比如 \u00A9 会转为 ©，\n 会转为一个空格
- 如果我们想要拿到没有经过转换的原版字符串，这里我们就可以使用这个数组上的raw属性来获取原版字符串的数组，我们可以看下代码

```javascript
function tagTest(strings, ...rest){
  for(let value of strings) {
    console.log(value)
  }
  for(let value of strings.raw) {
    console.log(value)
  }
}
let a = '张三'
let b = 24
tagTest`你${ a }\u00A9,哈哈\n${ b }我`
// strings遍历
// 你
// ©,哈哈 
// 我

// strings.raw遍历
// 你
// \u00A9,哈哈\n
// 我
```

##### String.raw

String.raw是ES6标准的一个字符串方法，使用方式类似于模板标记函数，后面直接跟一个模板字符串即可，会原样返回该模板字符串的值，不受可编译字符的影响，普通的字符串会返回经过转义的字符串结果，如下

```javascript
console.log(`\u00A9`)
// ©
console.log(String.raw`\u00A9`)
// \u00A9

console.log(`Hi\n`)
// Hi
console.log(String.raw`Hi\n`)
// Hi\n

console.log(`Hi
张三`)
// Hi
// 张三
console.log(String.raw`Hi
张三`)
// Hi
// 张三
```





#### Set和Map数据结构

##### Set

###### 基本用法

`Set`对象是值的集合，你可以按照插入的顺序迭代它的元素（元素是有序的）。 Set中的元素只会**出现一次**，即 Set 中的元素是唯一的。

```js
new Set([iterable])
```

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
- `WeakSet`持弱引用：集合中对象的引用为弱引用。 如果没有其他的对这些对象的引用时（任何weakSet或weakMap内对其的引用都看作是没有引用），那么这些对象会被当成垃圾回收掉。 这也意味着WeakSet中没有存储当前对象的列表。 正因为这样，`WeakSet` 是不可枚举的，没有办法拿到它包含的所有元素（所以没有遍历方法）。

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

Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。**Map 对键的类型没有限制**

上面的例子展示了如何向 Map 添加成员。作为构造函数，Map 也可以接受一个数组作为参数。该数组的成员是一个个表示键值对的数组。

`Map` 可以接受一个[可迭代对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...of)或空数组作为参数，且该可迭代对象的所有成员需要是对象或数组类型。

```js
new Map([iterable])
```

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

**`WeakMap`** 对象是一组键/值对的集合，其中的**键**是弱引用的，这意味着在没有其他引用这个键的存在时（任何weakSet或weakMap内对其的引用都看作是没有引用），垃圾回收能正确进行。其键必须是对象，而值可以是任意的。键是唯一的，相同键名，后面的键值会覆盖前面的键值。

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
- **WeakMap.prototype.has(key)**：返回一个布尔值，根据WeakMap对象的元素中是否存在key键。



**任何weakSet或weakMap内对其的引用都看作是没有引用**

```js
let a = new WeakMap();
let b = new WeakMap();
let c = { x: 1 };
let d = { y: 2 };
// 看似互相被引用了，形成死锁，不会被释放内存，实际上任何weakSet或weakMap内对其的引用都看作是没有引用
a.set(c, d);
b.set(d, c);

c=null
d=null

setTimeout(() => {
  console.log(a, b); // wWeakMap {} , WeakMap {}
}, 10000); // 时间长一些，不然gc还没释放内存
```



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
});
```

#### Promise

Promise 对象代表了未来将要发生的事件，用来传递异步操作的消息。

Promise 有三种状态，pending进行中，fulfilled已成功，rejected已失败

Promise对象里的函数会立即执行

```js
new Promise((resolve, reject) => {
  console.log(1)  // 立即执行
  resolve()  // 立即执行
}).then(res=>{})
```

当我们调用resolve()或reject()的时候，触发promise.then(...)实际上是一个异步操作，这个promise.then(...)并不是在resolve()或reject()的时候就立刻执行的，而也是要重新进入任务队列排队的，不过能直接在当前的事件循环新的执行栈中被取出执行（不用等下次事件循环）。

> promise一旦执行，无法取消

**Promise 原型方法**

##### Promise.prototype.then(onFulfilled [, onRejected])

最多需要有两个参数：Promise 的成功和失败情况的回调函数。返回一个新的 promise。

成功状态的回调函数接受前一个promise resolve的值作为参数。

失败状态的回调函数接受前一个promise reject的值作为参数。

##### Promise.prototype.catch(onRejected)

catch内的回调函数接受前一个promise reject的值或者错误作为参数。返回一个新的 promise。

##### 如果 Promise 状态已经变成resolved，再抛出错误是无效的。

```js
const promise = new Promise(function(resolve, reject) {
  resolve('ok');
  console.log(111)
  throw new Error('test');  //会被忽略，不会被捕获，等于没有抛出
});
promise
  .then(res => { console.log(res) })
  .catch(e => { console.log(e) });
// 打印 111
// 打印 ok ，但不会打印错误
catch是从pending到reject的状态改变，但是一个promise一旦resolve以后，状态就凝固了，无法再发生变化，因此会被忽略。
```

##### catch的捕获异常

不能捕获异步操作中产生的异常（`Promise.then`/`setTimeout`都是典型的异步操作）：

- 在非async函数中的try-catch
- Promise.catch`Promise`的`catch`会在`resolve`被调用之前的`throw`的`Error`对象，或者`reject`被调用后触发。

`Promise`的`catch`只会捕获到自身链式中的错误，不能捕获then等内部的其他promise的错误。

如果想捕获**promise**的异常：

- promise.catch

- ```js
  ;(async function a (){
      try{
          await Promise.reject(1)
      }catch(e){
          console.log(e)
      }
  })()
  ```

##### Promise.prototype.finally()

在promise**结束**时，无论结果是fulfilled或者是rejected，都会执行指定的回调函数。这避免了同样的语句需要在[`then()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/then)和[`catch()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch)中各写一次的情况。

##### Promise 静态方法

###### Promise.resolve()

返回一个状态由给定value决定的Promise对象。

```js
Promise.resolve() 
// 等价于
new Promise(resolve =>{
    resolve()
})
```

###### Promise.reject()

返回一个状态为失败的Promise对象，并将给定的失败信息传递给对应的处理方法—then或catch。

```js
Promise.reject() 
// 等价于
new Promise((resolve,reject) =>{
    reject()
})
```

###### Promise.all([ promise1,promise2, . . . ])

接收一个包含Promise对象的可迭代对象为参数，返回一个新的promise对象。该promise对象在数组里所有的promise对象都成功的时候才会触发成功，一旦有任何一个promise对象失败则立即触发该promise对象的失败。这个新的promise对象在触发成功状态以后，会把一个包含所有promise返回值的数组作为成功回调的返回值，顺序跟参数数组的顺序保持一致；如果这个新的promise对象触发了失败状态，它会把数组里第一个触发失败的promise对象的错误信息作为它的失败错误信息。Promise.all方法常被用于处理多个promise对象的状态集合。

###### Promise.allSettled([ promise1,promise2, . . . ])

接收一个包含Promise对象的可迭代对象为参数，等到所有promises都已敲定（settled）（每个promise都已兑现（fulfilled）或已拒绝（rejected））。
返回一个新promise对象，该promise在所有promise完成后完成。并带有一个对象数组（可以在then中获取），每个对象对应每个promise的结果。

###### Promise.any([ promise1,promise2, . . . ])

接收一个包含Promise对象的可迭代对象为参数，当其中的一个 promise 成功，就返回那个成功的promise对象。

###### Promise.race([ promise1,promise2, . . . ])

接收一个包含Promise对象的可迭代对象为参数。race就是赛跑的意思，意思就是说，Promise.race([p1, p2, p3])里面哪个结果获取得快，就返回那个结果，不管结果本身是成功状态还是失败状态。返回最快的那个promise对象。

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
    // 如果是return Promise.reject
    // return Promise.reject('catch里面的reject可以被之后的catch或then的第二个参数接收')
  })
  .then(res => {
    console.log(res)
  })
  // finally是获取不到then和catch的return
  .finally(res => {
    console.log(res)
  })
```

##### 手写Promise

```js
function MyPromise(constructor) {
  // 用self接受this，可以在resolve、reject中使用
  let self = this
  self.status = 'pending' //定义状态改变前的初始状态
  self.value = undefined //定义状态为resolved的时候的状态
  self.reason = undefined //定义状态为rejected的时候的状态
  function resolve(value) {
    // 两个==="pending"，保证了了状态的改变是不可逆的
    if (self.status === 'pending') {
      self.value = value
      self.status = 'resolved'
    }
  }
  function reject(reason) {
    //两个==="pending"，保证了了状态的改变是不不可逆的
    if (self.status === 'pending') {
      self.reason = reason
      self.status = 'rejected'
    }
  }
  //捕获构造异常
  try {
    constructor(resolve, reject)
  } catch (e) {
    reject(e)
  }
}
myPromise.prototype.then = function (onFullfilled, onRejected) {
  let self = this
  switch (self.status) {
    case 'resolved':
      onFullfilled(self.value)
      break
    case 'rejected':
      onRejected(self.reason)
      break
    default:
  }
}

// 测试
var p = new MyPromise(function (resolve, reject) {
  resolve(1)
})
// p调用then，this指向p
p.then(function (x) {
  console.log(x)
})
//输出1
```

#### async/await

`async`和`await`关键字让我们可以用一种更简洁的方式写出基于Promise的异步行为，而无需刻意地链式调用`promise`。

1、await  只能被用在 async 函数中，await后面一般是async函数（返回promise对象的函数）或者后面接new Promise() 或者任何要等待的值。await就是把promise的resolve的数据拿出来直接使用。

> await 会将后面的值（不是promise时）传进 promise.resolve 并赋值给左边的变量
>
> await 会将后面的promise对象内的resolve值赋值给左边的变量。

2、await会等待结果返回，再执行之后的代码

> 如果await后面的跟的是promise，且promise reject() 或 没有resolve()，对await来说是失败了，后面的代码不会执行
>
> 如果await右边的代码是同步执行，await左边的等号进行赋值相当于在then中取出参数resolve的值，是微任务
>
> await 后面的代码相当于是在new Promise内同步执行
>
> await下面的代码相当于是在then内执行，是微任务

3、async 修饰的函数  隐式返回一个 promise（即使不写return）。箭头函数也能用async修饰。

async函数内return  xxx ，实际上是 return new Promise (resolve => resolve(xxx)) === return Promise.solve(xxx)，跟Promise的用法一样。reject的话则 return Promise.reject()

4、在 await 外层  包一层  try{} catch(e){} 使用 可以获取异常

5、并发要使用await Promise.all([ promise1,promise2, . . . ])

6、async函数外部如果要到用then(res=>{})，要在async函数内return res。async函数内部就是promise 和 then 的语法糖。

7、async函数本身不是异步的，是它里面的代码可能是异步的，如果想要执行完async函数再执行其他操作（而这个操作不想写在async函数内，因为不想每次执行async都会执行这个操作），这时候要在async函数后使用then。

> 实际上，async / await 在底层转换成了 promise 和 then 回调函数。是基于promises的语法糖。每次我们使用 await, 解释器都创建一个 promise 对象，然后把剩下的 async 函数中的操作放到 then 回调函数中。

- await会阻其下面的代码执行

```js
async function fn(){
  await console.log('a')
  console.log('c')
}
fn()
console.log('b')
// a
// b
// c

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

- 如果await后面的promise没有resolve()，对await来说是失败了，函数内await后面的代码不会执行

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



- await在循环中的使用

```js
async function a1() {
  await console.log('aaa');
  console.log('bbb');
}

function a2() {
  console.log('ccc');
}

function test() {
  for (let cb of [a1, a2]) {
    cb();
  }
}

test();
// aaa
// ccc
// bbb
```

```js
async function a1() {
  await console.log('aaa');
  console.log('bbb');
}

function a2() {
  console.log('ccc');
}

async function test() {
  for (let cb of [a1, a2]) {
    await cb();
  }
}

test();
// aaa
// bbb
// ccc
```



#### 动态import

静态的 `import` 语句用于导入由另一个模块导出的绑定。无论是否声明了 [`strict mode`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode)，导入的模块都运行在严格模式下。在浏览器中，`import` 语句只能在声明了 `type="module"` 的 `script` 的标签中使用。

动态 `import()`，它不需要依赖 `type="module"` 的 script 标签。



在您希望按照一定的条件或者按需加载模块的时候，动态 `import()` 是非常有用的。而静态型的 `import` 是初始化加载依赖项的最优选择，使用静态 `import` 更容易从代码静态分析工具和 [tree shaking](https://developer.mozilla.org/zh-CN/docs/Glossary/Tree_shaking) 中受益。

```javascript
// module.js
export const a = 1
export const b = 2
export default {}

// app.js
import('module.js').then(module=>{
	console.log(module) // { default:{}, a:1, b:2 }
})
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
console.log(a.next()) // 打印 undefined（yield 123返回undefined），打印 { value: 'world', done: false }
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
  var result = yield fetch(url); // fetch() 返回一个Promise
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



##### 异步生成器函数 AsyncGenerator ES2018

```js
function resloveName() {
    return new Promise((reslove, reject) => {
        setTimeout(() => {
            reslove('也笑')
        }, 2000)
    })
}

async function* outputName() {
    console.log('开始', Date.now());
    const name = await resloveName();
    console.log('结束:', name, Date.now());
    yield 'slifree';
    console.log('yield 结束');
}
```

看到`*`标志，我们就知道它是一个生成器函数，而`async`关键字是异步函数的标志，所以合在一起就是异步生成器函数。

异步生成器函数的 `next`、`return`、`throw` 方法都是返回一个promise对象，可以使用then或在 async函数中使用await获取其返回的状态及结果。

```js
async function foo(){
	await bar.next();
}
```

此时我们直接运行`outputName()`是没有结果的，我们需要通过`next`方法调用，如下：

```js
const asyncGenerator=outputName();
asyncGenerator.next()
```

输出结果为：

```js
开始 1637464817195
结束: 也笑 1637464819215
```

如果我们再执行一次`asyncGenerator.next()`的输出结果是：

```
yield 结束
```





#### 原生js获取元素的各种位置

- offsetLeft （距离**定位**父级的距离）
- offsetTop （距离**定位**父级的距离）
- offsetWidth （可视宽度）
- offsetHeight （可视高度）
- clientLeft （左边框宽度）
- clientTop （上边框宽度）
- clientWidth（width + padding）
- clientHeight（height + padding）
- scrollTop（纵向滚动距离）
- scrollLeft（横向滚动距离）
- scrollWidth（内容宽度）
- scrollHeight（内容高度）

##### 子元素相对父容器的高度（父元素无需定位）

child.getBoundingClientRect().top - parent.getBoundingClientRect().top

##### getBoundingClientRect ( ) 返回值：对象 有6个属性

- left（元素左侧相对于可视区左上角的距离）
- right（元素右侧相对于可视区左上角的距离）
- top（元素上边相对于可视区左上角的距离）
- bottom（元素下边相对于可视区左上角的距离）
- width（可视宽度）
- height（可视高度）

##### 获取可视区宽高：

- window.innerWidth
- window.innerHeight
- document.documentElement.clientWidth
- document.documentElement.clientHeight

##### 屏幕宽高：

- window.screen.width

#### 原生事件

##### MouseEvent

```ts
function handle(e: MouseEvent){
	// screen参照点：电脑屏幕左上角
  // client参照点：浏览器可视内容区域左上角
  // page参照点：网页的左上角
  // pageY = clientY + window.pageYOffset(文档垂直方向滚动距离)
  const { screenX, screenY, clientX, clientY, pageX, pageY } = e;
}
```



#### try-catch-finally

##### 三者执行顺序

**1、如果try和catch模块中不存在return语句**，那么运行完try和catch模块中的代码后再运行finally中的代码。

**2、如果try和catch模块中存在return语句**，那么在运行return之前会运行finally中的代码，
 (1). 如果finally中存在return语句，则返回finally的return结果，代码运行结束。
 (2). 如果finally不存在return语句，则返回try或catch中的return结果，代码运行结束。

**3、如果try和catch模块中存在throw语句**，那么在catch运行throw之前会运行finally中的代码。
 (1). 如果finally中存在return语句，则返回finally的return结果，代码运行结束。
 (2). 如果finally不存在return语句，则运行catch中的throw语句，代码运行结束。



**总结**

finally中的代码最后执行，其return优先级最高



##### 捕获异常

**try-catch**只能捕获同步的异常，不能捕获异步的异常，例如：**setTimeout** 和 **promise**

因为**try-catch**是同步任务，执行时已经在JS主线程，当异步函数抛出异常时，**try-catch**已经执行完成了。

如果想捕获**promise**的异常：

- promise.catch

- ```js
  ;(async function a (){
      try{
          await Promise.reject(1)
      }catch(e){
          console.log(e)
      }
  })()
  ```

**try-catch**捕获到的异常不会冒泡。



#### 进程和线程

##### 区分进程和线程

线程和进程区分不清，是很多新手都会犯的错误，没有关系。这很正常。先看看下面这个形象的比喻：

```js
- 进程是一个工厂，工厂有它的独立资源

- 工厂之间相互独立

- 线程是工厂中的工人，多个工人协作完成任务

- 工厂内有一个或多个工人

- 工人之间共享空间
```

再完善完善概念：

```js
- 工厂的资源 -> 系统分配的内存（独立的一块内存）

- 工厂之间的相互独立 -> 进程之间相互独立

- 多个工人协作完成任务 -> 多个线程在进程中协作完成任务

- 工厂内有一个或多个工人 -> 一个进程由一个或多个线程组成

- 工人之间共享空间 -> 同一进程下的各个线程之间共享程序的内存空间（包括代码段、数据集、堆等）
```

最后，再用较为官方的术语描述一遍：

- 进程是cpu资源分配的最小单位（是能拥有资源和独立运行的最小单位）
- 线程是cpu调度的最小单位（线程是建立在进程的基础上的一次程序运行单位，一个进程中可以有多个线程）

**tips**

- 不同进程之间也可以通信，不过代价较大



##### 浏览器是多进程的

理解了进程与线程了区别后，接下来对浏览器进行一定程度上的认识：（先看下简化理解）

- 浏览器是多进程的
- 浏览器之所以能够运行，是因为系统给它的进程分配了资源（cpu、内存）
- 简单点理解，每打开一个Tab页，就相当于创建了一个独立的浏览器进程。

打开`Chrome的任务管理器`中看到有多个进程（分别是每一个Tab页面有一个独立的进程）。浏览器应该也有自己的优化机制，有时候打开多个tab页后，可以在Chrome任务管理器中看到，有些进程被合并了 （所以每一个Tab标签对应一个进程并不一定是绝对的），譬如打开多个空白标签页后，会发现多个空白标签页被合并成了一个进程。



##### 浏览器都包含哪些进程

仅列举主要进程

1. Browser进程：浏览器的主进程（负责协调、主控），只有一个。作用有
   - 负责浏览器界面显示，与用户交互。如前进，后退等
   - 负责各个页面的管理，创建和销毁其他进程
   - 将Renderer进程得到的内存中的Bitmap，绘制到用户界面上
   - 网络资源的管理，下载等
2. 第三方插件进程：每种类型的插件对应一个进程，仅当使用该插件时才创建
3. GPU进程：最多一个，用于3D绘制等
4. 浏览器渲染进程（内部是多线程）



##### 浏览器多进程的优势

相比于单进程浏览器，多进程有如下优点：

- 避免单个page crash影响整个浏览器
- 避免第三方插件crash影响整个浏览器
- 多进程充分利用多核优势
- 方便使用沙盒模型隔离插件等进程，提高浏览器稳定性

简单点理解：**如果浏览器是单进程，那么某个Tab页崩溃了，就影响了整个浏览器，体验有多差；同理如果是单进程，插件崩溃了也会影响整个浏览器；而且多进程还有其它的诸多优势**



##### 浏览器的渲染进程是多线程的

1、GUI渲染线程

- 负责渲染浏览器界面，解析HTML，CSS，构建DOM树和RenderObject树，布局和绘制等。
- 当界面需要重绘（Repaint）或由于某种操作引发回流(reflow)时，该线程就会执行
- 注意，**GUI渲染线程与JS引擎线程是互斥的**，当JS引擎执行时GUI线程会被挂起（相当于被冻结了），GUI更新会被保存在一个队列中**等到JS引擎空闲时**立即被执行。

> 由于JavaScript是可操纵DOM的，如果在修改这些元素属性同时渲染界面（即JS线程和UI线程同时运行），那么渲染线程前后获得的元素数据就可能不一致了。



2、JS引擎线程

- 负责处理Javascript脚本程序。（例如V8引擎）
- JS引擎一直等待着任务队列中任务的到来，然后加以处理，一个Tab页（renderer进程）中无论什么时候都只有一个JS线程在运行JS程序



3、事件触发线程

- 主要负责将准备好的事件交给 JS引擎线程执行。

比如 setTimeout定时器计数结束， ajax等异步请求成功并触发回调函数，或者用户触发点击事件时，该线程会将整装待发的事件依次加入到任务队列的队尾，等待 JS引擎线程的执行。



4、定时触发器线程

- 负责执行异步定时器一类的函数的线程，如： setTimeout，setInterval。
- 主线程依次执行代码时，遇到定时器，会将定时器交给该线程处理，当计数完毕后，事件触发线程会将计数完毕后的事件加入到任务队列的尾部，等待JS引擎线程执行。



5、异步http请求线程

- 负责执行异步请求一类的函数的线程
- 主线程依次执行代码时，遇到异步请求，会将函数交给该线程处理，当监听到状态码变更，如果有回调函数，事件触发线程会将回调函数加入到任务队列的尾部，等待JS引擎线程执行。





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

在JS引擎中，我们可以按性质把异步任务分为两类，**macrotask**（宏任务）和 **microtask**（微任务）。

**macrotask queue**可以有多个，相同任务源的宏任务，只能放到同一个宏任务队列中。

如 setTimeout setInterval setimmediate 会在一个task queue，网络IO会在一个task queue，用户的事件会在一个task queue。

**microtask queue** 只有一个，因为microtask queue是在当前task queue执行时生成的，在下一个task queue执行时会清空并重新生成。

**优先级**

1、macrotask（按优先级顺序排列）：scripts（同步代码）、 setTimeout、setInterval、setImmediate、 I/O、UI rendering、UI事件

2、microtask（按优先级顺序排列）：process.nextTick、Promise（这里指浏览器原生实现的 Promise）, Object.observe（已废弃）、 MutationObserver

> microtask任务 也可以生成新的 microtask任务 并且插入到同一个的微队列中执行，但是这个新的微任务执行会在放到最后，即使其优先级更高。

3、JS引擎首先从macrotask queue中取出第一个任务，执行完毕后，将microtask queue中的所有任务取出，按顺序全部执行；

> 宏任务队列一次只执行一个任务
>
> ```js
> setTimeout(() => {
> 	console.log('t1')
> 	Promise.resolve().then(() => {
> 		console.log('p1');
> 	})
> })
> 
> let i = 0
> while (i < 1000) {
> 	i++
> }
> 
> setTimeout(() => {
> 	console.log('t2')
> 	Promise.resolve().then(() => {
> 		console.log('p2');
> 	})
> }) 
> // t1 > p1 > t2 > p2
> ```

4、然后再从macrotask queue（宏任务队列）中取下一个，执行完毕后，再次将microtask queue（微任务队列）中的全部取出；

5、循环往复，直到两个queue中的任务都取完。

所以，**浏览器环境中**，js执行任务的流程是这样的：

1、第一个事件循环，先执行script中的所有同步代码（即 macrotask 中的第一项任务）

2、再取出 microtask 中的全部任务执行（先清空process.nextTick队列，再清空promise.then队列）

3、是否有必要渲染页面

4、下一个事件循环，再回到 macrotask 取其中的下一项任务

5、再重复2

6、反复执行事件循环…

**宏任务**

|                       | 浏览器 | Node |
| --------------------- | --- | ---- |
| setTimeout            | √   | √    |
| setInterval           | √   | √    |
| setImmediate          | x   | √    |
| requestAnimationFrame | √   | x    |
| I/O                   | √   | √    |

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
    },1000)
    console.log('微事件1')
  })
  .then(() => {
    setTimeout(() => {
      console.log('then-setTimeout-2')
    },0)
    console.log('微事件2')
  })
console.log('外层宏事件3')
```

我们看看打印结果

```
外层宏事件1
外层宏事件2
外层宏事件3
微事件1
微事件2
内层宏事件3
then-setTimeout-2
then-setTimeout-1
```

• 首先浏览器执行js进入第一个宏任务进入主线程, 遇到 setTimeout 分发到宏任务Event Queue中（setTimeout优先级比script低）

• 遇到 console.log() 直接执行 输出 外层宏事件1

• 遇到 Promise， new Promise 直接执行 输出 外层宏事件2

• 执行then 被分发到微任务Event Queue中

• 继续执行script，输出 外层宏事件3

• 第一轮宏任务执行结束，开始执行第一个微任务，遇到setTimeoutout，分发到宏任务Event Queue中，然后打印 '微事件1' ，生成第二个微任务（第二个then）

• 开始执行第二个微任务，遇到setTimeoutout，分发到宏任务Event Queue中，然后打印 '微事件2' 

• 第一轮微任务执行完毕，执行第二轮宏事件，在同一宏任务队列中按照顺序执行（注意定时器时间），打印 '内层宏事件3'、'then-setTimeout-2'、'then-setTimeout-1'

**NodeJS引擎中**：

1、先执行script中的所有同步代码，过程中把所有异步任务压进它们各自的队列（假设维护有process.nextTick队列、promise.then队列、setTimeout队列、setImmediate队列等4个队列）

2、按照优先级（process.nextTick > promise.then > setTimeout > setImmediate），选定一个  不为空 的任务队列，按先进先出的顺序，依次执行所有任务，执行过程中新产生的异步任务继续压进各自的队列尾，直到被选定的任务队列清空

3、重复2...

#### Object的方法

##### Object.keys()

返回一个由一个给定对象的自身可枚举属性组成的数组

```js
Object.keys(obj)
```

参数

- obj：对象或数组

返回值

- 新数组

**for...in** 循环和**Object.keys**方法都是获取对象可枚举属性

区别：前者包括对象继承自原型对象的属性，而后者只包括对象本身的属性。如果需要获取对象自身的所有属性，不管enumerable的值，可以使用**Object.getOwnPropertyNames**方法。

##### Object.create()

创建一个新对象，使用现有的对象来提供新创建的对象的`__proto__`。 

```
Object.create(proto，[propertiesObject])
```

参数

- proto：新创建对象的对象原型
- propertiesObject：可选，该传入对象自身的可枚举属性将为新创建的对象添加指定的属性值和对应的属性描述符。

返回值

- 一个新对象，带着指定的原型对象和属性。

**eg**

```js
o = Object.create(Object.prototype, {
  // foo会成为所创建对象的数据属性
  foo: {
    writable:true,
    configurable:true,
    value: "hello"
  },
  // bar会成为所创建对象的访问器属性
  bar: {
    configurable: false,
    get: function() { return 10 },
    set: function(value) {
      console.log("Setting `o.bar` to", value);
    }
  }
});
```

##### Object.values(obj)

返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用[`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in)循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

参数

- obj：被返回可枚举属性值的对象。

返回值

- 一个包含对象自身的所有可枚举属性值的数组。

##### Object.entries()

返回一个给定对象自身可枚举属性的键值对数组

```js
Object.entries(obj)
```

参数

- obj：可以返回其可枚举属性的键值对的对象。

返回值

- 新数组

##### Object.assign()

用于将所有**自身可枚举属性**的值从一个或多个源对象分配到目标对象。它将返回目标对象。

```js
Object.assign(target, source1, source2, ...)
```

参数

- target：目标对象
- sources：源对象

返回值

- 目标对象

如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

##### Object.setPrototypeOf

设置一个原型作为另一个对象的原型（`__proto__`）。

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

##### Reflect

**Reflect** 是一个内置的对象，它提供拦截 JavaScript 操作的方法。这些方法与**proxy handlers**的方法相同。`Reflect`不是一个函数对象，因此它是不可构造的。

#### Array的方法

##### Array.from()

从一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。

```js
Array.from(arrayLike[, mapFn[, thisArg]])
```

参数

- arrayLike：伪数组对象或可迭代对象
- mapFn（可选）：如果指定了该参数，新数组中的每个元素会执行该回调函数
- thisArg（可选）：可选参数，执行回调函数 `mapFn` 时 `this` 对象

返回值

- 新数组

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
  get(obj,prop){
    const v = Reflect.get(obj,prop);
    if(v !== null && typeof v === 'object'){
      return new Proxy(v,handler); // 代理内层
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

p.b.c = 3 //会先触发get方法获取p.b，然后触发返回的新代理对象.c的set。
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

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

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

1、Proxy代理整个对象，Object.defineProperty只代理对象上的某个属性。

2、对象上定义新属性时，Proxy可以监听到，Object.defineProperty监听不到。

3、数组新增删除修改时，Proxy可以监听到，Object.defineProperty监听不到。

4、Proxy不兼容IE，Object.defineProperty不兼容IE8及以下。

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

作用域即代码的执行环境，每个执行环境都有一个与之关联的变量对象，环境中定义的所有变量和函数都保存在这个对象中。在 web 浏览器中，全局执行环境被认为是 window 对象。且每个函数都有自己的执行环境，在进入函数时入栈，在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。

当代码在一个环境中执行时，会创建变量对象的一个作用域链，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终是当前执行的代码所在环境的变量对象，最后端，始终是全局执行环境的变量对象。

JavaScript 代码运行时，会产生一个全局的上下文环境（context，又称运行环境），包含了当前所有的变量和对象。然后，执行函数（或块级代码）的时候，又会在当前上下文环境的上层，产生一个函数运行的上下文，变成当前（active）的上下文，由此形成一个上作用域链。当该函数执行完毕后，对应的执行上下文从栈顶弹出，函数的作用域会随之销毁，其包含的所有变量也会统一释放并被自动回收。

> 内部环境可以通过作用域链访问外部环境，但外部环境不能访问内部环境的任何变量和函数。

这个栈是“后进先出”的数据结构。最后产生的上下文环境首先执行完成，退出栈，然后再执行完成它下层的上下文，直至所有代码执行完成，栈清空。

但是Generator函数是例外的：

Generator 函数不是这样，它执行产生的上下文环境，一旦遇到`yield`命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态。等到对它执行`next`命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。

#### Web Worker

https://mp.weixin.qq.com/s/OLUN9mHw3S3oBsfd6SONcw

#### FormData

##### FormData对象的作用

1. 用一些键值对模拟HTML表单（不需使用form标签），以便将数据通过`XMLHttpRequest.send()` 方法发送出去；
2. 可以上传文件等二进制数据（binary）

##### 构造函数

```js
new FormData(form)
```

参数 

- form：可选，HTML form元素，生成的实例对象将form中的表单值用键值对的方式加入自身

返回值

一个新的 FormData 实例对象

##### 方法

https://developer.mozilla.org/zh-CN/docs/Web/API/FormData

**FormDataInstance.append(key, value [, filename])**

- `name`
  
  `value中包含的数据对应的表单名称。`

- `value`
  
  `表单的值。`可以是[`USVString`](https://developer.mozilla.org/zh-CN/docs/Web/API/USVString) 或 [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) (包括子类型，如 [`File`](https://developer.mozilla.org/zh-CN/docs/Web/API/File))。

- `filename `可选
  
  传给服务器的文件名称 (一个 [`USVString`](https://developer.mozilla.org/zh-CN/docs/Web/API/USVString)), 当一个 [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 或 [`File`](https://developer.mozilla.org/zh-CN/docs/Web/API/File) 被作为第二个参数的时候， [`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob) 对象的默认文件名是 "blob"。 [`File`](https://developer.mozilla.org/zh-CN/docs/Web/API/File) 对象的默认文件名是该文件的名称。

向 `FormData` 中添加新的属性值，`FormData` 对应的属性值存在也不会覆盖原值，而是新增一个值，如果属性不存在则新增一项属性值。

可以直接向FormData对象附加File或Blob类型的文件。

##### multipart/form-data

使用FormData时，请求头设置：

```js
headers: {
    'Content-Type': 'multipart/form-data;charset=UTF-8'
}
```

**multipart/form-data：**

以消息的形式发送给服务器：会将表单的每个数据处理为一条消息，用分隔符分开。

在表单中进行文件上传时，就需要使用该格式。

既可以上传键值对，也可以上传文件。

上传文件是以二进制（binary）的形式提交。

##### 例子

下面的代码将创建一个空的FormData对象:

```
var formData = new FormData(); // 当前为空
```

Copy to Clipboard

你可以使用[`FormData.append`](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/append)来添加键/值对到表单里面；

```js
formData.append('username', 'Chris');
```

或者你可以使用可选的`*form参数来创建一个带预置数据的FormData对象*`:

```html
<form id="myForm" name="myForm">
  <div>
    <label for="username">Enter name:</label>
    <input type="text" id="username" name="username">
  </div>
  <div>
    <label for="useracc">Enter account number:</label>
    <input type="text" id="useracc" name="useracc">
  </div>
  <div>
    <label for="userfile">Upload file:</label>
    <input type="file" id="userfile" name="userfile">
  </div>
<input type="submit" value="Submit!">
</form>
```

**注意**: 所有的输入元素都需要有**name**属性，否则无法访问到值。

```js
var myForm = document.getElementById('myForm');
formData = new FormData(myForm);
```

##### 上传文件例子

```html
<input id="file" type="file">
```

```js
//初始化实例
var formData = new FormData();
//通过append() 方法向对象中添加content键值对
formData.append("file",document.getElementById('file').files[0]);
axios.post(url,formData)
```

#### Blob

Blob，Binary Large Object的缩写，代表二进制数据的大对象。在Web中，Blob类型的对象表示不可变的类似文件对象（File对象）的原始数据，通俗点说，就是Blob对象是表示二进制数据的类File对象，用于文件操作，因此可以像操作File对象一样操作Blob对象，实际上，File继承自Blob。

##### 使用场景

![](http://jay_ohhh.gitee.io/imagehosting/JS/Blob作用.jpg)

##### 构造函数

```js
new Blob( array, options )
```

参数

- array：数组中的每一项连接起来构成Blob对象的数据，数组中的每项元素可以是 ArrayBuffer(二进制数据缓冲区)、ArrayBufferView、Blob、DOMString。或其他类似对象的混合体。

- options：可选项，字典格式类型，可以指定如下两个属性：
  
  - type，默认值为`""`，它代表了将会被放入到blob中的数组内容的MIME类型。
  
  - endings， 默认值为"transparent"，用于指定包含行结束符`\n`的字符串如何被写入。 它是以下两个值中的一个： "native"，表示行结束符会被更改为适合宿主操作系统文件系统的换行符； "transparent"，表示会保持blob中保存的结束符不变。

##### 属性

- size：number类型，Blob对象中所包含数据的大小（字节）。

- type：string类型， Blob 对象所包含数据的 MIME 类型。如果类型未知，则该值为空字符串。

##### 方法

https://developer.mozilla.org/zh-CN/docs/Web/API/Blob

**slice**

```
var blob = instanceOfBlob.slice([start [, end [, contentType]]])
```

参数

- start： 可选，代表 `Blob` 里的下标，表示第一个会被会被拷贝进新的 `Blob` 的字节的起始位置。如果传入的是一个负数，那么这个偏移量将会从数据的末尾从后到前开始计算。默认值是0

- end： 可选，代表的是 `Blob` 的一个下标，这个下标-1的对应的字节将会是被拷贝进新的`Blob` 的最后一个字节。如果你传入了一个负数，那么这个偏移量将会从数据的末尾从后到前开始计算。默认值是 instanceOfBlob.size

- contentType： 可选，给新的 `Blob` 赋予一个新的文档类型。这将会把它的 type 属性设为被传入的值。它的默认值是一个空的字符串。

返回值

一个新的`Blob`对象

#### File

##### File 对象

通常情况下， `File` 对象是来自用户在一个 `<input>` 元素上选择文件后返回的 [`FileList`](https://developer.mozilla.org/zh-CN/docs/Web/API/FileList) 对象,也可以是来自由拖放操作生成的 [`DataTransfer`](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer) 对象。

File 继承 自Blob。其他操作二进制数据的API（比如File对象），都是建立在Blob对象基础上的，继承了它的属性和方法。

File 对象负责处理那些以文件形式存在的二进制数据。

最常见的使用场合是表单的文件上传控件（`<input type="file">`），用户选中文件以后，浏览器就会生成一个数组，里面是每一个用户选中的文件，它们都是 File 实例对象。

```html
// HTML 代码如下
<input id="fileItem" type="file">
var file = document.getElementById('fileItem').files[0];
file instanceof File // true
```

上面代码中，`file`是用户选中的第一个文件，它是 File 的实例。

##### 构造函数

浏览器原生提供一个`File()`构造函数，用来生成 File 实例对象。

```js
new File(array, name [, options])
```

`File()`构造函数接受三个参数。

- array：一个数组，成员可以是二进制对象或字符串，表示文件的内容。
- name：字符串，表示文件名或文件路径。
- options：配置对象，设置实例的属性。该参数可选。

第三个参数配置对象，可以设置两个属性。

- type：字符串，表示实例对象的 MIME 类型，默认值为空字符串。
- lastModified：时间戳，表示上次修改的时间，默认为`Date.now()`。

下面是一个例子。

```js
var file = new File(
  ['foo'],
  'foo.txt',
  {
    type: 'text/plain',
  }
);
```

##### 实例属性和实例方法

File 对象有以下实例属性。

- File.lastModified：最后修改时间
- File.name：文件名或文件路径
- File.size：文件大小（单位字节）
- File.type：文件的 MIME 类型

```js
var myFile = new File([], 'file.bin', {
  lastModified: new Date(2018, 1, 1),
});
myFile.lastModified // 1517414400000
myFile.name // "file.bin"
myFile.size // 0
myFile.type // ""
```

上面代码中，由于`myFile`的内容为空，也没有设置 MIME 类型，所以`size`属性等于0，`type`属性等于空字符串。

`File` 接口没有定义任何方法，但是它从 `Blob` 接口继承了以下方法：

**Blob.slice([start [, end [, contentType]]])**

##### FileList 对象

`FileList`对象是一个类似数组的对象，代表一组选中的文件，每个成员都是一个 File 实例。它主要出现在两个场合。

- 文件控件节点（`<input type="file">`）的`files`属性，返回一个 FileList 实例。
- 拖拉一组文件时，目标区的`DataTransfer.files`属性，返回一个 FileList 实例。

```js
// HTML 代码如下
// <input id="fileItem" type="file">
var files = document.getElementById('fileItem').files;
files instanceof FileList // true
```

上面代码中，文件控件的`files`属性是一个 FileList 实例。

FileList 的实例属性主要是`length`，表示包含多少个文件。

FileList 的实例方法主要是`item()`，用来返回指定位置的实例。它接受一个整数作为参数，表示位置的序号（从零开始）。但是，由于 FileList 的实例是一个类似数组的对象，可以直接用方括号运算符，即`myFileList[0]`等同于`myFileList.item(0)`，所以一般用不到`item()`方法。

##### FileReader 对象

FileReader 对象用于读取 File 对象或 Blob 对象所包含的文件内容。

浏览器原生提供一个`FileReader`构造函数，用来生成 FileReader 实例。

```js
var reader = new FileReader();
```

FileReader 有以下的实例属性。

- FileReader.error：读取文件时产生的错误对象
- FileReader.readyState：整数，表示读取文件时的当前状态。一共有三种可能的状态，`0`表示尚未加载任何数据，`1`表示数据正在加载，`2`表示加载完成。
- FileReader.result：读取完成后的文件内容，有可能是字符串，也可能是一个 ArrayBuffer 实例。
- FileReader.onabort：`abort`事件（用户终止读取操作）的监听函数。
- FileReader.onerror：`error`事件（读取错误）的监听函数。
- FileReader.onload：`load`事件（读取操作完成）的监听函数，通常在这个函数里面使用`result`属性，拿到文件内容。
- FileReader.onloadstart：`loadstart`事件（读取操作开始）的监听函数。
- FileReader.onloadend：`loadend`事件（读取操作结束）的监听函数。
- FileReader.onprogress：`progress`事件（读取操作进行中）的监听函数。

下面是监听`load`事件的一个例子。

```js
// HTML 代码如下
// <input type="file" onchange="onChange(event)">

function onChange(event) {
  var file = event.target.files[0];
  var reader = new FileReader();
  reader.onload = function (event) {
    console.log(event.target.result)
  };

  reader.readAsText(file);
}
```

上面代码中，每当文件控件发生变化，就尝试读取第一个文件。如果读取成功（`load`事件发生），就打印出文件内容。

FileReader 有以下实例方法。

- FileReader.abort()：终止读取操作，`readyState`属性将变成`2`。
- FileReader.readAsArrayBuffer()：以 ArrayBuffer 的格式读取文件，读取完成后`result`属性将返回一个 ArrayBuffer 实例。
- FileReader.readAsBinaryString()：读取完成后，`result`属性将返回原始的二进制字符串。
- FileReader.readAsDataURL()：读取完成后，`result`属性将返回一个 Data URL 格式（Base64 编码）的字符串，代表文件内容。对于图片文件，这个字符串可以用于`<img>`元素的`src`属性。注意，这个字符串不能直接进行 Base64 解码，必须把前缀`data:*/*;base64,`从字符串里删除以后，再进行解码。
- FileReader.readAsText()：读取完成后，`result`属性将返回文件内容的文本字符串。该方法的第一个参数是代表文件的 Blob 实例，第二个参数是可选的，表示文本编码，默认为 UTF-8。

下面是一个例子。

```js
/* HTML 代码如下
  <input type="file" onchange="previewFile()">
  <img src="" height="200">
*/

function previewFile() {
  var preview = document.querySelector('img');
  var file    = document.querySelector('input[type=file]').files[0];
  var reader  = new FileReader();

  reader.addEventListener('load', function () {
    preview.src = reader.result;
  }, false);

  if (file) {
    reader.readAsDataURL(file);
  }
}
```

上面代码中，用户选中图片文件以后，脚本会自动读取文件内容，然后作为一个 Data URL 赋值给`<img>`元素的`src`属性，从而把图片展示出来。

#### AurrayBuffer

https://wangdoc.com/es6/arraybuffer.html

**`ArrayBuffer`** 对象用来表示通用的、固定长度的原始二进制数据缓冲区。它是一个字节数组。

你不能直接操作 `ArrayBuffer` 的内容，而是要通过[类型数组对象（TypedArray）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/TypedArray)或 [`DataView`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/DataView) 对象来操作，它们会将缓冲区中的数据表示为特定的格式，并通过这些格式来读写缓冲区的内容。

##### 使用场景

![](http://jay_ohhh.gitee.io/imagehosting/JS/ArrayBuffer作用.jpg)

![](http://jay_ohhh.gitee.io/imagehosting/JS/ArrayBuffer与Array区别.jpg)

##### 构造函数

```js
new ArrayBuffer(length)
```

参数

- `length`
  
  要创建的 `ArrayBuffer` 的大小，单位为字节。

返回值

一个指定大小的 `ArrayBuffer` 对象，其内容被初始化为 0。

#### 垃圾回收机制

##### 可达性

JavaScript 中内存管理的主要概念是可达性。

简单地说，“可达性” 值就是那些以某种方式可访问或可用的值，它们被保证存储在内存中。

**1. 有一组基本的固有可达值，由于显而易见的原因无法删除。例如:**

- 当前正在执行的函数及其局部变量和参数
- 当前嵌套调用链上的其他函数及其局部变量和参数
- 全局变量

**这些值称为根。**

**2. 如果引用或引用链可以从根访问任何其他值，则认为该值是可访问的。**

例如，如果全局变量引用一个对象，并且该对象有一个引用另一个对象的属性，则该对象被认为是可访问的。它引用的那些对象也可以访问，详细的例子如下。

JavaScript 引擎中有一个后台进程称为 垃圾回收器，它监视所有对象，并删除那些不可访问的对象。

下面是最简单的例子:

```js
// user 具有对象的引用
let user = {
  name: "John"
};
```

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用1.png)

这里箭头表示一个对象引用。全局变量`“user”`引用对象 `{name:“John”}` (为了简洁起见，我们将其命名为**John**)。John 的 `“name”` 属性存储一个基本类型，因此它被绘制在对象中。

如果 `user` 的值被覆盖，则引用丢失:

```js
user = null;
```

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用2.png)

现在 **John** 变成不可达的状态，没有办法访问它，没有对它的引用。垃圾回收器将丢弃 **John** 数据并释放内存。

**两个引用**

现在让我们假设我们将引用从 `user` 复制到 `admin`:

```js
// user具有对象的引用
let user = {
  name: "John"
};

let admin = user;
```

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用3.png)

现在如果我们做同样的事情:

```js
user = null;
```

该对象仍然可以通过 `admin` 全局变量访问，所以它在内存中。

**相互关联的对象**

现在来看一个更复杂的例子， family 对象：

```js
function marry (man, woman) {
  woman.husban = man;
  man.wife = woman;

  return {
    father: man,
    mother: woman
  }
}

let family = marry({
  name: "John"
}, {
  name: "Ann"
})
```

函数 `marry` 通过给两个对象彼此提供引用来“联姻”它们，并返回一个包含两个对象的新对象。

产生的内存结构：

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用4.png)

到目前为止，所有对象都是可访问的。

现在让我们删除两个引用:

```js
delete family.father;
delete family.mother.husband;
```

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用5.png)

仅仅删除这两个引用中的一个是不够的，因为所有对象仍然是可访问的。

但是如果我们把这两个都删除，那么我们可以看到 **John** 不再有传入的引用（不再被引用）:

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用6.png)

输出引用无关紧要（输出引用：**John** 的wife属性指向 **Ann**）。因此，**John** 现在是不可访问的，并将从内存中删除所有不可访问的数据。

垃圾回收之后：

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用7.png)

**无法访问的数据块**

有可能整个相互连接的对象变得不可访问并从内存中删除。

源对象与上面的相同。然后:

```js
family = null;
```

内存中的图片变成：

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用8.png)

“family”对象已经从根上断开了链接，不再有对它的引用，因此下面的整个块变得不可到达，并将被删除。

##### 标记—清除 步骤

基本的垃圾回收算法称为**“标记-清除”**，定期执行以下“垃圾回收”步骤:

- 垃圾回收器获取根并**“标记”**(记住)它们。
- 然后它访问并“标记”所有来自它们的引用。
- 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
- 以此类推，直到有未访问的引用(可以从根访问)为止。
- 除标记的对象外，所有对象都被删除。

例如，对象结构如下:

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用9.png)

我们可以清楚地看到右边有一个“不可到达的块”。现在让我们看看**“标记并清除”**垃圾回收器如何处理它。

**第一步标记根**

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用10.png)

**然后标记他们的引用**

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用11.png)

**以及子孙代的引用**

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用12.png)

**现在进程中不能访问的对象被认为是不可访问的，将被删除**

![](http://jay_ohhh.gitee.io/imagehosting/JS/垃圾回收机制/引用13.png)

这就是垃圾收集的工作原理。JavaScript引擎应用了许多优化，使其运行得更快，并且不影响执行。

##### 优化

- **分代回收**——对象分为两组:“新对象”和“旧对象”。许多新对象出现，完成它们的工作 ，它们很快就会被清理干净。那些活得足够久的对象，会变“老”，并且很少接受检查。
- **增量回收**——如果有很多对象，并且我们试图一次遍历并标记整个对象集，那么可能会花费一些时间，并在执行中会有一定的延迟。因此，引擎试图将垃圾回收分解为多个部分。然后，各个部分分别执行。这需要额外的标记来跟踪变化，这样有很多微小的延迟，而不是很大的延迟。
- **空闲时间收集**——垃圾回收器只在 CPU 空闲时运行，以减少对执行的可能影响。

#### 存储

##### cookie

###### cookie的来源

**cookie 的本职工作并非本地存储，而是“维持状态”**。因为HTTP协议是**无状态的**，HTTP协议自身不对请求和响应之间的通信状态进行保存，通俗来说，服务器不知道用户上一次做了什么，这严重阻碍了交互式Web应用程序的实现。在典型的网上购物场景中，用户浏览了几个页面，买了一盒饼干和两瓶饮料。最后结帐时，由于HTTP的无状态性，不通过额外的手段，服务器并不知道用户到底买了什么，于是就诞生了cookie。它就是用来绕开HTTP的无状态性的“额外手段”之一。服务器可以设置或读取cookie中包含信息，借此维护用户跟服务器会话中的状态。

在刚才的购物场景中，当用户选购了第一项商品，服务器在向用户发送网页的同时，还发送了一段cookie，记录着那项商品的信息。当用户访问另一个页面，浏览器会把cookie发送给服务器，于是服务器知道他之前选购了什么。用户继续选购饮料，服务器就在原来那段Cookie里追加新的商品信息。结帐时，服务器读取发送来的cookie就行了。

> Cookie 自动在同源http请求头中携带，cookies在浏览器和服务器间来回传递信息，而sessionstorage和localstorage不会自动把数据发给服务器，仅在本地保存

###### 什么是cookie

cookie指某些网站为了辨别用户身份而储存在用户本地终端上的数据(通常经过加密)。**cookie是服务端生成，客户端进行维护和存储（保存在浏览器）**，存储在内存或者磁盘中。通过cookie,可以让服务器知道请求是来源哪个客户端，就可以进行客户端状态的维护，比如登陆后刷新，请求头就会携带登陆时response header中的Set-Cookie,Web服务器接到请求时也能读出cookie的值，根据cookie值的内容就可以判断和恢复一些用户的信息状态。

**cookie 遵循同源政策**

###### 使用场景

cookie 主要用于以下三个方面：

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

###### cookie的原理

![cookie](http://jay_ohhh.gitee.io/imagehosting/JS/cookie.webp)

第一次访问网站的时候，浏览器发出请求，服务器端生成 cookie在响应中通过Set-Cookie头部告知客户端(允许多个Set-Cookie头部传递多个cookie)，客户端得到 cookie后,后续请求都会自动将 cookie头部携带至请求中发送给服务器（见下面例子），另外，cookie的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

> 一个set-Cookie 字段只能设置一个cookie ，当你要想设置多个 cookie ，需要添加同样多的set-Cookie 字段
> 
> 如果你想要使用 Set-Cookie 修改一个已经存在的 cookie 的值，那么要注意，你必须匹配原有的所有的属性值（如果存在的话），否则就会生成一个新的 cookie，而不是修改它的值

```
// 一个HTTP响应：
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: name=value
Other-header: other-header-value
```

这个HTTP响应会设置一个名为"name"，值为"value"的cookie。名和值在发送时都会经过URL编码。浏览器会存储这些会话信息，并在之后的每个请求中都会通过HTTP头部cookie再将它们发回服务器，比如：

```
GET /index.jsl HTTP/1.1
Cookie: name=value
Other-header: other-header-value
```

###### cookie构成

cookie在浏览器中是由以下参数构成的：

- **name**：唯一标识cookie的名称。cookie名不区分大小写，因此myCookie和MyCookie是同一个名称。不过，实践中最好将cookie名当成区分大小写来对待，因为一些服务器软件可能这样对待它们。**cookie名必须经过URL编码**。
- **value**：存储在cookie里的字符串值。**这个值必须经过URL编码**。
- **Domain**：cookie有效的域。发送到这个域的所有请求都会包含对应的cookie。如果不指定，默认为文档来源（由协议、域名和端口共同定义），**不包含子域名**。如果指定了`Domain`，则一般包含子域名。因此，指定 `Domain` 比省略它的限制要少。但是，当子域需要共享有关用户的信息时，这可能会有所帮助。例如，如果设置 Domain=mozilla.org，则 Cookie 也包含在子域名中（如developer.mozilla.org）。
- **Path**：请求URL中包含这个路径才会把cookie发送到服务器。

```
// 例如，设置 Path=/docs，则以下地址都会匹配：
/docs
/docs/Web/
/docs/Web/HTTP
```

- **Expires/Max-Age**：设置cookie过期时间（Expires）或有效期（Max-Age）（即什么时间之后就不发送到服务器了）。**简单名/值对形式的cookie只在当前会话期间存在，用户关闭浏览器就会丢失**。如果想让cookie的生命周期超过单个浏览对话，那就指定Expires/Max-Age，**max-age优先级高于expires。**
- **Secure**：设置之后，**只在使用SSL安全连接**的情况下才会把cookie发送到服务器。例如，请求`https://www.wrox.com`会发送cookie，而请求`http://www.wrox.com`则不会。
- **HttpOnly**：设置了 HttpOnly 属性的 cookie 不能使用 JavaScript 经由  Document.cookie 属性、XMLHttpRequest 和  Request APIs 进行访问，以防范跨站脚本攻击（XSS）。

```
HTTP/1.1 200 OK
Content-type: text/html
Set-Cookie: name=value; domain=.wrox.com; path=/; secure
Other-header: other-header-value
```

这里创建的cookie对所有wrox.com的子域及该域中的所有页面有效（通过path=/指定）。不过，这个cookie只能在SSL连接上发送，因为设置了secure标志。

要知道，**域、路径、过期时间和secure标志用于告诉浏览器什么情况下应该在请求中包含cookie**。这些参数并不会随请求发送给服务器，**实际发送的只有cookie的名/值对**。

###### JavaScript中的Cookie

一般说来，cookie的生成方式主要有两种，一种是上文提到的在响应中通过Set-Cookie头部告知客户端；另外一种就是在JavaScript中可以通过document.cookie可以读写cookie，如下：

```js
//读取浏览器中的cookie
console.log(document.cookie);
//写入cookie
document.cookie='myname=langlixingzhou;path=/;domain=.baidu.com';
```

在JavaScript中处理cookie比较麻烦，因为接口过于简单，只有BOM的document.cookie属性。在设置值时，可以通过document.cookie属性设置新的cookie字符串。这个字符串在被解析后会添加到原有cookie中。设置document.cookie不会覆盖之前存在的任何cookie，除非设置了已有的cookie。要为创建的cookie指定额外的信息，只要像Set-Cookie头部一样直接在后面追加相同格式的字符串即可：

```js
document.cookie = encodeURIComponent("name") + "=" +
                  encodeURIComponent("Nicholas") + "; domain=.wrox.com; path=/";
// 使用encodeURIComponent()对名称和值进行编码
```

###### cookie缺陷

- cookie 不够大

每个cookie的大小为4KB（**名字和值都包含在这4KB之内**），对于复杂的存储需求来说是不够用的。当 cookie 超过 4KB 时，它将面临被裁切的命运。这样看来，cookie 只能用来存取少量的信息。此外很多浏览器对一个站点的cookie个数也是有限制的（一般来说不超过300个cookie）。

- 过多的 cookie 会带来巨大的性能浪费

cookie是与特定域绑定的。同一个域名下的所有请求，都会携带 cookie。大家试想，如果我们此刻仅仅是请求一张图片或者一个 CSS 文件，我们也要携带一个 cookie 跑来跑去（关键是 cookie 里存储的信息并不需要），这是一件多么劳民伤财的事情。cookie 虽然小，但随着请求的叠加，这样的不必要的 cookie 带来的开销将是无法想象的。

cookie是用来维护用户信息的，而域名(domain)下所有请求都会携带cookie，但对于静态文件的请求，携带cookie信息根本没有用，此时可以通过CDN（存储静态文件的）的域名和主站的域名分开来解决。

- 由于在HTTP请求中的cookie是明文传递的，所以安全性成问题，除非用HTTPS。

###### cookie与安全

| 属性        | 作用                                |
| --------- | --------------------------------- |
| value     | 如果用于保存用户登录态，应该将该值加密，不能使用明文的用户标识   |
| http-only | 不能通过JS访问cookie，减速 XSS 攻击          |
| secure    | 只能在协议为 HTTPS 的请求中携带               |
| same-site | 规定浏览器不能在跨域请求中携带 Cookie，减速 CSRF 攻击 |

有两种方法可以确保 cookie 被安全发送，并且不会被意外的参与者或脚本访问：`Secure` 属性和`HttpOnly` 属性。

标记为 `Secure` 的 cookie 只应通过被 HTTPS 协议加密过的请求发送给服务端，因此可以预防中间人攻击。但即便设置了 `Secure` 标记，**敏感信息也不应该通过 cookie 传输**，因为 cookie 有其固有的不安全性，`Secure` 标记也无法提供确实的安全保障, 例如，可以访问客户端硬盘的人可以读取它。

从 Chrome 52 和 Firefox 52 开始，不安全的站点（`http:`）无法使用cookie的 `Secure` 标记。

JavaScript Document.cookie API 无法访问带有 `HttpOnly` 属性的cookie；此类 cookie 仅作用于服务器。例如，持久化服务器端会话的 cookie 不需要对 JavaScript 可用，而应具有 `HttpOnly` 属性。此预防措施有助于缓解跨站点脚本（XSS）攻击。

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2019 07:28:00 GMT; Secure; HttpOnly
```

对cookie的限制及其特性决定了cookie并不是存储大量数据的理想方式，让“专业的人做专业的事情”，Web Storage 出现了。

HTML5中新增了本地存储的解决方案----Web Storage，这样有了Web Storage后，cookie能只做它应该做的事情了—— 作为客户端与服务器交互的通道，保持客户端状态。

##### session

除了使用Cookie，Web应用程序中还经常使用Session来记录客户端状态。**Session是服务器端使用的一种记录客户端状态的机制**，使用上比Cookie简单一些，相应的也**增加了服务器的存储压力**。Session Id 会被存储到客户端的cookie 中，请求时会自动携带。

##### [cookie 和 session](https://www.cnblogs.com/l199616j/p/11195667.html)

为了弥补HTTP协议无状态的不足，常用的会话跟踪技术是Cookie与Session。

**Cookie通过在客户端记录信息确定用户身份**，**Session通过在服务器端记录信息确定用户身份**。

cookie 和 session均由服务器生成。

如果说**Cookie机制是通过检查客户身上的“通行证”来确定客户身份的话，那么Session机制就是通过检查服务器上的“客户明细表”来确认客户身份。Session相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。**

##### Web Storage

Web Storage的目的是解决通过客户端存储不需要频繁发送回服务器的数据时使用cookie的问题。Web Storage API包含了两个对象：localStorage和sessionStorage，**本质上是映射字符串键和值的对象化**。localStorage是永久存储机制，sessionStorage是跨会话的存储机制。这两种浏览器存储API提供了在浏览器中不受页面刷新影响而存储数据的两种方式。

###### Storage 对象

Window 对象的localStorage 和 sessionStorage 属性引用的是 Storage对象。Storage对象用于保存名/值对数据，直至存储空间上限（由浏览器决定）。一般来说，客户端数据的大小限制是按照每个源（协议、域和端口）来设置的，因此每个源有固定大小的数据存储空间。不同浏览器给localStorage和sessionStorage设置了不同的空间限制，但大多数会限制为每个源5MB。

###### Storage 对象存储的键值采用什么字符编码

**UTF-16**

localStorage 存储的键和值始终采用 UTF-16 DOMString 格式，每个字符使用两个字节。与对象一样，整数键将自动转换为字符串。

每个字符使用两个字节，是有前提条件的，就是码点小于`0xFFFF`(65535)， 大于这个码点的是四个字节。

###### 5M 是什么多少

**字符串的长度值, 或者utf-16的编码单元数量，更合理的是 10M字节空间**

5M = 5 * 1024 * 1024

单位  m      kb         b       

字符的个数，并不等于字符的长度。

String.length-字符串的长度：返回字符串中utf-16的编码单元的数量。

```js
"a".length // 1
"人".length // 1
"𠮷".length // 2
"🔴".length // 2
```

而根据 UTF-16编码规则，一个字符要么2个字节，要么4个字节，**`所以不如说是 10M 的字节数，更为合理。`**

**当然，2个字节作为一个utf-16的字符编码单元，也可以说是 5M 的utf-16的编码单元。**

我们先编写一个utf-16字符串计算字节数的方法：非常简单，判断码点决定是2还是4

```js
function sizeofUtf16Bytes(str) {
    var total = 0,
        charCode,
        i,
        len;
    for (i = 0, len = str.length; i < len; i++) {
        charCode = str.charCodeAt(i);
        if (charCode <= 0xffff) {
            total += 2;
        } else {
            total += 4;
        }
    }
    return total;
}
```

###### Storage 对象键占不占存储空间

键和值都**占空间**

###### Storage 对象的键的数量，对写和读性能的影响

**键的数量对读取性能影响不大。值的大小对性能影响更大，不建议保存大的数据。**

不考虑键的大小的问题，如果键太大，你需要先想想键为何取这么大。

我们`500 * 1000`键，如下

```js
let keyCount = 500 * 1000;

localStorage.clear();
for (let i = 0; i < keyCount; i++) {
    localStorage.setItem(i, "");
}

setTimeout(() => {
    console.time("save_cost");
    localStorage.setItem("a", "1");
    console.timeEnd("save_cost");
}, 2000)


setTimeout(() => {
    console.time("read_cost");
    localStorage.getItem("a");
    console.timeEnd("read_cost");

}, 2000)

// save_cost: 0.05615234375 ms
// read_cost: 0.008056640625 ms
```

你单独执行保存代码：

```js
localStorage.clear();    
console.time("save_cost");
localStorage.setItem("a", "1");
console.timeEnd("save_cost");
// save_cost: 0.033203125 ms
```

可以多次测试， 影响肯定是有的，也仅仅是数倍，不是特别的大。

反过来，如果是保存的值表较大呢？

```js
const charTxt = "a";
const count = 5 * 1024 * 1024  - 1
const val1 = new Array(count).fill(charTxt).join("");

setTimeout(() =>{
    localStorage.clear();
    console.time("save_cost_1");
    localStorage.setItem("a", val1);
    console.timeEnd("save_cost_1");
},1000)


setTimeout(() =>{
    localStorage.clear();
    console.time("save_cost_2");
    localStorage.setItem("a", "a");
    console.timeEnd("save_cost_2");
},1000)

// save_cost_1: 12.276123046875 ms
// save_cost_2: 0.010009765625 ms
```

可以多测试很多次，单次值的大小对存的性能影响非常大，读取也一样，合情合理之中。

所以尽量不要保存大的值，因为其是同步读取，纯大数据，用indexedDB就好。

###### 写个方法统计一个 Storage 已使用空间

现代浏览器的精写版本：

```js
function sieOfLS(storege=sessionStorage) {
      const strLength = Object.entries(storege).map(v => v.join('')).join('').length;
    console.log(`当前已使用${strLength}个utf-16的编码单元，占用率为${(strLength/(5*1024*1024)*100).toFixed(2)}%`)
    return strLength;
}
sieOfLS()
```

###### 页面的utf-8编码

**这和localStorage的存储没有半毛钱的关系。**

我们的html页面，经常会出现`<meta charset="UTF-8">`。告知浏览器此页面属于什么字符编码格式，下一步浏览器做好解码工作。

```html
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>容器</title>
</head>
```

###### Storage遵循同源策略

Storage对象定义了如下方法：

- clear()：删除所有值；不在Firefox中实现。
- getItem(name)：取得给定name的值。（get的值如果不是字符串类型，需要通过 JSON.parse 解析）
- key(index)：取得给定数值位置的名称。
- removeItem(name)：删除给定name的名/值对。
- setItem(name, value)：设置给定name的值。（存储的值本身不是字符串类型，需要通过 JSON.stringfy 转化为 json 对象）

**Storage 对象中的键值对总是以字符串的形式存储**，这意味着数值类型会自动转化为字符串类型。

###### sessionStorage

sessionStorage对象只存储会话数据，这意味着数据只会存储到浏览器关闭。这跟浏览器关闭时会消失的会话cookie类似。存储在sessionStorage中的数据不受页面刷新影响，可以在浏览器崩溃并重启后恢复（取决于浏览器，Firefox和WebKit支持，IE不支持）。

sessionStorage 特别应该注意一点就是，**即便是相同域名下的两个页面，只要它们不在同一个浏览器窗口中打开，那么它们的 sessionStorage 数据便无法共享。**

###### localStorage

localStorage 与 sessionStorage 在 API 方面无异。

###### localStorage 和 SessionStorage的区别

- localStorage生命周期是永久，除非手动删除 

- sessionStorage生命周期为当前窗口或标签页，当页面被关闭时，存储在 sessionStorage 的数据会被清除 

- 不同浏览器无法共享localStorage或sessionStorage中的信息。相同协议、域名、端口的前提下，相同浏览器的不同页面间可以共享相同的 localStorage，但是不同页面或标签页间无法共享sessionStorage的信息。

##### Web Storage 和 Cookie 和 IndexDB 的区别

| 特性     | cookie                      | localStorage               | sessionStorage           | IndexedDB    |
| ------ | --------------------------- | -------------------------- | ------------------------ | ------------ |
| 数据生命周期 | 一般由服务器生成，可以设置过期时间           | 除非被清理，否则一直存在               | 页面关闭就清理                  | 除非被清理，否则一直存在 |
| 数据存储大小 | 4K                          | 5M                         | 5M                       | 无限           |
| 与服务端通信 | 每次请求都会自动携带在 header 中，影响请求性能 | 不参与                        | 不参与                      | 不参与          |
| 作用域    | 在所有的同源标签页（不同页面但域名端口相同）都是共享  | 在所有的同源标签页（不同页面但域名端口相同）都是共享 | 不同标签页面的sessionStorage不共享 | /            |

- 均遵循同源政策。
- Cookie 的本职工作并非本地存储，而是“维持状态”。
- Web Storage定义了两个对象用于存储数据：sessionStorage和localStorage。前者用于严格保存浏览器一次会话期间的数据，因为数据会在浏览器关闭时被删除。后者用于会话之外持久保存数据。
- IndexedDB是类似于SQL数据库的结构化数据存储机制。不同的是，IndexedDB存储的是对象，而不是数据表。

##### 不同域名之间共享 localStorage

两个不同的域名的localStorage不能直接互相访问。那么如何在`aaa.com`中如何调用`bbb.com`的localStorage?

**实现原理**

1、在`aaa.com`的页面中，在页面中嵌入一个src为`bbb.com`的`iframe`，此时这个`iframe`里可以调用`bbb.com`的localStorage。
2、用`postMessage`方法实现页面与`iframe`之间的通信。
 综合1、2便可以实现`aaa.com`中调用`bbb.com`的localStorage。

https://www.jianshu.com/p/8c4cee29d532

##### Cache Storage

`Cache Storage` 是用来存储 `Response` 对象 ，也就是对 HTTP 响应进行缓存。 `Cache Storage` 是多个 `cache` 的集合，每个 `cache` 可以存储多个响应对象。它基于 Promise。

##### Application Cache

它是 HTML5 中新引入的应用程序换粗技术，它的出现意味着 web 应用可以通过缓存，在没有网络的环境下运行，构建离线应用。

优点:

- 离线浏览
- 提升页面的载入速度
- 降低服务器的压力

一般来说 `Application Cache` 只用来存储一些静态资源，它的使用方式主要需要两个步骤：

1.服务端维护一个 `manifest`清单

2.浏览器端进行一个设置。

```html
<html manifest="demo">
```

> 一般的，我们必须给 manifest 文件设置正确的 MIME-type 为 "text/cache-manifest"，它需要在服务器端进行配置。

在 Progressive Web Application 中， Application Cache 配合 Service Worker 承担着主要的任务。

使用 IndexedDB 储存离线数据和 Service Workers 储存离线资源，其简述请查看 [Service Workers 制作离线 PWA](https://developer.mozilla.org/zh-CN/docs/Web/Progressive_web_apps/Offline_Service_workers)。

##### IndexedDB

IndexedDB 是一种底层 API，用于在客户端存储大量的结构化数据（也包括文件/二进制大型对象（blobs））。该 API 使用索引实现对数据的高性能搜索。虽然 [Web Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API) 在存储较少量的数据很有用，但对于存储更大量的结构化数据来说力不从心。

就数据库类型而言，IndexedDB 不属于关系型数据库（不支持 SQL 查询语句），更接近 NoSQL 数据库。

IndexedDB API是强大的，但对于简单的情况可能看起来太复杂。如果你更喜欢一个简单的API，请尝试  [localForage](https://localforage.github.io/localForage/)、[dexie.js](https://www.dexie.org/)、[PouchDB](https://pouchdb.com/)、[idb](https://www.npmjs.com/package/idb)、[idb-keyval](https://www.npmjs.com/package/idb-keyval)、[JsStore](https://jsstore.net/) 或者 [lovefield](https://github.com/google/lovefield) 之类的库，这些库使 IndexedDB 对开发者来说更加友好。

使用 IndexedDB 执行的操作是异步执行的，以免阻塞应用程序。IndexedDB 最初包括同步和异步 API。同步 API 仅用于 [Web Workers](https://developer.mozilla.org/en-US/docs/Web/Guide/Performance/Using_web_workers)，且已从规范中移除，因为尚不清晰是否需要。但如果 Web 开发人员有足够的需求，可以重新引入同步 API。

###### 特点

**（1）键值对储存。** IndexedDB 内部采用对象仓库（object store）存放数据。所有类型的数据都可以直接存入，包括 JavaScript 对象。对象仓库中，数据以"键值对"的形式保存，每一个数据记录都有对应的主键，主键是独一无二的，不能有重复，否则会抛出一个错误。

**（2）异步。** IndexedDB 操作时不会锁死浏览器，用户依然可以进行其他操作，这与 LocalStorage 形成对比，后者的操作是同步的。异步设计是为了防止大量数据的读写，拖慢网页的表现。

**（3）支持事务。** IndexedDB 支持事务（transaction），这意味着一系列操作步骤之中，只要有一步失败，整个事务就都取消，数据库回滚到事务发生之前的状态，不存在只改写一部分数据的情况。

**（4）同源限制** IndexedDB 受到同源限制，每一个数据库对应创建它的域名。网页只能访问自身域名下的数据库，而不能访问跨域的数据库。

**（5）储存空间大** IndexedDB 的储存空间比 LocalStorage 大得多，一般来说不少于 250MB，甚至没有上限。

**（6）支持二进制储存。** IndexedDB 不仅可以储存字符串，还可以储存二进制数据（ArrayBuffer 对象和 Blob 对象）。

###### 对象接口

IndexedDB 把不同的实体，抽象成一个个对象接口。

```
数据库：IDBDatabase 对象
对象仓库：IDBObjectStore 对象
索引： IDBIndex 对象
事务： IDBTransaction 对象
操作请求：IDBRequest 对象
指针： IDBCursor 对象
主键集合：IDBKeyRange 对象
```

**（1）数据库**

数据库是一系列相关数据的容器。每个域名（严格的说，是协议 + 域名 + 端口）都可以新建任意多个数据库。

IndexedDB 数据库有版本的概念。同一个时刻，只能有一个版本的数据库存在。如果要修改数据库结构（新增或删除表、索引或者主键），只能通过升级数据库版本完成。

**（2）对象仓库**

每个数据库包含若干个对象仓库（object store）。它类似于关系型数据库的表格。

**（3）数据记录**

对象仓库保存的是数据记录。每条记录类似于关系型数据库的行，但是只有主键和数据体两部分。主键用来建立默认的索引，必须是不同的，否则会报错。主键可以是数据记录里面的一个属性，也可以指定为一个递增的整数编号。

> ```javascript
> { id: 1, text: 'foo' }
> ```

上面的对象中，`id`属性可以当作主键。

数据体可以是任意数据类型，不限于对象。

**（4）索引**

为了加速数据的检索，可以在对象仓库里面，为不同的属性建立索引。

**（5）事务**

数据记录的读写和删改，都要通过事务完成。事务对象提供`error`、`abort`和`complete`三个事件，用来监听操作结果。

###### 操作流程

IndexedDB 数据库的各种操作，一般是按照下面的流程进行的。这个部分只给出简单的代码示例，用于快速上手，详细的各个对象的 API 请看[这里](https://wangdoc.com/javascript/bom/indexeddb.html#indexeddb-对象)。

**3.1 打开数据库**

使用 IndexedDB 的第一步是打开数据库，使用`indexedDB.open()`方法。

> ```javascript
> var request = window.indexedDB.open(databaseName, version);
> ```

这个方法接受两个参数，第一个参数是字符串，表示数据库的名字。如果指定的数据库不存在，就会新建数据库。第二个参数是整数，表示数据库的版本。如果省略，打开已有数据库时，默认为当前版本；新建数据库时，默认为`1`。

`indexedDB.open()`方法返回一个 IDBRequest 对象。这个对象通过三种事件`error`、`success`、`upgradeneeded`，处理打开数据库的操作结果。

**（1）error 事件**

`error`事件表示打开数据库失败。

> ```javascript
> request.onerror = function (event) {
>   console.log('数据库打开报错');
> };
> ```

**（2）success 事件**

`success`事件表示成功打开数据库。

> ```javascript
> var db;
> 
> request.onsuccess = function (event) {
>   db = request.result;
>   console.log('数据库打开成功');
> };
> ```

这时，通过`request`对象的`result`属性拿到数据库对象。

**（3）upgradeneeded 事件**

如果指定的版本号，大于数据库的实际版本号，就会发生数据库升级事件`upgradeneeded`。

> ```javascript
> var db;
> 
> request.onupgradeneeded = function (event) {
>   db = event.target.result;
> }
> ```

这时通过事件对象的`target.result`属性，拿到数据库实例。

**3.2 新建数据库**

新建数据库与打开数据库是同一个操作。如果指定的数据库不存在，就会新建。不同之处在于，后续的操作主要在`upgradeneeded`事件的监听函数里面完成，因为这时版本从无到有，所以会触发这个事件。

通常，新建数据库以后，第一件事是新建对象仓库（即新建表）。

> ```javascript
> request.onupgradeneeded = function(event) {
>   db = event.target.result;
>   var objectStore = db.createObjectStore('person', { keyPath: 'id' });
> }
> ```

上面代码中，数据库新建成功以后，新增一张叫做`person`的表格，主键是`id`。

更好的写法是先判断一下，这张表格是否存在，如果不存在再新建。

> ```javascript
> request.onupgradeneeded = function (event) {
>   db = event.target.result;
>   var objectStore;
>   if (!db.objectStoreNames.contains('person')) {
>     objectStore = db.createObjectStore('person', { keyPath: 'id' });
>   }
> }
> ```

主键（key）是默认建立索引的属性。比如，数据记录是`{ id: 1, name: '张三' }`，那么`id`属性可以作为主键。主键也可以指定为下一层对象的属性，比如`{ foo: { bar: 'baz' } }`的`foo.bar`也可以指定为主键。

如果数据记录里面没有合适作为主键的属性，那么可以让 IndexedDB 自动生成主键。

> ```javascript
> var objectStore = db.createObjectStore(
>   'person',
>   { autoIncrement: true }
> );
> ```

上面代码中，指定主键为一个递增的整数。

新建对象仓库以后，下一步可以新建索引。

> ```javascript
> request.onupgradeneeded = function(event) {
>   db = event.target.result;
>   var objectStore = db.createObjectStore('person', { keyPath: 'id' });
>   objectStore.createIndex('name', 'name', { unique: false });
>   objectStore.createIndex('email', 'email', { unique: true });
> }
> ```

上面代码中，`IDBObject.createIndex()`的三个参数分别为索引名称、索引所在的属性、配置对象（说明该属性是否包含重复的值）。

**3.3 新增数据**

新增数据指的是向对象仓库写入数据记录。这需要通过事务完成。

> ```javascript
> function add() {
>   var request = db.transaction(['person'], 'readwrite')
>     .objectStore('person')
>     .add({ id: 1, name: '张三', age: 24, email: 'zhangsan@example.com' });
> 
>   request.onsuccess = function (event) {
>     console.log('数据写入成功');
>   };
> 
>   request.onerror = function (event) {
>     console.log('数据写入失败');
>   }
> }
> 
> add();
> ```

上面代码中，写入数据需要新建一个事务。新建时必须指定表格名称和操作模式（"只读"或"读写"）。新建事务以后，通过`IDBTransaction.objectStore(name)`方法，拿到 IDBObjectStore 对象，再通过表格对象的`add()`方法，向表格写入一条记录。

写入操作是一个异步操作，通过监听连接对象的`success`事件和`error`事件，了解是否写入成功。

**3.4 读取数据**

读取数据也是通过事务完成。

> ```javascript
> function read() {
>    var transaction = db.transaction(['person']);
>    var objectStore = transaction.objectStore('person');
>    var request = objectStore.get(1);
> 
>    request.onerror = function(event) {
>      console.log('事务失败');
>    };
> 
>    request.onsuccess = function( event) {
>       if (request.result) {
>         console.log('Name: ' + request.result.name);
>         console.log('Age: ' + request.result.age);
>         console.log('Email: ' + request.result.email);
>       } else {
>         console.log('未获得数据记录');
>       }
>    };
> }
> 
> read();
> ```

上面代码中，`objectStore.get()`方法用于读取数据，参数是主键的值。

**3.5 遍历数据**

遍历数据表格的所有记录，要使用指针对象 IDBCursor。

> ```javascript
> function readAll() {
>   var objectStore = db.transaction('person').objectStore('person');
> 
>    objectStore.openCursor().onsuccess = function (event) {
>      var cursor = event.target.result;
> 
>      if (cursor) {
>        console.log('Id: ' + cursor.key);
>        console.log('Name: ' + cursor.value.name);
>        console.log('Age: ' + cursor.value.age);
>        console.log('Email: ' + cursor.value.email);
>        cursor.continue();
>     } else {
>       console.log('没有更多数据了！');
>     }
>   };
> }
> 
> readAll();
> ```

上面代码中，新建指针对象的`openCursor()`方法是一个异步操作，所以要监听`success`事件。

**3.6 更新数据**

更新数据要使用`IDBObject.put()`方法。

> ```javascript
> function update() {
>   var request = db.transaction(['person'], 'readwrite')
>     .objectStore('person')
>     .put({ id: 1, name: '李四', age: 35, email: 'lisi@example.com' });
> 
>   request.onsuccess = function (event) {
>     console.log('数据更新成功');
>   };
> 
>   request.onerror = function (event) {
>     console.log('数据更新失败');
>   }
> }
> 
> update();
> ```

上面代码中，`put()`方法自动更新了主键为`1`的记录。

**3.7 删除数据**

`IDBObjectStore.delete()`方法用于删除记录。

> ```javascript
> function remove() {
>   var request = db.transaction(['person'], 'readwrite')
>     .objectStore('person')
>     .delete(1);
> 
>   request.onsuccess = function (event) {
>     console.log('数据删除成功');
>   };
> }
> 
> remove();
> ```

**3.8 使用索引**

索引的意义在于，可以让你搜索任意字段，也就是说从任意字段拿到数据记录。如果不建立索引，默认只能搜索主键（即从主键取值）。

假定新建表格的时候，对`name`字段建立了索引。

> ```javascript
> objectStore.createIndex('name', 'name', { unique: false });
> ```

现在，就可以从`name`找到对应的数据记录了。

> ```javascript
> var transaction = db.transaction(['person'], 'readonly');
> var store = transaction.objectStore('person');
> var index = store.index('name');
> var request = index.get('李四');
> 
> request.onsuccess = function (e) {
>   var result = e.target.result;
>   if (result) {
>     // ...
>   } else {
>     // ...
>   }
> }
> ```

#### Token

##### Access Token

是访问资源接口（API）时所需要的资源凭证

**简单 token 的组成：** uid(用户唯一的身份标识)、time(当前时间的时间戳)、sign（签名，token 的前几位以哈希算法压缩成的一定长度的十六进制字符串）

**特点：**

- 服务端无状态化、可扩展性好
- 支持移动端设备
- 安全
- 支持跨程序调用

###### token身份验证流程

![](http://jay_ohhh.gitee.io/imagehosting/后台和网络/access token流程.png)

1. 客户端使用用户名跟密码请求登录
2. 服务端收到请求，去验证用户名与密码
3. 验证成功后，服务端会签发一个 token 并把这个 token 发送给客户端
4. 客户端收到 token 以后，会把它存储起来，比如放在 cookie 里或者 localStorage 里
5. 客户端每次向服务端请求资源的时候需要带着服务端签发的 token
6. 服务端收到请求，然后去验证客户端请求里面带着的 token ，如果验证成功，就向客户端返回请求的数据
- **每一次请求都需要携带 token，需要把 token 放到 HTTP 的 Header 里**
- **基于 token 的用户认证是一种服务端无状态的认证方式，服务端不用存放 token 数据。用解析 token 的计算时间换取 session 的存储空间，从而减轻服务器的压力，减少频繁的查询数据库**
- **token 完全由应用管理，所以它可以避开同源策略**

##### Refresh Token

refresh token 是专用于刷新 access token 的 token。如果没有 refresh token，也可以刷新 access token，但每次刷新都要用户输入登录用户名与密码，会很麻烦。有了 refresh token，可以减少这个麻烦，客户端直接用 refresh token 去更新 access token，无需用户进行额外的操作。

###### Refresh Token 验证流程

![](http://jay_ohhh.gitee.io/imagehosting/后台和网络/refresh token流程.png)

##### Access Token 和 Refresh Token 的区别

- Access Token 的有效期比较短，当 Acesss Token 由于过期而失效时，使用 Refresh Token 就可以获取到新的 Token，如果 Refresh Token 也失效了，用户就只能重新登录了。
- Refresh Token 及过期时间是存储在服务器的数据库中，只有在申请新的 Acesss Token 时才会验证，不会对业务接口响应时间造成影响，也不需要向 Session 一样一直保持在内存中以应对大量的请求。

##### JWT（JSON Web Token）

JWT 的原理是，服务器认证以后，生成一个 JSON 对象，发回给用户。以后，用户与服务端通信的时候，都要发回这个 JSON 对象。服务器完全只靠这个对象认定用户身份。为了防止用户篡改数据，服务器在生成这个对象的时候，会加上签名（详见后文）。

服务器就不保存任何 session 数据了，也就是说，**服务器变成无状态了**，从而比较容易实现扩展。

###### JWT的格式

```
Header.Payload.Signature
```

**Header**

Header 部分是一个 JSON 对象，描述 JWT 的元数据，通常是下面的样子。

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

上面代码中，`alg`属性表示签名的算法（algorithm），默认是 HMAC SHA256（写成 HS256）；`typ`属性表示这个令牌（token）的类型（type），JWT 令牌统一写为`JWT`。

最后，将上面的 JSON 对象使用 Base64URL 算法（详见后文）转成字符串。

**Payload**

Payload 部分也是一个 JSON 对象，用来存放实际需要传递的数据。JWT 规定了7个官方字段，供选用。

- iss (issuer)：签发人
- exp (expiration time)：过期时间
- sub (subject)：主题
- aud (audience)：受众
- nbf (Not Before)：生效时间
- iat (Issued At)：签发时间
- jti (JWT ID)：编号

除了官方字段，你还可以在这个部分定义私有字段，下面就是一个例子。

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

注意，JWT 默认是不加密的，任何人都可以读到，所以不要把秘密信息放在这个部分。

这个 JSON 对象也要使用 Base64URL 算法转成字符串。

**Signature**

Signature 部分是对前两部分的签名，防止数据篡改。

首先，需要指定一个密钥（secret）。这个密钥只有服务器才知道，不能泄露给用户。然后，使用 Header 里面指定的签名算法（默认是 HMAC SHA256），按照下面的公式产生签名。

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

算出签名以后，把 Header、Payload、Signature 三个部分拼成一个字符串，每个部分之间用"点"（`.`）分隔，就可以返回给用户。

###### Base64URL

前面提到，Header 和 Payload 串型化的算法是 Base64URL。这个算法跟 Base64 算法基本类似，但有一些小的不同。

JWT 作为一个令牌（token），有些场合可能会放到 URL（比如`api.example.com/?token=xxx`）。Base64 有三个字符`+`、`/`和`=`，在 URL 里面有特殊含义，所以要被替换掉：`=`被省略、`+`替换成`-`，`/`替换成`_` 。这就是 Base64URL 算法。

###### JWT的使用方式

客户端收到服务器返回的 JWT，可以储存在 Cookie 里面，也可以储存在 localStorage。

此后，客户端每次与服务器通信，都要带上这个 JWT。你可以把它放在 Cookie 里面自动发送，但是这样不能跨域，所以更好的做法是放在 HTTP 请求的头信息`Authorization`字段里面。

```
Authorization: Bearer <token>
```

另一种做法是，跨域的时候，JWT 就放在 POST 请求的数据体里面。

Bearer：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Authentication

###### JWT 的几个特点

（1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

（2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

（3）JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

（4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

（5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

（6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。

#### 同源限制

##### 概述

###### 含义

1995年，同源政策由 Netscape 公司引入浏览器。目前，所有浏览器都实行这个政策。

最初，它的含义是指，A 网页设置的 Cookie，B 网页不能打开，除非这两个网页“同源”。所谓“同源”指的是“三个相同”。

> - 协议相同
> - 域名相同
> - 端口相同（这点可以忽略，详见下文）

举例来说，`http://www.example.com/dir/page.html`这个网址，协议是`http://`，域名是`www.example.com`，端口是`80`（默认端口可以省略），它的同源情况如下。

- `http://www.example.com/dir2/other.html`：同源
- `http://example.com/dir/other.html`：不同源（域名不同）
- `http://v2.www.example.com/dir/other.html`：不同源（域名不同）
- `http://www.example.com:81/dir/other.html`：不同源（端口不同）
- `https://www.example.com/dir/page.html`：不同源（协议不同）

注意，标准规定端口不同的网址不是同源（比如8000端口和8001端口不是同源），但是浏览器没有遵守这条规定。实际上，同一个网域的不同端口，是可以互相读取 Cookie 的。

###### 目的

同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据。

设想这样一种情况：A 网站是一家银行，用户登录以后，A 网站在用户的机器上设置了一个 Cookie，包含了一些隐私信息。用户离开 A 网站以后，又去访问 B 网站，如果没有同源限制，B 网站可以读取 A 网站的 Cookie，那么隐私就泄漏了。更可怕的是，Cookie 往往用来保存用户的登录状态，如果用户没有退出登录，其他网站就可以冒充用户，为所欲为。因为浏览器同时还规定，提交表单不受同源政策的限制。

由此可见，同源政策是必需的，否则 Cookie 可以共享，互联网就毫无安全可言了。

###### 限制范围

随着互联网的发展，同源政策越来越严格。目前，如果非同源，共有三种行为受到限制。

> （1） 无法读取非同源网页的 Cookie、LocalStorage 和 IndexedDB。
> 
> （2） 无法接触非同源网页的 DOM。
> 
> （3） 无法向非同源地址发送 AJAX 请求（可以发送，但浏览器会拒绝接受响应）。

另外，通过 JavaScript 脚本可以拿到其他窗口的`window`对象。如果是非同源的网页，目前允许一个窗口可以接触其他网页的`window`对象的九个属性和四个方法。

- window.closed
- window.frames
- window.length
- window.location
- window.opener
- window.parent
- window.self
- window.top
- window.window
- window.blur()
- window.close()
- window.focus()
- window.postMessage()

上面的九个属性之中，只有`window.location`是可读写的，其他八个全部都是只读。而且，即使是`location`对象，非同源的情况下，也只允许调用`location.replace()`方法和写入`location.href`属性。

虽然这些限制是必要的，但是有时很不方便，合理的用途也受到影响。下面介绍如何规避上面的限制。

##### Cookie

https://juejin.cn/post/7052507369690890270

Cookie 是服务器写入浏览器的一小段信息，只有同源的网页才能共享。如果两个网页一级域名相同，只是次级域名不同，浏览器允许通过设置`document.domain`共享 Cookie。

举例来说，A 网页的网址是`http://w1.example.com/a.html`，B 网页的网址是`http://w2.example.com/b.html`，那么只要设置相同的`document.domain`，两个网页就可以共享 Cookie。因为浏览器通过`document.domain`属性来检查是否同源。

```js
// 两个网页都需要设置
document.domain = 'example.com';
```

注意，A 和 B 两个网页都需要设置`document.domain`属性，才能达到同源的目的。因为设置`document.domain`的同时，会把端口重置为`null`，因此如果只设置一个网页的`document.domain`，会导致两个网址的端口不同，还是达不到同源的目的。

现在，A 网页通过脚本设置一个 Cookie。

```
document.cookie = "test1=hello";
```

B 网页就可以读到这个 Cookie。

```
var allCookie = document.cookie;
```

注意，这种方法只适用于 Cookie 和 iframe 窗口，LocalStorage 和 IndexedDB 无法通过这种方法，规避同源政策，而要使用下文介绍 PostMessage API。

另外，服务器也可以在设置 Cookie 的时候，指定 Cookie 的所属域名为一级域名，比如`example.com`。

```
Set-Cookie: key=value; domain=example.com; path=/
```

这样的话，二级域名和三级域名不用做任何设置，都可以读取这个 Cookie。

##### iframe 和多窗口通信

`iframe`元素可以在当前网页之中，嵌入其他网页。每个`iframe`元素形成自己的窗口，即有自己的`window`对象。`iframe`窗口之中的脚本，可以获得父窗口和子窗口。但是，只有在同源的情况下，父窗口和子窗口才能通信；如果跨域，就无法拿到对方的 DOM。

比如，父窗口运行下面的命令，如果`iframe`窗口不是同源，就会报错。

```js
document
.getElementById("myIFrame")
.contentWindow
.document
// Uncaught DOMException: Blocked a frame from accessing a cross-origin frame.
```

上面命令中，父窗口想获取子窗口的 DOM，因为跨域导致报错。

反之亦然，子窗口获取主窗口的 DOM 也会报错。

```js
window.parent.document.body
// 报错
```

这种情况不仅适用于`iframe`窗口，还适用于`window.open`方法打开的窗口，只要跨域，父窗口与子窗口之间就无法通信。

如果两个窗口一级域名相同，只是二级域名不同，那么设置上一节介绍的`document.domain`属性，就可以规避同源政策，拿到 DOM。

对于完全不同源的网站，目前有两种方法，可以解决跨域窗口的通信问题。

> - 片段识别符（fragment identifier）
> - 跨文档通信API（Cross-document messaging）

###### 片段识别符

片段标识符（fragment identifier）指的是，URL 的`#`号后面的部分，比如`http://example.com/x.html#fragment`的`#fragment`。如果只是改变片段标识符，页面不会重新刷新。

父窗口可以把信息，写入子窗口的片段标识符。

```js
var src = originURL + '#' + data;
document.getElementById('myIFrame').src = src;
```

上面代码中，父窗口把所要传递的信息，写入 iframe 窗口的片段标识符。

子窗口通过监听`hashchange`事件得到通知。

```js
window.onhashchange = checkMessage;

function checkMessage() {
  var message = window.location.hash;
  // ...
}
```

同样的，子窗口也可以改变父窗口的片段标识符。

```js
parent.location.href = target + '#' + hash;
```

###### window.postMessage()

上面的这种方法属于破解，HTML5 为了解决这个问题，引入了一个全新的API：跨文档通信 API（Cross-document messaging）。

这个 API 为`window`对象新增了一个`window.postMessage`方法，允许跨窗口通信，不论这两个窗口是否同源。举例来说，父窗口`aaa.com`向子窗口`bbb.com`发消息，调用`postMessage`方法就可以了。

```js
// 父窗口打开一个子窗口
var popup = window.open('http://bbb.com', 'title');
// 父窗口向子窗口发消息
popup.postMessage('Hello World!', 'http://bbb.com');
```

`postMessage`方法的第一个参数是具体的信息内容，第二个参数是接收消息的窗口的源（origin），即“协议 + 域名 + 端口”。也可以设为`*`，表示不限制域名，向所有窗口发送。

子窗口向父窗口发送消息的写法类似。

```js
// 子窗口向父窗口发消息
window.opener.postMessage('Nice to see you', 'http://aaa.com');
```

父窗口和子窗口都可以通过`message`事件，监听对方的消息。

```js
// 父窗口和子窗口都可以用下面的代码，
// 监听 message 消息
window.addEventListener('message', function (e) {
  console.log(e.data);
},false);
```

`message`事件的参数是事件对象`event`，提供以下三个属性。

> - `event.source`：发送消息的窗口
> - `event.origin`: 消息发向的网址
> - `event.data`: 消息内容

下面的例子是，子窗口通过`event.source`属性引用父窗口，然后发送消息。

```js
window.addEventListener('message', receiveMessage);
function receiveMessage(event) {
  event.source.postMessage('Nice to see you!', '*');
}
```

上面代码有几个地方需要注意。首先，`receiveMessage`函数里面没有过滤信息的来源，任意网址发来的信息都会被处理。其次，`postMessage`方法中指定的目标窗口的网址是一个星号，表示该信息可以向任意网址发送。通常来说，这两种做法是不推荐的，因为不够安全，可能会被恶意利用。

`event.origin`属性可以过滤不是发给本窗口的消息。

```js
window.addEventListener('message', receiveMessage);
function receiveMessage(event) {
  if (event.origin !== 'http://aaa.com') return;
  if (event.data === 'Hello World') {
    event.source.postMessage('Hello', event.origin);
  } else {
    console.log(event.data);
  }
}
```

###### LocalStorage

通过`window.postMessage`，读写其他窗口的 LocalStorage 也成为了可能。

下面是一个例子，主窗口写入 iframe 子窗口的`localStorage`。

```js
window.onmessage = function(e) {
  if (e.origin !== 'http://bbb.com') {
    return;
  }
  var payload = JSON.parse(e.data);
  localStorage.setItem(payload.key, JSON.stringify(payload.data));
};
```

上面代码中，子窗口将父窗口发来的消息，写入自己的 LocalStorage。

父窗口发送消息的代码如下。

```js
var win = document.getElementsByTagName('iframe')[0].contentWindow;
var obj = { name: 'Jack' };
win.postMessage(
  JSON.stringify({key: 'storage', data: obj}),
  'http://bbb.com'
);
```

加强版的子窗口接收消息的代码如下。

```js
window.onmessage = function(e) {
  if (e.origin !== 'http://bbb.com') return;
  var payload = JSON.parse(e.data);
  switch (payload.method) {
    case 'set':
      localStorage.setItem(payload.key, JSON.stringify(payload.data));
      break;
    case 'get':
      var parent = window.parent;
      var data = localStorage.getItem(payload.key);
      parent.postMessage(data, 'http://aaa.com');
      break;
    case 'remove':
      localStorage.removeItem(payload.key);
      break;
  }
};
```

加强版的父窗口发送消息代码如下。

```js
var win = document.getElementsByTagName('iframe')[0].contentWindow;
var obj = { name: 'Jack' };
// 存入对象
win.postMessage(
  JSON.stringify({key: 'storage', method: 'set', data: obj}),
  'http://bbb.com'
);
// 读取对象
win.postMessage(
  JSON.stringify({key: 'storage', method: "get"}),
  "*"
);
window.onmessage = function(e) {
  if (e.origin != 'http://aaa.com') return;
  console.log(JSON.parse(e.data).name);
};
```

##### AJAX

同源政策规定，AJAX 请求只能发给同源的网址，否则就报错。

除了架设服务器代理（浏览器请求同源服务器，再由后者请求外部服务），有三种方法规避这个限制。

> - JSONP
> - WebSocket
> - CORS

###### JSONP

JSONP 是服务器与客户端跨源通信的常用方法。最大特点就是简单易用，没有兼容性问题，老式浏览器全部支持，服务端改造非常小。

它的做法如下。

第一步，网页添加一个`<script>`元素，向服务器请求一个脚本，这不受同源政策限制，可以跨域请求。

```html
<script src="http://api.foo.com?callback=bar"></script>
```

注意，请求的脚本网址有一个`callback`参数（`?callback=bar`），用来告诉服务器，客户端的回调函数名称（`bar`）。

第二步，服务器收到请求后，拼接一个字符串，将 JSON 数据放在函数名里面，作为字符串返回（`bar({...})`）。

第三步，客户端会将服务器返回的字符串，作为代码解析，因为浏览器认为，这是`<script>`标签请求的脚本内容。这时，客户端只要定义了`bar()`函数，就能在该函数体内，拿到服务器返回的 JSON 数据。

下面看一个实例。首先，网页动态插入`<script>`元素，由它向跨域网址发出请求。

```js
function addScriptTag(src) {
  var script = document.createElement('script');
  script.setAttribute('type', 'text/javascript');
  script.src = src;
  document.body.appendChild(script);
}

window.onload = function () {
  addScriptTag('http://example.com/ip?callback=foo');
}

function foo(data) {
  console.log('Your public IP address is: ' + data.ip);
};
```

上面代码通过动态添加`<script>`元素，向服务器`example.com`发出请求。注意，该请求的查询字符串有一个`callback`参数，用来指定回调函数的名字，这对于 JSONP 是必需的。

服务器收到这个请求以后，会将数据放在回调函数的参数位置返回。

```js
foo({
  'ip': '8.8.8.8'
});
```

由于`<script>`元素请求的脚本，直接作为代码运行。这时，只要浏览器定义了`foo`函数，该函数就会立即调用。作为参数的 JSON 数据被视为 JavaScript 对象，而不是字符串，因此避免了使用`JSON.parse`的步骤。

###### WebSocket

WebSocket 是一种通信协议，使用`ws://`（非加密）和`wss://`（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。

下面是一个例子，浏览器发出的 WebSocket 请求的头信息（摘自[维基百科](https://en.wikipedia.org/wiki/WebSocket)）。

```
GET /chat HTTP/1.1
Host: server.example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9GBhXDw==
Sec-WebSocket-Protocol: chat, superchat
Sec-WebSocket-Version: 13
Origin: http://example.com
```

上面代码中，有一个字段是`Origin`，表示该请求的请求源（origin），即发自哪个域名。

正是因为有了`Origin`这个字段，所以 WebSocket 才没有实行同源政策。因为服务器可以根据这个字段，判断是否许可本次通信。如果该域名在白名单内，服务器就会做出如下回应。

```
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: HSmrc0sMlYUkAGmm5OPpG2HaGWk=
Sec-WebSocket-Protocol: chat
```

###### CORS

CORS 是跨源资源分享（Cross-Origin Resource Sharing）的缩写。它是 W3C 标准，属于跨源 AJAX 请求的根本解决方法。相比 JSONP 只能发`GET`请求，CORS 允许任何类型的请求。

下一章将详细介绍，如何通过 CORS 完成跨源 AJAX 请求。

#### CORS

##### 简介

CORS 需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能。

整个 CORS 通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS 通信与普通的 AJAX 通信没有差别，代码完全一样。浏览器一旦发现 AJAX 请求跨域，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感知。因此，实现 CORS 通信的关键是服务器。只要服务器实现了 CORS 接口，就可以跨域通信。

##### 两种请求

CORS 请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。

只要同时满足以下两大条件，就属于简单请求。

（1）请求方法是以下三种方法之一。

> - HEAD
> - GET
> - POST

（2）HTTP 的头信息不超出以下几种字段。

> - Accept
> - Accept-Language
> - Content-Language
> - Last-Event-ID
> - Content-Type：只限于三个值`application/x-www-form-urlencoded`、`multipart/form-data`、`text/plain`

凡是不同时满足上面两个条件，就属于非简单请求。一句话，简单请求就是简单的 HTTP 方法与简单的 HTTP 头信息的结合。

这样划分的原因是，表单在历史上一直可以跨域发出请求。简单请求就是表单请求，浏览器沿袭了传统的处理方式，不把行为复杂化，否则开发者可能转而使用表单，规避 CORS 的限制。对于非简单请求，浏览器会采用新的处理方式。

##### 简单请求

###### 基本流程

对于简单请求，浏览器直接发出 CORS 请求。具体来说，就是在头信息之中，增加一个`Origin`字段。

下面是一个例子，浏览器发现这次跨域 AJAX 请求是简单请求，就自动在头信息之中，添加一个`Origin`字段。

```
GET /cors HTTP/1.1
Origin: http://api.bob.com
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

上面的头信息中，`Origin`字段用来说明，本次请求来自哪个域（协议 + 域名 + 端口）。服务器根据这个值，决定是否同意这次请求。

如果`Origin`指定的源，不在许可范围内，服务器会返回一个正常的 HTTP 回应。浏览器发现，这个回应的头信息没有包含`Access-Control-Allow-Origin`字段（详见下文），就知道出错了，从而抛出一个错误，被`XMLHttpRequest`的`onerror`回调函数捕获。注意，这种错误无法通过状态码识别，因为 HTTP 回应的状态码有可能是200。

如果`Origin`指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。

```
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

上面的头信息之中，有三个与 CORS 请求相关的字段，都以`Access-Control-`开头。

**（1）`Access-Control-Allow-Origin`**

该字段是必须的。它的值要么是请求时`Origin`字段的值，要么是一个`*`，表示接受任意域名的请求。

**（2）`Access-Control-Allow-Credentials`**

该字段可选。它的值是一个布尔值，表示是否允许发送 Cookie。默认情况下，Cookie 不包括在 CORS 请求之中。设为`true`，即表示服务器明确许可，浏览器可以把 Cookie 包含在请求中，一起发给服务器。这个值也只能设为`true`，如果服务器不要浏览器发送 Cookie，不发送该字段即可。

**（3）`Access-Control-Expose-Headers`**

该字段可选。CORS 请求时，`XMLHttpRequest`对象的`getResponseHeader()`方法只能拿到6个服务器返回的基本字段：`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`。如果想拿到其他字段，就必须在`Access-Control-Expose-Headers`里面指定。上面的例子指定，`getResponseHeader('FooBar')`可以返回`FooBar`字段的值。

###### withCredentials 属性

上面说到，CORS 请求默认不包含 Cookie 信息（以及 HTTP 认证信息等），这是为了降低 CSRF 攻击的风险。但是某些场合，服务器可能需要拿到 Cookie，这时需要服务器显式指定`Access-Control-Allow-Credentials`字段，告诉浏览器可以发送 Cookie。

```
Access-Control-Allow-Credentials: true
```

同时，开发者必须在 AJAX 请求中打开`withCredentials`属性。

```
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```

否则，即使服务器要求发送 Cookie，浏览器也不会发送。或者，服务器要求设置 Cookie，浏览器也不会处理。

但是，有的浏览器默认将`withCredentials`属性设为`true`。这导致如果省略`withCredentials`设置，这些浏览器可能还是会一起发送 Cookie。这时，可以显式关闭`withCredentials`。

```
xhr.withCredentials = false;
```

需要注意的是，如果服务器要求浏览器发送 Cookie，`Access-Control-Allow-Origin`就不能设为星号，必须指定明确的、与请求网页一致的域名。同时，Cookie 依然遵循同源政策，只有用服务器域名设置的 Cookie 才会上传，其他域名的 Cookie 并不会上传，且（跨域）原网页代码中的`document.cookie`也无法读取服务器域名下的 Cookie。

##### 非简单请求

###### 预检请求

非简单请求是那种对服务器提出特殊要求的请求，比如请求方法是`PUT`或`DELETE`，或者`Content-Type`字段的类型是`application/json`。

非简单请求的 CORS 请求，会在正式通信之前，增加一次 HTTP 查询请求，称为“预检”请求（preflight）。浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些 HTTP 方法和头信息字段。只有得到肯定答复，浏览器才会发出正式的`XMLHttpRequest`请求，否则就报错。这是为了防止这些新增的请求，对传统的没有 CORS 支持的服务器形成压力，给服务器一个提前拒绝的机会，这样可以防止服务器收到大量`DELETE`和`PUT`请求，这些传统的表单不可能跨域发出的请求。

下面是一段浏览器的 JavaScript 脚本。

```js
var url = 'http://api.alice.com/cors';
var xhr = new XMLHttpRequest();
xhr.open('PUT', url, true);
xhr.setRequestHeader('X-Custom-Header', 'value');
xhr.send();
```

上面代码中，HTTP 请求的方法是`PUT`，并且发送一个自定义头信息`X-Custom-Header`。

浏览器发现，这是一个非简单请求，就自动发出一个“预检”请求，要求服务器确认可以这样请求。下面是这个“预检”请求的 HTTP 头信息。

```
OPTIONS /cors HTTP/1.1
Origin: http://api.bob.com
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: X-Custom-Header
Host: api.alice.com
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

“预检”请求用的请求方法是`OPTIONS`，表示这个请求是用来询问的。头信息里面，关键字段是`Origin`，表示请求来自哪个源。

除了`Origin`字段，“预检”请求的头信息包括两个特殊字段。

**（1）`Access-Control-Request-Method`**

该字段是必须的，用来列出浏览器的 CORS 请求会用到哪些 HTTP 方法，上例是`PUT`。

**（2）`Access-Control-Request-Headers`**

该字段是一个逗号分隔的字符串，指定浏览器 CORS 请求会额外发送的头信息字段，上例是`X-Custom-Header`。

###### 预检请求的回应

服务器收到“预检”请求以后，检查了`Origin`、`Access-Control-Request-Method`和`Access-Control-Request-Headers`字段以后，确认允许跨源请求，就可以做出回应。

```
HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Content-Type: text/html; charset=utf-8
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```

上面的 HTTP 回应中，关键的是`Access-Control-Allow-Origin`字段，表示`http://api.bob.com`可以请求数据。该字段也可以设为星号，表示同意任意跨源请求。

```
Access-Control-Allow-Origin: *
```

如果服务器否定了“预检”请求，会返回一个正常的 HTTP 回应，但是没有任何 CORS 相关的头信息字段，或者明确表示请求不符合条件。

```
OPTIONS http://api.bob.com HTTP/1.1
Status: 200
Access-Control-Allow-Origin: https://notyourdomain.com
Access-Control-Allow-Method: POST
```

上面的服务器回应，`Access-Control-Allow-Origin`字段明确不包括发出请求的`http://api.bob.com`。

这时，浏览器就会认定，服务器不同意预检请求，因此触发一个错误，被`XMLHttpRequest`对象的`onerror`回调函数捕获。控制台会打印出如下的报错信息。

```
XMLHttpRequest cannot load http://api.alice.com.
Origin http://api.bob.com is not allowed by Access-Control-Allow-Origin.
```

服务器回应的其他 CORS 相关字段如下。

```
Access-Control-Allow-Methods: GET, POST, PUT
Access-Control-Allow-Headers: X-Custom-Header
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 1728000
```

**（1）`Access-Control-Allow-Methods`**

该字段必需，它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。注意，返回的是所有支持的方法，而不单是浏览器请求的那个方法。这是为了避免多次“预检”请求。

**（2）`Access-Control-Allow-Headers`**

如果浏览器请求包括`Access-Control-Request-Headers`字段，则`Access-Control-Allow-Headers`字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在“预检”中请求的字段。

**（3）`Access-Control-Allow-Credentials`**

该字段与简单请求时的含义相同。

**（4）`Access-Control-Max-Age`**

该字段可选，用来指定本次预检请求的有效期，单位为秒。上面结果中，有效期是20天（1728000秒），即允许缓存该条回应1728000秒（即20天），在此期间，不用发出另一条预检请求。

##### 浏览器的正常请求和回应

一旦服务器通过了“预检”请求，以后每次浏览器正常的 CORS 请求，就都跟简单请求一样，会有一个`Origin`头信息字段。服务器的回应，也都会有一个`Access-Control-Allow-Origin`头信息字段。

下面是“预检”请求之后，浏览器的正常 CORS 请求。

```
PUT /cors HTTP/1.1
Origin: http://api.bob.com
Host: api.alice.com
X-Custom-Header: value
Accept-Language: en-US
Connection: keep-alive
User-Agent: Mozilla/5.0...
```

上面头信息的`Origin`字段是浏览器自动添加的。

下面是服务器正常的回应。

```
Access-Control-Allow-Origin: http://api.bob.com
Content-Type: text/html; charset=utf-8
```

上面头信息中，`Access-Control-Allow-Origin`字段是每次回应都必定包含的。

##### 与 JSONP 的比较

CORS 与 JSONP 的使用目的相同，但是比 JSONP 更强大。JSONP 只支持`GET`请求，CORS 支持所有类型的 HTTP 请求。JSONP 的优势在于支持老式浏览器，以及可以向不支持 CORS 的网站请求数据。

##### 参考链接

- [Using CORS](https://www.html5rocks.com/en/tutorials/cors/), Monsur Hossain
- [HTTP access control (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS), MDN
- [CORS](https://frontendian.co/cors), Ryan Miller
- [Do You Really Know CORS?](http://performantcode.com/web/do-you-really-know-cors), Grzegorz Mirek

#### 常见跨域方式

https://www.cnblogs.com/sdcs/p/8484905.html

1. jsonp；

2. document.domain + iframe，二级域名和顶级域名的资源共享，两个页面设置同样的document.domain属性可以实现跨域；

3. window.name + iframe，页面中打开页面后window.name任然存在且可以访问；

4. location.hash + iframe

5. postMassage，html5新增接口，多窗口中的信息传递；

6. 跨域资源共享CORS，服务器端Access-Control-Allow-Origin设置可以通过的源，前端页面如果需要接收服务器带回的Cookie信息，需要打开`xhr.withCredentials = true;`确定请求是否携带Cookie；
   
   > 普通跨域请求：只服务端设置Access-Control-Allow-Origin即可，前端无须设置，若要带cookie请求：前后端都需要设置。

7. nginx反向代理；

8. nodejs中间件代理跨域；

9. WebSocket协议跨域

#### null和undefined区别

`null`代表空值代表空对象指针，没有对象此处不应该有值，对象原型链的终点；`undefined`代表声明但未赋值的变量

```javascript
typeof undefined //  undefined
typeof null // object
null == undefined // true
undefined + 6 // NaN
null + 6 // 6
```

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

#### hash和history

##### Hash模式

###### 1、定义

`hash` 模式是一种把前端路由的路径用井号 `#` 拼接在真实 `url` 后面的模式。当井号 `#` 后面的路径发生变化时，浏览器并不会重新发起请求，而是会触发 `onhashchange` 事件。

###### 2、网页url组成部分

（1）了解几个url的属性

| 属性                 | 含义   |
| ------------------ | ---- |
| location.protocal  | 协议   |
| location.hostname  | 主机名  |
| location.host      | 主机   |
| location.port      | 端口号  |
| location.patchname | 访问页面 |
| location.search    | 搜索内容 |
| location.hash      | 哈希值  |

（2）演示

**下面用一个网址来演示以上属性：**

```js
//http://127.0.0.1:8001/01-hash.html?a=100&b=20#/aaa/bbb
location.protocal // 'http:'
localtion.hostname // '127.0.0.1'
location.host // '127.0.0.1:8001'
location.port //8001
location.pathname //'01-hash.html'
location.serach // '?a=100&b=20'
location.hash // '#/aaa/bbb'
```

###### 3、hash的特点

- hash变化会触发网页跳转，即浏览器的前进和后退。
- `hash` 可以改变 `url` ，但是不会触发页面重新加载（hash的改变是记录在 `window.history` 中），即不会刷新页面。也就是说，所有页面的跳转都是在客户端进行操作。因此，这并不算是一次 `http` 请求，所以这种模式不利于 `SEO` 优化。`hash` 只能修改 `#` 后面的部分，所以只能跳转到与当前 `url` 同文档的 `url` 。
- `hash` 通过 `window.onhashchange` 的方式，来监听 `hash` 的改变，借此实现无刷新跳转的功能。
- `hash`（#以及后面的） 永远不会提交到 `server` 端（可以理解为只在前端自生自灭）。

##### History模式

###### 1、定义

`history API` 是 `H5` 提供的新特性，允许开发者**直接更改前端路由**，即更新浏览器 `URL` 地址而**不重新发起请求**。

###### 2、与hash的区别

我们用一个例子来演示， `hash` 与 `history` 在浏览器下刷新时的区别。**具体如下：**

**正常页面浏览**

```bash
https://github.com/xxx 刷新页面

https://github.com/xxx/yyy 刷新页面

https://github.com/xxx/yyy/zzz 刷新页面
```

**改造H5 history模式**

```bash
https://github.com/xxx 刷新页面

https://github.com/xxx/yyy 前端跳转，不刷新页面

https://github.com/xxx/yyy/zzz 前端跳转，不刷新页面
```

###### 3、history的API

下面阐述几种 `HTML5` 新增的 `history API` 。**具体如下表：**

| API                                       | 定义                                                                                                                                      |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| history.pushState(data, title [, url])    | pushState主要用于**往历史记录堆栈顶部添加一条记录**。各参数解析如下：**①data**会在onpopstate事件触发时作为参数传递过去；**②title**为页面标题，当前所有浏览器都会忽略此参数；③**url**为页面地址，可选，缺少时表示为当前页地址 |
| history.replaceState(data, title [, url]) | 更改当前的历史记录，参数同上； 上面的pushState是添加，这个更改                                                                                                    |
| history.state                             | 用于存储以上方法的data数据，不同浏览器的读写权限不一样                                                                                                           |
| window.onpopstate                         | 响应pushState或者replaceState的调用                                                                                                            |

###### 4、history的特点

对于 `history` 来说，主要有以下特点：

- 新的 `url` 可以是与当前 `url` 同源的任意 `url` ，也可以是与当前 `url` 一样的地址，但是这样会导致的一个问题是，会把**重复的这一次操作**记录到栈当中。
- 通过 `history.state` ，添加任意类型的数据到记录中。
- 可以额外设置 `title` 属性，以便后续使用。
- 通过 `pushState` 、 `replaceState` 来实现无刷新跳转的功能。

###### 5、存在问题

对于 `history` 来说，确实解决了不少 `hash` 存在的问题，但是也带来了新的问题。**具体如下：**

- 使用 `history` 模式时，在对当前的页面进行刷新时，此时浏览器会重新发起请求。如果 `nginx` 没有匹配得到当前的 `url` ，就会出现 `404` 的页面。
- 而对于 `hash` 模式来说，  它虽然看着是改变了 `url` ，但不会被包括在 `http` 请求中。所以，它算是被用来指导浏览器的动作，并不影响服务器端。因此，改变 `hash` 并没有真正地改变 `url` ，所以页面路径还是之前的路径， `nginx` 也就不会拦截。
- 因此，在使用 `history` 模式时，需要**通过服务端来允许地址可访问**，如果没有设置，就很容易导致出现 `404` 的局面。

#### Server-Sent Events

http://www.ruanyifeng.com/blog/2017/05/server-sent_events.html

##### SSE 的特点

SSE 与 WebSocket 作用相似，都是建立浏览器与服务器之间的通信渠道，然后服务器向浏览器推送信息。

总体来说，WebSocket 更强大和灵活。因为它是全双工通道，可以双向通信；SSE 是单向通道，只能服务器向浏览器发送，因为流信息本质上就是下载。如果浏览器向服务器发送信息，就变成了另一次 HTTP 请求。

但是，SSE 也有自己的优点。

> - SSE 使用 HTTP 协议，现有的服务器软件都支持。WebSocket 是一个独立协议。
> - SSE 属于轻量级，使用简单；WebSocket 协议相对复杂。
> - SSE 默认支持断线重连，WebSocket 需要自己实现。
> - SSE 一般只用来传送文本，二进制数据需要编码后传送，WebSocket 默认支持传送二进制数据。
> - SSE 支持自定义发送的消息类型。
> - SSE IE浏览器不支持。

#### 模块化规范

模块内具有作用域。

一个组件模块文件内声明的顶级变量或者是引入的外部模块的顶级变量，不会因为组件销毁而销毁。

```jsx
// other.js
export const s = 1; // 不会因为组件销毁而销毁

// Component.jsx
import { s } from 'other.js'
let a = 2; // 不会因为组件销毁而销毁

const Component = ()=>{
  console.log(a,s)
	return <div></div>
}
```



##### Node端

###### CommonJS

- 运行时同步加载（动态加载）
  
  > 同步意味着阻塞加载，浏览器资源是异步加载的，因此有了AMD CMD解决方案
  >
  > CommonJS 加载的是一个对象（即module.exports属性），该对象只有在脚本运行完才会生成。
  
- 输出值的拷贝，会缓存第一次运行的结果

**基本语法**

- 暴露模块：`module.exports.xxx = value`或`exports.xxx = value`
  
  > **exports指向的是module.exports**
  
  ```js
  // module.exports和exports必须指向同一个值
  exports = module.exports = {};
  
  // wrong: exports不再指向module.exports
  exports = {};
  ```
  
  保险起见，可使用以下方式：
  
  - modules.exports.foo = bar
  
  - export.foo = bar
  
- 引入模块：`require(xxx)`,如果是第三方模块，xxx为模块名；如果是自定义模块，xxx为模块文件路径

##### 浏览器端

###### AMD

- 在浏览器端中并行异步加载 

- 依赖前置，换句话说，在解析和执行当前模块之前，模块作者必须指明当前模块所依赖的模块

- 提前执行依赖

实现：RequireJS

###### CMD

- 在浏览器端中异步加载

- 按需加载

- 依赖就近，依赖可以就近书写，可以把依赖写进你的代码中的任意一行

- 延迟执行依赖

实现：Sea.js



##### 浏览器 和 Node 兼容端

###### UMD

统一AMD和CommonJS规范，解决跨平台问题。既可以在 node/webpack 环境中被 `require` 引用，也可以在浏览器中直接用 CDN 被 `script.src` 引入。

```js
(function (root, factory) {
  if (typeof define === "function" && define.amd) {
    // AMD
    define(["jquery"], factory);
  } else if (typeof exports === "object") {
    // CommonJS
    module.exports = factory(require("jquery"));
  } else {
    // 全局变量
    root.returnExports = factory(root.jQuery);
  }
})(this, function ($) {
  // ...
});
```



###### ESM

[Pure ESM package](https://gist.github.com/sindresorhus/a39789f98801d908bbc7ff3ecc99d99c)

浏览器和服务器通用的模块解决方案

ES6 模块设计思想：尽量的静态化、使得编译时就能确定模块的依赖关系，以及输入和输出的变量（CommonJS和AMD模块，都只能在运行时确定这些东西）。

- 编译时加载（静态加载）

- 动态引用，不会缓存值

- 输出值的引用

  > 当ES6遇到import时，不会像CommonJS一样去执行模块，而是生成一个动态的只读引用（在代码静态解析阶段就会生成），当真正需要的时候再到模块里去取值，所以ES6模块是动态引用，并且不会缓存值。
  > 
  > 编译：类似翻译，就是将源代码翻译成机器能识别的代码。
  > 
  > 运行：就是将代码跑起来，被装载到内存中去了。

##### [What's the purpose of `Object.defineProperty(exports, "__esModule", { value: !0 })`?](https://stackoverflow.com/questions/50943704/whats-the-purpose-of-object-definepropertyexports-esmodule-value-0)

**__esModule 标识**

不知道是 JS 圈子里的谁最先提出了 `__esModule` 这个解决方案，现在市面上的打包器都非常默契地遵守了这个约定。

exports是node的一个全局对象。

表面上看就是把一个导出对象标识为一个 ES 模块：

```
exports.__esModule = true
```

或

```
Object.defineProperty(exports, '__esModule', { value: true })
```



**Webpack 的处理方法**

上面 ES 模块中导入 CommonJS 模块的例子，在 Webpack 4.43.0 打包后变成了这样（去掉所有注释）：

```javascript
(function(modules) {
  // ...
  function __webpack_require__ (moduleId) {
    // ...
  }

  // ...

  __webpack_require__.d = function(exports, name, getter) {
    if(!__webpack_require__.o(exports, name)) {
      Object.defineProperty(exports, name, { enumerable: true, get: getter });
    }
  };

  __webpack_require__.r = function(exports) {
    if(typeof Symbol !== 'undefined' && Symbol.toStringTag) {
      Object.defineProperty(exports, Symbol.toStringTag, { value: 'Module' });
    }
    Object.defineProperty(exports, '__esModule', { value: true }); // <-- 重点
  };

  __webpack_require__.n = function(module) {
    var getter = module && module.__esModule ?
      function getDefault() { return module['default']; } :
      function getModuleExports() { return module; }; // <-- 兼容处理
    __webpack_require__.d(getter, 'a', getter);
    return getter;
  };

  return __webpack_require__(__webpack_require__.s = 0);
})({
  "./mod.js": function (module, exports) {
    function foo () {}
    function bar () {}
    module.exports = foo
    module.exports.bar = bar
  },
  "./index.js": function (module, __webpack_exports__, __webpack_require__) {
    "use strict";
    __webpack_require__.r(__webpack_exports__); // <-- 标识 ES 模块
    var _mod_js__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__("./mod.js");
    var _mod_js__WEBPACK_IMPORTED_MODULE_0___default = __webpack_require__.n(_mod_js__WEBPACK_IMPORTED_MODULE_0__);

    console.log(_mod_js__WEBPACK_IMPORTED_MODULE_0__["bar"])
    console.log(_mod_js__WEBPACK_IMPORTED_MODULE_0___default.a)
    console.log(_mod_js__WEBPACK_IMPORTED_MODULE_0___default()())
  },
  0: function (module, exports, __webpack_require__) {
    module.exports = __webpack_require__("./index.js");
  }
  // ...
})
```

可以看到在使用侧导入的默认导出实际上是一个 Getter 函数，读取值的时候访问了其自身的 `a` 属性，如果 __esModule 为 `true` 那么 `a` 就是 `module.exports.default`，Getter 调用也返回 `module.exports.default`，否则 `a` 的值和 Getter 返回值就是 `module.exports`。所以在 Webpack 中这样用是没有问题的，Webpack 会根据 __esModule 标识来自动处理 CommonJS 的模块导出对象，兼容 ES 模块中的导入。



**TypeScript 的处理方法**

同样的例子，在 TypeScript 3.9.5 中：

```javascript
// CJS mod.js
function foo () {}
function bar () {}
module.exports = foo
module.exports.bar = bar
// mod.d.ts
declare function foo(): void;

declare namespace foo {
  export function bar(): void;
}

export = foo;
// ESM index.ts
import { bar } from './mod'
import foo from './mod' // <-- 必须配置 esModuleInterop: true

console.log(bar)
console.log(foo)
console.log(foo())
// tsconfig.json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "ES2019",
    "esModuleInterop": true
  }
}
```

输出 `index.js` 是这样的：

```javascript
"use strict";
var __importDefault = (this && this.__importDefault) || function (mod) {
    return (mod && mod.__esModule) ? mod : { "default": mod };
};
Object.defineProperty(exports, "__esModule", { value: true }); // <-- 标识当前模块是 ES 模块

const mod_1 = require("./mod");
const mod_2 = __importDefault(require("./mod"));
console.log(mod_1.bar);
console.log(mod_2.default);
console.log(mod_2.default());
```

开启 `esModuleInterop` （支持使用import d from 'cjs'的方式引入commonjs包）后，如果被导入的模块没有标识 `__esModule`，则默认导入将直接返回一个只含有 `default` 属性的对象。如果不开启 `esModuleInterop` 编译选项，则不能使用默认导入，必须用 `import * as mod from './mod'` 才能通过编译。



#### Intersection Observer

https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver

[利用&quot;交叉观察者&quot;这个小宝贝儿，轻松实现懒加载、吸顶、触底 ❗ - 掘金](https://juejin.cn/post/6844903926815277069)

[谈谈IntersectionObserver懒加载 - 简书](https://www.jianshu.com/p/84a86e41eb2b)

`IntersectionObserver`**接口** (从属于[Intersection Observer API](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)) 提供了一种异步观察目标元素与其祖先元素或顶级文档视窗([viewport](https://developer.mozilla.org/zh-CN/docs/Glossary/Viewport))交叉状态的方法。祖先元素与视窗([viewport](https://developer.mozilla.org/zh-CN/docs/Glossary/Viewport))被称为**根(root)。**

##### polyfill

```js
npm install intersection-observer
// index.js
import 'intersection-observer';
```

##### 构造器

```js
var observer = new IntersectionObserver(callback[, options]);
```

##### 1. IntersectionObserver Callback

当目标元素和根元素相交发生改变时，会触发监视器的回调。

```ts
interface IntersectionObserverCallback {
    (entries: IntersectionObserverEntry[], observer: IntersectionObserver): void;
}
```

- **entries**

一个[`IntersectionObserverEntry`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserverEntry)对象的数组，每个被触发的阈值，都或多或少与指定阈值有偏差。

- **observer**

被调用的[`IntersectionObserver`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver)实例。



##### 2. IntersectionObserver Options & 实例属性

IntersectionObserver Options 是 IntersectionObserver 构造函数的第二个参数，用来配置监视器的部分信息。

如果`options`未指定，observer实例默认使用文档视口作为root，并且没有margin，阈值为0%（意味着即使一像素的改变都会触发回调函数）。你可以指定以下配置：

```ts
interface IntersectionObserverInit {
    root?: Element | Document | null;
    rootMargin?: string;
    threshold?: number | number[];
}
```

**root**：设置监视器的根节点，不传则默认为文档视口。当root为文档视口时，被观察的目标元素被其他元素滚动卷起（隐藏），也会判断为消失。

**rootMargin**： 类似于 CSS 的 margin 属性。用来缩小或扩大 rootBounds，从而影响相交的触发。默认值是"0px 0px 0px 0px"。（top、right、bottom 、 left）

**threshold**：属性决定相交比例为多少时，触发回调函数。取值为 0 ~ 1，或者 0 ~ 1的数组。 当我们把  threshold 设置为 [0, 0.25, 0.5, 0.75, 1]，绿色方块分别在 0%，25%，50%，75%，100% 可见时，触发回调函数。



##### 3. IntersectionObserver Entry

IntersectionObserverEntry 对象提供了目标元素与跟元素相交的详细信息。他有如下几个属性。

```ts
 const observer = new IntersectionObserver(
        (entries) => {
          for (const entry of entries) {
          	// entry: IntersectionObserverEntry
            if (entry.isIntersecting) {
              // do something
            }
            // or
            if (entries[0].intersectionRatio <= 0) return;
          }
        }
      );
```

```ts
interface IntersectionObserverEntry {
 	readonly boundingClientRect: DOMRectReadOnly;
  readonly intersectionRatio: number;
  readonly intersectionRect: DOMRectReadOnly;
  readonly isIntersecting: boolean;
  readonly rootBounds: DOMRectReadOnly | null;
  readonly target: Element;
  readonly time: DOMHighResTimeStamp;
};
```

**time**：发生相交到相应的时间，毫秒。

**rootBounds**：根元素矩形区域的信息，图中蓝色部分区域，如果没有设置根元素则返回null。

**boundingClientRect**：目标元素的矩形区域的信息，图中黑色边框的区域。

**intersectionRect**：目标元素与视口（或根元素）的交叉区域的信息，图中蓝色方块和粉红色方块相交的区域。

**isIntersecting**：目标元素与根元素是否相交。

**intersectionRatio**：目标元素与视口（或根元素）的相交比例。

**target**：目标元素，图中黑色边框的部分。

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/29/16cdccc7f71311ee~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



##### 4. 实例方法

[`IntersectionObserver.disconnect()`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver/disconnect)

使`IntersectionObserver`对象停止监听工作。

[`IntersectionObserver.observe(target)`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver/observe)

使`IntersectionObserver`开始监听一个目标元素。可以监听多个目标元素。

```ts
const observer = new IntersectionObserver((entries) => {});
observer.observe(element1)
observer.observe(element2)
```

[`IntersectionObserver.takeRecords()`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver/takeRecords)

返回所有观察目标的[`IntersectionObserverEntry`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserverEntry)对象数组。

[`IntersectionObserver.unobserve(target)`](https://developer.mozilla.org/zh-CN/docs/Web/API/IntersectionObserver/unobserve)

使`IntersectionObserver`停止监听特定目标元素。



##### 交集的计算

理解交集如何计算是重要的。首先，Intersection Observer API 将任意物体都视为矩形以便计算。这些矩形在包含目标内容的前提下，将被尽可能小的计算。

![Bounding box outlines](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/11/21/16e8c26fbf0623d8~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)目标矩形的边界轮廓



对于根元素，基于 `rootMargin` 的值考虑其矩形边界，这个值会扩大或减小根元素的尺寸。



![Root Margin Calculations](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/11/21/16e8c26fc9f4e57f~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)

#### ResizeObserver

**`ResizeObserver`** 接口可以监听到 [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element) 的内容区域或 [`SVGElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/SVGElement)的边界框改变。内容区域则需要减去内边距padding。（有关内容区域、内边距资料见[盒子模型](https://developer.mozilla.org/docs/Learn/CSS/Introduction_to_CSS/Box_model) ）

##### polyfill

```js
import ResizeObserver from 'resize-observer-polyfill';
```

##### 构造器

```ts
var resizeObserver:ResizeObserver = new ResizeObserver(callback[]);
resizeObserver.observe(element1);
resizeObserver.observe(element2);
```

- callback

```ts
interface ResizeObserverCallback {
	(entries: ResizeObserverEntry[], observer: ResizeObserver): void
}

interface ResizeObserverEntry {
  readonly target: Element; // 被观察的目标元素
  readonly contentRect: DOMRectReadOnly; // 目标元素的矩形区域，包含x,y,width,height,top,right,bottom,left属性），与元素的getBoundingClientRect不同，contentRect的width和height值不包含padding。contentRect.top/y是元素的顶部padding，contentRect.left/x是元素的左侧padding
}

// 实例，只有方法无属性
interface ResizeObserver {
  observe(target: Element): void; // 用于观察一个指定Element 或 SVGElement。
  unobserve(target: Element): void; // 用于结束一个指定的 Element 或 SVGElement 的观察。
  disconnect(): void; // 会停止和取消目标对象上所有对Element 或 SVGElement 的监听。
}
```



##### 示例

以下示例通过观察box的宽度变化从而改变其边框圆角半径。

```js
const resizeObserver = new ResizeObserver(entries => {
  for (let entry of entries) {
    entry.target.style.borderRadius = Math.max(0, 250 - entry.contentRect.width) + 'px';
  }
});
resizeObserver.observe(document.querySelector('.box'));
```

#### MutationObserver

MutationObserver 可以监听对元素的属性的修改、对它的子节点的增删改。



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

| `false`                                                                  | [false](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#Future_reserved_keywords_in_older_standards) 关键字                           |     |
| ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| 0                                                                        | 数值 [zero](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)                                                                       |     |
| -0                                                                       | 数值 负 [zero](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#Number_type)                                                                     |     |
| 0n                                                                       | 当 [BigInt](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) 作为布尔值使用时, 遵从其作为数值的规则. `0n` 是 *falsy* 值.                          |     |
| "", '', ``                                                               | 这是一个空字符串 (字符串的长度为零). JavaScript 中的字符串可用双引号 `**""**`, 单引号 `''`, 或 [模板字面量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals) `` 定义。 |     |
| [null](https://developer.mozilla.org/zh-CN/docs/Glossary/Null)           | [null](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/null) - 缺少值                                                                  |     |
| [undefined](https://developer.mozilla.org/zh-CN/docs/Glossary/undefined) | [undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined) - 原始值                                                        |     |
| [NaN](https://developer.mozilla.org/zh-CN/docs/Glossary/NaN)             | [NaN ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)- 非数值                                                                    |     |

##### 浅合并和深合并

###### 浅合并

将一个对象的第一层合并到另一个对象的第一层，若属性相同，则替换掉。

###### 深合并

将一个对象的所有层级的属性递归合并到另一个对象，若属性相同，则替换掉。