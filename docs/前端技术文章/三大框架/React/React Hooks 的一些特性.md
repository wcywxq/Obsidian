---
title: React Hooks 的一些特性
url: https://www.yuque.com/wcywxq/xxble8/ydrldx
---

<a name="fe406e93"></a>

# React Hooks 的一些特性

<a name="e350ad92"></a>

## 1. 逆潮而动

每一个组件内的函数(包括事件处理函数、effects、定时器 或者 API 调用等等)会捕捉某次渲染中定义的 props 和 state

```jsx
function Example(props) {
  useEffect(() => {
    setTimeout(() => {
      console.log(props.counter);
    }, 1000);
  });
}

// 等效于

function Example(props) {
  const counter = props.counter;
  useEffect(() => {
    setTimeout(() => {
      console.log(counter);
    });
  });
}
```

<a name="4f71ead0"></a>

## 2. effects 中的清理

目的: 有些 effects 可能需要有一个清理步骤。本质上，它的目的是消除副作用的 effect，比如取消订阅。

effect 的清除并不会读取 "最新" 的 props。它只能读取到定义它的那次渲染中的 props 的值

```jsx
// First render, props are {id: 10}
function Example() {
  // ...
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(10, handleStatusChange);
    // cleanup for effect from first render
    return () => {
      ChatAPI.subscribeToFriendStatus(10, handleStatusChange);
    };
  });
  // ...
}
// Next render, props are {id: 20}
function Example() {
  // ...
  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(20, handleStatusChange);
    return () => {
      ChatAPI.subscribeToFriendStatus(20, handleStatusChange);
    };
  });
  // ...
}
```

<a name="06aa1b26"></a>

## 3. 避免重复调用

为了避免 effects 不必要的重复调用，我们可以提供给 useEffect 一个依赖数组参数(deps)

```jsx
useEffect(() => {
  document.title = "Hello, " + title;
}, [name]);
```

如果当前渲染中的这些依赖项和上一次运行这个 effect 的时候值一样，因为没有什么需要同步 React 会自动跳过这次 effect。

<a name="3ab31427"></a>

## 4. 两种诚实告知依赖的方法

- 在依赖中包含所有 effect 中用到的组件内的值

```jsx
import React, { useState, useEffect } from "react";
import ReactDOM from "react-dom";

export default function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      setCount(count + 1);
    });
    return () => clearInterval(id);
  }, [count]);

  return (
    <div>
      <p>you clicked it {count} times</p>
      <button onClick={() => setCount(count + 1)}>click me</button>
    </div>
  );
}
ReactDOM.render(<App />, document.getElementById("root"));
```

- 修改 effect 内部的代码以确保它包含的值只会在需要的时候发生变更。我们不想告知错误的依赖 - 我们只是修改 effect 使依赖更少。

<a name="c5999af3"></a>

## 5. 让 Effect 自给自足

我们想要根据前一个状态更新状态的时候，我们可以使用 setState 的 函数形式

```jsx
useEffect(() => {
  const id = setInterval(() => {
    setCount(c => c + 1);
  }, 1000);
  return () => clearInterval(id);
}, []);
```

这正是 setCount(c => c + 1) 做的事情。你可以认为它是在给 React "发送指令" 告知如何更新状态。这种 "更新形式" 在其他情况下也有帮助，比如你需要 "批量更新"。

<a name="587b909c"></a>

## 6. 解耦来自 Actions 的更新

当你想更新一个状态，并且这个状态更新依赖于另一个状态的值时，你可能需要用 useReducer 去替换它们。

当你写类似 setSomething(something => ...) 这种代码的时候，也许就是考虑使用 reducer 的契机。reducer 可以让你 **把组件内发生了什么(actions)** 和 **状态如何响应并更新** 分开表述。

```jsx
const [state, dispatch] = useReducer(reducer, initialState);
const { count, step } = state;
useEffect(() => {
    const id = setInterval(() => {
        dispatch({ type: "tick" }); // Instead of setCount(c => c + step) }, 1000);
        return () => clearInterval(id);
}, [dispatch]);
```

相比于直接在 effect 里面读取状态，它 dispatch 了一个 action 来描述发生了什么。这使得我们的 effect 和 step 状态解耦。我们的 effect 不再关心怎么更新状态，它只负责告诉我们发生了什么。更新的逻辑全都交由 reducer 去统一处理。

```jsx
const initialState = {
  count: 0,
  step: 1
};
function reducer(state, action) {
  const { count, step } = state;
  if (action.type === "tick") {
    return { count: count + step, step };
  } else if (action.type === "step") {
    return { count, step: action.step };
  } else {
    throw new Error();
  }
}
```

<a name="4e22f74b"></a>

## 7. 为什么 useReducer 是 Hooks 的作弊模式

假如我们需要依赖 props 去计算下一个状态。举个例子，也许我们的 API 是 `<Counter step={1} />`。确定的是，在这种情况下，我们没法避免依赖 props.step 。

实际上， 我们可以避免！我们可以把 reducer 函数放到组件内去读取 props。

```jsx
function Counter({ step }) {
  const [count, dispatch] = useReducer(reducer, 0);
  function reducer(state, action) {
    if (action.type === "tick") {
      return state + step;
    } else {
      throw new Error();
    }
  }
  useEffect(() => {
    const id = setInterval(() => {
      dispatch({ type: "tick" });
    }, 1000);
  }, [dispatch]);
  return <h1>{count}</h1>;
}
```

注意：这种模式会使一些优化失败，所以你应该避免滥用它，不过如果你需要，完全可以在 reducer 里面访问 props。

它可以把更新逻辑和描述发生了什么分开。结果是，这可以帮助我移除不必需的依赖，避免不必要的 effect 调用。

<a name="a105eb33"></a>

## 8. 如何不把函数放到 Effect 里

有时候我们可能不想把函数移入 effect 里。

比如，组件内有几个 effect 使用了相同的函数，你不想在每个 effect 里复制黏贴一遍这个逻辑。也或许这个函数是一个 prop。

函数每次渲染都会改变这个事实本身就是个问题。比如有两个 effects 会调用 getFetchUrl:

```jsx
function SearchResults() {
  function getFetchUrl(query) {
    return "https://hn.algolia.com/api/v1/search?query=" + query;
  }

  useEffect(() => {
    const url = getFetchUrl("react");
    // ... Fetch data and do something ...
  }, []); // 🔴 Missing dep: getFetchUrl

  useEffect(() => {
    const url = getFetchUrl("redux");
    // ... Fetch data and do something ...
  }, []); // 🔴 Missing dep: getFetchUrl

  // ...
}
```

我们可能不想把 getFetchUrl 移到 effect 中，因为你想复用逻辑。

<a name="295e64b5"></a>

### 8.1 方法一

- 如果一个函数没有使用组件内的任何值，你应该把它提到组件外面去定义，然后就可以自由地在 effects 中使用

```jsx
// ✅ Not affected by the data flow
function getFetchUrl(query) {
  return "https://hn.algolia.com/api/v1/search?query=" + query;
}

function SearchResults() {
  useEffect(() => {
    const url = getFetchUrl("react");
    // ... Fetch data and do something ...
  }, []); // ✅ Deps are OK

  useEffect(() => {
    const url = getFetchUrl("redux");
    // ... Fetch data and do something ...
  }, []); // ✅ Deps are OK

  // ...
}
```

你不再需要把它设为依赖，因为它们不在渲染范围内，因此不会被数据流影响。它不可能突然意外地依赖于 props 或 state 。

<a name="b3f76ab9"></a>

### 8.2 方法二

- 你也可以把它包装成 useCallback Hook

```jsx
function SearchResults() {
  // ✅ Preserves identity when its own deps are the same
  const getFetchUrl = useCallback(query => {
    return "https://hn.algolia.com/api/v1/search?query=" + query;
  }, []); // ✅ Callback deps are OK

  useEffect(() => {
    const url = getFetchUrl("react");
    // ... Fetch data and do something ...
  }, [getFetchUrl]); // ✅ Effect deps are OK

  useEffect(() => {
    const url = getFetchUrl("redux");
    // ... Fetch data and do something ...
  }, [getFetchUrl]); // ✅ Effect deps are OK

  // ...
}
```

useCallback 本质上是添加了一层依赖检查。它以另一种方式解决了问题 - 我们使函数本身只在需要的时候才改变，而不是去掉对函数的依赖。

如果我们想添加一个输入框允许你输入任意的查询条件(query)。不同于传递 query 参数的方式，现在 getFetchUrl 会从状态中读取。

如果我们把 query 添加到 useCallback 的依赖中，任何调用了 getFetchUrl 的 effect 在 query 改变后都会重新运行：

```jsx
function SearchResults() {
  const [query, setQuery] = useState("react");

  // ✅ Preserves identity until query changes
  const getFetchUrl = useCallback(() => {
    return "https://hn.algolia.com/api/v1/search?query=" + query;
  }, [query]); // ✅ Callback deps are OK

  useEffect(() => {
    const url = getFetchUrl();
    // ... Fetch data and do something ...
  }, [getFetchUrl]); // ✅ Effect deps are OK

  // ...
}
```

这正是拥抱数据流和同步思维的结果。对于通过属性从父组件传入的函数这个方法也适用

```jsx
function Parent() {
  const [query, setQuery] = useState("react");
  // ✅ Preserves identity until query changes
  const fetchData = useCallback(() => {
    const url = "https://hn.algolia.com/api/v1/search?query=" + query;
    // ... Fetch data and return it ...
  }, [query]); // ✅ Callback deps are OK
  return <Child fetchData={fetchData} />;
}

function Child({ fetchData }) {
  let [data, setData] = useState(null);

  useEffect(() => {
    fetchData().then(setData);
  }, [fetchData]); // ✅ Effect deps are OK
  // ...
}
```

因为 fetchData 只有在 Parent 的 query 状态变更时才会改变，所以我们的 Child 只会在需要的时候才去重新请求数据。

<a name="e10c53f8"></a>

## 9. 函数是数据流的一部分么

在 class 组件中，函数属性本身并不是数据流的一部分。组件的方法中包含了可变的 this 变量导致我们不能确定无疑地认为它是不变的。因此，即使我们只需要一个函数，我们也必须把一堆数据传递下去仅仅是为了做 "diff"。我们无法知道传入的 this.props.fetchData 是否依赖状态，并且不知道它依赖的状态是否改变了。

使用 useCallback，函数完全可以参与到数据流中。我们可以说如果一个函数的输入改变了，这个函数就改变了。如果没有，函数也不会改变。使用的 useCallback，属性 props.fetchData 的改变也会自动传递下去。

类似地，useMemo 可以让我们对复杂对象做类似的事情。

```jsx
function ColorPicker() {
  // Doesn't break Child's shallow equality prop check
  // unless the color actually changes.
  const [color, setColor] = useState("pink");
  const style = useMemo(() => ({ color }), [color]);
  return <Child style={style} />;
}
```

<a name="8ecdf1f0"></a>

## 10. 竞态

下面是一个典型的在 class 组件里发请求的例子：

```jsx
class Article extends React.Component {
  state = {
    article: null
  };
  componentDidMount() {
    this.fetchData(this.props.id);
  }
  componentDidUpdate(prevProps) {
    if (prevProps.id !== this.props.id) {
      this.fetchData(this.props.id);
    }
  }
  async fetchData(id) {
    const article = await API.fetchArticle(id);
    this.setState({ article });
  }
  // ...
}
```

这被叫做竞态，这在混合了 async / await（假设在等待结果返回）和自顶向下数据流的代码中非常典型（ props 和 state 可能会在 async 函数调用过程中发生改变）。

Effects 并没有神奇地解决这个问题，尽管它会警告你如果你直接传了一个 async 函数给 effect。

- 解决方式

1. 如果你使用的异步方式支持取消。你可以直接在清除函数中取消异步请求。
2. 或者，最简单的权宜之计是用一个布尔值来跟踪它。

```jsx
function Article({ id }) {
  const [article, setArticle] = useState(null);

  useEffect(() => {
    let didCancel = false;
    const fetchData = async () => {
      const article = await API.fetchArticle(id);
      if (!didCancel) {
        setArticle(article);
      }
    };
    fetchData();
    return () => {
      didCancel = true;
    };
  }, [id]);
  // ...
}
```
