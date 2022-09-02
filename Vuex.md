### Vuex

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。

#### 核心概念

state、getter、mutation、action、module

##### state

单一状态树

state：存储数据，存储状态；在根实例中注册了store后，用this.$store.state来访问

存放数据方式为响应式,Vue组件

##### getter

getter：可以认为是 store的计算属性，它的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

##### mutation

mutation：更改Vuex的 store中的状态的唯一方法是 commit提交 mutation。

##### action

action：代替mutations执行异步操作或复杂逻辑的操作，通过提交 mutation 间接更变状态。

##### module

module：将 store分割成模块,每个模块都具有state、 mutation、 action、 getter、甚至是嵌套子模块。



Vuex的数据传递流程：当组件进行数据修改的时候我们需要调用 dispatch来触发 actions里面的方法。 actions里面的每个方法中都会有一个context参数，通过context 调用 commit 方法，触发 mutations里面的方法进行状态的修改。