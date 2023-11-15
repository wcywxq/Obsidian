---
title: props.children 和 React.Children 的区别
url: https://www.yuque.com/wcywxq/mxunh7/glhczw
---

在 `React` 中，当涉及组件嵌套，在父组件中使用 `props.children` 把所有子组件显示出来。如下：

```jsx
function ParentComponent(props){
	return (
		<div>
			{props.children}
		</div>
	)
}
```

如果想把父组件中的属性传给所有的子组件，需要使用 `React.Children` 方法。

比如，把几个 `Radio` 组合起来，合成一个 `RadioGroup`，这就要求所有的 `Radio` 具有同样的 `name` 属性值。可以这样：把 `Radio` 看做子组件，`RadioGroup` 看做父组件，`name` 的属性值在 `RadioGroup` 这个父组件中设置。

首先是子组件：

```jsx
//子组件
function RadioOption(props) {
  return (
    <label>
      <input type="radio" value={props.value} name={props.name} />
      {props.label}
    </label>
  )
}
```

然后是父组件，不仅需要把它所有的子组件显示出来，还需要为每个子组件赋上 `name` 属性和值：

```jsx
//父组件用,props是指父组件的props
function renderChildren(props) {
  //遍历所有子组件
  return React.Children.map(props.children, child => {
    if (child.type === RadioOption)
      return React.cloneElement(child, {
        //把父组件的props.name赋值给每个子组件
        name: props.name
      })
    else
      return child
  })
}
//父组件
function RadioGroup(props) {
  return (
    <div>
      {renderChildren(props)}
    </div>
  )
}
function App() {
  return (
    <RadioGroup name="hello">
      <RadioOption label="选项一" value="1" />
      <RadioOption label="选项二" value="2" />
      <RadioOption label="选项三" value="3" />
    </RadioGroup>
  )
}
export default App;
```

以上，`React.Children.map` 让我们对父组件的所有子组件又更灵活的控制。

<a name="k864S"></a>

### React.Children.map 和 js 的 map 有什么区别

- `JavaScript` 中的 `map` 不会对为 `null` 或者 `undefined` 的数据进行处理
- `React.Children.map` 中的 `map` 可以处理 `React.Children` 为 `null` 或者 `undefined` 的情况。如果 `children` 是数组则此方法返回一个数组，如果是 `null` 或 `undefined` 则返回 `null` 或 `undefined`
- `React.Children.forEach` 一样的原理
