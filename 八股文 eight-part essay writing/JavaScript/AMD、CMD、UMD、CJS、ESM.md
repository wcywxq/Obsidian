---
title: AMD、CMD、UMD、CJS、ESM
url: https://www.yuque.com/wcywxq/mxunh7/qthk0m
---

<a name="BuavH"></a>

## AMD

这种方案采用异步加载的方式来加载模块，模块的加载不加载后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。`require.js`实现了`AMD`规范

```javascript
// AMD 推崇依赖前置
define("alpha", ["require", "exports", "beta"], function (require, exports, beta) {
  exports.verb = function() {
    return beta.verb()
    // Or
    return require("beta").verb
  }
})
```

`require.js`是通过动态创建`script`脚本来异步引入模块，然后对每个脚本的`load`事件进行舰艇，如果每个脚本都加载完成了，再调用回调函数 <a name="IkzDg"></a>

### 特点

1. 所有代码都运行在模块作用域中，不会污染全局变量
2. 模块会被异步加载
3. 依赖模块加载完成后，会立即执行其回调函数(即`factory`函数)
4. 主模块会等所有的依赖模块加载完成后，再调用其对应的回调函数(依赖前置) <a name="z2JF5"></a>

## CMD

此方案和`AMD`方案都是为了解决异步模块加载的问题，`sea.js`实现了`CMD`规范，它和`require.js`的区别在于模块定义时对依赖的处理不同和对依赖模块的执行时机的处理不同

```javascript
// CMD 推崇依赖就近
define(function (require, exports, module) {
  var a = require("./a")
  a.doSomething()
  // ...
  var b = require("./b")
  b.doSomething()
})
```

<a name="xyIpv"></a>

### 特点

1. 所有代码都运行在模块作用域中，不会污染全局变量
2. 模块会被异步加载
3. 模块加载完成后，不会执行其回调函数，而是等到主函数运行且需要的执行依赖的时候才运行依赖函数(依赖后置、按需加载) <a name="RQywW"></a>

## UMD

`UMD`提供了支持多种风格的"通用"模式，在兼容`CommonJS`和`AMD`规范的同时，还兼容全局引用的方式

```javascript
(function(root, factory) {
  if (typeof define === "function" && define.amd) {
    // AMD
    define(["jquery", "lodash"], factory)
  } else if (typeof exports === "object") {
    // CommonJS
    module.exports = factory(require("jquery"), require("lodash"))
  } else {
    // 浏览器全局变量(root 和 window)
    root.returnExports = factory(root.jQuery, root._)
  }
})(this,  function ($, _) {
  // 方法
  function a() {} // 私有方法，因为它没有被返回
  function b() {} // 公共方法，因为被返回了
  function c() {} // 公共方法，因为被返回了
  // 暴露公共方法
  return {
    b: b,
    c: c
  }
})
```

<a name="eFAjU"></a>

### 原理

1. 判断是否支持`AMD`，若存在则使用`AMD`方式加载模块，否则继续步骤`2`
2. 判断是否支持`CommonJS`，若存在则使用`Node.js`的模块格式，否则继续步骤`3`
3. 将模块公开到全局(`window`或`global`) <a name="zKQIw"></a>

## CJS

`CJS`是`CommonJS`的简称，它是通过`require`来引入模块，通过`module.exports`定义模块的输出接口。`CommonJS`模块是运行时加载，模块输出是一个值的拷贝，`CommonJS`模块就是对象，即在输入时先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为 "运行时加载"。

```javascript
module.exports = class Square {
  constructor(width) {
    this.width = width
  } 
  area() {
    return this.width ** 2
  }
}
```

```javascript
const Square = require("./square.js")

const square = new Square(2)

module.exports = square.area()
```

<a name="DnURj"></a>

### 特点

1. 所有代码都运行在模块作用域中，不会污染全局变量
2. 模块按照在代码中的顺序，一次同步加载
3. 模块会在运行时加载并执行，执行得到对象`A`，后续通过`require`获取的都是对对象`A`值的拷贝(换句话说，模块可以多次加载，在第一次加载时执行并缓存其结果，后续加载会直接返回该结果)，要想模块再次运行，必须清楚缓存 <a name="vTHV0"></a>

## ESM

`ESM`是`ES6 Modules or JavaScript Modules`的简称，使用`import`和`export`的形式来导入导出模块。`ESM`是编译时输出接口，它不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。

```javascript
const obj = {
  sayHello() {
    console.log("Hello Ray")
  },
  myName: "Ray",
  obj: {
    myName: "Ray"
  },
  arr: [1, 2, 3]
}

export default obj
```

```html
<script type="module">
  import * as app from "./app.js"
  console.log(app.myName) // "Hello Ray"
  app.sayHi() // "Hello Ray"
  console.log(app.obj.myName) // "Ray"
  console.log(app.arr) // [1, 2, 3]
</script>
```

<a name="I2pJs"></a>

### 特点

1. 所有代码都运行在模块作用域中，不会污染全局变量
2. 在编译时输出模块
3. 输出的模块内容为只读，不可修改
4. 不会缓存模块结果，每次都会动态执行模块内容 <a name="q14BS"></a>

### import & export

- `import`语句会被提升
- `import`的变量都是只读的
- `import`是静态执行，因此无法使用表达式
- `import`语句支持`singleton`模式(如果多次重复执行同一句`import`语句，那么只会执行一次，而不会执行多次)
- `export`需要输出一个对象或变量声明语句
- `export default`相当于输出一个名叫`default`的变量/对象
- `export`与`import`连用

```javascript
export { foo, bar } from "my_module"

// 相当于
import { foo, bar } from "my_module"
export { foo, bar }
```
