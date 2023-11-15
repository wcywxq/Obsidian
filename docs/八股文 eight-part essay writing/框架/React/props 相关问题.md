---
title: props 相关问题
url: https://www.yuque.com/wcywxq/mxunh7/aopf74
---

<a name="rialk"></a>

## state 和 props 有什么区别

- props

props 是一个从外部传进组件的参数，主要作用就是从父组件向子组件传递数据，它具有可读性和不可变性，只能通过外部组件主动传入新的 props 来重新渲染子组件，否则子组件的 props 以及展现形式不会改变。

- state

state 的主要作用是用于组件保存、控制以及修改自己的状态，它只能在 constructor 中初始化，它算是组件的私有属性，不可通过外部访问和修改，只能通过组件内部的 this.setState 来修改，修改 state 属性会导致组件的重新渲染。
:::info
区别：

1. props 是传递给组件的(类似于函数的形参)，而 state 是在组件内被组件自己管理的(类似于在一个函数内声明的变量)。
2. props 是不可修改的，所有 react 组件都必须像纯函数一样保护它们的 props 不被更改。
3. state 是在组件中创建的，一般在 constructor 中初始化 state。state 是对变的、可以修改，每次 setState 都是异步更新的。
   ::: <a name="SuDHN"></a>

## props 为什么是只读的

this.props 是组件之间沟通的一个接口，原则上来讲，它只能从父组件流向子组件。React具有浓重的函数式编程的思想。
提到函数式编程就要提一个概念：纯函数。它有几个特点：

- 给定相同的输入，总是返回相同的输出。
- 过程没有副作用。
- 不依赖外部状态。

this.props 就是汲取了纯函数的思想。props 的不可以变性就保证的相同的输入，页面显示的内容是一样的，并且不会产生副作用 <a name="uZjWx"></a>

## props 改变时更新组件的方法有哪些

在一个组件传入的 props 更新时重新渲染该组件常用的方法是在 componentWillReceiveProps 中将新的 props 更新到组件的state 中(这种state被成为派生状态（Derived State）)，从而实现重新渲染。React 16.3 中还引入了一个新的钩子函数getDerivedStateFromProps 来专门实现这一需求。

- **componentWillReceiveProps（已废弃）**

在 react 的 componentWillReceiveProps(nextProps)生命周期中，可以在子组件的 render 函数执行前，通过 this.props 获取旧的属性，通过 nextProps 获取新的 props，对比两次 props 是否相同，从而更新子组件自己的 state。

这样的好处是，可以将数据请求放在这里进行执行，需要传的参数则从 componentWillReceiveProps(nextProps) 中获取。而不必将所有的请求都放在父组件中。于是该请求只会在该组件渲染时才会发出，从而减轻请求负担。

- **getDerivedStateFromProps（16.3引入）**

这个生命周期函数是为了替代 componentWillReceiveProps 存在的，所以在需要使用 componentWillReceiveProps 时，就可以考虑使用 getDerivedStateFromProps 来进行替代。
两者的参数是不相同的，而 getDerivedStateFromProps 是一个静态函数，也就是这个函数不能通过 this 访问到 class 的属性，也并不推荐直接访问属性。而是应该通过参数提供的 nextProps 以及 prevState 来进行判断，根据新传入的 props 来映射到state。

需要注意的是，**如果props传入的内容不需要影响到你的state，那么就需要返回一个null**，这个返回值是必须的，所以尽量将其写到函数的末尾：

```jsx
static getDerivedStateFromProps(nextProps, prevState) {
    const {type} = nextProps;
    // 当传入的type发生变化的时候，更新state
    if (type !== prevState.type) {
        return {
            type,
        };
    }
    // 否则，对于state不进行任何操作
    return null;
}
```

<a name="Kso9W"></a>

## 检验 props 的方式，验证 props 的目的

**React **为我们提供了 **PropTypes **以供验证使用。当我们向 **Props **传入的数据无效（向 Props 传入的数据类型和验证的数据类型不符）就会在控制台发出警告信息。它可以避免随着应用越来越复杂从而出现的问题。并且，它还可以让程序变得更易读。

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
