---
title: 编写 React 组件时常见的 5 个错误
url: https://www.yuque.com/wcywxq/xxble8/lscqxu
---

<a name="b343d5c4"></a>

# 编写 React 组件时常见的 5 个错误

<a name="afd85692"></a>

## 1. 在不需要重渲染时使用 useState

React 的一个核心概念是处理状态。你可以通过状态控制整个数据流和渲染过程。每次树被重新渲染时，很可能是因为状态的变化。

使用 useState hook，你现在还可以在函数组件中定义状态，这种方法可以真正简洁地在 React 中处理状态。但正如以下示例所示，它也可能被滥用。

关于下面这个示例我们需要说明一下。假设我们有两个按钮，一个按钮是计数器，另一个按钮使用当前计数发送请求或触发动作。但是，当前编号永远不会显示在组件内。当你单击第二个按钮时才需要这个请求。

这很危险❌

```jsx
function ClickButton(props) {
  const [count, setCount] = useState(0);
  const onClickCount = () => {
    setCount((c) => c + 1);
  };
  const onClickRequest = () => {
    apiCall(count);
  };
  return (
    <div>
      <button onClick={onClickCount}>Counter</button>
      <button onClick={onClickRequest}>Submit</button>
    </div>
  );
}
```

<a name="2bed77ed"></a>

### 问题⚡

乍一看，你可能会问这到底有什么问题？状态不就是这样用的吗？你当然没错，它运行很正常，并且可能永远不会出问题，但是在 React 中，每个状态更改都将强制对该组件，很有可能还有其子级进行重渲染，但在上面的示例中，因为我们从未在渲染部分中使用这个状态，结果每次设置计数器时都会有不必要的重渲染，这可能会影响性能或产生意外的副作用。

<a name="6d6afad5"></a>

### 解决方案✅

如果要在组件内部使用一个变量，希望该变量在渲染之间保持其值，但又不强制重新渲染，则可以使用 useRef hook。它将保留值，但不强制重新渲染组件。

```jsx
function ClickButton(props) {
  const count = useRef(0);
  const onClickCount = () => {
    count.current++;
  };
  const onClickRequest = () => {
    apiCall(count.current);
  };
  return (
    <div>
      <button onClick={onClickCount}>Counter</button>
      <button onClick={onClickRequest}>Submit</button>
    </div>
  );
}
```

<a name="c3296fec"></a>

## 2. 使用 router.push 代替链接

这可能是一个显而易见的错误，其实和 React 本身没什么关系，但是当人们编写 React 组件时经常会犯这种错误。

假设你要编写一个按钮，单击该按钮应将用户重定向到另一个页面。由于它是一个 SPA，因此这个动作是客户端路由机制。于是你需要某种库来执行此动作。在 React 中最流行的是 react-router，下面的示例就会使用它。

所以，添加一个点击侦听器会将用户重定向到所需的页面，对吗？

<a name="0682bbd6"></a>

### 这很危险❌

```jsx
function ClickButton(props) {
  const history = useHistory();
  const onClick = () => {
    history.push('/next-page');
  };
  return <button onClick={onClick}>Go to next page</button>;
}
```

<a name="2bed77ed-1"></a>

### 问题⚡

就算这段代码对于大多数用户来说都可以正常工作，但这里也有严重的可访问性问题。这个按钮根本不会被标记为链接到另一个页面，于是屏幕阅读器几乎无法识别它。而且你能在新标签页或窗口中打开它吗？很可能做不到。

<a name="6d6afad5-1"></a>

### 解决方案✅

只要指向其他页面的链接带有某种用户交互，就要尽量用 < Link> 组件或常规的 < a> 标签处理。

```jsx
function ClickButton(props) {
  return (
    <Link to="/next-page">
      <span>Go to next page</span>
    </Link>
  );
}
```

优点：这也使代码更易读，更短！

<a name="1a8f40e8"></a>

## 3. 通过 useEffect 处理动作

React 引入的最好用，最贴心的一个 hook 是 useEffect。它可以处理与 prop 或 state 更改相关的动作。可就算它很好用，人们也不该到处滥用它。

想象一下有一个组件，其获取一个项目列表并将其渲染给 dom。另外，如果请求成功，我们将调用“onSuccess”函数，该函数作为一个 prop 传递给这个组件。

<a name="0682bbd6-1"></a>

### 这很危险❌

```jsx
function DataList({ onSuccess }) {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [data, setData] = useState(null);
  const fetchData = useCallback(() => {
    setLoading(true);
    callApi()
      .then((res) => setData(res))
      .catch((err) => setError(err))
      .finally(() => setLoading(false));
  }, []);
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  useEffect(() => {
    if (!loading && !error && data) {
      onSuccess();
    }
  }, [loading, error, data, onSuccess]);
  return <div>Data: {data}</div>;
}
```

<a name="2bed77ed-2"></a>

### 问题⚡

一共有两个 useEffect hooks，第一个在初始渲染时处理 api 调用，第二个会调用 onSuccess 函数，假设当状态没有加载、没有错误但有数据时调用肯定成功。这很有道理是吧？

对第一个调用来说这肯定是正确的，并且可能永远不会失败。但你也失去了动作和需要调用的函数之间的直接联系。同样也没有 100％的保证可以说这种情况仅在 fetch 动作成功后才会发生，而这正是我们开发人员不想看到的。

<a name="6d6afad5-2"></a>

### 解决方案✅

一个简单明了的解决方案是将“onSuccess”函数设置为调用成功的实际位置：

```jsx
function DataList({ onSuccess }) {
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [data, setData] = useState(null);
  const fetchData = useCallback(() => {
    setLoading(true);
    callApi()
      .then((fetchedData) => {
        setData(fetchedData);
        onSuccess();
      })
      .catch((err) => setError(err))
      .finally(() => setLoading(false));
  }, [onSuccess]);
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  return <div>{data}</div>;
}
```

现在一目了然了，在 api 调用成功的情况下才调用 onSuccess。

<a name="088924da"></a>

## 4. 单一责任组件

组合组件可能不是什么轻松的事情。什么时候将一个组件拆分为几个较小的组件？如何构造组件树？使用基于组件的框架时，每天都会遇到这些问题。设计组件时常见的一个错误是将两个用例合并到一个组件中。以一个 header 为例，其在移动设备上显示一个汉堡按钮，或在桌面屏幕上显示标签。（这里的条件通过神奇的 isMobile 函数处理，这里就不深入讲解了。）

<a name="0682bbd6-2"></a>

### 这很危险❌

```jsx
function Header(props) {
  return (
    <header>
      <HeaderInner menuItems={menuItems} />
    </header>
  );
}
function HeaderInner({ menuItems }) {
  return isMobile() ? 
        <BurgerButton menuItems={menuItems} /> : 
        <Tabs tabData={menuItems} />;
}
```

<a name="2bed77ed-3"></a>

### 问题⚡

使用这种方法时，HeaderInner 组件试图同时兼顾两件事情，而我们都知道一心最好不要二用。而且，这种组件很难在其他地方测试或重用。

<a name="6d6afad5-3"></a>

### 解决方案✅

将条件提高一级，这样就能更容易看清组件的本来用途，搞明白它们只应该负责一个任务，不管是 Header、Tab 或 BurgerButton 也好，总之不要一心多用。

```jsx
function Header(props) {
  return (
    <header>
        {
            isMobile() ? 
                <BurgerButton menuItems={menuItems} /> : 
                <Tabs tabData={menuItems} />
        }
        </header>
  );
}
```

<a name="3691cbd4"></a>

## 5. 单一责任的 useEffects

还记得以前，我们只能用 componentWillReceiveProps 或 componentDidUpdate 方法挂接到 React 组件的渲染过程吗？那是一段黑暗的回忆，也让我们意识到了 useEffect hook 的美妙之处，尤其是你可以随意使用这些 hooks。

但是有时因为粗心而让“useEffect”身兼数职，就会带回那些黑暗的回忆。例如，假设你有一个组件以某种方式从后端获取一些数据，并且还会根据当前位置显示面包屑。（再次使用 react-router 获取当前位置。）

<a name="0682bbd6-3"></a>

### 这很危险❌

```jsx
function Example(props) {
  const location = useLocation();
  const fetchData = useCallback(() => {
    /*  Calling the api */
  }, []);
  const updateBreadcrumbs = useCallback(() => {
    /* Updating the breadcrumbs*/
  }, []);
  useEffect(() => {
    fetchData();
    updateBreadcrumbs();
  }, [location.pathname, fetchData, updateBreadcrumbs]);
  return (
    <div>
      <BreadCrumbs />
    </div>
  );
}
```

<a name="2bed77ed-4"></a>

### 问题⚡

这里有两个用例，即“数据获取”和“显示面包屑”。两者都通过 useEffect hook 更新。当 fetchData 和 updateBreadcrumbs 函数或 location 更改时，都会运行这个 useEffect hook。现在的主要问题是，当位置更改时，我们还调用了 fetchData 函数。这可能是我们没有想到的副作用。

<a name="6d6afad5-4"></a>

### 解决方案✅

把效果拆分开来，确保它们只用于一种效果，意外的副作用也就消失了。

```jsx
function Example(props) {
  const location = useLocation();
  const updateBreadcrumbs = useCallback(() => {
    /* Updating the breadcrumbs*/
  }, []);
  useEffect(() => {
    updateBreadcrumbs();
  }, [location.pathname, updateBreadcrumbs]);
  const fetchData = useCallback(() => {
    /*  Calling the api */
  }, []);
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  return (
    <div>
      <BreadCrumbs />
    </div>
  );
}
```

**额外的收获是**，这些用例现在也在组件内按顺序排好了。
