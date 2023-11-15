---
title: React16 文档
url: https://www.yuque.com/wcywxq/xxble8/di9gc4
---

<a name="fea6bf27"></a>

## 1. React 中的核心概念

<a name="fbfd7594"></a>

### 虚拟DOM

1. DOM 的本质是什么？

> 浏览器中的概念，用`js`对象来表示页面上的元素，并提供了操作 `DOM` 对象的 API；

2. 什么是 React 中的 虚拟DOM？（ 虚拟DOM 的本质）：

> 用`js`对象来模拟 页面上的`DOM和DOM嵌套`

3. 为什么要实现 虚拟DOM ？（ 虚拟DOM 的目的）：

> 为了实现页面中，`DOM`元素的高效更新

4. DOM 和 虚拟DOM 的区别：

- `DOM`：浏览器中，提供的概念；用`js`对象，表示页面上的元素，并提供了操作元素的 `API`；
- `虚拟DOM`：是框架中的概念；是开发框架的人员，手动用`js`对象来模拟`DOM`元素和嵌套关系；

5. DOM 树的概念：

> 一个网页的呈现过程：
>
> 1. 浏览器请求服务器获取页面的 `html` 代码；
> 2. 浏览器要先在内存中，解析 `DOM` 结构，并在浏览器内存中，渲染出一棵 `DOM` 树；
> 3. 浏览器把 `DOM` 树，呈现到页面上；

<a name="7e0b7eb8"></a>

### Diff算法

1. tree diff：

> 新旧两棵 `DOM` 树，逐层对比的过程，就是`tree diff`；
> 当整棵`DOM`逐层对比完毕，则所有需要被按需更新的元素，必然能够被找到；

2. component diff：

> 在进行 `tree diff` 的时候，每一层中，组件级别的对比，叫做 `component diff`；
> 如果对比前后，组件的类型相同，则**暂时**认为此组件不许要被更新；
> 如果对比前后，组件的类型不同，则需要移除旧组件，创建新组件，并追加到页面上；

3. element diff：

> 在进行组件对比的时候，如果两个组件的类型相同，则需要进行元素级别的对比，这叫做`element diff`；

<a name="42938890"></a>

## 2. React 中创建组件

<a name="8a91dc29"></a>

### 使用构造函数来创建组件

> 1、在组件中，必须要向外`return`一个合法的`jsx`创建的`虚拟DOM`元素；
> 2、如果要接收外界传递的数据，需要在`构造函数`的参数列表中使用`props`来接收；
> 3、无论是`vue`还是`react`，组件中的`props`永远都是只读`read-only`的，不能被重新赋值；
> 4、组件的名称`首字母`必须是`大写`的
> 5、省略 `.jsx` 文件名

> 打开 `webpack.config.js`，并在导出的配置对象中，新增以下几个节点：

```javascript
resolve: {
  extensions: [".js", ".jsx", ".json"];
}
```

> 如果在一个组件中 `return` 一个 `null`，则表示此组件是空的，什么都不会渲染

```jsx
function Hello(props) {
  return (
    <div>
      这是Hello组件 -- {props.name} -- {props.age} -- {props.gender}
    </div>
  );
}

const user = {
  name: "大黄",
  age: 3,
  gender: "雄性"
};

ReactDOM.render(
  <div>
    <Hello {...user} />
  </div>,
  document.getElementById("#app")
);
```

<a name="1b01f0d6"></a>

### 使用 class 关键字来创建组件

> `public`: 所有成员都可访问
> `private`: 只有当前类可访问
> `protected`: 只有当前类和其子类可访问，外部成员无法访问

> `es6`中的`class关键字`，是实现`面向对象编程`的新形式，也叫做`语法糖`

- constructor 构造器中的 super 函数

> 在子类中， `this` 只能放到 `super` 之后使用
> 子类中的 `super`，其实就是父类中，`contructor`构造器的一个引用

- 最基本的组件结构

> `render`函数的作用：渲染当前组件所对应的`虚拟DOM` 元素

```jsx
import React from 'react'
class 组件名称 extends React.Component {
    render() {
        return <div>这是 class 创建的组件</div>
    }
}
```

- this.props 和 this.state

> 1. `this.props` 接收外界传递的参数，`this.state` 设置私有数据
> 2. 在 `class` 关键字创建的组件中，直接使用 `this.props` 访问传递过来的数据
> 3. `props` 是只读的

```jsx
class Movie extends React.Component {
  constructor() {
    super();
    // 这里的 this.state = {}，就相当于 Vue中的data() { return {} }
    this.state = {
      msg: "这是Movie组件的私有数据"
    };
  }
  render() {
    {
      /* 注意：在 class 组件内容，this 表示当前组件的实例对象 */
    }
    return <div>这是Movie组件 -- {this.props.name}</div>;
  }
}
const user = {
  name: "大黄",
  age: 3,
  gender: "雄性"
};

ReactDOM.render(
  <div>
    <Movie {...user} />
    <h3>{this.state.msg}</h3>
  </div>,
  document.getElementById("#app")
);
```

<a name="03999182"></a>

### 两种创建组件的方式的对比

> 使用 `class` 关键字创建的组件，有自己的 `私有数据(this.state)` 和 `生命周期`
> 使用 `function` 创建的组件，只有 `props`，没有自己的 `私有数据` 和 `生命周期`
> 有状态组件和无状态组件之间的 `本质区别`：有无 `state` 属性和 `生命周期函数`

<a name="58370fd7"></a>

## 3. React 中 style 处理方式

<a name="2dd25b8c"></a>

### 内联

```jsx
style = {{ color: red }}
```

<a name="css"></a>

### css

> 1、如果直接导入 `css` 样式表，默认则是在全局上，整个项目都会生效
> 2、css 模块化，只针对 `class` 选择器 和 `id` 选择器生效
> 引用：`import style from './style.css'`

<a name="79dde80c"></a>

## 4. React 中事件绑定

1. 事件的名称都是 `React` 所提供的，因此名称的首字母必须大写`onClick`，`onMouseOver`
2. 为事件提供的处理函数，必须是以下格式：

```javascript
onClick = { function }
```

3. 用的最多的事件绑定形式为：

```jsx
<button onClick={() => this.show("传参")}>按钮</button>;

// 事件的处理函数，需要定义为 一个箭头函数，然后赋值给 函数名称
show = arg1 => {
  console.log("show方法" + arg1);
};
```

4. 在 `React` 中，如果想要修改 `state` 中的数据，推荐使用 `this.setState({ })`

> 1、在`setState`中，只会把对应的 `state`状态更新，而不会覆盖其它的 `state`状态。
> 2、`this.setState` 方法的执行时 `异步的`。
> 3、如果在调用完 `this.setState`之后，又想立即拿到最新的`state`的值，需要使用 `this.setState({}, callback)`，第二个参数【回调函数】中获取。

<a name="1007ed0a"></a>

## 5. 单向数据流(状态变化 => 自动更新页面)

> 1、`React` 中，默认是 `单向数据流`，只能把 `state` 上的数据绑定到页面，无法把页面中数据的变化，自动同步回 `state`；如果需要把页面上数据的变化，保存到 `state`，需要手动监听`onChange` 事件，拿到最新的数据，手动调用 `this.setState({ })` 更改。
> 2、当为文本框绑定 `value` 值以后，要么同时给标签提供一个 `readOnly` 属性，要么提供一个 `onChange` 事件处理函数。

```jsx
// 方案一：通过事件参数 e 来获取DOM元素的引用
<input
  type="text"
  value={this.state.msg}
  onChange={e => this.textChanged(e)}
/>;

textChanged = e => {
  console.log(e.target.value);
};

// 方案二：通过ref 来获取DOM元素的引用  this.refs.引用名称
<input
  type="text"
  value={this.state.msg}
  onChange={() => this.textChanged()}
  ref="txt"
/>;

textChanged = () => {
  console.log(this.refs.txt.value);
};
```

<a name="0de76a84"></a>

## 6. 生命周期

- 生命周期介绍

> 每个组件的实例，从创建、到运行、直到销毁，在这个过程中，会触发一系列事件，这些事件就叫做组件的生命周期

- React 的生命周期分为三个部分

1. 组件创建阶段

> 只执行一次

> 1. `componentWillMount` => `挂载之前`
> 2. `render` => `正在渲染，虚拟DOM创建到了内存中，还未挂载到页面上`
> 3. `componentDidMount` => `挂载结束，需要操作DOM节点的初始化操作放在这里`

2. 组件运行阶段：

> 根据 `props` 属性或者 `state` 状态的改变，有选择性的执行 `0` 到 `多次`

> 1. `props` 改变之后
> 2. `componentWillReceiveProps` => 当一个挂载的组件接收到新的 `props` 的时候被调用
> 3. `state` 改变之后
> 4. `shouldComponentUpdate（nextprops, nextState）` => 当组件做出是否要更新 `DOM` 的决定的时候被调用，在改变状态的时候可以选择通过( `return true` )或者不通过( `return false`)
> 5. `componentWillUpdate` => 在更新发生之前被调用
> 6. `render` => 数据是新的，页面是旧的

7.`componentDidUpdate` => 数据是新的，页面已经变成了最新的

3. 组件销毁阶段

> 只执行一次

> `componentWillUnmount` => 组件移除或者销毁的时候被调用

<a name="6098aabc"></a>

## 7. 验证数据类型

```javascript
import { ProtoTypes } from "prop-types";

// 定义组件需要传入的参数
MyCompo.protoTypes = {
  a: ProtoTypes.string.isRequired,
  b: ProtoTypes.string.isRequired,
  c: ProtoTypes.number.isRequired
};
```

<a name="57601065"></a>

## 8. flux

<a name="e05dce83"></a>

### 简介

> 传统的 `MVC` 和 `MVVM` 架构设计模式有一个致命的缺点：当项目越来越大、逻辑越来越复杂的时候，数据流动就越显得混乱。

> `Flux` 是致力于解决数据有序传输问题的架构设计模式，来自 `Facebook`。`Flux` 中最大的哲学：数据是 `单向流动` 的。

> [官方手册](https://github.com/facebook/flux/tree/master/examples/flux-concepts)

> `Flux` 中最重要的四个概念：`Dispatcher`、`Store`、`View`、`Action`。

![flux.png](../../assets/di9gc4/1608703835856-e1263bac-124a-40da-ae3c-b45a4c8081ae.png) <a name="e2d6d0e3"></a>

### 基本概念

1. 概述
   - `flux` 是一个管理 `App` 中数据流动的模式。
   - 最关键的概念：`数据的流动是单向的`。
2. Dispatcher 调度者
   - `Dispatcher` 接受 `action`，并且要把这些 `action` 分派给已经注册到 `Dispatcher` 的 `store` 上
   - 所有的 `store` 都将接收所有的 `action`
   - 在每个 `App` 中，应该确保只有一个 `Dispatcher` 的实例
3. store 仓库
   - `store` 是在 `App` 中持有数据的仓库
   - 所有的 `store` 要在 `App` 的 `Dispatcher` 上注册，确保它们可以接收 `action`
   - `store` 中的数据只能被 `action` 改变。
   - `store` 中不能够有公共的 `setter`，只能有 `getter`
   - `store` 决定了它们愿意响应哪些 `actions`
   - 无论何时，`store` 中的数据发生改变，就会触发一个 `change` 事件
   - 同一个 `App` 中可能有很多 `store`
4. Action 行为
   - `Action` 定义了 `App` 内部的 `API`
   - 它们捕获所有可能改变 `App` 的途径和方法
   - 它们是简单的`对象`，并且要有 `type属性` 和 `其他的一些数据属性`
   - `Action` 应该有一个具有语义的、直观的表示它是做什么的名字
   - 所有的`store`都将接收同一个`action`，并且通过这个 `action`，`store` 会知道它们要清除、更新哪些数据
5. Views 视图
   - `store` 中的数据被展示在了`view`上
   - `View` 层可以使用任何框架
   - 当一个视图想要获取 `store` 中的数据，它必须 `subscribe 订阅` 一下该 `store` 的`change` 事件
   - 当 `store` 触发了 `change` 事件，此时 `view` 就能得到新的数据并且重新渲染
   - 如果一个组件要使用 `store`，但是没有订阅这个 `store`，此时就会出错
   - `Action` 最常见的产生原因是：在 `App` 中的某一个部分，因为用户的交互行为，而被此`view` `dispatch`出来了

<a name="redux"></a>

### redux

- 简介
  - [官网](https://redux.js.org/)
  - Redux 就是 Flux 思想在 React 中的实现
  - Redux 是一个可预测状态的 Js app 容器
  - `[通过例子来学习redux](https://github.com/reactjs/redux/tree/master/examples)`
- Redux 创建的步骤
  - 设置一个 `reducer`；
  - 创建一个`store`，`Redux.createStore(reducer)`
  - 创建 `render` 函数
  - 注册 `render`，`store.subscribe(render)`
  - 监听，此时要记得 `store.dispatch(action)`，不是直接修改`store`

<a name="React-Redux"></a>

### React-Redux

- 简介
  - 将 `react` 和 `redux` 合并起来，可以让任何组件在任何地方看见 `store`
  - [官方文档](https://github.com/reactjs/react-redux/tree/master/docs)
  - `React-Redux` 给我们提供了：`Provider组件`，`connect函数`
- Provider 组件

> 1、使用 `react-redux` 提供的 `Provider` 组件传递 `store` 上下文之后，`包裹在其中的所有组件` 全都可以识别这个上下文

> 2、在 `Provider` 组件内部的自定义组件可以使用 `connect()` 函数，但是在其外部的不可使用

- connect 函数

> 1、将 `React组件` 和 `Redux` 的 `store` 进行连接
> 2、`connect` 提供了一个很方便的 `API` 能够适应绝大多数工作
> 3、它没有更改你传进来的类，反而会返回一个已经连接好的新类
> 4、提供了两个参数：`mapStateToProps`, `mapDispatchToProps`

- mapStateToProps

> 1、如果传入`mapStateToProps`，此时这个组件将订阅 `Redux` 中 `store` 的更新信息；
> 2、这意味着无论任何时候 `store` 被更改了，`mapStateToProps` 函数都将会被调用，`mapStateToProps` 的返回值必须是一个 `Object`；
> 3、这个 `Object` 将与组件的 `props` 融合，也就是说，这个返回的 `Object` 中的 `key` 将自动成为组件的 `props` 中的成员
> 4、如果不想订阅 `store` 的更新，此时可以不传递这个参数，采用 `null` 占位

- mapDispatchToProps

> 如果向 `connect` 函数中传入了第二个参数，并且是一个 `函数`，那么这个函数将获得`dispatch` 方法，该方法可以通过 `emit action`，间接的导致 `state` 的改变
> 可以使用 `bindActionCreators()` 方法轻松的将 `Action creator`(返回 `action` 的函数)接口和 `dispatch` 进行绑定

- 书写规则

> index.js

```jsx
import React from "react";
import { render } from "react-dom";
import { createStore } from "redux";
import { Provider } from "react-redux";
import App from "./containers/App";
import reducer from "./reducers";

const store = createStore(reducer);
render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

> App.js

```jsx
import React from "react";
import { connect } from "react-redux";
import * as actions from "./actions";

class App extends React.Component {
  constructor(props) {
    super();
    console.log(props);
    // props: { r: 0, g: 0, b: 0, actions: 许多方法 }
  }
  render() {
    return <div>这是App组件</div>;
  }
}
const mapStateToProps = state => {
  return {
    r: state.r,
    g: state.g,
    b: state.b
  };
};
const mapDispatchToProps = dispatch => {
  return {
    actions: bindActionCreators(actions, dispatch)
  };
};
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(App);
```

> reducer.js

```jsx
export default (state, action) => {
  if (state == undefined) {
    state = { r: 0, g: 0, b: 0 };
  }
  if (action.type == "ADD") {
    return {
      ...state,
      r: state.r + 1
    };
  }
  return state;
};
```

> actions.js

```jsx
export const ADD = () => { "type": "ADD" }
```

<a name="b51b03f9"></a>

### 组件内部的 state 和全局的 state

> 组件的数据三兄弟：`state`, `props`, `context` 不管是谁发生改变，都会引发 `render()` 执行，视图会被重绘。但是，构造函数不会被重新执行。所以不管基于什么理由，都不需要将全局的状态，用自己组件的 `state` 接收，而仅需要用`connect`连接一下全局`store`，然后使用`this.props.**`即可。

<a name="c798dcc0"></a>

### reducer 模块化

```javascript
import { combineReducers } from "redux";
import todoReducers from "./todoReducer.js"; // 标准reducer

export default combineReducers({
  todoReducers
});
```

<a name="redux-logger"></a>

### redux-logger

> 打印 redux log

```javascript
import { createStore, applyMiddleware } from "redux";
import { createLogger } from "redux-logger";
import reducer from "./reducers/index.js";
let store = createStore(reducer, applyMiddleware(createLogger()));
```

<a name="redux-thunk"></a>

### redux-thunk

> 解决异步问题

> `redux-thunk`帮助我们在所有的组件的 `props` 中添加了一个 `dispatch` 方法。

> 当然，这个组件一定要被 `connect` 函数进行处理

> 注意，如果使用 `thunk`，则 `connect` 函数不能传入第二个参数，否则会导致无法获取 `this.props.dispatch()`

```javascript
// 入口文件
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';
let store = createStore(reducer, applyMiddleware(thunk))

/** 组件中 */
/** 第一种写法，并没有将actions进行抽离 */
$.get('/shu.txt', data => {
    var number = Number(data);
    this.props.dispatch({
        "type": "ADD",
        number
    })
})
/** 第二种写法:常用 */
// 可枚举
import * as actions from './actions/actions.js'

class ** extends React.Component {
    ...
    add() {
        this.props.dispatch(actions.add())
        this.props.dispatch(actions.minus())
    }
    ...
}
/** 现在，一般不会再写第二个参数了，就是说省略掉mapDispatchToProps */
export default connect(
 (state) => {
    return {
        state: state
    }
 })(**)
// actions.js
/* 现在，异步的Action Creator不要直接返回 Action，而是返回一个携带 dispatch 的函数；这个函数相当于“延长”了dispatch的持续时间。*/
export const add = () => (dispatch, getState) => {
    console.log(getState()) // getState可以获取全局数据
    $.get('/shu.txt', data => {
        var number = Number(data);
        dispatch({"type": "MINUS", number})
    })
}
export const minus = () => { "type": "MINUS" }
// reducer.js
export default (state = 0, action) => {
    switch(action.type) {
        case "ADD":
            return state + action.number
        case "MINUS":
            return state - 1
    }
    return state;
}
```

<a name="098d4e0d"></a>

## 9. react-router4.x

<a name="61a3ec66"></a>

### 介绍

- [官网](https://reacttraining.com/react-router)
- 安装：`npm install react-router-dom`

<a name="demo"></a>

### demo

> `exact`表示严格匹配，如果路径为 `path="/"` 的 `Route` 不设置该属性，则会自动向下匹配路由，即都会显示出来

```jsx
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";

function Index() {
  return <h2>Home</h2>;
}

function About() {
  return <h2>About</h2>;
}

function Users() {
  return <h2>Users</h2>;
}

function AppRouter() {
  return (
    <Router>
      <div>
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about/">About</Link>
            </li>
            <li>
              <Link to="/users/">Users</Link>
            </li>
          </ul>
        </nav>

        <Route path="/" exact component={Index} />
        <Route path="/about/" component={About} />
        <Route path="/users/" component={Users} />
      </div>
    </Router>
  );
}
export default AppRouter;
```

<a name="d32ddad9"></a>

### 动态路由

```jsx
// App.js 定义动态路由
<Route path="/content/:aid"></Route>
// news.js 跳转
<Link to={`/content/${value.aid}`}></Link>
// Content.js 跳在生命周期函数中获取动态路由参数
class Content extends Component {
    /* ... */
      componentDidMount() {
          const { match } = this.props
          // 获取到传递的动态路由参数
          console.log(match.params.aid)
      }
    /* ... */
}
```

<a name="48d577aa"></a>

### get 传值

```jsx
// App.js 定义动态路由
<Route path="/content"></Route>
// news.js 跳转
<Link to={`/content?aid=${value.aid}`}></Link>
// Content.js 跳在生命周期函数中获取动态路由参数
class Content extends Component {
    ...
      componentDidMount() {
          const { location } = this.props
          // 获取到传递的动态路由参数
          console.log(location.search)
      }
    ...
}
```

<a name="2a32f5ec"></a>

### js 控制跳转

1. 引入 `Redirect` 组件
2. 定义一个 `flag`

```javascript
this.state = {
  loginFlag: false
};
```

3. 在 `Render` 中判断 `flag`，从而来决定是否进行跳转

```jsx
if (this.state.loginFlag) {
  return <Redirect to={{ pathname: "/" }} />;
}
```

4. 执行 `js` 跳转，通过 `js` 改变 `loginFlag` 的状态，改变以后，就可以从新的 `render` 中通过 `Redirect` 自己进行跳转

<a name="6f5f6eed"></a>

### 模块化路由

> router.js

```jsx
import Home from './components/Home/'
import About from './components/About/'
import User from './components/User/'
import UserList from './User/UserList'
import UserInfo from './User/UserInfo'

let router = [
    {
        path: '/',
        component: Home,
        exact: true
    },
    {
        path: '/about',
        component: About
    },
    {
        path: '/User',
        component: User,
        routes: [ // 嵌套路由设置
            {
                path: '/user/',
                component: UserList
            }, {
                path: '/user/info',
                component: UserInfo
            }
        ]
    }
]
export default router;
```

> App.js入口文件

```jsx
import React, { Component } from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";
import router from "./router.js";

class App extends Component {
  render() {
    return (
      <Router>
        <div>
          {router.map((route, key) => {
            if (route.exact) {
              return (
                <Route
                  exact
                  key={key}
                  path={route.path}
                  render={props => (
                    // 向子组件传递子路由
                    <route.component {...props} routes={route.routes} />
                  )}
                />
              );
            } else {
              return (
                <Route
                  key={key}
                  path={route.path}
                  render={props => (
                    // 向子组件传递子路由
                    <route.component {...props} routes={route.routes} />
                  )}
                />
              );
            }
          })}
        </div>
      </Router>
    );
  }
}
```

> User.js

```jsx
import React, { Component } from "react";
import { Route, Link } from "react-router-dom";

class User extends Component {
  componentWillMount() {
    console.log(this.props.routes);
  }
  render() {
    return (
      <div>
        <div className="contenr">
          <div className="left">
            <Link />
          </div>
          <div className="right">
            {this.props.routes.map((route, key) => {
              return (
                <Route
                  exact
                  key={key}
                  path={route.path}
                  component={route.component}
                />
              );
            })}
          </div>
        </div>
      </div>
    );
  }
}
export default User;
```

<a name="f757cf23"></a>

### 常用路由组件

- *BrowserRouter*：使用 `HTML5` 历史记录 `API` (`pushState`，`replaceState` 和`popstate` 事件)的 `<Router>` 来保持 `UI` 与 `URL` 的同步
- *HashRouter*：使用 `URL` 的哈希部分(即 `window.location.hash` )的<路由器>可以保持您的 `UI` 与 `URL` 同步。注意：哈希历史记录不支持 `location.key` 或 `location.state`。 在以前的版本中，我们试图缓和行为，但是有一些边缘案例我们无法解决。 任何需要此行为的代码或插件将无法正常工作。 由于此技术仅用于支持旧版浏览器，因此我们建议您将服务器配置为使用`<BrowserHistory>`
- *Link*：渲染成 `a` 标签
- *NavLink*：一种特殊版本的 `<Link>`，当与当前 `URL` 匹配时，将向渲染元素添加样式属性。
- *Redirect*：重定向
- *Route*：在位置与路线的路径匹配时呈现一些 `UI`。
- *Switch*：只渲染命中的第一个 `<Route>` 或 `<Redirect>` 。

```jsx
  // Switch的用法
  import { Switch, Route } from "react-router";
  <Switch>
      <Route exact path="/" component={Home} />
      <Route path="/about" component={About} />
      <Route path="/:user" component={User} />
      <Route component={NoMatch} />
  </Switch>;
```

<a name="7557e52d"></a>

## 10. context

- 介绍

> 在一个典型的 `React` 应用中，数据是通过 `props` 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，`UI` 主题），这些属性是应用程序中许多组件都需要的。`Context` 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 `props`。

- 繁琐的 `props` 方式

```jsx
class App extends React.Component {
    render() {
        return <Toolbar theme="dark" />
    }
}
function Toolbar(props) {
    return (
        <div>
            <ThemeButton theme={props.theme} />
        </div>
    )
}
class ThemeButton extends React.Component {
    render() {
        <Button theme={this.props.theme}>按钮</Button>
    }
}
```

- 使用 `context`

```jsx
// Context 可以让我们无须明确地传遍每一个组件，就能将值深入传递进组件树。
// 为当前的 theme 创建一个 context（“light”为默认值）。  
const ThemeContext = React.createContext("light");

class App extends React.Component {
    render() {
        // 使用一个 Provider 来将当前的 theme 传递给以下的组件树。
        // 无论多深，任何组件都能读取这个值。
        // 在这个例子中，我们将 “dark” 作为当前的值传递下去。
        return (
            <ThemeContext.Provider value="dark">
                <Toolbar />
            </ThemeContext.Provider>
        )
    }
}
function ToolBar() {
    return (
        <div>
            <ThemeButton />
        </div>
    )
}
class ThemeButton extends React.Component {
    // 指定 contextType 读取当前的 theme context。
  // React 会往上找到最近的 theme Provider，然后使用它的值。
    // 在这个例子中，当前的 theme 值为 “dark”。
    static contextType = ThemeContext;
    render() {
        return <Button theme={this.context}>按钮</Button>
    }
}
```

<a name="663b2eaa"></a>

## 11. react-hooks

> `Hook` 是 `React 16.8` 的新增特性。它可以让你在不编写 `class` 的情况下使用 `state` 以及其他的 `React` 特性。

<a name="e1945b8c"></a>

### 11.1 State Hook

> `useState` 就是一个 `Hook`，类似 `class` 组件的 `this.setState`，但是它不会把新的 `state` 和旧的 `state` 进行合并。`useState` 会返回一对值：*当前状态* 和一个让你 *更新它的函数*，你可以在事件处理函数中或其他一些地方调用这个函数。

- 计数器

```jsx
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 “count” 的 state 变量。
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

// 等价的 class 示例
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

- 声明多个 `state` 变量

```jsx
function ExampleWithManyStates() {
  // 声明多个 state 变量！
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```

- 什么是 `Hook`

> `Hook` 是一些可以让你在函数组件里“钩入” `React state` 及生命周期等特性的函数。`Hook` 不能在 `class` 组件中使用 —— 这使得你不使用 `class` 也能使用 `React`。

- 惰性 state

```jsx
const [state, setState] = useState(() => {
  const initialState = someExpensiveComputation(props);
  return initialState;
});
```

<a name="372afb18"></a>

### 11.2 Effect Hook

> `Effect Hook` 可以让你在函数组件中执行副作用操作

<a name="4e2a4a64"></a>

#### (1) 无需清除的 `Effect`

> 有时候，我们只想在 `React` 更新 `DOM` 之后运行一些额外的代码。比如发送网络请求，手动变更 `DOM`，记录日志，这些都是常见的无需清除的操作。因为我们在执行完这些操作之后，就可以忽略他们了。

- 使用 `class` 的示例

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

- 使用 `Hook` 的示例

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

> `useEffect` 做了什么？ 通过使用这个 `Hook`，你可以告诉 `React` 组件需要在渲染后执行某些操作。`React` 会保存你传递的函数（我们将它称之为 “`effect`”），并且在执行 `DOM` 更新之后调用它。在这个 `effect` 中，我们设置了 `document` 的 `title` 属性，不过我们也可以执行数据获取或调用其他命令式的 API。

> 为什么在组件内部调用 `useEffect`？ 将 `useEffect` 放在组件内部让我们可以在 `effect` 中直接访问 `count` `state` 变量（或其他 `props`）。我们不需要特殊的 `API` 来读取它 —— 它已经保存在函数作用域中。`Hook` 使用了 `JavaScript` 的闭包机制，而不用在 `JavaScript` 已经提供了解决方案的情况下，还引入特定的 React API。

> `useEffect` 会在每次渲染后都执行吗？ 是的，默认情况下，它在第一次渲染之后和每次更新之后都会执行。（我们稍后会谈到如何控制它。）你可能会更容易接受 `effect` 发生在“渲染之后”这种概念，不用再去考虑“挂载”还是“更新”。`React` 保证了每次运行 `effect` 的同时，`DOM` 都已经更新完毕。

{% note warning, 与 `componentDidMount` 或 `componentDidUpdate` 不同，使用 `useEffect` 调度的 `effect` 不会阻塞浏览器更新屏幕，这让你的应用看起来响应更快。大多数情况下，`effect` 不需要同步地执行。在个别情况下（例如测量布局），有单独的 `useLayoutEffect Hook` 供你使用，其 `API` 与 `useEffect` 相同。 %}

<a name="ae972712"></a>

#### (2) 需要清除的 `Effect`

> 之前，我们研究了如何使用不需要清除的副作用，还有一些副作用是需要清除的。例如订阅外部数据源。这种情况下，清除工作是非常重要的，可以防止引起内存泄露！现在让我们来比较一下如何用 `Class` 和 `Hook` 来实现。

- 使用 `class` 的示例

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

- 使用 `Hook` 的示例

{% note warning, 眼尖的读者可能已经注意到了，这个示例还需要编写 componentDidUpdate 方法才能保证完全正确。我们先暂时忽略这一点，本章节中后续部分会介绍它。 %}

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

> 为什么要在 `effect` 中返回一个函数？ 这是 `effect` 可选的清除机制。每个 `effect` 都可以返回一个清除函数。如此可以将添加和移除订阅的逻辑放在一起。它们都属于 `effect` 的一部分。

> `React` 何时清除 `effect`？ `React` 会在组件卸载的时候执行清除操作。正如之前学到的，`effect` 在每次渲染的时候都会执行。这就是为什么 `React` 会在执行当前 `effect` 之前对上一个 `effect` 进行清除。我们稍后将讨论为什么这将助于避免 `bug` 以及如何在遇到性能问题时跳过此行为。

{% note warning, 并不是必须为 `effect` 中返回的函数命名。这里我们将其命名为 `cleanup` 是为了表明此函数的目的，但其实也可以返回一个箭头函数或者给起一个别的名字。 %}

<a name="ede5397c"></a>

### 11.3 Hook 规则

<a name="ebf8e573"></a>

#### (1) 只在最顶层使用 `Hook`

> 不要在循环，条件或嵌套函数中调用 `Hook`， 确保总是在你的 `React` 函数的最顶层调用他们。遵守这条规则，你就能确保 `Hook` 在每一次渲染中都按照同样的顺序被调用。这让 `React` 能够在多次的 `useState` 和 `useEffect` 调用之间保持 `hook` 状态的正确。

<a name="e9917ca1"></a>

#### (2) 只在 `React` 函数中调用 `Hook`

> 不要在普通的 `JavaScript` 函数中调用 `Hook`。你可以：

- ✅ 在 `React` 的函数组件中调用 `Hook`
- ✅ 在自定义 `Hook` 中调用其他 `Hook`

<a name="27f298a9"></a>

### 11.4 自定义 Hook

> 通过自定义 `Hook`，可以将组件逻辑提取到可重用的函数中。

<a name="98ad417c"></a>

#### (1) 提取自定义 `Hook`

> 当我们想在两个函数之间共享逻辑时，我们会把它提取到第三个函数中。而组件和 `Hook` 都是函数，所以也同样适用这种方式。

> 自定义 `Hook` 是一个函数，其名称以 “`use`” 开头，函数内部可以调用其他的 `Hook`。 例如，下面的 `useFriendStatus` 是我们第一个自定义的 `Hook`:

```jsx
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

<a name="44cb8100"></a>

#### (2) 使用自定义 `Hook`

```jsx
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```

<a name="e74ceb0d"></a>

### 11.5 Hook API 索引

> 参见 [Hook API](https://react.docschina.org/docs/hooks-reference.html)

<a name="bd0a855d"></a>

#### 11.5.1 基础 Hook

<a name="useState"></a>

##### useState

> [点击跳转](#linkUseState)

<a name="useEffect"></a>

##### useEffect

> [点击跳转](#linkUseEffect)

<a name="useContext"></a>

##### useContext

> 订阅 `context` 的变化，感觉就是对于获取 `context` 的值换了一种写法而已。相对于之前的写法，在函数组件中添加 `context` 更加简单。

```jsx
const context = React.createContext({})
const { Provider, Consumer  } = context; 

// hooks的写法
class App extends React.Component {
    return (
        <Provider value={{ name: 'li' }}> 
            <Hello/>
        </Provider>
    </div>
}
function Hello () {
    const value = useContext(context); 
    return <h1>value: {value.name}</h1>
}
  
// 原本的写法
function Hello (props) {
    function render ({name}) {
      return <h1>value: {value.name}</h1>
    }
    return (
      <Consumer>
        {render}
      </Consumer>
    )
}
```

<a name="b56fd342"></a>

#### 11.5.2 额外的 Hook

<a name="useReducer"></a>

##### useReducer

> 类似于 `redux` 那样的状态更新方案。使用场景（基本上就是 `redux` 的应用场景），管理的状态值是对象，并且键值较多。`state` 每个 `key` 修改的逻辑比较复杂，需要单独放到一个文件里面管理。

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
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
    </>
  );
}
```

<a name="useCallback"></a>

##### useCallback

> 仅在指定的依赖项发生变化时，会返回一个新的函数引用，函数体并没有发生变化。

```jsx
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

> 这样使用的好处：不会在每次组件 `render` 的时候，重新生成一个函数，节省开销。例如

```jsx
function f () {
  const cacheCallback = useCallback(
      () => {
    			doSomething(a, b);
  			},
  			[a, b],
  )
  // 和下面这样的形式相比, 每次组件渲染的时候，都会重新创建一个 doSometing 函数
  function doSometing (a,b) {}
}
```

> 可以保持函数的引用保持不变。我们都知道在类组件，事件处理函数基本上都是通过 `this.method` 的方式绑定的，这样做的方式有一个好处，对方法的引用一直保持不变。 那么在函数组件就可以通过使用 `useCallback` 来实现。

> 可以实现在子组件把该回调作为依赖处理。

```jsx
function Parent ({a, b}) {
  const cacheCallback = useCallback(
      () => {
        doSometing(a, b);
      },
      [a, b]
  )
  return <Child handler={cacheCallback}/>
}

function Child ({ handler }) {
  useEffect(() => {
    handler();
  }, [handler])
}
```

<a name="useMemo"></a>

##### useMemo

> 类似于 `vue` 的 `computed`，在依赖发生变化的时候重新计算缓存值。其实自己实现起来也很容易，和 `vue` 的计算属性不同的是，`vue` 的计算属性是自动收集依赖的，而使用 `useMeno` 需要手动在数组种传入依赖项。

```jsx
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

> `useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`

<a name="useRef"></a>

##### useRef

> 故名思义，该 `hook` 主要是用来获取组件实例或者或者 `dom` 节点。 但是它更有用的地方，是可以返回一个在组件生命周期内，引用不变的对象。

```jsx
function f () {
  const elRef = uesRef(null);
  return <div ref={elRef}></div>
}
```

> 用来存储数据的话，考虑下面的场景。

```jsx
let handler = () => {}; // 事件处理函数
// 不使用 useRef, 可以使用函数外部的一个变量来存储数据
function f () {
  useEffect(() => {
    window.addEventListener('scroll', handler)
  }, [])
  
  const moveScroll = useCallback(
    () => {
      window.removeEventListener('scorll', handler)
    },
    []
  )
  
  return <div onClick={moveScroll} ref={elRef}>移除scroll监听</div>
}

// 使用useRef的版本，可以使代码更加内聚。但是前提是必须要理解useRef这个hooks。
function f () {
  
  const handler = useRef(null);
  handler.current = () => {}  // 事件处理
  
  useEffect(() => {
    window.addEventListener('scroll', handler.current)
  }, [])
  
  const moveScroll = useCallback(
    () => {
      window.removeEventListener('scorll', handler.current)
    },
    []
  )
  
  return <div onClick={moveScroll} ref={elRef}>移除scroll监听</div>
}
```

<a name="useImperativeHandle"></a>

##### useImperativeHandle

> `useImperativeHandle` 可以让你在使用 `ref` 时自定义暴露给父组件的实例值。

```jsx
const Fancy = React.forwardRef((props, ref) => {
    return <div>
        <input type="text" ref={ref}/>
    </div> 
})

function Hello () {
    const ref = useRef(null); 

    useEffect(() => {
        console.log('current', ref); // { current: Input }
    }, [])

    return <Fancy ref={ref}/>
}
```

<a name="useLayoutEffect"></a>

##### useLayoutEffect

> 函数签名和 `useEffect` 是一样的， 可以使用它来读取 `DOM` 布局并 `同步` 触发重渲染。

<a name="useDebugValue"></a>

##### useDebugValue

> 用来给 `hooks` 添加上打印信息。
