### Redux

Redux 是针对 JavaScript 应用程序的可预测状态容器。

#### 安装

```sh
npm install react-redux
```

 创建 React Redux 应用程序

```sh
npx create-react-app my-app --template redux
```

#### 动机

当我们遇到项目中存在很多页面公用的状态时，按照我们正常操作肯定多个页面有可能都会更改这些状态，而且对于公用状态，如果受到随意改动很容易引起问题，且不容易排查。

所以redux解决的首要问题就是把redux中的数据变化变得可控可预测。

```js
// eg1: 
let a = 0; a='sas' a= [] a+=1; a-=1;
// eg2: 
let b =[];
//操作1： b.filter(filterFunction)
//操作2： b.push() //b.pop()
```

例如例子1，我们对他进行的多种赋值操作，但是其中的部分操作可能是不被允许的，比如不允许被赋值为数组。

或者对于例子2，对于b我们可能不希望做push操作，但是目前并没有这个限制来告诉开发者。

所以redux便想到了一个方法：即在变更的时候加一步确认操作，用户发起变更操作，redux检查是否存在此操作（actions)，如果存在则允许变更。

要做到上述步骤的前提是将数据修改的权限收回到redux中,想要限制修改权限，必须存在一个通用store来存储数据。需要reducer函数来变更数据。需要actions来限制数据变更的种类和通知redux进行哪些变更操作。

这样，开发者在变更数据时，总是需要先定义一个action（即变更数据操作的类型，或者称之为名字更好些）

其次：需要定义一个变更的方法，即reducer，连接action和state，确定如何更改数据。

如果开发者没有定义上面的任何一种，他便无法变更数据，如果他想变更数据，就必须先定义变更的事件和如何变更数据。这样便使得redux中的数据变得流程可控。

#### redux流程图

![redux流程图](http://jay_ohhh.gitee.io/imagehosting/react/reduxprocess.jpg)

流程说明：

1、组件通过store.dispatch方法发起actions后，Store可以接收到action

```js
store.dispatch(action);
```

2、Store需要负责接收Views（React Components）传来的Action，根据Action.type和Action.payload（其他属性统称为payload）对Store里的数据进行修改

3、Store的数据修改，本质上是通过Reducer来完成，将previousState和action传入到reducer。Store只提供get方法（即getState），不提供set方法。

4、Store接收到更新后的state，state 一旦有变化，Store 就会调用监听函数，通知views，通过store.subscribe，views更新

```js
// 设置监听函数
store.subscribe(listener);
```

listener可以通过store.getState()得到当前状态，重新渲染Views。

```javascript
function listerner() {
  let newState = store.getState();
  component.setState(newState);   
}
```

5、Store除了存储数据之外，还有着消息发布/订阅（pub/sub）的功能，也正是因为这个功能，它才能够同时连接着Actions和Views。

#### 三大原则

##### 单一数据源

整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中，即整个应用只能有一个 Store。。

##### State是只读的

唯一改变 state 的方法就是触发 action，action 是一个用于描述已发生事件的普通对象。

这样确保了视图和网络请求都不能直接修改 state，相反它们只能表达想要修改的意图。因为所有的修改都被集中化处理，且严格按照一个接一个的顺序执行，因此不用担心竞态条件（race condition）的出现。 Action 就是普通对象而已，因此它们可以被日志打印、序列化、储存、后期调试或测试时回放出来。

```js
store.dispatch({
  type: 'COMPLETE_TODO',
  index: 1
})

store.dispatch({
  type: 'SET_VISIBILITY_FILTER',
  filter: 'SHOW_COMPLETED'
})
```

##### 使用纯函数来执行修改

为了描述 action 如何改变 state tree ，你需要编写 reducers。

Reducer 只是一些纯函数（没有副作用），它接收先前的 state 和 action，并返回新的 state。

#### Action

**action** 是把数据从应用传到 store 的有效载荷，是一个用于描述已发生事件的普通对象，并没有描述应用如何更新 state。它是 store 数据的**唯一**来源。一般来说你会通过 `store.dispatch()` 将 action 传到 store。

添加新 todo 任务的 action 是这样的：

```js
const ADD_TODO = 'ADD_TODO'
```

```js
{
  type: ADD_TODO,
  text: 'Build my first Redux app'
}
```

action 本质上是 JavaScript 普通对象。我们约定，action 内必须使用一个字符串类型的 `type` 字段来表示将要执行的动作。多数情况下，`type` 会被定义成字符串常量。当应用规模越来越大时，建议使用单独的模块或文件来存放 action。

```js
import { ADD_TODO, REMOVE_TODO } from '../actionTypes'
```

`action`的`type`向使用者标识已发生动作的性质。`type`是字符串常量。

##### action 结构

除了 `type` 字段外，action 对象的结构完全由你自己决定。参照 [Flux 标准 Action](https://github.com/acdlite/flux-standard-action) 获取关于如何构造 action 的建议。

这时，我们还需要再添加一个 action index 来表示用户完成任务的动作序列号。因为数据是存放在数组中的，所以我们通过下标 `index` 来引用特定的任务。而实际项目中一般会在新建数据的时候生成唯一的 ID 作为数据的引用标识。

```js
{
  type: TOGGLE_TODO,
  index: 5
}
```

**我们应该尽量减少在 action 中传递的数据**。

##### action creator

action creator（action 创建函数）是生成 action 的方法。

在 Redux 中的 action 创建函数只是简单的返回一个 action：

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

这样做将使 action creator更容易被移植和测试。

Redux 中只需把 action creator的结果（action对象）传给 `dispatch()` 方法即可发起一次 dispatch 过程。

```js
dispatch(addTodo(text))
dispatch(completeTodo(index))
```

或者创建一个 **被绑定的 action creator** 来自动 dispatch：

```js
const boundAddTodo = text => dispatch(addTodo(text))
const boundCompleteTodo = index => dispatch(completeTodo(index))
```

然后直接调用它们：

```js
boundAddTodo(text);
boundCompleteTodo(index);
```

store 里能直接通过 `store.dispatch()` 调用 `dispatch()` 方法，但是多数情况下你会使用 react-redux 提供的 `connect()` 帮助器来调用。`bindActionCreators()` 可以自动把多个 action 创建函数 绑定到 `dispatch()` 方法上。

#### Reducer

描述 action 如何改变 state tree ，并发送到 store。

Reducer 只是一些纯函数（没有副作用），它接收先前的 state 和 action，并返回state（遇到未知action时返回**旧**的state，其余情况下返回**新**的state）。它仅仅用于计算下一个 state。它应该是完全可预测的：多次传入相同的输入必须产生相同的输出。它不应做有副作用的操作。

之所以将这样的函数称之为 reducer，是因为这种函数与被传入 [`Array.prototype.reduce(reducer, ?initialValue)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce) 里的回调函数属于相同的类型。保持 reducer 纯净非常重要。**永远不要**在 reducer 里做这些操作：

- 修改传入参数（值或地址。如果是地址传参，修改之后会直接改变原对象）；
- 执行有副作用的操作，如 API 请求和路由跳转；
- 调用非纯函数，如 `Date.now()` 或 `Math.random()`。

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```

1. **不要修改 `state`。** 使用 [`Object.assign()`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) 新建了一个副本。不能这样使用 `Object.assign(state, { visibilityFilter: action.filter })`，因为它会改变第一个参数的值。你**必须**把第一个参数设置为空对象{ }。你也可以开启对 ES7 提案[对象展开运算符](https://www.reduxjs.cn/recipes/using-object-spread-operator)的支持, 从而使用 `{ ...state, ...newState }` 达到相同的目的。
2. **在 `default` 情况下返回旧的 `state`。**遇到未知的 action 时，一定要返回**旧**的 `state`。

> ```js
> Object.assign(target, ...sources)
> ```
>
> target：目标对象
>
> source：源对象
>
> 返回值是target目标对象
>
> 如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。
>
> `Object.assign` 方法只会拷贝源对象自身的并且可枚举的属性到目标对象

##### 合成reducer

我们可以开发一个函数来做为主 reducer，它调用多个子 reducer 分别处理 state 中的一部分数据，然后再把这些数据合成一个大的单一对象。主 reducer 并不需要设置初始化时完整的 state。初始时，如果传入 `undefined`, 子 reducer 将负责返回它们的默认值。

```js
function todos(state = [], action) {
  // 省略处理逻辑...
  return nextState
}

function visibleTodoFilter(state = 'SHOW_ALL', action) {
  // 省略处理逻辑...
  return nextState
}

function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```

Redux 提供了 [`combineReducers()`](https://www.reduxjs.cn/api/combinereducers) 工具类来做上面 `todoApp` 做的事情，这样就能消灭一些样板代码了。有了它，可以这样重构 `todoApp`：

```js
import { combineReducers } from 'redux'

const todoApp = combineReducers({
  visibilityFilter,
  todos
})

export default todoApp
```

注意上面的写法和下面完全等价：

```js
export default function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  }
}
```

你也可以给它们设置不同的 key，或者调用不同的函数。下面两种合成 reducer 方法完全等价：

```js
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})
```

```js
function reducer(state = {}, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  }
}
```

当你触发 action 后，`combineReducers` 返回的 `todoApp` 会负责调用两个 reducer：

```js
let nextTodos = todos(state.todos, action)
let nextVisibleTodoFilter = visibleTodoFilter(state.visibleTodoFilter, action)
```

然后会把两个结果集合并成一个 state 树：

```js
return {
  todos: nextTodos,
  visibleTodoFilter: nextVisibleTodoFilter
}
```

总之，`combineReducers()`做的就是用一个函数根据 State 的 key 去执行相应的子 Reducer，并将返回结果合并成一个大的 State 对象。

`combineReducers` 接收一个对象，可以把所有顶级的 reducer 放到一个独立的文件中，通过 `export` 暴露出每个 reducer 函数，然后使用 `import * as reducers` ：

```js
import { combineReducers } from 'redux'
import * as reducers from './reducers'

const todoApp = combineReducers(reducers)
```

#### Store

Store 是用来维持应用所有的 [state 树](https://www.reduxjs.cn/glossary#state) 的一个对象。 整个应用只能有一个 Store。改变 store 内 state 的唯一途径是对它 dispatch 一个 [action](https://www.reduxjs.cn/glossary#action)。

当需要拆分数据处理逻辑时，你应该使用 [reducer 组合](https://www.reduxjs.cn/basics/reducers#splitting-reducers) 而不是创建多个 store。

Redux 提供`createStore`这个函数，用来生成 Store。 要创建它，只需要把根部的 [reducing 函数](https://www.reduxjs.cn/glossary#reducer) 传递给 [`createStore`](https://www.reduxjs.cn/api/createstore)。

Store 有以下职责：

- 维持应用的 state；
- 提供 [`getState()`](https://www.reduxjs.cn/api/store#getState) 方法获取 state；
- 提供 [`dispatch(action)`](https://www.reduxjs.cn/api/store#dispatch) 方法更新 state；
- 通过 [`subscribe(listener)`](https://www.reduxjs.cn/api/store#subscribe) 注册监听器;
- 通过 [`subscribe(listener)`](https://www.reduxjs.cn/api/store#subscribe) 返回的函数注销监听器

#### API

##### redux

###### createStore(reducer, [preloadedState], enhancer)

创建一个 Redux store 来以存放应用中所有的 state。
应用中应有且仅有一个 store。

```js
import { createStore } from 'redux'
```

**参数**

1. `reducer` *(Function)*: 接收两个参数，分别是当前的 state 树和要处理的 [action](https://www.reduxjs.cn/glossary#action)，返回新的 [state 树](https://www.reduxjs.cn/glossary#state)。
2. [`preloadedState`] *(any)*: 初始时的 state。 如果你使用 [`combineReducers`](https://www.reduxjs.cn/api/combinereducers) 创建 `reducer`，改参数必须是一个普通对象，且其keys与 [`combineReducers`](https://www.reduxjs.cn/api/combinereducers) 构造的state对象的keys保持一致。
3. `enhancer` *(Function)*: Store enhancer 是一个组合 store creator 的高阶函数，返回一个新的强化过的 store creator。与React高阶组件的概念一致。这与 middleware 相似，它也允许你通过复合函数改变 store 接口。

 **返回值**

(*`Store`*): 保存了应用所有 state 的对象。改变 state 的惟一方法是 [dispatch](https://www.reduxjs.cn/api/store#dispatch) action。你也可以 [subscribe 监听](https://www.reduxjs.cn/api/store#subscribe) state 的变化，然后更新 UI。

###### combineReducers(reducers)

`combineReducers` 函数的作用是，把一个由多个不同 reducer 函数作为 value 的 object，合并成一个最终的 reducer 函数，然后就可以对这个 reducer 调用 [`createStore`](https://www.reduxjs.cn/api/createstore) 方法。

合并后的 reducer 可以调用各个子 reducer，并把它们返回的结果合并成一个 state 对象。

**构造的 state 对象，会将传入的每个 reducer 返回的 state 按其传递给 `combineReducers()` 时对应的 key 进行命名**。

```js
import { combineReducers } from 'redux'

const rootReducer = combineReducers({potato: potatoReducer, tomato: tomatoReducer})
// rootReducer 将会构造出如下的 state 对象
{
  potato: {
    // ... 
  },
  tomato: {
    // ...
  }
}
```

一般来说，我们通常采取ES6的简写语法

`combineReducers({ counter, todos })`。这与 `combineReducers({ counter: counter, todos: todos })` 是等价的。

**参数**

`reducers` (*Object*): 一个对象，它的值（value）对应不同的 reducer 函数，这些 reducer 函数将被合并成一个。下面会介绍传入 reducer 函数需要满足的规则。

**返回值**

(*Function*)：返回一个可以调用用 `reducers` 对象里所有 reducer 的 reducer，并且构造一个与 `reducers` 对象结构相同的 state 对象。

###### applyMiddleware(...middleware)

将所有中间件组成一个数组。

中间件可以扩展`store.dispatch`。middleware 还拥有“可组合”这一关键特性。多个 middleware 可以被组合到一起使用，形成 middleware 链。其中，每个 middleware 都不需要关心链中它前后的 middleware 的任何信息。

Middleware 并不需要和 `createStore` 绑在一起使用。

 **参数**

`...middleware` (*arguments*)：遵循 Redux *middleware API* 的函数。每个 middleware 接受 [`Store`](https://www.reduxjs.cn/api/store) 的 [`dispatch`](https://www.reduxjs.cn/api/store#dispatch) 和 [`getState`](https://www.reduxjs.cn/api/store#getState) 函数作为参数，并返回一个函数，且该函数返回一个接收 action 的新函数。调用链中最后一个 middleware 会接受真实的 store 的 [`dispatch`](https://www.reduxjs.cn/api/store#dispatch) 方法作为 `next` 参数，并借此结束调用链。所以，middleware 的函数签名是 `({ getState, dispatch }) => next => action=>{}`。

> next 是调用下一个中间件，下一层中间件能否得到执行，取决于next这个函数有没有被调用。

中间件的次序有讲究:

```js
import { createStore, applyMiddleware } from 'redux'

const store = createStore(
  reducer,
  applyMiddleware(thunk, promise, logger)
);
```

有的中间件有次序要求，使用前要查一下文档。比如，`logger`就一定要放在最后，否则输出结果会不正确。

 **返回值**

(*Function*)： 返回一个应用了 middleware 后的 store enhancer。这个 store enhancer 的签名是 `createStore => createStore`（该函数签名的参数和返回值`createStore`是通过 `import {createStore} from 'redux'`引入），但是最简单的使用方法就是直接作为最后一个 `enhancer` 参数传递给 [`createStore()`](https://www.reduxjs.cn/api/createstore) 函数。

 **示例: 自定义 Logger Middleware**

```js
import { createStore, applyMiddleware } from 'redux'
import todos from './reducers'

function logger({ getState }) {
  return next => action => {
    console.log('will dispatch', action)

    // 调用 middleware 链中下一个 middleware 的 dispatch。
    const returnValue = next(action)

    console.log('state after dispatch', getState())

    // 一般会是 action 本身，除非
    // 后面的 middleware 修改了它。
    return returnValue
  }
}

const store = createStore(todos, ['Use Redux'], applyMiddleware(logger))

store.dispatch({
  type: 'ADD_TODO',
  text: 'Understand the middleware'
})
// (将打印如下信息:)
// will dispatch: { type: 'ADD_TODO', text: 'Understand the middleware' }
// state after dispatch: [ 'Use Redux', 'Understand the middleware' ]
```

**注意**

- 如果除了 `applyMiddleware`，你还用了其它 store enhancer，一定要把 `applyMiddleware` 放到组合链的前面，因为 middleware 可能会包含异步操作。比如，它应该在 [redux-devtools](https://github.com/reduxjs/redux-devtools) 前面，否则 DevTools 就看不到 Promise middleware 里 dispatch 的 action 了。
- 想要使用多个 store enhancer，可以使用 [`compose()`](https://www.reduxjs.cn/api/compose) 方法。

###### bindActionCreator(actionCreators, dispatch)

把一个 value 为不同 [action creator](https://www.reduxjs.cn/glossary#action-creator) 的对象，转成拥有同名 key 的对象。同时使用 [`dispatch`](https://www.reduxjs.cn/api/store#dispatch) 对每个 action creator 进行包装，以便可以直接调用它们。

一般情况下你可以直接在 [`Store`](https://www.reduxjs.cn/api/store) 实例上调用 [`dispatch`](https://www.reduxjs.cn/api/store#dispatch)。如果你在 React 中使用 Redux，[react-redux](https://github.com/gaearon/react-redux) 会提供 [`dispatch`](https://www.reduxjs.cn/api/store#dispatch) 函数让你直接调用它 。

唯一会使用到 `bindActionCreators` 的场景是当你需要把 action creator 往下传到一个组件上，却不想让这个组件觉察到 Redux 的存在，而且不希望把 `dispatch` 或 Redux store 传给它。

```js
import { bindActionCreators } from 'redux'
```

 **参数**

1. `actionCreators` (*Function* or *Object*)： 一个 action creator，或者一个 value 是 action creator 的对象。
2. `dispatch` (*Function*): 一个由 `Store` 实例提供的 `dispatch` 函数。

 **返回值**

(*Function* or *Object*)：一个与原对象类似的对象，只不过这个对象的key与原 action creator 同名， value 都是会直接 dispatch 原 action creator 返回的action的函数（调用：obj.key(参数)）。如果传入一个单独的函数作为 `actionCreators`，那么返回的结果也是一个单独的函数（调用：fun(参数)）。



###### compose(...functions)

从右到左来组合多个函数。
当需要把多个 `store enhancer` 依次执行的时候，需要用到它。

```js
import { compose } from 'redux'
```

**参数**

(*arguments*)：需要合成的多个函数。预计每个函数都接收一个参数。它的返回值将作为一个参数提供给它左边的函数，以此类推。例外是最右边的参数可以接受多个参数，因为它将为返回的组合函数提供签名。`compose(funcA, funcB, funcC)` 形象为 `compose(funcA(funcB(funcC())))`

 **返回值**

(*Function*)：从右到左把接收到的函数合成后的最终函数。

示例：

下面示例演示了如何使用 `compose` 增强 [store](https://www.reduxjs.cn/api/store)，这个 store 与 [`applyMiddleware`](https://www.reduxjs.cn/api/applymiddleware) 和 [redux-devtools](https://github.com/gaearon/redux-devtools) 一起使用。

```js
import { createStore, applyMiddleware, compose } from 'redux'
import thunk from 'redux-thunk'
import DevTools from './containers/DevTools'
import reducer from '../reducers'

// 一定要把 applyMiddleware 放到组合链的前面，因为 middleware 可能会包含异步操作。
const store = createStore(
  reducer,
  compose(
    applyMiddleware(thunk),
    DevTools.instrument()
  )
)
```

##### react-redux

###### connect()

详情查看 **UI组件和容器组件章节**

```js
import { connect } from 'react-redux'
```



##### Store

###### getState()

返回应用当前的 state 树。



###### dispatch(action)

这是触发 state 变化的惟一途径。state变化后，变化监听器会被执行。

在 Redux 里，只会在根 reducer 返回新 state 结束后才会调用事件监听器，因此，你可以在事件监听器里再做 dispatch。若你在 reducer 中 dispatch 要确保 reducer 没有副作用。如果 action 处理会产生副作用，正确的做法是使用异步 [action 创建函数](https://www.reduxjs.cn/glossary#action-creator)。

**参数**

`action` (Object)：描述应用变化的普通对象。

Action 是把数据传入 store 的惟一途径，所以任何数据，无论来自 UI 事件，网络回调或者是其它资源如 WebSockets，最终都应该以 action 的形式被 dispatch。按照约定，action 具有 `type` 字段来表示它的类型。type 也可被定义为常量或者是从其它模块引入。最好使用字符串，而不是 [Symbols](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Symbol) 作为 action，因为字符串是可以被序列化的。除了 `type` 字段外，action 对象的结构完全取决于你。

 **返回值**

(Object): 要 dispatch 的 action。



###### subscribe(listener)

添加一个变化监听器。Store 允许使用`store.subscribe`方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。你可以在监听函数里调用 [`getState()`](https://www.reduxjs.cn/api/store#getState) 来拿到当前 state。

 **参数**

`listener` (*Function*): 每当 dispatch action 的时候都会执行的回调。state 树中的一部分可能已经变化。你可以在回调函数里调用 [`getState()`](https://www.reduxjs.cn/api/store#getState) 来拿到当前 state。

 **返回值**

(*Function*): 一个可以解绑变化监听器的函数。

把 View 的更新函数（对于 React 项目，就是组件的`render`方法或`setState`方法）放入`listen`，就会实现 View 的自动渲染。

###### replaceReducer(nextReducer)

替换 store 当前用来计算 state 的 reducer。

这是一个高级 API。只有在你需要实现代码分隔，而且需要立即加载一些 reducer 的时候才可能会用到它。在实现 Redux 热加载机制的时候也可能会用到。

**参数**

`nextReducer` (*Function*) store 会使用的下一个 reducer。

没有返回值。

#### 数据流

**严格的单向数据流**是 Redux 架构的设计核心。

Redux 应用中数据的生命周期遵循下面 4 个步骤：

1. **调用** [`store.dispatch(action)`](https://www.reduxjs.cn/api/store#dispatch)。

   你可以在任何地方调用 [`store.dispatch(action)`](https://www.reduxjs.cn/api/store#dispatch)，包括组件中、XHR 回调中、甚至定时器中。

2. **Redux store 调用传入的 reducer 函数**

   注意 reducer 是纯函数。它仅仅用于计算下一个 state。它应该是完全可预测的：多次传入相同的输入必须产生相同的输出。它不应做有副作用的操作，如 API 调用或路由跳转。这些应该在 dispatch action 前发生。

3. **根 reducer 应该把多个子 reducer 输出合并成一个单一的 state 树**

   根 reducer 的结构完全由你决定。Redux 原生提供[`combineReducers()`](https://www.reduxjs.cn/api/combinereducers)辅助函数，来把根 reducer 拆分成多个函数，用于分别处理 state 树的一个分支。

4. **Redux store 保存了根 reducer 返回的完整 state 树**

   这个新的树就是应用的下一个 state！所有订阅 [`store.subscribe(listener)`](https://www.reduxjs.cn/api/store#subscribe) 的监听器都将被调用；监听器里可以调用 [`store.getState()`](https://www.reduxjs.cn/api/store#getState) 获得当前 state。

#### UI组件和容器组件

Redux 的 React 绑定库（React-Redux）是基于 [容器组件和展示组件相分离](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) 的开发思想。

React-Redux 将所有组件分成两大类：UI 组件（presentational component）和容器组件（container component）。

##### UI组件

UI 组件有以下几个特征：

- 只负责 UI 的呈现，不带有任何业务逻辑
- 没有状态（即不使用`this.state`这个变量）
- 所有数据都由参数（`this.props`）提供
- 不使用任何 Redux 的 API

下面就是一个 UI 组件的例子。

> ```JSX
> const Title = value => <h1>{value}</h1>;
> ```

因为不含有状态，UI 组件又称为"纯组件"，即它纯函数一样，纯粹由参数决定它的值。

##### 容器组件

容器组件有以下几个特征：

- 负责管理数据和业务逻辑，不负责 UI 的呈现
- 带有内部状态
- 使用 Redux 的 API

总之，只要记住一句话就可以了：UI 组件负责 UI 的呈现，容器组件负责管理数据和逻辑。

**如果一个组件既有 UI 又有业务逻辑，那怎么办？**

回答是，将它拆分成下面的结构：外面是一个容器组件，里面包了一个UI 组件。前者负责与外部的通信，将数据传给后者，由后者渲染出视图。

React-Redux 规定，所有的 UI 组件都由用户提供，容器组件则是由 React-Redux 自动生成。也就是说，用户负责视觉层，状态管理则是全部交给它。

##### connect()

React-Redux 提供`connect`方法，用于从 UI 组件生成容器组件。`connect`的意思，就是将这两种组件连起来。

```javascript
import { connect } from 'react-redux'
const VisibleTodoList = connect()(TodoList);
```

上面代码中，`TodoList`是 UI 组件，`VisibleTodoList`就是由 React-Redux 通过`connect`方法自动生成的容器组件。

但是，因为没有定义业务逻辑，上面这个容器组件毫无意义，只是 UI 组件的一个单纯的包装层。为了定义业务逻辑，需要给出下面两方面的信息。

> （1）输入逻辑：外部的数据（即`state`对象）如何转换为 UI 组件的参数
>
> （2）输出逻辑：用户发出的动作如何变为 Action 对象，从 UI 组件传出去。

因此，`connect`方法的完整 API 如下：

```javascript
import { connect } from 'react-redux'

const VisibleTodoList = connect(
  mapStateToProps,
  mapDispatchToProps
)(TodoList)
```

上面代码中，`connect`方法接受两个参数：`mapStateToProps`和`mapDispatchToProps`。它们定义了 UI 组件的业务逻辑。前者负责输入逻辑，即将`state`映射到 UI 组件的参数（`props`），后者负责输出逻辑，即将用户对 UI 组件的操作映射成 Action。

当`connect`第一个参数为`null`时，即`connect(null,mapDispatchToProps)`，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新。

当`connect`第二个参数为`null`时，即`connect(mapStateToProps,null)`，可以直接在UI组件的`this.props.dispatch`拿到`dispatch`方法。

##### mapStateToProps()

`mapStateToProps`是一个函数。它的作用就是像它的名字那样，建立一个从（外部的）`state`对象到（UI 组件的）`props`对象的映射关系，即将store中的数据作为props绑定到组件上。接受state的UI/容器组件需要实现该函数。

参数：

- state：redux中的state（执行时会自动传入）

- ownProps：容器组件的props对象（执行时会自动传入）

返回值：

一个（state）对象，对象内的键值对会传入到UI组件，在UI组件中通过 this.props.xxx或解构赋值获取

```javascript
const mapStateToProps = (state) => {
  return {
    todos: getVisibleTodos(state.todos, state.visibilityFilter)
  }
}
```

下面就是`getVisibleTodos`的一个例子，用来算出`todos`：

```javascript
const getVisibleTodos = (todos, filter) => {
  switch (filter) {
    case 'SHOW_ALL':
      return todos
    case 'SHOW_COMPLETED':
      return todos.filter(t => t.completed)
    case 'SHOW_ACTIVE':
      return todos.filter(t => !t.completed)
    default:
      throw new Error('Unknown filter: ' + filter)
  }
}
```

`mapStateToProps`会订阅 Store，每当`state`更新的时候，就会自动执行，重新计算 UI 组件的参数，从而触发 UI 组件的重新渲染。

`mapStateToProps`的第一个参数总是`state`对象，还可以使用第二个参数，代表容器组件的`props`对象。

```javascript
// 	容器组件的代码
//  <FilterLink filter="SHOW_ALL">
//    All
//  </FilterLink>

const mapStateToProps = (state, ownProps) => {
  return {
    active: ownProps.filter === state.visibilityFilter
  }
}
```

使用`ownProps`作为参数后，如果容器组件的参数发生变化，也会引发 UI 组件重新渲染。

`connect`方法可以省略`mapStateToProps`这个参数，那样的话，UI 组件就不会订阅Store，就是说 Store 的更新不会引起 UI 组件的更新。

##### mapDispatchToProps()

`mapDispatchToProps`是`connect`函数的第二个参数，用来建立 UI 组件的操作到`store.dispatch`方法的映射。也就是说，它定义了哪些用户的操作应该当作 Action，传给 Store。它可以是一个函数，也可以是一个对象。发送action的UI/容器组件需要实现该函数。

如果`mapDispatchToProps`是一个函数，会得到两个参数：

- `dispatch`：store.dispatch（执行时会自动传入）
- `ownProps`：容器组件的`props`对象（执行时会自动传入）

返回值：

一个对象，对象内的键值对会传入到UI组件，在UI组件中通过this.props.xxx或解构赋值获取，通过this.props.xxx(参数)发送action

```js
// 返回对象中的键名可以随意写，只要符合规范，在UI组件中通过 this.props.xxx(参数)发送action
const mapDispatchToProps = (
  dispatch,
  ownProps
) => ({
  onClick: () => {
      dispatch(action对象);
    }
})
```

`mapDispatchToProps`返回对象的每个键值对都是一个映射，定义了 UI 组件的参数怎样发出 Action。

如果`mapDispatchToProps`是一个对象，它的每个键名也是对应 UI 组件中出现的同名参数或方法，键值应该是一个action creator，返回的 Action 会由 Redux 自动发出。举例来说，上面的`mapDispatchToProps`写成对象就是下面这样：

```javascript
const mapDispatchToProps = {
  onClick: () => action对象
}
```

示例：

```js
// TodoListContainer.js
// todos和toggleTodo都会传入UI组件，UI组件中可以通过this.props.xxx或解构赋值获取
const mapStateToProps = state => ({
  todos: [
    // something
  ]
})

const mapDispatchToProps = dispatch => ({
  toggleTodo: id => dispatch(toggleTodo(id))
})

// TodoLsit.js
const TodoList = ({ todos, toggleTodo }) => (
  <ul>
    {todos.map(todo =>
      <Todo
        key={todo.id}
        onClick={() => toggleTodo(todo.id)}
      />
    )}
  </ul>
)
```



##### Provider组件

`connect`方法生成容器组件以后，需要让容器组件拿到`state`对象，才能生成 UI 组件的参数。

一种解决方法是将`state`对象作为参数，传入容器组件。但是，这样做比较麻烦，尤其是容器组件可能在很深的层级，一级级将`state`传下去就很麻烦。

React-Redux 提供`Provider`组件，可以让容器组件拿到`state`。

```jsx
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import todoApp from './reducers'
import App from './components/App'

let store = createStore(todoApp);

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

上面代码中，`Provider`在根组件外面包了一层，这样一来，`App`的所有子组件通过 `props.store` 就可以拿到`store`了。



##### 总结

|                |          展示组件          |              容器组件              |
| :------------: | :------------------------: | :--------------------------------: |
|      作用      | 描述如何展现（骨架、样式） | 描述如何运行（数据获取、状态更新） |
| 直接使用 Redux |             否             |                 是                 |
|    数据来源    |           props            |          监听 Redux state          |
|    数据修改    |   从 props 调用回调函数    |       向 Redux 派发 actions        |
|    生成方式    |            手动            | 通常由 React-Redux  connect()生成  |

大部分的组件都应该是展示型的，但一般需要少数的几个容器组件把它们和 Redux store 连接起来。

容器组件不必位于组件树的最顶层。如果一个容器组件变得太复杂（例如，它有大量的嵌套组件以及传递数不尽的回调函数），那么在组件树中引入另一个容器，就像[FAQ](https://www.reduxjs.cn/faq/react-redux#react-multiple-components)中提到的那样。

技术上讲你可以直接使用 `store.subscribe()` 来编写容器组件。但不建议这么做的原因是无法使用 React-Redux 带来的性能优化。也因此，不要手写容器组件，而使用 React Redux 的 `connect()` 方法来生成。

#### 搭配react

Redux 和 React 之间没有关系。Redux 支持 React、Angular、Ember、jQuery 甚至纯 JavaScript。

Redux和React-Redux是两个概念。

#####  传入 Store

 React Redux 组件 <Provider> 来让所有容器组件都可以访问 store，而不必显式地传递它。只需要在渲染根组件时使用即可。

```jsx
// index.js
import React from 'react'
import { render } from 'react-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import todoApp from './reducers'
import App from './components/App'

let store = createStore(todoApp)

render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```

#### 搭配React-Router

使用`React-Router`的项目，与其他项目没有不同之处，也是使用`Provider`在`Router`外面包一层，毕竟`Provider`的唯一功能就是传入`store`对象。

```jsx
import { render } from 'react-dom'
import { BrowserRouter as Router, Route } from 'react-router-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import rootReducer from './reducers'

let store = createStore(rootReducer)

const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/" component={App} />
    </Router>
  </Provider>
);
Root.propTypes = {
  store: PropTypes.object.isRequired
}

render(<Root store={store} />, document.getElementById('root'))
```

#### 异步action

异步 action 是一个发给 dispatching 函数的值，但是这个值还不能被 reducer 消费。在发往真实的`store.dispatch`之前，middleware 会把异步 action 转换成一个或一组 action。异步 action 可以有多种数据类型，这取决于你所使用的 middleware。它

同步操作只要发出一种 Action 即可，异步操作的差别是它要发出三种 Action。

- 操作发起时的 Action
- 操作成功（结束）时的 Action
- 操作失败时的 Action

三种 Action 可以有但不仅限于以下两种不同的写法，采用哪种写法取决于团队的约定。

```javascript
// 写法一：名称相同，参数不同
{ type: 'FETCH_POSTS' }
{ type: 'FETCH_POSTS', status: 'error', error: 'Oops' }
{ type: 'FETCH_POSTS', status: 'success', response: { ... } }

// 写法二：名称不同
{ type: 'FETCH_POSTS_REQUEST' }
{ type: 'FETCH_POSTS_FAILURE', error: 'Oops' }
{ type: 'FETCH_POSTS_SUCCESS', response: { ... } }
```

除了 Action 的 type不同，异步操作的 State 也要进行改造，反映不同的操作状态。下面是 State 的一个例子。

```javascript
let state = {
  // ... 
  isFetching: true,
  didInvalidate: true,
  lastUpdated: 'xxxxxxx'
};
```

上面代码中，State 的属性`isFetching`表示是否在抓取数据。`didInvalidate`表示数据是否过时，`lastUpdated`表示上一次更新时间。

现在，整个异步操作的思路就，至少发出两个Action：

- 操作开始时，送出一个 Action，触发 State 更新为"正在操作"状态，View 重新渲染
- 操作结束后，再送出一个 Action，触发 State 更新为"操作结束"状态，View 再一次重新渲染

下面来看一个例子：

```javascript
const fetchPosts = postTitle => (dispatch, getState) => {
  dispatch(requestPosts(postTitle));
  return fetch(`/some/API/${postTitle}.json`)
    .then(response => response.json())
    .then(json => dispatch(receivePosts(postTitle, json)));
  };
};
```

上面代码中，`fetchPosts`是一个Action Creator，返回一个函数。这个函数执行后，先发出一个Action（`requestPosts(postTitle)`），然后进行异步操作`fetch`。拿到结果后，先将结果转成 JSON 格式，然后再发出一个 Action（ `receivePosts(postTitle, json)`）。

上面代码中，有几个地方需要注意：

（1）`fetchPosts`返回了一个函数，而普通的 Action Creator 需要返回一个对象。

（2）返回的函数的参数是`dispatch`和`getState`这两个 Redux 方法

（3）在返回的函数之中，先发出一个 Action（`requestPosts(postTitle)`），表示操作开始。

（4）异步操作结束之后，再发出一个 Action（`receivePosts(postTitle, json)`），表示操作结束。

这样的处理，就解决了自动发送第二个 Action 的问题。但是，又带来了一个新的问题，Action 是由`store.dispatch`方法发送的。而`store.dispatch`方法正常情况下，参数只能是对象，不能是函数。

这时，就要使用中间件`redux-thunk`（可以是`dispatch`接受函数作为参数）。

因此，异步操作的第一种解决方案就是，写出一个返回函数的 Action Creator，然后使用`redux-thunk`中间件改造`store.dispatch`。

#### redux-thunk

`redux-thunk`主要的功能就是可以让我们`dispatch`一个函数，而不只是普通的 action 对象。我们可以在这个函数中进行副作用的操作。

因此在使用`redux-thunk`的情况下，编写Action creator可以返回函数，该函数接受 dispatch 、getState 、extraArgument 作为参数。

```js
import thunkMiddleware from 'redux-thunk'
const store = createStore(reducer, applyMiddleware(thunkMiddleware))
```

注册后可以这样使用：

```js
// 用于发起登录请求，并处理请求结果
// 接受参数用户名，并返回一个函数(参数为dispatch)
const login = (userName) => (dispatch) => {
  dispatch({ type: 'loginStart' })
  request.post('/api/login', { data: userName }, () => {
    dispatch({ type: 'loginSuccess', payload: userName })
  })
}
store.dispatch(login('Lucy'))
```

redux-thunk源码

```js
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) => next => action => {
    if (typeof action === 'function') {
      return action(dispatch, getState, extraArgument);
    }

    return next(action);
  };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

#### redux-promise

既然 Action Creator 可以返回函数，当然也可以返回其他值。另一种异步操作的解决方案，就是让 Action Creator 返回一个 Promise 对象。

`redux-promise`主要的功能就是可以让我们`dispatch`一个 `Promise`对象。

```js
import promiseMiddleware from 'redux-promise';
```

这时，Action Creator 有两种写法。写法一，返回值是一个 Promise 对象：

```javascript
const fetchPosts = 
  (dispatch, postTitle) => new Promise(function (resolve, reject) {
    // ...
		});
});
```

写法二，Action 对象的`payload`属性是一个 Promise 对象。

如果 Action 本身是一个 Promise，它 resolve 以后的值应该是一个 Action 对象，会被`dispatch`方法送出（`action.then(dispatch)`），但 reject 以后不会有任何动作；如果 Action 对象的`payload`属性是一个 Promise 对象，那么无论 resolve 和 reject，`dispatch`方法都会发出 Action。resolve的值不会传到下一个中间件。

redux-promise源码

```javascript
import isPromise from 'is-promise';
import { isFSA } from 'flux-standard-action';

export default function promiseMiddleware({ dispatch }) {
    return next => action => {
        /**
         * 判断是不是标准的FSA
         */
        if (!isFSA(action)) {
            /**
             * 不是标准的FSA
             * 判断是不是promise,
             * 如果是则执行，只会处理resolve的值，
             * 反之交给下一个中间件
             */
            return isPromise(action) ? action.then(dispatch) : next(action);
        }

        // 是标准的FSA, 判断是不是一个promise
        return isPromise(action.payload)
            /**
             * 1.promise的时候，执行then,同时捕获异常，
             * 在处理这两种情况以后，会分别添加另外一个约束error
             *  这我们需要在reducer里面还需要判断error的值，
             * 做不同的处理
             * 
             * 2. 如果不是promise则交给下一个中间件
             * 
             * */
            ? action.payload
                .then(result => dispatch({ ...action, payload: result }))
                .catch(error => {
                    dispatch({ ...action, payload: error, error: true });
                    return Promise.reject(error);
                })
            : next(action);
    };
}
```

> FSA - [Flux Standard Action](https://github.com/acdlite/flux-standard-action)

#### Middleware

middleware 最优秀的特性就是可以被链式组合。

middleware 数据流向遵循洋葱模型，先入后出，是按照类似栈的方式组织和执行的。

洋葱模型：

![](http://jay_ohhh.gitee.io/imagehosting/react/中间件洋葱模型.png)

redux middleware的洋葱模型：

![](http://jay_ohhh.gitee.io/imagehosting/react/redux_middleware洋葱模型.png)

在Express和koa这类框架中，middleware 是指可以被嵌入在框架接收请求到产生响应过程之中的代码。例如，Express 或者 Koa 的 middleware 可以完成添加 CORS headers、记录日志、内容压缩等工作。

在Redux中，**它提供的是位于 action 被发起之后，到达 reducer 之前的扩展点（一旦action被发起后，middleware会对dispatch进行扩展，调用链最后一个middleware才会接受真实的store.dispactch）。** 你可以利用 Redux middleware 来进行日志记录、创建崩溃报告、调用异步接口或者路由等等。

每个 middleware 接受 [`Store`](https://www.reduxjs.cn/api/store) 的 [`dispatch`](https://www.reduxjs.cn/api/store#dispatch) 和 [`getState`](https://www.reduxjs.cn/api/store#getState) 函数作为参数，并返回一个函数，且该函数返回一个接收 action 的新函数。调用链中最后一个 middleware 会接受真实的 store 的 [`dispatch`](https://www.reduxjs.cn/api/store#dispatch) 方法作为 `next` 参数，并借此结束调用链。所以，middleware 的函数签名是 `({ getState, dispatch }) => next => action=>{ 函数内需要调用next(action) }`。

> next 是调用下一个中间件，下一层中间件能否得到执行，取决于next这个函数有没有被调用。

未使用middleware的redux数据流：

> 以下两图，dispatch和reducer之间、reducer和view之间应该加上store。

![](http://jay_ohhh.gitee.io/imagehosting/react/redux数据流.webp)

使用middleware的redux数据流：

![](http://jay_ohhh.gitee.io/imagehosting/react/redux_middleware数据流.webp)

有的中间件有次序要求，使用前要查一下文档。比如，`logger`就一定要放在最后，否则输出结果会不正确。

#### redux-saga

https://www.jianshu.com/p/7cac18e8d870

##### 含义

redux-saga 是一个用于管理 Redux 应用异步操作的中间件（又称异步 action）。 redux-saga 通过创建 Sagas 将所有的异步操作逻辑收集在一个地方集中处理，可以用来代替 redux-thunk 中间件。

这意味着应用的逻辑会存在两个地方：

- Reducers 负责处理 action 的 state 更新
- Sagas 负责协调那些复杂或异步的操作

Sagas是通过Generator函数来创建的，如果有不熟悉 Generator函数使用的，[请查看阮老师对Generator的介绍](http://www.ruanyifeng.com/blog/2015/04/generator.html)。

Sagas 不同于thunks，thunks 是在action被创建时调用，而 Sagas只会在应用启动时调用（但初始启动的 Sagas 可能会动态调用其他 Sagas），Sagas 可以被看作是在后台运行的进程，Sagas 监听发起的action，然后决定基于这个 action来做什么：是发起一个异步调用（比如一个 fetch 请求），还是发起其他的action到Store，甚至是调用其他的 Sagas

在 redux-saga 的世界里，所有的任务都通用 yield Effects 来完成（Effect 可以看作是 redux-saga 的任务单元）。Effects 都是简单的 Javascript 对象，包含了要被 Saga middleware 执行的信息（打个比方，你可以看到 Redux action其实是一个个包含执行信息的对象）， redux-saga 为各项任务提供了各种Effect创建器，比如调用一个异步函数，发起一个action到Store，启动一个后台任务或者等待一个满足某些条件的未来的 action

##### API

###### Effect Creators

redux-saga框架提供了很多创建effect的函数，下面我们就来简单的介绍下开发中最常用的几种：

```js
import {
	// some Effect Creators
} from 'redux-saga/effects'
```

- take(pattern)
- put(action)
- call(fn, ...args)
- fork(fn, ...args)
- select(selector, ...args)

**take(pattern)**

take函数可以理解为监听未来的action，它创建了一个命令对象，告诉middleware等待一个特定的action， Generator会暂停，直到一个与pattern匹配的action被发起，才会继续执行下面的语句，也就是说，**take是一个阻塞的 effect**

用法：

```jsx
function* watchFetchData() {
   while(true) {
     // 监听一个type为 'FETCH_REQUESTED' 的action的执行，直到等到这个Action被触发，才会接着执行下面的 yield fork(fetchData)  语句
     yield take('FETCH_REQUESTED');
     yield fork(fetchData);
   }
}
```

**put(action)**

put函数是用来发送action的 effect，你可以简单的把它理解成为redux框架中的dispatch函数，当put一个action后，reducer中就会计算新的state并返回， **put 是一个阻塞的 effect**

用法：

```jsx
export function* toggleItemFlow() {
    let list = []
    // 发送一个type为 'UPDATE_DATA' 的Action，用来更新数据，参数为 `data：list`
    yield put({
      type: actionTypes.UPDATE_DATA,
      data: list
    })
}
```

**call(fn, ...args)**

call函数你可以把它简单的理解为就是可以调用其他函数的函数，它命令 middleware 来调用fn 函数， args为函数的参数，**注意：**  fn 函数可以是一个 Generator 函数，也可以是一个返回 Promise 的普通函数，**call 函数是一个阻塞的 effect**

用法：

```jsx
export const delay = ms => new Promise(resolve => setTimeout(resolve, ms))

export function* removeItem() {
  try {
    // 这里call 函数就调用了 delay 函数，delay 函数为一个返回promise 的函数
    return yield call(delay, 500)
  } catch (err) {
    yield put({type: actionTypes.ERROR})
  }
}
```

**fork(fn, ...args)**

fork 函数和 call 函数很像，都是用来调用其他函数的，但是fork函数是非阻塞函数，也就是说，程序执行完 `yield fork(fn， args)` 这一行代码后，会立即接着执行下一行代码语句，而不会等待fn函数返回结果后，在执行下面的语句

用法：

```jsx
import { fork } from 'redux-saga/effects'

export default function* rootSaga() {
  // 下面的四个 Generator 函数会一次执行，不会阻塞执行
  yield fork(addItemFlow)
  yield fork(removeItemFlow)
  yield fork(toggleItemFlow)
  yield fork(modifyItem)
}
```

**select(selector, ...args)**

select 函数是用来指示 middleware调用提供的选择器获取Store上的state数据，你也可以简单的把它理解为redux框架中获取store上的 state数据一样的功能 ：`store.getState()`

用法：

```jsx
export function* toggleItemFlow() {
     // 通过 select effect 来获取 全局 state上的 `getTodoList` 中的 list
     let tempList = yield select(state => state.getTodoList.list)
}
```

###### Saga 辅助函数

redux-saga提供了一些辅助函数，用来在一些特定的action 被发起到Store时派生任务，下面我先来讲解两个辅助函数：`takeEvery` 和 `takeLatest`。 `takeEvery` 和 `takeLatest` effect 是用来管理同一个 Effects 的并发。

```js
import {
	// some Saga 辅助函数
} from 'redux-saga'
```

- takeEvery

`takeEvery` 可以让多个 `saga` 任务并行被 fork 执行。

例如：每次点击 Fetch 按钮时，我们发起一个 FETCH_REQUESTED 的 action。 我们想通过启动一个任务从服务器获取一些数据，来处理这个action

首先我们创建一个将执行异步 action 的任务：

```tsx
import { call, put } from 'redux-saga/effects'

export function* fetchData(action) {
   try {
      const data = yield call(Api.fetchUser, action.payload.url);
      yield put({type: "FETCH_SUCCEEDED", data});
   } catch (error) {
      yield put({type: "FETCH_FAILED", error});
   }
}
```

然后在每次 FETCH_REQUESTED action 被发起时启动上面的任务

```jsx
import { takeEvery } from 'redux-saga'

function* watchFetchData() {
  yield* takeEvery("FETCH_REQUESTED", fetchData)
}
```

**注意**：上面的 takeEvery 函数可以使用下面的写法替换

```jsx
function* watchFetchData() {
   while(true){
     yield take('FETCH_REQUESTED');
     yield fork(fetchData);
   }
}
```

- takeLatest

`takeLatest` 不允许多个 `saga` 任务并行地执行。一旦接收到新的发起的 action，它就会取消前面所有 fork 过的任务（如果这些任务还在执行的话）。

在处理 AJAX 请求的时候，如果我们只希望获取最后那个请求的响应，`takeLatest` 就会非常有用。

在上面的例子中，takeEvery 允许多个 fetchData 实例同时启动，在某个特定时刻，我们可以启动一个新的 fetchData 任务， 尽管之前还有一个或多个 fetchData 尚未结束。

如果我们只想得到最新那个请求的响应（例如，始终显示最新版本的数据），我们可以使用 takeLatest 辅助函数

```jsx
import { takeLatest } from 'redux-saga'

function* watchFetchData() {
  yield* takeLatest('FETCH_REQUESTED', fetchData)
}
```

和takeEvery不同，在任何时刻 takeLatest 只允许执行一个 fetchData 任务，并且这个任务是最后被启动的那个，如果之前已经有一个任务在执行，那之前的这个任务会自动被取消

###### Middleware API

**createSagaMiddleware()**

createSagaMiddleware 函数是用来创建一个 Redux 中间件，将 Sagas 与 Redux Store 链接起来

sagas 中的每个函数都必须返回一个 Generator 对象，middleware 会迭代这个 Generator 并执行所有 yield 后的 Effect（Effect 可以看作是 redux-saga 的任务单元）

用法：

```jsx
import {createStore, applyMiddleware} from 'redux'
import createSagaMiddleware from 'redux-saga'
import reducers from './reducers'
import rootSaga from './rootSaga'

// 创建一个saga中间件
const sagaMiddleware = createSagaMiddleware()

// 创建store
const store = createStore(
  reducers,
  // 将sagaMiddleware 中间件传入到 applyMiddleware 函数中
  applyMiddleware(sagaMiddleware)
)

// 动态执行saga，注意：run函数只能在store创建好之后调用
sagaMiddleware.run(rootSaga)

export default store
```

**middleware.run(sagas, ...args)**

动态执行sagas，用于applyMiddleware阶段之后执行sagas

- sagas: Function: 一个 Generator 函数
- args: Array: 提供给 saga 的参数 (除了 Store 的 getState 方法)

注意：动态执行saga语句 `middleware.run(sagas)` 必须要在store创建好之后才能执行，在 store 之前执行，程序会报错。

###### Effect 组合器

**race(effects)**

多个 Effect 间运行 竞赛（Race）,与 [Promise.race](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise/race) 的行为类似。谁是第一就用谁。

`effects: Object` - 一个 {label: effect, ...} 形式的字典对象

**例子**

下面的例子在两个 effect 间运行了一次竞赛：

1. 对函数 `fetchUsers` 的一次 call，该函数将返回一个 Promise
2. 一个 `CANCEL_FETCH` action，该 action 可能最终在 Store 上被发起

> 根据情况进行多个effect 比较，这里只是列出两个effect例子

```javascript
import { take, call, race } from `redux-saga/effects`
import fetchUsers from './path/to/fetchUsers'

function* fetchUsersSaga {
  const { response, cancel } = yield race({
    response: call(fetchUsers),
    cancel: take(CANCEL_FETCH)
  })
}
```

如果 `call(fetchUsers)` 先 resolve（或 reject），那么 `race` 的结果将是一个对象，该对象包含一个单键对象 `{response: result}`，其中 `result` 是 `fetchUsers` resolve 的结果。`cancel`则是 `undefined`。

如果在 `fetchUsers` 完成之前，Store 上先发起了一个 `CANCEL_FETCH` 类型的 action，那么结果将是一个单键对象 `{cancel: action}`，其中 `action` 是被发起的 action。`response`则是 `undefined`。

**注意事项**

当 resolve `race` 的时候，middleware 会自动地取消所有输掉的 Effect。

**race([...effects]) (with Array)**

与 [`race(effects)`](https://redux-saga-in-chinese.js.org/docs/api/#raceeffects) 相同，但传入的是 effect 的数组。

**例子**

下面的例子在两个 effect 间运行了一次竞赛：

1. 对函数 `fetchUsers` 的一次 call，该函数将返回一个 Promise
2. 一个 `CANCEL_FETCH` action，该 action 可能最终在 Store 上被发起

```javascript
import { take, call, race } from `redux-saga/effects`
import fetchUsers from './path/to/fetchUsers'

function* fetchUsersSaga {
  const [response, cancel] = yield race([
    call(fetchUsers),
    take(CANCEL_FETCH)
  ])
}
```

如果 `call(fetchUsers)` 先 resolve（或 reject），那么 `response` 将是 `fetchUsers` 的结果，并且 `cancel` 将是 `undefined`。

如果在 `fetchUsers` 完成之前，Store 上先发起了一个 `CANCEL_FETCH` 类型的 action，那么 `response` 将是 `undefined`，并且 `cancel` 将是被发起的 action。

**all([...effects]) - parallel effects**

并行多个 Effect，并等待它们全部完成。这是与标准的 [`Promise#all`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) 相当对应的 API。

**例子**

以下的例子并行地运行了两个阻塞型调用：

```javascript
import { fetchCustomers, fetchProducts } from './path/to/api'
import { all, call } from `redux-saga/effects`

function* mySaga() {
  const [customers, products] = yield all([
    call(fetchCustomers),
    call(fetchProducts)
  ])
}
```

**all(effects)**

与 [`all([...effects\])`](https://redux-saga-in-chinese.js.org/docs/api/#alleffects-parallel-effects) 相同，但就像 [`race(effects)`](https://redux-saga-in-chinese.js.org/docs/api/#alleffects) 那样，传入的是一个带有 label 的 effect 的字典对象。

- `effects: Object` - 一个 {label: effect, ...} 形式的字典对象

**例子**

以下的例子并行地运行了两个阻塞型调用：

```javascript
import { fetchCustomers, fetchProducts } from './path/to/api'
import { all, call } from `redux-saga/effects`

function* mySaga() {
  const { customers, products } = yield all({
    customers: call(fetchCustomers),
    products: call(fetchProducts)
  })
}
```

**注意事项**

当并发运行 Effect 时，middleware 将暂停 Generator，直到以下任一情况发生：

- 所有 Effect 都成功完成：返回一个包含所有 Effect 结果的数组，并恢复 Generator。
- 在所有 Effect 完成之前，有一个 Effect 被 reject：在 Generator 中抛出 reject 错误。

##### Counter Demo

**index.js**

```tsx
import React from 'react';
import ReactDOM from 'react-dom';
import {createStore, applyMiddleware} from 'redux'
import createSagaMiddleware from 'redux-saga'

import rootSaga from './sagas'
import Counter from './Counter'
import rootReducer from './reducers'

const sagaMiddleware = createSagaMiddleware()
let middlewares = []
middlewares.push(sagaMiddleware)

const createStoreWithMiddleware = applyMiddleware(...middlewares)(createStore)
const store = createStoreWithMiddleware(rootReducer)

sagaMiddleware.run(rootSaga)

const action = type => store.dispatch({ type })

function render() {
  ReactDOM.render(
    <Counter
      value={store.getState()}
      onIncrement={() => action('INCREMENT')}
      onDecrement={() => action('DECREMENT')}
      onIncrementAsync={() => action('INCREMENT_ASYNC')} />,
    document.getElementById('root')
  )
}

render()

store.subscribe(render)
```

**sagas.js**

```jsx
import { put, call, take,fork } from 'redux-saga/effects';
import { takeEvery, takeLatest } from 'redux-saga'

export const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

function* incrementAsync() {
  // 延迟 1s 在执行 + 1操作
  yield call(delay, 1000);
  yield put({ type: 'INCREMENT' });
}

export default function* rootSaga() {
  // while(true){
  //   yield take('INCREMENT_ASYNC');
  //   yield fork(incrementAsync);
  // }

  // 下面的写法与上面的写法上等效
  yield* takeEvery("INCREMENT_ASYNC", incrementAsync)
}
```

**reducer.js**

```jsx
export default function counter(state = 0, action) {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    case 'INCREMENT_ASYNC':
      return state
    default:
      return state
  }
}
```

##### 使用 `yield*` 对 Sagas 进行排序

你可以使用内置的 `yield*` 操作符来组合多个 Sagas，使得它们保持顺序。

```javascript
function* playLevelOne() { ... }

function* playLevelTwo() { ... }

function* playLevelThree() { ... }

function* game() {
  const score1 = yield* playLevelOne()
  yield put(showScore(score1))

  const score2 = yield* playLevelTwo()
  yield put(showScore(score2))

  const score3 = yield* playLevelThree()
  yield put(showScore(score3))
}
```

注意，使用 `yield*` 将导致该 Javascript 运行环境 *漫延* 至整个序列。个更强大的替代方案是使用更通用的中间件组合机制。

#### redux数据刷新页面数据丢失

使用redux-persist持久化数据存储

**configureStore.js**

```js
import { createStore } from 'redux'
import { persistStore, persistReducer } from 'redux-persist'
// import storage from 'redux-persist/lib/storage' // defaults to localStorage for web
// 存储机制，可换成其他机制，当前使用sessionStorage机制
import sessionStorage from 'redux-persist/lib/storage/session'

import rootReducer from './reducers'
 

const persistConfig = {
  key: 'root', // 必须有的
  storage: sessionStorage, // 缓存机制
  whitelist: ['user', 'system'], // reducer 里持久化的数据,除此外均为不持久化数据
  //  blacklist: ['xxx'] // reducer 里不持久化的数据,除此外均为持久化数据
}

const persistedReducer = persistReducer(persistConfig, rootReducer)
const store = createStore(persistedReducer)
export const persistor = persistStore(store)
export default store
```

**index.js**

```js
import store, { persistor } from './store'
import { PersistGate } from 'redux-persist/integration/react'
import { Provider } from 'react-redux'

// loading属性是加载时显示的组件
const App = () => {
  return (
    <Provider store={store}>
      <PersistGate loading={null} persistor={persistor}>
        <RootComponent />
      </PersistGate>
    </Provider>
  );
};
```



#### 术语表

##### 副作用（side effect）

副作用就是对所处的环境有改变：

函数副作用是指函数被调用，完成了函数既定的计算任务，但同时因为访问了外部数据，尤其是因为对外部数据进行了写操作，从而一定程度地改变了外部环境。 

副作用函数的对立面是纯函数。

##### 纯函数

相同的输入（参数），永远会得到相同的输出，并且在执行过程里面没有副作用 。

数据获取，设置订阅以及手动更改 React 组件中的 DOM 都属于副作用。

##### thunk

其实`thunk`是函数编程界的一个专有名词：返回函数的函数。主要用于*calculation delay*，也就是延迟计算。

用代码演示如下：

```js
function wrapper(arg) {
  // 内部返回的函数就叫`thunk`
  return function thunk() {
    console.log('thunk running, arg: ', arg)
  }
}
// 我们通过调用wrapper来获得thunk
const thunk = wrapper('wrapper arg')

// 然后在需要的地方再去执行thunk
thunk()
```

#### 示例

https://www.reduxjs.cn/introduction/examples

#### 学习资源

https://www.reduxjs.cn/introduction/learning-resources

