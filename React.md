### React v17.0.1

#### 简介

**React** 是一个用于构建用户界面的 JavaScript 库。 

**原生JS**

1. 原生 JavaScript操作DOM繁琐、效率低(DOM-API操作UI)。
2. 使用 JavaScript直接操作DOM,浏览器会进行大量的重绘回流。
3. 原生 JavaScript设有组件化编码方案,代码复用率低。

**React特点**

1. 采用组件化模式、声明式编码,提高开发效率及组件复用率。
2. 在 React Native中可以使用React语法进行移动端开发。
3. 使用虚拟DOM+优秀的 Diffing算法,尽量減少与真实DOM的交互。

#### JSX
JSX（JavaScript XML）是一个 JavaScript 的语法扩展。

document有createElement()方法，React也有createElement()方法，下面就来介绍React的createElement()方法。

```js
var reactElement = ReactElement.createElement(
... // 标签名称字符串/组件,
... // [元素的属性值对对象],
... // [元素的子节点]
)
```

1、参数：
- 第一个参数：可以是一个html标签名称字符串，也可以是一个组件（必须）
- 第二个参数：包含元素属性值对的对象（可选），这些属性可以通过this.props.\*来调用
- 第三个参数开始：元素的子节点（可选）

2、返回值：一个给定类型的ReactElement元素



**为什么使用JSX**

React 并没有采用将视图与逻辑进行分离到不同文件的方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现关注点分离。

**定义组件时，最外层必须只有一个根元素**

 原因：组件中的标签最终会被转换成React.createElement来进行创建，最外层没有一个唯一的标签进行包裹，在转换时没有办法解析，因为不可能同时创建多个并列的组件同时返回。 

**在JSX中嵌入表达式**

```jsx
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```
在 JSX 语法中，你可以在大括号内放置任何有效的 JavaScript 表达式。例如，2 + 2，user.firstName 或 formatName(user) ，三元运算符都是有效的 JavaScript 表达式。


为了便于阅读，我们会将 JSX 拆分为多行。同时，我们建议将内容包裹在括号中，虽然这样做不是强制要求的，但是这可以避免遇到自动插入分号陷阱。
```jsx
const element = (
  <h1>
    Hello, {name}
  </h1>
);
```
**JSX 也是一个表达式**

JSX本身也是一个表达式，在编译后，JSX表达式会变成普通的JavaScript对象。

你可以在 if 语句和 for 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：
例如下面的代码：
```jsx
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```
上面的代码在if语句中使用JSX，并将JSX作为函数返回值。实际上，这些JSX经过编译后都会变成JavaScript对象。经过babel会变成下面的js代码：
```jsx
function test(user) {
    if (user) {
        return React.createElement(
            "h1",
            null,
            "Hello, ",
            formatStr(user),
            "!"
        );
    }
    return React.createElement(
        "h1",
        null,
        "Hello, Stranger."
    );
}
```

**JSX 特定属性**

你可以通过使用引号，来将属性值指定为字符串字面量：
```jsx
const element = <div tabIndex="0"></div>;
```
也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：
```jsx
const element = <img src={user.avatarUrl}></img>;
```
在属性中嵌入 JavaScript 表达式时，不要在大括号外面加上引号。你应该仅使用引号（对于字符串值）或大括号（对于表达式）中的一个，对于同一属性不能同时使用这两种符号。

因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 camelCase（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。例如，JSX 里的 class 变成了 className，而 tabindex 则变为 tabIndex。

使用扩展运算符：
如果你想将一个prop对象传入JSX，你可以使用扩展运算符...直接将整个prop对象传入。
下面的2个组件是等价的：
```jsx
function App1() {
  return <Greeting firstName="Ben" lastName="Hector" />;
}

function App2() {
  const props = {firstName: 'Ben', lastName: 'Hector'};
  return <Greeting {...props} />;
}
```

JSX 防止注入攻击
你可以安全地在 JSX 当中插入用户输入内容：
```jsx
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```
React DOM 在渲染所有输入内容之前，默认会进行转义。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 XSS（cross-site-scripting, 跨站脚本）攻击。

**JSX 表示对象**

Babel 会把 JSX 转译成一个名为 React.createElement() 函数调用。以下两种示例代码完全等效：
```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
 );
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
 );
```
`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：
```jsx
// 注意：这是简化过的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```
这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新。

**JSX 注释**

```jsx
<div>
  {/* 注释写在这里 */}
  Hello, {name}!
</div>
```

```jsx
<div>
  {/* 多行注释 
  也同样有效。 */}
  Hello, {name}! 
</div>
```

#### 函数组件与class组件

定义组件最简单的方式就是编写 JavaScript 函数：
```jsx
// props是包含组件标签内属性的对象
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
// 组件标签
<Welcome text="hello" />
```
该函数是一个有效的 React 组件，因为它接收唯一带有数据的 “props”（代表属性）对象与并返回一个 React 元素。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。你同时还可以使用 ES6 的 class 来定义组件：
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```
上述两个组件在 React 里是等效的。

Class 组件应该始终使用 props 参数来调用父类的构造函数。
```js
constructor(props) {    
   super(props);    
   this.state = {};
}
```

**渲染组件**

之前，我们遇到的 React 元素都只是 DOM 标签：
```jsx
const element = <div />;
```
不过，React 元素也可以是用户自定义的组件：
```jsx
const element = <Welcome name="Sara" />;
```
当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）以及子组件（children）转换为单个对象传递给组件，这个对象被称之为 “props”。例如，这段代码会在页面上渲染 “Hello, Sara”：
```jsx
function Welcome(props) {  
    return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
**注意： 组件名称必须以大写字母开头。**
React 会将以小写字母开头的组件视为原生 DOM 标签。例如，<div /> 代表 HTML 的 div 标签，而 <Welcome /> 则代表一个组件，并且需在作用域内使用 Welcome。

**组合组件**

组件可以在其输出中引用其他组件。
例如，我们可以创建一个可以多次渲染 Welcome 组件的 App 组件：
```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>      
        <Welcome name="Sara" />      
        <Welcome name="Cahal" />      
        <Welcome name="Edite" />    
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

**Props 的只读性**

组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props。来看下这个 sum 函数：
```js
function sum(a, b) {
  return a + b;
}
```
这样的函数被称为“纯函数”，因为该函数不会尝试更改入参，且多次调用下相同的入参始终返回相同的结果。相反，下面这个函数则不是纯函数，因为它更改了自己的入参：
```js
function withdraw(account, amount) {
  account.total -= amount;
}
```
React 非常灵活，但它也有一个严格的规则：所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。

当然，应用程序的 UI 是动态的，并会伴随着时间的推移而变化。在下一章节中，我们将介绍一种新的概念，称之为 “state”。在不违反上述规则的情况下，state 允许 React 组件随用户操作、网络响应或者其他变化而动态更改输出内容。

props 只读不改，单向数据流

state 是私有的，并且完全受控于当前组件。

#### state
**正确使用state**

- **不能直接修改 state**
例如，此代码不会重新渲染组件：
```js
// Wrong
this.state.comment = 'Hello';
```
而是应该使用 setState()：

```js
// Correct
this.setState({comment: 'Hello'});
```
更多关于 setState() 请查看 **其他API** 章节。

**构造函数是唯一可以给 this.state 赋值的地方**

- **state 的更新可能是异步的**
出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用。因为 this.props 和 this.state 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。例如，此代码可能会无法更新计数器：
```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```
确保每次调用都是使用最新的 state，可以让 setState() 接收一个函数。这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数：
```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```
上面使用了箭头函数，不过使用普通的函数也同样可以：
```js
// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  }
});
```

- **State 的更新会被合并**
当你调用 setState() 的时候，React 会把你在不同方法内提供的对象（this.setState({ })）合并到当前的 state 。

例如，你的 state 包含几个独立的变量：  
```js
constructor(props) {
    super(props);
    this.state = {      
        posts: [],      
        comments: []    
    };
}
```
然后你可以分别调用 setState() 来单独地更新它们：  
```js
componentDidMount() {
    fetchPosts().then(response => {
      this.setState({       
      	posts: response.posts      
      });
    });

    fetchComments().then(response => {
      this.setState({        
      	comments: response.comments      
      });
    });
}
```
这里的合并是浅合并，所以 this.setState({comments}) 完整保留了 this.state.posts， 但是完全替换了 this.state.comments。

- **数据是单向向下流动的**
不管是父组件或是子组件都无法知道某个组件是有状态的还是无状态的，并且它们也并不关心它是函数组件还是 class 组件。

这因为 state 是局部私有的。除了拥有并设置了它的组件，其他组件都无法访问。

组件可以选择把它的 state 作为 props 向下传递到它的子组件中：
```jsx
class A extends React.Component {
  constructor(props){
     super(props);
     this.state = {date : new Date()}
  }
  render() {
    return <h2>It is {this.state.date.toLocaleTimeString()}.</h2>;
  }
}
```
这对于自定义组件同样适用：
```js
function B(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}

class A extends React.Component {
  constructor(props){
     super(props);
     this.state = {date : new Date()}
  }
  render() {
    return <B date={this.state.date} />;
  }
}
```
B 组件会在其 props 中接收参数 date，但是组件本身无法知道它是来自于 A 的 state，或是 A 的 props，还是手动输入的。

这通常会被叫做“自上而下”或是“单向”的数据流。任何的 state 总是所属于特定的组件，而且从该 state 派生的任何数据或 UI 只能影响树中“低于”它们的组件。如果你把一个以组件构成的树想象成一个 props 的数据瀑布的话，那么每一个组件的 state 就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动。

**确定 UI state 的最小（且完整）表示**

 为了正确地构建应用，你首先需要找出应用所需的 state 的最小表示，并根据需要计算出其他所有数据。只保留应用所需的可变 state 的最小集合，其他数据均由它们计算产生。

通过问自己以下三个问题，你可以逐个检查相应数据是否属于 state：

1. 该数据是否是由父组件通过 props 传递而来的？如果是，那它应该不是 state。
2. 该数据是否随时间的推移而保持不变？如果是，那它应该也不是 state。
3. 你能否根据其他 state 或 props 计算出该数据的值？如果是，那它也不是 state。

 **确定 state 放置的位置**

React 中的数据流是单向的，并顺着组件层级从上往下传递。 

对于应用中的每一个 state：

- 找到根据这个 state 进行渲染的所有组件。
- 找到他们的共同所有者（common owner）组件（在组件层级上高于所有需要该 state 的组件）。
- 该共同所有者组件或者比它层级更高的组件应该拥有该 state。
- 如果你找不到一个合适的位置来存放该 state，就可以直接创建一个新的组件来存放该 state，并将这一新组件置于高于共同所有者组件层级的位置。


#### 事件处理
- React 事件的命名采用小驼峰式（camelCase），而不是纯小写。
- 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。

例如，传统的 HTML：
```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```
在 React 中略微不同：
```jsx
// onClick注意大小写
<button onClick={activateLasers}>  
    Activate Lasers
</button>
```
在 React 中另一个不同点是你不能通过返回 false 的方式阻止默认行为。你必须显式的使用 preventDefault 。例如，传统的 HTML 中阻止链接默认打开一个新页面，你可以这样写：
```html
<a href="#" onclick="console.log('The link was clicked.'); return false">
  Click me
</a>
```
在 React 中，可能是这样的：
```jsx
function ActionLink() {  
    function handleClick(e) {    
        e.preventDefault();    
        console.log('The link was clicked.');  
    }
    return (    
      <a href="#" onClick={handleClick}>      
          Click me    
      </a>
    );
}
```
在这里，e 是一个合成事件。React 根据 W3C 规范来定义这些合成事件，所以你不需要担心跨浏览器的兼容性问题。如果想了解更多，请查看 SyntheticEvent 参考指南。

使用 React 时，你一般不需要使用 addEventListener 为已创建的 DOM 元素添加监听器。事实上，你只需要在该元素初始渲染的时候添加监听器即可。

当你使用 ES6 class 语法定义一个组件的时候，通常的做法是将事件处理函数声明为 class 中的方法。例如，下面的 Toggle 组件会渲染一个让用户切换开关状态的按钮：
```jsx
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 为了在回调中使用 `this`，这个绑定是必不可少的             
    this.handleClick = this.handleClick.bind(this);  }

  handleClick() {    
    this.setState(state => ({      
        isToggleOn: !state.isToggleOn    
    }));  
  }
  render() {
    return (      
    <button onClick={this.handleClick}>        
        {this.state.isToggleOn ? 'ON' : 'OFF'}      
    </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```
你必须谨慎对待 JSX 回调函数中的 this，在 JavaScript 中，class 的方法的 this 默认不会绑定绑定到实例，只是通常会被实例调用，指向实例。如果你忘记绑定 this.handleClick 并把它传入了 onClick，当你调用这个函数的时候 this 的值为 undefined。

这并不是 React 特有的行为；这其实与 [JavaScript 函数工作原理](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)有关。通常情况下，如果你没有在方法后面添加 `()`，例如 `onClick={this.handleClick}`，你应该为这个方法绑定 `this`。

如果觉得使用 `bind` 很麻烦，这里有两种方式可以解决。如果你正在使用实验性的 [public class fields 语法](https://babeljs.io/docs/plugins/transform-class-properties/)，你可以使用 class fields 正确的绑定回调函数：

```jsx
class LoggingButton extends React.Component {
  // 此语法确保 `handleClick` 内的 `this` 已被绑定。  
  // 注意: 这是 *实验性* 语法。  
  handleClick = () => {    
  	console.log('this is:', this);  
  }
  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

Create React App 默认启用此语法。

如果你没有使用 class fields 语法，你可以在回调中使用箭头函数：

```jsx
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // 此语法确保 `handleClick` 内的 `this` 已被绑定。    
    return (      
      <button onClick={(e) => this.handleClick(e)}>        
      	Click me
      </button>
    );
  }
}
```

此语法问题在于每次渲染 `LoggingButton` 时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题。

请注意，上面 public class fields 语法目前还处于**试验性阶段**，这意味着语法随时都可能改变，也存在最终不被列入框架标准的可能。

为了保险起见，以下三种做法都是可以的：

- 在 constructor 中绑定方法。
- 使用箭头函数，比如：`onClick={(e) => this.handleClick(e)}`。
- 继续使用 `createReactClass`，组件中的方法会自动绑定至实例（不使用 ES6 class）。

**向事件处理程序传递参数**

在循环中，通常我们会为事件处理函数传递额外的参数。例如，若 `id` 是你要删除那一行的 ID，以下两种方式都可以向事件处理函数传递参数：

```html
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

上述两种方式是等价的，分别通过 箭头函数 和 `Function.prototype.bind` 来实现。

在这两种情况下，React 的事件对象 `e` 会被作为参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过 `bind` 的方式，事件对象以及更多的参数将会被隐式的进行传递（第二种情况下，e 是最后一个参数—隐藏的）。

值得注意的是，通过 bind 方式向监听函数传参的情况下，在类组件中定义的事件监听函数，事件对象 e 要排在所传递参数的后面。 

如果事件监听函数只有事件对象e这个参数，在标签内绑定该函数时（非箭头函数形式），其后面可以不添加 `()` 或 `(e)`，但在组件中定义该函数时需要用e，依然需要显式在参数列表中写上e。

#### 条件渲染

 React 中的条件渲染和 JavaScript 中的一样，使用 JavaScript 运算符`if`或者 条件运算符 去创建元素来表现当前的状态，然后让 React 根据它们来更新 UI。 

**与运算符 &&**

通过花括号包裹代码，你可以在 JSX 中嵌入任何表达式。这也包括 JavaScript 中的逻辑与 (&&) 运算符。它可以很方便地进行元素的条件渲染。

```jsx
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
    		<h2>          
    			You have {unreadMessages.length} unread messages.        
    		</h2>
      }    
		</div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];
ReactDOM.render(
  <Mailbox unreadMessages={messages} />,
  document.getElementById('root')
);
```

之所以能这样做，是因为在 JavaScript 中，`true && expression` 总是会返回 `expression`, 而 `false && expression` 总是会返回 `false`。

因此，如果条件是 `true`，`&&` 右侧的元素就会被渲染，如果是 `false`，React 会忽略并跳过它。

**三目运算符**

另一种内联条件渲染的方法是使用 JavaScript 中的三目运算符`condition ? true : false` 。

在下面这个示例中，我们用它来条件渲染一小段文本

```jsx
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

**阻止组件渲染**

在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让 `render` 方法直接返回 `null`，而不进行任何渲染。

下面的示例中，` <WarningBanner /> ` 会根据 prop 中 `warn` 的值来进行条件渲染。如果 `warn` 的值是 `false`，那么组件则不会渲染:

```jsx
function WarningBanner(props) {
  // 函数组件本身就是 render 方法
  if (!props.warn) {
    return null;
  }
  return (
    <div className="warning">
      Warning!
    </div>
  );
}
```

```jsx
class WarningBanner extends React.Component {
  constructor(props){
    super(props);
    this.state = {warn: true};
  }

  render() {
    if (!this.state.warn) {
      return null;
    }
    return (      
      <div className="warning">
      	Warning!
    	</div>
    );
  }
}
```

 在组件的 `render` 方法中返回 `null` 并不会影响组件的生命周期。 

#### 列表 & Key

**渲染多个组件**

你可以通过使用 `{}` 在 JSX 内构建一个元素集合。

下面，我们使用 Javascript 中的 `map()` 方法来遍历 `numbers` 数组。将数组中的每个元素变成 `<li>` 标签，最后我们将得到的数组赋值给 `listItems`：

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>  <li>{number}</li>);
```

我们把整个 `listItems` 插入到 `<ul>` 元素中，然后渲染进 DOM：

```jsx
ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
```

**基础列表组件**

通常你需要在一个组件中渲染列表。

我们可以把前面的例子重构成一个组件，这个组件接收 `numbers` 数组作为参数并输出一个元素列表。

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
     <li>{number}</li>
  );  
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

当我们运行这段代码，将会看到一个警告 `a key should be provided for list items`，意思是当你创建一个元素时，必须包括一个特殊的 `key` 属性。我们将在下一节讨论这是为什么。

让我们来给每个列表元素分配一个 `key` 属性来解决上面的那个警告：

```jsx
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 需要添加key
    <li key={number.toString()}> 
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

**key**

key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。

当元素没有确定 id 的时候，万不得已你可以使用元素索引 index 作为 key：

```jsx
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs  
  <li key={index}>
    {todo.text}
  </li>
);
```

如果列表项目的顺序可能会变化，我们不建议使用索引来用作 key 值，因为这样做会导致性能变差，还可能引起组件状态的问题。可以看看 Robin Pokorny 的[深度解析使用索引作为 key 的负面影响](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)这一篇文章。如果你选择不指定显式的 key 值，那么 React 将默认使用索引用作为列表项目的 key 值。

要是你有兴趣了解更多的话，这里有一篇文章[深入解析为什么 key 是必须的](https://react.docschina.org/docs/reconciliation.html#recursing-on-children)可以参考。

元素的 key 只有放在就近的数组上下文中才有意义。

比方说，如果你提取出一个 `ListItem` 组件，你应该把 key 保留在数组中的这个 `<ListItem />` 元素上，而不是放在 `ListItem` 组件中的 `<li>` 元素上。

```jsx
function ListItem(props) {
  // 正确！这里不需要指定 key：  
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 正确！key 应该在就近数组的上下文中被指定    
    <ListItem key={number.toString()} value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

一个好的经验法则是：在 `map()` 方法中的元素需要设置 key 属性。

数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。然而，它们不需要是全局唯一的。 

key值是不可读的：key 会传递信息给 React ，但不会传递给你的组件。如果你的组件中需要使用 `key` 属性的值，请用其他属性名显式传递这个值 ，即 **key 不是 prop 属性**。

#### 表单

**受控组件**

在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 state（例如type、name、value属性），并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用` setState()`来更新。

我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

 例如，如果我们想让前一个示例在提交时打印出名称，我们可以将表单写为受控组件： 

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {    this.setState({value: event.target.value});  }
  handleSubmit(event) {
    alert('提交的名字: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          名字:
          <input type="text" value={this.state.value} onChange={this.handleChange} />        			</label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

**textarea 标签**

在 HTML 中, `<textarea>` 元素通过其子元素定义其文本:

```html
<textarea>
  你好， 这是在 text area 里的文本
</textarea>
```

而在 React 中，`<textarea>` 使用 `value` 属性代替。这样，可以使得使用 `<textarea>` 的表单和使用单行 input 的表单非常类似：

```jsx
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: '请撰写一篇关于你喜欢的 DOM 元素的文章.'};
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {    
    this.setState({value: event.target.value});
  }
  handleSubmit(event) {
    alert('提交的文章: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          文章:
          <textarea value={this.state.value} onChange={this.handleChange} />        						</label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

**select 标签**

在 HTML 中，`<select>` 创建下拉列表标签。例如，如下 HTML 创建了水果相关的下拉列表：

```jsx
<select>
  <option value="grapefruit">葡萄柚</option>
  <option value="lime">酸橙</option>
  <option selected value="coconut">椰子</option>
  <option value="mango">芒果</option>
</select>
```

请注意，由于 `selected` 属性的缘故，椰子选项默认被选中。React 并不会使用 `selected` 属性，而是在根 `select` 标签上使用 `value` 属性。这在受控组件中更便捷，因为您只需要在根标签中更新它。例如：

```jsx
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};
    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {    
    this.setState({value: event.target.value});  
  }
  handleSubmit(event) {
    alert('你喜欢的风味是: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          选择你喜欢的风味:
          <select value={this.state.value} onChange={this.handleChange}>            								<option value="grapefruit">葡萄柚</option>
            <option value="lime">酸橙</option>
            <option value="coconut">椰子</option>
            <option value="mango">芒果</option>
          </select>
        </label>
        <input type="submit" value="提交" />
      </form>
    );
  }
}
```

总的来说，这使得 `<input type="text">`, `<textarea>` 和 `<select>` 之类的标签都非常相似—它们都接受一个 `value` 属性，你可以使用它来实现受控组件。 

你可以将数组传递到 `value` 属性中，以支持在 `select` 标签中同时选择多个选项：

```html
<select multiple={true} value={['B', 'C']}>
```

**文件 input 标签**

在 HTML 中，`<input type="file">` 允许用户从存储设备中选择一个或多个文件，将其上传到服务器，或通过使用 JavaScript 的 [File API](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications) 进行控制。

```
<input type="file" />
```

因为它的 value 只读，所以它是 React 中的一个**非受控**组件。将与其他非受控组件在后续文档中一起讨论。

 在受控组件上指定 value 但没有指定可修改值的函数，这样会阻止用户更改输入。如果是在这种情况下，输入仍可编辑，则可能是你意外地将`value` 设置为 `undefined` 或 `null`。 

**受控组件的替代品**

有时使用受控组件会很麻烦，因为你需要为数据变化的每种方式都编写事件处理函数，并通过一个 React 组件传递所有的输入 state。当你将之前的代码库转换为 React 或将 React 应用程序与非 React 库集成时，这可能会令人厌烦。在这些情况下，你可能希望使用[非受控组件](https://react.docschina.org/docs/uncontrolled-components.html), 这是实现输入表单的另一种方式。

**成熟的解决方案**

如果你想寻找包含验证、追踪访问字段以及处理表单提交的完整解决方案，使用 [Formik](https://jaredpalmer.com/formik) 是不错的选择。然而，它也是建立在受控组件和管理 state 的基础之上 —— 所以不要忽视学习它们。

#### 状态提升 & 父子组件通信

 在 React 中，将多个组件中需要共享的 state 向上移动到它们的最近共同父组件中，便可实现共享 state。这就是所谓的“状态提升” 。

这时共享的 state 就由父组件单独控制，成为唯一的数据源，传递给子组件。子组件通过this.props.xxx 来获取父组件传递的 state。

父组件与子组件通信，与vue类似：

父传子：props

子传父：自定义事件——父组件将一个能够触发 setState 的回调函数传递给子组件，子组件通过事件监听调用该回调函数，然后该回调函数将调用 `setState()`，从而更新应用。  

```jsx
// 父组件
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.state = {temperature: ''};
  }
  
  handleChange(temperature) {
    this.setState({temperature});
  }
  
  render() {
    const temperature = this.state.temperature;
    // onTemperatureChange={this.handleChange} 是自定义事件，由子组件调用
    return (
      <div>
        <TemperatureInput
          temperature={temperature}
          onTemperatureChange={this.handleChange} />
      </div>
    );
  }
}
```

```jsx
// 子组件
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    // 子传父：调用父组件事件
    this.props.onTemperatureChange(e.target.value);
  }
  
  render() {
    // 父传子：props
    const temperature = this.props.temperature;
    return (
      <input value={temperature} onChange={this.handleChange} />
    );
  }
}
```

#### 组合

 在 React 组件中，[代码重用的主要方式是组合而不是继承](https://react.docschina.org/docs/composition-vs-inheritance.html)。

**包含关系 **

与vue的插槽类似。

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      <!-- 插槽 -->
      {props.children}
    </div>
  );
}
```

这使得别的组件可以通过 JSX 嵌套，将任意组件作为子组件传递给它们。

```jsx
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <!-- 填到插槽的内容-开始 -->
      <h1 className="Dialog-title">
      	Welcome      
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
      <!-- 填到插槽的内容-结束 -->
    </FancyBorder>
  );
}
```

 `<FancyBorder>` JSX 标签中的所有内容都会作为一个 `children` 属性传递给 `FancyBorder` 组件。因为 `FancyBorder` 将 `{props.children}` 渲染在一个 `<div>` 中，被传递的这些子组件最终都会出现在输出结果中。 

少数情况下，你可能需要在一个组件中预留出几个“洞”。这种情况下，我们可以不使用 `children`，而是自行约定：将所需内容传入 props，并使用相应的 prop。

```jsx
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

`<Contacts>` 和 `<Chat>` 之类的 React 元素本质就是对象（object），所以你可以把它们当作 props，像其他数据一样传递。这种方法可能使你想起别的库中“槽”（slot）的概念，但在 React 中没有“槽”这一概念的限制，你可以将任何东西作为 props 进行传递。 

**特例关系**

有些时候，我们会把一些组件看作是其他组件的特殊实例，比如 `WelcomeDialog` 可以说是 `Dialog` 的特殊实例。

在 React 中，我们也可以通过组合来实现这一点。“一般组件” 通过 props 保留位置渲染 “特殊组件” 的特定要素：

```jsx
// 一般组件
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

// 特殊组件
function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

如果你想要在组件间复用非 UI 的功能，我们建议将其提取为一个单独的 JavaScript 模块，如函数、对象或者类。组件可以直接引入（import）而无需通过 extend 继承它们。 

#### React API

`React` 是 React 库的入口。如果你通过使用 `<script>` 标签的方式来加载 React，则可以通过 `React` 全局变量对象来获得 React 的顶层 API。当你使用 ES6 与 npm 时，可以通过编写 `import React from 'react'` 来引入它们。当你使用 ES5 与 npm 时，则可以通过编写 `var React = require('react')` 来引入它们。

##### 组件

使用 React 组件可以将 UI 拆分为独立且复用的代码片段，每部分都可独立维护。你可以通过子类 `React.Component` 或 `React.PureComponent` 来定义 React 组件。

- [`React.Component`](https://react.docschina.org/docs/react-api.html#reactcomponent)
- [`React.PureComponent`](https://react.docschina.org/docs/react-api.html#reactpurecomponent)

如果你不使用 ES6 的 class，则可以使用 `create-react-class` 模块来替代。请参阅[不使用 ES6](https://react.docschina.org/docs/react-without-es6.html) 以获取更多详细信息。

React 组件也可以被定义为可被包装的函数：

- [`React.memo`](https://react.docschina.org/docs/react-api.html#reactmemo)

###### React.Component

`React.Component` 是使用 [ES6 classes](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 方式定义 React 组件的基类：

```jsx
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

`React.Component` 存在两个问题：

- 只要执行 setState( )，组件也会重新 render( )  
- 当前组件重新 render(），就会自动重新 render子组件，即使子组件没有用到父组件的任何数据=>效率低 

###### React.PureComponent

`React.PureComponent` 与 [`React.Component`](https://react.docschina.org/docs/react-api.html#reactcomponent) 很相似。无论组件是否是 `PureComponent`，如果定义了 `shouldComponentUpdate()`，那么会调用它并以它的执行结果来判断是否 update。在组件未定义 `shouldComponentUpdate()` 的情况下，会判断该组件是否是 `PureComponent`，如果是的话，会对新旧 props、state 进行浅层对比（对象第一层的属性及其值，即使其值是地址），一旦新旧不一致，会触发 update。大部分情况下，你可以使用 `React.PureComponent` 来代替手写 `shouldComponentUpdate`。

`React.PureComponent` 使用原则：

- props和state对象内的数据最好是值类型
- 如果是引用类型（对象、数组等），则不能再有更深层次的数据结构

如果赋予 React 组件相同的 props 和 state，`render()` 函数会渲染相同的内容，那么在某些情况下使用 `React.PureComponent` 可提高性能。需要确保组件渲染仅取决于 `props`和 `state`：

> 注意
>
> `React.PureComponent` 中的 `shouldComponentUpdate()` 仅作对象的浅层比较（对象第一层的属性及其值）。如果对象中包含复杂的数据结构，则有可能因为无法检查深层的差别，产生错误的比对结果。仅在你的 props 和 state 较为简单时（props和state对象内的数据最好不是对象），才使用 `React.PureComponent`，或者在深层数据结构发生变化时调用 [`forceUpdate()`](https://react.docschina.org/docs/react-component.html#forceupdate) 来确保组件被正确地更新。你也可以考虑使用 [immutable 对象](https://facebook.github.io/immutable-js/)加速嵌套数据的比较。
>
> 此外，`React.PureComponent` 中的 `shouldComponentUpdate()` 将跳过所有子组件树的 prop 更新。因此，请确保所有子组件也都是PureCopmponent。

###### React.memo

```jsx
const MyComponent = React.memo(function MyComponent(props) {
  /* 使用 props 渲染 */
});
```

`React.memo` 为[高阶组件](https://react.docschina.org/docs/higher-order-components.html)。

如果你的组件在相同 props 的情况下渲染相同的结果，那么你可以通过将其包装在 `React.memo` 中调用，以此通过记忆组件渲染结果的方式来提高组件的性能表现。这意味着在这种情况下，React 将跳过渲染组件的操作并直接复用最近一次渲染的结果。

`React.memo` 仅检查函数组件 props 变更。如果函数组件被 `React.memo` 包裹，且其实现中拥有 [`useState`](https://react.docschina.org/docs/hooks-state.html) 或 [`useContext`](https://react.docschina.org/docs/hooks-reference.html#usecontext) 的 Hook，当 context 发生变化时，它仍会重新渲染。与`React.PureComponent`的区别：`React.PureComponent` 浅层对比类组件的 props 和 state 。

默认情况下其只会对复杂对象做浅层对比，如果你想要控制对比过程，那么请将自定义的比较函数通过第二个参数传入来实现。

```jsx
function MyComponent(props) {
  /* 使用 props 渲染 */
}
function areEqual(prevProps, nextProps) {
  /*
  如果把 nextProps 传入 render 方法的返回结果与
  将 prevProps 传入 render 方法的返回结果一致则返回 true，
  否则返回 false
  */
}
export default React.memo(MyComponent, areEqual);
```

此方法仅作为**[性能优化](https://react.docschina.org/docs/optimizing-performance.html)**的方式而存在。但请不要依赖它来“阻止”渲染，因为这会产生 bug。

> 注意
>
> 与 class 组件中 [`shouldComponentUpdate()`](https://react.docschina.org/docs/react-component.html#shouldcomponentupdate) 方法不同的是，如果 props 相等，`areEqual` 会返回 `true`；如果 props 不相等，则返回 `false`。这与 `shouldComponentUpdate` 方法的返回值相反。

##### 创建 React 元素

我们建议[使用 JSX](https://react.docschina.org/docs/introducing-jsx.html) 来编写你的 UI 组件。每个 JSX 元素都是调用 [`React.createElement()`](https://react.docschina.org/docs/react-api.html#createelement) 的语法糖。一般来说，如果你使用了 JSX，就不再需要调用以下方法。

- [`createElement()`](https://react.docschina.org/docs/react-api.html#createelement)

###### createElement()

```js
React.createElement(
  type,
  [props],
  [...children]
)
```

创建并返回指定类型的新 [React 元素](https://react.docschina.org/docs/rendering-elements.html)。其中的类型参数既可以是标签名字符串（如 `'div'` 或 `'span'`），也可以是 [React 组件](https://react.docschina.org/docs/components-and-props.html) 类型 （class 组件或函数组件），或是 [React fragment](https://react.docschina.org/docs/react-api.html#reactfragment) 类型。

使用 [JSX](https://react.docschina.org/docs/introducing-jsx.html) 编写的代码将会被转换成使用 `React.createElement()` 的形式。如果使用了 JSX 方式，那么一般来说就不需要直接调用 `React.createElement()`。请查阅[不使用 JSX](https://react.docschina.org/docs/react-without-jsx.html) 章节获得更多信息。

##### 转换元素

`React` 提供了几个用于操作元素的 API：

- [`cloneElement()`](https://react.docschina.org/docs/react-api.html#cloneelement)
- [`isValidElement()`](https://react.docschina.org/docs/react-api.html#isvalidelement)
- [`React.Children`](https://react.docschina.org/docs/react-api.html#reactchildren)

###### cloneElement()

```js
React.cloneElement(
  element,
  [props],
  [...children]
)
```

以 `element` 元素为样板克隆并返回新的 React 元素。返回元素的 props 是将新的 props 与原始元素的 props 浅层合并后的结果。新的子元素将取代现有的子元素，而来自原始元素的 `key` 和 `ref` 将被保留。

`React.cloneElement()` 几乎等同于：

```jsx
// element.props 是原始元素的 props
<element.type {...element.props} {...props}>{children}</element.type>
```

###### isValidElement()

```
React.isValidElement(object)
```

验证对象是否为 React 元素，返回值为 `true` 或 `false`。

###### React.Children

`React.Children` 提供了用于处理 `this.props.children` 不透明数据结构的实用方法。

**`React.Children.map`**

```js
React.Children.map(children, (child, index) => {
  // ...
})
```

如果 `children` 是一个数组，它将被遍历并为数组中的每个子节点调用该函数，并返回一个数组。如果子节点为 `null` 或是 `undefined`，则此方法将返回 `null` 或是 `undefined`，而不会返回数组。

> 注意
>
> 如果 `children` 是一个 `Fragment` 对象，它将被视为单一子节点的情况处理，而不会被遍历。

**`React.Children.forEach`**

```js
React.Children.forEach(children, (child, index) => {  
// ...
})
```

与 [`React.Children.map()`](https://react.docschina.org/docs/react-api.html#reactchildrenmap) 类似，但它不会返回一个数组。


**`React.Children.count`**

```js
React.Children.count(children)
```

返回 `children` 中的组件总数量，等同于通过 `map` 或 `forEach` 调用回调函数的次数。

**`React.Children.only`**

```
React.Children.only(children)
```

验证 `children` 是否只有一个子节点（一个 React 元素），如果有则返回它，否则此方法会抛出错误。

> 注意：
>
> `React.Children.only()` 不接受 [`React.Children.map()`](https://react.docschina.org/docs/react-api.html#reactchildrenmap) 的返回值，因为它是一个数组而并不是 React 元素。

**`React.Children.toArray`**

```js
React.Children.toArray(children)
```

将 `children` 这个复杂的数据结构以数组的方式扁平展开并返回，并为每个子节点分配一个 key。当你想要在渲染函数中操作子节点的集合时，它会非常实用，特别是当你想要在向下传递 `this.props.children` 之前对内容重新排序或获取子集时。

##### Fragments

`React` 还提供了用于减少不必要嵌套的组件。

- [`React.Fragment`](https://react.docschina.org/docs/react-api.html#reactfragment)

###### React.Fragment

`React.Fragment` 组件能够在不额外创建 DOM 元素的情况下，让 `render()` 方法中返回多个元素。

```
render() {
  return (
    <React.Fragment>
      Some text.
      <h2>A heading</h2>
    </React.Fragment>
  );
}
```

你也可以使用其简写语法 `<></>`。

##### Refs

- [`React.createRef`](https://react.docschina.org/docs/react-api.html#reactcreateref)
- [`React.forwardRef`](https://react.docschina.org/docs/react-api.html#reactforwardref)

###### React.createRef

`React.createRef` 创建一个能够通过 ref 属性附加到 React 元素的 [ref](https://react.docschina.org/docs/refs-and-the-dom.html)。

> createRef每次渲染都会返回一个新的引用，而useRef每次都会返回相同的引用。

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);

    this.inputRef = React.createRef();
  }

  render() {
    return <input type="text" ref={this.inputRef} />;
  }

  componentDidMount() {
    this.inputRef.current.focus();
  }
}
```

###### React.forwardRef

`React.forwardRef` 会创建一个React组件，这个组件能够将其接受的 [ref](https://react.docschina.org/docs/refs-and-the-dom.html) 属性转发到其组件树下的另一个组件中（将子组件的DOM直接暴露给了父组件）。这种技术并不常见，但在以下两种场景中特别有用：

- [转发 refs 到 DOM 组件](https://react.docschina.org/docs/forwarding-refs.html#forwarding-refs-to-dom-components)
- [在高阶组件中转发 refs](https://react.docschina.org/docs/forwarding-refs.html#forwarding-refs-in-higher-order-components)

`React.forwardRef` 接受渲染函数作为参数。React 将使用 `props` 和 `ref` 作为参数来调用此函数。此函数应返回 React 节点。

```jsx
const FancyButton = React.forwardRef((props, ref) => (  
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

在上述的示例中，React 会将 `<FancyButton ref={ref}>` 元素的 `ref` 作为第二个参数传递给 `React.forwardRef` 函数中的渲染函数。该渲染函数会将 `ref` 传递给 `<button ref={ref}>` 元素。

因此，当 React 附加了 ref 属性之后，父组件中`ref.current` 将直接指向 `<button>` DOM 元素实例。

##### Suspense

Suspense 使得组件可以“等待”某些操作结束后，再进行渲染。目前，Suspense 仅支持的使用场景是：[通过 `React.lazy` 动态加载组件](https://react.docschina.org/docs/code-splitting.html#reactlazy)。它将在未来支持其它使用场景，如数据获取等。

- [`React.Suspense`](https://react.docschina.org/docs/react-api.html#reactsuspense)

- [`React.lazy`](https://react.docschina.org/docs/react-api.html#reactlazy)

###### React.Suspense

`React.Suspense` 可以指定加载指示器（loading indicator），以防其组件树中的某些子组件尚未具备渲染条件。目前，懒加载组件是 `<React.Suspense>` 支持的**唯一**用例：

```jsx
// 该组件是动态加载的
const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    // 显示 <Spinner> 组件直至 OtherComponent 加载完成
    <React.Suspense fallback={<Spinner />}>
      <div>
        <OtherComponent />
      </div>
    </React.Suspense>
  );
}
```

`fallback` 属性接受任何在组件加载过程中你想展示的元素或组件。你可以将 `Suspense` 组件置于懒加载组件之上的任何位置。你甚至可以用一个 `Suspense` 组件包裹多个懒加载组件。

它已被收录在了我们的[代码分割指南](https://react.docschina.org/docs/code-splitting.html#reactlazy)中。

###### React.lazy

`React.lazy()` 允许你定义一个动态加载的组件。这有助于缩减 bundle 的体积，并延迟加载在初次渲染时未用到的组件。

`React.lazy` 接受一个函数，这个函数需要动态调用 `import()`。它必须返回一个 `Promise`，该 Promise 需要 resolve 一个 `default` export 的 React 组件。

你可以在[代码分割文档](https://react.docschina.org/docs/code-splitting.html#reactlazy)中学习如何使用它。查阅[此文章](https://medium.com/@pomber/lazy-loading-and-preloading-components-in-react-16-6-804de091c82d)可以了解更多用法细节。

```js
// 这个组件是动态加载的
const SomeComponent = React.lazy(() => import('./SomeComponent'));
```

请注意，渲染 `lazy` 组件依赖该组件渲染树上层的 `<React.Suspense>` 组件。这是指定加载指示器（loading indicator）的方式。

> **注意**
>
> 使用 `React.lazy` 的动态引入特性需要 JS 环境支持 Promise。在 IE11 及以下版本的浏览器中需要通过引入 polyfill 来使用该特性。



##### 生命周期

每个组件都包含“生命周期方法”，你可以重写这些方法，以便于在运行过程中特定的阶段执行这些方法。**你可以使用此[生命周期图谱](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)作为速查表**。在下述列表中，常用的生命周期方法会被加粗。其余生命周期函数的使用则相对罕见。

> 函数组件没有生命周期。

**挂载**

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- [**`constructor()`**](https://react.docschina.org/docs/react-component.html#constructor)
- [`static getDerivedStateFromProps()`](https://react.docschina.org/docs/react-component.html#static-getderivedstatefromprops)
- [**`render()`**](https://react.docschina.org/docs/react-component.html#render) 切勿设置 refs
- [**`componentDidMount()`**](https://react.docschina.org/docs/react-component.html#componentdidmount) — 只执行一次，通常在此阶段进行与浏览器的交互和添加订阅，例如发送请求，拿到数据，通过 setState() 保存在 state 中

**更新**

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

- [`static getDerivedStateFromProps()`](https://react.docschina.org/docs/react-component.html#static-getderivedstatefromprops)
- [`shouldComponentUpdate()`](https://react.docschina.org/docs/react-component.html#shouldcomponentupdate)— 首次渲染或使用 `forceUpdate()` 时不会调用该方法
- [**`render()`**](https://react.docschina.org/docs/react-component.html#render)
- [`getSnapshotBeforeUpdate()`](https://react.docschina.org/docs/react-component.html#getsnapshotbeforeupdate)
- [**`componentDidUpdate()`**](https://react.docschina.org/docs/react-component.html#componentdidupdate) — 首次渲染不会执行

**卸载**

当组件从 DOM 中移除时会调用如下方法：

- [**`componentWillUnmount()`**](https://react.docschina.org/docs/react-component.html#componentwillunmount)

**错误处理**

当渲染过程，生命周期，或子组件的构造函数中抛出错误时，会调用如下方法：

- [`static getDerivedStateFromError()`](https://react.docschina.org/docs/react-component.html#static-getderivedstatefromerror) — 渲染阶段调用，不允许出现副作用
- [`componentDidCatch()`](https://react.docschina.org/docs/react-component.html#componentdidcatch) — 提交阶段调用，允许执行副作用

生命周期方法详解

###### constructor()

```js
constructor(props)
```

**如果不初始化 state 或不进行方法绑定，则不需要为 React 组件实现构造函数。**

在 React 组件挂载之前，会调用它的构造函数。在为 React.Component 子类实现构造函数时，应在其他语句之前前调用 `super(props)`。否则，`this.props` 在构造函数中可能会出现未定义的 bug。

通常，在 React 中，构造函数仅用于以下两种情况：

- 通过给 `this.state` 赋值对象来初始化[内部 state](https://react.docschina.org/docs/state-and-lifecycle.html)。
- 为[事件处理函数](https://react.docschina.org/docs/handling-events.html)绑定实例

在 `constructor()` 函数中**不要调用 `setState()` 方法**。如果你的组件需要使用内部 state，请直接在构造函数中为 **`this.state` 赋值初始 state**：

```js
constructor(props) {
  super(props);
  // 不要在这里调用 this.setState()
  this.state = { counter: 0 };
  this.handleClick = this.handleClick.bind(this);
}
```

只能在构造函数中直接为 `this.state` 赋值。如需在其他方法中赋值，你应使用 `this.setState()` 替代。

要避免在构造函数中引入任何副作用或订阅。如遇到此场景，请将对应的操作放置在 `componentDidMount` 中。

**避免将 props 的值复制给 state！这是一个常见的错误：**

```js
constructor(props) {
 super(props);
 // 不要这样做
 this.state = { color: props.color };
}
```

如此做毫无必要（你可以直接使用 `this.props.color`），同时还产生了 bug（更新 prop 中的 `color` 时，并不会影响 state）。

**只有在你刻意忽略 prop 更新的情况下使用（知道prop 更新不会影响state的情况下使用）。**此时，应将 prop.color的`color` 重命名为 `initialColor` 或 `defaultColor`。必要时，你可以[修改它的 `key`](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#recommendation-fully-uncontrolled-component-with-a-key)，以强制“重置”其内部 state。

请参阅关于[避免派生状态的博文](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)，以了解出现 state 依赖 props 的情况该如何处理。



###### render()

```js
render()
```

`render()` 方法是 class 组件中唯一必须实现的方法。

当 `render` 被调用时，它会检查 `this.props` 和 `this.state` 的变化并返回以下类型之一：

- **React 元素**。通常通过 JSX 创建。例如，`<div />` 会被 React 渲染为 DOM 节点，`<MyComponent />` 会被 React 渲染为自定义组件，无论是 `<div />` 还是 `<MyComponent />` 均为 React 元素。
- **数组或 fragments**。 使得 render 方法可以返回多个元素。欲了解更多详细信息，请参阅 [fragments](https://react.docschina.org/docs/fragments.html) 文档。
- **Portals**。可以渲染子节点到不同的 DOM 子树中。欲了解更多详细信息，请参阅有关 [portals](https://react.docschina.org/docs/portals.html) 的文档
- **字符串或数值类型**。它们在 DOM 中会被渲染为文本节点
- **布尔类型或 `null`**。什么都不渲染。（主要用于支持返回 `test && <Child />` 的模式，其中 test 为布尔类型。)

`render()` 函数应该为纯函数，这意味着在不修改组件 state 的情况下，每次调用时都返回相同的结果，并且它不会直接与浏览器交互。

如需与浏览器进行交互，请在 `componentDidMount()` 或其他生命周期方法中执行你的操作。保持 `render()` 为纯函数，可以使组件更容易思考。

> 注意
>
> 如果 `shouldComponentUpdate()` 返回 false，则不会调用 `render()`。



###### componentDidMount()

```js
componentDidMount()
```

`componentDidMount()` 会在组件挂载后（插入 DOM 树中）立即调用。依赖于 DOM 节点的初始化应该放在这里。如需通过网络请求获取数据，此处是实例化请求的好地方。

这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 `componentWillUnmount()` 里取消订阅。

你可以在 `componentDidMount()` 里**直接调用 `setState()`**。它将触发额外渲染，但此渲染会发生在浏览器更新屏幕之前。如此保证了即使在 `render()` 两次调用的情况下，用户也不会看到中间状态。请谨慎使用该模式，因为它会导致性能问题。通常，你应该在 `constructor()` 中初始化 state。如果你的渲染依赖于 DOM 节点的大小或位置，比如实现 modals 和 tooltips 等情况下，你可以使用此方式处理。



###### componentDidUpdate()

```js
componentDidUpdate(prevProps, prevState, snapshot)
```

`componentDidUpdate()` 会在更新后会被立即调用。首次渲染不会执行此方法。

当组件更新后，可以在此处对 DOM 进行操作。如果你对更新前后的 props 进行了比较，也可以选择在此处进行网络请求。（例如，当 props 未发生变化时，则不会执行网络请求）。

```js
componentDidUpdate(prevProps) {
  // 典型用法（不要忘记比较 props）：
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
```

你也可以在 `componentDidUpdate()` 中**直接调用 `setState()`**，但请注意**它必须被包裹在一个条件语句里**，正如上述的例子那样进行处理，否则会导致死循环。它还会导致额外的重新渲染，虽然用户不可见，但会影响组件性能。不要将 props “镜像”给 state，请考虑直接使用 props。 欲了解更多有关内容，请参阅[为什么 props 复制给 state 会产生 bug](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)。

如果组件实现了 `getSnapshotBeforeUpdate()` 生命周期（不常用），则它的返回值将作为 `componentDidUpdate()` 的第三个参数 “snapshot” 参数传递。否则此参数将为 undefined。

> 注意
>
> 如果 [`shouldComponentUpdate()`](https://react.docschina.org/docs/react-component.html#shouldcomponentupdate) 返回值为 false，则不会调用 `componentDidUpdate()`。



###### componentWillUnmount()

```js
componentWillUnmount()
```

`componentWillUnmount()` 会在组件卸载及销毁之前直接调用。在此方法中执行必要的清理操作，例如，清除 timer，取消网络请求或清除在 `componentDidMount()` 中创建的订阅等。

`componentWillUnmount()` 中**不应调用 `setState()`**，因为该组件将永远不会重新渲染。组件实例卸载后，将永远不会再挂载它。

###### shouldComponentUpdate()

```
shouldComponentUpdate(nextProps, nextState)
```

根据 `shouldComponentUpdate()` 的返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。默认行为是 state 每次发生变化组件都会重新渲染。大部分情况下，你应该遵循默认行为。

当 props 或 state 发生变化时，`shouldComponentUpdate()` 会在渲染执行之前被调用。返回值默认为 true。首次渲染或使用 `forceUpdate()` 时不会调用该方法。

此方法仅作为**[性能优化的方式](https://react.docschina.org/docs/optimizing-performance.html)**而存在。不要企图依靠此方法来“阻止”渲染，因为这可能会产生 bug。你应该**考虑使用内置的 [`PureComponent`](https://react.docschina.org/docs/react-api.html#reactpurecomponent) 组件**，而不是手动编写 `shouldComponentUpdate()`。`PureComponent` 会对 props 和 state 进行浅层比较，并减少了跳过必要更新的可能性。

如果你一定要手动编写此函数，可以将 `this.props` 与 `nextProps` 以及 `this.state` 与`nextState` 进行比较，并返回 `false` 以告知 React 可以跳过更新。请注意，返回 `false` 并不会阻止子组件在其 state 更改时重新渲染。

不建议在 `shouldComponentUpdate()` 中进行深层比较或使用 `JSON.stringify()`。这样非常影响效率，且会损害性能。

目前，如果 `shouldComponentUpdate()` 返回 `false`，则不会调用 [`UNSAFE_componentWillUpdate()`](https://react.docschina.org/docs/react-component.html#unsafe_componentwillupdate)，[`render()`](https://react.docschina.org/docs/react-component.html#render) 和 [`componentDidUpdate()`](https://react.docschina.org/docs/react-component.html#componentdidupdate)。后续版本，React 可能会将 `shouldComponentUpdate` 视为提示而不是严格的指令，并且，当返回 `false` 时，仍可能导致组件重新渲染。



###### static getDerivedStateFromProps()

```js
static getDerivedStateFromProps(props, state)
```

`getDerivedStateFromProps` 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。它应返回一个对象来更新 state，如果返回 null 则不更新任何内容。

`getDerivedStateFromProps` 的存在只有一个目的：让组件在 **props 变化**时更新 state。

此方法适用于[罕见的用例](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#when-to-use-derived-state)，即 state 的值在任何时候都取决于 props。例如，实现 `<Transition>` 组件可能很方便，该组件会比较当前组件与下一组件，以决定针对哪些组件进行转场动画。

派生状态会导致代码冗余，并使组件难以维护。 [确保你已熟悉这些简单的替代方案：](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html)

- 如果你需要**执行副作用**（例如，数据提取或动画）以响应 props 中的更改，请改用 [`componentDidUpdate`](https://react.docschina.org/docs/react-component.html#componentdidupdate)。
- 如果只想在 **prop 更改时重新计算某些数据**，[请使用 memoization helper 代替](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization)。
- 如果你想**在 prop 更改时“重置”某些 state**，请考虑使组件[完全受控](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#recommendation-fully-controlled-component)或[使用 `key` 使组件完全不受控](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#recommendation-fully-uncontrolled-component-with-a-key) 代替。

此方法无权访问组件实例。如果你需要，可以通过提取组件 props 的纯函数及 class 之外的状态，在`getDerivedStateFromProps()`和其他 class 方法之间重用代码。

请注意，不管原因是什么，都会在*每次*渲染前触发此方法。这与 `UNSAFE_componentWillReceiveProps` 形成对比，后者仅在父组件重新渲染时触发，而不是在内部调用 `setState` 时。



###### getSnapshotBeforeUpdate()

```js
getSnapshotBeforeUpdate(prevProps, prevState)
```

`getSnapshotBeforeUpdate()` 在最近一次渲染输出（提交到 DOM 节点）之前调用（render之后）。它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 `componentDidUpdate()`。

此用法并不常见，但它可能出现在 UI 处理中，如需要以特殊方式处理滚动位置的聊天线程等。

应返回 snapshot 的值（或 `null`）。

例如：

```jsx
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    // 我们是否在 list 中添加新的 items ？
    // 捕获滚动位置以便我们稍后调整滚动位置。
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    // 如果我们 snapshot 有值，说明我们刚刚添加了新的 items，
    // 调整滚动位置使得这些新 items 不会将旧的 items 推出视图。
    //（这里的 snapshot 是 getSnapshotBeforeUpdate 的返回值）
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }

  render() {
    return (
      <div ref={this.listRef}>{/* ...contents... */}</div>
    );
  }
}
```

**Error boundaries（错误边界）**

[Error boundaries](https://react.docschina.org/docs/error-boundaries.html) 是 React 组件，它会在其子组件树中的任何位置捕获 JavaScript 错误，并记录这些错误，展示降级 UI 而不是崩溃的组件树。Error boundaries 组件会捕获在渲染期间，在生命周期方法以及其整个树的构造函数中发生的错误。

如果 class 组件定义了生命周期方法 `static getDerivedStateFromError()` 或 `componentDidCatch()` 中的任何一个（或两者），它就成为了 Error boundaries。通过生命周期更新 state 可让组件捕获树中未处理的 JavaScript 错误并展示降级 UI。

仅使用 Error boundaries 组件来从意外异常中恢复的情况；**不要将它们用于流程控制。**

如果一个 class 组件中定义了 [`static getDerivedStateFromError()`](https://react.docschina.org/docs/react-component.html#static-getderivedstatefromerror) 或 [`componentDidCatch()`](https://react.docschina.org/docs/react-component.html#componentdidcatch) 这两个生命周期方法中的任意一个（或两个）时，那么它就变成一个错误边界。当抛出错误后，请使用 `static getDerivedStateFromError()` 渲染备用 UI ，使用 `componentDidCatch()` 打印错误信息。

###### static getDerivedStateFromError()

```js
static getDerivedStateFromError(error)
```

此生命周期会在**后代**组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 state。

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {    
  // 更新 state 使下一次渲染能够显示降级后的 UI
  	return { hasError: true };  }
  componentDidCatch(error, errorInfo) {    
    // 你同样可以将错误日志上报给服务器
    // "组件堆栈" 例子:
    //   in ComponentThatThrows (created by App)
    //   in ErrorBoundary (created by App)
    //   in div (created by App)
    //   in App
    logErrorToMyService(error, errorInfo);
  }
  render() {
    if (this.state.hasError) {
      // 你可以自定义降级后的 UI 并渲染
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children; 
  }
}
```

然后你可以将它作为一个常规组件去使用：

```jsx
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

> 注意
>
> `getDerivedStateFromError()` 会在`渲染`阶段调用，因此不允许出现副作用。 如遇此类情况，请改用 `componentDidCatch()`。

###### componentDidCatch()

```
componentDidCatch(error, info)
```

此生命周期在后代组件抛出错误后被调用。 它接收两个参数：

1. `error` —— 抛出的错误。
2. `info` —— 带有 `componentStack` key 的对象，其中包含[有关组件引发错误的栈信息](https://react.docschina.org/docs/error-boundaries.html#component-stack-traces)。

`componentDidCatch()` 会在“提交”阶段被调用，因此允许执行副作用。 它应该用于记录错误之类的情况。



##### 其他API

###### setState()

```js
setState(updater, [callback])
```

`setState()` 将对组件 state 的更改排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件。这是用于更新用户界面以响应事件处理器和处理服务器数据的主要方式

将 `setState()` 视为*请求*而不是立即更新组件的命令。为了更好的感知性能，React 会延迟调用它，然后通过一次传递更新多个组件。React 并不会保证 state 的变更会立即生效。

**`setState` 什么时候是异步的？**

目前，在 React 管理的事件回调和生命周期中，setState 是异步的，而其他时候 setState 都是同步的。这个问题根本原因就是 React 在自己管理的事件回调和生命周期中，对于 setState 是批量更新的，而在其他时候是立即更新的。

> 在setTimeout和原生DOM事件中，setState是同步的

`setState()` 并不**总**是立即更新组件。它会批量推迟更新。这使得在调用 `setState()` 后立即读取 `this.state` 成为了隐患。为了消除隐患，请使用 `componentDidUpdate` 或者 `setState` 的回调函数（`setState(updater, callback)`），这两种方式都可以保证在应用更新后触发。如需基于之前的 state 来设置当前的 state，请阅读下述关于参数 `updater` 的内容。

除非 `shouldComponentUpdate()` 返回 `false`，否则 `setState()` 将始终执行重新渲染操作。

参数一为带有形式参数的 `updater` 函数：

```js
(state, props) => stateChange
```

`state` 是对应用变化时组件状态的引用。当然，它不应直接被修改。你应该使用基于 `state` 和 `props` 构建的新对象来表示变化。例如，假设我们想根据 `props.step` 来增加 state：

```js
this.setState((state, props) => {
  return {counter: state.counter + props.step};
});
```

updater 函数中接收的 `state` 和 `props` 都保证为最新。updater 的返回值会与组件 `state` 进行浅合并。

`setState()` 的第二个参数为可选的回调函数，它将在 `setState` 完成合并并重新渲染组件后执行。通常，我们建议使用 `componentDidUpdate()` 来代替此方式。

`setState()` 的第一个参数除了接受函数外，还可以接受对象类型：

```js
setState(stateChange[, callback])
```

`stateChange` 会将传入的对象浅层合并到组件的 state 中，例如，调整购物车商品数：

```js
this.setState({quantity: 2})
```

这种形式的 `setState()` 也是异步的，并且在同一周期内会对多个 `setState` 进行批处理。例如，如果在同一周期内多次设置商品数量增加（对象形式），则相当于：

```js
Object.assign(
  previousState,
  {quantity: state.quantity + 1},
  {quantity: state.quantity + 1},
  ...
)
```

后调用的 `setState()` 将覆盖同一周期内先调用 `setState` 的值，因此商品数仅增加一次。如果后续状态取决于当前状态，我们建议使用 updater 函数的形式代替（异步调用多个updater，链式地进行更新）：

```js
this.setState((state) => {
  return {quantity: state.quantity + 1};
});
```

###### forceUpdate()

```js
component.forceUpdate(callback)
```

默认情况下，当组件的 state 或 props 发生变化时，组件将重新渲染。如果 `render()` 方法依赖于其他数据，则可以调用 `forceUpdate()` 强制让组件重新渲染。

调用 `forceUpdate()` 将致使组件调用 `render()` 方法，此操作会跳过该组件的 `shouldComponentUpdate()`。但其子组件会触发正常的生命周期方法，包括 `shouldComponentUpdate()` 方法。如果标记发生变化，React 仍将只更新 DOM。

通常你应该避免使用 `forceUpdate()`，尽量在 `render()` 中使用 `this.props` 和 `this.state`。

###### Class 属性

**`defaultProps`**

`defaultProps` 可以为 Class 组件添加默认 props。这一般用于 props 未赋值，但又不能为 null 的情况。例如：

```jsx
class CustomButton extends React.Component {
  // ...
}

CustomButton.defaultProps = {
  color: 'blue'
};
```

如果未提供 `props.color`，则默认设置为 `'blue'`

**`displayName`**

`displayName` 字符串多用于调试消息。通常，你不需要设置它，因为它可以根据函数组件或 class 组件的名称推断出来。如果调试时需要显示不同的名称或创建高阶组件，请参阅[使用 displayname 轻松进行调试](https://react.docschina.org/docs/higher-order-components.html#convention-wrap-the-display-name-for-easy-debugging)了解更多。

###### 实例属性

**`props`**

`this.props` 包括被该组件调用者定义的 props。欲了解 props 的详细介绍，请参阅[组件 & Props](https://react.docschina.org/docs/components-and-props.html)。

需特别注意，`this.props.children` 是一个特殊的 prop，通常由 JSX 表达式中的子组件组成，而非组件本身定义。

**`state`**

组件中的 state 包含了随时可能发生变化的数据。state 由用户自定义，它是一个普通 JavaScript 对象。

如果某些值未用于渲染或数据流（例如，计时器 ID），则不必将其设置为 state。此类值可以在组件实例上定义。

欲了解关于 state 的更多信息，请参阅 [State & 生命周期](https://react.docschina.org/docs/state-and-lifecycle.html)。

永远不要直接改变 `this.state`，因为后续调用的 `setState()` 可能会替换掉你的改变。请把 `this.state` 看作是不可变的。

##### ReactDom

如果你使用一个 `<script>` 标签引入 React，所有的顶层 API 都能在全局 `ReactDOM` 上调用。如果你使用 npm 和 ES6，你可以用 `import ReactDOM from 'react-dom'`。如果你使用 npm 和 ES5，你可以用 `var ReactDOM = require('react-dom')`。

**概览**

`react-dom` 的 package 提供了可在应用顶层使用的 DOM（DOM-specific）方法，如果有需要，你可以把这些方法用于 React 模型以外的地方。不过一般情况下，大部分组件都不需要使用这个模块。

###### render()

```js
ReactDOM.render(element, container[, callback])
```

在提供的 `container` 里渲染一个 React 元素，并返回对该组件的[引用](https://react.docschina.org/docs/more-about-refs.html)（或者针对[无状态组件](https://react.docschina.org/docs/components-and-props.html#function-and-class-components)返回 `null`，目前应该避免使用返回的引用，因为它是历史遗留下来的内容，而且在未来版本的 React 中，组件渲染在某些情况下可能会是异步的。 如果你真的需要获得对根组件 `ReactComponent` 实例的引用，那么推荐为根元素添加 [callback ref](https://react.docschina.org/docs/more-about-refs.html#the-ref-callback-attribute)。）。

渲染多个 **dom** 元素外层需使用一个标签（根元素）进行包裹。

如果 React 元素之前已经在 `container` 里渲染过，这将会对其执行更新操作，并仅会在必要时改变 DOM 以映射最新的 React 元素。

如果提供了可选的回调函数，该回调将在组件被渲染或更新之后被执行。

> 注意：
>
> `ReactDOM.render()` 会控制你传入容器节点里的内容。当首次调用时，容器节点里的所有 DOM 元素都会被替换，后续的调用则会使用 React 的 DOM 差分算法（DOM diffing algorithm）进行高效的更新。
>
> `ReactDOM.render()` 不会修改容器节点（只会覆盖容器的子节点）。
>
> 使用 `ReactDOM.render()` 对服务端渲染容器进行 hydrate 操作的方式已经被废弃，并且会在 React 17 被移除。作为替代，请使用 [`hydrate()`](https://react.docschina.org/docs/react-dom.html#hydrate)。

###### unmountComponentAtNode()

```js
ReactDOM.unmountComponentAtNode(container)
```

从 DOM 中卸载组件，会将其事件处理器（event handlers）和 state 一并清除。如果指定容器上没有对应已挂载的组件，这个函数什么也不会做。如果组件被移除将会返回 `true`，如果没有组件可被移除将会返回 `false`。

###### findDOMNode()

> 注意:
>
> `findDOMNode` 是一个访问底层 DOM 节点的应急方案（escape hatch）。在大多数情况下，不推荐使用该方法，因为它会破坏组件的抽象结构。[严格模式下该方法已弃用。](https://react.docschina.org/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)

```
ReactDOM.findDOMNode(component)
```

如果组件已经被挂载到 DOM 上，此方法会返回浏览器中相应的原生 DOM 元素。此方法对于从 DOM 中读取值很有用，例如获取表单字段的值或者执行 DOM 检测（performing DOM measurements）。**大多数情况下，你可以绑定一个 ref 到 DOM 节点上，可以完全避免使用 findDOMNode。**

> 注意:
>
> `findDOMNode` 只能找到已挂载的组件（即已经放置在 DOM 中的组件）。
>
> `findDOMNode` 不能用于函数组件。

###### createPortal()

```js
ReactDOM.createPortal(child, container)
```

创建 portal。[Portal](https://react.docschina.org/docs/portals.html) 将提供一种将子节点渲染到 DOM 节点中的方式，该节点存在于 DOM 组件的层次结构之外。

##### DOM 元素

React 实现了一套独立于浏览器的 DOM 系统，兼顾了性能和跨浏览器的兼容性。我们借此机会完善了浏览器 DOM 实现的一些特殊情况。

在 React 中，所有的 DOM 特性和属性（包括事件处理）都应该是小驼峰命名的方式。例如，与 HTML 中的 `tabindex` 属性对应的 React 的属性是 `tabIndex`。例外的情况是 `aria-*` 以及 `data-*` 属性，一律使用小写字母命名。比如, 你依然可以用 `aria-label` 作为 `aria-label`。

###### 属性差异

React 与 HTML 之间有很多属性存在差异：

###### className

`className` 属性用于指定 CSS 的 class，此特性适用于所有常规 DOM 节点和 SVG 元素，如 `<div>`，`<a>` 及其它标签。

如果你在 React 中使用 Web Components（这是一种不常见的使用方式），请使用 class 属性代替。

###### value

`<input>` 和 `<textarea>` 组件支持 `value` 属性。你可以使用它为组件设置 value。这对于构建受控组件是非常有帮助。`defaultValue` 属性对应的是非受控组件的属性，用于设置组件第一次挂载时的 value。

###### checked

当 `<input>` 组件的 type 类型为 `checkbox` 或 `radio` 时，组件支持 `checked` 属性。你可以使用它来设置组件是否被选中。这对于构建受控组件（controlled components）很有帮助。而 `defaultChecked` 则是非受控组件的属性，用于设置组件首次挂载时是否被选中。

###### onChange

`onChange` 事件与预期行为一致：每当表单字段变化时，该事件都会被触发。我们故意没有使用浏览器已有的默认行为，是因为 `onChange` 在浏览器中的行为和名称不对应，并且 React 依靠了该事件实时处理用户输入。

###### selected

`<option>` 组件支持 `selected` 属性。你可以使用该属性设置组件是否被选择。这对构建受控组件很有帮助。

###### dangerouslySetInnerHTML

`dangerouslySetInnerHTML` 是 React 为浏览器 DOM 提供 `innerHTML` 的替换方案。通常来讲，使用代码直接设置 HTML 存在风险，因为很容易无意中使用户暴露于[跨站脚本（XSS）](https://en.wikipedia.org/wiki/Cross-site_scripting)的攻击。这就意味着在React中，如果你不得不设置HTML，你就需要用DangerouslySetInnerHTML来代替传统的innerHTML方法。

设置 `dangerouslySetInnerHTML` 时，需要向其传递包含 key 为 `__html` 的对象。例如：

`dangerouslySetInnerHTML`被设计用来警示开发者，赋值给__html属性的值必须得是经过处理的，你可以用第三方库dompurify来帮你处理数据。

`dangerouslySetInnerHTML`命名是用来告诉用户设置HTML是存在风险的，需要开发者明确自己在做什么。

```jsx
function createMarkup() {
  return {__html: 'First &middot; Second'};
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}
```

###### htmlFor

由于 `for` 在 JavaScript 中是保留字，所以 React 元素中使用了 `htmlFor` 来代替。即 HTML 标签的 for 属性在 React 中用 htmlFor 表示。

###### style

> 注意
>
> 在文档中，部分例子为了方便，直接使用了 `style`，但是**通常不推荐将 `style` 属性作为设置元素样式的主要方式**。在多数情况下，应使用 [`className`](https://react.docschina.org/docs/dom-elements.html#classname) 属性来引用外部 CSS 样式表中定义的 class。`style` 在 React 应用中多用于在渲染过程中添加动态计算的样式。

`style` 接受一个采用小驼峰命名属性的 JavaScript 对象，而不是 CSS 字符串。这与 DOM 中 `style` 的 JavaScript 属性是一致的（例如background-image的 JavaScript 属性是backgroundImage），同时会更高效的，且能预防跨站脚本（XSS）的安全漏洞。例如：

```jsx
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```

注意：样式不会自动补齐前缀。如需支持旧版浏览器，请手动补充对应的样式属性：

```jsx
const divStyle = {
  WebkitTransition: 'all', // 谷歌、苹果
  MozTransition:'all', // 火狐
  OTransition:'all', // Opera
  msTransition: 'all' // 微软，'ms' is the only lowercase vendor prefix
};

function ComponentWithTransition() {
  return <div style={divStyle}>This should work cross-browser</div>;
}
```

Style 中的 key 采用小驼峰命名是为了与 JS 访问 DOM 节点的属性保持一致（例如：`node.style.backgroundImage` ）。浏览器引擎前缀都应以大写字母开头，[除了 `ms`](https://www.andismith.com/blog/2012/02/modernizr-prefixed/)。因此，`WebkitTransition` 首字母为 ”W”。

React 会自动添加 ”px” 后缀到内联样式为数字的属性后。如需使用 ”px” 以外的单位，请将此值设为数字与所需单位组成的字符串。例如：

```jsx
// Result style: '10px'
<div style={{ height: 10 }}>
  Hello World!
</div>

// Result style: '10%'
<div style={{ height: '10%' }}>
  Hello World!
</div>
```

但并非所有样式属性都转换为像素字符串。有些样式属性是没有单位的(例如 `zoom`，`order`，`flex`)。无单位属性的完整列表在[此处](https://github.com/facebook/react/blob/4131af3e4bf52f3a003537ec95a1655147c81270/src/renderers/dom/shared/CSSProperty.js#L15-L59)。

###### suppressContentEditableWarning

通常，当拥有子节点的元素被标记为 `contentEditable` 时，React 会发出一个警告，因为这不会生效。该属性将禁止此警告。尽量不要使用该属性，除非你要构建一个类似 [Draft.js](https://facebook.github.io/draft-js/) 的手动管理 `contentEditable` 属性的库。

###### All Supported HTML Attributes

在 React 16 中，任何标准的[或自定义的](https://react.docschina.org/blog/2017/09/08/dom-attributes-in-react-16.html) DOM 属性都是完全支持的。

React 为 DOM 提供了一套以 JavaScript 为中心的 API。由于 React 组件经常采用自定义或和 DOM 相关的 props 的关系，React 采用了`小驼峰命名`的方式，正如 DOM APIs 那样：

```div
<div tabIndex="-1" />      // Just like node.tabIndex DOM API
<div className="Button" /> // Just like node.className DOM API
<input readOnly={true} />  // Just like node.readOnly DOM API
```

除了上述文档提到的特殊拼写方式以外，这些 props 的用法与 HTML 的属性也极为类似。

React 支持的 DOM 属性有：

```
accept acceptCharset accessKey action allowFullScreen alt async autoComplete
autoFocus autoPlay capture cellPadding cellSpacing challenge charSet checked
cite classID className colSpan cols content contentEditable contextMenu controls
controlsList coords crossOrigin data dateTime default defer dir disabled
download draggable encType form formAction formEncType formMethod formNoValidate
formTarget frameBorder headers height hidden high href hrefLang htmlFor
httpEquiv icon id inputMode integrity is keyParams keyType kind label lang list
loop low manifest marginHeight marginWidth max maxLength media mediaGroup method
min minLength multiple muted name noValidate nonce open optimum pattern
placeholder poster preload profile radioGroup readOnly rel required reversed
role rowSpan rows sandbox scope scoped scrolling seamless selected shape size
sizes span spellCheck src srcDoc srcLang srcSet start step style summary
tabIndex target title type useMap value width wmode wrap
```

同样，所有的 SVG 属性也完全得到了支持：

```
accentHeight accumulate additive alignmentBaseline allowReorder alphabetic
amplitude arabicForm ascent attributeName attributeType autoReverse azimuth
baseFrequency baseProfile baselineShift bbox begin bias by calcMode capHeight
clip clipPath clipPathUnits clipRule colorInterpolation
colorInterpolationFilters colorProfile colorRendering contentScriptType
contentStyleType cursor cx cy d decelerate descent diffuseConstant direction
display divisor dominantBaseline dur dx dy edgeMode elevation enableBackground
end exponent externalResourcesRequired fill fillOpacity fillRule filter
filterRes filterUnits floodColor floodOpacity focusable fontFamily fontSize
fontSizeAdjust fontStretch fontStyle fontVariant fontWeight format from fx fy
g1 g2 glyphName glyphOrientationHorizontal glyphOrientationVertical glyphRef
gradientTransform gradientUnits hanging horizAdvX horizOriginX ideographic
imageRendering in in2 intercept k k1 k2 k3 k4 kernelMatrix kernelUnitLength
kerning keyPoints keySplines keyTimes lengthAdjust letterSpacing lightingColor
limitingConeAngle local markerEnd markerHeight markerMid markerStart
markerUnits markerWidth mask maskContentUnits maskUnits mathematical mode
numOctaves offset opacity operator order orient orientation origin overflow
overlinePosition overlineThickness paintOrder panose1 pathLength
patternContentUnits patternTransform patternUnits pointerEvents points
pointsAtX pointsAtY pointsAtZ preserveAlpha preserveAspectRatio primitiveUnits
r radius refX refY renderingIntent repeatCount repeatDur requiredExtensions
requiredFeatures restart result rotate rx ry scale seed shapeRendering slope
spacing specularConstant specularExponent speed spreadMethod startOffset
stdDeviation stemh stemv stitchTiles stopColor stopOpacity
strikethroughPosition strikethroughThickness string stroke strokeDasharray
strokeDashoffset strokeLinecap strokeLinejoin strokeMiterlimit strokeOpacity
strokeWidth surfaceScale systemLanguage tableValues targetX targetY textAnchor
textDecoration textLength textRendering to transform u1 u2 underlinePosition
underlineThickness unicode unicodeBidi unicodeRange unitsPerEm vAlphabetic
vHanging vIdeographic vMathematical values vectorEffect version vertAdvY
vertOriginX vertOriginY viewBox viewTarget visibility widths wordSpacing
writingMode x x1 x2 xChannelSelector xHeight xlinkActuate xlinkArcrole
xlinkHref xlinkRole xlinkShow xlinkTitle xlinkType xmlns xmlnsXlink xmlBase
xmlLang xmlSpace y y1 y2 yChannelSelector z zoomAndPan
```

你也可以使用自定义属性，但要注意属性名全都为小写。

##### SVG

SVG 有两种引入用法：

**1、作为url**

```css
.Logo {
  background-image: url(./logo.png);
}
```

**2、作为React 组件**

```js
import { ReactComponent as Logo } from './logo.svg';
const App = () => (
  <div>
    {/* Logo 是一个实际的 React 组件 */}
    <Logo />
  </div>
);
```

如果你不想将 SVG 作为单独的文件加载，可以使用这种方法。不要忘记导入中的花括号！ `ReactComponent` 导入名称是特殊的，它告诉 Create React App 你想要一个渲染 SVG 的 React 组件，而不是它的文件名。

如果使用这种方式且用TS开发，需要在全局声明文件中：

```ts
/// <reference types="react" />
/// <reference types="react-dom" />
/// <reference types="react-scripts" />
declare module '*.svg' {
  import React from 'react'
  export const ReactComponent: React.FC<
    React.SVGProps<SVGSVGElement> & { title?: string }
  >
  export const src: string
}
```



##### 合成事件

###### **概览**

`SyntheticEvent` 实例将作为参数被传递给你的事件处理函数，它是浏览器的原生事件的跨浏览器包装器。除兼容所有浏览器外，它还拥有和浏览器原生事件相同的接口，包括 `stopPropagation()` 和 `preventDefault()`。

如果因为某些原因，当你需要使用浏览器的底层事件时，只需要使用 `nativeEvent` 属性来获取即可。合成事件与浏览器的原生事件不同，也不会直接映射到原生事件。例如，在 `onMouseLeave` 事件中 `event.nativeEvent` 将指向 `mouseout` 事件。

每个 `SyntheticEvent` 对象都包含以下属性：

```
boolean bubbles
boolean cancelable
DOMEventTarget currentTarget
boolean defaultPrevented
number eventPhase
boolean isTrusted
DOMEvent nativeEvent
void preventDefault()
boolean isDefaultPrevented()
void stopPropagation()
boolean isPropagationStopped()
void persist()
DOMEventTarget target
number timeStamp
string type
```

###### **事件池**

`SyntheticEvent` 是合并而来。这意味着 `SyntheticEvent` 对象可能会被重用，而且在事件回调函数被调用后，所有的属性都会无效。出于性能考虑，你不能通过异步访问事件。

> 所有产生的事件都会生成一个事件对象，按正常逻辑 在我们的事件处理函数执行完后，这个事件对象就应该被释放了，等待着被内存回收；但如果在短时间内触发了许多次事件，就要频繁的生成和销毁事件对象；那么 为了提高性能，React就用了一个“事件池”这么一个池子，被使用完后的事件，并不直接销毁，而是将其身上的属性清空掉了后放进事件池中， 等到了下一次有同类型事件发生时，就不用再new一个新的事件对象了，直接从事件池取出一个现成的就可以用了， 从而实现事件对象的*重用*。

在合成事件机制里，一旦事件监听回调被执行，合成事件对象的属性就会被清空，将合成事件对象放进事件池。如果你想异步访问事件属性，你需在事件上调用 `event.persist()`，此方法会从池中移除合成事件，允许用户代码保留对事件的引用。

> 注意：
>
> 从 v17 开始，`e.persist()` 将不再生效，因为 `SyntheticEvent` 不再放入[事件池](https://zh-hans.reactjs.org/docs/legacy-event-pooling.html)中。
>
> Web 端的 React 17 **不使用**事件池。

```js
function onClick(event) {
  // event.persist()  // 异步访问事件属性
  console.log(event); // => nullified object.
  console.log(event.type); // => "click"
  const eventType = event.type; // => "click"

  setTimeout(function() {
    console.log(event.type); // => null
    console.log(eventType); // => "click"
  }, 0);

  // 不起作用，this.state.clickEvent 的值将会只包含 null
  this.setState({clickEvent: event});

  // 你仍然可以导出事件属性
  this.setState({eventType: event.type});
}
```

###### 支持的事件

React 通过将事件 normalize 以让他们在不同浏览器中拥有一致的属性。

以下的事件处理函数在冒泡阶段被触发。如需注册捕获阶段的事件处理函数，则应为事件名添加 `Capture`。例如，处理捕获阶段的点击事件请使用 `onClickCapture`，而不是 `onClick`。

**剪贴板事件**

事件名：

```
onCopy onCut onPaste
```

属性：

```
DOMDataTransfer clipboardData
```

------

**复合事件**

复合事件主要的用处在于使用输入法编辑器时，往往需要使用多个键，但是最终只输入一个字符。复合事件就是为了检测和输入这种输入而设计的。例如：当我们需要在输入框中输入中文“张”的时候，需要输入`zhang`最后按回车键输入到输入框中。

- compositionstart：要开始输入；
- compositionupdate：插入新字符；
- compositionend：复合系统关闭，返回正常键盘输入状态；

data属性：

- compositionstart访问data：正在编辑的文本；
- compositionupdate访问data：正插入的新字符；
- compositionend访问data：插入的所有字符；

事件名:

```
onCompositionEnd onCompositionStart onCompositionUpdate
```

属性:

```
string data
```

------

**键盘事件**

事件名:

```
onKeyDown onKeyPress onKeyUp
```

属性:

```
boolean altKey
number charCode
boolean ctrlKey
boolean getModifierState(key)
string key
number keyCode
string locale
number location
boolean metaKey
boolean repeat
boolean shiftKey
number which
```

`key` 属性可以是 [DOM Level 3 Events spec](https://www.w3.org/TR/uievents-key/#named-key-attribute-values) 里记录的任意值。

------

**焦点事件**

事件名：

```
onFocus onBlur
```

这些焦点事件在 React DOM 上的所有元素都有效，不只是表单元素。

属性：

```
DOMEventTarget relatedTarget
```

------

**表单事件**

事件名：

```
onChange onInput onInvalid onReset onSubmit 
```

想了解 onChange 事件的更多信息，查看 [Forms](https://react.docschina.org/docs/forms.html) 。

------

**通用事件**

事件名称：

```
onError onLoad
```

------

**Mouse Events**

鼠标事件：

```
onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave
onMouseMove onMouseOut onMouseOver onMouseUp
```

`onMouseEnter` 和 `onMouseLeave` 事件从离开的元素向进入的元素传播，不是正常的冒泡，也没有捕获阶段。

属性：

```
boolean altKey
number button
number buttons
number clientX
number clientY
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
number pageX
number pageY
DOMEventTarget relatedTarget
number screenX
number screenY
boolean shiftKey
```

------

**指针事件**

事件名:

```
onPointerDown onPointerMove onPointerUp onPointerCancel onGotPointerCapture
onLostPointerCapture onPointerEnter onPointerLeave onPointerOver onPointerOut
```

`onPointerEnter` 和 `onPointerLeave` 事件从离开的元素向进入的元素传播，不是正常的冒泡，也没有捕获阶段。

属性：

如 [W3 spec](https://www.w3.org/TR/pointerevents/) 中定义的，指针事件通过以下属性扩展了[鼠标事件](https://react.docschina.org/docs/events.html#mouse-events)：

```
number pointerId
number width
number height
number pressure
number tangentialPressure
number tiltX
number tiltY
number twist
string pointerType
boolean isPrimary
```

关于跨浏览器支持的说明：

并非每个浏览器都支持指针事件（在写这篇文章时，已支持的浏览器有：Chrome，Firefox，Edge 和 Internet Explorer）。React 故意不通过 polyfill 的方式适配其他浏览器，主要是符合标准的 polyfill 会显著增加 react-dom 的 bundle 大小。

如果你的应用要求指针事件，我们推荐添加第三方的指针事件 polyfil 。

------

**选择事件**

事件名：

```
onSelect
```

------

**触摸事件**

事件名：

```
onTouchCancel onTouchEnd onTouchMove onTouchStart
```

属性：

```
boolean altKey
DOMTouchList changedTouches
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
boolean shiftKey
DOMTouchList targetTouches
DOMTouchList touches
```

------

**UI 事件**

事件名：

```
onScroll
```

属性：

```
number detail
DOMAbstractView view
```

------

**滚轮事件**

事件名：

```
onWheel
```

属性：

```
number deltaMode
number deltaX
number deltaY
number deltaZ
```

------

**媒体事件**

事件名：

```
onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted
onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay
onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend
onTimeUpdate onVolumeChange onWaiting
```

------

**图像事件**

事件名：

```
onLoad onError
```

------

**动画事件**

事件名：

```
onAnimationStart onAnimationEnd onAnimationIteration
```

属性：

```
string animationName
string pseudoElement
float elapsedTime
```

------

**过渡事件**

事件名：

```
onTransitionEnd
```

属性：

```
string propertyName
string pseudoElement
float elapsedTime
```

------

**其他事件**

事件名：

```
onToggle
```



#### 高级指引

##### 组件通信方式

组件关系：

- 父子组件
- 跨级组件
- 非嵌套组件

通信方式：

- 父组件向子组件通信：props、forwardRef（将父组件接收的ref属性转发到后代组件）
- 子组件向父组件通信：自定义事件 — 父组件将一个能够触发 setState 的回调函数传递给子组件，子组件通过事件监听调用该回调函数
- 跨级组件（祖—>孙）：redux、context、层层传递props
- 非嵌套组件：redux、events包（第三方）

events包的使用（和Vue的事件总线一样）：

```sh
npm install events --save
```

在src下新建一个util目录里面建一个events.js：

```js
import { EventEmitter } from 'events';

export default new EventEmitter();
```

Brother.jsx：

```jsx
import React, { Component } from 'react';
import emitter from '../util/events';

class Brother extends Component {
    constructor(props) {
      super(props);
      this.state = {
        message: 'Brother',
      };
    }
    componentDidMount() {
      // 组件装载完成以后声明一个自定义事件
      this.eventEmitter = emitter.addListener('changeMessage', (message) => {
        this.setState({
          message,
        });
      });
    }
    componentWillUnmount() {
      // 组件卸载时销毁事件
      emitter.removeListener(this.eventEmitter);
    }
    render() {
      return (
        <div>
          {this.state.message}
        </div>
      );
    }
}

export default Brother;
```

Sister.jsx：

```jsx
import React, { Component } from 'react';
import emitter from '../util/events';

class List2 extends Component {
    handleClick = (message) => {
       // 通知Brother组件
       emitter.emit('changeMessage', message);
    };
    render() {
        return (
            <div>
                <button onClick={this.handleClick.bind(this, 'Sister')}>点击我改变Brother组件中显示信息</button>
            </div>
        );
    }
}

```



##### 共享组件状态逻辑

 [render props](https://react.docschina.org/docs/render-props.html) 、[高阶组件](https://react.docschina.org/docs/higher-order-components.html)、自定义 Hook

##### 逻辑和UI分割

UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。

如果一个组件既有 UI 又有业务逻辑，那怎么办？

回答是，将它拆分成下面的结构：外面是一个容器组件，里面包了一个UI 组件。前者负责与外部的通信，将数据传给后者，由后者渲染出视图。

##### 代码分割

 对你的应用进行代码分割能够帮助你“懒加载”当前用户所需要的内容，能够显著地提高你的应用性能。尽管并没有减少应用整体的代码体积，但你可以避免加载用户永远不需要的代码，并在初始加载的时候减少所需加载的代码量。 

**import()**

 在你的应用中引入代码分割的最佳方式是通过动态 `import()` 语法。 

**使用之前：**

```js
import { add } from './math';

console.log(add(16, 26));
```

**使用之后：**

```js
// import("./math") 会返回 Promise 
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

当 Webpack 解析到该语法时，会自动进行代码分割。如果你使用 Create React App，该功能已开箱即用，你可以立刻使用该特性。Next.js也已支持该特性而无需进行配置。

如果你自己配置 Webpack，你可能要阅读下 Webpack 关于[代码分割](https://webpack.docschina.org/guides/code-splitting/)的指南。你的 Webpack 配置应该[类似于此](https://gist.github.com/gaearon/ca6e803f5c604d37468b0091d9959269)。

当使用Babel时，你要确保 Babel 能够解析动态 import 语法而不是将其进行转换。对于这一要求你需要 [babel-plugin-syntax-dynamic-import](https://yarnpkg.com/en/package/babel-plugin-syntax-dynamic-import) 插件。

**React.lazy**

> `React.lazy` 和 Suspense 技术还不支持服务端渲染。如果你想要在使用服务端渲染的应用中使用，我们推荐 [Loadable Components](https://github.com/gregberge/loadable-components) 这个库。它有一个很棒的[服务端渲染打包指南](https://loadable-components.com/docs/server-side-rendering/)。 

`React.lazy` 函数能让你像渲染常规组件一样处理动态引入（的组件）。

 Suspense 让组件“等待”某个异步操作，直到该异步操作结束即可渲染，后续章节会作出详细说明。 

**使用之前：**

```js
import OtherComponent from './OtherComponent';
```

**使用之后：**

```js
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

此代码将会在组件首次渲染时，自动导入包含 `OtherComponent` 组件的包。

`React.lazy` 接受一个函数，这个函数需要动态调用 `import()`。它必须返回一个 `Promise`，该 Promise 需要 resolve 一个 `defalut` export 的 React 组件。 

**异常捕获边界（Error boundaries）**

如果模块加载失败（如网络问题），它会触发一个错误。你可以通过[异常捕获边界（Error boundaries）](https://react.docschina.org/docs/error-boundaries.html)技术来处理这些情况，以显示良好的用户体验并管理恢复事宜。

**基于路由的代码分割**

决定在哪引入代码分割需要一些技巧。你需要确保选择的位置能够均匀地分割代码包而不会影响用户体验。

一个不错的选择是从路由开始。大多数网络用户习惯于页面之间能有个加载切换过程。你也可以选择重新渲染整个页面，这样您的用户就不必在渲染的同时再和页面上的其他元素进行交互。

这里是一个例子，展示如何在你的应用中使用 `React.lazy` 和 [React Router](https://react-router.docschina.org/) 这类的第三方库，来配置基于路由的代码分割。

```jsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <MyBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <Switch>
          <Route exact path="/" component={Home}/>
          <Route path="/about" component={About}/>
        </Switch>
      </Suspense>
    </MyBoundary>
  </Router>
);
```

错误边界处理：ErrorBoundary，当路由懒加载失败时显示

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
	// 此生命周期会在后代组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 state
  static getDerivedStateFromError(error) {    
    // 更新 state 使下一次渲染能够显示降级后的 UI
    return { hasError: true };
  }
  componentDidCatch(error, errorInfo) {
    // 你同样可以将错误日志上报给服务器
    logErrorToMyService(error, errorInfo);
  }
  render() {
    if (this.state.hasError) {
      // 你可以自定义降级后的 UI 并渲染
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children; 
  }
}
```

> getDerivedStateFromError 不能执行副作用，componentDidCatch 可以执行副作用

**命名导出（Named Exports）**

`React.lazy` 目前只支持默认导出（default exports）。如果你想被引入的模块使用命名导出（named exports），你可以创建一个中间模块，来重新导出为默认模块。这能保证 tree shaking 不会出错，并且不必引入不需要的组件。

```js
// ManyComponents.js
export const MyComponent = /* ... */;
// ......
```

```js
// MyComponent.js 中间模块：重新导出为默认模块
export { MyComponent as default } from "./ManyComponents.js";
```

```js
// MyApp.js
import React, { lazy } from 'react';
const MyComponent = lazy(() => import("./MyComponent.js"));
```

##### Context

Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。 

在一个典型的 React 应用中，数据是通过 props 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。 

Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。举个例子，在下面的代码中，我们通过一个 “theme” 属性手动调整一个按钮组件的样式：

```jsx
class App extends React.Component {
  render() {
    <!-- 这里传递 -->
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  // Toolbar 组件接受一个额外的“theme”属性，然后传递给 ThemedButton 组件。
  // 如果应用中每一个单独的按钮都需要知道 theme 的值，这会是件很麻烦的事，
  // 因为必须将这个值层层传递所有组件。
  return (
    <div>
      <!-- 这里传递 -->
      <ThemedButton theme={props.theme} />
    </div>  );
}

class ThemedButton extends React.Component {
  render() {
    <!-- 最后传递到这里 -->
    return <Button theme={this.props.theme} />;
  }
}
```

使用 context, 我们可以避免通过中间元素传递 props：

```jsx
// Context 可以让我们无须明确地传遍每一个组件，就能将值深入传递进组件树。
// 为当前的 theme 创建一个 context（“light”为默认值）。
const ThemeContext = React.createContext('light');
class App extends React.Component {
  render() {
    // 使用一个 Provider 来将当前的 theme 传递给以下的组件树。
    // 无论多深，任何组件都能读取这个值。
    // 在这个例子中，我们将 “dark” 作为当前的值传递下去。
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// 中间的组件再也不必指明往下传递 theme 了。
function Toolbar() {  
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // 指定 contextType 读取当前的 theme context。
  // React 会往上找到最近的 theme Provider，然后使用它的值。
  // 在这个例子中，当前的 theme 值为 “dark”。
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;  }
}
```

**如何使用context**

**React.createContext**

```jsx
const MyContext = React.createContext(defaultValue);
const {Provider, Consumer} = React.createContext(defaultValue);
```

创建一个 Context 对象。当 React 渲染一个订阅了这个 Context 对象的组件，这个组件会从组件树中离自身最近的那个匹配的 `Provider` 中读取到当前的 context 值。

**只有**当组件所处的树中没有匹配到 Provider 时，其 `defaultValue` 参数才会生效。这有助于在不使用 Provider 包装组件的情况下对组件进行测试。注意：将 `undefined` 传递给 Provider 的 value 时，消费组件的 `defaultValue` 不会生效。

**Context.Provider**

```jsx
<MyContext.Provider value={/* 某个值 */}>
```

每个 Context 对象的 Provider React 组件，它允许 Consumer 组件订阅 context 的变化。

Provider 接收一个 `value` 属性，传递给消费组件。一个 Provider 可以和多个消费组件有对应关系。多个 Provider 也可以嵌套使用，里层的会覆盖外层的数据。

当 Provider 的 `value` 值发生变化时，它内部的所有消费组件都会重新渲染。Provider 及其内部 consumer 组件都不受制于 `shouldComponentUpdate` 函数，因此当 consumer 组件在其祖先组件退出更新的情况下也能更新。

**Class.contextType**

```jsx
const MyContext = React.createContext(defaultValue);
class MyClass extends React.Component {
  componentDidMount() {
    let value = this.context;
    /* 在组件挂载完成后，使用 MyContext 组件的值来执行一些有副作用的操作 */
  }
  componentDidUpdate() {
    let value = this.context;
    /* ... */
  }
  componentWillUnmount() {
    let value = this.context;
    /* ... */
  }
  render() {
    let value = this.context;
    /* 基于 MyContext 组件的值进行渲染 */
  }
}
// ES6静态属性只能在类外添加，只能通过类来访问
MyClass.contextType = MyContext;
```

当订阅单一context时，挂载在 class 上的 `contextType` 属性会被重赋值为一个由 `React.createContext()` 创建的 Context 对象。这能让你在组件内使用 `this.context` 来消费这个 Context 上的那个值。你可以在任何生命周期中访问到它，包括 render 函数中。

> 你只通过该 API 订阅单一 context。如果你想订阅多个，阅读[使用多个 Context](https://react.docschina.org/docs/context.html#consuming-multiple-contexts) 章节
>
> 如果你正在使用实验性的 [public class fields 语法](https://babeljs.io/docs/plugins/transform-class-properties/)，你可以使用 `static` 这个类属性来初始化你的 `contextType`。

```jsx
const MyContext = React.createContext(defaultValue);
class MyClass extends React.Component {
  static contextType = MyContext;
  render() {
    let value = this.context;
    /* 基于这个值进行渲染工作 */
  }
}
```

**Context.Consumer**

```jsx
<MyContext.Consumer>
  {value => /* 基于 context 值进行渲染*/}
</MyContext.Consumer>
```

这里，React 组件也可以订阅到 context 变更。这能让你在[函数式组件](https://react.docschina.org/docs/components-and-props.html#function-and-class-components)中完成订阅 context。

这需要[函数作为子元素（function as a child）](https://react.docschina.org/docs/render-props.html#using-props-other-than-render)这种做法。这个函数接收当前的 context 值，返回一个 React 节点。传递给函数的 `value` 值等同于往上组件树离这个 context 最近的 Provider 提供的 `value` 值。如果没有对应的 Provider，`value` 参数等同于传递给 `createContext()` 的 `defaultValue`。

> 注意
>
> 想要了解更多关于 “函数作为子元素（function as a child）” 模式，详见 [render props](https://react.docschina.org/docs/render-props.html)。

**Context.displayName**

context 对象接受一个名为 `displayName` 的 property（起别名），类型为字符串。React DevTools 使用该字符串来确定 context 要显示的内容。

示例，下述组件在 DevTools 中将显示为 MyDisplayName：

```jsx
const MyContext = React.createContext(/* some value */);
MyContext.displayName = 'MyDisplayName';
<MyContext.Provider> // "MyDisplayName.Provider" 在 DevTools 中
<MyContext.Consumer> // "MyDisplayName.Consumer" 在 DevTools 中
```

**例子**

App.js 父组件

```jsx
//App.js
import React from 'react';
import Son from './son';//引入子组件
// 创建一个 theme Context
// 这种解构赋值的方式只适用于单个context，如果是多个context则要用多个变量存储进行区分
export const {Provider,Consumer} = React.createContext("默认名称"); 
export default class App extends React.Component {
    render() {
        let name ="小人头"
        return (
            //Provider共享容器 接收一个name属性
            <Provider value={name}>
                <div>
                    <p>父组件定义的值:{name}</p>
                    <Son />
                </div>
            </Provider>
        );
    }
}
```

son.js 子组件

```jsx
//son.js 子类
import React from 'react';
import { Consumer } from "./index";//引入父组件的Consumer容器
import Grandson from "./grandson.js";//引入孙组件
function Son(props) {
    return (
        //Consumer容器,可以拿到上文传递下来的name属性,并可以展示对应的值
        <Consumer>
            {( name ) =>
                <div>
                    <p>子组件。获取父组件的值:{name}</p>
                    {/* 孙组件内容 */}
                    <Grandson />
               </div>
            }
        </Consumer>
    );
}
export default Son;
```

grandson.js 孙组件

```jsx
//grandson.js 孙类
import React from 'react';
import { Consumer } from "./index";//引入父组件的Consumer容器
function Grandson(props) {
    return (
         //Consumer容器,可以拿到上文传递下来的name属性,并可以展示对应的值
        <Consumer>
            {(name) =>
                   <div>
                   <p>孙组件。获取传递下来的值:{name}</p>
               </div>
            }
        </Consumer>
    );
}
export default Grandson;
```

**context传递函数**

```jsx
// 确保传递给 createContext 的默认值数据结构是调用的组件（consumers）所能匹配的！
const ThemeContext = React.createContext({
  theme: themes.dark,
  toggleTheme: () => {},
});

function ThemeTogglerButton() {
  // Theme Toggler 按钮不仅仅只获取 theme 值，它也从 context 中获取到一个 toggleTheme 函数
  return (
    <ThemeContext.Consumer>
      {({theme, toggleTheme}) => (
        <button onClick={toggleTheme}>
          Toggle Theme
        </button>
      )}
    </ThemeContext.Consumer>
  );
}
```

**消费多个context**

```jsx
// Theme context，默认的 theme 是 “light” 值
const ThemeContext = React.createContext('light');

// 用户登录 context
const UserContext = React.createContext({
  name: 'Guest',
});

class App extends React.Component {
  render() {
    const {signedInUser, theme} = this.props;

    // 提供初始 context 值的 App 组件
    return (
      <ThemeContext.Provider value={'dark'}>
        <UserContext.Provider value={'Jay'}>
        </UserContext.Provider>
      </ThemeContext.Provider>
    );
  }
}

// 一个组件可能会消费多个 context
function Content() {
  return (
    <ThemeContext.Consumer>
      {theme => (
        <UserContext.Consumer>
          {user => (
            <demo user={user} theme={theme} />
          )}
        </UserContext.Consumer>
      )}
    </ThemeContext.Consumer>
  );
}

// 使用
<App>
	<Content></Content>
</App>
```

**注意事项**

因为 context 会使用参考标识（reference identity）来决定何时进行渲染，这里可能会有一些陷阱，当 provider 的父组件进行重渲染时，可能会在 consumers 组件中触发意外的渲染。举个例子，当每一次 Provider 重新渲染时，以下的代码会重渲染所有下面的 consumers 组件，因为 `value` 属性总是被赋值为新的对象：

```jsx
class App extends React.Component {
  render() {
    return (
// 每当 Provider 重新渲染时，value 属性总是被赋值为新的对象，因此Provider下面的所有 consumers 组件都会重新渲染，意思是要将{something: 'something'}用一个变量存储
      <MyContext.Provider value={{something: 'something'}}>
      	<Toolbar />
      </MyContext.Provider>
    );
  }
}
```

为了防止这种情况，将 value 状态提升到父节点的 state 里，只有`this.state.value`改变时，consumer组件才会重新渲染：

```jsx
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: {something: 'something'},    };
  }

  render() {
    return (
      <Provider value={this.state.value}>
        <Toolbar />
      </Provider>
    );
  }
}
```

**谨慎使用 Context**

Context 是跨组件传值的一种方案，但我们需要知道，我们无法阻止 Context 触发的 render。

不像 props 和 state，React 提供了 API 进行浅比较，避免无用的 render，Context 完全没有任何方案可以避免无用的渲染。

有几点关于 Context 的建议：

- Context 只放置必要的，关键的，被大多数组件所共享的状态。
- 对非常昂贵的组件，建议在父级获取 Context 数据，通过 props 传递进来。

##### 错误边界（Error Boundaries）

部分 UI 的 JavaScript 错误不应该导致整个应用崩溃，为了解决这个问题，React 16 引入了一个新的概念 —— 错误边界。

错误边界是一种 React 组件，这种组件**可以捕获并打印发生在其子组件树任何位置的 JavaScript 错误，并且，它会渲染出备用 UI**，而不是渲染那些崩溃了的子组件树。错误边界在渲染期间、生命周期方法和整个组件树的构造函数中捕获错误。

注意

错误边界**无法**捕获以下场景中产生的错误：

- 事件处理（[了解更多](https://react.docschina.org/docs/error-boundaries.html#how-about-event-handlers)）
- 异步代码（例如 `setTimeout` 或 `requestAnimationFrame` 回调函数）
- 服务端渲染
- 它自身抛出来的错误（并非它的子组件）

如果一个 class 组件中定义了 [`static getDerivedStateFromError()`](https://react.docschina.org/docs/react-component.html#static-getderivedstatefromerror) 或 [`componentDidCatch()`](https://react.docschina.org/docs/react-component.html#componentdidcatch) 这两个生命周期方法中的任意一个（或两个）时，那么它就变成一个错误边界。当抛出错误后，请使用 `static getDerivedStateFromError()` 渲染备用 UI ，使用 `componentDidCatch()` 打印错误信息。 

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
	// 此生命周期会在后代组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 state
  static getDerivedStateFromError(error) {    
    // 更新 state 使下一次渲染能够显示降级后的 UI
    return { hasError: true };
  }
  componentDidCatch(error, errorInfo) {
    // 你同样可以将错误日志上报给服务器
    logErrorToMyService(error, errorInfo);
  }
  render() {
    if (this.state.hasError) {
      // 你可以自定义降级后的 UI 并渲染
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children; 
  }
}
```

> getDerivedStateFromError 不能执行副作用，componentDidCatch 可以执行副作用

然后你可以将它作为一个常规组件去使用：

```jsx
<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```

错误边界的工作方式类似于 JavaScript 的 `catch {}`，不同的地方在于错误边界只针对 React 组件。**只有 class 组件才可以成为错误边界组件。**大多数情况下, 你只需要声明一次错误边界组件, 并在整个应用中使用它。

 注意**错误边界仅可以捕获其后代组件的错误**，它无法捕获其自身的错误。如果一个错误边界无法渲染错误信息，则错误会冒泡至最近的上层错误边界，这也类似于 JavaScript 中 catch {} 的工作机制。 

**错误边界应该放置在哪？**

错误边界的粒度由你来决定，可以将其包装在最顶层的路由组件并为用户展示一个 “Something went wrong” 的错误信息，就像服务端框架经常处理崩溃一样。你也可以将单独的部件包装在错误边界以保护应用其他部分不崩溃。

**未捕获错误（Uncaught Errors）的新行为**

这一改变具有重要意义，**自 React 16 起，任何未被错误边界捕获的错误将会导致整个 React 组件树被卸载。**

把一个错误的 UI 留在那比完全移除它要更糟糕。例如，在类似 Messenger 的产品中，把一个异常的 UI 展示给用户可能会导致用户将信息错发给别人。同样，对于支付类应用而言，显示错误的金额也比不呈现任何内容更糟糕。 

**组件栈追踪**

在开发环境下，React 16 会把渲染期间发生的所有错误打印到控制台，即使该应用意外的将这些错误掩盖。除了错误信息和 JavaScript 栈外，React 16 还提供了组件栈追踪。

你也可以在组件栈追踪中查看文件名和行号，这一功能在 Create React App 项目中默认开启 。

如果你没有使用 Create React App，可以手动将[该插件](https://www.npmjs.com/package/babel-plugin-transform-react-jsx-source)添加到你的 Babel 配置中。注意它仅用于开发环境，**在生产环境必须将其禁用** 。 

**关于 try/catch **

`try` / `catch` 很棒但它仅能用于命令式代码（imperative code）：

```js
try {
  showButton();
} catch (error) {
  // ...
}
```

然而，React 组件是声明式的并且具体指出 *什么* 需要被渲染：

```js
<Button />
```

错误边界保留了 React 的声明性质，其行为符合你的预期。例如，即使一个错误发生在 `componentDidUpdate` 方法中，并且由某一个深层组件树的 `setState` 引起，其仍然能够冒泡到最近的错误边界。

**关于事件处理器**

错误边界**无法**捕获事件处理器内部的错误。

React 不需要错误边界来捕获事件处理器中的错误。与 render 方法和生命周期方法不同，事件处理器不会在渲染期间触发。因此，如果它们抛出异常，React 仍然能够知道需要在屏幕上显示什么。

如果你需要在事件处理器内部捕获错误，使用普通的 JavaScript `try` / `catch` 语句。

##### Refs 转发

Ref 转发是一项将 [ref](https://react.docschina.org/docs/refs-and-the-dom.html) 自动地通过组件传递到其一子组件的技巧。 

`ref` 不是 prop 属性。就像 `key` 一样，其被 React 进行了特殊处理。 

**Ref 转发是一个可选特性，其允许某些组件接收 `ref`，并将其向下传递（换句话说，“转发”它）给子组件。** 

```jsx
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 创建 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
// 使用 ref.current 获取这个 <button> DOM 节点
```

以下是对上述示例发生情况的逐步解释：

1. 我们通过调用 `React.createRef` 创建了一个 [React ref](https://react.docschina.org/docs/refs-and-the-dom.html) 并将其赋值给 `ref` 变量。
2. 我们通过指定 `ref` 为 JSX 属性，将其向下传递给 `<FancyButton ref={ref}>`。
3. React 传递 `ref` 给 `forwardRef` 内函数 `(props, ref) => ...`，作为其第二个参数。
4. 我们向下转发该 `ref` 参数到 `<button ref={ref}>`，将其指定为 JSX 属性。
5. 当 ref 挂载完成，`ref.current` 将指向 `<button>` DOM 节点。

>注意
>
>第二个参数 `ref` 只在使用 `React.forwardRef` 定义组件时存在。常规函数和 class 组件不接收 `ref` 参数，且 props 中也不存在 `ref`。
>
>Ref 转发不仅限于 DOM 组件，你也可以转发 refs 到 class 组件实例中。

**在高阶组件中转发 refs**

这个技巧对[高阶组件](https://react.docschina.org/docs/higher-order-components.html)（也被称为 HOC）特别有用。

**高阶组件是参数为组件，返回值为新组件的函数。** 

如果你对 HOC 添加 ref，该 ref 将引用最外层的容器组件，而不是被包裹的组件。 

幸运的是，我们可以使用 `React.forwardRef` API 明确地将 refs 转发到被包裹的组件。`React.forwardRef` 接受一个渲染函数，其接收 `props` 和 `ref` 参数并返回一个 React 节点。例如： 

```jsx
function logProps(Component) {
  class LogProps extends React.Component {
    componentDidUpdate(prevProps) {
      console.log('old props:', prevProps);
      // 因为 ref 和 key 不是 prop 属性，所以不会被打印
      console.log('new props:', this.props);
    }

    render() {
      const {forwardedRef, ...rest} = this.props;
      // 在子组件中转发ref
      return <Component ref={forwardedRef} {...rest} />;    }
  }
	// React会将高阶组件受到的props和ref传递给forwardRef内函数 (props, ref) => ...
  // 注意 React.forwardRef 回调的第二个参数 “ref”。
  // 利用自定义属性“forwardedRef”接收ref
  // 然后它就可以被挂载到被 LogProps 包裹的子组件上。
  return React.forwardRef((props, ref) => {
    return <LogProps {...props} forwardedRef={ref} />;
  });
}
```

**在 DevTools 中显示自定义名称（了解）**

如果你命名了渲染函数，DevTools 也将包含其名称（例如 “*ForwardRef(myFun)*”）：

```jsx
const WrappedComponent = React.forwardRef(
  function myFun(props, ref) {
    return <LogProps {...props} forwardedRef={ref} />;
  }
);
```

你甚至可以设置函数的 `displayName` 属性来包含被包裹组件的名称：

```jsx
function logProps(Component) {
  class LogProps extends React.Component {
    // ...
  }

  function forwardRef(props, ref) {
    return <LogProps {...props} forwardedRef={ref} />;
  }

  // 在 DevTools 中为该组件提供一个更有用的显示名。
  // 例如 “ForwardRef(logProps(MyComponent))”
  const name = Component.displayName || Component.name;  forwardRef.displayName = `logProps(${name})`;
  return React.forwardRef(forwardRef);
}
```

##### Fragments

React 中的一个常见模式是一个组件返回多个元素。Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

```jsx
render() {
  return (
    <React.Fragment>
      <ChildA />
      <ChildB />
      <ChildC />
    </React.Fragment>
  );
}
```

**动机**

一种常见模式是组件返回一个子元素列表。以此 React 代码片段为例：

```jsx
class Table extends React.Component {
  render() {
    return (
      <table>
        <tr>
          <Columns />
        </tr>
      </table>
    );
  }
}
```

`<Columns />` 需要返回多个 `<td>`元素以使渲染的 HTML 有效。如果在 `<Columns />` 的 `render()` 中使用了父 div，则生成的 `<div>` 将无效。

```jsx
class Columns extends React.Component {
  render() {
    return (
      <div>
        <td>Hello</td>
        <td>World</td>
      </div>
    );
  }
}
```

得到一个 `<Table>` 输出：

```html
<table>
  <tr>
    <div>
      <td>Hello</td>
      <td>World</td>
    </div>
  </tr>
</table>
```

Fragments 解决了这个问题。

**用法**

```jsx
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
      	<td>Hello</td>
        <td>World</td>
      </React.Fragment>
      );
  }
}
```

这样可以正确的输出 `<Table>`：

```html
<table>
  <tr>
    <td>Hello</td>
    <td>World</td>
  </tr>
</table>
```

**短语法**

你可以使用一种新的，且更简短的语法来声明 Fragments。它看起来像空标签：

```jsx
class Columns extends React.Component {
  render() {
    return (
      <>
      	<td>Hello</td>
        <td>World</td>
      </>
    );
  }
}
```

你可以像使用任何其他元素一样使用 `<> </> `，但这种短语法不支持key、ref 和属性。 

**带 key 的 Fragments**

使用显式 `<React.Fragment>` 语法声明的片段可能具有 key。一个使用场景是将一个集合映射到一个 Fragments 数组 - 举个例子，创建一个描述列表：

```jsx
function Glossary(props) {
  return (
    <dl>
      {props.items.map(item => (
        // 没有`key`，React 会发出一个关键警告
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
}
```

`key` 是唯一可以传递给 `Fragment` 的属性。未来我们可能会添加对其他属性的支持，例如事件。

##### 高阶组件

高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。

具体而言，**高阶组件是参数为组件，返回值为新组件的函数。**

组件是将 props 转换为 UI，而高阶组件是将组件转换为另一个组件。

HOC 在 React 的第三方库中很常见。

例如，假设有一个 `CommentList` 组件，它订阅外部数据源，用以渲染评论列表：

```jsx
class CommentList extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      // 假设 "DataSource" 是个全局范围内的数据源变量
      comments: DataSource.getComments()
    };
  }
  
  componentDidMount() {
    // 订阅更改
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // 清除订阅
    DataSource.removeChangeListener(this.handleChange);
  }
  
  handleChange() {
    // 当数据源更新时，更新组件状态
    this.setState({
      comments: DataSource.getComments()
    });
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}
```

稍后，编写了一个用于订阅单个博客帖子的组件，该帖子遵循类似的模式：

```jsx
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {
      blogPost: DataSource.getBlogPost(props.id)
    };
  }

  componentDidMount() {
    // 订阅更改
    DataSource.addChangeListener(this.handleChange);
  }

  componentWillUnmount() {
    // 清除订阅
    DataSource.removeChangeListener(this.handleChange);
  }
  
  handleChange() {
    this.setState({
      blogPost: DataSource.getBlogPost(this.props.id)
    });
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```

`CommentList` 和 `BlogPost` 不同 - 它们在 `DataSource` 上调用不同的方法，且渲染不同的结果。但它们的大部分实现都是一样的 。

 可以想象，在一个大型应用程序中，这种订阅 `DataSource` 和调用 `setState` 的模式将一次又一次地发生。我们需要一个抽象，允许我们在一个地方定义这个逻辑，并在许多组件之间共享它。这正是高阶组件擅长的地方（抽取公共部分，接口）。 

 对于订阅了 `DataSource` 的组件，比如 `CommentList` 和 `BlogPost`，我们可以编写高阶组件：

```jsx
// 此函数接收一个组件...
function withSubscription(WrappedComponent, selectData) {
  // ...并返回另一个组件...
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.handleChange = this.handleChange.bind(this);
      this.state = {
        data: selectData(DataSource, props)
      };
    }
    
    componentDidMount() {
      // 订阅更改
      DataSource.addChangeListener(this.handleChange);
    }

    componentWillUnmount() {
      // 清除订阅
      DataSource.removeChangeListener(this.handleChange);
    }
  
    handleChange() {
      this.setState({
        data: selectData(DataSource, this.props)
      });
    }

    render() {
      // ... 并使用新数据渲染被包装的组件!
      // 请注意，我们可能还会传递其他属性
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```
被包装组件接收来自容器组件的所有 prop，同时也接收一个新的用于 render 的 data 属性。HOC 不需要关心数据的使用方式或原因，而被包装组件也不需要关心数据是怎么来的。

```jsx
// 修改 CommentList 结构
class CommentList extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      comments: this.props.data
    };
  }

  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}

// 修改 BlogPost 结构
class BlogPost extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      blogPost: this.props.data
    };
  }

  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
```

```jsx
// withSubscription 第一个参数是被包装组件。第二个参数是获取数据的方法，通过 DataSource 和当前的 props 返回我们需要的数据。
// 容器组件
const CommentListWithSubscription = withSubscription(
  CommentList,
  (DataSource) => DataSource.getComments()
);

const BlogPostWithSubscription = withSubscription(
  BlogPost,
  (DataSource, props) => DataSource.getBlogPost(props.id)
);
// 使用
<CommentListWithSubscription 自定义属性 />
<BlogPostWithSubscription 自定义属性 />
```

当然也可以使用ES7装饰器语法（实验性）

```js
npm install -D @babel/plugin-proposal-decorators
```

```jsx
// 装饰器工厂
function withSubscription(DataSource,selectData) {
  // 返回的这个函数才是真正的装饰器
  return function(WrappedComponent){
    // ...并返回另一个class组件...
    return class extends React.Component {
      constructor(props) {
        super(props);
        this.handleChange = this.handleChange.bind(this);
        this.state = {
          data: selectData(DataSource, props)
        };
      }

      componentDidMount() {
        // 订阅更改
        DataSource.addChangeListener(this.handleChange);
      }

      componentWillUnmount() {
        // 清除订阅
        DataSource.removeChangeListener(this.handleChange);
      }

      handleChange() {
        this.setState({
          data: selectData(DataSource, this.props)
        });
      }

      render() {
        // ... 并使用新数据渲染被包装的组件!
        // 请注意，我们可能还会传递其他属性
        return <WrappedComponent data={this.state.data} {...this.props} />;
      }
    };
  }
}

// 也可以是函数组件的形式
// 装饰器工厂
/*
function withSubscription(DataSource,selectData) {
  // 返回的这个函数才是真正的装饰器
  return function(WrappedComponent){
    // ...并返回另一个函数组件...
    return props=>{
      // do somthing...
      return <WrappedComponent {...props} />;
    }
  }
}
*/

@withSubscription(DataSource,selectData)
class CommentList extends React.Component {
	// ...
  render() {
    return (
      <div>
        {this.state.comments.map((comment) => (
          <Comment comment={comment} key={comment.id} />
        ))}
      </div>
    );
  }
}

@withSubscription(DataSource,selectData)
class BlogPost extends React.Component {
	// ...
  render() {
    return <TextBlock text={this.state.blogPost} />;
  }
}
// 使用
<CommentList 自定义属性 />
<BlogPost 自定义属性 />
```



请注意，HOC 不会修改传入的组件，也不会使用继承来复制其行为。相反，HOC 通过将组件包装在容器组件中来组成新组件。HOC 是纯函数，没有副作用。 

不要试图在 HOC 中修改传入组件的原型对象（或以其他方式改变传入组件）：

```jsx
function logProps(InputComponent) {
  InputComponent.prototype.componentDidUpdate = function(prevProps) {
    console.log('Current props: ', this.props);
    console.log('Previous props: ', prevProps);
  };
  // 返回原始的 input 组件，暗示它已经被修改。
  return InputComponent;
}

// 每次调用 logProps 时，增强组件都会有 log 输出。
const EnhancedComponent = logProps(InputComponent);
```

**约定：将不相关的 props 传递给被包裹的组件**

HOC 为组件添加特性。自身不应该大幅改变约定。HOC 返回的组件与原组件应保持类似的接口。

HOC 应该透传与自身无关的 props。

这种约定保证了 HOC 的灵活性以及可复用性。 

**不要在 render 方法中使用 HOC**

React 的 diff 算法（称为协调）使用组件标识来确定它是应该更新现有子树还是将其丢弃并挂载新子树。 如果从 `render` 返回的组件与前一个渲染中的组件相同（`===`），则 React 通过将子树与新子树进行区分来递归更新子树。 如果它们不相等，则完全卸载前一个子树。

这不仅仅是性能问题 - 重新挂载组件会导致该组件及其所有子组件的状态丢失。

如果在组件之外创建 HOC，这样一来组件只会创建一次。因此，每次 render 时都会是同一个组件。

在极少数情况下，你需要动态调用 HOC。你可以在组件的生命周期方法或其构造函数中进行调用。

**务必复制静态方法**

 当你将 HOC 应用于组件时，原始组件将使用容器组件进行包装。这意味着新组件没有原始组件的任何静态方法。 

为了解决这个问题，你可以在返回之前把这些方法拷贝到容器组件上：

```js
function enhance(WrappedComponent) {
  // 容器组件Enhance
  class Enhance extends React.Component {/*...*/}
  // 必须准确知道应该拷贝哪些方法
  Enhance.staticMethod = WrappedComponent.staticMethod;
  return Enhance;
}
```

**Refs 不会被传递**

虽然高阶组件的约定是将所有 props 传递给被包装组件，但这对于 refs 并不适用。那是因为 `ref` 实际上并不是一个 prop ，就像 `key` 一样，它是由 React 专门处理的。如果将 ref 添加到 HOC 的返回组件中，则 ref 引用指向容器组件，而不是被包装组件。

这个问题的解决方案是通过使用 `React.forwardRef` API（React 16.3 中引入）。

##### import组件

当export default 组件时，import时即引入对应的组件模块

例如

```js
// MyComponent.jsx
export default MyComponent 

// other.jsx
import MyComponent from 'MyComponent'  // MyComponent即为对应的模块，其实是模块中的default的值
// or
const MyComponentModule = await import ('./MyComponent')  // 动态引入MyComponent组件模块，其中MyComponentModule包含模块的default属性（export default 导出的 default）

```

##### 强制刷新组件

- key属性值的改变

> 适用于函数组件和类组件

- ref + component.forceUpdate(callback)

> 适用于类组件，类组件支持传入ref属性

##### 与第三方库协调同

React 不会理会 React 自身之外的 DOM 操作。它根据内部虚拟 DOM 来决定是否需要更新，而且如果同一个 DOM 节点被另一个库操作了，React 会觉得困惑而且没有办法恢复。 

避免冲突的最简单方式就是防止 React 组件更新。你可以渲染无需更新的 React 元素，比如一个空的 `<div />`。

以 jQuery 为例：

为了防止 React 在挂载之后去触碰这个 DOM，我们会从 `render()` 函数返回一个空的 `<div />`。这个 `<div />` 元素既没有属性也没有子元素，所以 React 没有理由去更新它，使得 jQuery 插件可以自由的管理这部分的 DOM： 

```jsx
class SomePlugin extends React.Component {
  componentDidMount() {
    this.$el = $(this.el);
    this.$el.somePlugin();
  }

  componentWillUnmount() {
    this.$el.somePlugin('destroy');
  }
	// ref 如果是回调函数，这个函数中接受 React 组件实例或 HTML DOM 元素作为参数
  render() {
    return <div ref={el => this.el = el} />;
  }
}
```

注意我们同时定义了 `componentDidMount` 和 `componentWillUnmount` [生命周期函数](https://react.docschina.org/docs/react-component.html#the-component-lifecycle)。许多 jQuery 插件绑定事件监听到 DOM 上，所以在 `componentWillUnmount` 中注销监听是很重要的。如果这个插件没有提供一个用于清理的方法，你很可能会需要自己来提供一个，为了避免内存泄漏要记得把所有插件注册的监听都移除掉。



##### 深入JSX

实际上，JSX 仅仅只是 `React.createElement(component, props, ...children)` 函数的语法糖。如下 JSX 代码：

```JSX
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>
```

会编译为：

```JSX
React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```

如果没有子节点，你还可以使用自闭合的标签形式，如：

```js
<div className="sidebar" />
```

**指定 React 元素类型**

JSX 标签的第一部分指定了 React 元素的类型。

大写字母开头的 JSX 标签意味着它们是 React 组件。这些标签会被编译为对命名变量的直接引用，当你使用 JSX `<Foo />` 表达式时，`Foo` 必须包含在作用域内。

**React 必须在作用域内**

由于 JSX 会编译为 `React.createElement` 调用形式，所以 `React` 库也必须包含在 JSX 代码作用域内。

例如，在如下代码中，虽然 `React` 和 `CustomButton` 并没有被直接使用，但还是需要导入：

```jsx
import React from 'react';
import CustomButton from './CustomButton';
function WarningButton() {
  // return React.createElement(CustomButton, {color: 'red'}, null);
  return <CustomButton color="red" />;
}
```

如果你不使用 JavaScript 打包工具而是直接通过 `` 标签加载 React，则必须将 `React` 挂载到全局变量中。

**在 JSX 类型中使用点语法**

在 JSX 中，你也可以使用点语法来引用一个 React 组件。当你在一个模块中导出许多 React 组件时，这会非常方便。例如，如果 `MyComponents.DatePicker` 是一个组件，你可以在 JSX 中直接使用：

```jsx
import React from 'react';

const MyComponents = {
  DatePicker: function DatePicker(props) {
    return <div>Imagine a {props.color} datepicker here.</div>;
  }
}

function BlueDatePicker() {
  return <MyComponents.DatePicker color="blue" />;
}
```

**用户定义的组件必须以大写字母开头**

以小写字母开头的元素代表一个 HTML 内置组件，比如 `` 或者 `` 会生成相应的字符串 `'div'` 或者 `'span'` 传递给 `React.createElement`（作为参数）。大写字母开头的元素则对应着在 JavaScript 引入或自定义的组件，如 `<Foo />` 会编译为 `React.createElement(Foo)`。

我们建议使用大写字母开头命名自定义组件。如果你确实需要一个以小写字母开头的组件，则在 JSX 中使用它之前，必须将它赋值给一个大写字母开头的变量。

**在运行时选择类型**

你不能将通用表达式作为 React 元素类型。如果你想通过通用表达式来（动态）决定元素类型，你需要首先将它赋值给大写字母开头的变量。这通常用于根据 prop 来渲染不同组件的情况下:

```json
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 错误！JSX 类型不能是一个表达式。
  return <components[props.storyType] story={props.story} />;
}
```

要解决这个问题, 需要首先将类型赋值给一个大写字母开头的变量：

```jsx
import React from 'react';
import { PhotoStory, VideoStory } from './stories';

const components = {
  photo: PhotoStory,
  video: VideoStory
};

function Story(props) {
  // 正确！JSX 类型可以是大写字母开头的变量。
  const SpecificStory = components[props.storyType];
  return <SpecificStory story={props.story} />;
}
```

**JSX 中的 Props**

有多种方式可以在 JSX 中指定 props。

**JavaScript 表达式作为 Props**

你可以把包裹在 `{}` 中的 JavaScript 表达式作为一个 prop 传递给 JSX 元素。例如，如下的 JSX：

```jsx
<MyComponent foo={1 + 2 + 3 + 4} />
```

`if` 语句以及 `for` 循环不是 JavaScript 表达式，所以不能在 JSX 中直接使用。但是，你可以用在 JSX 以外的代码中。比如：

```jsx
function NumberDescriber(props) {
  let description;
  if (props.number % 2 == 0) {
    description = <strong>even</strong>;
  } else {
    description = <i>odd</i>;
  }
  return <div>{props.number} is an {description} number</div>;
}
```

**字符串字面量**

你可以将字符串字面量赋值给 prop. 如下两个 JSX 表达式是等价的：

```jsx
<MyComponent message="hello world" />

<MyComponent message={'hello world'} />
```

转义，以下两个 JSX 表达式是等价的：

```jsx
<MyComponent message="&lt;3" />

<MyComponent message={'<3'} />
```

**Props 默认值为 “True”**

如果你没给 prop 赋值，它的默认值是 `true`。以下两个 JSX 表达式是等价的：

```jsx
<MyTextBox autocomplete />

<MyTextBox autocomplete={true} />
```

**支持扩展运算符...传递props**

还可以选择只保留当前组件需要接收的 props，并使用展开运算符将其他 props 传递下去。

```jsx
const Button = props => {
  const { kind, ...other } = props;
  const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
  return <button className={className} {...other} />;
};
```

**JSX 中的子元素**

包含在开始和结束标签之间的 JSX 表达式内容将作为特定属性 `props.children` 传递给外层组件。有几种不同的方法来传递子元素：

**字符串字面量**

你可以将字符串放在开始和结束标签之间，此时 `props.children` 就只是该字符串。这对于很多内置的 HTML 元素很有用。例如：

```jsx
<MyComponent>Hello world!</MyComponent>
```

JSX 会移除行首尾的空格以及空行。与标签相邻的空行均会被删除，文本字符串之间的新行会被压缩为一个空格。因此以下的几种方式都是等价的：

```html
<div>Hello World</div>

<div>
  Hello World
</div>

<div>
  Hello
  World
</div>

<div>

  Hello World
</div>
```

**JSX 子元素**

子元素允许由多个 JSX 元素组成。这对于嵌套组件非常有用：

```jsx
<MyContainer>
  <MyFirstComponent />
  <MySecondComponent />
</MyContainer>
```

React 组件也能够返回存储在数组中的一组元素：

```jsx
render() {
  // 不需要用额外的元素包裹列表元素！
  return [
    // 不要忘记设置 key :)
    <li key="A">First item</li>,
    <li key="B">Second item</li>,
    <li key="C">Third item</li>,
  ];
}
```

 **JavaScript 表达式可以被包裹在 `{}` 中作为子元素。** 

```jsx
function Item(props) {
  return <li>{props.message}</li>;}

function TodoList() {
  const todos = ['finish doc', 'submit pr', 'nag dan to review'];
  return (
    <ul>
      {todos.map((message) => <Item key={message} message={message} />)}    </ul>
  );
}
```

**函数作为子元素**

通常，JSX 中的 JavaScript 表达式将会被计算为字符串、React 元素或者是列表。不过，`props.children` 和其他 prop 一样，它可以传递任意类型的数据，而不仅仅是 React 已知的可渲染类型。例如，如果你有一个自定义组件，你可以把回调函数作为 `props.children` 进行传递

```jsx
// 调用子元素回调 numTimes 次，来重复生成组件
function Repeat(props) {
  let items = [];
  for (let i = 0; i < props.numTimes; i++) {
    items.push(props.children(i)); // props.children = {}里的回调函数
  }
  return <div>{items}</div>;
}

function ListOfTenThings() {
  return (
    <Repeat numTimes={10}>
      {(index) => <div key={index}>This is item {index} in the list</div>}
    </Repeat>
  );
}
```

**布尔类型、Null 以及 Undefined 将会忽略**

`false`, `null`, `undefined`, and `true` 是合法的子元素。但它们并不会被渲染。以下的 JSX 表达式渲染结果相同：

```jsx
<div />

<div></div>

<div>{false}</div>

<div>{null}</div>

<div>{undefined}</div>

<div>{true}</div>
```

这有助于依据特定条件来渲染其他的 React 元素。例如，在以下 JSX 中，仅当 `showHeader` 为 `true` 时，才会渲染 `<Header>` 组件：

```jsx
<div>
  {showHeader && <Header />}  <Content />
</div>
```
值得注意的是有一些 “falsy” 值，如数字 0，空字符串等，仍然会被 React 渲染。例如，以下代码并不会像你预期那样工作，因为当 props.messages 是空数组时，0 仍然会被渲染：
```jsx
<div>
  {props.messages.length &&
  <MessageList messages={props.messages} />
  }
</div>
```

要解决这个问题，确保 `&&` 之前的表达式总是布尔值：

```jsx
<div>
  {props.messages.length > 0 &&
  <MessageList messages={props.messages} />
  }
</div>
```

如果你想渲染 `false`、`true`、`null`、`undefined` 等值，你需要使用`String(value)`等方法先将它们转换为字符串。

##### 在React中使用TypeSscript

###### [@types/react提供的接口类型（接口）](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react/index.d.ts)

###### [@types/react-dom提供的类型（接口）](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react-dom/index.d.ts)

###### [@types/react-router-dom提供的类型（接口）](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react-router-dom/index.d.ts)

###### [@types/react-redux提供的类型（接口）](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react-redux/index.d.ts)

###### 创建一个React的TS项目

```sh
npx create-react-app ts-demo --typescript
```

将 [TypeScript](https://www.typescriptlang.org/) 添加到 Create React App 项目，请先安装它：

```bash
$ npm install --save typescript @types/node @types/react @types/react-dom @types/jest
$ # 或者
$ yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

###### 组件类型

```ts
React.ComponentType<P = {}> = React.ComponentClass<P> | React.FunctionComponent<P>
```

###### 编写一个类组件

类组件可以接受props,并且每个类组件都有state数据,所以他需要有两个数据类型规范

**子组件**

```tsx
import React from 'react'

interface IState {
	title:string
}

interface IProps {
    count: number
  	// CSSProperties提供样式声明的类型信息
    // 用户传入style的时候就能够获得类型检查和代码补全
    // style?: React.CSSProperties;
    // 用于函数组件，使用@types/react提供的事件类型定义，React.MouseEvent<T>，T是元素类型
    // onClick?(event: React.MouseEvent<HTMLButtonElement>): void;
    // ...
}

class Child extends React.PureComponent<IProps, IState> {
  // ES7 写法
	state = {
    	title: 'ts'
	}
	render(){
        return <div>
          {this.state.title}
        	{this.props.count}
        </div>
    }
}

export default Child
```

**父组件**

```tsx
import React from 'react'
import Child from './child'

interface IState {
	count: number
}

interface Iprops {}

class Parents extends React.PureComponent<Iprops, IState> {
	state = {
		count: 0
	}
	render(){
        return <Child count={count} />
    }
}
```

###### 编写一个函数组件

由于函数组件的state使用的是钩子一个一个勾进来的 ，所以他就需要一个泛型

**子组件**

```tsx
import React from 'react'

interface Iprops {
	count: number
}
// 1.React.FC是React函数组件的泛型接口，用 React.FC 声明 React函数组件 类型与定义 Props 参数类型。React.FC可以写成React.FunctionComponent。
// 2.React.FC 包含了 PropsWithChildren 的泛型，不用显式的声明 props.children 的类型。props.children是render Prop时需要给children声明类型，而不是使用默认的React.ReactNode。React.FC<> 对于返回类型是显式的，而普通函数版本是隐式的（否则需要附加注释）。
// 3.React.FC提供了类型检查和自动完成的静态属性：displayName，propTypes和defaultProps（注意：defaultProps与React.FC结合使用会存在一些问题）。
// 4.我们使用React.FC来写 React 组件的时候，是不能用setState的，取而代之的是useState()、useEffect等 Hook API。
// 5.React.FC<T>不传参数的时候就用｛｝，即React.FC<{}>或直接使用 React.FC，因为{}是默认参数
const Parent:React.FC<Iprops> = props => {
    const { count } = props;
    return <div>{count}</div>
}
```

**父组件**

```tsx
import React, { useState } from 'react'
import Child from './child'

interface Iprops {}

const Child:React.FC<Iprops> = () => {
    const [count,setCount] = useState<number>(0)
    return <div>
        	<button onClick={()=>setCount(count+1)}>+1</button>
        	<Child count={count} />
        </div>
}

export default Child
```

######  React.FC 声明函数组件和**普通声明**以及 **PropsWithChildren** 的区别是：

- React.FC 显式地定义了返回类型，其他方式是隐式推导的

- React.FC 对静态属性：displayName、propTypes、defaultProps 提供了类型检查和自动补全
- React.FC 为 children 提供了隐式的类型（ReactElement | null），但是目前，提供的类型存在**一些 issue**[6]（问题）

比如以下用法 React.FC 会报类型错误:

```tsx
const App: React.FC = props => props.children
const App: React.FC = () => [1, 2, 3]
const App: React.FC = () => 'hello'
```

解决方法:

```tsx
const App: React.FC<{}> = props => props.children as any
const App: React.FC<{}> = () => [1, 2, 3] as any
const App: React.FC<{}> = () => 'hello' as any
// 或者
const App: React.FC<{}> = props => (props.children as unknown) as JSX.Element
const App: React.FC<{}> = () => ([1, 2, 3] as unknown) as JSX.Element
const App: React.FC<{}> = () => ('hello' as unknown) as JSX.Element
```

在通常情况下，使用 **React.FC** 的方式声明最简单有效，推荐使用；如果出现类型不兼容问题，建议使用**以下两种方式：**

第二种：使用 **PropsWithChildren**，这种方式可以为你省去频繁定义 children 的类型，自动设置 children 类型为 ReactNode:

```tsx
type AppProps = React.PropsWithChildren<{ message: string }>
const App = ({ message, children }: AppProps) => (
  <div>
    {message}
    {children}
  </div>
)
```

第三种：直接声明:

```tsx
type AppProps = {
  message: string
  children?: React.ReactNode
}

const App = ({ message, children }: AppProps) => (
  <div>
    {message}
    {children}
  </div>
)
```

###### 在TS中使用react-router

当我们使用`Route`组件或者使用`withRouter`的时候,都会给组件绑定`history,location,match`三个属性,但是创建函数组件时，props默认是没有属性的，需要手动引入`history,location,match`三个属性，`RouteComponentProps`是一个对象的类型，其包含了这些属性的值的类型。

```ts
import React from 'react'
import { withRouter,RouteComponentProps } from 'react-router-dom'

interface Iprops extends RouteComponentProps{}

const App:React.FC<Iprops> = props => {
    console.log(props.pathname) // 有提示,并且props有他的类型规定
    return <div></div>
}

export default withRouter(App)
```

###### 在TS中使用Redux

这里面我选择使用 `react-redux,redux-thunk,redux`

```ts
import { DispatchProp } from 'react-redux' // 获取仅包含dispatch属性的对象的类型
import { Dispatch } from 'redux' // 获取 redux 中dispatch函数的类型 
const a:React.FC<DispatchProp> = function({dispatch}){
  // ...
}
```



**reducer**

```ts
// import { AnyAction } from 'redux'
// Iactions === AnyAction
interface Iactions {
    type:string;
    [propName: string]: any;
}
export interface Istate {
    count: number
}

const defaultState:Istate {
    count: 0
}

export default (state = defaultState, action: Iactions): Istate => {
    swtich(action.type){
        case 'add':
        	return {...state,count: state.count+1}
        default: 
        	return state;
    }
}
```

**combineReducer**

```ts
import { combineReducers } from "redux";
import User from "./reudcer";

export default combineReducers({
  User,
});
```

**store**

```ts
import { createStore, compose, applyMiddleware } from "redux";
import reducer from "./reudcers";
import thunk from "redux-thunk";
import { Istate } from "./reudcers/user"; //这个是为了在react-redux中的state设置

// __REDUX_DEVTOOLS_EXTENSION_COMPOSE__是Redux DevTools Extension的compose，开发用
const composeEnhancers = (window as any).__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;

const enhancer = composeEnhancers(applyMiddleware(thunk));

export interface StoreState {
  User: Istate;
}

const store = createStore(reducer, enhancer);

export default store;
```

> 注意: 这个地方会报错,说windows上面没有`__REDUX_DEVTOOLS_EXTENSION_COMPOSE__`这个属性

在根目录下面创建`types`目录,再创建`index.d.ts`

```ts
// 修改ReduxTools工具
interface Window extends REDUXTOOS {
  __REDUX_DEVTOOLS_EXTENSION_COMPOSE__:
    | string
    | __REDUX_DEVTOOLS_EXTENSION_COMPOSE__;
}

declare var window: Window;
```

**组件上**

```ts
import React from 'react'
import { connect } from 'react-redux'
import { StoreState } from 'src/store'   //引入store中导出的state数据类型
import { setUserInfo } from 'src/store/action'

const App:React.FC<{}> = props => {
    const { count } = props;
    return <div>{count}</div>
}

export default connect(
  (state: StoreState) => ({
      count: state.User.count
  }),
  (dispatch: any) => {
    return {
      addCount(count: number) {
        dispatch(setUserInfo(count));
      },
    };
})(App)
```

###### 引入第三方包

在引入第三方包文件的时候 比如`react-redux`的时候

`import {connect} from 'react-redux'`会发现报错

当我们鼠标停留在react-redux上面的时候,会提示`npm install @types/react-redux`当我们安装完成之后,我们的项目文件才算完整。

###### CSS和函数组件事件

```ts
interface Iprops {
  	// CSSProperties提供样式声明的类型信息
    // 用户传入style的时候就能够获得类型检查和代码补全
    style?: React.CSSProperties;
    // 用于函数组件，使用@types/react提供的事件类型定义，React.MouseEvent<T>，T是元素类型
    onClick?(event: React.MouseEvent<HTMLButtonElement>): void;
    // ...
}
```

###### defaultProps

如果你需要定义defaultProps，那么不要使用React.FC，因为[React.FC对defaultProps的支持不是很好](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet#typing-defaultprops)：

```TS
const defaultProps = {
  who: "Johny Five"
};
type IProps = { age: number } & typeof defaultProps;

export const Greet = (props: IProps) => { return <div>123</div> };
Greet.defaultProps = defaultProps;
```

事实上，[一个提议在函数组件中废弃defaultProps的React rfc已经被接受](https://github.com/reactjs/rfcs/blob/createlement-rfc/text/0000-create-element-changes.md#deprecate-defaultprops-on-function-components)，所以以后还是尽量减少在函数组件上使用defaultProps，使用ES6原生的参数解构+默认参数特性就已经能够满足需要：

```tsx
interface Props{
  foo:string
}
const TestFunction: FunctionComponent<Props> = ({ foo = "bar" }) => <div>{foo}</div>
```

###### 元素类型

```
HTMLDivElement  HTMLButtonElement  HTMLInputElement  ...
```

###### 可渲染节点类型

可渲染节点就是：可以直接被组件渲染函数返回的值。

与可渲染节点有关的类型定义如下（摘录自[@types/react](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/8a1b68be3a64e5d2aa1070f68cc935d668a976ad/types/react/index.d.ts#L187)）：

```ts
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;
interface ReactNodeArray extends Array<ReactNode> {}
type ReactFragment = {} | ReactNodeArray;
type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;
```

###### 组件类型

- `React.FC<Props>`（即 `React.FunctionComponent<Props>`）

- `React.Component<Props, State>`

- `React.ComponentType<Props>`（即`ComponentClass<P> | FunctionComponent<P>`）
  在写HOC的时候经常用到。

  ```ts
  const withState = <P extends WrappedComponentProps>(
    WrappedComponent: React.ComponentType<P>,
  ) => { 
  	// ...
  }
  ```

###### Event 事件对象类型

@types/react提供了各种事件的类型

常用 Event 事件对象类型：

- FormEvent 表单事件对象

- ClipboardEvent 剪贴板事件对象
- DragEvent 拖拽事件对象
- ChangeEvent Change 事件对象
- KeyboardEvent 键盘事件对象
- MouseEvent 鼠标事件对象
- TouchEvent 触摸事件对象
- WheelEvent 滚轮事件对象
- AnimationEvent 动画事件对象
- TransitionEvent 过渡事件对象

```ts
import { 
MouseEvent,
FormEvent,
// ... 
} from 'react'
```

> 更多事件请点击 react 进入库中查看

比如以下是使用`React.FormEvent`的例子：

```ts
class App extends React.Component<{},{text: string}> {
  state = {
    text: '',
  }
  onChange = (e: React.FormEvent<HTMLInputElement>): void => {
    this.setState({ text: e.currentTarget.value })
  }
  render() {
    return (
      <div>
        <input type="text" value={this.state.text} onChange={this.onChange} />
      </div>
    )
  }
}
```

在React中，所有事件（包括[FormEvent](https://reactjs.org/docs/events.html#form-events)、[KeyboardEvent](https://reactjs.org/docs/events.html#keyboard-events)、[MouseEvent](https://reactjs.org/docs/events.html#mouse-events)等）都是[SyntheticEvent](https://reactjs.org/docs/events.html)的子类型。他们在@types/react中定义如下：

```ts
// DOM事件的基本属性都定义在这里
interface BaseSyntheticEvent<E = object, C = any, T = any> {
  nativeEvent: E;
  currentTarget: C;
  target: T;
  bubbles: boolean;
  cancelable: boolean;
  defaultPrevented: boolean;
  eventPhase: number;
  isTrusted: boolean;
  preventDefault(): void;
  isDefaultPrevented(): boolean;
  stopPropagation(): void;
  isPropagationStopped(): boolean;
  persist(): void;
  timeStamp: number;
  type: string;
}
interface SyntheticEvent<T = Element, E = Event> extends BaseSyntheticEvent<E, EventTarget & T, EventTarget> {}

// 具体的事件类型：
interface FormEvent<T = Element> extends SyntheticEvent<T> {}
interface KeyboardEvent<T = Element> extends SyntheticEvent<T, NativeKeyboardEvent> {
  altKey: boolean;
  // ...
```

###### Promise类型

Promise 是一个泛型类型，T 泛型变量用于确定使用 then 方法时接收的第一个回调函数（onfulfilled）的参数的类型。

```ts
interface IResponse<T> {
  message: string,
  result: T,
  success: boolean,
}
async function getResponse (): Promise<IResponse<number[]>> {
  return {
    message: '获取成功',
    result: [1, 2, 3],
    success: true,
  }
}
getResponse()
  .then(response => {
    console.log(response.result)
  })
```

###### 获取并扩展原生元素的props类型

比如，以下例子获取并扩展了`<button>`的props类型：

```js
export const PrimaryButton = (
  props: Props & React.HTMLProps<HTMLButtonElement>
) => <Button size={ButtonSizes.default} {...props} />;
// React.SVGProps<SVGElement> 获取 SVG 元素的类型
```

PrimaryButton能够接受所有原生`<button>`所接受的props。关键在于`React.HTMLProps`。

###### 获取并扩展第三方组件的props类型

```js
import { Button } from "library"; // but doesn't export ButtonProps! oh no!
type ButtonProps = React.ComponentProps<typeof Button>; // no problem! grab your own!
type AlertButtonProps = Omit<ButtonProps, "onClick">; // modify
const AlertButton: React.FC<AlertButtonProps> = props => (
  <Button onClick={() => alert("hello")} {...props} />
);
```

##### Portal（子节点渲染到存在于父组件以外的 DOM 节点）

 Portal 提供了一种将子节点渲染到存在于父组件以外的 DOM 节点的优秀的方案。 

```jsx
ReactDOM.createPortal(child, container)
```

第一个参数（`child`）是任何[可渲染的 React 子元素](https://react.docschina.org/docs/react-component.html#render)，例如一个元素，字符串或 fragment。第二个参数（`container`）是一个 DOM 元素。

**用法**

```jsx
render() {
  // React 并*没有*创建一个新的 div。它只是把子元素渲染到 `domNode` 中。
  // `domNode` 是一个可以在任何位置的有效 DOM 节点。
  return ReactDOM.createPortal(
    this.props.children,
    domNode  );
}
```

 一个 portal 的典型用例是当父组件有 `overflow: hidden` 或 `z-index` 样式时，但你需要子组件能够在视觉上“跳出”其容器。例如，对话框、悬浮卡以及提示框 。

**通过 Portal 进行事件冒泡**

尽管 portal 可以被放置在 DOM 树中的任何地方，但在任何其他方面，其行为和普通的 React 子节点行为一致。由于 portal 仍存在于 *React 树*， 且与 *DOM 树* 中的位置无关，那么无论其子节点是否是 portal，像 context 这样的功能特性都是不变的。

 这包含事件冒泡。一个从 portal 内部触发的事件会一直冒泡至包含 *React 树*的祖先，即便这些元素并不是 *DOM 树* 中的祖先。

##### Profiler（测试用，了解）

`Profiler` 测量渲染一个 React 应用多久渲染一次以及渲染一次的“代价”。 它的目的是识别出应用中渲染较慢的部分，或是可以使用[类似 memoization 优化](https://react.docschina.org/docs/hooks-faq.html#how-to-memoize-calculations)的部分，并从相关优化中获益。 

>注意：
>
>Profiling 增加了额外的开支，所以**它在[生产构建](https://react.docschina.org/docs/optimizing-performance.html#use-the-production-build)中会被禁用**。

**用法**

`Profiler` 能添加在 React 树中的任何地方来测量树中这部分渲染所带来的开销。 它需要两个 prop ：一个是 `id`(string)，一个是当组件树中的组件“提交”更新的时候被React调用的回调函数 `onRender`(function)。

例如，为了分析 `Navigation` 组件和它的子代：

```jsx
render(
  <App>
    <Profiler id="Navigation" onRender={callback}>      <Navigation {...props} />
    </Profiler>
    <Main {...props} />
  </App>
);
```

嵌套使用 `Profiler` 组件来测量相同一个子树下的不同组件。

```jsx
render(
  <App>
    <Profiler id="Panel" onRender={callback}>
      <Panel {...props}>
        <Profiler id="Content" onRender={callback}>
          <Content {...props} />
        </Profiler>
        <Profiler id="PreviewPane" onRender={callback}>
          <PreviewPane {...props} />
        </Profiler>
      </Panel>
    </Profiler>
  </App>
);
```

> 注意
>
> 尽管 `Profiler` 是一个轻量级组件，我们依然应该在需要时才去使用它。对一个应用来说，每添加一些都会给 CPU 和内存带来一些负担。

**`onRender` 回调**

`Profiler` 需要一个 `onRender` 函数作为参数。 React 会在 profiler 包含的组件树中任何组件 “提交” 一个更新的时候调用这个函数。 它的参数描述了渲染了什么和花费了多久。

```js
function onRenderCallback(
  id, // 发生提交的 Profiler 树的 “id”
  phase, // "mount" （如果组件树刚加载） 或者 "update" （如果它重渲染了）之一
  actualDuration, // 本次更新 committed 花费的渲染时间
  baseDuration, // 估计不使用 memoization 的情况下渲染整颗子树需要的时间
  startTime, // 本次更新中 React 开始渲染的时间
  commitTime, // 本次更新中 React committed 的时间
  interactions // 属于本次更新的 interactions 的集合
) {
  // 合计或记录渲染时间。。。
}
```

让我们来仔细研究一下各个 prop:

- **`id: string`** - 发生提交的 `Profiler` 树的 `id`。 如果有多个 profiler，它能用来分辨树的哪一部分发生了“提交”。
- **`phase: "mount" | "update"`** - 判断是组件树的第一次装载引起的重渲染，还是由 props、state 或是 hooks 改变引起的重渲染。
- **`actualDuration: number`** - 本次更新在渲染 `Profiler` 和它的子代上花费的时间。 这个数值表明使用 memoization 之后能表现得多好。（例如 [`React.memo`](https://react.docschina.org/docs/react-api.html#reactmemo)，[`useMemo`](https://react.docschina.org/docs/hooks-reference.html#usememo)，[`shouldComponentUpdate`](https://react.docschina.org/docs/hooks-faq.html#how-do-i-implement-shouldcomponentupdate)）。 理想情况下，由于子代只会因特定的 prop 改变而重渲染，因此这个值应该在第一次装载之后显著下降。
- **`baseDuration: number`** - 在 `Profiler` 树中最近一次每一个组件 `render` 的持续时间。 这个值估计了最差的渲染时间。（例如当它是第一次加载或者组件树没有使用 memoization）。
- **`startTime: number`** - 本次更新中 React 开始渲染的时间戳。
- **`commitTime: number`** - 本次更新中 React commit 阶段结束的时间戳。 在一次 commit 中这个值在所有的 profiler 之间是共享的，可以将它们按需分组。
- **`interactions: Set`** - 当更新被制定时，[“interactions”](https://fb.me/react-interaction-tracing) 的集合会被追踪。（例如当 `render` 或者 `setState` 被调用时）。

##### 不使用 ES6

你可以使用 `create-react-class` 模块：

```JSX
var createReactClass = require('create-react-class');
var Greeting = createReactClass({
  render: function() {
    return <h1>Hello, {this.props.name}</h1>;
  }
});
```

ES6 中的 class 与 `createReactClass()` 方法十分相似，但有以下几个区别值得注意。

**声明默认属性**

无论是函数组件还是 class 组件，都拥有 `defaultProps` 属性：

```jsx
class Greeting extends React.Component {
  // ...
}

Greeting.defaultProps = {
  name: 'Mary'
};
```

如果使用 `createReactClass()` 方法创建组件，如果需要声明默认属性，那就需要在组件中定义 `getDefaultProps()` 函数：

```jsx
var Greeting = createReactClass({
  getDefaultProps: function() {
    return {
      name: 'Mary'
    };
  },

  // ...

});
```

**初始化 State**

如果使用 ES6 的 class 关键字创建组件，你可以通过给 `this.state` 赋值的方式来定义组件的初始 state：

```jsx
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: props.initialCount};
  }
  // ...
}
```

如果使用 `createReactClass()` 方法创建组件，你需要提供一个单独的 `getInitialState` 方法，让其返回初始 state：

```jsx
var Counter = createReactClass({
  getInitialState: function() {
    return {count: this.props.initialCount};
  },
  // ...
});
```

**自动绑定**

对于使用 ES6 的 class 关键字创建的 React 组件，组件中的方法遵循与常规 ES6 class 相同的语法规则。这意味着这些方法不会自动绑定 `this` 到这个组件实例。 你需要在 constructor 中显式地调用 `.bind(this)`：

```jsx
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
    // 这一行很重要！
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    alert(this.state.message);
  }

  render() {
    // 由于 `this.handleClick` 已经绑定至实例，因此我们才可以用它来处理点击事件
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}
```

如果使用 `createReactClass()` 方法创建组件，组件中的方法会自动绑定至实例，所以不需要像上面那样做：

```jsx
var SayHello = createReactClass({
  getInitialState: function() {
    return {message: 'Hello!'};
  },

  handleClick: function() {
    alert(this.state.message);
  },

  render: function() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
});
```

这就意味着，如果使用 ES6 class 关键字创建组件，在处理事件回调时就要多写一部分代码。但对于大型项目来说，这样做可以提升运行效率。

如果你觉得上述写法很繁琐，那么可以尝试使用**目前还处于试验性阶段**的 Babel 插件 [Class Properties](https://babeljs.io/docs/plugins/transform-class-properties/)。

```jsx
class SayHello extends React.Component {
  constructor(props) {
    super(props);
    this.state = {message: 'Hello!'};
  }
  // 警告：这种语法还处于试验性阶段！
  // 在这里使用箭头函数就可以把方法绑定给实例：
  handleClick = () => {
    alert(this.state.message);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Say hello
      </button>
    );
  }
}
```

为了保险起见，以下三种做法都是可以的：

- 在 constructor 中绑定方法。
- 使用箭头函数，比如：`onClick={(e) => this.handleClick(e)}`。
- 继续使用 `createReactClass`。





##### Mixins

> **注意：**
>
> ES6 本身是不包含任何 mixin 支持。因此，当你在 React 中使用 ES6 class 时，将不支持 mixins 。因此在使用 `createReactClass`才能使用 mixin 。
>
> **我们也发现了很多使用 mixins 然后出现了问题的代码库。[并且不建议在新代码中使用它们](https://react.docschina.org/blog/2016/07/13/mixins-considered-harmful.html)。**

如果完全不同的组件有相似的功能，这就会产生[“横切关注点（cross-cutting concerns）“问题](https://en.wikipedia.org/wiki/Cross-cutting_concern)。针对这个问题，在使用 createReactClass 创建 React 组件的时候，引入 `mixins` 功能会是一个很好的解决方案。

比较常见的用法是，组件每隔一段时间更新一次。使用 `setInterval()` 可以很容易实现这个功能，但需要注意的是，当你不再需要它时，你应该清除它以节省内存。React 提供了[生命周期方法](https://react.docschina.org/docs/working-with-the-browser.html#component-lifecycle)，这样你就可以知道一个组件何时被创建或被销毁了。让我们创建一个简单的 mixin，它使用这些方法提供一个简单的 `setInterval()` 函数，它会在组件被销毁时被自动清理。

使用 mixin，需要你对 mixin 的内容足够了解。

```jsx
var SetIntervalMixin = {
  componentWillMount: function() {
    this.intervals = [];
  },
  setInterval: function() {
    this.intervals.push(setInterval.apply(null, arguments));
  },
  componentWillUnmount: function() {
    this.intervals.forEach(clearInterval);
  }
};

var createReactClass = require('create-react-class');

var TickTock = createReactClass({
  mixins: [SetIntervalMixin], // 使用 mixin
  getInitialState: function() {
    return {seconds: 0};
  },
  componentDidMount: function() {
    this.setInterval(this.tick, 1000); // 调用 mixin 上的方法
  },
  tick: function() {
    this.setState({seconds: this.state.seconds + 1});
  },
  render: function() {
    return (
      <p>
        React has been running for {this.state.seconds} seconds.
      </p>
    );
  }
});

ReactDOM.render(
  <TickTock />,
  document.getElementById('example')
);
```

如果组件拥有多个 mixins，且这些 mixins 中定义了相同的生命周期方法（例如，当组件被销毁时，几个 mixins 都想要进行一些清理工作），那么这些生命周期方法都会被调用的。使用 mixins 时，mixins 会先按照定义时的顺序执行，最后调用组件上对应的方法。



##### Diffing

当对比两颗树时，React 首先比较两棵树的根节点。不同类型的根节点元素会有不同的形态。

**比对不同类型的元素**

当根节点为不同类型的元素时，React 会拆卸原有的树并且建立起新的树。举个例子，当一个元素从 `<a>` 变成 `<img>`，从 `<Article>` 变成 `<Comment>`，或从 `<Button>` 变成 `<div>` 都会触发一个完整的重建流程。

当拆卸一棵树时，对应的 DOM 节点也会被销毁。组件实例将执行 `componentWillUnmount()` 方法。当建立一棵新的树时，对应的 DOM 节点会被创建以及插入到 DOM 中。组件实例将执行 `componentWillMount()` 方法，紧接着 `componentDidMount()` 方法。所有跟之前的树所关联的 state 也会被销毁。

在根节点以下的组件也会被卸载，它们的状态会被销毁。比如，当比对以下更变时：

```jsx
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

React 会销毁 `Counter` 组件并且重新装载一个新的组件。

**比对同一类型的元素**

当比对两个相同类型的 React 元素时，React 会保留 DOM 节点，仅比对及更新有改变的属性。比如：

```jsx
<div className="before" title="stuff" />

<div className="after" title="stuff" />
```

通过比对这两个元素，React 知道只需要修改 DOM 元素上的 `className` 属性。

当更新 `style` 属性时，React 仅更新有所更变的属性。比如：

```jsx
<div style={{color: 'red', fontWeight: 'bold'}} />

<div style={{color: 'green', fontWeight: 'bold'}} />
```

 在处理完当前节点之后，React 继续对子节点进行递归。 

**比对同类型的组件元素**

当一个组件更新时，组件实例保持不变，这样 state 在跨越不同的渲染时保持一致。React 将更新该组件实例的 props 以跟最新的元素保持一致，并且调用该实例的 `componentWillReceiveProps()` 和 `componentWillUpdate()` 方法。

下一步，调用 `render()` 方法，diff 算法将在之前的结果以及新的结果中进行递归。

**对子节点进行递归**

在默认条件下，当递归 DOM 节点的子元素时，React 会同时遍历两个子元素的列表；当产生差异时，生成一个 mutation。

在子元素列表末尾新增元素时，更变开销比较小。比如：

```jsx
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>
```

React 会先匹配两个 `first` 对应的树，然后匹配第二个元素 `second` 对应的树，最后插入第三个元素的 `third` 树。

如果简单实现的话，那么在列表头部插入会很影响性能，那么更变开销会比较大。比如：

```jsx
<ul>
  <li>Duke</li>
  <li>Villanova</li>
</ul>

<ul>
  <li>Connecticut</li>
  <li>Duke</li>
  <li>Villanova</li>
</ul>
```

React 会针对每个子元素 mutate 而不是保持相同的 `Duke` 和 `Villanova` 子树完成。这种情况下的低效可能会带来性能问题。

##### keys

为了解决以上问题，React 支持 `key` 属性。当子元素拥有 key 时，React 使用 key 来匹配原有树上的子元素以及最新树上的子元素。以下例子在新增 `key` 之后使得之前的低效转换变得高效：

```jsx
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>
```

现在 React 知道只有带着 `'2014'` key 的元素是新元素，带着 `'2015'` 以及 `'2016'` key 的元素仅仅移动了。

现实场景中，产生一个 key 并不困难。你要展现的元素可能已经有了一个唯一 ID，于是 key 可以直接从你的数据中提取：

```jsx
<li key={item.id}>{item.name}</li>
```

当以上情况不成立时，你可以新增一个 ID 字段到你的模型中，或者利用一部分内容作为哈希值来生成一个 key。这个 key 不需要全局唯一，但在列表中需要保持唯一。

最后，你也可以使用元素在数组中的下标作为 key。这个策略在元素不进行重新排序时比较合适，但一旦有顺序修改，diff 就会变得慢。

当基于下标的组件进行重新排序时，组件 state 可能会遇到一些问题。由于组件实例是基于它们的 key 来决定是否更新以及复用，如果 key 是一个下标，那么修改顺序时会修改当前的 key，导致非受控组件的 state（比如输入框）可能相互篡改导致无法预期的变动。

Key 应该具有稳定，可预测，以及列表内唯一的特质。不稳定的 key（比如通过 `Math.random()` 生成的）会导致许多组件实例和 DOM 节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失。

![diff-key](http://jay_ohhh.gitee.io/imagehosting/react/diff_key.png)

添加key属性有时会提高性能：例如当key值非index时，数组的增删操作，例如上图中的在数组里插入新元素。如果key值是index时，反而会降低性能。

**为什么遍历列表时，key最好不要用 index?**

1、虚拟D0M中key的作用:

当状态中的数据发生变化时, react会根据【新数据】生成【新的虚拟DOM】

随后 React进行【新虚拟DOM】与【旧虚拟DOM】的diff比较,比较规则如下：

- 旧虚拟DOM中找到了与新虚拟D0M相同的key :

（1）若虚拟DOM中内容没变,直接使用之前的真实D0M

（2）若虚拟D0M中内容变了,则生成新的真实DOM，随后替换掉页面中之前的真实DOM

- 虚拟D中未找到与新虚拟D0M相同的key：

根据数据创建新的真实DOM，渲染到到页面

 

2、用 index作为key可能会引发的问题：

（1）若对数据进行：逆序添加、逆序删除等破坏顺序的操作

会产生没有必要的真实DOM更新，效率低

（2）如果DOM是输入型表单类

会产生错误DOM更新==>界面有问题

（3）注意!如果不存在对数据的逆序添加、逆序删除等破坏顺序操作

仅用于渲染列表展示，使用 index作为key是没有问题的

 

3.开发中如何选择key?

（1）最好使用每条数据的唯一标识作为key,比如id、手机号、身份证号、学号等唯一值

（2）如果确定只是简单的展示数据,用 index也是可以的



##### Refs

Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。 

**创建 Refs**

Refs 是使用 `React.createRef()` 创建的，并通过 `ref` 属性附加到 React 元素。在构造组件时，通常将 Refs 分配给实例属性，以便可以在整个组件中引用它们。

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

**访问 Refs**

当 ref 被传递给 `render` 中的元素时，对该节点的引用可以在 ref 的 `current` 属性中被访问。

```js
const node = this.myRef.current;
```

ref 的值根据节点的类型而有所不同：

- 当 `ref` 属性用于 HTML 元素时，构造函数中使用 `React.createRef()` 创建的 `ref` 接收底层 DOM 元素作为其 `current` 属性。
- 当 `ref` 属性用于自定义 class 组件时，`ref` 对象接收组件的挂载实例作为其 `current` 属性。
- **你不能在函数组件（标签）上使用 `ref` 属性**，因为他们没有实例。
- forwardRef 返回的组件（没有定义是class组件还是函数组件）能够使用 `ref` 属性，该组件ref属性接受的 React.createRef() 会传递到其组件树下的另一个组件中。

**为 DOM 元素添加 ref**

以下代码使用 `ref` 去存储 DOM 节点的引用：

```jsx
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);
    // 创建一个 ref 来存储 textInput 的 DOM 元素
    this.textInput = React.createRef();
    this.focusTextInput = this.focusTextInput.bind(this);
  }

  focusTextInput() {
    // 直接使用原生 API 使 text 输入框获得焦点
    // 注意：我们通过 "current" 来访问 DOM 节点
    this.textInput.current.focus();  }

  render() {
    // 告诉 React 我们想把 <input> ref 关联到
    // 构造器里创建的 `textInput` 上
    return (
      <div>
        <input
          type="text"
          ref={this.textInput} />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

React 会在组件挂载时给 `current` 属性传入 DOM 元素，并在组件卸载时传入 `null` 值。`ref` 会在 `componentDidMount` 或 `componentDidUpdate` 生命周期钩子触发前更新。

**为 class 组件添加 Ref**

如果我们想包装上面的 `CustomTextInput`，来模拟它挂载之后立即被点击的操作，我们可以使用 ref 来获取这个自定义的 input 组件并手动调用它的 `focusTextInput` 方法：

```jsx
class AutoFocusTextInput extends React.Component {
  constructor(props) {
    super(props);
    this.textInput = React.createRef();
  }

  componentDidMount() {
    this.textInput.current.focusTextInput();
  }

  render() {
    return (
      <CustomTextInput ref={this.textInput} />
    );
  }
}
```

请注意，这仅在 `CustomTextInput` 声明为 class 时才有效：

```jsx
class CustomTextInput extends React.Component {
  // ...
}
```

如果要在函数组件中使用 `ref`，你可以使用 [`forwardRef`](https://react.docschina.org/docs/forwarding-refs.html)（可与 [`useImperativeHandle`](https://react.docschina.org/docs/hooks-reference.html#useimperativehandle) 结合使用），或者可以将该组件转化为 class 组件。

你可以**在函数组件内部使用 `ref` 属性**，只要它指向一个 DOM 元素或 class 组件：

```jsx
function CustomTextInput(props) {
  // 这里必须声明 textInput，这样 ref 才可以引用它  
  const textInput = useRef(null);
  function handleClick() {
    textInput.current.focus();
  }

  return (
    <div>
      <input
        type="text"
        ref={textInput} />
      <input
        type="button"
        value="Focus the text input"
        onClick={handleClick}
      />
    </div>
  );
}
```

**将 DOM Refs 暴露给父组件**

在极少数情况下，你可能希望在父组件中引用子节点的 DOM 节点。通常不建议这样做，因为它会打破组件的封装，但它偶尔可用于触发焦点或测量子 DOM 节点的大小或位置。

虽然你可以[向子组件添加 ref](https://react.docschina.org/docs/refs-and-the-dom.html#adding-a-ref-to-a-class-component)，但这不是一个理想的解决方案，因为你只能获取组件实例而不是 DOM 节点。并且，它还在函数组件上无效。

如果你使用 16.3 或更高版本的 React, 这种情况下我们推荐使用 [ref 转发](https://react.docschina.org/docs/forwarding-refs.html)。**Ref 转发使组件可以像暴露自己的 ref 一样暴露子组件的 ref**。

**回调 Refs**

React 也支持另一种设置 refs 的方式，称为“回调 refs”。它能助你更精细地控制何时 refs 被设置和解除。

不同于传递 `createRef()` 创建的 `ref` 属性，你会传递一个函数。这个函数中接受 React 组件实例或 HTML DOM 元素作为参数，以使它们能在其他地方被存储和访问。

下面的例子描述了一个通用的范例：使用 `ref` 回调函数，在实例的属性中存储对 DOM 节点的引用。

```jsx
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;
    this.setTextInputRef = element => {
      this.textInput = element;
    };
    this.focusTextInput = () => {
      // 使用原生 DOM API 使 text 输入框获得焦点
      if (this.textInput) this.textInput.focus();
    };
  }

  componentDidMount() {
    // 组件挂载后，让文本框自动获得焦点
    this.focusTextInput();
  }

  render() {
    // 使用 `ref` 的回调函数将 text 输入框 DOM 节点的引用存储到 React
    // 实例上（比如 this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
          />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
          />
      </div>
    );
  }
}
```

React 将在组件挂载时，会调用 `ref` 回调函数并传入 DOM 元素，当卸载时调用它并传入 `null`。在 `componentDidMount` 或 `componentDidUpdate` 触发前，React 会保证 refs 一定是最新的。

你可以在组件间传递回调形式的 refs，就像你可以传递通过 `React.createRef()` 创建的对象 refs 一样。

```jsx
function CustomTextInput(props) {
  return (
    <div>
      <input ref={props.inputRef} />
    </div>
  );
}

class Parent extends React.Component {
  render() {
    return (
      <CustomTextInput
        inputRef={el => this.inputElement = el}
      />
    );
  }
}
```

在上面的例子中，`Parent` 把它的 refs 回调函数当作 `inputRef` props 传递给了 `CustomTextInput`，而且 `CustomTextInput` 把相同的函数作为特殊的 `ref` 属性传递给了 `<input>`。结果是，在 `Parent` 中的 `this.inputElement` 会被设置为与 `CustomTextInput` 中的 `input` 元素相对应的 DOM 节点。

**过时 API：String 类型的 Refs**

如果你之前使用过 React，你可能了解过之前的 API 中的 string 类型的 ref 属性，例如 `"textInput"`。你可以通过 `this.refs.textInput` 来访问 DOM 节点。我们不建议使用它，因为 string 类型的 refs 存在 [一些问题](https://github.com/facebook/react/pull/8333#issuecomment-271648615)。它已过时并可能会在未来的版本被移除。

> 注意
>
> 如果你目前还在使用 `this.refs.textInput` 这种方式访问 refs ，我们建议用[回调函数](https://react.docschina.org/docs/refs-and-the-dom.html#callback-refs)或 [`createRef` API](https://react.docschina.org/docs/refs-and-the-dom.html#creating-refs) 的方式代替。



**关于回调 refs 的说明**

如果 `ref` 回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数 `null`，然后第二次会传入参数 DOM 元素或 React 组件实例。这是因为在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 ref 并且设置新的。通过将 ref 的回调函数定义成 class 的绑定函数（constructor内绑定）的方式可以避免上述问题，但是大多数情况下它是无关紧要的。

##### Render Prop（共享代码）

render prop 指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术

具有 render prop 的组件接受一个函数，该函数返回一个 React 元素并调用它而不是实现自己的渲染逻辑。

```jsx
<DataProvider render={data => (
  <h1>Hello {data.target}</h1>
)}/>
```

**使用 Render Props 来解决横切关注点（Cross-Cutting Concerns）**

>**横切关注点**
>
>指的是一些具有横越多个模块的行为 。
>
>关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。 
>
>横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。 

组件是 React 代码复用的主要单元，但如何分享一个组件封装到其他需要相同 state 组件的状态或行为并不总是很容易。

例如，以下组件跟踪 Web 应用程序中的鼠标位置：

```jsx
class MouseTracker extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>
        <h1>移动鼠标!</h1>
        <p>当前的鼠标位置是 ({this.state.x}, {this.state.y})</p>
      </div>
    );
  }
}
```

当光标在屏幕上移动时，组件在 `<p>` 中显示其（x，y）坐标。

现在的问题是：我们如何在另一个组件中复用这个行为？换个说法，若另一个组件需要知道鼠标位置，我们能否封装这一行为，以便轻松地与其他组件共享它？？

由于组件是 React 中最基础的代码复用单元，现在尝试重构一部分代码使其能够在 `` 组件中封装我们需要共享的行为。

```jsx
// <Mouse> 组件封装了我们需要的行为...
class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>

        {/* ...但我们如何渲染 <p> 以外的东西? */}
        <p>The current mouse position is ({this.state.x}, {this.state.y})</p>
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <>
        <h1>移动鼠标!</h1>
        <Mouse />
      </>
    );
  }
}
```

现在 `<Mouse>` 组件封装了所有关于监听 `mousemove` 事件和存储鼠标 (x, y) 位置的行为，但其仍不是真正的可复用。

举个例子，假设我们有一个 `<Cat>` 组件，它可以呈现一张在屏幕上追逐鼠标的猫的图片。我们或许会使用 `<Cat mouse={{x,y}}>` prop 来告诉组件鼠标的坐标以让它知道图片应该在屏幕哪个位置。

首先, 你或许会像这样，尝试在 `<Mouse>` 内部的渲染方法渲染 `<Cat>` 组件：

```jsx
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class MouseWithCat extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>

        {/*
          我们可以在这里换掉 <p> 的 <Cat>   ......
          但是接着我们需要创建一个单独的 <MouseWithSomethingElse>
          每次我们需要使用它时，<MouseWithCat> 是不是真的可以重复使用.
        */}
        <Cat mouse={this.state} />
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>移动鼠标!</h1>
        <MouseWithCat />
      </div>
    );
  }
}
```

这种方法适用于我们的特定用例，但我们还没有达到以可复用的方式真正封装行为的目标。现在，每当我们想要鼠标位置用于不同的用例时，我们必须创建一个新的组件（本质上是另一个 `<MouseWithCat>` ），它专门为该用例呈现一些东西.

这也是 render prop 的来历：我们可以提供一个带有函数 prop 的 `<Mouse>` 组件，它能够动态决定什么需要渲染的，而不是将 `<Cat>` 硬编码到 `<Mouse>` 组件里，并有效地改变它的渲染结果。

```jsx
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>移动鼠标!</h1>
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```

现在，我们提供了一个 `render` 方法 让 `<Mouse>` 能够动态决定什么需要渲染，而不是克隆 `<Mouse>` 组件然后硬编码来解决特定的用例。

更具体地说，**render prop 是一个用于告知组件需要渲染什么内容的函数 prop。**

这项技术使我们共享行为非常容易。要获得这个行为，只要渲染一个带有 `render` prop 的 `<Mouse>` 组件就能够告诉它当前鼠标坐标 (x, y) 要渲染什么。

关于 render prop 一个有趣的事情是你可以使用带有 render prop 的常规组件来实现大多数[高阶组件](https://react.docschina.org/docs/higher-order-components.html) (HOC)。 你可以使用带有 render prop 的常规 `<Mouse>` 轻松创建一个：

```jsx
function withMouse(Component) {
  return class extends React.Component {
    render() {
      return (
        <Mouse render={mouse => (
          <Component {...this.props} mouse={mouse} />
        )}/>
      );
    }
  }
}
```

**使用 Props 而非 `render`**

重要的是要记住，render prop 是因为模式才被称为 *render* prop ，你不一定要用名为 `render` 的 prop 来使用这种模式。事实上， [*任何*被用于告知组件需要渲染什么内容的函数 prop 在技术上都可以被称为 “render prop”](https://cdb.reacttraining.com/use-a-render-prop-50de598f11ce).

尽管之前的例子使用了 `render`，我们也可以简单地使用 `children` prop！

```jsx
<Mouse children={mouse => (
  <p>鼠标的位置是 {mouse.x}，{mouse.y}</p>
)}/>
```

`children` prop 并不真正需要添加到 JSX 元素的 “attributes” 列表中。你可以直接放置到元素的*内部*！

```jsx
<Mouse>
  {mouse => (
    <p>鼠标的位置是 {mouse.x}，{mouse.y}</p>
  )}
</Mouse>
```

`<Mouse>`组件 render 函数内的 {this.props.render(this.state)} 改为  {this.props.children(this.state)} 即可。

**Render Props 与 React.PureComponent 一起使用时要小心**

如果你在 render 方法里创建函数，那么使用 render prop （每次渲染都会生成一个新的函数，prop在浅层上发生了变化，除非该函数始终是同一个函数）会抵消使用 [`React.PureComponent`](https://react.docschina.org/docs/react-api.html#reactpurecomponent) 带来的优势。因为浅比较 props 的时候总会得到 false，并且在这种情况下每一个 `render` 对于 render prop 将会生成一个新的值。

例如，继续我们之前使用的 `<Mouse>` 组件，如果 `Mouse` 继承自 `React.PureComponent` 而不是 `React.Component`，我们的例子看起来就像这样：

```jsx
class Mouse extends React.PureComponent {
  // 与上面相同的代码......
}

class MouseTracker extends React.Component {
  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>

        {/*
          这是不好的！
          每个渲染的 `render` prop的值将会是不同的。
        */}
        <Mouse render={mouse => (
          <Cat mouse={mouse} />
        )}/>
      </div>
    );
  }
}
```

在这样例子中，每次 `<MouseTracker>` 渲染，它会生成一个新的函数作为 `<Mouse render>` 的 prop，因而在同时也抵消了继承自 `React.PureComponent` 的 `<Mouse>` 组件的效果！

为了绕过这一问题，有时你可以定义一个 prop 作为实例方法，类似这样：

```jsx
class MouseTracker extends React.Component {
  // 定义为实例方法：`this.renderTheCat`
  // 当我们在渲染中使用它时，它指的是相同的函数
  renderTheCat(mouse) {
    return <Cat mouse={mouse} />;
  }

  render() {
    return (
      <div>
        <h1>Move the mouse around!</h1>
        <Mouse render={this.renderTheCat} />
      </div>
    );
  }
}
```

如果你无法静态定义 prop（例如，因为你需要关闭组件的 props 和/或 state），则 `<Mosue>` 应该扩展 `React.Component`。

##### 静态类型检查

完成以下步骤，便可开始使用 TypeScript：

- 将 TypeScript 添加到你的项目依赖中。
- 配置 TypeScript 编译选项
- 使用正确的文件扩展名
- 为你使用的库添加定义

下面让我们详细地介绍一下这些步骤：

**在 Create React App 中使用 TypeScript**

Create React App 内置了对 TypeScript 的支持。

需要创建一个使用 TypeScript 的**新项目**，在终端运行：

```
npx create-react-app my-app --template typescript
```

如需将 TypeScript 添加到**现有的 Create React App 项目**中，[请参考此文档](https://facebook.github.io/create-react-app/docs/adding-typescript).

> 注意：
>
> 如果你使用的是 Create React App，可以跳过本节的其余部分。其余部分讲述了不使用 Create React App 脚手架，手动配置项目的用户。

**添加 TypeScript 到现有项目中**

这一切都始于在终端中执行的一个命令。

如果你使用 [Yarn](https://yarnpkg.com/)，执行：

```
yarn add --dev typescript
```

如果你使用 [npm](https://www.npmjs.com/)，执行：

```
npm install --save-dev typescript
```

恭喜！你已将最新版本的 TypeScript 安装到项目中。安装 TypeScript 后我们就可以使用 `tsc` 命令。在配置编译器之前，让我们将 `tsc` 添加到 `package.json` 中的 “scripts” 部分：

```json
{
  // ...
  "scripts": {
    "build": "tsc",    // ...
  },
  // ...
}
```

**配置 TypeScript 编译器**

没有配置项，编译器提供不了任何帮助。在 TypeScript 里，这些配置项都在一个名为 `tsconfig.json` 的特殊文件中定义。可以通过执行以下命令生成该文件：

如果你使用 [Yarn](https://yarnpkg.com/)，执行：

```
yarn run tsc --init
```

如果你使用 [npm](https://www.npmjs.com/)，执行：

```
npx tsc --init
```

`tsconfig.json` 文件中，有许多配置项用于配置编译器。查看所有配置项的的详细说明，[请参考此文档](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html)。

我们来看一下 `rootDir` 和 `outDir` 这两个配置项。编译器将从项目中找到 TypeScript 文件并编译成相对应 JavaScript 文件。但我们不想混淆源文件和编译后的输出文件。

为了解决该问题，我们将执行以下两个步骤：

- 首先，让我们重新整理下项目目录，把所有的源代码放入 `src` 目录中。

```
├── package.json
├── src
│   └── index.ts
└── tsconfig.json
```

- 其次，我们将通过配置项告诉编译器源码和输出的位置。

```json
// tsconfig.json

{
  "compilerOptions": {
    // ...
    "rootDir": "src",    "outDir": "build"    // ...
  },
}
```

很好！现在，当我们运行构建脚本时，编译器会将生成的 javascript 输出到 `build` 文件夹。 [TypeScript React Starter](https://github.com/Microsoft/TypeScript-React-Starter/blob/master/tsconfig.json) 提供了一套默认的 `tsconfig.json` 帮助你快速上手：

```json
{
  "compilerOptions": {
    "outDir": "build/dist",
    "module": "commonjs",
    "target": "es5",
    "lib": ["es6", "dom"],
    "sourceMap": true,
    "allowJs": true,
    "jsx": "react",
    "moduleResolution": "node",
    "rootDir": "src",
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noImplicitAny": true,
    "strictNullChecks": true
  },
  "exclude": [
    "node_modules",
    "build",
    "scripts",
    "acceptance-tests",
    "webpack",
    "jest",
    "src/setupTests.ts"
  ],
  "types": [
    "typePatches"
  ]
}
```

通常情况下，你不希望将编译后生成的 JavaScript 文件保留在版本控制内。因此，应该把`build`文件夹添加到 `.gitignore` 中。

**文件扩展名**

在 React 中，你的组件文件大多数使用 `.js` 作为扩展名。在 TypeScript 中，提供两种文件扩展名：

`.ts` 是默认的文件扩展名，而 `.tsx` 是一个用于包含 `JSX` 代码的特殊扩展名。

**运行 TypeScript**

如果你按照上面的说明操作，现在应该能运行 TypeScript 了。

```
yarn build
```

如果你使用 npm，执行：

```
npm run build
```

如果你没有看到输出信息，这意味着它编译成功了。

**类型定义**

为了能够显示来自其他包的错误和提示，编译器依赖于声明文件。声明文件提供有关库的所有类型信息。这样，我们的项目就可以用上像 npm 这样的平台提供的三方 JavaScript 库。

获取一个库的声明文件有两种方式：

**Bundled** - 该库包含了自己的声明文件。这样很好，因为我们只需要安装这个库，就可以立即使用它了。要知道一个库是否包含类型，看库中是否有 `index.d.ts` 文件。有些库会在 `package.json` 文件的 `typings` 或 `types` 属性中指定类型文件。

**[DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)** - DefinitelyTyped 是一个庞大的声明仓库，为没有声明文件的 JavaScript 库提供类型定义。这些类型定义通过众包的方式完成，并由微软和开源贡献者一起管理。例如，React 库并没有自己的声明文件。但我们可以从 DefinitelyTyped 获取它的声明文件。只要执行以下命令。

```
# yarn
yarn add --dev @types/react
```
```
# npm
npm i --save-dev @types/react
```

**局部声明** 有时，你要使用的包里没有声明文件，在 DefinitelyTyped 上也没有。在这种情况下，我们可以创建一个本地的定义文件。因此，在项目的根目录中创建一个 `declarations.d.ts` 文件。一个简单的声明可能是这样的：

```ts
declare module 'querystring' {
  export function stringify(val: object): string
  export function parse(val: string): object
}
```

##### 严格模式

 `StrictMode` 是一个用来突出显示应用程序中潜在问题的工具。与 `Fragment` 一样，`StrictMode` 不会渲染任何可见的 UI。它为其后代元素触发额外的检查和警告。 

> 注意：
>
> 严格模式检查仅在开发模式下运行；*它们不会影响生产构建*。

你可以为应用程序的任何部分启用严格模式。例如：

```jsx
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

在上述的示例中，*不*会对 `Header` 和 `Footer` 组件运行严格模式检查。但是，`ComponentOne` 和 `ComponentTwo` 以及它们的所有后代元素都将进行检查。

`StrictMode` 目前有助于：

- [识别不安全的生命周期](https://react.docschina.org/docs/strict-mode.html#identifying-unsafe-lifecycles)
- [关于使用过时字符串 ref API 的警告](https://react.docschina.org/docs/strict-mode.html#warning-about-legacy-string-ref-api-usage)
- [关于使用废弃的 findDOMNode 方法的警告](https://react.docschina.org/docs/strict-mode.html#warning-about-deprecated-finddomnode-usage)
- [检测意外的副作用](https://react.docschina.org/docs/strict-mode.html#detecting-unexpected-side-effects)
- [检测过时的 context API](https://react.docschina.org/docs/strict-mode.html#detecting-legacy-context-api)

未来的 React 版本将添加更多额外功能。

**识别不安全的生命周期**

正如[这篇博文](https://react.docschina.org/blog/2018/03/27/update-on-async-rendering.html)所述，某些过时的生命周期方法在异步 React 应用程序中使用是不安全的。但是，如果你的应用程序使用了第三方库，很难确保它们不使用这些生命周期方法。幸运的是，严格模式可以帮助解决这个问题！

当启用严格模式时，React 会列出使用了不安全生命周期方法的所有 class 组件，并打印一条包含这些组件信息的警告消息。

**关于使用过时字符串 ref API 的警告**

以前，React 提供了两种方法管理 refs 的方式：已过时的字符串 ref API 的形式及回调函数 API 的形式。尽管字符串 ref API 在两者中使用更方便，但是它有[一些缺点](https://github.com/facebook/react/issues/1373)，因此官方推荐采用[回调的方式](https://react.docschina.org/docs/refs-and-the-dom.html#legacy-api-string-refs)。

React 16.3 新增了第三种选择，它提供了使用字符串 ref 的便利性，并且不存在任何缺点：

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);

    this.inputRef = React.createRef(); // ref对象
    }

  render() {
    return <input type="text" ref={this.inputRef} />;
    }

  componentDidMount() {
    this.inputRef.current.focus();
    }
}
```

由于对象 ref 主要是为了替换字符串 ref 而添加的，因此严格模式现在会警告使用字符串 ref。

> **注意：**
>
> 除了新增加的 `createRef` API，回调 ref 依旧适用。
>
> 你无需替换组件中的回调 ref。它们更灵活，因此仍将作为高级功能保留。

**检测意外的副作用**

从概念上讲，React 分两个阶段工作：

- **渲染** 阶段会确定需要进行哪些更改，比如 DOM。在此阶段，React 调用 `render`，然后将结果与上次渲染的结果进行比较。
- **提交** 阶段发生在当 React 应用变化时。（对于 React DOM 来说，会发生在 React 插入，更新及删除 DOM 节点的时候。）在此阶段，React 还会调用 `componentDidMount` 和 `componentDidUpdate` 之类的生命周期方法。

提交阶段通常会很快，但渲染过程可能很慢。因此，即将推出的 concurrent 模式 (默认情况下未启用) 将渲染工作分解为多个部分，对任务进行暂停和恢复操作以避免阻塞浏览器。这意味着 React 可以在提交之前多次调用渲染阶段生命周期的方法，或者在不提交的情况下调用它们（由于出现错误或更高优先级的任务使其中断）。

渲染阶段的生命周期包括以下 class 组件方法：

- `constructor`
- `componentWillMount` (or `UNSAFE_componentWillMount`)
- `componentWillReceiveProps` (or `UNSAFE_componentWillReceiveProps`)
- `componentWillUpdate` (or `UNSAFE_componentWillUpdate`)
- `getDerivedStateFromProps`
- `shouldComponentUpdate`
- `render`
- `setState` 更新函数（第一个参数）

因为上述方法可能会被多次调用，所以不要在它们内部编写副作用相关的代码，这点非常重要。忽略此规则可能会导致各种问题的产生，包括内存泄漏和或出现无效的应用程序状态。不幸的是，这些问题很难被发现，因为它们通常具有[非确定性](https://en.wikipedia.org/wiki/Deterministic_algorithm)。

严格模式不能自动检测到你的副作用，但它可以帮助你发现它们，使它们更具确定性。通过故意重复调用以下函数来实现的该操作：

- class 组件的 `constructor`，`render` 以及 `shouldComponentUpdate` 方法
- class 组件的生命周期方法 `getDerivedStateFromProps`
- 函数组件体
- 状态更新函数 (即 `setState` 的第一个参数）
- 函数组件通过使用 `useState`，`useMemo` 或者 `useReducer`

> 注意：
>
> 这仅适用于开发模式。*生产模式下上述的生命周期不会被调用两次。*

##### 使用 PropTypes 进行类型检查

你可以使用 [Flow](https://flow.org/) 或 [TypeScript](https://www.typescriptlang.org/) 等 JavaScript 扩展来对整个应用程序做类型检查。但即使你不使用这些扩展，React 也内置了一些类型检查的功能。要在组件的 props 上进行类型检查，你只需配置特定的 `propTypes` 属性：

```jsx
import PropTypes from 'prop-types';

class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

Greeting.propTypes = {
  name: PropTypes.string
};
```

`PropTypes` 提供一系列验证器，可用于确保组件接收到的数据类型是有效的。在本例中, 我们使用了 `PropTypes.string`。当传入的 `prop` 值类型不正确时，JavaScript 控制台将会显示警告。出于性能方面的考虑，`propTypes` 仅在开发模式下进行检查。

**PropTypes**

以下提供了使用不同验证器的例子：

```js
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你可以将属性声明为 JS 原生类型，默认情况下
  // 这些属性都是可选的。
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、元素或数组）
  // (或 Fragment) 也包含这些类型。
  optionalNode: PropTypes.node,

  // 一个 React 元素。
  optionalElement: PropTypes.element,

  // 一个 React 元素类型（即，MyComponent）。
  optionalElementType: PropTypes.elementType,

  // 你也可以声明 prop 为类的实例，这里使用
  // JS 的 instanceof 操作符。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你可以让你的 prop 只能是特定的值，指定它为
  // 枚举类型。
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 一个对象可以是几种类型中的任意一个类型
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 可以指定一个数组由某一类型的元素组成
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 可以指定一个对象由某一类型的值组成
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 可以指定一个对象由特定的类型值组成
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),
  
  // An object with warnings on extra properties（额外属性会警告）
  optionalObjectWithStrictShape: PropTypes.exact({
    name: PropTypes.string,
    quantity: PropTypes.number
  }),   

  // 你可以在任何 PropTypes 属性后面加上 `isRequired` ，确保
  // 这个 prop 没有被提供时，会打印警告信息。
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的数据
  requiredAny: PropTypes.any.isRequired,

  // 你可以指定一个自定义验证器。它在验证失败时应返回一个 Error 对象。
  // 请不要使用 `console.warn` 或抛出异常，因为这在 `onOfType` 中不会起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 你也可以提供一个自定义的 `arrayOf` 或 `objectOf` 验证器。
  // 它应该在验证失败时返回一个 Error 对象。
  // 验证器将验证数组或对象中的每个值。验证器的前两个参数
  // 第一个是数组或对象本身
  // 第二个是他们当前的键。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};
```

**限制单个元素**

你可以通过 `PropTypes.element` 来确保传递给组件的 children 中只包含一个元素。

```jsx
import PropTypes from 'prop-types';

class MyComponent extends React.Component {
  render() {
    // 这必须只有一个元素，否则控制台会打印警告。
    const children = this.props.children;
    return (
      <div>
        {children}
      </div>
    );
  }
}

MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};
```

**默认 Prop 值**

`defaultProps` 用于确保 `this.props.name` 在父组件没有指定其值时，有一个默认值。`propTypes` 类型检查发生在 `defaultProps` 赋值后，所以类型检查也适用于 `defaultProps`。 

您可以通过配置特定的 `defaultProps` 属性来定义 `props` 的默认值：

```jsx
class Greeting extends React.Component {
  render() {
    return (
      <h1>Hello, {this.props.name}</h1>
    );
  }
}

// 指定 props 的默认值：
Greeting.defaultProps = {
  name: 'Stranger'
};

// 渲染出 "Hello, Stranger"：
ReactDOM.render(
  <Greeting />,
  document.getElementById('example')
);
```

如果你正在使用像 [transform-class-properties](https://babeljs.io/docs/plugins/transform-class-properties/) 的 Babel 转换工具，你也可以在 React 组件类中声明 `defaultProps` 作为静态属性。此语法提案还没有最终确定，需要进行编译后才能在浏览器中运行。要了解更多信息，请查阅 [class fields proposal](https://github.com/tc39/proposal-class-fields)。

```jsx
class Greeting extends React.Component {
  static defaultProps = {
    name: 'stranger'
  }

  render() {
    return (
      <div>Hello, {this.props.name}</div>
    )
  }
}
```

##### 受控和非受控组件

受控组件：表单元素的状态和用户输入过程中表单发生的操作由 React 控制。

非受控组件：表单元素的状态由自己控制，并根据用户输入进行更新，不受 React 控制。

非受控组件通常使用ref来操作真实的DOM。

例如，下面的代码使用非受控组件接受一个表单的值：

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.input = React.createRef();
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={this.input} />
          </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

因为非受控组件将真实数据储存在 DOM 节点中，所以在使用非受控组件时，有时候反而更容易同时集成 React 和非 React 代码。如果你不介意代码美观性，并且希望快速编写代码，使用非受控组件往往可以减少你的代码量。否则，你应该使用受控组件。

如果你还是不清楚在某个特殊场景中应该使用哪种组件，那么 [这篇关于受控和非受控输入组件的文章](https://goshakkk.name/controlled-vs-uncontrolled-inputs-react/) 会很有帮助。

| 特征                                                         | 不受控制 | 受控 |
| ------------------------------------------------------------ | -------- | ---- |
| 一次性取值（例如在提交时）                                   | ✅        | ✅    |
| [提交时验证](https://goshakkk.name/submit-time-validation-react/) | ✅        | ✅    |
| [即时现场验证](https://goshakkk.name/instant-form-fields-validation-react/) | ❌        | ✅    |
| [有条件地禁用提交按钮](https://goshakkk.name/form-recipe-disable-submit-button-react/) | ❌        | ✅    |
| 强制输入格式                                                 | ❌        | ✅    |
| 一个数据的多个输入                                           | ❌        | ✅    |
| [动态输入](https://goshakkk.name/array-form-inputs/)         | ❌        | ✅    |

**默认值**

在 React 渲染生命周期时，表单元素上的 `value` 将会覆盖 DOM 节点中的值，在非受控组件中，你经常希望 React 能赋予组件一个初始值，但是不去控制后续的更新。 在这种情况下, 你可以指定一个 `defaultValue` 属性，而不是 `value`。

```jsx
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={this.input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```

同样，`<input type="checkbox">` 和 `<input type="radio">` 支持 `defaultChecked`，`<select>` 和 `<textarea>` 支持 `defaultValue`。

**文件输入**

在 HTML 中，`<input type="file">` 可以让用户选择一个或多个文件上传到服务器，或者通过使用 [File API](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications) 进行操作。

```
<input type="file" />
```

在 React 中，`<input type="file" />` 始终是一个非受控组件，因为它的值只能由用户设置，而不能通过代码控制。

您应该使用 File API 与文件进行交互。下面的例子显示了如何创建一个 [DOM 节点的 ref](https://react.docschina.org/docs/refs-and-the-dom.html) 从而在提交表单时获取文件的信息。

```jsx
class FileInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.fileInput = React.createRef();
    }
  handleSubmit(event) {
    event.preventDefault();
    alert(
      `Selected file - ${this.fileInput.current.files[0].name}`
      );
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Upload file:
          <input type="file" ref={this.fileInput} />
          </label>
        <br />
        <button type="submit">Submit</button>
      </form>
    );
  }
}

ReactDOM.render(
  <FileInput />,
  document.getElementById('root')
);
```

##### 你可能不需要使用派生 state

父组件传递给子组件的 props 发生改变，引发子组件的render（假设render依赖于该props），并没有执行子组件的constructor函数，子组件没有被卸载自然不会重新加载，只会重新render，而如果父组件的props传递给子组件的state，那么子组件的state只会在第一次加载的时候被赋值，后续的父组件props变化并不会被赋值到子组件的state上。

**什么时候使用派生state**

`getDerivedStateFromProps` 的存在只有一个目的：让组件在 **props 变化**时更新 state。上一个 blog 展示了一些示例，比如 [props 的 offset 变化时，修改当前的滚动方向](https://react.docschina.org/blog/2018/03/27/update-on-async-rendering.html#updating-state-based-on-props)和[根据 props 变化加载外部数据](https://react.docschina.org/blog/2018/03/27/update-on-async-rendering.html#fetching-external-data-when-props-change)。

大部分使用派生 state 导致的问题，不外乎两个原因：

1、直接复制 props 到 state 上；

2、如果 props 和 state 不一致就更新 state。下面的示例包含了这两种情况。

- 如果你只是为了缓存（memoize）基于当前 props 计算后的结果的话，你就没必要使用派生 state。[尝试一下 memoization？](https://react.docschina.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization)。
- 如果只是用来保存 props 或者和当前 state 比较之后不一致后更新 state，那你的组件应该是太频繁的更新了 state。请继续阅读。

**派生 state 的常见 bug**

名词[“受控”](https://react.docschina.org/docs/forms.html#controlled-components)和[“非受控”](https://react.docschina.org/docs/uncontrolled-components.html)通常用来指代表单的 inputs，但是也可以用来描述数据频繁更新的组件。

用 props 传入数据的话，组件可以被认为是**受props控制**（因为组件被父级传入的 props 控制）。数据只保存在组件内部的 state 的话，是**不受props控制**组件（因为外部没办法直接控制 state）。

常见的错误就是把两者混为一谈。

**反面模式: 直接复制 prop 到 state**

最常见的误解就是 `getDerivedStateFromProps` 和 `componentWillReceiveProps` 只会在 props “改变”时才会调用。实际上只要父级重新渲染时（props、state修改会导致重新渲染），这两个生命周期函数就会重新调用，不管 props 有没有“变化”。所以，在这两个方法内直接复制（*unconditionally*）props 到 state 是不安全的。**这样做会导致 state 修改后没有正确渲染**。

```jsx
class EmailInput extends Component {
  state = { email: this.props.email };

  render() {
    return <input onChange={this.handleChange} value={this.state.email} />;
  }

  handleChange = event => {
    // 修改state，组件重新渲染，组件调用 componentWillReceiveProps ，再次设置state，导致该方法内修改state没有达到预期效果
    this.setState({ email: event.target.value });
  };

  componentWillReceiveProps(nextProps) {
    this.setState({ email: nextProps.email });
  }
}
```

**反面模式: 在 props 变化后修改 state**

继续上面的示例，我们可以只使用 `props.email` 来更新组件，这样能防止修改 state 导致的 bug：

```jsx
class EmailInput extends Component {
  state = {
    email: this.props.email
  };

  componentWillReceiveProps(nextProps) {
    // 只要 props.email 改变，就改变 state
    if (nextProps.email !== this.props.email) {
      this.setState({
        email: nextProps.email
      });
    }
  }
	render() {
    return <input onChange={this.handleChange} value={this.state.email} />;
  }
}
```


> 注意
>
> 示例中使用了 `componentWillReceiveProps` ，使用 `getDerivedStateFromProps` 也是一样。

但是仍然有个问题。此 state 必须要依赖于 props 的变化才能改变。

幸运的是，有两个方案能解决这些问题。这两者的关键在于，**任何数据，都要保证只有一个数据来源，而且避免直接复制它**。我们来看看这两个方案。

**建议的模式**

**建议：完全受props控制的组件**

阻止上述问题发生的一个方法是，**从组件里删除 state**。如果 prop 里包含了 email，我们就没必要担心它和 state 冲突。我们甚至可以把 `EmailInput` 转换成一个轻量的函数组件：

```jsx
function EmailInput(props) {
  return <input onChange={props.onChange} value={props.email} />;
}
```

**建议：有 key 的不受props控制组件**

另外一个选择是让组件自己存储临时的 email state。在这种情况下，组件仍然可以从 prop 接收“初始值”，但是更改之后的值就和 prop 没关系了：

```jsx
class EmailInput extends Component {
  state = { email: this.props.defaultEmail };

  handleChange = event => {
    // 不在componentWillReceiveProps或getDerivedStateFromProps内修改state
    this.setState({ email: event.target.value });
  };

  render() {
    return <input onChange={this.handleChange} value={this.state.email} />;
  }
}
```

为了在不同的页面切换不同的值，我们可以使用 `key` 这个特殊的 React 属性。当 `key` 变化时， React 会[创建一个新的而不是更新一个既有的组件](https://react.docschina.org/docs/reconciliation.html#keys)。 Keys 一般用来渲染动态列表，但是这里也可以使用。在这个示例里，当用户输入时，我们使用 user ID 当作 key 重新创建一个新的 email input 组件：

```jsx
<EmailInput
  defaultEmail={this.props.user.email}
  key={this.props.user.id}
/>
```

每次 ID 更改，都会重新创建 `EmailInput` ，并将其状态重置为最新的 `defaultEmail` 值。([点击查看这个模式的演示](https://codesandbox.io/s/6v1znlxyxn)) 使用此方法，不用为每次输入都添加 `key`，在整个表单上添加 `key` 更有位合理。每次 key 变化，表单里的所有组件都会用新的初始值重新创建。

大部分情况下，这是处理重置 state 的最好的办法。

**选项二：使用实例方法重置不受props控制组件**

更少见的情况是，即使没有合适的 `key`，我们也想重新创建组件。一种解决方案是给一个随机值或者递增的值当作 `key`，另外一种是用实例方法强制重置内部状态：

```jsx
class EmailInput extends Component {
  state = {
    email: this.props.defaultEmail
  };

  resetEmailForNewUser(newEmail) {
    this.setState({ email: newEmail });
  }

  // ...
}
```

然后父级组件可以使用 ref 是回调函数的方法（此函数会（根据元素的类型）接收底层 DOM 元素或 class 实例作为其参数）调用 resetEmailForNewUser ([点击查看这个模式的演示](https://codesandbox.io/s/l70krvpykl))。

refs 在某些情况下很有用，比如这个。但通常我们建议谨慎使用。即使是做一个演示，这个命令式的方法也是非理想的，因为这会导致两次而不是一次渲染。

**概括**

设计组件时，重要的是确定组件是受 props 控制组件还是不受 props 控制组件。

##### 样式和CSS

**如何为组件添加 CSS 的 class？**

传递一个字符串作为 `className` 属性：

```jsx
render() {
  return <span className="menu navigation-menu">Menu</span>
}
```

CSS 的 class 依赖组件的 props 或 state 的情况很常见：

```jsx
render() {
  let className = 'menu';
  if (this.props.isActive) {
    className += ' menu-active';
  }
  return <span className={className}>Menu</span>
}
```

在多数情况下，应使用 [`className`](https://react.docschina.org/docs/dom-elements.html#classname) 属性来引用外部 CSS 样式表中定义的 class。`style` 在 React 应用中多用于在渲染过程中添加动态计算的样式。从性能角度来说，CSS 的 class 通常比行内样式更好。

`style` 接受一个采用小驼峰命名属性的 JavaScript 对象，而不是 CSS 字符串。这与 DOM 中 `style` 的 JavaScript 属性是一致的（例如background-image的 JavaScript 属性是backgroundImage），同时会更高效的，且能预防跨站脚本（XSS）的安全漏洞。例如：

```jsx
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```

注意：样式不会自动补齐前缀。如需支持旧版浏览器，请手动补充对应的样式属性：

```jsx
const divStyle = {
  WebkitTransition: 'all', // 谷歌、苹果
  MozTransition:'all', // 火狐
  OTransition:'all', // Opera
  msTransition: 'all' // 微软，'ms' is the only lowercase vendor prefix
};

function ComponentWithTransition() {
  return <div style={divStyle}>This should work cross-browser</div>;
}
```

Style 中的 key 采用小驼峰命名是为了与 JS 访问 DOM 节点的属性保持一致（例如：`node.style.backgroundImage` ）。浏览器引擎前缀都应以大写字母开头，[除了 `ms`](https://www.andismith.com/blog/2012/02/modernizr-prefixed/)。因此，`WebkitTransition` 首字母为 ”W”。

React 会自动添加 ”px” 后缀到内联样式为数字的属性后。如需使用 ”px” 以外的单位，请将此值设为数字与所需单位组成的字符串。例如：

```jsx
// Result style: '10px'
<div style={{ height: 10 }}>
  Hello World!
</div>

// Result style: '10%'
<div style={{ height: '10%' }}>
  Hello World!
</div>
```

但并非所有样式属性都转换为像素字符串。有些样式属性是没有单位的(例如 `zoom`，`order`，`flex`)。无单位属性的完整列表在[此处](https://github.com/facebook/react/blob/4131af3e4bf52f3a003537ec95a1655147c81270/src/renderers/dom/shared/CSSProperty.js#L15-L59)。

###### 样式模块化

样式模块化可以防止样式类名冲突。

通过create react app创建的项目，在CSS文件名加入module，例如 index.css —> index.module.css

```jsx
import xxx from './index.module.css'
// 使用
<div className={xxx.active}>hello<div />
// 或
<div className="xxx.active">hello<div />
```

**suppressContentEditableWarning**

通常，当拥有子节点的元素被标记为 `contentEditable` 时，React 会发出一个警告，因为这不会生效。该属性将禁止此警告。尽量不要使用该属性，除非你要构建一个类似 [Draft.js](https://facebook.github.io/draft-js/) 的手动管理 `contentEditable` 属性的库。

**All Supported HTML Attributes**

在 React 16 中，任何标准的[或自定义的](https://react.docschina.org/blog/2017/09/08/dom-attributes-in-react-16.html) DOM 属性都是完全支持的。

React 为 DOM 提供了一套以 JavaScript 为中心的 API。由于 React 组件经常采用自定义或和 DOM 相关的 props 的关系，React 采用了`小驼峰命名`的方式，正如 DOM APIs 那样：

```div
<div tabIndex="-1" />      // Just like node.tabIndex DOM API
<div className="Button" /> // Just like node.className DOM API
<input readOnly={true} />  // Just like node.readOnly DOM API
```

除了上述文档提到的特殊拼写方式以外，这些 props 的用法与 HTML 的属性也极为类似。

React 支持的 DOM 属性有：

```
accept acceptCharset accessKey action allowFullScreen alt async autoComplete
autoFocus autoPlay capture cellPadding cellSpacing challenge charSet checked
cite classID className colSpan cols content contentEditable contextMenu controls
controlsList coords crossOrigin data dateTime default defer dir disabled
download draggable encType form formAction formEncType formMethod formNoValidate
formTarget frameBorder headers height hidden high href hrefLang htmlFor
httpEquiv icon id inputMode integrity is keyParams keyType kind label lang list
loop low manifest marginHeight marginWidth max maxLength media mediaGroup method
min minLength multiple muted name noValidate nonce open optimum pattern
placeholder poster preload profile radioGroup readOnly rel required reversed
role rowSpan rows sandbox scope scoped scrolling seamless selected shape size
sizes span spellCheck src srcDoc srcLang srcSet start step style summary
tabIndex target title type useMap value width wmode wrap
```

同样，所有的 SVG 属性也完全得到了支持：

```
accentHeight accumulate additive alignmentBaseline allowReorder alphabetic
amplitude arabicForm ascent attributeName attributeType autoReverse azimuth
baseFrequency baseProfile baselineShift bbox begin bias by calcMode capHeight
clip clipPath clipPathUnits clipRule colorInterpolation
colorInterpolationFilters colorProfile colorRendering contentScriptType
contentStyleType cursor cx cy d decelerate descent diffuseConstant direction
display divisor dominantBaseline dur dx dy edgeMode elevation enableBackground
end exponent externalResourcesRequired fill fillOpacity fillRule filter
filterRes filterUnits floodColor floodOpacity focusable fontFamily fontSize
fontSizeAdjust fontStretch fontStyle fontVariant fontWeight format from fx fy
g1 g2 glyphName glyphOrientationHorizontal glyphOrientationVertical glyphRef
gradientTransform gradientUnits hanging horizAdvX horizOriginX ideographic
imageRendering in in2 intercept k k1 k2 k3 k4 kernelMatrix kernelUnitLength
kerning keyPoints keySplines keyTimes lengthAdjust letterSpacing lightingColor
limitingConeAngle local markerEnd markerHeight markerMid markerStart
markerUnits markerWidth mask maskContentUnits maskUnits mathematical mode
numOctaves offset opacity operator order orient orientation origin overflow
overlinePosition overlineThickness paintOrder panose1 pathLength
patternContentUnits patternTransform patternUnits pointerEvents points
pointsAtX pointsAtY pointsAtZ preserveAlpha preserveAspectRatio primitiveUnits
r radius refX refY renderingIntent repeatCount repeatDur requiredExtensions
requiredFeatures restart result rotate rx ry scale seed shapeRendering slope
spacing specularConstant specularExponent speed spreadMethod startOffset
stdDeviation stemh stemv stitchTiles stopColor stopOpacity
strikethroughPosition strikethroughThickness string stroke strokeDasharray
strokeDashoffset strokeLinecap strokeLinejoin strokeMiterlimit strokeOpacity
strokeWidth surfaceScale systemLanguage tableValues targetX targetY textAnchor
textDecoration textLength textRendering to transform u1 u2 underlinePosition
underlineThickness unicode unicodeBidi unicodeRange unitsPerEm vAlphabetic
vHanging vIdeographic vMathematical values vectorEffect version vertAdvY
vertOriginX vertOriginY viewBox viewTarget visibility widths wordSpacing
writingMode x x1 x2 xChannelSelector xHeight xlinkActuate xlinkArcrole
xlinkHref xlinkRole xlinkShow xlinkTitle xlinkType xmlns xmlnsXlink xmlBase
xmlLang xmlSpace y y1 y2 yChannelSelector z zoomAndPan
```

你也可以使用自定义属性，但要注意属性名全都为小写。

**什么是 CSS-in-JS?**

“CSS-in-JS” 是指一种模式，其中 CSS 由 JavaScript 生成而不是在外部文件中定义。在[此处](https://github.com/MicheleBertoli/css-in-js)阅读 CSS-in-JS 库之间的对比。

*注意此功能并不是 React 的一部分，而是由第三方库提供。* React 对样式如何定义并没有明确态度；如果存在疑惑，比较好的方式是和平时一样，在一个单独的 `*.css` 文件定义你的样式，并且通过 [`className`](https://react.docschina.org/docs/dom-elements.html#classname) 指定它们。

**可以在 React 中实现动画效果吗？**

React 可以被用来实现强大的动画效果。参见 [React Transition Group](https://reactcommunity.org/react-transition-group/)、[React Motion](https://github.com/chenglou/react-motion) 以及 [React Spring](https://github.com/react-spring/react-spring) 等示例。



##### Web Components（了解）

React 和 [Web Components](https://developer.mozilla.org/en-US/docs/Web/Web_Components) 为了解决不同的问题而生。Web Components 为可复用组件提供了强大的封装，而 React 则提供了声明式的解决方案，使 DOM 与数据保持同步。两者旨在互补。作为开发人员，可以自由选择在 Web Components 中使用 React，或者在 React 中使用 Web Components，或者两者共存。

大多数开发者在使用 React 时，不使用 Web Components，但可能你会需要使用，尤其是在使用 Web Components 编写的第三方 UI 组件时。

**在 React 中使用 Web Components**

```jsx
class HelloMessage extends React.Component {
  render() {
    return <div>Hello <x-search>{this.props.name}</x-search>!</div>;
  }
}
```

> 注意：
>
> Web Components 通常暴露的是命令式 API。例如，Web Components 的组件 `video` 可能会公开 `play()` 和 `pause()` 方法。要访问 Web Components 的命令式 API，你需要使用 `ref` 直接与 DOM 节点进行交互。如果你使用的是第三方 Web Components，那么最好的解决方案是编写 React 组件包装该 Web Components。
>
> Web Components 触发的事件可能无法通过 React 渲染树正确的传递。 你需要在 React 组件中手动添加事件处理器来处理这些事件。

常见的误区是在 Web Components 中使用的是 `class` 而非 `className`。

```jsx
function BrickFlipbox() {
  return (
    <brick-flipbox class="demo">
      <div>front</div>
      <div>back</div>
    </brick-flipbox>
  );
}
```

**在 Web Components 中使用 React**

```jsx
class XSearch extends HTMLElement {
  connectedCallback() {
    const mountPoint = document.createElement('span');
    this.attachShadow({ mode: 'open' }).appendChild(mountPoint);

    const name = this.getAttribute('name');
    const url = 'https://www.google.com/search?q=' + encodeURIComponent(name);
    ReactDOM.render(<a href={url}>{name}</a>, mountPoint);
  }
}
customElements.define('x-search', XSearch);
```



#### Hooks

##### 简介

Hook 是 React 16.8 的新增特性。它可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性。

Hook 在 class 内部是**不**起作用的。但你可以使用它们来取代 class 。

Hook 是一个特殊的函数，它可以让你“钩入” React 的特性。例如，`useState` 是允许你在 React 函数组件中添加 state 的 Hook。

**动机**

可以使用 Hook 从组件中提取状态逻辑，使得这些逻辑可以单独测试并复用。**Hook 使你在无需修改组件结构的情况下复用状态逻辑。** 

**Hook 将组件中相互关联的部分拆分成更小的函数（比如设置订阅或请求数据）**，而并非强制按照生命周期划分。

##### State Hook

Hook 示例：

```jsx
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量  
  const [count, setCount] = useState(0); // 数组解构
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**等价的 class 示例**

```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

state 初始值为 `{ count: 0 }` ，当用户点击按钮后，我们通过调用 `this.setState()` 来增加 `state.count`。整个章节中都将使用该 class 的代码片段做示例。

**声明 State 变量**

```jsx
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 “count” 的 state 变量  
  const [count, setCount] = useState(0);
}
```

**`调用 useState` 方法的时候做了什么?** 它定义一个 “state 变量”。我们的变量叫 `count`， 但是我们可以叫他任何名字，比如 `banana`。这是一种在函数调用时保存变量的方式 —— `useState` 是一种新方法，它与 class 里面的 `this.state` 提供的功能完全相同。一般来说，在函数退出后变量就会”消失”，而 state 中的变量会被 React 保留。

**`useState` 需要哪些参数？** `useState()` 方法里面唯一的参数就是初始 state。不同于 class 的是，我们可以按照需要使用数字或字符串对其进行赋值，而不一定是对象。在示例中，只需使用数字来记录用户点击次数，所以我们传了 `0` 作为变量的初始 state。（如果我们想要在 state 中存储两个不同的变量，只需调用 `useState()` 两次即可。）

**`useState` 方法的返回值是什么？** 返回值为：当前 state 以及更新 state 的函数。这就是我们写 `const [count, setCount] = useState()` 的原因。这与 class 里面 `this.state.count` 和 `this.setState` 类似，唯一区别就是你需要成对的获取它们。

**读取 State**

当我们想在 class 中显示当前的 count，我们读取 `this.state.count`：

```jsx
  <p>You clicked {this.state.count} times</p>
```

在函数中，我们可以直接用 `count`:

```jsx
  <p>You clicked {count} times</p>
```

**更新 State**

在 class 中，我们需要调用 `this.setState()` 来更新 `count` 值：

```jsx
  <button onClick={() => this.setState({ count: this.state.count + 1 })}>    
  	Click me
  </button>
```

在函数中，我们已经有了 `setCount` 和 `count` 变量，所以我们不需要 `this`:

```jsx
  <button onClick={() => setCount(count + 1)}>
  	Click me
  </button>
```

与 class 中的 `this.setState`不同，Hook 更新 state 变量总是**替换**它而不是合并它。

##### **Effect Hook**

*Effect Hook* 可以让你在函数组件中执行副作用操作

```jsx
import React, { useState, useEffect } from 'react';
function Example() {
  const [count, setCount] = useState(0);

  // Similar to componentDidMount and componentDidUpdate:
  useEffect(() => {
    // Update the document title using the browser API
    document.title = `You clicked ${count} times`;
  });
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

数据获取，设置订阅以及手动更改 React 组件中的 DOM 都属于副作用。不管你知不知道这些操作，或是“副作用”这个名字，应该都在组件中使用过它们。

> 提示
>
> 如果你熟悉 React class 的生命周期函数，你可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合。

**无需清除的 effect**

有时候，我们只想**在 React 更新 DOM 之后运行一些额外的代码。**比如发送网络请求，手动变更 DOM，记录日志，这些都是常见的无需清除的操作。因为我们在执行完这些操作之后，就可以忽略他们了。让我们对比一下使用 class 和 Hook 都是怎么实现这些副作用的。

**使用 class 的示例**

在 React 的 class 组件中，`render` 函数是不应该有任何副作用的。一般来说，在这里执行操作太早了，我们基本上都希望在 React 更新 DOM 之后才执行我们的操作。

这就是为什么在 React class 中，我们把副作用操作放到 `componentDidMount` 和 `componentDidUpdate` 函数中。回到示例中，这是一个 React 计数器的 class 组件。它在 React 对 DOM 进行操作之后，立即更新了 document 的 title 属性

```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }
  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }
  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

注意，**在这个 class 中，我们需要在两个生命周期函数中编写重复的代码。**

这是因为很多情况下，我们希望在组件加载和更新时执行同样的操作。从概念上说，我们希望它在每次渲染之后执行 —— 但 React 的 class 组件没有提供这样的方法。即使我们提取出一个方法，我们还是要在两个地方调用它。

现在让我们来看看如何使用 `useEffect` 执行相同的操作。

**使用 Hook 的示例**

我们在本章节开始时已经看到了这个示例，但让我们再仔细观察它：

```jsx
import React, { useState, useEffect } from 'react';
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    document.title = `You clicked ${count} times`;
  });
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

**`useEffect` 做了什么？** 通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作。React 会保存你传递的函数（我们将它称之为 “effect”），并且在执行 DOM 更新之后调用它。在这个 effect 中，我们设置了 document 的 title 属性，不过我们也可以执行数据获取或调用其他命令式的 API。

**为什么在组件内部调用 `useEffect`？** 将 `useEffect` 放在组件内部让我们可以在 effect 中直接访问 `count` state 变量（或其他 props）。我们不需要特殊的 API 来读取它 —— 它已经保存在函数作用域中。Hook 使用了 JavaScript 的闭包机制，而不用在 JavaScript 已经提供了解决方案的情况下，还引入特定的 React API。

**`useEffect` 会在每次渲染后都执行吗？** 是的，默认情况下，它在第一次渲染之后和每次更新之后都会执行。（我们稍后会谈到[如何控制它](https://react.docschina.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)。）你可能会更容易接受 effect 发生在“渲染之后”这种概念，不用再去考虑“挂载”还是“更新”。React 保证了每次运行 effect 的同时，DOM 都已经更新完毕。

我们可以在 effect 中获取到最新的 `count` 值，因为他在函数的作用域内。当 React 渲染组件时，会保存已使用的 effect，并在更新完 DOM 后执行它。这个过程在每次渲染时都会发生，包括首次渲染。

传递给 `useEffect` 的函数在每次渲染中都会有所不同，这是刻意为之的。事实上这正是我们可以在 effect 中获取最新的 `count` 的值，而不用担心其过期的原因。每次我们重新渲染，都会生成*新的* effect，替换掉之前的。某种意义上讲，effect 更像是渲染结果的一部分 —— 每个 effect “属于”一次特定的渲染。

> 提示
>
> 与 `componentDidMount` 或 `componentDidUpdate` 不同，使用 `useEffect` 调度的 effect 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快。大多数情况下，effect 不需要同步地执行。在个别情况下（例如测量布局），有单独的 [`useLayoutEffect`](https://react.docschina.org/docs/hooks-reference.html#uselayouteffect) Hook 供你使用，其 API 与 `useEffect` 相同。

**需要清除的 effect**

之前，我们研究了如何使用不需要清除的副作用，还有一些副作用是需要清除的。例如**订阅外部数据源**。这种情况下，清除工作是非常重要的，可以防止引起内存泄露！现在让我们来比较一下如何用 Class 和 Hook 来实现。

**使用 Class 的示例**

在 React class 中，你通常会在 `componentDidMount` 中设置订阅，并在 `componentWillUnmount` 中清除它。例如，假设我们有一个 `ChatAPI` 模块，它允许我们订阅好友的在线状态。以下是我们如何使用 class 订阅和显示该状态：

```jsx
class FriendStatus extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOnline: null };
    this.handleStatusChange = this.handleStatusChange.bind(this);
  }

  componentDidMount() {
  	ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id, 
      this.handleStatusChange
    );
  }
  handleStatusChange(status) {
    this.setState({
      isOnline: status.isOnline
    });
  }
  render() {
    if (this.state.isOnline === null) {
      return 'Loading...';
    }
    return this.state.isOnline ? 'Online' : 'Offline';
  }
}
```

你会注意到 `componentDidMount` 和 `componentWillUnmount` 之间相互对应。使用生命周期函数迫使我们拆分这些逻辑代码，即使这两部分代码都作用于相同的副作用。

> 注意
>
> 眼尖的读者可能已经注意到了，这个示例还需要编写 `componentDidUpdate` 方法才能保证完全正确。我们先暂时忽略这一点，本章节中[后续部分](https://react.docschina.org/docs/hooks-effect.html#explanation-why-effects-run-on-each-update)会介绍它。

**使用 Hook 的示例**

如何使用 Hook 编写这个组件。

你可能认为需要单独的 effect 来执行清除操作。但由于添加和删除订阅的代码的紧密性，所以 `useEffect` 的设计是在同一个地方执行。如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它：

```jsx
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // Specify how to clean up after this effect:
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

**为什么要在 effect 中返回一个函数？** 这是 effect 可选的清除机制。每个 effect 都可以返回一个清除函数。如此可以将添加和移除订阅的逻辑放在一起。它们都属于 effect 的一部分。

**React 何时清除 effect？**  effect 的清除阶段在每次重新渲染时都会执行，而不是只在卸载组件的时候执行一次。这就是为什么 React 会在执行当前 effect 之前对上一个 effect 进行清除。我们稍后将讨论[为什么这将助于避免 bug](https://react.docschina.org/docs/hooks-effect.html#explanation-why-effects-run-on-each-update)以及[如何在遇到性能问题时跳过此行为](https://react.docschina.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)。

> 注意
>
> 并不是必须为 effect 中返回的函数命名。这里我们将其命名为 `cleanup` 是为了表明此函数的目的，但其实也可以返回一个箭头函数或者给起一个别的名字。

**使用多个State 和 Effect 实现关注点分离**

使用 Hook 其中一个[目的](https://react.docschina.org/docs/hooks-intro.html#complex-components-become-hard-to-understand)就是要解决 class 中生命周期函数经常包含不相关的逻辑，但又把相关逻辑分离到了几个不同方法中的问题。

**为什么每次更新的时候都要运行 Effect**

如果你已经习惯了使用 class，那么你或许会疑惑为什么 effect 的清除阶段在每次重新渲染时都会执行，而不是只在卸载组件的时候执行一次。让我们看一个实际的例子，看看为什么这个设计可以帮助我们创建 bug 更少的组件。

在[本章节开始时](https://react.docschina.org/docs/hooks-effect.html#example-using-classes-1)，我们介绍了一个用于显示好友是否在线的 `FriendStatus` 组件。从 class 中 props 读取 `friend.id`，然后在组件挂载后订阅好友的状态，并在卸载组件的时候取消订阅：

```jsx
  componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
```

在 class 组件中，我们还需要添加 `componentDidUpdate` 来更新好友状态：

```jsx
  componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }

  componentDidUpdate(prevProps) {
  	// 取消订阅之前的 friend.id
    ChatAPI.unsubscribeFromFriendStatus(
      prevProps.friend.id,
      this.handleStatusChange
    );
  	// 订阅新的 friend.id
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
```

忘记正确地处理 `componentDidUpdate` 是 React 应用中常见的 bug 来源。

现在看一下使用 Hook 的版本：

```jsx
function FriendStatus(props) {
  // ...
  useEffect(() => {
    // ...
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });
```

并不需要特定的代码来处理更新逻辑，因为 `useEffect` *默认*就会处理。它会在调用一个新的 effect 之前对前一个 effect 进行清理。为了说明这一点，下面按时间列出一个可能会产生的订阅和取消订阅操作调用序列：

```jsx
// Mount with { friend: { id: 100 } } props
ChatAPI.subscribeToFriendStatus(100, handleStatusChange);     // 运行第一个 effect

// Update with { friend: { id: 200 } } props
ChatAPI.unsubscribeFromFriendStatus(100, handleStatusChange); // 清除上一个 effect
ChatAPI.subscribeToFriendStatus(200, handleStatusChange);     // 运行下一个 effect

// Update with { friend: { id: 300 } } props
ChatAPI.unsubscribeFromFriendStatus(200, handleStatusChange); // 清除上一个 effect
ChatAPI.subscribeToFriendStatus(300, handleStatusChange);     // 运行下一个 effect

// Unmount
ChatAPI.unsubscribeFromFriendStatus(300, handleStatusChange); // 清除最后一个 effect
```

此默认行为保证了一致性，避免了在 class 组件中因为没有处理更新逻辑而导致常见的 bug。

**通过跳过 Effect 进行性能优化**

在某些情况下，每次渲染后都执行清理或者执行 effect 可能会导致性能问题。在 class 组件中，我们可以通过在 `componentDidUpdate` 中添加对 `prevProps` 或 `prevState` 的比较逻辑解决：

```jsx
componentDidUpdate(prevProps, prevState) {
  if (prevState.count !== this.state.count) {
    document.title = `You clicked ${this.state.count} times`;
  }
}
```

这是很常见的需求，所以它被内置到了 `useEffect` 的 Hook API 中。如果某些特定值在两次重渲染之间没有发生变化，你可以通知 React **跳过**对 effect 的调用，只要传递数组作为 `useEffect` 的第二个可选参数即可：

```jsx
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新
```

上面这个示例中，我们传入 `[count]` 作为第二个参数。这个参数是什么作用呢？如果 `count` 的值是 `5`，而且我们的组件重渲染的时候 `count` 还是等于 `5`，React 将对前一次渲染的 `[5]` 和后一次渲染的 `[5]` 进行比较。因为数组中的所有元素都是相等的(`5 === 5`)，React 会跳过这个 effect，这就实现了性能的优化。

当渲染时，如果 `count` 的值更新成了 `6`，React 将会把前一次渲染时的数组 `[5]` 和这次渲染的数组 `[6]` 中的元素进行对比。这次因为 `5 !== 6`，React 就会再次调用 effect。如果数组中有多个元素，即使只有一个元素发生变化，React 也会执行 effect。

对于有清除操作（返回清除函数）的 effect 同样适用。

> 注意：
>
> 如果你要使用此优化方式，请确保数组中包含了**所有外部作用域中会随时间变化并且在 effect 中使用的变量**，否则你的代码会引用到先前渲染中的旧变量。参阅文档，了解更多关于[如何处理函数](https://react.docschina.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies)以及[数组频繁变化时的措施](https://react.docschina.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often)内容。
>
> 如果想执行只运行一次的 effect（仅在组件挂载和卸载时执行），可以传递一个空数组（`[]`）作为第二个参数。这就告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。这并不属于特殊情况 —— 它依然遵循依赖数组的工作方式。
>
> 如果你传入了一个空数组（`[]`），effect 内部的 props 和 state 就会一直拥有其初始值。但我们有[更好的](https://react.docschina.org/docs/hooks-faq.html#is-it-safe-to-omit-functions-from-the-list-of-dependencies)[方式](https://react.docschina.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often)来避免过于频繁的重复调用 effect，详看下列内容。除此之外，请记得 React 会等待浏览器完成画面渲染之后才会延迟调用 `useEffect`，因此会使得额外操作很方便。
>
> 我们推荐启用 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation) 中的 [`exhaustive-deps`](https://github.com/facebook/react/issues/14920) 规则。此规则会在添加错误依赖时发出警告并给出修复建议。

**在依赖列表中省略函数是否安全？**

一般来说，不安全。

```jsx
function Example({ someProp }) {
  function doSomething() {
    console.log(someProp);
  }

  useEffect(() => {
    doSomething();
  }, []); // 🔴 []省略了doSomething：这样不安全（它调用的 `doSomething` 函数使用了 `someProp`）
}
```

要记住 effect 外部的函数使用了哪些 props 和 state 很难。这也是为什么 **通常你会想要在 effect 内部去声明它所需要的函数。** 这样就能容易的看出那个 effect 依赖了组件作用域中的哪些值：

```jsx
function Example({ someProp }) {
  useEffect(() => {
    function doSomething() {
      console.log(someProp);
    }

    doSomething();
  }, [someProp]); // ✅ 安全（我们的 effect 仅用到了 `someProp`）}
```

如果这样之后我们依然没用到组件作用域中的任何值，就可以安全地把它指定为 `[]`：

```jsx
useEffect(() => {
  function doSomething() {
    console.log('hello');
  }

  doSomething();
}, []); // ✅ 在这个例子中是安全的，因为我们没有用到组件作用域中的 *任何* 值
```

如果你指定了一个 [依赖列表](https://react.docschina.org/docs/hooks-reference.html#conditionally-firing-an-effect) 作为 `useEffect`、`useLayoutEffect`、`useMemo`、`useCallback` 或 `useImperativeHandle` 的最后一个参数，它必须包含回调中的所有值，并参与 React 数据流。这就包括 props、state，以及任何由它们衍生而来的东西。

**只有** 当函数（以及它所调用的函数）不引用 props、state 以及由它们衍生而来的值时，你才能放心地把它们从依赖列表中省略。下面这个案例有一个 Bug：

```jsx
function ProductPage({ productId }) {
  const [product, setProduct] = useState(null);

  async function fetchProduct() {
    const response = await fetch('http://myapi/product/' + productId); // 使用了 productId prop    const json = await response.json();
    setProduct(json);
  }

  useEffect(() => {
    fetchProduct();
  }, []); // 🔴 这样是无效的，因为 `fetchProduct` 使用了 `productId`  // ...
}
```

**推荐的修复方案是把那个函数移动到你的 effect 内部**。这样就能很容易的看出来你的 effect 使用了哪些 props 和 state，并确保它们都被声明了：

```jsx
function ProductPage({ productId }) {
  const [product, setProduct] = useState(null);

  useEffect(() => {
    // 把这个函数移动到 effect 内部后，我们可以清楚地看到它用到的值。
    async function fetchProduct() {
      const response = await fetch('http://myapi/product/' + productId);
      const json = await response.json();
      setProduct(json);
    }
    fetchProduct();
  }, [productId]); // ✅ 有效，因为我们的 effect 只用到了 productId  // ...
}
```

**如果处于某些原因你无法把一个函数移动到 effect 内部，还有一些其他办法：**

- **你可以尝试把那个函数移动到你的组件之外**。那样一来，这个函数就肯定不会依赖任何 props 或 state，并且也不用出现在依赖列表中了。
- 如果你所调用的方法是一个纯计算，并且可以在渲染时调用，你可以 **转而在 effect 之外调用它，** 并让 effect 依赖于它的返回值。
- 万不得已的情况下，你可以 **把函数加入 effect 的依赖，但把它的定义包裹** 进 [`useCallback`](https://react.docschina.org/docs/hooks-reference.html#usecallback) Hook。这就确保了它不随渲染而改变，除非 *它自身* 的依赖发生了改变：

```jsx
function ProductPage({ productId }) {
  // ✅ 用 useCallback 包裹以避免随渲染发生改变
  const fetchProduct = useCallback(() => {
    // ... Does something with productId ...
  }, [productId]); // ✅ useCallback 的所有依赖都被指定了
  return <ProductDetails fetchProduct={fetchProduct} />;
}

function ProductDetails({ fetchProduct }) {
  useEffect(() => {
    fetchProduct();
  }, [fetchProduct]); // ✅ useEffect 的所有依赖都被指定了
  // ...
}
```

**如果我的 effect 的依赖频繁变化，我该怎么办？**

有时候，你的 effect 可能会使用一些频繁变化的值。你可能会忽略依赖列表中 state，但这通常会引起 Bug：

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1); // 这个 effect 依赖于 `count` state
    }, 1000);
    return () => clearInterval(id);
  }, []); // 🔴 Bug: `count` 没有被指定为依赖
  return <h1>{count}</h1>;
}
```

传入空的依赖数组 `[]`，意味着该 hook 只在组件挂载时运行一次，并非重新渲染时。告诉 React 你的 effect 不依赖于 props 或 state 中的任何值，所以它永远都不需要重复执行。但如此会有问题，在 `setInterval` 的回调中，`count` 的值不会发生变化。因为当 effect 执行时，我们会创建一个闭包，并将 `count` 的值被保存在该闭包当中，且初值为 `0`，之后也不会随着。每隔一秒，回调就会执行 `setCount(0 + 1)`，因此，`count` 永远不会超过 1。

指定 `[count]` 作为依赖列表就能修复这个 Bug，但会导致每次改变发生时定时器都被重置。事实上，每个 `setInterval` 在被清除前（类似于 `setTimeout`）都会调用一次。但这并不是我们想要的。要解决这个问题，我们可以使用 [`setState` 的函数式更新形式](https://react.docschina.org/docs/hooks-reference.html#functional-updates)。

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + 1); // ✅ 在这不依赖于外部的 `count` 变量
    }, 1000);
    return () => clearInterval(id);
  }, []); // ✅ 我们的 effect 不适用组件作用域中的任何变量
  return <h1>{count}</h1>;
}
```

（`setCount` 函数的身份/标识是被确保稳定的，不会在组件重新渲染时发生变化，所以可以放心的省略掉）

此时，`setInterval` 的回调依旧每秒调用一次，但每次 `setCount` 内部的回调取到的 `count` 是最新值（在回调中变量命名为 `c`）。

##### 自定义 Hook

通过自定义 Hook，可以将组件逻辑提取到可重用的函数中。

自定义 Hook 是一个函数，其名称必须以 “`use`” 开头，函数内部可以调用其他的 Hook。

请确保只在自定义 Hook 的顶层无条件地调用其他 Hook。

自定义 Hook 只能被函数组件和自定义 Hook 调用。

与 React 组件不同的是，自定义 Hook 不需要具有特殊的标识。我们可以自由的决定它的参数是什么，以及它应该返回什么（如果需要的话）。换句话说，它就像一个正常的函数。

**在两个组件中使用相同的 Hook 会共享 state 吗？**

不会。自定义 Hook 是一种重用状态逻辑的机制(例如设置为订阅并存储当前值)，所以每次使用自定义 Hook 时，其中的所有 state 和副作用都是完全独立且隔离的。

##### Hook 规则

Hook 本质就是 JavaScript 函数，但是在使用它时需要遵循两条规则。

**1、只在最顶层使用 Hook**

**不要在循环，条件或嵌套函数中调用 Hook，** 确保总是在你的 React 函数的最顶层调用他们。遵守这条规则，你就能确保 Hook 在每一次渲染中都按照同样的顺序被调用。这让 React 能够在多次的 `useState` 和 `useEffect` 调用之间保持 hook 状态的正确。(如果你对此感到好奇，我们在[下面](https://react.docschina.org/docs/hooks-rules.html#explanation)会有更深入的解释。)

**2、只在 React 函数中调用 Hook**

**不要在普通的 JavaScript 函数中调用 Hook。**你可以：

- ✅ 在 React 的函数组件中调用 Hook
- ✅ 在自定义 Hook 中调用其他 Hook

对于第一条规则：

我们可以在单个组件中使用多个 State Hook 或 Effect Hook

```jsx
function Form() {
  // 1. Use the name state variable
  const [name, setName] = useState('Mary');

  // 2. Use an effect for persisting the form
  useEffect(function persistForm() {
    localStorage.setItem('formData', name);
  });

  // 3. Use the surname state variable
  const [surname, setSurname] = useState('Poppins');

  // 4. Use an effect for updating the title
  useEffect(function updateTitle() {
    document.title = name + ' ' + surname;
  });

  // ...
}
```

React 怎么知道哪个 state 对应哪个 `useState`？答案是 React 靠的是 Hook 调用的顺序。因为我们的示例中，Hook 的调用顺序在每次渲染中都是相同的，所以它能够正常工作：

```jsx
// ------------
// 首次渲染
// ------------
useState('Mary')           // 1. 使用 'Mary' 初始化变量名为 name 的 state
useEffect(persistForm)     // 2. 添加 effect 以保存 form 操作
useState('Poppins')        // 3. 使用 'Poppins' 初始化变量名为 surname 的 state
useEffect(updateTitle)     // 4. 添加 effect 以更新标题

// -------------
// 二次渲染
// -------------
useState('Mary')           // 1. 读取变量名为 name 的 state（参数被忽略）
useEffect(persistForm)     // 2. 替换保存 form 的 effect
useState('Poppins')        // 3. 读取变量名为 surname 的 state（参数被忽略）
useEffect(updateTitle)     // 4. 替换更新标题的 effect

// ...
```

只要 Hook 的调用顺序在多次渲染之间保持一致，React 就能正确地将内部 state 和对应的 Hook 进行关联。但如果我们将一个 Hook (例如 `persistForm` effect) 调用放到一个条件语句中会发生什么呢？

```jsx
// 🔴 在条件语句中使用 Hook 违反第一条规则
  if (name !== '') {
    useEffect(function persistForm() {
      localStorage.setItem('formData', name);
    });
  }
```

在第一次渲染中 `name !== ''` 这个条件值为 `true`，所以我们会执行这个 Hook。但是下一次渲染时我们**有可能**清空了表单，表达式值变为 `false`。此时的渲染会跳过该 Hook，Hook 的调用顺序发生了改变：

```jsx
useState('Mary')           // 1. 读取变量名为 name 的 state（参数被忽略）
// useEffect(persistForm)  // 🔴 此 Hook 被忽略！
useState('Poppins')        // 🔴 2 （之前为 3）。读取变量名为 surname 的 state 失败
useEffect(updateTitle)     // 🔴 3 （之前为 4）。替换更新标题的 effect 失败
```

这就是为什么 Hook 需要在我们组件的最顶层调用。如果我们想要有条件地执行一个 effect，可以将判断放到 Hook 的内部：

```jsx
  useEffect(function persistForm() {
    // 👍 将条件判断放置在 effect 中
    if (name !== '') {
      localStorage.setItem('formData', name);
    }
  });
```

**注意：如果使用了[提供的 lint 插件](https://www.npmjs.com/package/eslint-plugin-react-hooks)，就无需担心此问题。** 

**ESLint 插件**

我们发布了一个名为 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks) 的 ESLint 插件来强制执行这两条规则。如果你想尝试一下，可以将此插件添加到你的项目中：

我们打算后续版本默认添加此插件到 [Create React App](https://react.docschina.org/docs/create-a-new-react-app.html#create-react-app) 及其他类似的工具包中。

```
npm install eslint-plugin-react-hooks --save-dev
```

```json
// 你的 ESLint 配置
{
  "plugins": [
    // ...
    "react-hooks"
  ],
  "rules": {
    // ...
    "react-hooks/rules-of-hooks": "error", // 检查 Hook 的规则
    "react-hooks/exhaustive-deps": "warn" // 检查 effect 的依赖
  }
}
```

useEffect的依赖项数组是空数组的情况下（仅在组件挂载和卸载时执行），不需检查 effect 的依赖，则可以添加 `// eslint-disable-next-line`，例如：

```ts
useEffect(() => {
	// ...
  // eslint-disable-next-line
}, []);
```

##### Hook API

###### useState

```js
const [value, setValue] = useState(initialValue);
```

返回一个 value，以及更新 value的函数。

> 更新value要使用setValue函数，不能直接对value进行修改。

在初始渲染期间，返回的状态 (`value`) 与传入的第一个参数 (`initialValue`) 值相同。

`setValue` 函数用于更新 value。**它接收一个新的 state 值替换旧值**，并将组件的一次重新渲染加入队列。

```js
setValue(newValue);
```

在后续的重新渲染中，`useState` 返回的第一个值将始终是更新后最新的 value。

与 class 组件中的 `setState` 方法不同，Hook 的`setState` 不会自动合并更新对象。你可以用函数式的 `setState` 结合展开运算符来达到合并更新对象的效果。

```js
setValue(preValue => {
  // 也可以使用 Object.assign
  return {...preValue, ...updatedValues};
});
```

`useReducer` 是另一种可选方案，它更适合用于管理包含多个子值的 state 对象。

> 注意
>
> React 会确保 `setState` 函数的标识是稳定的，并且不会在组件重新渲染时发生变化。这就是为什么可以安全地从 `useEffect` 或 `useCallback` 的依赖列表中省略 `setState`。

**函数式更新**

如果新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 `setState`。该函数将接收先前的 state，并返回一个更新后的值。

```jsx
setCount(v => v + 1)
```

如果你的更新函数返回值与当前 state 完全相同，则随后的重渲染会被完全跳过。

与class组件的 `setState`一样，`useState`的`setState`在 React 管理的事件回调和生命周期中，setState 是异步的，而其他时候 setState 都是同步的。

批量更新 setState 时，多次执行 setState 只会触发一次 Render 过程。相反在立即更新 setState 时，每次 setState 都会触发一次 Render 过程，就存在性能影响。

假设有如下组件代码，该组件在 `getData()` 的 API 请求结果返回后，分别更新了两个 State 。

```jsx
function NormalComponent() {
  const [list, setList] = useState(null)
  const [info, setInfo] = useState(null)

  useEffect(() => {
    ;(async () => {
      const data = await getData()
      // 注意上面的await，下面的两个setXXX不在 React 管理的事件回调和生命周期中
      setList(data.list)
      setInfo(data.info)
    })()
  }, [])

  return (
    <div>
      非批量更新组件时 Render 次数：
      {renderOnce('normal')}
    </div>
  )
}
```

该组件会在 `setList(data.list)` 后触发组件的 Render 过程，然后在 `setInfo(data.info)` 后再次触发 Render 过程，造成性能损失。遇到该问题，开发者有两种实现批量更新的方式来解决该问题：

1. 将多个 State 合并为单个 State。例如 `useState({ list: null, info: null })` 替代 list 和 info 两个 State。
2. 使用 React 官方提供的 **unstable_batchedUpdates** 方法，将多次 setState 封装到 unstable_batchedUpdates 回调中。修改后代码如下。

```jsx
function BatchedComponent() {
  const [list, setList] = useState(null)
  const [info, setInfo] = useState(null)

  useEffect(() => {
    ;(async () => {
      const data = await getData()
      unstable_batchedUpdates(() => {
        setList(data.list)
        setInfo(data.info)
      })
    })()
  }, [])

  return (
    <div>
      批量更新组件时 Render 次数：
      {renderOnce('batched')}
    </div>
  )
```

在 React 并发模式中（实验性），将默认以批量更新方式执行 setState。到那时候，也可能就不需要这个优化了。

**惰性初始 state**

`initialState` 参数只会在组件的初始渲染中起作用，后续渲染时会被忽略。如果初始 state 需要通过复杂计算获得，则可以传入一个函数，在函数中计算并返回初始的 state，此函数只在初始渲染时被调用：

```js
const [state, setState] = useState(fn()); // fn() 每次渲染都会被调用
const [state, setState] = useState(() => fn()); // fn() 只会被调用一次
```

**跳过 state 更新**

调用 State Hook 的更新函数并传入当前的 state 时，React 将跳过子组件的渲染及 effect 的执行。（React 使用 [`Object.is` 比较算法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#Description) 来比较 state。）

需要注意的是，React 可能仍需要在跳过渲染前渲染该组件。不过由于 React 不会对组件树的“深层”节点进行不必要的渲染，所以大可不必担心。如果你在渲染期间执行了高开销的计算，则可以使用 `useMemo` 来进行优化。

> useState不能设置set和map

###### useEffect

```js
useEffect(didUpdate);
```

该 Hook 接收一个包含命令式、且可能有副作用代码的函数。

在函数组件主体内（这里指在 React 渲染阶段）改变 DOM、添加订阅、设置定时器、记录日志以及执行其他包含副作用的操作都是不被允许的，因为这可能会产生莫名其妙的 bug 并破坏 UI 的一致性。

使用 `useEffect` 完成副作用操作。赋值给 `useEffect` 的函数会在组件渲染到屏幕之后执行。

默认情况下，effect 将在每轮渲染结束后执行，但你可以选择让它 [在只有某些值改变的时候](https://react.docschina.org/docs/hooks-reference.html#conditionally-firing-an-effect) 才执行。

**清除 effect**

通常，组件卸载时需要清除 effect 创建的诸如订阅或计时器 ID 等资源。要实现这一点，`useEffect` 函数需返回一个清除函数。以下就是一个创建订阅的例子：

```js
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    // 清除订阅
    subscription.unsubscribe();
  };
});
```

为防止内存泄漏，清除函数会在组件卸载前执行。另外，如果组件多次渲染（通常如此），则**在执行下一个 effect 之前，上一个 effect 就已被清除**。在上述示例中，意味着组件的每一次更新都会创建新的订阅。若想避免每次更新都触发 effect 的执行，请参阅下一小节。

**effect 的执行时机**

与 `componentDidMount`、`componentDidUpdate` 不同的是，在浏览器完成布局与绘制**之后**，传给 `useEffect` 的函数会延迟调用。这使得它适用于许多常见的副作用场景，比如设置订阅和事件处理等情况，因此不应在函数中执行阻塞浏览器更新屏幕的操作。

然而，并非所有 effect 都可以被延迟执行。例如，在浏览器执行下一次绘制前，用户可见的 DOM 变更就必须同步执行，这样用户才不会感觉到视觉上的不一致。（概念上类似于被动监听事件和主动监听事件的区别。）React 为此提供了一个额外的 [`useLayoutEffect`](https://react.docschina.org/docs/hooks-reference.html#uselayouteffect) Hook 来处理这类 effect。它和 `useEffect` 的结构相同，区别只是调用时机不同。

虽然 `useEffect` 会在浏览器绘制后延迟执行，但会保证在任何新的渲染前刷新上一轮的 effect 。React 将在组件更新前刷新上一轮渲染的 effect。

**effect 的条件执行**

默认情况下，effect 会在每轮组件渲染完成后执行。这样的话，一旦 effect 的依赖发生变化，它就会被重新创建。

详情查看 **Effect  Hook ** 章节。

###### useContext

```js
const value = useContext(MyContext);
```

接收一个 context 对象（`React.createContext` 的返回值）并返回该 context 的当前值。当前的 context 值由上层组件中距离当前组件最近的 `<MyContext.Provider>` 的 `value` prop 决定。

当组件上层最近的 `<MyContext.Provider>` 更新时，该 Hook 会触发重渲染，并使用最新传递给 `MyContext` provider 的 context `value` 值。即使祖先使用 [`React.memo`](https://react.docschina.org/docs/react-api.html#reactmemo) 或 [`shouldComponentUpdate`](https://react.docschina.org/docs/react-component.html#shouldcomponentupdate)，也会在组件本身使用 `useContext` 时重新渲染。

调用了 `useContext` 的组件总会在 context 值变化时重新渲染。如果重渲染组件的开销较大，你可以 [通过使用 memoization 来优化](https://github.com/facebook/react/issues/15156#issuecomment-474590693)。

> 提示
>
> 如果你在接触 Hook 前已经对 context API 比较熟悉，那应该可以理解，`useContext(MyContext)` 相当于 class 组件中的 `static contextType = MyContext` 或者 `<MyContext.Consumer>`。
>
> `useContext(MyContext)` 只是让你能够*读取* context 的值以及订阅 context 的变化。你仍然需要在上层组件树中使用 `<MyContext.Provider>` 来为下层组件*提供* context。

**把如下代码与 Context.Provider 放在一起**

```jsx
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);
  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

###### useReducer

```js
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

[`useState`](https://react.docschina.org/docs/hooks-reference.html#usestate) 的替代方案。它接收一个形如 `(state, action) => newState` 的 reducer，并返回当前的 state 以及与其配套的 `dispatch` 方法。（如果你熟悉 Redux 的话，就已经知道它如何工作了。）

在某些场景下，`useReducer` 会比 `useState` 更适用，例如 state 逻辑较复杂且包含多个子值，或者下一个 state 依赖于之前的 state 等。并且，使用 `useReducer` 还能给那些会触发深更新的组件做性能优化，因为[你可以向子组件传递 `dispatch` 而不是回调函数](https://react.docschina.org/docs/hooks-faq.html#how-to-avoid-passing-callbacks-down) 。

以下是用 reducer 重写 [`useState`](https://react.docschina.org/docs/hooks-reference.html#usestate) 一节的计数器示例：

```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

> 注意
>
> React 会确保 `dispatch` 函数的标识是稳定的，并且不会在组件重新渲染时改变。这就是为什么可以安全地从 `useEffect` 或 `useCallback` 的依赖列表中省略 `dispatch`。



**指定初始 state**

有两种不同初始化 `useReducer` state 的方式，你可以根据使用场景选择其中的一种。将初始 state 作为第二个参数传入 `useReducer` 是最简单的方法：

```js
  const [state, dispatch] = useReducer(
    reducer,
    {count: initialCount}
  );
```

> 注意
>
> React 不使用 `state = initialState` 这一由 Redux 推广开来的参数约定。有时候初始值依赖于 props，因此需要在调用 Hook 时指定。如果你特别喜欢上述的参数约定，可以通过调用 `useReducer(reducer, undefined, reducer)` 来模拟 Redux 的行为，但我们不鼓励你这么做。

**惰性初始化**

你可以选择惰性地创建初始 state。为此，需要将 `init` 函数作为 `useReducer` 的第三个参数传入，这样初始 state 将被设置为 `init(initialArg)`。

这么做可以将用于计算 state 的逻辑提取到 reducer 外部，这也为将来对重置 state 的 action 做处理提供了便利：

```jsx
function init(initialCount) {  return {count: initialCount};}
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    case 'reset':      
      return init(action.payload);    
    default:
      throw new Error();
  }
}

function Counter({initialCount}) {
  const [state, dispatch] = useReducer(reducer, initialCount, init);  
  return (
    <>
      Count: {state.count}
      <button
        onClick={() => dispatch({type: 'reset', payload: initialCount})}>
  			Reset
      </button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

**跳过 dispatch**

如果 Reducer Hook 的返回值与当前 state 相同，React 将跳过子组件的渲染及副作用的执行。（React 使用 [`Object.is` 比较算法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#Description) 来比较 state。）

需要注意的是，React 可能仍需要在跳过渲染前再次渲染该组件。不过由于 React 不会对组件树的“深层”节点进行不必要的渲染，所以大可不必担心。如果你在渲染期间执行了高开销的计算，则可以使用 `useMemo` 来进行优化。

**useReducer的优势**

举一个例子：

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const [step, setStep] = useState(1);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + step); // 依赖其他state来更新
    }, 1000);
    return () => clearInterval(id);
    // 为了保证setCount中的step是最新的，
    // 我们还需要在deps数组中指定step
  }, [step]);

  return (
    <>
      <h1>{count}</h1>
      <input value={step} onChange={e => setStep(Number(e.target.value))} />
    </>
  );
}
```

这段代码能够正常工作，但是随着相互依赖的状态变多，**setState中的逻辑会变得很复杂**，**useEffect的deps数组也会变得更复杂**，降低可读性的同时，useEffect重新执行时机变得更加**难以预料**。

使用useReducer替代useState以后：

```jsx
function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const { count, step } = state;

  useEffect(() => {
    const id = setInterval(() => {
      dispatch({ type: 'tick' });
      }, 1000);
    return () => clearInterval(id);
  }, []); // deps数组不需要包含step

  return (
    <>
      <h1>{count}</h1>
      <input value={step} onChange={e => setStep(Number(e.target.value))} />
    </>
  )
}
```

**现在组件只需要发出action，而无需知道如何更新状态**。也就是将**What to do**与**How to do**解耦。彻底解耦的标志就是：useReducer总是返回**相同的dispatch函数**（发出action的渠道），不管reducer（状态更新的逻辑）如何变化。

> 这是useReducer的逆天之处之一，下面会详述

另一方面，step的更新不会造成useEffect的失效、重执行。因为现在useEffect依赖于dispatch，而不依赖于**状态值**（得益于上面的解耦模式）。**这是一个重要的模式，能用来避免useEffect、useMemo、useCallback需要频繁重执行的问题**。

以下是state的定义，其中reducer封装了“如何更新状态”的逻辑：

```js
const initialState = {
  count: 0,
  step: 1,
};

function reducer(state, action) {
  const { count, step } = state;
  if (action.type === 'tick') {
    return { count: count + step, step };
  } else if (action.type === 'step') {
    return { count, step: action.step };
  } else {
    throw new Error();
  }
}
```

总结：

- 当状态更新逻辑比较复杂的时候，就应该考虑使用useReducer。因为：
  - **reducer比setState更加擅长描述“如何更新状态”**。比如，reducer能够读取相关的状态、同时更新多个状态。
  - 【组件负责发出action，reducer负责更新状态】的解耦模式，使得代码逻辑变得更加清晰，代码行为更加可预测(比如useEffect的更新时机更加稳定)。
  - 简单来记，就是每当编写`setState(prevState => newState)`的时候，就应该考虑是否值得将它换成useReducer。
- **通过传递useReducer的dispatch，可以减少状态值的传递**。
  - useReducer总是返回**相同的dispatch函数**，这是彻底解耦的标志：状态更新逻辑可以任意变化，而发起actions的渠道始终不变
  - 得益于前面的解耦模式，useEffect函数体、callback function只需要使用dispatch来发出action，而无需直接依赖**状态值**。因此在useEffect、useCallback、useMemo的deps数组中无需包含**状态值**，也减少了它们更新的需要。不但能提高可读性，而且能提升性能（useCallback、useMemo的更新往往会造成子组件的刷新）。



###### useMemo

```js
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

返回一个 [memoized](https://en.wikipedia.org/wiki/Memoization) 值。

把“创建”函数和依赖项数组作为参数传入 `useMemo`，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。

记住，传入 `useMemo` 的函数会在渲染期间执行。请不要在这个函数内部执行与渲染无关的操作，诸如副作用这类的操作属于 `useEffect` 的适用范畴，而不是 `useMemo`。

如果没有提供依赖项数组，`useMemo` 在每次渲染时都会计算新的值。

**你可以把 `useMemo` 作为性能优化的手段，但不要把它当成语义上的保证。useMemo 允许你记住一次昂贵的计算。但是，这仅作为一种提示，并不保证计算不会重新运行。**将来，React 可能会选择“遗忘”以前的一些 memoized 值，并在下次渲染时重新计算它们，比如为离屏组件释放内存。先编写在没有 `useMemo` 的情况下也可以执行的代码 —— 之后再在你的代码中添加 `useMemo`，以达到优化性能的目的。

> 注意
>
> 依赖项数组不会作为参数传给“创建”函数。虽然从概念上来说它表现为：所有“创建”函数中引用的值都应该出现在依赖项数组中。未来编译器会更加智能，届时自动创建数组将成为可能。
>
> 我们推荐启用 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation) 中的 [`exhaustive-deps`](https://github.com/facebook/react/issues/14920) 规则。此规则会在添加错误依赖时发出警告并给出修复建议。

useMemo 与 React.memo 类似，但与 React.memo 相比有以下优势：

1. 更方便。React.memo 需要对组件进行一次包装，生成新的组件（React.memo 是高价组件）。而 useMemo 只需在存在性能瓶颈的地方使用，不用修改组件。
2. 更灵活。useMemo 不用考虑组件的所有 Props，而只需考虑当前场景中用到的值。

**useMemo 使用**

我们来看一个反例：

```jsx
import React from 'react';
export default function WithoutMemo() {
    const [count, setCount] = useState(1);
    const [val, setValue] = useState('');
 
    function expensive() {
        console.log('compute');
        let sum = 0;
        for (let i = 0; i < count * 100; i++) {
            sum += i;
        }
        return sum;
    }
 
    return <div>
        <h4>{count}-{val}-{expensive()}</h4>
        <div>
            <button onClick={() => setCount(count + 1)}>+c1</button>
            <input value={val} onChange={event => setValue(event.target.value)}/>
        </div>
    </div>;
}
```

这里创建了两个state，然后通过expensive函数，执行一次昂贵的计算，拿到count对应的某个值。我们可以看到：无论是修改count还是val，由于组件的重新渲染，都会触发expensive的执行(能够在控制台看到，即使修改val，也会打印)；但是这里的昂贵计算只依赖于count的值，在val修改的时候，是没有必要再次计算的。在这种情况下，我们就可以使用useMemo，只在count的值修改时，执行expensive计算：

```jsx
export default function WithMemo() {
    const [count, setCount] = useState(1);
    const [val, setValue] = useState('');
    const expensive = useMemo(() => function(){
        console.log('compute');
        let sum = 0;
        for (let i = 0; i < count * 100; i++) {
            sum += i;
        }
        return sum;
    }, [count]);
 
    return <div>
        <h4>{count}-{expensive}</h4>
        {val}
        <div>
            <button onClick={() => setCount(count + 1)}>+c1</button>
            <input value={val} onChange={event => setValue(event.target.value)}/>
        </div>
    </div>;
}
```

上面我们可以看到，使用useMemo来执行昂贵的计算，然后将计算值返回，并且将count作为依赖值传递进去。这样，就只会在count改变的时候触发expensive执行，在修改val的时候，返回上一次缓存的值。

###### useCallback

```js
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

返回一个 [memoized](https://en.wikipedia.org/wiki/Memoization) 回调函数。

把内联回调函数及依赖项数组作为参数传入 `useCallback`，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会返回新的函数。当你把回调函数传递给经过优化的并使用引用相等性去避免非必要渲染（例如 `shouldComponentUpdate`）的子组件时，它将非常有用。

`useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`。

> 注意
>
> 依赖项数组不会作为参数传给回调函数。虽然从概念上来说它表现为：所有回调函数中引用的值都应该出现在依赖项数组中。未来编译器会更加智能，届时自动创建数组将成为可能。
>
> 我们推荐启用 [`eslint-plugin-react-hooks`](https://www.npmjs.com/package/eslint-plugin-react-hooks#installation) 中的 [`exhaustive-deps`](https://github.com/facebook/react/issues/14920) 规则。此规则会在添加错误依赖时发出警告并给出修复建议。

**useCallabck 使用**

当依赖变更时，会返回新的函数。既然返回的是函数，我们无法很好的判断返回的函数是否变更，所以我们可以借助ES6新增的数据类型Set来判断：

```js
const set = new Set();
const callback = useCallback(() => {
        console.log(count);
    }, [count]);
let flag = set.has(callback); // 由于 Set 集合不允许有重复的元素，在执行has方法时候，如果这个元素已经在set中存在，那么就返回true，否则返回false。
```

使用场景是：有一个父组件，其中包含子组件，子组件接收一个函数作为props；通常而言，如果父组件更新了，子组件也会执行更新；但是大多数场景下，更新是没有必要的，我们可以借助useCallback来返回函数，然后把这个函数作为props传递给子组件；这样，子组件就能避免不必要的更新。

```jsx
import React, { useState, useCallback, useEffect } from 'react';
function Parent() {
    const [count, setCount] = useState(1);
    const [val, setVal] = useState('');
 
    const callback = useCallback(() => {
        return count;
    }, [count]);
    return <div>
        <h4>{count}</h4>
        <Child callback={callback}/>
        <div>
            <button onClick={() => setCount(count + 1)}>+</button>
            <input value={val} onChange={event => setVal(event.target.value)}/>
        </div>
    </div>;
}
 
function Child({ callback }) {
    const [count, setCount] = useState(() => callback());
    useEffect(() => {
        setCount(callback());
    }, [callback]);
    return <div>
        {count}
    </div>
}
```

useMemo和useCallback都会在组件第一次渲染的时候执行，之后会在其依赖的变量发生改变时再次执行；并且这两个hooks都返回缓存的值，useMemo返回缓存的变量，useCallback返回缓存的函数。useEffect 可以处理副作用，useMemo和useCallback不可以。



###### useRef

```js
const refContainer = useRef(initialValue);
```

`useRef` 返回一个可变的 ref 对象，其 `.current` 属性被初始化为传入的参数（`initialValue`）。返回的 ref 对象在组件的整个生命周期内保持不变。而且 useRef 实现了类似 class 中的 `this` 的功能。

使用场景：useRef( ).current 可以跨越渲染周期存储数据（在current 上增加属性存储对象，current 可以保存任何可变值，当其被赋值给 DOM 元素或组件的 ref 属性，current 会指向 DOM 元素或组件实例），而且对 current 修改也不会引起组件渲染。

> React.createRef 每次渲染都会返回一个新的引用，而 useRef 每次都会返回相同的引用。

本质上，`useRef` 就像是可以在其 `.current` 属性中保存一个可变值的“盒子”。

一个常见的用例便是命令式地访问子组件：

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

`useRef`创建了`inputEl`对象，并将其赋给了`input`的`ref`属性。这样，通过访问`inputEl.current`就可以访问到`button`对应的DOM对象。

然后再来看看它保存数据的用法。

```jsx
import React, { useState, useEffect, useMemo, useRef } from 'react';

function App(props){
  const [count, setCount] = useState(0);

  const doubleCount = useMemo(() => {
    return 2 * count;
  }, [count]);

  const timerID = useRef();
  
  useEffect(() => {
    timerID.current = setInterval(()=>{
        setCount(count => count + 1);
    }, 1000); 
  }, []);
  
  useEffect(()=>{
      if(count > 10){
          clearInterval(timerID.current);
      }
  });
  
  return (
   // ...
  );
}

```

在上面的例子中，我用`ref`对象的`current`属性来存储定时器的ID，这样便可以在多次渲染之后依旧保存定时器ID，从而能正常清除定时器。

`useRef()` 比 `ref` 属性更有用。它可以[很方便地保存任何可变值](https://react.docschina.org/docs/hooks-faq.html#is-there-something-like-instance-variables)，其类似于在 class 中使用实例字段的方式。

这是因为它创建的是一个普通 Javascript 对象。而 `useRef()` 和自建一个 `{current: ...}` 对象的唯一区别是，`useRef` 会在每次渲染时返回同一个 ref 对象。

请记住，当 ref 对象内容发生变化时，`useRef` 并*不会*通知你。变更 `.current` 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用[回调 ref](https://react.docschina.org/docs/hooks-faq.html#how-can-i-measure-a-dom-node) 来实现。

###### useImperativeHandle

```js
useImperativeHandle(ref, createHandle, [deps])
```

`useImperativeHandle` 应当与 [`forwardRef`](https://react.docschina.org/docs/react-api.html#reactforwardref) 一起使用，forwardRef 会创建一个React组件，这个组件能够将其接受的 ref 属性转发到其组件树下的另一个组件中（将子组件的DOM直接暴露给了父组件）。

forwardRef的做法本身没有什么问题, 但是我们是将子组件的DOM直接暴露给了父组件:

- 直接暴露给父组件带来的问题是某些情况的不可控
- 父组件可以拿到DOM后进行任意的操作
- 我们只是希望父组件可以操作某一项方法，其他并不希望它随意操作其他方法

因此，useImperativeHandle 的出现解决了该问题，使用总结:

- 作用：减少暴露给父组件获取的DOM元素属性, 只暴露给父组件需要用到的方法
- 参数1：父组件传递的ref属性
- 参数2：一个函数，返回值是一个对象，以供给父组件中通过ref.current调用该对象中的方法。即定义ref.current。
- 参数3：依赖项数组，只有当第三个参数发生改变时，父组件接收到的值会发生改变，才会重新渲染

```jsx
import React, { useRef, forwardRef, useImperativeHandle } from 'react'

const JMInput = forwardRef((props, ref) => {
  const inputRef2 = useRef()
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef2.current.focus()
    },
  }))
  return <input type="text" ref={inputRef2} />
})

export default function ImperativeHandleDemo() {
  // useImperativeHandle 主要作用:用于减少父组件中通过forward+useRef获取子组件DOM元素暴露的属性过多
  // 为什么使用: 因为使用forward+useRef获取子函数式组件DOM时,获取到的dom属性暴露的太多了
  // 解决: 使用uesImperativeHandle解决,在子函数式组件中定义父组件需要进行DOM操作,减少获取DOM暴露的属性过多
  const inputRef1 = useRef()

  return (
    <div>
      <button
        onClick={inputRef1.current.focus}
      >父组件调用子组件的focus</button>
      <JMInput ref={inputRef1} />
    </div>
  )
}
```

###### useLayoutEffect

其函数签名与 `useEffect` 相同，但它会在所有的 DOM 变更之后同步调用 effect。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，`useLayoutEffect` 内部的更新计划将被同步刷新。

**useEffect 和 useLayoutEffect 的区别？**

 `useEffect` 的回调函数会在浏览器绘制之后延迟执行。

 `useLayoutEffect` 的回调函数会在浏览器执行绘制之前同步执行，会阻塞浏览器的绘制。

`useLayoutEffect` 与 `componentDidMount`、`componentDidUpdate` 的调用阶段是一样的。

`useLayoutEffect` 适用于用户可见的 DOM 变更场景（在浏览器执行下一次绘制前，用户可见的 DOM 变更必须同步执行，这样用户才不会感觉到视觉上的不一致），但是会在浏览器进行任何绘制之前运行完成，阻塞了浏览器的绘制：

DOM 已经被修改，但浏览器渲染线程依旧处于被阻塞阶段，所以还没有发生回流、重绘过程。由于内存中的 DOM 已经被修改，通过 useLayoutEffect 可以拿到最新的 DOM 节点，并且在此时对 DOM 进行样式上的修改，所有的更改被一次性渲染到屏幕上，依旧只有一次回流、重绘的代价。

如果放在 useEffect 里，useEffect 的函数会**在组件渲染到屏幕之后执行**，此时对 DOM 进行修改，会触发浏览器再次进行回流、重绘，增加了性能上的损耗。

> 提示
>
> 如果你正在将代码从 class 组件迁移到使用 Hook 的函数组件，则需要注意 `useLayoutEffect` 与 `componentDidMount`、`componentDidUpdate` 的调用阶段是一样的。但是，我们推荐你**一开始先用 `useEffect`**，只有当它出问题的时候再尝试使用 `useLayoutEffect`。

###### useDebugValue（了解）

```js
useDebugValue(value)
```

`useDebugValue` 可用于在 React 开发者工具中显示自定义 hook 的标签。

```js
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  // ...

  // 在开发者工具中的这个 Hook 旁边显示标签  // e.g. "FriendStatus: Online"
  useDebugValue(isOnline ? 'Online' : 'Offline');
  return isOnline;
}
```

**延迟格式化 debug 值**

在某些情况下，格式化值的显示可能是一项开销很大的操作。除非需要检查 Hook，否则没有必要这么做。

因此，`useDebugValue` 接受一个格式化函数作为可选的第二个参数。该函数只有在 Hook 被检查时才会被调用。它接受 debug 值作为参数，并且会返回一个格式化的显示值。

例如，一个返回 `Date` 值的自定义 Hook 可以通过格式化函数来避免不必要的 `toDateString` 函数调用：

```js
useDebugValue(date, date => date.toDateString());
```

##### Hook FAQ

**如何获取上一轮的 props 或 state？**

目前，你可以 [通过 ref](https://react.docschina.org/docs/hooks-faq.html#is-there-something-like-instance-variables) 来手动实现：

```jsx
function Counter() {
  const [count, setCount] = useState(0);

  const prevCountRef = useRef(); // 返回的 ref 对象在组件的整个生命周期内保持不变
  useEffect(() => {
    prevCountRef.current = count;
  });
  const prevCount = prevCountRef.current;
  return <h1>Now: {count}, before: {prevCount}</h1>;
}
```

你可以把它抽取成一个自定义 Hook：

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);  
  return <h1>Now: {count}, before: {prevCount}</h1>;
}

function usePrevious(value) {  
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```

而且 setState 的函数形式可以获取上一轮的 state

```js
setState(preState=>{
	// ...
})
```

**想要从异步回调中读取最新的 state**

你可以用 useRef() 来保存它，修改它，并从中读取。

**useReducer**

在一些更加复杂的场景中（比如一个 state 依赖于另一个 state），尝试用 [`useReducer` Hook](https://react.docschina.org/docs/hooks-reference.html#usereducer) 把 state 更新逻辑移到 effect 之外。[这篇文章](https://adamrackis.dev/state-and-use-reducer/) 提供了一个你该如何做到这一点的案例。 **`useReducer` 的 `dispatch` 的身份永远是稳定的** —— 即使 reducer 函数是定义在组件内部并且依赖 props。

**你或许想要重新创建或避免重新创建 `useRef()` 的初始值**

```jsx
// 重新创建 uesRef 的初始值
function Image(props) {
  // Demo 实例在每次渲染都会被创建
  const ref = useRef(new Demo());
  // ...
}
```

```jsx
// 避免重新创建 uesRef 的初始值
function Image(props) {
  const ref = useRef(null);

  // Demo 只会被惰性创建一次
  function getDemo() {
    if (ref.current === null) {
      ref.current = new Demo();
    }
    return ref.current;
  }

  // 当你需要时，调用 getDemo()
  // ...
}
```

#### FAQ

**为什么箭头函数渲染不到组件**

可能是花括号的问题。

箭头函数返回组件：

```jsx
// 正确
()=>(
	<div></div>
)
// 或

()=>{
  return (
    <div></div>
  )
}

// 错误
()=>{
	<div></div>
}
```

**怎样阻止函数被调用太快或者太多次？**

如果你有一个 `onClick` 或者 `onScroll` 这样的事件处理器，想要阻止回调被触发的太快，那么可以限制执行回调的速度，可以通过以下几种方式做到这点：

- **节流**：基于时间的频率来进行抽样更改 (例如 [`_.throttle`](https://lodash.com/docs#throttle))
- **防抖**：一段时间的不活动之后发布更改 (例如 [`_.debounce`](https://lodash.com/docs#debounce))
- **`requestAnimationFrame` 节流**：基于 requestAnimationFrame 的抽样更改 (例如 [raf-schd](https://react.docschina.org/docs/[](https://github.com/alexreardon/raf-schd)))

可以看这个比较 throttle 和 debounce 的[可视化页面](http://demo.nimius.net/debounce_throttle/)

> **注意：**
>
> `_.debounce`、`_.throttle` 和 `raf-schd` 都提供了一个 `cancel` 方法来取消延迟回调。你需要在 `componentWillUnmount` 中调用该方法，或者对代码进行检查来保证在延迟函数有效期间内组件始终挂载。

**`requestAnimationFrame` 节流**

[`requestAnimationFrame`](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) 是在浏览器中排队等待执行的一种方法，它可以在呈现性能的最佳时间执行。一个函数被 `requestAnimationFrame` 放入队列后将会在下一帧触发。浏览器会努力确保每秒 60 帧（60fps）。然而，如果浏览器无法确保，那么自然会*限制*每秒的帧数。例如，某个设备可能只能处理每秒 30 帧，所以每秒只能得到 30 帧。使用 `requestAnimationFrame` 来节流是一种有用的技术，它可以防止在一秒中进行 60 帧以上的更新。如果一秒钟内完成 100 次更新，则会为浏览器带来额外的负担，而用却户无法感知到这些工作。

**项目文件结构**

**不要过度思考**

对于个人开发，如果你刚刚开始一个项目，不要花费过多时间在选择项目文件组织结构上。选择上述任何方式（或提出自己的方式）并开始编写代码！因为，在你编写了一些真正的代码之后，你将很有可能会重新考虑它。

如果您感觉完全卡住，请先将所有文件保存在同一个文件夹中。它最终会变得足够大，以至于让你想要将其中一些文件拆分出去。到那时，你将有足够的知识去区分你最频繁编辑的文件。通常，将经常一起变化的文件组织在一起是个好主意。这个原则被称为 “colocation”。

随着项目规模的扩大，人们通常会在实践中混搭使用上述这些方式。因此，在开始时选择“正确”的那个方式并不是很重要。

对于团队开发，有一个项目文件结构是一种开发约定和规范，避免后期过程整合的混乱，但是随着项目规模的扩大，起初约定的一系列也会不尽人意，需要通过彼此沟通新的约定。

**如果出现 Invalid hook call. Hooks can only be called inside of the body of a function**

尝试给react这个包起别名：

```js
module.exports = {
  webpack: {
    // 别名
    alias: {
      'react': path.resolve('./node_modules/react'),
    },
  }
}
```

#### [性能优化](https://mp.weixin.qq.com/s?__biz=Mzg2NDAzMjE5NQ==&mid=2247488520&idx=1&sn=726952e0144efa93bf60dc9faf7631f6&chksm=ce6ed0a4f91959b22719d6679d2efec24f51cbbfa5d0cf027e2f087b6b686e6834c609e38ee6&mpshare=1&scene=1&srcid=0331U6V7NKeG2nd5DVXIwKVp&sharer_sharetime=1617795727517&sharer_shareid=27dcefb05bb42e9bc0099d713541b753&key=57e1fc72bbd501350975280e1b5bf504d6ec8bcefe7c46df6ee28c16dcc462d3d4e88cec8be61b8bd5028a2ce4a710ff34e5ef2266b635a7b7310050aca9187a6b682594413eb8d26c1d15aa81cdce249d4035f12fcc406aa2027858a80afa418cfd22690beec3bbc99123e76e46101079aa70253de1c1291f1132c5116f93ab&ascene=1&uin=NjMxODk4NDM4&devicetype=Windows+7+x64&version=62090529&lang=zh_CN&exportkey=AU4S22Wf9vC9t8m2TmZ2m64%3D&pass_ticket=qd6n4VfoLUbV%2BmeLBAR6v9m0XmSt6qVoS7gykW3W0rq9g8Jbr2vJ5T7BWLiKD1F8&wx_header=0)

##### React工作流

https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

**调和阶段**

该阶段不允许有副作用。

1、计算出目标 State 对应的虚拟 DOM 结构。

2、寻找「将虚拟 DOM 结构修改为目标虚拟 DOM 结构」的最优更新方案。 

React 按照深度优先遍历虚拟 DOM 树的方式，在一个虚拟 DOM 上完成两件事的计算后，再计算下一个虚拟 DOM。

第一件事主要是调用类组件的 render 方法或函数组件自身。

第二件事为 React 内部实现的 Diff 算法，Diff 算法会记录虚拟 DOM 的更新方式（如：Update、Mount、Unmount），为提交阶段做准备。

**预提交阶段**

可以读取DOM。

**提交阶段**

可以处理DOM，执行副作用，计划更新。

1、将调和阶段记录的更新方案应用到 DOM 中。

2、调用暴露给开发者的钩子方法，如：componentDidUpdate、useLayoutEffect 等。 

提交阶段中这两件事的执行时机与调和阶段不同，在提交阶段 React 会先执行 1，等 1 完成后再执行 2。

在子组件的 componentDidMount 方法中，可以执行 `document.querySelector('.parentClass')` ，拿到父组件渲染的 `.parentClass` DOM 节点，尽管这时候父组件的 componentDidMount 方法还没有被执行。（调和阶段以及计算出虚拟DOM）。useLayoutEffect 的执行时机与 componentDidMount 相同。

##### PureComponent、React.memo

在 React 工作流中，如果只有父组件发生状态更新，即使父组件传给子组件的所有 Props 都没有修改，也会引起子组件的 Render 过程。

从 React 的声明式设计理念来看，如果子组件的 Props 和 State 都没有改变，那么其生成的 DOM 结构和副作用也不应该发生改变。当子组件符合声明式设计理念时，就可以忽略子组件本次的 Render 过程。

PureComponent 和 React.memo 就是应对这种场景的，PureComponent 是对类组件的 Props 和 State 进行浅比较，React.memo 是对函数组件的 Props 进行浅比较。

##### shouldComponentUpdate

在项目初始阶段，开发者往往图方便会给子组件传递一个大对象作为 Props，后面子组件想用啥就用啥。

当大对象中某个「子组件未使用的属性」发生了更新，子组件也会触发 Render 过程。

在这种场景下，通过实现子组件的 shouldComponentUpdate 方法，仅在「子组件使用的属性」发生改变时才返回 `true`，便能避免子组件重新 Render。

但使用 shouldComponentUpdate 优化有个弊端。

如果存在很多子孙组件，「找出所有子孙组件使用的属性」就会有很多工作量，也容易因为漏测导致 bug。

##### useMemo、useCallback 实现稳定的 Props 值

如果传给子组件的状态或函数，每次都是新的引用，那么 PureComponent 和 React.memo 优化就会失效。所以需要使用 useMemo 和 useCallback 来生成稳定值（当它的依赖未发生改变时，就不会触发重新计算），并结合 PureComponent 或 React.memo 避免子组件重新 Render。

##### useMemo 返回虚拟 DOM 可跳过该组件 Render 过程

利用 useMemo 可以缓存计算结果的特点，如果 useMemo 返回的是组件的虚拟 DOM，则将在 useMemo 依赖不变时，跳过组件的 Render 阶段。

该方式与 React.memo 类似，但与 React.memo 相比有以下优势：

1. 更方便。React.memo 需要对组件进行一次包装，生成新的组件（React.memo 是高价组件）。而 useMemo 只需在存在性能瓶颈的地方使用，不用修改组件。
2. 更灵活。useMemo 不用考虑组件的所有 Props，而只需考虑当前场景中用到的值。

##### 列表项使用 key 属性

那么是否在所有列表渲染的场景下，使用 ID 都优于使用索引呢？答案是否定的，在常见的分页列表中，第一页和第二页的列表项 ID 都是不同，假设每页展示三条数据，那么切换页面前后组件 Render 结果如下。

```html
<!-- 第一页的列表项虚拟 DOM -->
<li key="a">dataA</li>
<li key="b">dataB</li>
<li key="c">dataC</li>

<!-- 切换到第二页后的虚拟 DOM -->
<li key="d">dataD</li>
<li key="e">dataE</li>
<li key="f">dataF</li>
```

切换到第二页后，由于所有 `<li>` 的 key 值不同，所以 Diff 算法会将第一页的所有 DOM 节点标记为删除，然后将第二页的所有 DOM 节点标记为新增。

整个更新过程需要三次 DOM 删除、三次 DOM 创建。如果不使用 key，Diff 算法只会将三个 `<li>` 节点标记为更新，执行三次 DOM 更新。

使用 key 时每次翻页耗时约为 140ms，而不使用 key 仅为 70ms。尽管存在以上场景，React 官方仍然推荐使用 ID 作为每项的 key 值。其原因有两：

1. 在列表中执行删除、插入、排序列表项的操作时，使用 ID 作为 key 将更高效。而翻页操作往往伴随着 API 请求，DOM 操作耗时远小于 API 请求耗时，是否使用 key 在该场景下对用户体验影响不大。

2. 使用 ID 做为 key 可以维护该 ID 对应的列表项组件的 State。举个例子，某表格中每列都有普通态和编辑态两个状态，起初所有列都是普通态，用户点击第一行第一列，使其进入编辑态。然后用户又拖拽第二行，将其移动到表格的第一行。如果开发者使用索引作为 key，那么第一行第一列的状态仍然为编辑态，而用户实际希望编辑的是第二行的数据，在用户看来就是不符合预期的。

   **即列表项不存在操作交互时，不使用key会更高效，存在操作时必须使用key，且不能使用索引作为key值。**

##### 批量更新，减少 Render 次数

在 React 管理的事件回调和生命周期中，setState 是异步的，而其他时候 setState 都是同步的。

 对于同步的setState可使用 unstable_batchedUpdates 方法，将多次 setState 封装到 unstable_batchedUpdates 回调中。

```js
 useEffect(() => {
    ;(async () => {
      const data = await getData()
      unstable_batchedUpdates(() => {
        setList(data.list)
        setInfo(data.info)
      })
    })()
  }, [])
```

##### 按优先级更新，及时响应用户

```js
const fastHandle = () => {
  // 优先响应用户行为
  setShowInput(false)
  // 将耗时任务移动到下一个宏任务执行
  setTimeout(() => {
    setNumbers([...numbers, +inputValue].sort((a, b) => a - b))
  })
}
```



##### 组件懒加载

实现懒加载优化时，不仅要考虑加载态，还需要对加载失败进行容错处理。

```jsx
// MyErrorBoundary.jsx
// 对加载失败进行容错处理
class MyErrorBoundary extends Component {
  constructor(props) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError(error) {
    return { hasError: true }
  }

  render() {
    if (this.state.hasError) {
      return <h1>这里处理出错场景</h1>
    }

    return this.props.children
  }
}

// other.jsx
import React, { Suspense , lazy } from 'react';
import MyErrorBoundary from './MyErrorBoundary';

const OtherComponent = lazy(() => import('./OtherComponent'));

const MyComponent = () => (
  <div>
    <MyErrorBoundary>
      <Suspense fallback={<div>Loading...</div>}>
        <section>
          <OtherComponent />
        </section>
      </Suspense>
    </MyErrorBoundary>
  </div>
);
```

##### 谨慎使用 Context

Context 是跨组件传值的一种方案，但我们需要知道，我们无法阻止 Context 触发的 render。

不像 props 和 state，React 提供了 API 进行浅比较，避免无用的 render，Context 完全没有任何方案可以避免无用的渲染。

有几点关于 Context 的建议：

- Context 只放置必要的，关键的，被大多数组件所共享的状态。
- 对非常昂贵的组件，建议在父级获取 Context 数据，通过 props 传递进来。

##### debounce、throttle 优化频繁触发的回调

在搜索场景中，只需响应用户最后一次输入，无需响应用户的中间输入值，debounce 更适合使用在该场景中。

而 throttle 更适合需要实时响应用户的场景中更适合，如通过拖拽调整尺寸或通过拖拽进行放大缩小（如：window 的 resize 事件）。

实时响应用户操作场景中，如果回调耗时小，甚至可以用  requestAnimationFrame 代替 throttle。

##### 动画库直接修改 DOM 属性，跳过组件 Render 阶段

这个优化在业务中应该用不上，但还是非常值得学习的，将来可以应用到组件库中。

 [react-spring](https://github.com/pmndrs/react-spring) 库，当一个动画启动后，每次动画属性改变不会引起组件重新 Render ，而是直接修改了 dom 上相关属性值。

#### 术语表

##### 无状态组件

无state，无生命周期， 接收来自父组件props传递过来的数据，无状态组件主要用来定义模板。

##### 派生状态

如果一个组件的state中的某个数据来自prop，就将该数据称之为派生状态。

[你可能不需要使用派生 state](https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#common-bugs-when-using-derived-state)

##### Memoization

在计算机领域，记忆（memoization）是主要用于加速程序计算的一种优化技术，它使得函数避免重复演算之前已被处理过的输入，而返回已缓存的结果。

`Memoization` 的原理就是把函数的每次执行结果都放入一个对象中，在接下来的执行中，在对象中查找是否已经有相应执行过的值，如果有，直接返回该值，没有才真正执行函数体的求值部分。在对象里找值是要比执行函数的速度要快的。

另外，`Memoization` 只适用于确定性算法，对于相同的输入总是生成相同的输出，即纯函数。

##### 副作用（side effect）

副作用就是对所处的环境有改变：

函数副作用是指函数被调用，完成了函数既定的计算任务，但同时因为访问了外部数据，尤其是因为对外部数据进行了写操作，从而改变了外部环境。 

副作用函数的对立面是纯函数。

##### 纯函数

相同的输入（参数），永远会得到相同的输出，并且在执行过程里面没有副作用 。

设置订阅以及手动更改 React 组件中的 DOM 等改变了外部环境的都属于副作用。

##### **[协调](https://react.docschina.org/docs/reconciliation.html)**

当组件的 props 或 state 发生变化时，React 通过将最新返回的元素与原先渲染的元素进行比较，来决定是否有必要进行一次实际的 DOM 更新。当它们不相等时，React 才会更新 DOM。这个过程被称为“协调”。

##### 函数签名

一个**函数签名 (**或类型签名，或方法签名**)** 定义了 [函数](https://developer.mozilla.org/zh-CN/docs/Glossary/Function) 或 [方法](https://developer.mozilla.org/zh-CN/docs/Glossary/Method) 的输入与输出。

一个签名可以包括：

- [参数](https://developer.mozilla.org/en-US/docs/Glossary/Parameter) 及参数的 [类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Type)
- 一个返回值及其类型
- 可能会抛出或传回的 [异常](https://developer.mozilla.org/en-US/docs/Glossary/Exception)
- 有关 [面向对象](https://developer.mozilla.org/zh-CN/docs/Glossary/面向对象编程) 程序中方法可用性的信息 (例如关键字 `public`、`static` 或 `prototype)`。

##### 订阅

和原生JS事件监听一个意思，不过美其名曰订阅/发布模式，或者叫观察者模式。

原生JS，比如你监听了一个onclick事件，那么触发onclick事件后进行回调的函数就和你这里的订阅一个意思，都是指动作发生后，需要执行的回调函数。



有状态的组件没有渲染：
包含实际业务状态的组件不应该进行视图的渲染，而是将状态作为参数传递给子孙组件，应该让子孙组件来进行视图渲染；

有渲染的组件没有状态：
能够进行视图渲染的组件，不要包含实际的业务状态，而是通过接受父辈的参数来进行渲染；
这样的话，有渲染的组件没有实际的业务状态，就与实际的业务解耦了，能够更好的服务于其他的有状态的组件，实现组件的复用。



#### 浏览器支持

React 支持所有的现代浏览器，包括 IE9 及以上版本，但是需要为旧版浏览器比如 IE9 和 IE10 引入[相关的 polyfills 依赖](https://react.docschina.org/docs/javascript-environment-requirements.html)。不支持那些不兼容 ES5 方法的旧版浏览器。

#### JavaScript 环境要求

React 16 依赖集合类型 [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) 和 [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) 。如果你要支持无法原生提供这些能力（例如 IE < 11）或实现不规范（例如 IE 11）的旧浏览器与设备，考虑在你的应用库中包含一个全局的 polyfill ，例如 [core-js](https://github.com/zloirock/core-js) 或 [babel-polyfill](https://babeljs.io/docs/usage/polyfill/) 。

React 同时还依赖于 `requestAnimationFrame`（甚至包括测试环境）。 

#### Hook的优缺点

#### React的ESlint配置（JS）

##### 项目需要安装的插件

```
"babel-eslint",
"eslint",
"eslint-plugin-react"
```

##### 配置详情

下面的配置涵盖了开发者所需要的绝大部分信息，rules中的值0、1、2分别表示不开启检查、警告、错误。你可以看到下面有些是0，如果有需要开启检查，可以自己修改为1或者2。

```ts
module.exports = {
    // 启用的环变量
    "env": {
        "browser": true,
        "commonjs": true,
        "es6": true
    },
    'extends': ['eslint:recommended', 'plugin:react/recommended'],
    // 启用的全局变量
    "globals": {
        "$": true,
        "process": true,
        "__dirname": true
    },
    "parser": "babel-eslint",
    "parserOptions": {
        "ecmaFeatures": {
            "experimentalObjectRestSpread": true,
            "jsx": true
        },
        "sourceType": "module",
        "ecmaVersion": 7
    },
    "plugins": [
        "react"
    ],
    "rules": {
        "quotes": [2, "single"], //单引号
        "no-console": 0, //不禁用console
        "no-debugger": 2, //禁用debugger
        "no-var": 0, //对var警告
        "semi": 0, //不强制使用分号
        "no-irregular-whitespace": 0, //不规则的空白不允许
        "no-trailing-spaces": 1, //一行结束后面有空格就发出警告
        "eol-last": 0, //文件以单一的换行符结束
        "no-unused-vars": [2, {"vars": "all", "args": "after-used"}], //不能有声明后未被使用的变量或参数
        "no-underscore-dangle": 0, //标识符不能以_开头或结尾
        "no-alert": 2, //禁止使用alert confirm prompt
        "no-lone-blocks": 0, //禁止不必要的嵌套块
        "no-class-assign": 2, //禁止给类赋值
        "no-cond-assign": 2, //禁止在条件表达式中使用赋值语句
        "no-const-assign": 2, //禁止修改const声明的变量
        "no-delete-var": 2, //不能对var声明的变量使用delete操作符
        "no-dupe-keys": 2, //在创建对象字面量时不允许键重复
        "no-duplicate-case": 2, //switch中的case标签不能重复
        "no-dupe-args": 2, //函数参数不能重复
        "no-empty": 2, //块语句中的内容不能为空
        "no-func-assign": 2, //禁止重复的函数声明
        "no-invalid-this": 0, //禁止无效的this，只能用在构造器，类，对象字面量
        "no-redeclare": 2, //禁止重复声明变量
        "no-spaced-func": 2, //函数调用时 函数名与()之间不能有空格
        "no-this-before-super": 0, //在调用super()之前不能使用this或super
        "no-undef": 2, //不能有未定义的变量
        "no-use-before-define": 2, //未定义前不能使用
        "camelcase": 0, //强制驼峰法命名
        "jsx-quotes": [2, "prefer-double"], //强制在JSX属性（jsx-quotes）中一致使用双引号
        "react/display-name": 0, //防止在React组件定义中丢失displayName
        "react/forbid-prop-types": [2, {"forbid": ["any"]}], //禁止某些propTypes
        "react/jsx-boolean-value": 2, //在JSX中强制布尔属性符号
        "react/jsx-closing-bracket-location": 1, //在JSX中验证右括号位置
        "react/jsx-curly-spacing": [2, {"when": "never", "children": true}], //在JSX属性和表达式中加强或禁止大括号内的空格。
        "react/jsx-indent-props": [2, 4], //验证JSX中的props缩进
        "react/jsx-key": 2, //在数组或迭代器中验证JSX具有key属性
        "react/jsx-max-props-per-line": [1, {"maximum": 1}], // 限制JSX中单行上的props的最大数量
        "react/jsx-no-bind": 0, //JSX中不允许使用箭头函数和bind
        "react/jsx-no-duplicate-props": 2, //防止在JSX中重复的props
        "react/jsx-no-literals": 0, //防止使用未包装的JSX字符串
        "react/jsx-no-undef": 1, //在JSX中禁止未声明的变量
        "react/jsx-pascal-case": 0, //为用户定义的JSX组件强制使用PascalCase
        "react/jsx-sort-props": 2, //强化props按字母排序
        "react/jsx-uses-react": 1, //防止反应被错误地标记为未使用
        "react/jsx-uses-vars": 2, //防止在JSX中使用的变量被错误地标记为未使用
        "react/no-danger": 0, //防止使用危险的JSX属性
        "react/no-did-mount-set-state": 0, //防止在componentDidMount中使用setState
        "react/no-did-update-set-state": 1, //防止在componentDidUpdate中使用setState
        "react/no-direct-mutation-state": 2, //防止this.state的直接变异
        "react/no-multi-comp": 2, //防止每个文件有多个组件定义
        "react/no-set-state": 0, //防止使用setState
        "react/no-unknown-property": 2, //防止使用未知的DOM属性
        "react/prefer-es6-class": 2, //为React组件强制执行ES5或ES6类
        "react/prop-types": 0, //防止在React组件定义中丢失props验证
        "react/react-in-jsx-scope": 2, //使用JSX时防止丢失React
        "react/self-closing-comp": 0, //防止没有children的组件的额外结束标签
        "react/sort-comp": 2, //强制组件方法顺序
        "no-extra-boolean-cast": 0, //禁止不必要的bool转换
        "react/no-array-index-key": 0, //防止在数组中遍历中使用数组key做索引
        "react/no-deprecated": 1, //不使用弃用的方法
        "react/jsx-equals-spacing": 2, //在JSX属性中强制或禁止等号周围的空格
        "no-unreachable": 1, //不能有无法执行的代码
        "comma-dangle": 2, //对象字面量项尾不能有逗号
        "no-mixed-spaces-and-tabs": 0, //禁止混用tab和空格
        "prefer-arrow-callback": 0, //比较喜欢箭头回调
        "arrow-parens": 0, //箭头函数用小括号括起来
        "arrow-spacing": 0 //=>的前/后括号
    },
    // 自定义的 eslint-rule 插件时可以利用到的，这里面可以声明一些自定义的全局属性，然后在自己实现规则的插件中可以访问到。
    /*
    "settings": {
        "import/ignore": [
            "node_modules"
        ]
    }
    */
};
```

哦，老天，你还希望看到更多的react检查器，那就去 eslint-plugin-react 的github文档去慢慢翻译吧。

##### 某些文件关闭eslint检查

你不总是希望所有的文件都开启eslint检查，那么，给单独的js文件关闭eslint的方式，只需要在该文件的最顶部加上一段注释。

```
/*eslint-disable*/
function test() {
    return true
}
```

##### 给某一行js代码关闭eslint检查

关闭整个js文件的行为有点暴力，别担心，你还可以只给其中某段代码关闭eslint。

```
// eslint-disable-next-line
alert('foo')
```

##### eslint配置文件类型

eslint配置文件类型不只有js和json，其实包括下面这些：

- .eslintrc.js
- .eslintrc.yaml
- .eslintrc.yml
- .eslintrc.json
- .eslintrc
- package.json

#### React的ESlint配置（TS）

##### 项目需要安装的插件

```
"@typescript-eslint/eslint-plugin",
"@typescript-eslint/parser",
"eslint",
"eslint-plugin-react"
```

##### 配置详情

```JS
module.exports = {
  'env': {
    'browser': true,
    'commonjs': true,
    'es6': true,
  },
  'extends': ['eslint:recommended', 'plugin:react/recommended'],
  'globals': {
    '$': true,
    'process': true,
    '__dirname': true,
  },
  parser: '@typescript-eslint/parser',
  'parserOptions': {
    'ecmaFeatures': {
      'experimentalObjectRestSpread': true,
      'jsx': true,
    },
    'sourceType': 'module',
    'ecmaVersion': 7,
  },
  'plugins': ['react'],
  'rules': {},
}

```



#### React的tsconfig.json配置

```json
{
  "compilerOptions": {
    "target": "es5", // 指定 ECMAScript 版本
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ], // 要包含在编译中的依赖库文件列表
    "allowJs": true, // 允许编译 JavaScript 文件
    "skipLibCheck": true, // 跳过所有声明文件的类型检查
    "esModuleInterop": true, // 禁用命名空间引用 (import * as fs from "fs") 启用 CJS/AMD/UMD 风格引用 (import fs from "fs")
    "allowSyntheticDefaultImports": true, // 允许从没有默认导出的模块进行默认导入
    "strict": true, // 启用所有严格类型检查选项
    "forceConsistentCasingInFileNames": true, // 不允许对同一个文件使用不一致格式的引用
    "module": "esnext", // 指定模块代码生成
    "moduleResolution": "node", // 使用 Node.js 风格解析模块
    "resolveJsonModule": true, // 允许使用 .json 扩展名导入的模块
    "noEmit": true, // 不输出(意思是不编译代码，只执行类型检查)
    "jsx": "react", // 在.tsx文件中支持JSX
    "sourceMap": true, // 生成相应的.map文件
    "declaration": true, // 生成相应的.d.ts文件
    "noUnusedLocals": true, // 报告未使用的本地变量的错误
    "noUnusedParameters": true, // 报告未使用参数的错误
    "experimentalDecorators": true, // 启用对ES装饰器的实验性支持
    "incremental": true, // 通过从以前的编译中读取/写入信息到磁盘上的文件来启用增量编译
    "noFallthroughCasesInSwitch": true 
  },
  "include": [
    "src" // *** src文件夹应该进行类型检查 ***
  ],
  "exclude": ["node_modules", "build"] // *** 不进行类型检查的文件 ***
}
```

