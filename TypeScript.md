## TypeScript

### 简介

#### 什么是TypeScript

TypeScript 是 JavaScript 的类型的超集，它可以编译成纯 JavaScript。编译出来的 JavaScript 可以运行在任何浏览器上。TypeScript 编译工具可以运行在任何服务器和任何系统上。TypeScript 是结构类型系统，其最关键的功能是静态类型检查 (Type Guard) .

**TypeScript 增加了代码的可读性和可维护性**

- 类型系统实际上是最好的文档，大部分的函数看看类型的定义就可以知道如何使用了
- 可以在编译阶段就发现大部分错误，这总比在运行时候出错好
- 增强了编辑器和 IDE 的功能，包括代码补全、接口提示、跳转到定义、代码重构等

**TypeScript 非常包容**

- TypeScript 是 JavaScript 的超集，`.js` 文件可以直接重命名为 `.ts` 即可
- 即使不显式的定义类型，也能够自动做出[类型推论](https://ts.xcatliu.com/basics/type-inference.html)
- TypeScript 的类型系统是图灵完备的，可以定义从简单到复杂的几乎一切类型
- 即使 TypeScript 编译报错，也可以生成 JavaScript 文件
- 兼容第三方库，即使第三方库不是用 TypeScript 写的，也可以编写单独的类型文件供 TypeScript 读取

#### [搜索声明文件](https://microsoft.github.io/TypeSearch/)

### 基础

#### JS 数据类型

 JavaScript 的类型分为两种：基本类型和引用数据类型。 

基本类型包括：布尔值、数值、字符串、`null`、`undefined` 以及 ES6 中的新类型 [`Symbol`](http://es6.ruanyifeng.com/#docs/symbol) 和 [`BigInt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)。 

> **`BigInt`** 是一种内置对象，它提供了一种方法来表示大于 `253 - 1` 的整数。这原本是 Javascript中可以用`Number` 表示的最大数字。**`BigInt`** 可以表示任意大的整数。 

引用数据类型包括： 对象(Object)、数组(Array)、函数(Function) 。

#### TS 数据类型

boolean、number、string、void、null、undefined、never、unkonwn、any、数组、元组、枚举、object

**布尔值**

 使用 `boolean` 定义布尔值类型： 

```ts
let isDone: boolean = false;
let isDone: boolean = Boolean();
```

 `new Boolean()`返回的是一个 `Boolean` 对象 ，不是布尔值
 `Boolean()`返回的是布尔值

**数值**

使用 `number` 定义数值类型：

```ts
let decLiteral: number = 6;
// ES6 中的十六进制表示法
let hexLiteral: number = 0xf00d;
// ES6 中的二进制表示法
let binaryLiteral: number = 0b1010;
// ES6 中的八进制表示法
let octalLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

编译结果：

```js
var decLiteral = 6;
var hexLiteral = 0xf00d;
// ES6 中的二进制表示法
var binaryLiteral = 10;
// ES6 中的八进制表示法
var octalLiteral = 484;
var notANumber = NaN;
var infinityNumber = Infinity
```

 其中 `0b1010` 和 `0o744` 是 [ES6 中的二进制和八进制表示法](http://es6.ruanyifeng.com/#docs/number#二进制和八进制表示法)，它们会被编译为十进制数字。 

**字符串**

使用 `string` 定义字符串类型：

```ts
let myName: string = 'Tom';
```

也可以用 ` 定义 ES6 中的模板字符串，${expr} 用来在模板字符串中嵌入表达式。

**空值**

JavaScript 没有空值（Void）的概念，在 TypeScript 中，可以用 `void` 表示没有任何返回值的函数：

```ts
function alertName(): void {
    alert('My name is Tom');
}
```

声明一个 `void` 类型的变量没有什么用，因为你只能将它赋值为 `undefined` 和 `null`：

```ts
let unusable: void = undefined;
```

**Null 和 Undefined**

在 TypeScript 中，可以使用 `null` 和 `undefined` 来定义这两个原始数据类型：

```ts
let u: undefined = undefined;
let n: null = null;
```

与 `void` 的区别是，`undefined` 和 `null` 是所有类型（包括void）的子类型。

#### 任意值

  `any` 类型，则允许被赋值为任意类型，同时可以将 any 类型的变量赋值给任意类型的变量。

- **任意值的属性和方法**
  
  在任意值上可以添加或访问任何属性和方法。
  
  声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值。 

- **未声明类型的变量**

变量如果在声明（未赋值）的时候，未指定其类型，那么它会被推断为`any`类型。

#### never类型

`never`类型表示的是那些永不存在的值的类型。 例如， never类型总是那些会抛出异常或根本就不会有返回值（函数终点不可到达）的函数表达式或箭头函数表达式的返回值类型（例如无限循环） 。

`never`类型避免出现由于联合类型新增了类型但没有对应的实现而出现的错误，目的是写出类型绝对安全的代码。 

- never是所有类型的子类型并且可以赋值给所有类型。
- 除never类型本身外，没有其他类型是never的子类型或能赋值给never， 即使 any也不可以赋值给never。 
- 在有明确never返回类型注解的函数中，所有return语句（如果有的话）必须有never类型的表达式并且函数的终点必须是不可执行的。

#### [unkonwn类型](https://blog.csdn.net/weixin_33716557/article/details/93177689)

**unknown 和 any 的区别**

unknown 类型会更加严格：在对 unknown 类型的值执行大多数操作之前，我们必须进行某种形式的检查。

对 any 类型的值执行操作之前，我们不必进行任何检查。

any的意思是，认为变量值可以是任何类型（动态的）。

unkonwn的意思是，目前不知道变量值的类型，一般需要在使用前通过判断语句和typeof 运算符、instanceof 运算符、自定义类型保护函数、类型断言（慎用）等缩小 unknown 类型范围。

any 类型本质上是类型系统的一个逃逸舱。作为开发者，这给了我们很大的自由：TypeScript允许我们对 any 类型的值执行任何操作，而无需事先执行任何形式的检查。

**unkonwn 到来的原因**

这许多场景下，any 的机制都太宽松了。使用any类型，可以很容易地编写类型正确但是执行异常的代码。如果我们使用 any 类型，就无法享受 TypeScript 大量的保护机制。但如果能有顶级类型也能默认保持安全呢？这就是 unknown 到来的原因。

就像所有类型都可以被归为 any，所有类型也都可以被归为 unknown。这使得 unknown 成为 TypeScript 类型系统的另一种顶级类型（另一种是 any）。

unknown 类型只能被赋值给 any 类型和 unknown 类型本身。直观的说，这是有道理的：只有能够保存任意类型值的容器才能保存 unknown 类型的值。毕竟我们不知道变量 value 中存储了什么类型的值。

TypeScript 不允许我们对类型为 unknown 的值执行任意操作。相反，我们必须首先执行某种类型检查以缩小我们正在使用的值的类型范围。

**使用场景**

如果一个变量的类型或自身结构是变化的（对象增加属性），就用any。

如果一个变量的类型是固定的，但是目前还不能确定或不想确定，就用unkonwn。

**缩小 `unknown` 类型范围**

我们可以通过不同的方式将 `unknown` 类型缩小为更具体的类型范围，包括 `typeof` 运算符，`instanceof` 运算符和自定义类型保护函数。

 `typeof` 运算符、`instanceof` 运算符：

```ts
function stringifyForLogging(value: unknown): string {
  if (typeof value === "function") {
    // Within this branch, `value` has type `Function`,
    // so we can access the function's `name` property
    const functionName = value.name || "(anonymous)";
    return `[function ${functionName}]`;
  }

  if (value instanceof Date) {
    // Within this branch, `value` has type `Date`,
    // so we can call the `toISOString` method
    return value.toISOString();
  }
}
```

使用自定义类型保护函数缩小 `unknown` 类型范围：

```ts
/**
 * A custom type guard function that determines whether
 * `value` is an array that only contains numbers.
 */
function isNumberArray(value: unknown): value is number[] {
  return (
    Array.isArray(value) &&
    value.every(element => typeof element === "number")
  );
}

const unknownValue: unknown = [15, 23, 8, 4, 42, 16];

if (isNumberArray(unknownValue)) {
  // Within this branch, `unknownValue` has type `number[]`,
  // so we can spread the numbers as arguments to `Math.max`
  const max = Math.max(...unknownValue);
  console.log(max);
}
```

尽管 `unknownValue` 已经被归为 `unknown` 类型，请注意它如何依然在 if 分支下获取到 `number[]` 类型。

**对 `unknown` 类型使用类型断言**

```ts
const value: unknown = "Hello World";
const someString: string = value as string;
const otherString = someString.toUpperCase();  // "HELLO WORLD"
```

请注意，TypeScript 事实上未执行任何特殊检查以确保类型断言实际上有效。类型检查器假定你更了解并相信你在类型断言中使用的任何类型都是正确的。请谨慎使用类型断言！

**联合类型中的 unkonwn**

在联合类型中，类型会表示为范围更大的类型

```ts
type UnionType1 = unknown | null; // unknown
type UnionType2 = unknown | undefined; // unknown
type UnionType3 = unknown | string; // unknown
type UnionType4 = unknown | number[]; // unknown
type UnionType5 = unknown | any; // any
```

**交叉类型中的 unkonwn**

任何类型都可以吸收 `unknown` 类型。任何类型与 unknown 相交不会改变结果类型：

```ts
type IntersectionType1 = unknown & null; // null
type IntersectionType2 = unknown & undefined; // undefined
type IntersectionType3 = unknown & string; // string
type IntersectionType4 = unknown & number[]; // number[]
type IntersectionType5 = unknown & any; // any
```

**类型为 unknown 的值使用运算符**

unknown 类型的值不能用作大多数运算符的操作数。这是因为如果我们不知道我们正在使用的值的类型，大多数运算符不太可能产生有意义的结果。

你可以在类型为 unknown 的值上使用的运算符只有相等和不等运算符：

- ===
- ==
- !==
- !=

如果要对类型为 unknown 的值使用任何其他运算符，则必须先指定类型（或使用类型断言强制编译器信任你）。

#### 类型推论

 如果没有初始化（声明+赋值）时没有明确的指定类型，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。 

- **什么是类型推论**

以下代码虽然没有指定类型，但是会在编译的时候报错：

```ts
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

事实上，它等价于：

```ts
let myFavoriteNumber: string = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```

#### Object vs object vs { }

**object类型**

TypeScript 2.2 引入了被称为 object 类型的新类型，它用于表示非原始类型。在 JavaScript 中以下类型被视为原始类型：`string`、`boolean`、`number`、`bigint`、`symbol`、`null` 和 `undefined`。 

object 类型表示除上述原始类型以外的非原始类型， 使用这种类型，我们不能访问值的未定义的任何属性。 

接口类型与 object 类型相比，用接口表示对象的类型会更加具体。

`Record<string, any>` 也可以创建了一个 key 为任意 string，value 为任意类型的索引类型。



**Object 类型**

TypeScript 定义了另一个与新的 object 类型几乎同名的类型，那就是 Object 类型。该类型是所有 Object 类的实例的类型。它由以下两个接口来定义：

- Object 接口定义了其原型对象 Object.prototype 的属性；
- ObjectConstructor 接口定义了 Object 构造器（类）的属性。

1、Object 接口定义

```ts
// node_modules/typescript/lib/lib.es5.d.ts
interface Object {  
  constructor: Function;  
  toString(): string;  
  toLocaleString(): string;  
  valueOf(): Object;  
  hasOwnProperty(v: PropertyKey): boolean;  
  isPrototypeOf(v: Object): boolean;  
  propertyIsEnumerable(v: PropertyKey): boolean;
}
```

2、ObjectConstructor 接口定义

```ts
// node_modules/typescript/lib/lib.es5.d.ts
interface ObjectConstructor { 
    /** Invocation via `new` */  
    new(value?: any): Object;  
    /** Invocation via function calls */  
    (value?: any): any;  
    readonly prototype: Object;  
    getPrototypeOf(o: any): any; 
    // ···
}

declare var Object: ObjectConstructor;
```

Object 类的所有实例都继承了 Object 接口中的所有属性。

**Object vs object**

有趣的是，类型 `Object` 包括原始值：

```ts
function func1(x: Object) { }
func1('semlinker'); // OK
```

 `object` 类型不包括原始值。

需要注意的是，当对 Object 类型的变量进行赋值时，如果值对象属性名与 Object 接口中的属性或方法冲突，则 TypeScript 编译器会提示相应的错误（因为 TypeScript 已经定义好了这些属性和方法）：

```ts
// Type '() => number' is not assignable to type 
// '() => string'.
// Type 'number' is not assignable to type 'string'.
const obj1: Object = {    
  toString() { 
    return 123
  } // Error
}; 
```

而对于 object 类型来说，不会报错：

```ts
const obj2: object = {   
  toString() { 
    return 123
  }
};
```

另外在处理 object 类型和字符串索引对象类型的赋值操作时，也要特别注意。比如：

```ts
let strictTypeHeaders: { [key: string]: string } = {};
let header: object = {};
header = strictTypeHeaders; // OK
// Type 'object' is not assignable to type '{ [key: string]: string; }'.
strictTypeHeaders = header; // Error
```

在上述例子中，最后一行会出现编译错误，这是因为 `{ [key: string]: string }` 类型相比 `object` 类型更加精确。（父类类型的变量不能赋值给子类类型的变量）

**空类型{ }**

还有另一种类型与之非常相似，即空类型：`{}`。它描述了一个没有成员的对象。当你试图访问这样一个对象的任意属性时，TypeScript 会产生一个编译时错误：

```ts
// Type {}
const obj = {};
// Error: Property 'prop' does not exist on type '{}'.
obj.prop = "semlinker";
```

但是，你仍然可以使用在 Object 原型对象上定义的所有属性和方法，这些属性和方法可通过 JavaScript 的原型链隐式地使用：

```ts
// Type {}
const obj = {};
// "[object Object]"
obj.toString();
```

#### 联合类型

联合类型（Union Types）表示取值可以为多种类型中的一种。 

联合类型使用 `|` 分隔每个类型。

这里的 `let myFavoriteNumber: string | number` 的含义是，允许 `myFavoriteNumber` 的类型是 `string` 或者 `number`，但是不能是其他类型。

- **访问联合类型的属性或方法**
  
  当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型里共有的属性或方法**： 

```ts
function getLength(something: string | number): number {
    return something.length;
}

// index.ts(2,22): error TS2339: Property 'length' does not exist on type 'string | number'.
//   Property 'length' does not exist on type 'number'.
```

上例中，`length` 不是 `string` 和 `number` 的共有属性，所以会报错。

访问 `string` 和 `number` 的共有属性是没问题的：

```ts
function getString(something: string | number): string {
    return something.toString();
}
```

 联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型 。

**交叉类型优先级比联合类型的高**

```ts
type A = { a: 1 } & { b: 2 } | { c: 3 } // ({ a: 1 } & { b: 2 }) | { c: 3 }
type B = { a: 1 } | { b: 2 } & { c: 3 } // { a: 1 } | ({ b: 2 } & { c: 3 })
```



#### 交叉类型

TypeScript 交叉类型是将多个类型合并为一个类型。这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。

**交叉类型一般用于交叉接口类型**

```ts
interface IPerson {
    id: string;
    age: number;
}
interface IWorker {
    companyId: string;
}
type IStaff = IPerson & IWorker;
const staff: IStaff = {
    id: 'E1006',
    age: 33,
    companyId: 'EXE'
};
```

在上面示例中，我们首先为 IPerson 和 IWorker 类型定义了不同的成员，然后通过 & 运算符定义了 IStaff 交叉类型，所以该类型同时拥有 IPerson 和 IWorker 这两种类型的成员。那么现在问题来了，假设在合并多个类型的过程中，刚好出现某些类型存在相同的成员，但对应的类型又不一致，比如：

```ts
interface X {
    c: string;
    d: string;
}
interface Y {
    c: number;
    e: string
}
type XY = X & Y;
type YX = Y & X;
let p: XY;
let q: YX;
```

在上面的代码中，接口 X 和接口 Y 都含有一个相同的成员 c，但它们的类型不一致。对于这种情况，此时 XY 类型或 YX 类型中成员 c 的类型是不是可以是 string 或 number 类型呢？答案：不是。

接口 X 和接口 Y 混入后，成员 c 的类型会变成 `never`。

因为混入后成员 c 的类型为 `string`&`number`，即成员 c 的类型既是 `string`类型又是`number` 类型。很明显这种类型是不存在的，所以混入后成员 c 的类型为 `never`。

**在混入多个类型时，若存在相同的成员，成员类型都为基本数据类型且不一致时，交叉类型后，该成员类型为 never**

**任何类型与any交叉后，结果为any**

接口 X 和接口 Y 中内部相同成员 c 的类型都是非基本数据类型：

```ts
interface D { 
    d: boolean;
}interface E { 
    e: string;
}
interface F { 
    f: number; 
}
interface A { 
      // D是非数据类型
    x: D; 
}
interface B { 
    // E是非数据类型
        x: E;
}
interface C {
    // F是非数据类型
    x: F;
}
type ABC = A & B & C;
let abc: ABC = {
    x: {
        d: true,
        e: 'semlinker',
        f: 666
    }
};
console.log('abc:', abc);
```

```ts
interface A {
  a: { s: 1 }
}

interface B {
  a: { k: 2 }
}

type AB = A & B

let ab: AB = {
  a: { s: 1, k: 2 },
}
```

**在混入多个类型时，若存在相同的成员，且成员类型都为引用类型，那么是可以成功合并。**

**在混入父级接口和子级接口时，交叉结果为子级接口。**

**基本数据类型不能和引用类型合并。**

通过交叉类型，可以让我们更好地将抽取类型的公共部分，进行代码复用，并方便地实现把多种类型组合到一起，成为不同的类型。

#### 对象的类型—接口

 在 TypeScript 中，使用接口（Interfaces）来定义对象的类型。 

在面向对象语言中，接口（Interfaces）是一个很重要的概念，它是对行为（方法）的抽象集合，而具体如何行动需要由类（classes）去实现（implement），详细见 **类与接口** 章节。

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

- **简单的例子**

```ts
interface Person {
    name: string;
    age: number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
```

上面的例子中，我们定义了一个接口 `Person`，接着定义了一个变量 `tom`，它的类型是 `Person`。这样，我们就约束了 `tom` 的属性及其值的类型必须和接口 `Person` 一致。

 **赋值的时候，变量的形状必须和接口的形状保持一致**。 （属性名、属性数量、属性值类型一致）

接口的属性可以限定值：

```ts
interface Person {
    name: 'Tom'|'han';
    age: number;
}

let tom: Person = {
    name: 'Tom', // 只能是 'Tom' 或 'han'
    age: 25
};
```

- **可选属性和可选方法**
  
  可选属性的含义是该属性可以不存在。 

有时我们希望不要完全匹配一个形状，那么可以用可选属性：

```ts
interface Person {
  name: string;
  age?: number;
  swim?():void;
}

let tom: Person = {
  name: 'Tom'
};
```

- **任意属性和任意方法**

有时候我们希望一个接口允许有任意的属性和方法，可以使用如下方式：

```ts
interface Person {
  name: string;
  age?: number;
  [propName: string]: any;
}

let tom: Person = {
  name: 'Tom',
  gender: 'male',
  favorite:'basketball',
  swim():boolean{
    return true
  }
};
```

任意属性名的类型不能是联合类型，任意属性的值的类型可以是。

一旦定义了任意属性，那么**确定属性、可选属性、确定方法、可选方法的返回值**的类型都必须是任意属性的值的类型的子集：

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};

// index.ts(3,5): error TS2411: Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): error TS2322: Type '{ [x: string]: string | number; name: string; age: number; gender: string; }' is not assignable to type 'Person'.
//   Index signatures are incompatible.
//     Type 'string | number' is not assignable to type 'string'.
//       Type 'number' is not assignable to type 'string'.
```

上例中，任意属性的值允许是 `string`，但是可选属性 `age` 的值却是 `number`，`number` 不是 `string` 的子属性，所以报错了。 

一个接口中只能定义一个任意属性。如果接口中有多个类型的属性，则可以在任意属性中使用联合类型：

```ts
interface Person {
    name: string;
    age?: number;
    [propName: string]: string | number;
}

let tom: Person = {
    name: 'Tom',
    age: 25,
    gender: 'male'
};
```

- **只读属性**
  
  有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用 `readonly` 定义只读属性：

```ts
interface Person {
    readonly id: number;
}

let tom: Person = {
    id: 89757
};

tom.id = 9527; // 报错
```

**注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候**：

```ts
interface Person {
    readonly id: number;
}

let tom: Person = {

};

tom.id = 89757; 

// 报错信息有两处，第一处是在对 tom 进行赋值的时候，没有给 id 赋值。
// 第二处是在给 tom.id 赋值的时候，由于它是只读属性，所以报错了。
```

**注意，不能对接口进行递归使用**

例如：

```ts
interface A {
  a: string
  b: A // 禁止递归使用
}
const a :A = {
  a:'1',
  b:{
    a:'1',
    b:{
      a:'1',
      b:// ... 需要无限声明
    }
  }
}
```

但是可以这样用：

```ts
interface A {
  a: string
  b: A[] // 允许
}
const a: A = {
  a: '1',
  b: [],
}
```

**在对象内给属性值声明类型**

```ts
const a = {
  b: <number | string>1,
}
```

#### 对象的类型—类型别名

我们除了可以通过 Object 和 object 类型来描述对象之外，  也可以通过  `type`和 `interface`来定义对象的类型：

```ts
// 类型别名
type ObjType1 = {
  a: boolean;
  b: number;
  c: string;
};

// Interface
interface ObjType2 {
  a: boolean;
  b: number;
  c: string;
}
```

**字面量和接口定义对象类型的区别**

**内联**

对象字面量类型可以内联，而接口不能：

```ts
// 对象字面量类型可以内联
function f1(x: { prop: number }) {}

function f2(x: ObjectInterface) {} 
interface ObjectInterface {  
    prop: number;
}
```

#### type与interface的区别

##### 相似之处

1. `interface `和` type` 都可以被继承

2. 类可以实现（implements）`interface` 以及 `type`(除联合类型外)



##### 区别

1. type可以声明 基本类型，联合类型，交叉类型，元组，interface不行
2. type 语句中可以使用 typeof 获取类型实例
3. type 支持类型映射，interface不支持
4. `interface` 能够声明合并，`type`不能
5. `interface` 能用于多态this类型
6. **由于interface可以进行声明合并**，所以总有可能将新成员添加到同一个interface定义的类型上。**在使用interface去声明变量时，它们在那一刻类型并不是最终的类型**。会导致索引签名问题。



- **索引签名问题**

```typescript
interface propType{
    [key: string] : string
}

let props: propType

type dataType = {
    title: string
}

interface dataType1 {
    title: string
}
const data: dataType = {title: "订单页面"}
const data1: dataType1 = {title: "订单页面"}
props = data
// 不能将类型“dataType1”分配给类型“propType”。类型“dataType1”中缺少类型为“string”的索引签名，因为interfae有可能会进行声明合并，导致结构不确定
props = data1
```



- **interface 继承 interface**

```typescript
interface Person{
    name:string
}

interface Student extends Person { stuNo: number }
```

- **interface 继承 type**

```typescript
type Person{
    name:string
}

interface Student extends Person { stuNo: number }
```

- **type 继承 type**

```typescript
type Person{
    name:string
}

type Student = Person & { stuNo: number }
```

- **type 继承 interface**

```typescript
interface Person{
    name:string
}

type Student = Person & { stuNo: number }
```

- **声明合并**

```typescript
interface Person { name: string }
interface Person { age: number }

let user: Person = {
    name: "Tolu",
    age: 0,
};
```

这种情况下，如果是`type`的话，重复使用`Person`是会报错的：

```typescript
type Person { name: string }; 

// Error: 标识符“Person”重复。ts(2300)
type Person { age: number }
```



- 多态 this 类型仅适用于接口：

```ts
interface AddsStrings {
    add(str: string): this;
};
class StringBuilder implements AddsStrings {
  result = '';
  add(str: string){
    this.result += str;
    return this;
  }
}
```

- type 支持类型映射，interface不支持：

```ts
interface Point {  
  x: number;
  y: number;
}
type PointCopy1 = {  
  [Key in keyof Point]: Point[Key];
};

// Syntax error:
interface PointCopy2 {
  [Key in keyof Point]: Point[Key];
};
```

#### import/export  type

仅仅导入/导出声明，不会引入/导出运行时内容。

#### 类型解构

在使用类型时，我们可以进行解构

```ts
interface A {
  a: 1
  b: 2
}
type B = {
  a: 1
  b: 2
}
function a1 ({ a, ...rest }: A) {}
function a2 ({ a }: A) {}
function b ({ a }: B) {}
```

#### 数组的类型

 在 TypeScript 中，数组类型有多种定义方式 。

- **「类型 + 方括号」表示法**

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];
let fibonacci: (number|string)[] = [1, '1'];
let fibonacci: any[] = [1, '1', true];
```

数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];
fibonacci.push('8');

// Argument of type '"8"' is not assignable to parameter of type 'number'.
```

- **数组泛型**

我们也可以使用数组泛型（Array Generic） `Array<elemType>` 来表示数组：

```ts
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

- **用接口表示数组**

接口也可以用来描述数组：

```ts
interface NumberArray {
    [index: number]: number|string;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

`NumberArray` 表示：只要索引的类型是数字时，那么值的类型必须是数字。

虽然接口也可以用来描述数组，但是我们一般不会这么做，因为这种方式比前两种方式复杂多了，而且没有`length`属性以及数组的方法（未在接口定义）。

不过有一种情况例外，那就是它常用来表示类数组。

- **类数组**

类数组（Array-like Object）不是数组类型，比如 `arguments`：

```ts
function sum() {
    let args: number[] = arguments;
}

// Type 'IArguments' is missing the following properties from type 'number[]': pop, push, concat, join, and 24 more.
```

上例中，`arguments` 实际上是一个类数组，不能用普通的数组的方式来描述，而应该用接口：

```ts
function sum() {
    let args: {
        [index: number]: number;
        length: number;
        callee: Function; // callee指向当前arguments指向的函数,只能在非严格模式下使用
    } = arguments;
}
```

事实上常用的类数组都有自己的接口定义，如 `IArguments`, `NodeList`, `HTMLCollection` 等：

```ts
function sum() {
    let args: IArguments = arguments;
}
```

其中 `IArguments` 是 TypeScript 中定义好了的类型，它实际上就是：

```ts
interface IArguments {
    [index: number]: any;
    length: number;
    callee: Function;
}
```

#### 函数的类型

- **函数声明**

在 JavaScript 中，有两种常见的定义函数的方式——函数声明（Function Declaration）和函数表达式（Function Expression）：

```js
// 函数声明（Function Declaration）
function sum(x, y) {
    return x + y;
}

// 函数表达式（Function Expression）
let mySum = function (x, y) {
    return x + y;
};
```

一个函数有输入和输出，要在 TypeScript 中对其进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义较简单：

```ts
function sum(x: number, y: number): number {
    return x + y;
}
```

 **输入多余的（或者少于要求的）参数，是不允许的** 。

如果没指定输出类型，则通过类型推论的规则推断出一个类型：

```ts
function sum(x: number, y: number) {
    return x + y;
}
// 如果没指定输出类型，则通过类型推论的规则推断出一个类型。
```

- **函数表达式**

如果要我们现在写一个对函数表达式（Function Expression）的定义，可能会写成这样：

```ts
let mySum = function (x: number, y: number): number {
    return x + y;
};
```

这是可以通过编译的，不过事实上，上面的代码只对等号右侧的匿名函数进行了类型定义，而等号左边的 `mySum`，是通过赋值操作进行类型推论而推断出来的。如果需要我们手动给 `mySum` 添加类型，则应该是这样：

```ts
let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
}; 
// = 左边是指定函数类型，= 右边是将函数赋值给变量
```

在指定类型时，`=>` 可以用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

在TypeScript中，依然可以使用箭头函数

```ts
let fun1:(data:number) => number = (data:number):number=> data * data;
let fun2:(data:number) => number = (data:number)=> data * data; // 输出类型通过类型推论推断
```

- **用接口定义函数的形状**

我们也可以使用接口的方式来定义一个函数需要符合的形状：

```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
// = 右边的函数可以省略对输出类型的指定
// 也可以简写成
mySearch = function(source, subString) {
    return source.search(subString) !== -1;
}
```

采用 函数表达式  的方式时，

对等号左侧进行类型限制之后，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变，因此可以 = 右边的函数可以省略对输出类型的指定，不过前提是要确认函数的返回值类型与指定的类型一样。

- **可选参数**

前面提到，输入多余的（或者少于要求的）参数，是不允许的。那么如何定义可选的参数呢？

与接口中的可选属性类似，我们用 `?` 表示可选的参数：

```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName) {
        return firstName + ' ' + lastName;
    } else {
        return firstName;
    }
}
let tomcat = buildName('Tom', 'Cat');
let tom = buildName('Tom');
```

 **需要注意的是，可选参数必须接在必需参数后面。** 

在 ES6 中，我们允许给函数的参数添加默认值，**TypeScript 会将添加了默认值的参数识别为可选参数**（不用再加 `?`），但此时不受「可选参数必须接在必需参数后面」的限制。

```ts
function buildName(firstName: string = 'Tom', lastName: string) {
    return firstName + ' ' + lastName;
}
let tomcat = buildName('Tom', 'Cat');
let cat = buildName(undefined, 'Cat');
```

- **剩余参数**

ES6 中，可以使用 `...rest` 的方式获取函数中的剩余参数（rest 参数）：

```js
function push(array, ...items) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a: any[] = [];
push(a, 1, 2, 3);
```

事实上，`items` 是一个数组。所以我们可以用数组的类型来定义它：

```ts
function push(array: any[], ...items: any[]) {
    items.forEach(function(item) {
        array.push(item);
    });
}

let a = [];
push(a, 1, 2, 3);
```

注意，rest 参数只能是最后一个参数剩余参数

ES6 中，可以使用 `...rest` 的方式将不定量参数存进一个数组传入：

```js
function push(array, ...items) {
   console.log(items)
}
```

事实上，`items` 是一个数组。所以我们可以用数组的类型来定义它：

```ts
function push(array: any[], ...items: any[]) {

}
```

注意，rest 参数只能是最后一个参数。

- **函数不限定参数传参**

```ts
// 函数声明
function sum(x: number, y: number): number {
  return x + y;
}

// 函数表达式
let mySum: (...args:any[]) => number = function (x: number, y: number): number {
  return x + y;
}; 

// 接口定义函数形状 
interface SearchFunc {
  (...args:any[]): any;
}
```

- **重载**

重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。重载实际上是函数签名的重载。

比如，我们需要实现一个函数 `reverse`，输入数字 `123` 的时候，输出反转的数字 `321`，输入字符串 `'hello'` 的时候，输出反转的字符串 `'olleh'`。

利用联合类型，我们可以这么实现：

```ts
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

**然而这样有一个缺点，就是不能够精确的表达，输入为数字的时候，输出也应该为数字，输入为字符串的时候，输出也应该为字符串。**

这时，我们可以使用重载定义多个 `reverse` 的函数类型：

```ts
function reverse(x: number): number;
function reverse(x: string): string;
// 实现签名 对外不可见，而且以下的函数类型并非签名，上面的两个才是
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

上例中，我们重复定义了多次函数 `reverse`，前几次都是函数声明，最后一次是函数定义。在编辑器的代码提示中，可以正确的看到前两个函数给我们的提示。

注意，TypeScript 会优先从最前面的函数声明开始匹配，所以多个函数声明如果有包含关系，需要优先把精确的定义写在前面。

**利用type重载**

fun 代表的是一个具有重载的函数类型，那么实现显然也必须是重载的函数

```ts
type fun = {
  (normal: number): number
  (normal: number, abnormal: string): string
}
function f1(normal: number): number
function f1(normal: number, abnormal: string): string
function f1(normal: number, abnormal?: string): number | string {
  return abnormal || normal
}
const f: fun = f1
```

#### 类型断言

类型断言（Type Assertion）可以用来告诉 TypeScript 编译器值的类型， 不会真的影响到变量的类型（并非真正指定类型） 。

- **语法**

```ts
值 as 类型
```

或

```ts
<类型>值
```

在 tsx 语法（React 的 jsx 语法的 ts 版）中必须使用前者，即 `值 as 类型`。

形如 `<foo>` 的语法在 tsx 中表示的是一个 `ReactNode`，在 ts 中除了表示类型断言之外，也可能是表示一个泛型。

故建议大家在使用类型断言时，统一使用 `值 as 类型` 这样的语法，

- **用法**

**1. 将一个联合类型断言为其中一个类型**

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，我们**只能访问此联合类型的所有类型中共有的属性或方法**：

```ts
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function getName(animal: Cat | Fish) {
    return animal.name;
}
```

 有时候，我们确实需要在还不确定类型的时候就访问其中一个类型特有的属性或方法 ，例如要访问`Fish`类型的`swim`方法 此时可以使用类型断言，将 `animal` 断言成 `Fish`： 

```ts
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

 需要注意的是，类型断言只能够「欺骗」TypeScript 编译器，当断言的类型与实际类型不相符时会导致运行时的错误，反而滥用类型断言可能会导致运行时错误： 

```ts
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function swim(animal: Cat | Fish) {
    (animal as Fish).swim();
}

const tom: Cat = {
    name: 'Tom',
    run() { console.log('run') }
};
swim(tom);
// Uncaught TypeError: animal.swim is not a function`
```

上面的例子编译时不会报错，但在运行时会报错：

```autoit
Uncaught TypeError: animal.swim is not a function`
```

原因是 `(animal as Fish).swim()` 这段代码隐藏了 `animal` 可能为 `Cat` 的情况，将 `animal` 直接断言为 `Fish` 了，而 TypeScript 编译器信任了我们的断言，故在调用 `swim()` 时没有编译错误。

可是 `swim` 函数接受的参数是 `Cat | Fish`，一旦传入的参数是 `Cat` 类型的变量，由于 `Cat` 上没有 `swim` 方法，就会导致运行时错误了。

**2. 将一个父类断言为更加具体的子类**

当类之间有继承关系时，类型断言也是很常见的：

```ts
class ApiError extends Error {
    code: number = 0;
}
class HttpError extends Error {
    statusCode: number = 200;
}

function isApiError(error: Error) {
    if (typeof (error as ApiError).code === 'number') {
        return true;
    }
    return false;
}
```

上面的例子中，我们声明了函数 `isApiError`，它用来判断传入的参数是不是 `ApiError` 类型，为了实现这样一个函数，它的参数的类型肯定得是比较抽象的父类 `Error`，这样的话这个函数就能接受 `Error` 或它的子类作为参数了。

但是由于父类 `Error` 中没有 `code` 属性，故直接获取 `error.code` 会报错，需要使用类型断言获取 `(error as ApiError).code`。

大家可能会注意到，在这个例子中有一个更合适的方式来判断是不是 `ApiError`，那就是使用 `instanceof`：

```ts
class ApiError extends Error {
    code: number = 0;
}
class HttpError extends Error {
    statusCode: number = 200;
}

function isApiError(error: Error) {
    if (error instanceof ApiError) {
        return true;
    }
    return false;
}
```

上面的例子中，确实使用 `instanceof` 更加合适，因为 `ApiError` 是一个 JavaScript 的类，能够通过 `instanceof` 来判断 `error` 是否是它的实例。

但是有的情况下 `ApiError` 和 `HttpError` 不是一个真正的类，而只是一个 TypeScript 的接口（`interface`），接口是一个类型，不是一个真正的值，它在编译结果中会被删除，当然就无法使用 `instanceof` 来做运行时判断了：

```ts
interface ApiError extends Error {
    code: number;
}
interface HttpError extends Error {
    statusCode: number;
}

function isApiError(error: Error) {
    if (error instanceof ApiError) {
        return true;
    }
    return false;
}

// index.ts:9:26 - error TS2693: 'ApiError' only refers to a type, but is being used as a value here.
```

 此时就只能用类型断言，通过判断是否存在 `code` 属性，来判断传入的参数是不是 `ApiError` 了。

**3. 将任何一个类型断言为 any ** 

有的时候，我们非常确定这段代码不会出错，比如下面这个例子：

```ts
window.foo = 1;

// index.ts:1:8 - error TS2339: Property 'foo' does not exist on type 'Window & typeof globalThis'.
```

上面的例子中，我们需要将 `window` 上添加一个属性 `foo`，但 TypeScript 编译时会报错，提示我们 `window` 上不存在 `foo` 属性。

此时我们可以使用 `as any` 临时将 `window` 断言为 `any` 类型：

```ts
(window as any).foo = 1;
```

在`any`类型的变量上，访问或添加任何属性和方法都是允许的。

需要注意的是，将一个变量断言为 `any` 可以说是解决 TypeScript 中类型问题的最后一个手段。

**它极有可能掩盖了真正的类型错误，所以如果不是非常确定，就不要使用 `as any`。**

 上面的例子中，我们也可以通过 扩展 window 的类型 解决这个错误，不过如果只是临时的增加 `foo` 属性，`as any` 会更加方便。 

**4. 将 any 断言为一个具体的类型**

在日常的开发中，我们不可避免的需要处理 `any` 类型的变量，它们可能是由于第三方库未能定义好自己的类型，也有可能是历史遗留的或其他人编写的烂代码，还可能是受到 TypeScript 类型系统的限制而无法精确定义类型的场景。

遇到 `any` 类型的变量时，我们可以选择无视它，任由它滋生更多的 `any`。

我们也可以选择改进它，通过类型断言及时的把 `any` 断言为精确的类型，亡羊补牢，使我们的代码向着高可维护性的目标发展。

**类型断言的限制**

从上面的例子中，我们可以总结出：

- 联合类型可以被断言为其中一个类型
- 父类可以被断言为子类 （TypeScript 设计的）
- 子类可以被断言为父类
- 任何类型都可以被断言为 any （TypeScript 设计的，为了可以像JS一样任意增加属性和方法）
- any 可以被断言为任何类型（TypeScript 设计的，提高精确性和可维护性）

具体来说，若 `A` 兼容 `B`，`A` 和 `B`可以互相进行类型断言。

注意的是，any 并不是兼容任何类型，但 any 和 任何类型可以互相进行类型断言

下面我们通过一个简化的例子，来理解类型断言的限制：

```ts
interface Animal {
    name: string;
}
interface Cat {
    name: string;
    run(): void;
}

let tom: Cat = {
    name: 'Tom',
    run: () => { console.log('run') }
};
let animal: Animal = tom; // 可以将子类的实例赋值给类型为父类的变量
```

TypeScript 是结构类型系统，只会看类型之间最终的结构，而不关心它们定义时的关系。

在上面的例子中，`Cat` 包含了 `Animal` 中的所有属性，除此之外，它还有一个额外的方法 `run`。TypeScript 并不关心 `Cat` 和 `Animal` 之间定义时是什么关系，而只会看它们最终的结构有什么关系——所以它与 `Cat extends Animal` 是等价的：

```ts
interface Animal {
    name: string;
}
interface Cat extends Animal {
    run(): void;
}
```

那么也不难理解为什么 `Cat` 类型的 `tom` 可以赋值给 `Animal` 类型的 `animal` 了——就像面向对象编程中我们可以将子类的实例赋值给类型为父类的变量。（男人是人，但人不一定是男人）

我们把它换成 TypeScript 中更专业的说法，即：`Animal` 兼容 `Cat`。

 当 `Animal` 兼容 `Cat` 时，它们就可以互相进行类型断言了 。

这样的设计其实也很容易就能理解：

- 允许 `animal as Cat` 是因为「父类可以被断言为子类」，TypeScript 设计就是这样的。
- 允许 `cat as Animal` 是因为既然子类拥有父类的属性和方法，那么被断言为父类，获取父类的属性、调用父类的方法，就不会有任何问题，故「子类可以被断言为父类」。

##### 双重断言

既然：

- 任何类型都可以被断言为 any 或 unknown
- any 或 unknown可以被断言为任何类型

那么我们是不是可以使用双重断言 `as any as Foo` 来将任何一个类型断言为任何另一个类型呢？

```ts
interface Cat {
    run(): void;
}
interface Fish {
    swim(): void;
}

function testCat(cat: Cat) {
    return (cat as any as Fish); // 双重断言
}
```

在上面的例子中，若直接使用 `cat as Fish` 肯定会报错，因为 `Cat` 和 `Fish` 互相都不兼容。

但是若使用双重断言，将任何一个类型断言为 `any` 或 `unknown` 类型，在断言为任何另一个类型。

若你使用了这种双重断言，那么十有八九是非常错误的，它很可能会导致运行时错误。

**除非迫不得已，千万别用双重断言。**

`as any as`  VS  `as unknown as`

相同点：两者都是不安全的

区别：

- Linters prefer `unknown` (with `no-explicit-any` rule)
- `any` is less characters to type than `unknown`



##### as const

const 断言，它的作用是让里头的所有东西变成只读，且数组被限定成了一个元组。

```ts
function a () {
  let a,b;
 	return [a,b] as const;
}
```



**类型断言 vs 类型转换**

类型断言只会影响 TypeScript 编译时的类型，类型断言语句在编译结果中会被删除：

```ts
function toBoolean(something: any): boolean {
    return something as boolean;
}

toBoolean(1);
// 返回值为 1
```

在上面的例子中，将 `something` 断言为 `boolean` 虽然可以通过编译，但是并没有什么用，代码在编译后会变成：

```js
function toBoolean(something) {
    return something;
}

toBoolean(1);
// 返回值为 1
```

所以类型断言不是类型转换，它不会真的影响到变量的类型。

若要进行类型转换，需要直接调用类型转换的方法：

```ts
function toBoolean(something: any): boolean {
    return Boolean(something);
}

toBoolean(1);
// 返回值为 true
```

#### 高级类型

##### 多态this类型

多态this类型表示类的实例的类型，这被称做 *F*-bounded多态性。 它能很容易的表现连贯接口间的继承，例如链式调用等等：

```ts
class BaseCalculator {
  constructor(
    public value: number 
  ){}

  add(num: number): this{
    this.value += num;
    return this;
  }

  multiply(num: number): this {
    this.value *= num;
    return this;
  }
}

let a = new BaseCalculator(3);
a.add(7).multiply(5).add(5).value;// 链式
// 类可以拓展
class ScientificCalculator extends BaseCalculator {
  constructor(value: number = 0){
    super(value);
  }
  sin():this {
    this.value = Math.sin(this.value);
    return this;
  }
}
let b = new ScientificCalculator();
b.sin().add(1).multiply(4).value;
```

##### 映射类型

一个常见的任务是将一个已知的类型每个属性都变为可选的：

```ts
interface PersonPartial {
    name?: string;
    age?: number;
}
```

或者我们想要一个只读版本：

```ts
interface PersonReadonly {
    readonly name: string;
    readonly age: number;
}
```

这在JavaScript里经常出现，TypeScript提供了从旧类型中创建新类型的一种方式 — **映射类型**。 在映射类型里，新类型以相同的形式去转换旧类型里每个属性。 例如，你可以令每个属性成为 `readonly`类型或可选的。 下面是一些例子：

```ts
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}
type Partial<T> = {
    [P in keyof T]?: T[P];
}
```

```ts
type Keys = 'option1' | 'option2';
// 内部使用了 for .. in
type Flags = { [K in Keys]: boolean };
```

等同于

```ts
type Flags = {
    option1: boolean;
    option2: boolean;
}
```

#### 内置对象

JavaScript 中有很多[内置对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)，它们可以直接在 TypeScript 中当做定义好了的类型。

内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

**ECMAScript 的内置对象**

ECMAScript 标准提供的内置对象有：

`Boolean`、`Error`、`Date`、`RegExp` 等。

我们可以在 TypeScript 中将变量定义为这些类型：

```ts
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

而他们的定义文件，则在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib)中。

**DOM 和 BOM 的内置对象**

DOM 和 BOM 提供的内置对象有：

`Document`、`HTMLElement`、`Event`、`NodeList` 等。

TypeScript 中会经常用到这些类型：

```ts
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
  // Do something
});
```

它们的定义文件同样在 [TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib)中。

**TypeScript 核心库的定义文件**

[TypeScript 核心库的定义文件](https://github.com/Microsoft/TypeScript/tree/master/src/lib)中定义了所有浏览器环境需要用到的类型，并且是预置在 TypeScript 中的。

当你在使用一些常用的方法的时候，TypeScript 实际上已经帮你做了很多类型判断的工作了，比如：

```ts
Math.pow(10, '2');

// index.ts(1,14): error TS2345: Argument of type 'string' is not assignable to parameter of type 'number'.
```

上面的例子中，`Math.pow` 必须接受两个 `number` 类型的参数。事实上 `Math.pow` 的类型定义如下：

```ts
interface Math {
    /**
     * Returns the value of a base expression taken to a specified power.
     * @param x The base value of the expression.
     * @param y The exponent value of the expression.
     */
    pow(x: number, y: number): number;
}
```

再举一个 DOM 中的例子：

```ts
document.addEventListener('click', function(e) {
    console.log(e.targetCurrent);
});

// index.ts(2,17): error TS2339: Property 'targetCurrent' does not exist on type 'MouseEvent'.
```

上面的例子中，`addEventListener` 方法是在 TypeScript 核心库中定义的：

```ts
interface Document extends Node, GlobalEventHandlers, NodeSelector, DocumentEvent {
    addEventListener(type: string, listener: (ev: MouseEvent) => any, useCapture?: boolean): void;
}
```

所以 `e` 是 `MouseEvent`类型，而 `MouseEvent` 是没有 `targetCurrent` 属性的，所以报错了。

注意，TypeScript 核心库的定义中不包含 Node.js 部分。

**用 TypeScript 写 Node.js**

Node.js 不是内置对象的一部分，如果想用 TypeScript 写 Node.js，则需要引入第三方声明文件：

```bash
npm install @types/node --save-dev
```

#### 声明文件、模块、第三方库

在开发过程中不可避免要引用其他第三方的 JavaScript 的库。虽然通过直接引用可以调用库的类和方法，但是却无法使用TypeScript 诸如类型检查等特性功能。为了解决这个问题，需要将这些库里的属性值、方法体去掉后只保留导出类型声明，而产生了一个描述 JavaScript 库和模块信息的声明文件。通过引用这个声明文件，就可以借用 TypeScript 的各种特性来使用库文件了。 

**新语法索引**

由于本章涉及大量新语法，故在本章开头列出新语法的索引，方便大家在使用这些新语法时能快速查找到对应的讲解：

- `declare var`声明全局变量
- `declare function`声明全局方法
- `declare class`声明全局类
- `declare enum`声明全局枚举类型
- `declare namespace`声明（含有子属性的）全局对象
- `interface` 和 `type`声明全局类型
- `export`导出变量
- `export namespace`导出（含有子属性的）对象
- `export default`ES6 默认导出
- `export =`commonjs 导出模块
- `export as namespace`UMD 库声明全局变量
- `declare global` 扩展全局变量
- `declare module`扩展模块
- `/// `三斜线指令

##### 什么是声明语句

假如我们想使用第三方库 jQuery，一种常见的方式是在 html 中通过 `<script>` 标签引入 jQuery，然后就可以使用全局变量 `$` 或 `jQuery` 了。

我们通常这样获取一个 `id` 是 `foo` 的元素：

```js
$('#foo');
// or
jQuery('#foo');
```

但是在 ts 中，编译器并不知道 `$` 或 `jQuery` 是什么东西：

```ts
jQuery('#foo');
// ERROR: Cannot find name 'jQuery'.
```

这时，我们需要使用 `declare var` 来定义它的类型：

```ts
declare var jQuery: (selector: string) => any;

jQuery('#foo');
```

上例中，`declare var` 并没有真的声明一个变量，只是声明了全局变量 `jQuery` 的类型，仅仅会用于编译时的检查，在编译结果中会被删除。它编译结果是：

```js
jQuery('#foo');
```

而`var`是真正声明一个变量

```ts
var jQuery: (selector: string) => any;

jQuery('#foo');
```

`jQuery`在编译结果中不会被删除，编译结果是：

```ts
var jQuery;
jQuery('#foo');
```

##### 什么是声明文件

通常我们会把声明语句放到一个单独的文件（`jQuery.d.ts`）中，这就是声明文件：

```ts
// src/jQuery.d.ts

declare var jQuery: (selector: string) => any;
// src/index.ts

jQuery('#foo');
```

声明文件必需以 `.d.ts` 为后缀。

一般来说，ts 会解析项目中所有的 `*.ts` 文件，当然也包含以 `.d.ts` 结尾的文件。所以当我们将 `jQuery.d.ts` 放到项目中时，其他所有 `*.ts` 文件就都可以获得 `jQuery` 的类型定义了。

```autoit
/path/to/project
├── src
|  ├── index.ts
|  └── jQuery.d.ts
└── tsconfig.json
```

假如仍然无法解析，那么可以检查下 `tsconfig.json` 中的 `files`、`include` 和 `exclude` 配置，确保其包含了 `jQuery.d.ts` 文件。

在全局变量的声明文件中，是不允许出现 `import`, `export` 关键字的。一旦出现了，那么他就会被视为一个 npm 包或 UMD 库，就不再是全局变量的声明文件了。全局变量不允许通过 `import`, `export`导入导出。

这里只演示了全局变量这种模式的声明文件，假如是通过模块导入的方式使用第三方库的话，那么引入声明文件又是另一种方式了（通过模块导入的方式将变量引到其它文件，不再是全局变量），将会在后面详细介绍。

##### .d.ts和ts文件

在**.d.ts**文件声明的变量相当于在全局中声明。

在**.d.ts**文件声明的变量都能在**.ts**中使用。

##### 第三方声明文件

当然，jQuery 的声明文件不需要我们定义了，社区已经帮我们定义好了。

我们可以直接下载下来使用，但是更推荐的是使用 `@types` 统一管理第三方库的声明文件。

`@types` 的使用方式很简单，直接用 npm 安装对应的声明模块即可，以 jQuery 举例：

```bash
npm install @types/jquery --save-dev
```

可以在[这个页面](https://microsoft.github.io/TypeSearch/)搜索你需要的声明文件。

##### 书写声明文件

当一个第三方库没有提供声明文件时，我们就需要自己书写声明文件了。前面只介绍了最简单的声明文件内容，而真正书写一个声明文件并不是一件简单的事，以下会详细介绍如何书写声明文件。

在不同的场景下，声明文件的内容和使用方式会有所区别。

库的使用场景主要有以下几种：

- [全局变量]()：通过 `<script>` 标签引入第三方库，注入全局变量
- [npm 包]()：通过 `import foo from 'foo'` 导入，符合 ES6 模块规范
- [UMD 库]()：既可以通过 `<script>` 标签引入，又可以通过 `import` 导入
- [直接扩展全局变量]()：通过 `<script>` 标签引入后，改变一个全局变量的结构
- [在 npm 包或 UMD 库中扩展全局变量]()：引用 npm 包或 UMD 库后，改变一个全局变量的结构
- [模块插件]()：通过 `<script>` 或 `import` 导入后，改变另一个模块的结构

##### 全局变量

全局变量是最简单的一种场景，之前举的例子就是通过 `<script>` 标签引入 jQuery，注入全局变量 `$` 和 `jQuery`。

使用全局变量的声明文件时，如果是以 `npm install @types/xxx --save-dev` 安装的，则不需要任何配置。如果是将声明文件直接存放于当前项目中，则建议和其他源码一起放到 `src` 目录下（或者对应的源码目录下）：

```autoit
/path/to/project
├── src
|  ├── index.ts
|  └── jQuery.d.ts
└── tsconfig.json
```

如果没有生效，可以检查下 `tsconfig.json` 中的 `files`、`include` 和 `exclude` 配置，确保其包含了 `jQuery.d.ts` 文件。

全局变量的声明文件主要有以下几种语法：

- [`declare var`]() 声明全局变量
- [`declare function`]() 声明全局方法
- [`declare class`]() 声明全局类
- [`declare enum`]() 声明全局枚举类型
- [`declare namespace`]() 声明（含有子属性的）全局对象
- [`interface` 和 `type`]() 声明全局类型

##### declare var

在所有的声明语句中，`declare var` 是最简单的，如之前所学，它能够用来定义一个全局变量的类型。与其类似的，还有 `declare let` 和 `declare const`，使用 `let` 与使用 `var` 没有什么区别。

```ts
// src/jQuery.d.ts

declare let jQuery: (selector: string) => any;
```

```ts
// src/index.ts

jQuery('#foo');
// 使用 declare let 定义的 jQuery 类型，允许修改这个全局变量
jQuery = function(selector) {
    return document.querySelector(selector);
};
```

 而当我们使用 `const` 定义时，表示此时的全局变量是一个常量，不允许再去修改它的值了：

```ts
// src/jQuery.d.ts

declare const jQuery: (selector: string) => any;

jQuery('#foo');
// 使用 declare const 定义的 jQuery 类型，禁止修改这个全局变量
jQuery = function(selector) {
    return document.querySelector(selector);
};
// ERROR: Cannot assign to 'jQuery' because it is a constant or a read-only property.
```

一般来说，全局变量都是禁止修改的常量，所以大部分情况都应该使用 `const` 而不是 `var` 或 `let`。

需要注意的是，**在声明文件中，声明语句中只能指定类型**，切勿在声明语句中定义具体的实现（不能出现属性值和函数体）：

```ts
declare const jQuery = function(selector) {
    return document.querySelector(selector);
};
// ERROR: An implementation cannot be declared in ambient contexts.
```

##### declare function

`declare function` 用来定义全局函数的类型。jQuery 其实就是一个函数，所以也可以用 `function` 来定义：

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
```

```ts
// src/index.ts

jQuery('#foo');
```

在函数类型的声明语句中，函数重载也是支持的：

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
declare function jQuery(domReadyCallback: () => any): any;
```

```ts
// src/index.ts

jQuery('#foo');
jQuery(function() {
    alert('Dom Ready!');
});
```

##### declare class

当全局变量是一个类的时候，我们用 `declare class` 来定义它的类型：

```ts
// src/Animal.d.ts

// 属性只能指定类型，不能赋值
// 方法只能指定输入、输出类型，不能定义具体的实现
declare class Animal {
    name: string;
    constructor(name: string);
    sayHi(): string;
}
```

```ts
// src/index.ts

let cat = new Animal('Tom');
```

同样的，`declare class` 语句也只能用来定义类型，不能用来定义具体的实现 。

##### declare enum

使用 `declare enum` 定义的枚举类型也称作外部枚举（Ambient Enums），举例如下：

```ts
// src/Directions.d.ts

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
```

```ts
// src/index.ts

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

与其他全局变量的类型声明一致，`declare enum` 仅用来指定类型，而不是具体的值。

`Directions.d.ts` 仅仅会用于编译时的检查，声明文件里的内容在编译结果中会被删除。它编译结果是：

```js
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

 其中 `Directions` 是由第三方库定义好的全局变量。 

##### declare namespace

`namespace` 是 ts 早期时为了解决模块化而创造的关键字，中文称为命名空间。

由于历史遗留原因，在早期还没有 ES6 的时候，ts 提供了一种模块化方案，使用 `module` 关键字表示内部模块。但由于后来 ES6 也使用了 `module` 关键字，ts 为了兼容 ES6，使用 `namespace` 替代了自己的 `module`，更名为命名空间。

随着 ES6 的广泛应用，现在已经不建议再使用 ts 中的 `namespace`，而推荐使用 ES6 的模块化方案了，故我们不再需要学习 `namespace` 的使用了。

`namespace` 被淘汰了，但是在声明文件中，`declare namespace` 还是比较常用的，它用来表示全局变量是一个对象，包含很多子属性。

比如 `jQuery` 是一个全局变量，它是一个对象，提供了一个 `jQuery.ajax` 方法可以调用，那么我们就应该使用 `declare namespace jQuery` 来声明这个拥有多个子属性的全局变量。

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
// src/index.ts

jQuery.ajax('/api/get_something');
```

注意，在 `declare namespace` 内部，我们直接使用 `function ajax` 来声明函数，而不是使用 `declare function ajax`。类似的，也可以使用 `const`, `class`, `enum` 等语句：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
    const version: number;
    class Event {
        blur(eventType: EventType): void
    }
    enum EventType {
        CustomClick
    }
}
```

```ts
// src/index.ts

jQuery.ajax('/api/get_something');
console.log(jQuery.version);
const e = new jQuery.Event();
e.blur(jQuery.EventType.CustomClick);
```

**嵌套的命名空间**

为什么上面例子中的`class`、`枚举`不属于深层的层级：可以把 `class`看成构造函数，枚举是函数表达式和函数立即执行的语法糖，两者都不是Object。

如果对象拥有深层的层级，则需要用嵌套的 `namespace` 来声明深层的属性的类型：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
    namespace fn {
        function extend(object: any): void;
    }
}
```

 假如 `jQuery` 下仅有 `fn` 这一个属性（没有 `ajax` 等其他属性或方法），则可以不需要嵌套 `namespace` ：

```ts
// src/jQuery.d.ts

declare namespace jQuery.fn {
    function extend(object: any): void;
}
```

##### declare global

在 d.ts 声明文件中，任何的 declare 默认就是 global 的了，所以你在 d.ts 文件中是不能出现 `declare global` 的。只有在模块文件中的定义，如果想要全局就使用 `declare global`。



##### interface 和 type

除了全局变量之外，可能有一些类型我们也希望能暴露出来。在类型声明文件中，我们可以直接使用 `interface` 或 `type` 来声明一个全局的接口或类型（不需要在前面加 `declare`）：

```ts
// src/jQuery.d.ts

interface AjaxSettings {
    method?: 'GET' | 'POST'
    data?: any;
}
declare namespace jQuery {
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

这样的话，在其他文件中也可以使用这个接口或类型了：

```ts
// src/index.ts

let settings: AjaxSettings = {
    method: 'POST',
    data: {
        name: 'foo'
    }
};
jQuery.ajax('/api/post_something', settings);
```

`type` 与 `interface` 类似，不再赘述。

##### 防止命名冲突

暴露在最外层的 `interface` 或 `type` 会作为全局类型作用于整个项目中，我们应该尽可能的减少全局变量或全局类型的数量。故最好将他们放到 `namespace` 下：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
    interface AjaxSettings {
        method?: 'GET' | 'POST'
        data?: any;
    }
    function ajax(url: string, settings?: AjaxSettings): void;
}
```

注意，在使用这个 `interface` 的时候，也应该加上 `jQuery` 前缀：

```ts
// src/index.ts

let settings: jQuery.AjaxSettings = {
    method: 'POST',
    data: {
        name: 'foo'
    }
};
jQuery.ajax('/api/post_something', settings);
```

##### 声明合并

假如 `jQuery` 既是一个函数，可以直接被调用 `jQuery('#foo')`，又是一个对象，拥有子属性 `jQuery.ajax()`（事实确实如此），那么我们可以组合多个声明语句，它们会不冲突的合并起来：

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
declare namespace jQuery {
    function ajax(url: string, settings?: any): void;
}
```

```ts
// src/index.ts

jQuery('#foo');
jQuery.ajax('/api/get_something');
```

关于声明合并的更多用法，可以查看[声明合并](https://ts.xcatliu.com/advanced/declaration-merging.html)章节。

##### npm包

一般我们通过 `import foo from 'foo'` 导入一个 npm 包，这是符合 ES6 模块规范的。

在我们尝试给一个 npm 包创建声明文件之前，需要先看看它的声明文件是否已经存在。一般来说，npm 包的声明文件可能存在于两个地方：

1. 与该 npm 包绑定在一起。判断依据是 `package.json` 中有 `types` 字段，或者有一个 `index.d.ts` 声明文件。这种模式不需要额外安装其他包，是最为推荐的，所以以后我们自己创建 npm 包的时候，最好也将声明文件与 npm 包绑定在一起。
2. 发布到 `@types` 里。我们只需要尝试安装一下对应的 `@types` 包就知道是否存在该声明文件，安装命令是 `npm install @types/foo --save-dev`。这种模式一般是由于 npm 包的维护者没有提供声明文件，所以只能由其他人将声明文件发布到 `@types` 里了。

假如以上两种方式都没有找到对应的声明文件，那么我们就需要自己为它写声明文件了。由于是通过 `import` 语句导入的模块，所以声明文件存放的位置也有所约束，一般有两种方案：

1. 创建一个 `node_modules/@types/foo/index.d.ts` 文件，存放 `foo` 模块的声明文件。这种方式不需要额外的配置，但是 `node_modules` 目录不稳定，代码也没有被保存到仓库中，无法回溯版本，有不小心被删除的风险，故不太建议用这种方案，一般只用作临时测试。
2. 创建一个 `types` 目录，专门用来管理自己写的声明文件，将 `foo` 的声明文件放到 `types/foo/index.d.ts` 中。这种方式需要配置下 `tsconfig.json` 中的 `paths` 和 `baseUrl` 字段。

目录结构：

```autoit
/path/to/project
├── src
|  └── index.ts
├── types
|  └── foo
|     └── index.d.ts
└── tsconfig.json
```

`tsconfig.json` 内容：

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "baseUrl": "./",
        "paths": {
            "*": ["types/*"]
        }
    }
}
```

 如此配置之后，通过 `import` 导入 `foo` 的时候，也会去 `types` 目录下寻找对应的模块的声明文件了。 

 注意 `module` 配置可以有很多种选项，不同的选项会影响模块的导入导出模式。这里我们使用了 `commonjs` 这个最常用的选项 。

npm 包的声明文件主要有以下几种语法：

- [`export`]() 导出变量
- [`export namespace`]() 导出（含有子属性的）对象
- [`export default`]() ES6 默认导出
- [`export =`]() commonjs 导出模块

##### export

npm 包的声明文件与全局变量的声明文件有很大区别。在 npm 包的声明文件中，使用 `declare` 不再会声明一个全局变量，而只会在当前文件中声明一个局部变量。只有在声明文件中使用 `export` 导出，然后在使用方 `import` 导入后，才会应用到这些类型声明。

同样的，`export` 在**声明文件中，声明语句中只能指定类型**，禁止定义具体的实现（不能出现属性值和函数体）：

```ts
// types/foo/index.d.ts

export const name: string;
export function getName(): string;
export class Animal {
    constructor(name: string);
    sayHi(): string;
}
export enum Directions {
    Up,
    Down,
    Left,
    Right
}
export interface Options {
    data: any;
}
```

对应的导入和使用模块应该是这样：

```ts
// src/index.ts

import { name, getName, Animal, Directions, Options } from 'foo';

console.log(name);
let myName = getName();
let cat = new Animal('Tom');
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
let options: Options = {
    data: {
        name: 'foo'
    }
};
```

##### 混用 declare 和 export

我们也可以使用 `declare` 先声明多个变量，最后再用 `export` 一次性导出。上例的声明文件可以等价的改写为[16](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/declaration-files/16-declare-and-export)：

```ts
// types/foo/index.d.ts

declare const name: string;
declare function getName(): string;
declare class Animal {
    constructor(name: string);
    sayHi(): string;
}
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
interface Options {
    data: any;
}

export { name, getName, Animal, Directions, Options };
```

注意，与全局变量的声明文件类似，`interface` 前是不需要 `declare` 的。

##### export namespace

与 `declare namespace` 类似，`export namespace` 用来导出一个拥有子属性的对象：

```ts
// types/foo/index.d.ts

export namespace foo {
    const name: string;
    namespace bar {
        function baz(): string;
    }
}
```

```ts
// src/index.ts

import { foo } from 'foo';

console.log(foo.name);
foo.bar.baz();
```

##### export default

在 ES6 模块系统中，使用 `export default` 可以导出一个默认值，使用方可以用 `import foo from 'foo'` 而不是 `import { foo } from 'foo'` 来导入这个默认值。

在类型声明文件中，`export default` 用来导出默认值的类型：

```ts
// types/foo/index.d.ts

export default function foo(): string;
```

```ts
// src/index.ts

import foo from 'foo';

foo();
```

 注意，只有 `function`、`class` 和 `interface` 可以直接默认导出，其他的变量需要先定义出来，再默认导出 

针对这种默认导出，我们一般会将导出语句放在整个声明文件的最前面：

```ts
// types/foo/index.d.ts

export default Directions;

declare enum Directions {
    Up,
    Down,
    Left,
    Right
}
```

##### export =

`exports` 和 `module.exports` 的关系：
`exports`指向`module.exports`。在`common.js`里模块对外输出永远是`module.exports`。保险起见，可使用两种方式：

- modules.exports.foo = bar

- export.foo = bar

```js
// module.exports和exports必须指向同一个值
exports = module.exports = {};

// wrong: exports不再指向module.exports
exports = {};
```

在 commonjs 规范中，我们用以下方式来导出一个模块：

```js
// 整体导出
module.exports = foo;
// 单个导出
exports.bar = bar;
```

在 ts 中，针对commonjs模块导出，有多种方式可以导入：

第一种方式是 `const ... = require`：

```js
// 整体导入
const foo = require('foo');
// 单个导入
const bar = require('foo').bar;
```

第二种方式是 `import ... from`，注意针对整体导出，需要使用 `import * as` 来导入：

```ts
// 整体导入
import * as foo from 'foo';
// 单个导入
import { bar } from 'foo';
```

第三种方式是 `import ... require`（只能再ts中使用），这也是 ts 官方推荐的方式：

```ts
// 兼容CommonJS和AMD的导入
// 整体导入
import foo = require('foo');
// 单个导入
import bar = foo.bar;
```

 对于这种使用 commonjs 规范的库，假如要为它写类型声明文件的话，就需要使用到 `export =` 这种 ts 语法了 ：

```ts
// types/foo/index.d.ts

export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

需要注意的是，上例中使用了 `export =` 之后，就不能再单个导出 `export { bar }` 了。所以我们通过声明合并，使用 `declare namespace foo` 来将 `bar` 合并到 `foo` 里。

准确地讲，`export =` 不仅可以用在声明文件中，也可以用在普通的 ts 文件中。实际上，`import ... require` 和 `export =` 都是 **ts 为了兼容 AMD 规范和 commonjs 规范而创立的新语法**，由于并不常用也不推荐使用，所以这里就不详细介绍了，感兴趣的可以看[官方文档](https://www.typescriptlang.org/docs/handbook/modules.html#export--and-import--require)。

由于很多第三方库是 commonjs 规范的，所以声明文件也就不得不用到 `export =` 这种语法了。但是还是需要再强调下，相比于 `export =`，我们更推荐使用 ES6 标准的 `export default` 和 `export`。 

##### UMD 库

 UMD库相比于 npm 包的类型声明文件，我们需要额外声明一个全局变量，为了实现这种方式，ts 提供了一个新语法 `export as namespace`。

**对于 npm 包和 UMD 库， 只有 `export` 导出的类型声明才能被导入。** 

##### export as namespace

一般使用 `export as namespace` 时，都是先有了 npm 包的声明文件，再基于它添加一条 `export as namespace` 语句，即可将声明好的一个变量声明为全局变量，举例如下：

```ts
// types/foo/index.d.ts

export as namespace foo;
export = foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}

// 其余ts文件
import foo from 'foo';
```

当然它也可以与 `export default` 一起使用：

```ts
// types/foo/index.d.ts

export as namespace foo;
export default foo;

declare function foo(): string;
declare namespace foo {
    const bar: number;
}
```

**直接扩展全局变量**

有的第三方库扩展了一个全局变量，此时需要扩展全局变量的类型。比如扩展 `String` 类型：

```ts
// src/index.d.ts
interface String {
    prependHello(): string;
}
```

```ts
// src/index.ts
'bar'.prependHello();
```

 通过声明合并，使用 `interface String` 即可给 `String` 添加属性或方法。 

##### 在 npm 包或 UMD 库中扩展全局变量

如之前所说，对于一个 npm 包或者 UMD 库的声明文件，只有 `export` 导出的类型声明才能被导入。所以对于 npm 包或 UMD 库，如果导入此库之后会扩展全局变量，则需要使用另一种语法在声明文件中扩展全局变量的类型，那就是 `declare global`。

##### declare global

使用 `declare global` 可以在此库的 npm 包或者 UMD 库的声明文件中扩展全局变量的类型：

```ts
// types/foo/index.d.ts

declare global {
    interface String {
        prependHello(): string;
    }
}

export {};
```

```ts
// src/index.ts

'bar'.prependHello();
```

注意即使此声明文件不需要导出任何东西，仍然需要导出一个空对象，用来告诉编译器这是一个模块的声明文件，而不是一个全局变量的声明文件。

##### 模块插件

有时通过 `import` 导入一个模块插件，可以改变另一个原有模块的结构。此时如果原有模块已经有了类型声明文件，而插件模块没有类型声明文件，就会导致类型不完整，缺少插件部分的类型。ts 提供了一个语法 `declare module`，它可以用来扩展原有模块的类型。

##### declare module

如果是需要扩展原有模块或新建新模块的话，需要在类型声明文件中先引用原有模块，再使用 `declare module` 扩展原有模块：

```ts
// types/moment-plugin/index.d.ts

import * as moment from 'moment';

// 对原有模块'moment'进行扩展
declare module 'moment' {
    export function foo(): moment.CalendarKey; 
}
```

```ts
// src/index.ts

import * as moment from 'moment';
import 'moment-plugin';

moment.foo();
```

```ts
// types/foo-bar.d.ts

// 声明新模块'foo'
declare module 'foo' {
    export interface Foo {

    }
}
// 声明新模块'bar'
declare module 'bar' {
    export function bar(): string;
}
```

##### 声明文件中的依赖

一个声明文件有时会依赖另一个声明文件中的类型，比如在前面的 `declare module` 的例子中，我们就在声明文件中导入了 `moment`，并且使用了 `moment.CalendarKey` 这个类型：

```ts
// types/moment-plugin/index.d.ts

import * as moment from 'moment';

declare module 'moment' {
    export function foo(): moment.CalendarKey;
}
```

除了可以在声明文件中通过 `import` 导入另一个声明文件中的类型之外，还有一个语法也可以用来导入另一个声明文件，那就是三斜线指令。`import`与三斜线指令的区别是：`import`是引入变量（但不能在全局的声明文件最外层使用）， 三斜线指令是引入声明文件。

##### 三斜线指令

与 `namespace` 类似，三斜线指令也是 ts 在早期版本中为了描述模块之间的依赖关系而创造的语法。随着 ES6 的广泛应用，现在已经不建议再使用 ts 中的三斜线指令来声明模块之间的依赖关系了。

但是在声明文件中，它还是有一定的用武之地。

类似于声明文件中的 `import`，它可以用来导入另一个声明文件。与 `import` 的区别是，当且仅当在以下任何一个场景下，我们才需要使用三斜线指令替代 `import`：

- 当我们需要**书写**全局变量的声明文件
- 当我们需要**依赖**全局变量的声明文件

```ts
// react-scripts会从模块中寻找
/// <reference types="react-scripts" />

// ./types/index.d.ts是自定义的全局变量声明文件
/// <reference types="./types/index.d.ts" />
```

三斜线指令中有两种`types` 和 `path` 两种不同的属性，它们的区别是：`types` 用于声明对另一个库的依赖，而 `path` 用于声明对另一个文件的依赖。

##### 书写一个全局变量的声明文件

在全局变量的声明文件中，最外层是不允许出现 `import`, `export` 关键字的。一旦出现了，那么他就会被视为一个 npm 包或 UMD 库，就不再是全局变量的声明文件了。故当我们在书写一个全局变量的声明文件时，如果需要引用另一个库的类型，那么就必须用三斜线指令了： 

```ts
// types/jquery-plugin/index.d.ts

/// <reference types="jquery" />

declare function foo(options: JQuery.AjaxSettings): string;
```

```ts
// src/index.ts

foo({});
```

三斜线指令的语法如上，`///` 后面使用 xml 的格式添加了对 `jquery` 类型的依赖，这样就可以在声明文件中使用 `JQuery.AjaxSettings` 类型了。

注意，三斜线指令必须放在文件的最顶端，三斜线指令的前面只允许出现单行或多行注释。

##### 依赖一个全局变量的声明文件

在另一个场景下，当我们需要依赖一个全局变量的声明文件时，由于全局变量不支持通过 `import` 导入，当然也就必须使用三斜线指令来引入了：

```ts
// types/node-plugin/index.d.ts

/// <reference types="node" />

export function foo(p: NodeJS.Process): string;
```

```ts
// src/index.ts

import { foo } from 'node-plugin';

foo(global.process); // global是node环境下的全局对象
```

###### js文件三斜杠注释///reference path用途

编辑某个js文件时，要想这个js文件出现其他js成员的ide提示，可以再js文件开头使用3个斜杠注释和reference指令的path指向此 js/ts 文件路径，这样在编写这个 js/ts 文件时，ide就会自动出现path指向的js文件中的成员。



##### 拆分声明文件

当我们的全局变量的声明文件太大时，可以通过拆分为多个文件，然后在一个入口文件中将它们一一引入，来提高代码的可维护性。比如 `jQuery` 的声明文件就是这样的：

```ts
// node_modules/@types/jquery/index.d.ts

/// <reference types="sizzle" />
/// <reference path="JQueryStatic.d.ts" />
/// <reference path="JQuery.d.ts" />
/// <reference path="misc.d.ts" />
/// <reference path="legacy.d.ts" />

export = jQuery;
```

其中用到了 `types` 和 `path` 两种不同的指令。它们的区别是：`types` 用于声明对另一个库的依赖，而 `path` 用于声明对另一个文件的依赖。

上例中，`sizzle` 是与 `jquery` 平行的另一个库，所以需要使用 `types="sizzle"` 来声明对它的依赖。而其他的三斜线指令就是将 `jquery` 的声明拆分到不同的文件中了，然后在这个入口文件中使用 `path` 将它们一一引入。

##### 自动生成声明文件

如果库的源码本身就是由 ts 写的，那么在使用 `tsc` 脚本将 ts 编译为 js 的时候，添加 `declaration` 选项，就可以同时也生成 `.d.ts` 声明文件了。

我们可以在命令行中添加 `--declaration`（简写 `-d`），或者在 `tsconfig.json` 中添加 `declaration` 选项。这里以 `tsconfig.json` 为例：

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "outDir": "lib",
        "declaration": true,
    }
}
```

上例中我们添加了 `outDir` 选项，将 ts 文件的编译结果输出到项目根目录的 `lib` 目录下，然后添加了 `declaration` 选项，设置为 `true`，表示将会由 ts 文件自动生成 `.d.ts` 声明文件，也会输出到 `lib` 目录下。 

运行 `tsc` 之后，目录结构如下：

```autoit
/path/to/project
├── lib
|  ├── bar
|  |  ├── index.d.ts
|  |  └── index.js
|  ├── index.d.ts
|  └── index.js
├── src
|  ├── bar
|  |  └── index.ts
|  └── index.ts
├── package.json
└── tsconfig.json
```

在这个例子中，`src` 目录下有两个 ts 文件，分别是 `src/index.ts` 和 `src/bar/index.ts`，它们被编译到 `lib` 目录下的同时，也会生成对应的两个声明文件 `lib/index.d.ts` 和 `lib/bar/index.d.ts`。它们的内容分别是：

```ts
// src/index.ts

export * from './bar';

export default function foo() {
    return 'foo';
}
```

```ts
// src/bar/index.ts

export function bar() {
    return 'bar';
}
```

```ts
// lib/index.d.ts

export * from './bar';
export default function foo(): string;
```

```ts
// lib/bar/index.d.ts

export declare function bar(): string;
```

可见，自动生成的声明文件基本保持了源码的结构，而将具体实现去掉了，生成了对应的类型声明。

使用 `tsc` 自动生成声明文件时，每个 ts 文件都会对应一个 `.d.ts` 声明文件。这样的好处是，使用方不仅可以在使用 `import foo from 'foo'` 导入默认的模块时获得类型提示，还可以在使用 `import bar from 'foo/lib/bar'` 导入一个子模块时，也获得对应的类型提示。

除了 `declaration` 选项之外，还有几个选项也与自动生成声明文件有关，这里只简单列举出来，不做详细演示了：

- `declarationDir` 设置生成 `.d.ts` 文件的目录
- `declarationMap` 对每个 `.d.ts` 文件，都生成对应的 `.d.ts.map`（sourcemap）文件
- `emitDeclarationOnly` 仅生成 `.d.ts` 文件，不生成 `.js` 文件

##### 发布声明文件

当我们为一个库写好了声明文件之后，下一步就是将它发布出去了。

此时有两种方案：

1. 将声明文件和源码放在一起
2. 将声明文件发布到 `@types` 下

这两种方案中优先选择第一种方案。保持声明文件与源码在一起，使用时就不需要额外增加单独的声明文件库的依赖了，而且也能保证声明文件的版本与源码的版本保持一致。

仅当我们在给别人的仓库添加类型声明文件，但原作者不愿意合并 pull request 时，才需要使用第二种方案，将声明文件发布到 `@types` 下。

**1. 将声明文件和源码放在一起**

如果声明文件是通过 `tsc` 自动生成的，那么无需做任何其他配置，只需要把编译好的文件也发布到 npm 上，使用方就可以获取到类型提示了。

如果是手动写的声明文件，那么需要满足以下条件之一，才能被正确的识别：

- 给 `package.json` 中的 `types` 或 `typings` 字段指定一个类型声明文件地址
- 在项目根目录下，编写一个 `index.d.ts` 文件
- 针对入口文件（`package.json` 中的 `main` 字段指定的入口文件），编写一个同名不同后缀的 `.d.ts` 文件

第一种方式是给 `package.json` 中的 `types` 或 `typings` 字段指定一个类型声明文件地址。比如：

```json
{
    "name": "foo",
    "version": "1.0.0",
    "main": "lib/index.js",
    "types": "foo.d.ts",
}
```

指定了 `types` 为 `foo.d.ts` 之后，导入此库的时候，就会去找 `foo.d.ts` 作为此库的类型声明文件了。

`typings` 与 `types` 一样，只是另一种写法。

如果没有指定 `types` 或 `typings`，那么就会在根目录下寻找 `index.d.ts` 文件，将它视为此库的类型声明文件。

如果没有找到 `index.d.ts` 文件，那么就会寻找入口文件（`package.json` 中的 `main` 字段指定的入口文件）是否存在对应同名不同后缀的 `.d.ts` 文件。

比如 `package.json` 是这样时：

```json
{
    "name": "foo",
    "version": "1.0.0",
    "main": "lib/index.js"
}
```

就会先识别 `package.json` 中是否存在 `types` 或 `typings` 字段。发现不存在，那么就会寻找根目录下是否存在 `index.d.ts` 文件。如果还是不存在，那么就会寻找是否存在 `lib/index.d.ts` 文件。假如说连 `lib/index.d.ts` 都不存在的话，就会被认为是一个没有提供类型声明文件的库了。

 有的库为了支持导入子模块，比如 `import bar from 'foo/lib/bar'`，就需要额外再编写一个类型声明文件 `lib/bar.d.ts` 或者 `lib/bar/index.d.ts`，这与自动生成声明文件类似，一个库中同时包含了多个类型声明文件。 

**2. 将声明文件发布到 `@types` 下**

如果我们是在给别人的仓库添加类型声明文件，但原作者不愿意合并 pull request，那么就需要将声明文件发布到 `@types` 下。

与普通的 npm 模块不同，`@types` 是统一由 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 管理的。要将声明文件发布到 `@types` 下，就需要给 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 创建一个 pull-request，其中包含了类型声明文件，测试代码，以及 `tsconfig.json` 等。

pull-request 需要符合它们的规范，并且通过测试，才能被合并，稍后就会被自动发布到 `@types` 下。

在 [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 中创建一个新的类型声明，需要用到一些工具，[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/) 的文档中已经有了[详细的介绍](https://github.com/DefinitelyTyped/DefinitelyTyped#create-a-new-package)，这里就不赘述了，以官方文档为准。

### 进阶

#### 类型别名

 类型别名用来给一个类型起个新名字。 

```ts
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

上例中，我们使用 `type` 创建类型别名。

类型别名常用于联合类型。

#### 字符串字面量类型

字符串字面量类型用来约束取值只能是某几个字符串中的一个。

```ts
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dblclick'); // 报错，event 不能为 'dblclick'

// index.ts(7,47): error TS2345: Argument of type '"dblclick"' is not assignable to parameter of type 'EventNames'.
```

上例中，我们使用 `type` 定了一个字符串字面量类型 `EventNames`，它只能取三种字符串中的一种。

注意，**类型别名与字符串字面量类型都是使用 `type` 进行定义。**

#### 模板字面量类型

模板字面量类型和 JavaScript 中的模板字符串语法完全一致，只不过是用在类型定义里面：

```ts
type topBottom = "top" | "bottom"
type leftRight = "left" | "right"

type Position = `${topBottom }-${leftRight }`
```

当我们定义了一个具体的字面量类型时，TypeScript 会通过拼接内容的方式产生新的字符串字面量类型。

#### 元组

数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

元组起源于函数编程语言（如 F#），这些语言中会频繁使用元组。

**例子**

定义一对值分别为 `string` 和 `number` 的元组：

```ts
let tom: [string, number] = ['Tom', 25];
```

当赋值或访问一个已知索引的元素时，会得到正确的类型：

```ts
let tom: [string, number];
tom[0] = 'Tom';
tom[1] = 25;

tom[0].slice(1);
tom[1].toFixed(2);
```

声明但未初始化元祖，可以只赋值其中一项：

```ts
let tom: [string, number];
tom[0] = 'Tom';
```

但是当直接对元组类型的变量（tom）进行初始化或者赋值的时候，需要提供所有元组类型中指定的项。

```ts
// 元祖类型变量初始化或者赋值时规定元素的类型和数量，类型可以是子类型
let tom: [string, number] = ['Tom', 25]; // 不报错
let tom: [string, number]; // [null, null]
tom[0] = 'Tom'; // 不报错

let tom: [string, number] = ['Tom']; // 报错
let tom: [string, number] = [25, 'Tom']; // 报错
let tom: [string, number] = ['Tom', 25, 1]; // 报错
```

**越界的元素**

当添加越界的元素时，它的类型会被限制为元组中每个类型的联合类型：

```ts
let tom: [string, number];
tom = ['Tom', 25];
tom.push('male');
tom.push(true); // 报错

// Argument of type 'true' is not assignable to parameter of type 'string | number'.
```

#### 命名元组类型(Labeled tuple types)

命名元组类型适需要 TypeScript 4.0及以上版本才能使用，它极大的改善了我们的开发体验及效率，先来看一个例子:

```ts
// 这是没有命名的元组，语义化较差
type Address = [string, number]

function setAddress(...args: Address) {
  // some code here
  console.log(args)
}
```

```ts
// 这是命名的元组，具备语义化
type Address = [streetName: string, streetNumber: number]

function setAddress(...args: Address) {
  // some code here
  console.log(args)
}
```

这样，在调用函数时（鼠标指向函数参数Address时），我们的参数就获得了相应的语义，这使得代码更加容易维护。

#### 枚举

枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。 

枚举还具有正向映射（ `name` -> `value`）和反向映射（ `value` -> `name`）的特点。  要注意的是不会为字符串枚举成员生成反向映射。 

**数字枚举**

枚举使用 `enum` 关键字来定义：

```ts
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
```

枚举成员会被赋值为从 `0` 开始递增的数字，同时也会对枚举值到枚举名进行反向映射：

```ts
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 0); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true

console.log(Days[0] === "Sun"); // true
console.log(Days[1] === "Mon"); // true
console.log(Days[2] === "Tue"); // true
console.log(Days[6] === "Sat"); // true
```

事实上，上面的例子会被编译为：

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 0] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

**手动赋值**

我们也可以给枚举项手动赋值：

```ts
enum Days {Sun = 7, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1); // true
console.log(Days["Tue"] === 2); // true
console.log(Days["Sat"] === 6); // true
```

上面的例子中，未手动赋值的枚举项会接着上一个枚举项递增。

如果未手动赋值的枚举项与手动赋值的重复了，TypeScript 是不会察觉到这一点的：

```ts
enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 3); // true
console.log(Days["Wed"] === 3); // true
console.log(Days[3] === "Sun"); // false
console.log(Days[3] === "Wed"); // true
```

上面的例子中，递增到 `3` 的时候与前面的 `Sun` 的取值重复了，但是 TypeScript 并没有报错，导致 `Days[3]` 的值先是 `"Sun"`，而后又被 `"Wed"` 覆盖了。编译的结果是：

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 3] = "Sun";
    Days[Days["Mon"] = 1] = "Mon";
    Days[Days["Tue"] = 2] = "Tue";
    Days[Days["Wed"] = 3] = "Wed";
    Days[Days["Thu"] = 4] = "Thu";
    Days[Days["Fri"] = 5] = "Fri";
    Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

所以使用的时候需要注意，最好不要出现这种覆盖的情况。

当然，手动赋值的枚举项也可以为小数或负数，此时后续未手动赋值的项的递增步长仍为 `1`：

```ts
enum Days {Sun = 7, Mon = 1.5, Tue, Wed, Thu, Fri, Sat};

console.log(Days["Sun"] === 7); // true
console.log(Days["Mon"] === 1.5); // true
console.log(Days["Tue"] === 2.5); // true
console.log(Days["Sat"] === 6.5); // true
```

**字符串枚举**

```ts
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT",
}
```

**异构枚举**

异构枚举可以混合字符串和数字成员，但不能混合其他类型，不过可以使用断言 any，变成计算值 ：

```ts
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = "S"}; 
```

含字符串值成员的枚举中不允许使用计算值。 

但字符串值断言为any后（自身变成计算值），可以使用计算值。（其实是欺骗了tsc编译器，让编译器以为枚举中没有字符串值）

```js
var Days;
(function (Days) {
    Days[Days["Sun"] = 7] = "Sun";
    Days[Days["Mon"] = 8] = "Mon";
    Days[Days["Tue"] = 9] = "Tue";
    Days[Days["Wed"] = 10] = "Wed";
    Days[Days["Thu"] = 11] = "Thu";
    Days[Days["Fri"] = 12] = "Fri";
    Days[Days["Sat"] = "S"] = "Sat";
})(Days || (Days = {}));
```

如果要混合其它类型，此时需要使用类型断言为any来欺骗tsc，无视类型检查 (编译出的 js 仍然是可用的)：

```ts
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = true as any }; // 只能断言为any
```

值类型断言之后变成计算成员。

如果紧接在字符串值成员或者类型断言后面的是未手动赋值的项，那么它就会因为无法获得初始值而报错。

**字面量枚举成员**

字面量枚举成员是指不带有初始值的常量枚举成员，或者是值被初始化为

- 任何字符串字面量

- 任何数字字面量

- 应用了一元 `-`符号的数字字面量（例如： `-1`, `-100`）
  
  **枚举成员成为了类型， 枚举类型本身变成了每个枚举成员的联合类型** 

```ts
enum ShapeKind {
    Circle,
    Square,
}

interface Circle {
    kind: ShapeKind.Circle;
    radius: number;
}
```

**常量成员和计算成员**

枚举项有两种类型：常量成员（constant member）和计算成员（computed member）。

前面我们所举的例子都是常量成员，一个典型的计算成员的例子：

```ts
enum Color {Red, Green, Blue = "blue".length};
```

上面的例子中，`"blue".length` 就是一个计算成员。

上面的例子不会报错，但是**如果紧接在计算成员后面的是未手动赋值的项，那么它就会因为无法获得初始值而报错**：

```ts
enum Color {Red = "red".length, Green, Blue};

// index.ts(1,33): error TS1061: Enum member must have initializer.
// index.ts(1,40): error TS1061: Enum member must have initializer.
```

每个枚举成员都带有一个值，它可以是**常量**或**计算得到的**。 当满足如下条件时，枚举成员被当作是常量：

- 它是枚举的第一个成员且没有初始化器，这种情况下它被赋予值 `0`：
  
  ```ts
  // E.X is constant:
  enum E { X }
  ```

- 它不带有初始化器且它之前的枚举成员是一个数字常量。 这种情况下，当前枚举成员的值为它上一个枚举成员的值加1。
  
  ```ts
  // All enum members in 'E1' and 'E2' are constant.
  
  enum E1 { X, Y, Z }
  
  enum E2 {
      A = 1, B, C
  }
  ```

- 枚举成员使用 常量枚举表达式 初始化。 常数枚举表达式是TypeScript表达式的子集，它可以在编译阶段求值。 当一个表达式满足下面条件之一时，它就是一个常量枚举表达式：
  
  1. 一个枚举表达式字面量（主要是字符串字面量或数字字面量）
  2. 一个对之前定义的常量枚举成员的引用（可以是在不同的枚举类型中定义的）
  3. 带括号的常量枚举表达式
  4. 一元运算符`+`, `-`, `~`其中之一应用在了常量枚举表达式
  5. 常量枚举表达式做为二元运算符`+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `>>>`, `&`, `|`, `^`的操作对象。
  
  若常量枚举表达式求值后为`NaN`或`Infinity`，则会在编译阶段报错。

所有其它情况的枚举成员被当作是需要计算得出的值。

**const枚举**

const枚举是使用 `const enum` 定义的枚举类型：

```ts
const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

const枚举与普通枚举的区别是，它会在编译阶段被删除，并且不能包含计算成员。

上例的编译结果是：

```js
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

**外部枚举**

外部枚举（Ambient Enums）是使用 `declare enum` 定义的枚举类型，用来描述已经存在的枚举类型的形状

```ts
declare enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

之前提到过，`declare` 定义的类型只会用于编译时的检查，编译结果中会被删除。

上例的编译结果是：

```js
// 假设Directions已存在
var directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

外部枚举与声明语句一样，常出现在声明文件中。

同时使用 `declare` 和 `const` 也是可以的：

```ts
declare const enum Directions {
    Up,
    Down,
    Left,
    Right
}

let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
```

编译结果：

```js
var directions = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

#### 类

 传统方法中，JavaScript 通过构造函数实现类的概念，通过原型链实现继承。而在 ES6 中，我们终于迎来了 `class`。 

##### 相关概念

- 类（Class）：定义了一件事物的抽象特点，包含它的属性和方法

- 对象（Object）：类的实例，通过 `new` 生成

- 面向对象（OOP）的三大特性：封装、继承、多态

- 封装（Encapsulation）：将对数据的操作细节隐藏起来，只暴露对外的接口。外界调用端不需要（也不可能）知道细节，就能通过对外提供的接口来访问该对象，同时也保证了外界无法任意更改对象内部的数据。保护属性、可扩展、增加复用性。

- 继承（Inheritance）：子类继承父类，子类除了拥有父类的所有特性外，还有一些更具体的特性

- 多态（Polymorphism）：多态性是指一种事物的多种形态，在类中具有不同功能的函数可以使用相同的函数名。类的多态性体现在方法的重载（同一个类中方法同名不同参数）、方法的重写（子类对父类方法的重写，方法同名参数相同，返回值类型与被重写方法的返回值或其派生类型相同）和抽象类抽象方法在子类的实现。
  
  > 由继承抽象类而产生了相关的不同的类，对同一个方法可以有不同的响应。比如 `Cat` 和 `Dog` 都继承自 `Animal`，但是分别实现了自己的 `eat` 方法。此时针对某一个实例，我们无需了解它是 `Cat` 还是 `Dog`，就可以直接调用 `eat` 方法，程序会自动判断出来应该如何执行 `eat`。

- 存取器（setter & getter）：用以改变属性的赋值和读取行为

- 修饰符（Modifiers）：修饰符是一些关键字，用于限定成员或类型的性质。比如 `public` 表示公有属性或方法

- 抽象类（Abstract Class）：抽象类是供其他类继承的基类，抽象类不允许被实例化。抽象类中的抽象方法必须在子类中被实现

- 接口（Interfaces）：不同类之间公有的属性或方法，可以抽象成一个接口。接口可以被类实现（implements）。一个类只能继承自另一个类，但是可以实现多个接口

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

##### ES6中类的用法

下面我们先回顾一下 ES6 中类的用法，更详细的介绍可以参考 [ECMAScript 6 入门 - Class](http://es6.ruanyifeng.com/#docs/class)。

###### 类的由来

JavaScript 语言中，生成实例对象的传统方法是通过构造函数。下面是一个例子。

```javascript
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过`class`关键字，可以定义类。

基本上，ES6 的`class`可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用 ES6 的`class`改写，就是下面这样。

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

`Point`类除了构造方法，还定义了一个`toString()`方法。注意，定义`toString()`方法的时候，前面不需要加上`function`这个关键字，直接把函数定义放进去了就可以了。另外，方法与方法之间不需要逗号分隔，加了会报错。

ES6 的类，完全可以看作构造函数的另一种写法。

```javascript
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true
```

上面代码表明，类的数据类型就是函数，类本身指向构造函数。

使用的时候，也是直接对类使用`new`命令，跟构造函数的用法完全一致。

```javascript
class Bar {
  doStuff() {
    console.log('stuff');
  }
}

const b = new Bar();
b.doStuff() // "stuff"
```

构造函数的`prototype`属性，在 ES6 的“类”上面继续存在。事实上，类的所有方法都定义在类的`prototype`属性上面。

```javascript
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};
```

上面代码中，`constructor()`、`toString()`、`toValue()`这三个方法，其实都是定义在`Point.prototype`上面。

因此，在类的实例上面调用方法，其实就是调用原型上的方法。

```javascript
class B {}
const b = new B();

b.constructor === B.prototype.constructor === B // true
```

上面代码中，`b`是`B`类的实例，它（本身若没有constructor方法会在原型链上找）的`constructor()`方法就是`B`类原型的`constructor()`方法。

由于类的方法都定义在`prototype`对象上面，所以类的新方法可以添加在`prototype`对象上面。`Object.assign()`方法可以很方便地一次向类添加多个方法。 

`Object.assign`方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。 

```dart
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

```javascript
class Point {
  constructor(){
    // ...
  }
}

Object.assign(Point.prototype, {
  toString(){},
  toValue(){}
});
```

`prototype`对象的`constructor()`属性，直接指向“类”的本身，这与 ES5 的行为是一致的，因此我们能可以把类看成是一个构造函数 `class A {} => function A (){}`。

```javascript
Point.prototype.constructor === Point // true
```

另外，类的内部所有定义的方法（除了`constructor`），在浏览器环境下是不可枚举的（non-enumerable），在node环境下是可枚举。`constructor`无论在何种环境下都是不可枚举的。

```javascript
class Point {
  constructor(x, y) {
    // ...
  }

  toString() {
    // ...
  }
}

Object.keys(Point.prototype)
// 浏览器环境：[]
// node环境：['toString']

// Object.getOwnPropertyNames返回的是对象所有自己的属性，无论可否枚举
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

上面代码中，`toString()`方法是`Point`类内部定义的方法，它是不可枚举的。这一点与 ES5 的行为不一致。

```javascript
var Point = function (x, y) {
  // ...
};

Point.prototype.toString = function () {
  // ...
};

Object.keys(Point.prototype)
// ["toString"]
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

上面代码采用 ES5 的写法，`toString()`方法就是可枚举的。

###### constructor 方法

`constructor()`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。一个类必须有`constructor()`方法，如果没有显式定义，一个空的`constructor()`方法会被默认添加。

`constructor()`的参数可多可少。

```javascript
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```

上面代码中，定义了一个空的类`Point`，JavaScript 引擎会自动为它添加一个空的`constructor()`方法。

`constructor()`方法默认返回实例对象（即`this`），完全可以指定返回另外一个对象。

```javascript
class Foo {
  constructor() {
    return Object.create(null);
  }
}

new Foo() instanceof Foo
// false
```

上面代码中，`constructor()`函数返回一个全新的对象，结果导致实例对象不是`Foo`类的实例。

类必须使用`new`调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用`new`也可以执行。

```javascript
class Foo {
  constructor() {
    return Object.create(null);
  }
}

Foo()
// TypeError: Class constructor Foo cannot be invoked without 'new'
```

constructor 不可被枚举。

###### 类的实例

生成类的实例的写法，与 ES5 完全一样，也是使用`new`命令。前面说过，如果忘记加上`new`，像函数那样调用`Class`，将会报错。

```javascript
class Point {
  // ...
}

// 报错
var point = Point(2, 3);

// 正确
var point = new Point(2, 3);
```

与 ES5 一样，实例属性是定义在`this`指向的对象上，静态成员和方法都是定义在原型上。

> ES5 构造函数的方法是手动定义到 prototype 上

```javascript
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}

var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```

上面代码中，`x`和`y`都是实例对象`point`自身的属性（因为定义在`this`变量上），所以`hasOwnProperty()`方法返回`true`，而`toString()`是原型对象的属性（因为定义在`Point`类上），所以`hasOwnProperty()`方法返回`false`。这些都与 ES5 的行为保持一致。 

与 ES5 一样，类的所有实例共享一个原型对象。

```javascript
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__ === p2.__proto__
//true
```

上面代码中，`p1`和`p2`都是`Point`的实例，它们的原型都是`Point.prototype`，所以`__proto__`属性是相等的。

这也意味着，可以通过实例的`__proto__`属性为“类”添加方法。但是不推荐使用，因为这会改变“类”的原始定义，影响到所有实例。 

> `__proto__` 并不是语言本身的特性，这是各大厂商具体实现时添加的私有属性，虽然目前很多现代浏览器的 JS 引擎中都提供了这个私有属性，但依旧不建议在生产中使用该属性，避免对环境产生依赖。生产环境中，我们可以使用 `Object.getPrototypeOf` 方法来获取实例对象的原型，然后再来为原型添加方法/属性。

###### 取值函数（getter）和存值函数（setter）

与 ES5 一样，在“类”的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```javascript
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```

上面代码中，`prop`属性有对应的存值函数和取值函数，因此赋值和读取行为都被自定义了。

其实存值函数和取值函数是设置在 `prop` 属性的 descriptor 属性描述符上，与ES5的Object.defineProperty()一致。

###### 属性表达式

类的属性名，可以采用表达式。

```javascript
let methodName = 'getArea';

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```

上面代码中，`Square`类的方法名`getArea`，是从表达式得到的。

###### Class 表达式

与函数一样，类也可以使用表达式的形式定义。

```javascript
const MyClass = class Me {
  getClassName() {
    return Me.name;
  }
};
```

上面代码使用表达式定义了一个类。需要注意的是，这个类的名字是`Me`，但是`Me`只在 Class 的内部可用，指代当前类。在 Class 外部，这个类只能用`MyClass`引用。

```javascript
let inst = new MyClass();
inst.getClassName() // Me
Me.name // ReferenceError: Me is not defined
```

上面代码表示，`Me`只在 Class 内部有定义。

如果类的内部没用到的话，可以省略`Me`，也就是可以写成下面的形式。

```javascript
const MyClass = class { /* ... */ };
```

采用 Class 表达式，可以写出立即执行的 Class。

```javascript
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName(); // "张三"
```

###### 类的继承

使用`extends`继承类，且只允许继承一个父类。

子类的构造函数中的 "this" 前，必须调用 "super" ，且传入父类构造函数规定的参数。可以这么理解：要想生成子类的实例，必须先生成父类的实例。

```js
class Animal {
    constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name); // this.name是父类实例的name属性
  }
}
```



###### 注意点

**（1）严格模式**

类和模块的内部，默认就是严格模式，所以不需要使用`use strict`指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。考虑到未来所有的代码，其实都是运行在模块之中，所以 ES6 实际上把整个语言升级到了严格模式。

**（2）不存在提升**

类不存在变量提升（hoist），这一点与 ES5 完全不同。

```javascript
new Foo(); // ReferenceError
class Foo {}
```

上面代码中，`Foo`类使用在前，定义在后，这样会报错，因为 ES6 不会把类的声明提升到代码头部。这种规定的原因与下文要提到的继承有关，必须保证子类在父类之后定义。

```javascript
{
  let Foo = class {};
  class Bar extends Foo {
  }
}
```

上面的代码不会报错，因为`Bar`继承`Foo`的时候，`Foo`已经有定义了。但是，如果存在`class`的提升，上面代码就会报错，因为`class`会被提升到代码头部，而`let`命令是不提升的，所以导致`Bar`继承`Foo`的时候，`Foo`还没有定义。

**（3）name 属性**

由于本质上，ES6 的类只是 ES5 的构造函数的一层包装，所以函数的许多特性都被`Class`继承，包括`name`属性。

```javascript
class Point {}
Point.name // "Point"
```

`name`属性总是返回紧跟在`class`关键字后面的类名。

**（4）Generator 方法**

如果某个方法之前加上星号（`*`），就表示该方法是一个 Generator 函数。

```javascript
class Foo {
  constructor(...args) {
    this.args = args;
  }
  * [Symbol.iterator]() {
    for (let arg of this.args) {
      yield arg;
    }
  }
}

for (let x of new Foo('hello', 'world')) {
  console.log(x);
}
// hello
// world
```

上面代码中，`Foo`类的`Symbol.iterator`方法前有一个星号，表示该方法是一个 Generator 函数。`Symbol.iterator`方法返回一个`Foo`类的默认遍历器，`for...of`循环会自动调用这个遍历器。

**（5）this 的指向**

类的方法内部如果含有`this`，它默认指向类的实例。但是，必须非常小心，一旦单独使用该方法，很可能报错。

```javascript
class Logger {
  printName(name = 'there') {
    this.print(`Hello ${name}`);
  }

  print(text) {
    console.log(text);
  }
}

const logger = new Logger();
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

上面代码中，`printName`方法中的`this`，默认指向`Logger`类的实例。但是，如果将这个方法提取出来单独使用，`this`指向该方法运行时所在的环境（由于是ES6环境严格模式，所以 this 实际指向的是`undefined`），从而导致找不到`print`方法而报错。

一个比较简单的解决方法是，在构造方法中将 `printName`方法 绑定`this`，这样就不会找不到`print`方法了。

```javascript
class Logger {
  constructor() {
    this.printName = this.printName.bind(this);
  }

  // ...
}
```

另一种解决方法是使用箭头函数。

```javascript
class Obj {
  constructor() {
    this.getThis = () => this;
  }
}

const myObj = new Obj();
myObj.getThis() === myObj // true
```

箭头函数本身是没有`this`的，它的`this`是根据外层函数的this来决定的，如它的外层函数可以这样找，向上找到( ){ }这样的函数体结构。 上面代码中，`constructor`的 `this`指向实例对象。

还有一种解决方法是使用`Proxy`，获取方法的时候，自动绑定`this`。

```javascript
function selfish (target) {
  const cache = new WeakMap();
  const handler = {
    get (target, key) {
      const value = Reflect.get(target, key);
      if (typeof value !== 'function') {
        return value;
      }
      if (!cache.has(value)) {
        cache.set(value, value.bind(target));
      }
      return cache.get(value);
    }
  };
  const proxy = new Proxy(target, handler);
  return proxy;
}

const logger = selfish(new Logger());
```

###### 静态成员

**1. 静态方法**

类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。 静态方法可以与非静态方法重名。 

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

Foo.classMethod() // 'hello'

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

注意，如果静态方法包含`this`关键字，这个`this`指的是Foo.prototype，而不是实例。

```javascript
class Foo {
  static bar() {
    this.baz();
  }
  static baz() {
    console.log('hello');
  }
  baz() {
    console.log('world');
  }
}

Foo.bar() // hello
```

父类的静态方法，可以被子类继承。

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
}

Bar.classMethod() // 'hello'
```

静态方法也是可以从`super`对象上调用的。

```javascript
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
  static classMethod() {
    return super.classMethod() + ', too';
  }
}

Bar.classMethod() // "hello, too"
```

**2.  静态属性**

静态属性指的是 Class 本身的属性，即`Class.propName`，而不是定义在实例对象（`this`）上的属性。

```javascript
class Foo {
}

Foo.prop = 1;
Foo.prop // 1
```

上面的写法为`Foo`类定义了一个静态属性`prop`。

目前，只有这种写法可行，因为 **ES6 明确规定，Class 定义时内部只有静态方法，没有静态属性**。现在[ES7提案](https://github.com/tc39/proposal-class-fields)提供了类的静态属性，加上`static`关键字。

这个新写法大大方便了静态属性的表达。

```javascript
// 老写法
class Foo {
  // ...
}
Foo.prop = 1;

// 新写法
class Foo {
  static prop = 1;
}
```

**3. 实例属性的新写法**

 实例属性除了定义在`constructor()`方法里面的`this`上面，也可以定义在类的最顶层（ES7）。 

```javascript
class IncreasingCounter {
  constructor() {
    this._count = 0;
  }
}
```

等同于

```javascript
class IncreasingCounter {
  _count = 0;
}
```

这种新写法的好处是，所有实例对象自身的属性都定义在类的头部，看上去比较简洁和整齐，一眼就能看出这个类有哪些实例属性。

```javascript
class foo {
  bar = 'hello';
  baz = 'world';

  constructor() {
    // ...
  }
}
```

**4. 私有方法和私有属性**

目前，有一个[提案](https://github.com/tc39/proposal-private-methods)，为`class`加了私有属性。方法是在属性名之前，使用`#`表示。

```javascript
class Point {
  #x;

  constructor(x = 0) {
    this.#x = +x;
  }

  get x() {
    return this.#x;
  }

  set x(value) {
    this.#x = +value;
  }
}
```

上面代码中，`#x`就是私有属性，只能在类的内部使用。如果在类的外部使用，就会报错。

私有属性不限于从`this`引用，**只要是在类的内部**，实例也可以引用私有属性。

```javascript
class Foo {
  #privateValue = 42;
  static getPrivateValue(foo) {
    return foo.#privateValue;
  }
}

Foo.getPrivateValue(new Foo()); // 42
```

这种写法不仅可以写私有属性，还可以用来写私有方法。

```javascript
class Foo {
  #a;
  #b;
  constructor(a, b) {
    this.#a = a;
    this.#b = b;
  }
  #sum() {
    return this.#a + this.#b;
  }
}
```

另外，私有属性也可以设置 getter 和 setter 方法。

```javascript
class Counter {
  #xValue = 0;

  constructor() {
    super();
    // ...
  }

  get #x() { return #xValue; }
  set #x(value) {
    this.#xValue = value;
  }
}
```

上面代码中，`#x`是一个私有属性，它的读写都通过`get #x()`和`set #x()`来完成。

 私有属性和私有方法前面，也可以加上`static`关键字，表示这是一个静态的私有属性或私有方法。 

**5. new.target 属性**

 `new`是从构造函数生成实例对象的命令。ES6 为`new`命令引入了一个`new.target`属性，该属性一般用在构造函数之中，返回`new`命令作用于的那个构造函数。如果构造函数不是通过`new`命令或`Reflect.construct()`调用的，`new.target`会返回`undefined`，因此这个属性可以用来确定构造函数是怎么调用的。

注意，在函数外部，使用`new.target`会报错。 

```javascript
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```

上面代码确保构造函数只能通过`new`命令调用。

Class 内部调用`new.target`，返回当前 Class。

需要注意的是，子类继承父类时，`new.target`会返回子类。

```javascript
class Rectangle {
  constructor(length, width) {
    console.log(new.target === Rectangle);
    // ...
  }
}

class Square extends Rectangle {
  constructor(length, width) {
    super(length, width);
  }
}

var obj = new Square(3,4); // 输出 false
```

 上面代码中，`new.target`会返回子类 Square 。 

利用这个特点，可以写出不能独立使用、必须继承后才能使用的类。

```javascript
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    // ...
  }
}

var x = new Shape();  // 报错
var y = new Rectangle(3, 4);  // 正确
```

上面代码中，`Shape`类不能被实例化，只能用于继承。



###### 实例方法是可以指定 this 的类型的

```ts
class Dong {
    name: string;

    constructor() {
        this.name = "dong";
    }

    hello(this: Dong) {
        return 'hello, I\'m ' + this.name;
    }
}
```

这样，就只能限定this的指向，当 bind/call/apply 调用的时候，就会报错，并检查出 this 指向的对象是否是对的。

##### TypeScript中类的用法

当我们在声明 `class Demo` 时，除了会创建一个名为 `Demo` 的类之外，同时也创建了一个名为 `Demo` 的类型（实例类型—对应的接口），详细解释请查看 **接口继承类** 章节。 

TypeScript中类的属性必须要在构造函数外声明。

###### public private 和 protected

TypeScript 可以使用三种访问修饰符（Access Modifiers，分别是 `public`、`private` 和 `protected`。

- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是 `public` 的，因此可省略 `public`。
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问（实例也不能访问）。
- `protected` 修饰的属性或方法是受保护的，它和 `private` 类似，区别是它在子类中也是允许被访问的。

下面举一些例子：

```ts
class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';
console.log(a.name); // Tom
```

 需要注意的是，TypeScript 编译之后的代码中，并没有限制 `private` 属性在外部的可访问性。 

```ts
class Animal {
  private name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // 报错
a.name = 'Tom'; // 报错

// index.ts(9,13): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

上面的例子编译后的代码是：

```js
var Animal = (function () {
  function Animal(name) {
    this.name = name;
  }
  return Animal;
})();
var a = new Animal('Jack');
console.log(a.name);
a.name = 'Tom';
```

使用 `private` 修饰的属性或方法，在子类中也是不允许访问的：

```ts
class Animal {
  private name;
  public constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name);
  }
}

// index.ts(11,17): error TS2341: Property 'name' is private and only accessible within class 'Animal'.
```

而如果是用 `protected` 修饰，则允许在子类中访问：

```ts
class Animal {
  protected name;
  public constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    console.log(this.name); // this.name是父类实例的name属性
  }
}
```

当构造函数修饰为 `private` 时，该类不允许被继承或者实例化：

```ts
class Animal {
  public name;
  private constructor(name) {
    this.name = name;
  }
}
class Cat extends Animal {
  constructor(name) {
    super(name);
  }
}

let a = new Animal('Jack');

// index.ts(7,19): TS2675: Cannot extend a class 'Animal'. Class constructor is marked as private.
// index.ts(13,9): TS2673: Constructor of class 'Animal' is private and only accessible within the class declaration.
```

当构造函数修饰为 `protected` 时，该类只允许被继承：

```ts
class Animal {
  public name;
  protected constructor(name) {
    this.name = name;
  }
}
class Cat extends Animal {
  constructor(name) {
    super(name);
  }
}

let a = new Animal('Jack');

// index.ts(13,9): TS2674: Constructor of class 'Animal' is protected and only accessible within the class declaration.
```

###### 私有类字段

TypeScript 3.8 将支持 ECMAScript 私有字段，千万别和 TypeScript private 修饰符 混淆。

这是在 TypeScript 中具有私有类字段的类：

```ts
class Animal {
  #name: string;
  constructor(theName: string) {
    this.#name = theName;
  }
}
```

私有类字段`#`有更好的运行时保证。用`private`关键字声明的 TypeScript 字段将在编译后的JavaScript代码中成为常规字段，非私有。另一方面，私有类字段在编译后的代码中仍然是私有的。

试图在运行时访问私有类字段将导致语法错误。我们也使用浏览器开发工具也检查不了私有类字段。

有了私有类字段，我们终于在JavaScript中得到了真正的私有。

###### constructor的简洁写法

```ts
class Animal {
  // public readonly name;
  constructor(public readonly name) {
    // this.name = name;
  }
}
```

###### 参数属性

修饰符和`readonly`还可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使代码更简洁。

```ts
class Animal {
  // public name: string;
  public constructor(public name) {
    // this.name = name;
  }
}
```

###### readonly

只读属性关键字，只允许出现在属性声明或索引签名或构造函数中。

```ts
class Animal {
  readonly name;
  public constructor(name) {
    this.name = name;
  }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom'; // 报错

// index.ts(10,3): TS2540: Cannot assign to 'name' because it is a read-only property.
```

注意如果 `readonly` 和其他访问修饰符同时存在的话，需要写在其后面。

```ts
class Animal {
  // public readonly name;
  public constructor(public readonly name) {
    // this.name = name;
  }
}
```

###### 抽象类

`abstract` 用于定义抽象类和其中的抽象方法。

什么是抽象类？

首先，抽象类是不允许被实例化的：

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

let a = new Animal('Jack');

// index.ts(9,11): error TS2511: Cannot create an instance of the abstract class 'Animal'.
```

上面的例子中，我们定义了一个抽象类 `Animal`，并且定义了一个抽象方法 `sayHi`。在实例化抽象类的时候报错了。

其次，抽象类中的抽象方法必须被子类实现：

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public eat() {
    console.log(`${this.name} is eating.`);
  }
}

let cat = new Cat('Tom');

// index.ts(9,7): error TS2515: Non-abstract class 'Cat' does not implement inherited abstract member 'sayHi' from class 'Animal'.
```

上面的例子中，我们定义了一个类 `Cat` 继承了抽象类 `Animal`，但是没有实现抽象方法 `sayHi`，所以编译报错了。

下面是一个正确使用抽象类的例子：

```ts
abstract class Animal {
  public name;
  public constructor(name) {
    this.name = name;
  }
  public abstract sayHi();
}

class Cat extends Animal {
  public sayHi() {
    console.log(`Meow, My name is ${this.name}`);
  }
}

let cat = new Cat('Tom');
```

上面的例子中，我们实现了抽象方法 `sayHi`，编译通过了。

需要注意的是，即使是抽象方法，TypeScript 的编译结果中，仍然会存在这个类，上面的代码的编译结果是：

```js
var __extends =
  (this && this.__extends) ||
  function (d, b) {
    for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
    function __() {
      this.constructor = d;
    }
    d.prototype = b === null ? Object.create(b) : ((__.prototype = b.prototype), new __());
  };
var Animal = (function () {
  function Animal(name) {
    this.name = name;
  }
  return Animal;
})();
var Cat = (function (_super) {
  __extends(Cat, _super);
  function Cat() {
    _super.apply(this, arguments);
  }
  Cat.prototype.sayHi = function () {
    console.log('Meow, My name is ' + this.name);
  };
  return Cat;
})(Animal);
var cat = new Cat('Tom');
```

###### 类的类型

在没有加上类型之前，属性、方法、参数的类型都默认为`any`， 但可以从用法中通过类型推论推断出更好的类型。 

给类加上 TypeScript 的类型很简单，与接口类似：

```ts
class Animal {
  name: string;
  constructor(name: string) {
    this.name = name;
  }
  sayHi(): string {
    return `My name is ${this.name}`;
  }
}

let a: Animal = new Animal('Jack');
console.log(a.sayHi()); // My name is Jack
```

#### 接口

接口（Interfaces）可以用于对「对象的形状（Shape）」进行描述，接口的属性可以初始化。

这一章主要介绍接口的另一个用途，对类的一部分行为的抽象集合（方法只能声明不能定义，属性可以初始化），而具体如何行动需要由类（class）去实现（implement）。

##### 类实现接口

实现（implements）是面向对象中的一个重要概念。一般来讲，一个类只能继承自另一个类，有时候不同类之间可以有一些共有的特性，这时候就可以把特性提取成接口（interfaces），用 `implements` 关键字来实现。这个特性大大提高了面向对象的灵活性。

举例来说，门是一个类，防盗门是门的子类。如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法。这时候如果有另一个类，车，也有报警器的功能，就可以考虑把报警器提取出来，作为一个接口，防盗门和车都去实现它：

```ts
interface Alarm {
    alert(): void;
}

class Door {
}

class SecurityDoor extends Door implements Alarm {
    alert() {
        console.log('SecurityDoor alert');
    }
}

class Car implements Alarm {
    alert() {
        console.log('Car alert');
    }
```

一个类可以实现多个接口：

```ts
interface Alarm {
    alert(): void;
}

interface Light {
    lightOn(): void;
    lightOff(): void;
}

class Car implements Alarm, Light {
    alert() {
        console.log('Car alert');
    }
    lightOn() {
        console.log('Car light on');
    }
    lightOff() {
        console.log('Car light off');
    }
}
```

上例中，`Car` 实现了 `Alarm` 和 `Light` 接口，既能报警，也能开关车灯。

因为接口是将公有特性抽取在一起，因此接口的属性和方法一般是声明未定义，由不同的类去具体实现，类实现接口时必须实现接口的所有属性和方法。

##### 接口继承接口

接口与接口之间可以是继承关系：

```ts
interface Alarm {
    alert(): void;
}

interface LightableAlarm extends Alarm {
    lightOn(): void;
    lightOff(): void;
}
```

这很好理解，`LightableAlarm` 继承了 `Alarm`，除了拥有 `alert` 方法之外，还拥有两个新方法 `lightOn` 和 `lightOff`。

一个接口可以继承多个接口：

```ts
interface Alarm {
  alert(): void;
}
interface Speed {
  increase():void;
}
interface LightableAlarm extends Alarm,Speed {
  lightOn(): void;
  lightOff(): void;
}
```

##### 接口继承类

常见的面向对象语言中，接口是不能继承类的，但是在 TypeScript 中却是可以的：

```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

为什么 TypeScript 会支持接口继承类呢？

实际上，当我们在声明 `class Point` 时，除了会创建一个名为 `Point` 的类之外，同时也创建了一个名为 `Point` 的类型（实例的类型—接口）。

所以我们既可以将 `Point` 当做一个类来用（使用 `new Point` 创建它的实例）：

```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

const p = new Point(1, 2);
```

也可以将 `Point` 当做一个类型来用（使用 `: Point` 表示参数的类型）：

> 需要注意的是该类型与`class Point`是两个东西，`: Point` 表示参数的类型是接口，`class Point`是构造函数

```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

function printPoint(p: Point) {
    console.log(p.x, p.y);
}

printPoint(new Point(1, 2));
```

这个例子实际上可以等价于：

```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface PointInstanceType {
    x: number;
    y: number;
}

function printPoint(p: PointInstanceType) {
    console.log(p.x, p.y);
}

printPoint(new Point(1, 2));
```

上例中我们新声明的 `PointInstanceType` 类型，与声明 `class Point` 时创建的 `Point` 类型是等价的。

所以回到 `Point3d` 的例子中，我们就能很容易的理解为什么 TypeScript 会支持接口继承类了：

```ts
class Point {
    x: number;
    y: number;
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
}

interface PointInstanceType {
    x: number;
    y: number;
}

// 等价于 interface Point3d extends PointInstanceType
interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

当我们声明 `interface Point3d extends Point` 时，`Point3d` 继承的实际上是类 `Point` 的实例的类型。

换句话说，可以理解为定义了一个接口 `Point3d` 继承另一个接口 `PointInstanceType`。

**所以「接口继承类」和「接口继承接口」没有什么本质的区别。**

值得注意的是，`PointInstanceType` 相比于 `Point`，缺少了 `constructor` 方法，这是因为声明 `Point` 类时创建的 `Point` 类型是不包含构造函数的。另外，除了构造函数是不包含的，静态属性或静态方法也是不包含的（实例的类型当然不应该包括构造函数、静态属性或静态方法）。

换句话说，声明 `Point` 类时创建的 `Point` 类型只包含其中的实例属性和实例方法：

```ts
class Point {
    /** 静态属性，坐标系原点 */
    static origin = new Point(0, 0);
    /** 静态方法，计算与原点距离 */
    static distanceToOrigin(p: Point) {
        return Math.sqrt(p.x * p.x + p.y * p.y);
    }
    /** 实例属性，x 轴的值 */
    x: number;
    /** 实例属性，y 轴的值 */
    y: number;
    /** 构造函数 */
    constructor(x: number, y: number) {
        this.x = x;
        this.y = y;
    }
    /** 实例方法，打印此点 */
    printPoint() {
        console.log(this.x, this.y);
    }
}

interface PointInstanceType {
    x: number;
    y: number;
    printPoint(): void;
}

let p1: Point;
let p2: PointInstanceType;
```

上例中最后的类型 `Point` 和类型 `PointInstanceType` 是等价的。

同样的，在接口继承类的时候，也只会继承它的实例属性和实例方法。

一个接口可以继承多个类。

##### 函数参数使用接口时可以解构

```ts
interface E {
  a: number
  b: string
}
// 解构
function s({ a }: E) {}
```

#### TS抽象类和接口的区别

**抽象类的使用原则：**

1. 抽象类不能被实例化，需要依靠子类采用向上转型的方式处理；

2. 抽象类必须有子类去继承，一个子类只能继承一个继承抽象类；

3. 抽象方法必须是public和protected（因为如果是private，则不能被子类继承，子类就不能实现此方法）；

4. 如果子类继承了此抽象类，则子类必须要重写抽象类中的全部抽象方法（如果子类没有全部重写父类中的抽象方法，则子类也需要定义为abstract的）

5. 抽象类不能用final声明，因为抽象类必须有子类；（TS没有final关键字）

**抽象类和接口的区别：**

1. 抽象类里面可以有方法的实现，但是接口完全都是抽象的，不存在方法的实现；
2. 子类只能继承一个抽象类，而接口可以被多个类实现、或被多个接口继承；
3. 抽象方法可以是public，protected，但是接口只能是public，默认的；
4. 抽象类可以有构造器，而接口不能有构造器；
5. 抽象类当做父类，被继承。且抽象类的派生类的构造函数中必须调用super()；接口可以当做“子类”继承其他类

#### 泛型

 泛型（Generics）是指在定义函数签名、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型。可以理解为类型变量。

- 把类型当作参数传递
- 作用：不用写死类型

泛型可跟在函数名、类名、接口名、类型别名 的后面，数组、对象、箭头函数的前面。

**简单的例子**

首先，我们来实现一个函数 `createArray`，它可以创建一个指定长度的数组，同时将每一项都填充一个默认值：

```ts
function createArray(length: number, value: any): Array<any> {
    let result = []; // 默认是any类型
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

上例中，我们使用了之前提到过的数组泛型来定义返回值的类型。

这段代码编译不会报错，但是一个显而易见的缺陷是，它并没有准确的定义返回值的类型：

`Array` 允许数组的每一项都为任意类型。但是我们预期的是，数组中每一项都应该是输入的 `value` 的类型。

这时候，泛型就派上用场了：

```ts
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

上例中，我们在函数名后添加了 `<T>`，其中 `T` 用来指代任意输入的类型，在后面的输入 `value: T` 和输出 `Array` 中即可使用了。

接着在调用的时候，可以指定它具体的类型为 `string`。当然，也可以不手动指定，而让类型推论自动推算出来：

```ts
function createArray<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

**多个类型参数**

定义泛型的时候，可以一次定义多个类型参数：

```ts
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

上例中，我们定义了一个 `swap` 函数，用来交换输入的元组。

**泛型约束**

在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法：

```ts
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);
    return arg;
}

// index.ts(2,19): error TS2339: Property 'length' does not exist on type 'T'.
```

上例中，泛型 `T` 不一定包含属性 `length`，所以编译的时候报错了。

这时，我们可以对泛型进行约束，只允许这个函数传入那些包含 `length` 属性的变量。这就是泛型约束：

```ts
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

上例中，我们使用了 `extends` 约束了泛型 `T` 必须符合接口 `Lengthwise` 的形状，也就是必须包含 `length` 属性。

此时如果调用 `loggingIdentity` 的时候，传入的 `arg` 不包含 `length`，那么在编译阶段就会报错了：

```ts
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}

loggingIdentity(7);

// index.ts(10,17): error TS2345: Argument of type '7' is not assignable to parameter of type 'Lengthwise'.
```

多个类型参数之间也可以互相约束：

```ts
function copyFields<T extends U, U>(target: T, source: U): T {
    for (let id in source) {
        target[id] = (<T>source)[id]; // 与 子类实例可以赋值为父类类型变量 相似
    }
    return target;
}

let x = { a: 1, b: 2, c: 3, d: 4 };

copyFields(x, { b: 10, d: 20 });
```

上例中，我们使用了两个类型参数，其中要求 `T` 继承 `U`，这样就保证了 `U` 上不会出现 `T` 中不存在的字段。

**泛型接口**

之前学习过，可以使用接口的方式来定义一个函数需要符合的形状：

```ts
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    return source.search(subString) !== -1;
}
```

当然也可以使用含有泛型的接口来定义函数的形状：

```ts
interface CreateArrayFunc {
    <T>(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

进一步，我们可以把泛型参数提前到接口名上：

```ts
interface CreateArrayFunc<T> {
    (length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

```ts
// A extends E = {a:1} 先执行A extends E 再执行赋值运算，因此{a:1}是默认值
// 因此在使用DispatchProp时不需再传入类型。
interface E {
  a: number
}
interface DispatchProp<
  A extends E = {
    a: 1
  }
> {
  dispatch: A
}
let a: DispatchProp = {
  dispatch: {
    a: 1,
  },
}

// 没有默认值的情况
// 使用DispatchProp时需要传入类型。
interface E {
  a: number
}
interface DispatchProp<A extends E> {
  dispatch: A
}
let a: DispatchProp<{ a: 1 }> = {
  dispatch: {
    a: 1,
  },
}
```

**泛型类**

与泛型接口类似，泛型也可以用于类的类型定义中：

```ts
class GenericNumber<T> {
    zeroValue: T;
    add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

**泛型参数的默认类型**

在 TypeScript 2.3 以后，我们可以为泛型中的类型参数指定默认类型。当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用。

```ts
function createArray<T = string>(length: number, value: T): Array<T> {
    let result: T[] = [];
    for (let i = 0; i < length; i++) {
        result[i] = value;
    }
    return result;
}
```

#### 声明合并

如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型：

**重载**

之前学习过，我们可以使用重载定义多个函数类型：

```ts
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

**接口的合并**

接口中的属性在合并时会简单的合并到一个接口中：

```ts
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```

相当于：

```ts
interface Alarm {
    price: number;
    weight: number;
}
```

注意，**合并的属性的类型必须是唯一的**：

```ts
interface Alarm {
    price: number;
}
interface Alarm {
    price: number;  // 虽然重复了，但是类型都是 `number`，所以不会报错
    weight: number;
}
interface Alarm {
    price: number;
}
interface Alarm {
    price: string;  // 类型不一致，会报错
    weight: number;
}

// index.ts(5,3): error TS2403: Subsequent variable declarations must have the same type.  Variable 'price' must be of type 'number', but here has type 'string'.
```

接口中方法的合并，与重载一样：

```ts
interface Alarm {
    price: number;
    alert(s: string): string;
}
interface Alarm {
    weight: number;
    alert(s: string, n: number): string;
}
```

相当于：

```ts
interface Alarm {
    price: number;
    weight: number;
    alert(s: string): string;
    alert(s: string, n: number): string;
}
```

**类的合并**

类的合并与接口的合并规则一致。

#### 装饰器

若要启用实验性的装饰器特性，你必须在命令行或`tsconfig.json`里启用`experimentalDecorators`编译器选项： 

**命令行：**

```shell
tsc --target ES5 --experimentalDecorators
```

或

**tsconfig.json：**

```json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```

装饰器是一种特殊类型的声明，它能够被附加到类声明、属性、方法、参数、访问符上。

通俗的说，装饰器就是一个方法，可以注入到类、属性、方法、参数、访问符上进行扩展。

常见的装饰器有：类装饰器、属性装饰器、方法装饰器、参数装饰器。

装饰器的写法：普通装饰器（无法传入被修饰对象以外的参数）、装饰器工厂（可以传入被修饰对象以外的参数）

##### 类装饰器

类装饰器在类声明顶部声明，后面不能加 `;`。

类装饰器在运行时会被调用，类装饰器应用于类的构造函数，可以监视、修改、替换类的定义，传入一个参数：类的构造函数。

###### 普通装饰器

```ts
// 普通装饰器（无法传入被修饰对象以外的参数）
function logClass(params:Function):void{
  // params是被修饰的类的构造函数
  console.log(params);
  // class中实例属性是定义在`this`指向的对象上，静态成员和方法都是定义在原型上
  params.prototype.apiUrl = 'xxx'  // 定义在构造函数的原型上，实例也可以访问到
}

@logClass
class HttpClient{
  constructor(){}
}
// 需要指定any类型(可以在任意值上访问任何属性和方法)
// 否则编译会因为不知道原型上有apiUrl属性而报错
let hc:any = new HttpClient(); // 打印 [Function: HttpClient]
console.log(hc.apiUrl); // 'xxx'
```

###### 装饰器工厂

```ts
// 装饰器工厂（可以传入被修饰对象以外的参数）
function logClass(params:string):Function{
  // params:被修饰类的构造函数以外的参数
  // target:被修饰类的构造函数
  // 返回的这个函数才是真正的装饰器
  return function (target:Function ):void{
    console.log(target);
    console.log(params);
  }
}

@logClass('test')
class HttpClient{
  constructor(){
    console.log('constructor');
  }
}
// 需要指定any类型(可以在任意值上访问任何属性和方法)
let hc:any = new HttpClient();
// 打印
// [Function: HttpClient]
// test
// constructor
```

###### 类装饰器重载类

类装饰器在运行时会被调用，如果类装饰器返回一个值（只能是函数或者class）。

若是函数，则执行函数体，不再执行旧类的constructor，但依然可以通过new params()调用旧类的constructor生成旧类的实例对象。

若是class，则先调用被修饰类的constructor，然后再执行新类的constructor，对于类的方法，要么合并，要么新类覆盖旧类的方法。

方式一：

```ts
function logClass(params: any): any {
  // 类的重载
  return class extends params {
    apiUrl: number
    id: number
    constructor() {
      super()
      this.apiUrl = 1
      this.id = 1
    }
    getData(): void {
      console.log(this.apiUrl)
      console.log(this.id)
    }
  }
}

@logClass
class HttpClient {
  apiUrl: string
  constructor() {
    this.apiUrl = '我是类构造函数中的apiUrl'
    console.log('旧类')
  }
  getData(): void {
    console.log(this.apiUrl)
  }
}

let hc: any = new HttpClient()
hc.getData()
// 旧类
// 1
// 1
```

方式二：

```ts
function logClass(params:any):any{
  // 类的重载
  return class {
    apiUrl : string;
    id:number;
    constructor(){
      this.apiUrl = '我是重载的apiUrl'
      this.id = 1
    }
    getData():void{
      console.log(this.apiUrl);
      console.log(this.id);
    }
  }
}

@logClass
class HttpClient{
  apiUrl : string;
  constructor(){
    this.apiUrl = '我是类构造函数中的apiUrl'
  }
  getData():void{
    console.log(this.apiUrl);
  }
}

let hc:any = new HttpClient()
hc.getData() 
// 我是重载的apiUrl
// 1
```

##### 属性装饰器

属性装饰器在运行时会被调用，传入2个参数：

1. 对于静态成员：是类的构造函数，对于实例成员：是类的构造函数的原型对象
2. 成员的名字

属性装饰器在属性声明上面声明，后面不能加 `;`。

```ts
// 装饰器工厂
function logProperty(params:any):Function{
  // 属性装饰器在运行时会被当作函数调用，传入2个参数：
  // 1. 对于静态成员是类的构造函数，对于实例成员是类的构造函数的原型对象
  // 2. 成员的名字
  return function (target:any ,name:string|number):void{
    console.log(target);
    console.log(name);
  }
}

class HttpClient{
  @logProperty(null)
  apiUrl : string;
  constructor(){
    this.apiUrl = '我是类构造函数中的apiUrl'
  }
  getData():void{
    console.log(this.apiUrl);
  }
}
let hc = new HttpClient()
// 打印
// { getData: [Function] }
// apiUrl
```

##### 方法装饰器

方法装饰器会被用到方法的属性描述符上，可以用来监视、修改或者替换方法定义。

如果方法装饰器要返回一个值，则需要用它作为方法的属性描述符。 

方法装饰器会在运行时传入下列3个参数：

1. 对于静态成员是类的构造函数，对于实例成员是类的构造函数的原型对象

2. 成员的名字

3. 成员的属性描述符

```ts
// 装饰器工厂
function logMethod(params:any):Function{
  return function (target:any ,name:string,desc:any):void{
    console.log(target);
    console.log(name);
    console.log(desc);
  }
}

class HttpClient{
  apiUrl : string;
  constructor(){
    this.apiUrl = '我是类构造函数中的apiUrl'
  }
  @logMethod(null)
  getData():void{
    console.log(this.apiUrl);
  }
}
let hc = new HttpClient()
// 打印
// { getData: [Function] }

// getData

// {
//   value: [Function],
//   writable: true,
//   enumerable: true,
//   configurable: true
// }
```

###### 方法装饰器重写方法

```ts
// 装饰器工厂
function logMethod(params:any):Function{
  return function (target:any ,name:string,desc:any):void{
    // 方法重写
    // 把方法传入的所有参数改为string类型
    // args是方法传入的所有参数的数组
    desc.value = function (...args:any[]):string[]{
      args = args.map((value:any):string => String(value))
      console.log(args);
    }
  }
}

class HttpClient{
  constructor(){}
  @logMethod(null)
  getData(...args:any[]):void{}
}
let hc = new HttpClient()
hc.getData(1,true) // [ '1', 'true' ]
```

###### 方法装饰器扩展方法

与上述的重写类似

```ts
// 装饰器工厂
function logMethod(params:any):Function{
  return function (target:any ,name:string,desc:any):void{
    let method:Function = desc.value
    desc.value = function (...args:any[]):void{
      args = args.map((value:any):string => String(value))
      console.log(args);
      method.apply(this,args)
    }
  }
}

class HttpClient{
  constructor(){}
  @logMethod(null)
  getData(...args:any[]):void{
    console.log('我在类里');
    console.log(args);
  }
}
let hc = new HttpClient()
hc.getData(1,true) 
// [ '1', 'true' ]
// 我在类里
// [ '1', 'true' ]
```

##### 访问器装饰器

在`set`或`get`前声明。

 不允许同时装饰一个成员的`get`和`set`访问器。 

访问器装饰器会在运行时传入下列3个参数：

1. 对于静态成员是类的构造函数，对于实例成员是类的构造函数的原型对象

2. 成员的名字

3. 成员的属性描述符

```ts
// 装饰器工厂
function configurable(value: boolean) {
    return function (target: any, propertyKey: string, descriptor: PropertyDescriptor) {
        descriptor.configurable = value;
    };
}

class Point {
    private _x: number;
    private _y: number;
    constructor(x: number, y: number) {
        this._x = x;
        this._y = y;
    }

    @configurable(false)
    get x() { return this._x; }

    @configurable(false)
    get y() { return this._y; }
}
```

##### 参数装饰器

在对应的单个参数前面声明。

参数装饰器会在运行时会被调用，可以使用参数装饰器为类的原型增加一些元数据，传入下列3个参数：

1. 对于静态成员是类的构造函数，对于实例成员是实例的原型对象
2. 方法的名字
3. 参数在函数参数列表中的索引

```ts
// 装饰器工厂
function logParams(params:any){
  return function (target:any,name:string,index:number){
    console.log(target);
    console.log(name);
    console.log(index);
  }
}

class HttpClient{
  constructor(){}
  getData(@logParams(null) id:number,@logParams(null)name:string):void{
  }
}
let hc = new HttpClient()
hc.getData(123,'Tom') 
/*
HttpClient { getData: [Function] }
getData
1

HttpClient { getData: [Function] }
getData
0
*/
```

**装饰器执行顺序**

类中不同声明上的装饰器将按以下规定的顺序应用：

1. 参数装饰器，然后依次是方法装饰器，访问器装饰器，或属性装饰器应用到每个实例成员。
2. 参数装饰器，然后依次是方法装饰器，访问器装饰器，或属性装饰器应用到每个静态成员。
3. 参数装饰器应用到构造函数。
4. 类装饰器应用到类。

> 如果同一类的装饰器有多个，则遵循从下往上，从右到左的顺序（先执行后面的装饰器，可能跟栈有关：先进后出）



#### 协变和逆变

https://juejin.cn/post/6844904169921314829#heading-1

https://zhuanlan.zhihu.com/p/410029602

**结论：函数参数要逆变，返回值（或对象属性）要协变**

协变和逆变是编程理论中一个很重要的话题。用于表达父类子类在安全类型转换后的兼容性（或者说继承关系）。定义为：`f()`表示类型转换

- 协变时：`A` 通过 `fn()`，生成表示类型更广的类型 `B`，关系： `A extends B`
- 逆变时：`A` 通过 `fn()`，生成表示类型更窄的类型 `B`,   关系： `B extends A`
- 双变时：则以上均成立
- 不变时：若，则以上均不成立，没有兼容关系

```js
class Animal {
  move(){
    console.log("animal is moving");
  }
}

class Cat extends Animal {
  purr() {
    console.log("cat is purring");
  }
}

class WhiteCat extends Cat {
  showoffColor() {
    console.log("see my hair color");
  }
}

```

我们有名为`Animial`的父类，`Cat`是`Animal`的子类。`WhiteCat`是`Cat`的子类， 即`WhiteCat -> Cat -> Animal`。根据父类兼容子类的原则可知：

```js
let animal: Animal;
let cat: Cat;
let whiteCat: WhiteCat;

animal = cat;
animal = whiteCat;
cat = whiteCat;
```

##### 抛出问题

假如现在有一个函数，类型为`(param: Cat) => Cat`。那么它的兼容类型是什么呢？

我们可以把这个问题分解成两个部分参数兼容性和返回值兼容性。

- `(param: Cat) => void`的兼容类型是什么？
- `() => Cat`的兼容类型是什么？

##### 参数兼容性

我们假设`(param: Cat) => void`为`A`，此时有以下两种函数：

- `B`: `(param: WhiteCat) => void`
- `C`：`(param: Animal) => void`

那么`A`兼容哪一个函数？

**假设兼容B**

那么此时 `A = B`成立：

```js
let A: (param: Cat) => void;
const B = (param: WhiteCat) => {
  param.move();
  param.purr();
  param.showoffColor();
};

A = B;
A(new Cat());
```

函数运行到`param.showoffColor()`会报错。那么假设不成立。

**假设兼容C**

那么此时 `A = C`成立：

```js
let A: (param: Cat) => void;
const C = (param: Animal) => {
  param.move();
};

A = C;
A(new Cat());

```

此时函数成功运行。那么假设成立。

所以`(param: Animal) => void -> (param: Cat) => void` 。根据前面的定义可以看出函数参数是逆变的。

**再举一个例子**

```ts
let fn1!: (a: string, b: number) => void;
let fn2!: (a: string, b: number, c: boolean) => void;
// 第一种情况
fn1 = fn2; // TS Error: 不能将fn2的类型赋值给fn1
fn1('a', 1); // 报错，因为实际上调用的是fn2，缺少一个参数

fn2 = fn1; // correct
fn2('a', 1, true ); // 实际上调用的是fn1，多一个参数没关系
// 结论：参数选择类型更窄的那个
```





##### 返回值兼容性

我们假设`() => Cat`为`A`，此时有以下两种函数：

- `B`: `() => Animal`
- `C`：`() => WhiteCat`

那么`A`兼容哪一个函数？

**假设兼容B**

那么此时 `A = B`成立：

```js
let A: () => Cat;
const B = () => new Animal();

A = B;
const result = A();
result.move();
result.purr();
```

函数运行到`result.purr()`会报错。那么假设不成立。

**假设兼容C**

那么此时 `A = C`成立：

```js
let A: () => Cat;
const C = () => new WhiteCat();

A = C;
const result = A();
result.move();
result.purr();
```

此时函数成功运行。那么假设成立。

所以`() => WhiteCat -> () => Cat`。根据前面的定义可以看出函数返回值是协变的。

**再举一个例子**

```ts
let fn1!: (a: string, b: number) => string;
let fn2!: (a: string, b: number) => string | number | boolean;

// 第一种情况
fn2 = fn1; // correct 
let a = fn2(); // a的类型为string | number | boolean，而实际调用的是fn1，返回string

// 第二种情况
fn1 = fn2 // error: 不可以将 string|number|boolean 赋给 string 类型
let b = fn1(); // b的类型是string，实际调用的是fn2，返回 string|number|boolean
// 结论：返回值选择类型更广的那个
```



**在协变位置上，若同一个类型变量存在多个候选者，则最终的类型将被推断为联合类型**

```ts
type PropertyType<T> = T extends { id: infer U; name: infer U } ? U : T;

type User = {
  id: number;
  name: string;
};

type U1 = PropertyType<User>; // string | number
```

**在逆变位置上，若同一个类型变量存在多个候选者，则最终的类型将被推断为交叉类型**

```ts
type Bar<T> = T extends { a: (x: infer U) => void, b: (x: infer U) => void } ? U : never;

type U5 = Bar<{ a: (x: string) => void, b: (x: number) => void }>;  // string & number, 即never
```



##### 双变

在ts中，参数类型是双变的，也就是说既是协变，也是逆变。这当然不安全。所以我们可以通过开启`strictFunctionTypes`修复这个问题，保证参数类型是逆变。

那么为什么ts会让函数参数类型保留双变转换呢？下面是一个十分常见的例子：

```ts
interface Event { timestamp: number; }
interface MouseEvent extends Event { x: number; y: number }

function listenEvent(eventType: EventType, handler: (n: Event) => void) {
    /* ... */
}

// 虽然不安全，且编译无法通过，但是十分常见的使用方式
listenEvent(EventType.Mouse, (e: MouseEvent) => console.log(e.x, e.y));

// 为了保证编译通过，只能通过以下方式
listenEvent(EventType.Mouse, (e: Event) => console.log((e as MouseEvent).x, (e as MouseEvent).y));
listenEvent(EventType.Mouse, ((e: MouseEvent) => console.log(e.x, e.y)) as (e: Event) => void);

```

而如果函数参数类型是双变，那么上面第一种形式的代码也能顺利通过编译，无需使用后两种绕路的方式。



#### 类型收窄

TypeScript 类型收窄就是从宽类型转换成窄类型的过程。类型收窄常用于处理联合类型变量的场景，一个常见的例子是非空检查：

```ts
// Type is HTMLElement | null
const el = document.getElementById("foo");
if (el) {  
  // Type is HTMLElement  
  el.innerHTML = "semlinker"; 
} else {  
  // Type is null  
  alert("id为foo的元素不存在");
}
```

如果 el 为 null，则第一个分支中的代码将不会执行。因此，TypeScript 能够从此代码块内的联合类型中排除 null 类型，从而产生更窄的类型，更易于使用。

此外，你还可以通过抛出异常或从分支返回，来收窄变量的类型。例如：

```ts
// Type is HTMLElement | null
const el = document.getElementById("foo");
if (!el) throw new Error("找不到id为foo的元素");
// Type is HTMLElement
el.innerHTML = "semlinker";
```

其实在 TypeScript 中，有许多方法可以收窄变量的类型。比如使用 instanceof 运算符：

```ts
function contains(text: string, search: string | RegExp) {
  if (search instanceof RegExp) {
    // Type is RegExp
    return !!search.exec(text);
  }
  // Type is string
  return text.includes(search);
}
```

当然属性检查也是可以的：

```ts
interface Admin {  
  name: string;  
  privileges: string[];
}
interface Employee {  
  name: string;  
  startDate: Date;
}
type UnknownEmployee = Employee | Admin;
function printEmployeeInformation(emp: UnknownEmployee) {  
  console.log("Name: " + emp.name);  
  // in 关键字：如果指定的属性在指定的对象或其原型链中，则 in 运算符返回true。
  if ("privileges" in emp) {    
    // Type is Admin    
    console.log("Privileges: " + emp.privileges);  
  }  
  if ("startDate" in emp) {    
    // Type is Employee    
    console.log("Start Date: " + emp.startDate);  
  }
}
```

使用一些内置的函数，比如 `Array.isArray` 也能够收窄类型：

```ts
function contains(terms: string | string[]) {  
  const termList = Array.isArray(terms) ? terms : [terms];  
  termList; // Type is string[]  
  // ...
}
```

一般来说 TypeScript 非常擅长通过条件来判别类型，但在处理一些特殊值时要特别注意 —— 它可能包含你不想要的东西！例如，以下从联合类型中排除 null 的方法是错误的：

```ts
const el = document.getElementById("foo"); // Type is HTMLElement | null
if (typeof el === "object") {  
  el; // Type is HTMLElement | null
}
```

因为在 JavaScript 中 `typeof null` 的结果是 “object” ，所以你实际上并没有通过这种检查排除 `null` 值。除此之外，falsy 的原始值也会产生类似的问题：

```ts
function foo(x?: number | string | null) {  
  if (!x) {    
    // 除null和undefined之外还有可能是字符串或数值
    x; // Type is string | number | null | undefined   
  }
}
```

因为空字符串和 0 都属于 falsy 值，所以在分支中 x 的类型可能是 string 或 number 类型。帮助类型检查器缩小类型的另一种常见方法是在它们上放置一个明确的 “标签”：

```ts
interface UploadEvent {  
  type: "upload";  
  filename: string;  
  contents: string;
}
interface DownloadEvent {  
  type: "download";  
  filename: string;
}
type AppEvent = UploadEvent | DownloadEvent;
function handleEvent(e: AppEvent) {  
  switch (e.type) {    
    case "download":      
      e; // Type is DownloadEvent       
      break;    
    case "upload":      
      e; // Type is UploadEvent       
      break;  
  }
}
```

这种模式也被称为 ”标签联合“ 或 ”可辨识联合“，它在 TypeScript 中的应用范围非常广。

如果 TypeScript 不能识别出类型，你甚至可以引入一个自定义函数来帮助它：

```ts
function isInputElement(el: HTMLElement): el is HTMLInputElement {  
  return "value" in el;
}
function getElementContent(el: HTMLElement) {  
  if (isInputElement(el)) {    
    // Type is HTMLInputElement    
    return el.value;  
  }  
  // Type is HTMLElement  
  return el.textContent;
}
```

这就是所谓的 “用户定义类型保护”。`el is HTMLInputElement`，作为返回类型告诉类型检查器，如果函数返回true，则 el 变量的类型就是 HTMLInputElement。

在 TypeScript 中我们可以利用类型收窄和 never 类型的特性来全面性检查，比如：

```ts
type Foo = string | number;
function a(foo: Foo) {  
    if(typeof foo === "string") {    
    // 这里 foo 被收窄为 string 类型  
  } else if(typeof foo === "number") {    
    // 这里 foo 被收窄为 number 类型  
  } else {    
    // foo 在这里是 never    
    const check: never = foo;  
  }
}
```

注意在 else 分支里面，我们把收窄为 never 的 foo 赋值给一个显式声明的 never 变量。如果一切逻辑正确，那么这里应该能够编译通过。但是假如后来有一天你的同事修改了 Foo 的类型：

```ts
type Foo = string | number | boolean;
```

然而他忘记同时修改 `a` 方法中的控制流程，这时候 else 分支的 foo 类型会被收窄为 `boolean` 类型，导致无法赋值给 never 类型，这时就会产生一个编译错误。通过`never`这个方式，在编译阶段我们可以确保`a` 方法中的分支判断是否对 Foo 的所有类型有遗漏：

通过这个示例，我们可以得出一个结论：**`never`类型避免出现由于联合类型新增了类型但没有对应的实现而出现的错误，目的是写出类型绝对安全的代码。 **

#### 条件类型

如果 T 是 U 的子类，返回 X 类型，如果不是返回 Y 类型

```tsx
T extends U ? X : Y;
```

#### 操作符&运算符

##### typeof

在JavaScript中，`typeof`可以判断出'string','number','boolean','undefined','symbol'，但判断 typeof(null) 时值为 'object'; 判断数组和对象时值均为 'object'。

在 TypeScript 中，`typeof` 操作符可以用来获取一个变量声明或对象的类型。

```ts
interface Person {
  name: string;
  age: number;
}

const sem: Person = { name: 'semlinker', age: 30 };
type Sem= typeof sem; // -> Person

function toArray(x: number): Array<number> {
  return [x];
}

type Func = typeof toArray; // -> (x: number) => number[]
```

一般我们都是先定义类型，再去赋值使用，但是使用 typeof 我们可以把使用顺序倒过来。

```ts
const options = {
  a: 1
}
type Options = typeof options // 等于 type Options = { a: number }
```

##### keyof

`keyof` 与 `Object.keys` 略有相似，只不过 `keyof` 取 **类型** 的键。

TypeScript 通过 keyof 操作符遍历某种类型的属性，并提取其属性的名称。

如果**T**是一个类型，那么**keyof T**产生的类型是**T**里面的属性名称字符串字面量类型构成的联合类型。



在 TypeScript 中的索引签名支持 string、number、symbol 这三种类型的。

那我知道了，要 K extends string | number | symbol 。

不不不，TypeScript 有个编译选项叫做 keyofStringsOnly，开启了那么就就只会用 string 作为索引，否则才是 string ｜ number | symbol：

```ts
// 不开启 keyofStringsOnly 时：
type res = k extends any // string | number | symbol
// 开启 keyofStringsOnly 时：
type res = k extends any // string
// 因此根据keyofStringsOnly对属性类型的需求，用 keyof any 动态获取比写死 string | number | symbol 更好。
```

```ts
// 接口
interface Person {
  name: string;
  age: number;
  location: string;
}

type K1 = keyof Person; // "name" | "age" | "location"
// 索引是number类型
type K2 = keyof Person[];  // number | "length" | "push" | "concat" | ...
type K3 = keyof { [x: string]: Person };  // string | number
type K4 = keyof { [x: number]: Person }; // number

// 类
class Person1 {
  name: string = "Semlinker";
}

let sname: keyof Person1;
sname = "name";

// 基本数据类型
let K1: keyof boolean; // let K1: "valueOf"
let K2: keyof number; // let K2: "toString" | "toFixed" | "toExponential" | ...
let K3: keyof symbol; // let K1: "valueOf"
```

```ts
// 字符串索引 -> keyof StringArray => string | number
type K3 = keyof { [x: string]: Person };  // string | number
// 数字索引 -> keyof StringArray1 => number
type K4 = keyof { [x: number]: Person }; // number
```

JavaScript 在执行索引操作时，会先把数值索引先转换为字符串索引，为了同时支持两种索引类型，规定索引参数类型必须为 "string" 或 "number"。所以 `keyof { [x: string]: Person }` 的结果会返回 `string | number`。

##### in

`in` 用来遍历枚举类型：

in 后跟的是键的联合类型（可keyof用生成）

```ts
type Keys = "a" | "b" | "c"

type Obj =  {
  [p in Keys]: any
} // -> { a: any, b: any, c: any }
```

##### -

去掉已有的修饰的，用 - 号，减去的意思：

比如 `-?` ， `-readonly`

##### infer

在条件类型语句中，可以用 `infer` 推断一个类型变量并且对它进行使用。

```ts
type ReturnType<T> = T extends (
  ...args: any[]
) => infer R ? R : any;
```

以上代码中 `infer R` 就是声明一个变量来承载传入函数签名的返回值类型，简单说就是用它取到函数返回值的类型方便之后使用。

**需要注意的是，infer 只能在条件类型的 extends 子句中使用，同时 infer 声明的类型变量只在条件类型的 true 分支中可用。**

```ts
type Wrong1<T extends (infer U)[]> = T[0] // Error 仅条件类型的extends子句中才允许infer声明
type Wrong2<T> = (infer U)[] extends T ? U : T // Error 仅条件类型的extends子句中才允许infer声明
type Wrong3<T> = T extends (infer U)[] ? T : U // Error 找不到名称U
```



**在协变位置上，若同一个类型变量存在多个候选者，则最终的类型将被推断为联合类型**

```ts
type PropertyType<T> = T extends { id: infer U; name: infer U } ? U : T;

type User = {
  id: number;
  name: string;
};

type U1 = PropertyType<User>; // string | number
```

**在逆变位置上，若同一个类型变量存在多个候选者，则最终的类型将被推断为交叉类型**

```ts
type Bar<T> = T extends { a: (x: infer U) => void, b: (x: infer U) => void } ? U : never;

type U5 = Bar<{ a: (x: string) => void, b: (x: number) => void }>;  // string & number, 即never
```



##### extends

有时候我们定义的泛型不想过于灵活或者说想继承某些类等，可以通过 extends 关键字添加泛型约束。

**若A  extends B ，则 A能表示的类型更窄，B能表示的类型更广**

```ts
type A = 'name' | 'age'
type B = 'name' | 'age' | 'test'
type C = A extends B ? A : never
// C:'name'|'age'
```

```ts
type A = 'name' | 'age' | 'test' | 'a'
type B = 'name' | 'age' | 'test'
type C = A extends B ? A : never 
// C:never
```

```ts
interface A {
  name: string
  age: number
  a: string
}
interface B {
  name: string
  age: number
}
type C = A extends B ? A : never
// C:A
```

```ts
interface A {
  name: string
  age: number
}
interface B {
  name: string
  age: number
  b: string
}
type C = A extends B ? A : never
// C:never
```

```ts
interface ILengthwise {
  length: number;
}

function loggingIdentity<T extends ILengthwise>(arg: T): T {
  console.log(arg.length);
  return arg;
}
```

现在这个泛型函数被定义了约束，因此它不再是适用于任意类型：

```ts
loggingIdentity(3);  // Error, number doesn't have a .length property
```

这时我们需要传入符合约束类型的值，必须包含必须的属性：

```ts
loggingIdentity({length: 10, value: 3});
```

##### ?: 可选属性

```ts
interface Person {
  name:string;
  age?:number; // 可选属性
}

function a(age?:number){
  // ...
}
```

##### ?. 运算符

TypeScript 3.7 实现了呼声最高的 ECMAScript 功能之一：可选链（Optional Chaining）。有了可选链后，我们编写代码时如果遇到 `null` 或 `undefined` 就可以立即停止某些表达式的运行。可选链的核心是新的 `?.` 运算符，它支持以下语法：

> ```
> obj?.prop
> obj?.[expr]
> arr?.[index]
> func?.(args)
> ```

这里我们来举一个可选的属性访问的例子：

```ts
const val = a?.b;
```

为了更好的理解可选链，我们来看一下该 `const val = a?.b` 语句编译生成的 ES5 代码：

```js
var val = a === null || a === void 0 ? void 0 : a.b;
```

上述的代码会自动检查对象 a 是否为 `null` 或 `undefined`，如果是的话就立即返回 `undefined`，这样就可以立即停止某些表达式的运行。你可能已经想到可以使用 `?.` 来替代很多使用 `&&` 执行空检查的代码：

```ts
if(a && a.b) { } 

if(a?.b){ }
/**
* if(a?.b){ } 编译后的ES5代码
* 
* if(
*  a === null || a === void 0 
*  ? void 0 : a.b) {
* }
*/
```

但需要注意的是，`?.` 与 `&&` 运算符行为略有不同，`&&` 专门用于检测 `falsy` 值，比如空字符串、0、NaN、null 和 false 等。而 `?.` 只会验证对象是否为 `null` 或 `undefined`，对于 0 或空字符串来说，并不会出现 “短路”。

##### ?? 空值合并运算符

在 TypeScript 3.7 版本中除了引入了前面介绍的可选链 `?.` 之外，也引入了一个新的逻辑运算符 —— 空值合并运算符 `??`。**当左侧操作数为 null 或 undefined 时，其返回右侧的操作数，否则返回左侧的操作数**。

与逻辑或 `||` 运算符不同，逻辑或会在左操作数为 falsy 值时返回右侧操作数。

```ts
 a ?? false // 可以用来排除a 是 null 或 undefined
```

##### ! 非空断言操作符

在上下文中当类型检查器无法断定类型时，一个新的后缀表达式操作符 `!` 可以用于断言操作对象是非 null 和非undefined 类型。**具体而言，`x!` 将从 x 值域中排除 `null` 和 `undefined` 。**

> 需要注意的是，非空断言操作符仅在启用 `strictNullChecks` 标志的时候才生效。当关闭该标志时，编译器不会检查 undefined 类型和 null 类型的赋值。

##### is 关键字

把参数的范围缩小化，如果返回true，则代表name是string类型

```ts
function a(name: any): name is string { 
   return typeof name === 'string'
}
```

#### TS内置工具

##### `Partial<T>`

`Partial<T>` 快速把某个接口类型中定义的属性变成可选的，实现原理：

```ts
type Partial<T> = {
  [P in keyof T]?: T[P];
};
```

##### `Required<T>`

`Required<T>`来快速把某个接口类型中定义的属性变成必选的，实现原理：

```ts
type Required<T> = {
  [P in keyof T]-?: T[P];
};
```

通过 `-?` 移除了可选属性中的 `?`，使得属性从可选变为必选的。

##### `Readonly<T>`

`Readonly<T>`将所有属性标记为只读，实现原理：

```ts
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};
```

##### `Pick<T, K>`

从T类型t挑选出K属性，实现原理：

```ts
// 利用 extends 关键字约束 K 类型必须为 keyof T 联合类型的子类型
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};
```

比如上面的那个 Person 属性，我们想把 name 和 age 挑出来：

```ts
interface Person {
  name: string,
  age: number,
  sex: string,
}
let person: Pick<Person, 'name' | 'age'> = {
  name: '小王',
  age: 21,
}
```

##### `Record<K, T>`

```ts
type Record<K extends keyof any, T> = {
    [P in K]: T;
};
```

`Record<string, any>` 创建了一个 key 为任意 string，value 为任意类型的索引类型

设置对象的属性（key）或者属性值（value）的类型

```ts
// 设置key和value的类型
const userMap = Record<number, Person> = {
  1: {
    name: '张三',
    gender: 0,
    age: 12;
  }
};

// 设置value的类型
const info = Record<'name' | 'id', string> = {
  name: '张三',
  id: '1',
}
```

##### `Exclude<T, U>`

从 T 中移除 U 中的属性，实现原理：

```ts
// T extends U ? never : 如果T是U的子类，返回never类型，如果不是返回T类型
type Exclude<T, U> = T extends U ? never : T;
```

```ts
type T = Exclude<1|2|3|4|5, 3|4>; // T = 1|2|5
```

##### Omit<T, K>

从 T 中排除 key 是 K 的属性，实现原理：

```ts
type Omit<T, K> = Pick<T, Exclude<keyof T, K>>
```

排除 **name** 、**sex** 属性：

```ts
interface Person {
  name: string,
  age: number,
  sex: string,
}

let person: Omit<Person, 'name'|'sex'> = {
  age: 1,
}
```

##### Extract<T, U>

从 T 中提取那些可以赋值给 U 的属性，实现原理：

```ts
type Extract<T, U> = T extends U ? T : never;
```

```ts
type T = Extract<1|2|3|4|5, 3|4>; // T = 3|4
```

##### `NonNullable<T>`

排除 T 为 null 、undefined，实现原理：

```ts
type NonNullable<T> = T extends null | undefined ? never : T;
```

```ts
type T = NonNullable<string | string[] | null | undefined>;
```

##### `ReturnType<T>`

获取函数 T 返回值的类型，实现原理：

```ts
type ReturnType<T extends (...args: any[]) => any> = T extends (...args: any[]) => infer R ? R : any;
```

```ts
function a():number{
    return 1
}
type b = ReturnType<typeof a>; // number
```

**infer R 相当于声明一个变量，接收传入函数的返回值类型。**

```ts
type T1 = ReturnType<() => string>; // string 

type T2 = ReturnType<(s: string) => void>; // void
```

##### `InstanceType<T>`

获取构造函数类型 T 的实例类型，实现原理：

```ts
type InstanceType<T extends new (...args: any) => any> = T extends new (...args: any) => infer R ? R : any;
```

```ts
class C {
  x = 0;
  y = 0;
}

type T0 = InstanceType<typeof C>; // C
```

##### `ThisType<T>`

用于指定上下文对象的类型，实现原理：

```ts
interface ThisType<T> { }
```

注意：使用 `ThisType<T>` 时，必须确保 `--noImplicitThis` 标志设置为 true。

```ts
interface Person {
    name: string;
    age: number;
}

const obj: ThisType<Person> = {
  dosth() {
    this.name // string
  }
}
```

##### `Parameters<T>`

用于获得函数的参数类型组成的元组类型，实现原理：

```ts
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;
```

```ts
function a (){}

type A = Parameters<() => void>; // []
// or
type A = Parameters<typeof a>; // []
```

##### `ConstructorParameters<T>`

提取构造函数类型的所有参数类型。它会生成具有所有参数类型的元组类型（如果 T 不是函数，则返回的是 never 类型），实现原理：

```ts
type ConstructorParameters<T extends new (...args: any) => any> = T extends new (...args: infer P) => any ? P : never;
```

```ts
type A = ConstructorParameters<FunctionConstructor>; // string[]
```



##### ThisParameterType<T>

用于提取 函数/方法参数中 this 的类型

```ts
type ThisParameterType<T> = T extends (this: infer U, ...args: any[]) => any ? U : unknown
```

```ts
class Dong {
  name: string;

  constructor() {
      this.name = "dong";
  }

  hello(this: Dong) {
      return 'hello, I\'m ' + this.name;
  }
}
const dong = new Dong()
type res = ThisParameterType<typeof dong.hello> // Dong
```



#### Mixins

### 工具

#### 编译工具

##### ts-node、tsconfig-paths

前提是已经安装好`TypeScript`、`node`

```sh
npm install ts-node -g
npm install nodemon -g
```

命令行输入 

```sh
ts-node index.ts
```

编译ts文件并执行编译后的js文件

命令行输入

```sh
nodemon -x ts-node --files index.ts
// or
nodemon --exec ts-node --files index.ts 
// 语义：nodemon 执行 ts-node index.ts 命令后，监控编译后的js文件
```

编译ts文件并执行编译后的js文件且热更新，ts文件改变后不需重新输入命令便可自动编译和执行。

exec： execute （执行）

###### ts-node cli

```sh
-p,--project #指定tsconfig.json，默认是最近的tsconfig.json
--files      #Load files, include and exclude from tsconfig.json on startup
-r,--require #Require a node module before execution 在执行之前引入指定的模块
```

这些选项都可以在 `tsconfig.json`中定义

```json
{
  // This is an alias to @tsconfig/node12: https://github.com/tsconfig/bases
  "extends": "ts-node/node12/tsconfig.json",

  // Most ts-node options can be specified here using their programmatic names.
  "ts-node": {
    // It is faster to skip typechecking.
    // Remove if you want ts-node to do typechecking.
    "transpileOnly": true,

    "files": true,

    "compilerOptions": {
      // compilerOptions specified here will override those declared below,
      // but *only* in ts-node.  Useful if you want ts-node and tsc to use
      // different options with a single tsconfig.json.
    }
  },
  "compilerOptions": {
    // typescript options here
  }
}
```

###### tsconfig-paths

tsconfig-paths —— 解析**tsconfig.json**中**paths**定义的模块

https://github.com/TypeStrong/ts-node

`ts-node`默认是不会修改自身解析node module的行为，如果要解析**tsconfig.json**中**paths**定义的模块，需要 **tsconfig-paths**这个库去实现。

You can use ts-node together with [tsconfig-paths](https://www.npmjs.com/package/tsconfig-paths) to load modules according to the `paths` section in `tsconfig.json`.

**方式一：**

```json
{
  "ts-node": {
    // Do not forget to `npm i -D tsconfig-paths`
    "require": ["tsconfig-paths/register"]
  }
}
```

**方式二：**

```sh
ts-node -r tsconfig-paths/register main.ts
```

使用 `process.env.TS_NODE_PROJECT` 可以指定 tsconfig.json。



#### [编译选项](https://www.tslang.cn/docs/handbook/compiler-options.html)

tsconfig.josn需要放在项目的根目录。

编译选项也可以在命令行输入，例如

```shell
tsc index.ts --allowJs
```

##### 配置说明

```json
{
  "compilerOptions": {
    /* Basic Options */
    "target": "es5" /* target用于指定编译后js文件里的语法应该遵循哪个JavaScript的版本的版本目标: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019' or 'ESNEXT'. */,
    "module": "commonjs" /* 用来指定编译后的js要使用的模块标准: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', or 'ESNext'. */,
    "lib": ["dom", "dom.iterable", "esnext"] /* 编译过程中需要引入的库文件的列表 */,
    "allowJs": true /* allowJs设置的值为true或false，用来指定是否允许编译js文件，默认是false，即不编译js文件 */,
    "checkJs": true /* checkJs的值为true或false，用来指定是否检查和报告js文件中的错误，默认是false */,
    "jsx": "preserve" /* 指定jsx代码用于的开发环境: 'preserve', 'react-native', or 'react'. */,
    "declaration": true /* declaration的值为true或false，用来指定是否在编译的时候生成相应的".d.ts"声明文件。如果设为true，编译每个ts文件之后会生成一个js文件和一个声明文件。 */,
    "declarationDir": "" /* 生成声明文件的输出路径。 */,
    "emitDeclarationOnly": true /* 默认为false，只生成.d.ts文件，不生成js文件，不能和 noEmit 同时设置为true */,
    "noEmit": true /* 不输出编译文件(js)，为false时，allowJs不能设置为true，否则当项目有js文件时会报错 */,
    "declarationMap": true /* 值为true或false，指定是否为声明文件.d.ts生成map文件 */,
    "sourceMap": true /* sourceMap的值为true或false，用来指定编译时是否生成.map文件 */,
    "outFile": "./" /* outFile用于指定将输出文件合并为一个文件，它的值为一个文件路径名。比如设置为"./dist/main.js"，则输出的文件为一个main.js文件。但是要注意，只有设置module的值为amd和system模块时才支持这个配置 */,
    "outDir": "./" /* outDir用来指定输出文件夹，值为一个文件夹路径字符串，输出的文件都将放置在这个文件夹 */,
    "rootDir": "./" /* 用来指定编译文件的根目录，编译器会在根目录查找入口文件，如果编译器发现以rootDir的值作为根目录查找入口文件并不会把所有文件加载进去的话会报错，但是不会停止编译 */,
    "composite": true /* 是否编译构建引用项目  */,
    "incremental": true /* Enable incremental compilation */,
    "tsBuildInfoFile": "./" /* Specify file to store incremental compilation information */,
    "removeComments": true /* removeComments的值为true或false，用于指定是否将编译后的文件中的注释删掉，设为true的话即删掉注释，默认为false */,
    "importHelpers": true /* importHelpers的值为true或false，指定是否引入tslib里的辅助工具函数，默认为false */,
    "downlevelIteration": true /* 当target为'ES5' or 'ES3'时，为'for-of', spread, and destructuring'中的迭代器提供完全支持 */,
    "isolatedModules": true /* isolatedModules的值为true或false，指定是否将每个文件作为单独的模块，默认为true，它不可以和declaration同时设定 */,
    "skipLibCheck": true /* 忽略所有的声明文件（ *.d.ts）的类型检查 */,

    /* Strict Type-Checking Options */
    "strict": true /* strict的值为true或false，用于指定是否启动所有类型检查，如果设为true则会同时开启下面这几个严格类型检查，默认为false */,
    "noImplicitAny": true /* noImplicitAny的值为false时，如果我们没有为一些值设置明确的类型，编译器会默认认为这个值为any，如果noImplicitAny的值为true的话。则没有明确的类型会报错。默认值为false */,
    "strictNullChecks": true /* strictNullChecks为true时，null和undefined值不能赋给非这两种类型的值，别的类型也不能赋给他们，除了any类型。还有个例外就是undefined可以赋值给void类型 */,
    "strictFunctionTypes": true /* 设置为 true 支持函数参数的逆变，设置为 false 则是双向协变 */,
    "strictBindCallApply": true /* 设为true后会对bind、call和apply绑定的方法的参数的检测是严格检测的 */,
    "strictPropertyInitialization": true /* 设为true后会检查类的非undefined属性是否已经在构造函数里初始化，如果要开启这项，需要同时开启strictNullChecks，默认为false */,
    "noImplicitThis": true /* 当this表达式的值为any类型的时候，生成一个错误 */,
    "alwaysStrict": true /* alwaysStrict的值为true或false，指定始终以严格模式检查每个模块，并且在编译之后的js文件中加入"use strict"字符串，用来告诉浏览器该js为严格模式 */,
    "forceConsistentCasingInFileNames": false /* 禁止对同一个文件的不一致的引用 */,

    /* Additional Checks */
    "noUnusedLocals": true /* 用于检查是否有定义了但是没有使用的变量，对于这一点的检测，使用eslint可以在你书写代码的时候做提示，你可以配合使用。它的默认值为false */,
    "noUnusedParameters": true /* 用于检查是否有在函数体中没有使用的参数，这个也可以配合eslint来做检查，默认为false */,
    "noImplicitReturns": true /* 用于检查函数是否有返回值，设为true后，如果函数没有返回值则会提示，默认为false */,
    "noFallthroughCasesInSwitch": true /* 用于检查switch中是否有case没有使用break跳出switch，默认为false */,
    "keyofStringsOnly": false /* 开启了那么就就只会用 string 作为索引，否则才是 string ｜ number | symbol */,

    /* Module Resolution Options */
    "moduleResolution": "node" /* 用于选择模块解析策略，有'node'和'classic'两种类型' */,
    "resolveJsonModule": true /* 允许引入json */,
    "baseUrl": "./" /* baseUrl用于设置解析非相对模块名称的基本目录，相对模块不会受baseUrl的影响 */,
    "paths": {} /* 用于设置模块名称到基于baseUrl的路径映射 */,
    "rootDirs": [] /* rootDirs可以指定一个路径列表，在构建时编译器会将这个路径列表中的路径的内容都放到一个文件夹中 */,
    "typeRoots": [] /* typeRoots用来指定声明文件或文件夹的路径列表，如果指定了此项，则只有在这里列出的声明文件才会被加载。在默认情况下，所有 node_modules/@types 中的任何包都被认为是可见的。如果手动指定了 typeRoots ，则仅会从指定的目录里查找类型文件。 */,
    "types": [] /* 指定要包括但未在源文件中引用的类型包名称，即自动引入声明文件。 */,
    "allowSyntheticDefaultImports": true /* 用来指定允许从没有默认导出的模块中默认导入 */,
    "esModuleInterop": true /* 通过为导入内容创建命名空间，实现CommonJS和ES模块之间的互操作性，esModuleInterop选项的作用是支持使用import d from 'cjs'的方式引入commonjs包。 */,
    "preserveSymlinks": true /* 不把符号链接解析为其真实路径，具体可以了解下webpack和nodejs的symlink相关知识 */,

    /* Source Map Options */
    "sourceRoot": "" /* sourceRoot用于指定调试器应该找到TypeScript文件而不是源文件位置，这个值会被写进.map文件里 */,
    "mapRoot": "" /* mapRoot用于指定调试器找到映射文件而非生成文件的位置，指定map文件的根路径，该选项会影响.map文件中的sources属性 */,
    "inlineSourceMap": true /* 指定是否将map文件的内容和js文件编译在同一个js文件中，如果设为true，则map的内容会以//# sourceMappingURL=然后拼接base64字符串的形式插入在js文件底部 */,
    "inlineSources": true /* 用于指定是否进一步将.ts文件的内容也包含到输入文件中 */,

    /* Experimental Options */
    "experimentalDecorators": true /* 用于指定是否启用实验性的装饰器特性 */,
    "emitDecoratorMetadata": true /* 用于指定是否为装饰器提供元数据支持，关于元数据，也是ES6的新标准，可以通过Reflect提供的静态方法获取元数据，如果需要使用Reflect的一些方法，需要引入ES2015.Reflect这个库 */,

    /* format */
    "pretty": true /* 默认true，颜色、格式化输出  */
  },
  "files": [], // files可以配置一个数组列表，里面包含指定文件的相对或绝对路径，编译器在编译的时候只会编译包含在files中列出的文件，如果不指定，则取决于有没有设置include选项，如果没有include选项，则默认会编译根目录以及所有子目录中的文件。这里列出的路径必须是指定文件，而不是某个文件夹，而且不能使用* ? **/ 等通配符
  "include": [], // include也可以指定要编译的路径列表，但是和files的区别在于，这里的路径可以是文件夹，也可以是文件，可以使用相对和绝对路径，而且可以使用通配符，比如"./src"即表示要编译src文件夹下的所有文件以及子文件夹的文件
  "exclude": [], // exclude表示要排除的、不编译的文件，它也可以指定一个列表，规则和include一样，可以是文件或文件夹，可以是相对路径或绝对路径，可以使用通配符
  "extends": "", // extends属性指定一个其他的tsconfig.json文件路径，来继承这个配置文件里的配置，继承来的文件的配置和当前文件属性重复时会覆盖当前文件定义的配置。（因为本文件中的配置最先被加载，之后被extends的文件覆盖同名配置，extends的文件的相对路径是根据其所在的路径，与本文件的路径无关。）如果发现循环引用，则会报错。TS在3.2版本开始，支持继承一个来自Node.js包的tsconfig.json配置文件
  "compileOnSave": true, // compileOnSave的值是true或false，如果设为true，在我们编辑了项目中的文件保存的时候，编辑器会根据tsconfig.json中的配置重新生成文件，不过这个要编辑器支持
  "references": [] // 一个对象数组，指定要引用的项目
}
```

##### 常用选项

```json
{
  "compilerOptions": {
    "module": "esnext",
    "target": "es5",
    "allowSyntheticDefaultImports": true, /* 用来指定允许从没有默认导出的模块中默认导入 */
    "jsx": "react",
    "lib": ["dom", "dom.iterable", "esnext"], /* 编译过程中需要引入的库文件的列表 */
    "moduleResolution": "node",
    "experimentalDecorators": true, /* 用于指定是否启用实验性的装饰器特性 */
    "downlevelIteration": true, /* 当target为'ES5' or 'ES3'时，为'for-of', spread, and destructuring'中的迭代器提供完全支持 */
    "allowJs": true,
    "esModuleInterop": true, /* 通过为导入内容创建命名空间，实现CommonJS和ES模块之间的互操作性，esModuleInterop选项的作用是支持使用import d from 'cjs'的方式引入commonjs包 */
    "resolveJsonModule": true, // 允许引入json
    "forceConsistentCasingInFileNames": true, /*     禁止对同一个文件的不一致的引用。 */
    "noEmit": true, /* 不输出编译文件（js） */
    "noFallthroughCasesInSwitch": true, /* 用于检查switch中是否有case没有使用break跳出switch，默认为false */
    "noImplicitAny": false,   /* noImplicitAny的值为false时，如果我们没有为一些值设置明确的类型，编译器会默认认为这个值为any，如果noImplicitAny的值为true的话。则没有明确的类型会报错。默认值为false */
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"],
      "@pages/*": ["./src/pages/*"],
      "@components/*": ["./src/components/*"],
      "@img/*": ["./src/assets/img/*"],
      "@css/*": ["./src/assets/css/*"],
      "@utils/*": ["./src/utils/*"],
      "@hooks/*": ["./src/hooks/*"]
    }
  },
  "include": ["src"],
  "exclude": ["node_modules", "build"],
  "ts-node": { // 需安装 ts-node
    "require": ["tsconfig-paths/register"], // 需安装 tsconfig-paths
    "compilerOptions": {
      "module": "commonjs"
    }
  }
}
```

**allowJs**

> 允许编译 js 文件。

设置为 `true` 时，js 文件会被 tsc 编译，否则不会。一般在项目中 js, ts 混合开发时需要设置。

[查看示例](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/compiler-options/01-allowJs)

```bash
# 设置为 true 时，编译后的文件包含 foo.js
├── lib
│   ├── foo.js
│   └── index.js
├── src
│   ├── foo.js
│   └── index.ts
├── package.json
└── tsconfig.json
# 设置为 false 时，编译后的文件不包含 foo.js
├── lib
│   └── index.js
├── src
│   ├── foo.js
│   └── index.ts
├── package.json
└── tsconfig.json
```

**allowSyntheticDefaultImports**

> 允许对不包含默认导出的模块使用默认导入。这个选项不会影响生成的代码，只会影响类型检查。

`export = foo` 是 ts 为了兼容 commonjs 创造的语法，它对应于 commonjs 中的 `module.exports = foo`。

在 ts 中，如果要引入一个通过 `export = foo` 导出的模块，标准的语法是 `import foo = require('foo')`，或者 `import * as foo from 'foo'`。

但由于历史原因，我们已经习惯了使用 `import foo from 'foo'`。

这个选项就是为了解决这个问题。当它设置为 `true` 时，允许使用 `import foo from 'foo'` 来导入一个通过 `export = foo` 导出的模块。当它设置为 `false` 时，则不允许，会报错。

当然，我们一般不会在 ts 文件中使用 `export = foo` 来导出模块，而是在[写（符合 commonjs 规范的）第三方库的声明文件](https://ts.xcatliu.com/basics/declaration-files#export-1)时，才会用到 `export = foo` 来导出类型。

比如 React 的声明文件中，就是通过 `export = React` 来导出类型：

```ts
export = React;
export as namespace React;

declare namespace React {
    // 声明 React 的类型
}
```

此时若我们通过 `import React from 'react'` 来导入 react 则会报错，[查看示例](https://github.com/xcatliu/typescript-tutorial/tree/master/examples/compiler-options/02-allowSyntheticDefaultImports) ：

```ts
import React from 'react';
// Module '"typescript-tutorial/examples/compiler-options/02-allowSyntheticDefaultImports/false/node_modules/@types/react/index"' can only be default-imported using the 'esModuleInterop' flagts(1259)
```

解决办法就是将 `allowSyntheticDefaultImports` 设置为 `true`。

**strictNullChecks**

空值检查：在严格的 `null`检查模式下， `null`和 `undefined`值不包含在任何类型里，只允许用自己和 `any`来赋值给 `null` 和 `undefined` 类型（有个例外， `undefined`可以赋值到 `void`）。 

```ts
interface User {
  name: string;
  age?: number;
}
function printUserInfo(user?: User|null) {
  console.log(`${user.name}, ${user.age.toString()}`)
  // => error TS2532: Object is possibly 'undefined'.
  console.log(`${user.name}, ${user.age!.toString()}`)
  // => OK, you confirm that you're sure user.age is non-null.
  // => 好的，你已经确认user.age是非空的。

  if (user.age != null) {
    console.log(`${user.name}, ${user.age.toString()}`)
  }
  // => OK, the if-condition checked that user.age is non-null.
    // => 好的，if条件检查了user.age是非空的。

  console.log(user.name + ', ' + user.age !== null ? user.age.toString() : 'age unknown');
  // => Unfortunately TypeScript can't infer that age is non-null here.
  // => 不幸的是TypeScript不能在这里推断年龄是非空的。
}
```

1. 感叹号表示你确信(例如，通过在代码中的某个地方执行检查)可能为空的变量实际上是非空的。
2. 如果执行`If`条件检查, TypeScript可以推断某些内容是非空的。
3. 然而，对于三元运算符来说，不幸的是情况并非如此。