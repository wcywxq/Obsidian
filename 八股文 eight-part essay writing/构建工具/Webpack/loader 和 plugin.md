---
title: loader 和 plugin
url: https://www.yuque.com/wcywxq/mxunh7/hl39v0
---

<a name="Mawih"></a>

## loader

- **概念**：`loader` 本质是一个函数，`loader runner` 会调用这个函数，该函数接收一个参数，参数是：**上一个 **`**loader**`** 产生的结果** 或者 **资源文件内容**，然后进行转换，返回转换后的结果。函数的 `this` 上下文将由 `webpack` 填充，并且 `loader runner` 具有 一些有用的方法，可以使 `loader` 改变为异步调用方式，或者获取 `query` 参数。本质上起到了非 `javaScript` 类型资源的转译预处理工作。
- **同步模式**：用于处理单个结果，如果有多个结果处理，则必须调用 `this.callback()`，同时要返回 `undefined`，以让 `webpack` 知道该 `loader` 返回的结果在 `this.callback` 中，而不是在 `return` 中。
- **异步模式**：必须调用 `this.async()`，来指示 `loader runner` 等待异步结果，它会返回 `this.callback()` 回调函数，随后 `loader` 必须返回 `undefined` 并且调用该回调函数。
- **执行顺序：**当链式调用多个 `loader` 的时候，它们会以相反的顺序执行。取决于数组写法格式，**从右向左 **或者 **从下向上 **执行。
- **配置**：`loader` 在 `module.rules` 中进行配置，作为模块的解析规则，是一个数组，每一项都是一个对象，其内部包含了 `test`(类型文件)、`loader`、`options`(参数)等属性。 <a name="dY2Is"></a>

### 常用 loader

1. 在`webpack v5`中，可以通过设置`type: "javascript/auto"`来使用旧的`assets loader`

2. `raw-loader`，用来导出资源的源代码。在`webpack v5`中，被`asset/source`模块替代

3. `file-loader`，用来发发送一个单独的文件并导出`URL`。在`webpack v5`中，被`asset/resource`模块替代

4. `url-loader`，用来导出一个资源的`data uri`，与`file-loader`类似，区别是：可以设置一个阀值，大于的交给`file-laoder`处理，小于的返回文件的`base64`编码。在`webpack v5`中，被`asset/inline`模块替代

5. `style-loader`：将`css`插入到`dom`中

6. `mini-css-extract-plugin`的`MiniCssExtractPlugin.loader`：替换`style-loader`，用于将`javaScript`中的`css`分离

7. `css-loader`：会对`@import 和 url()`进行处理，支持`css`模块化、压缩、文件导入等特性

8. `postcss-loader`：扩展`css`语法，使用下一代`css`，可以配合`autofixer`插件添加不同浏览器前缀

9. `sass-loader`：将`sass/scss`编译为`css`的`loader`，需要同时安装`sass`

10. `less-loader`：将`less`编译为`css`的`loader`，需要同时安装`less`

11. `stylus-loader`：将`stylus`编译为`css`的`loader`，需要同时安装`stylus`

12. `babel-loader`：用于`javaScript`高版本语法降级和语法转译工作

13. `ts-loader`：将`TypeScript`代码转译为`JavaScript`，需要同时安装`typescript`、`@babel/preset-typescript`

14. `remark-loader`：用于加载`markdown`

15. `vue-loader`：用来加载`vue`单文件组件

16. `eslint-loader`：通过`eslint`检查`javaScript`代码

17. `tslint-loader`：通过`tslint`检查`typeScript`代码

18. `cache-loader`webpack5不适用：可以在一些性能开销较大的`loader`之前添加，目的是将结果缓存到磁盘里，在`webpack v5`中提供了该功能

19. `svg-inline-loader`：将压缩后的 `svg` 文件内联为模块，注入到代码中

20. `source-map-loader`：从现有的源文件中提取`source maps`，可以加载额外的`source map`文件，允许`webpack`跨库维护`source map`数据，方便调试

21. `mocha-loader`：允许通过 `webpack` 加载和运行 `Mocha` 测试

22. `coverjs-loader`：用来计算测试的覆盖率
    :::warning
    注意：

23. `loader`支持链式调用，因此开发过程中需要严格遵守 **单一职责**，即每个 `loader` 只负责自己需要处理的内容

24. `loader` 运行在 `node.js` 中，因此需要导出一个函数，参数是加载文件原始 `utf-8` 格式编码的字符串，返回处理后的内容

25. 如果要返回多个结果，可以使用 `this.calback(err: Error | null, content: string | Buffer, sourceMap?: SourceMap, meta?: any) `

26. 在 `loader` 中，异步操作需要使用 `this.async`

27. `loader-utils` 提供了 `getOptions` 方法来获取参数信息(`webpack5` 内置为 `this.getOptions` 来获取)；`schema-utils` 提供了 `validate` 方法，用来做 `json` 模式验证
    ::: <a name="Cqzlp"></a>

### loader 介绍

<a name="UG63r"></a>

#### pitching loader

`loader` 总是从右到左地被调用，有些情况下，`loader` 只关心 `request` 后面的元数据(`metadata`)，并且忽略前一个 `loader` 的结果。在实际(从右到左)执行 `loader` 之前，会先从左到右调用 `loader` 上的 `pitch` 方法。

对于以下 `use` 配置：

```javascript
use: [
  'a-loader',
  'b-loader',
  'c-loader'
]
```

将会发生这些步骤：

```javascript
|- a-loader `pitch`
  |- b-loader `pitch`
    |- c-loader `pitch`
      |- requested module is picked up as a dependency
    |- c-loader normal execution
  |- b-loader normal execution
|- a-loader normal execution
```

那么，为什么 `loader` 可以利用 "跳跃(`pitching`)" 阶段呢？

首先，传递给 `pitch` 方法的 `data`，在执行阶段也会暴露在 `this.data` 之下，并且可以用于在循环时，捕获和共享前面的信息。

```javascript
// pitch 的捕获和共享
module.exports = function(content) {
  return someSyncOperation(content, this.data.value)
}

module.exports.pitch = function(remainingRequest, precedingRequest, data) {
  data.value = 42
}
```

其次，如果某个 `loader` 在 `pitch` 方法中给出一个结果，那么这个过程会回过身来，并跳过剩下的 `loader`，在上面的例子中，如果 `loader` 的 `pitch` 方法返回了一些东西：

```javascript
module.exports = function(content) {
  return someSyncOperation(content)
}

module.exports.pitch = function(remainingRequest, precedingRequest, data) {
  if (someCondition()) {
    return `module.exports = require(${JSON.stringify(`-!${remainingRequest}`)})`
  }
}

// 上面的步骤将被缩短为
|- a-loader `pitch`
  |- b-loader `pitch` returns a module
|- a-loader normal execution
```

<a name="lyy5O"></a>

#### 同步 loader

```javascript
// 单个结果
module.exports = function(content, sourceMap, meta) {
  return someSyncOperation(content)
}

// 多个 loader
module.exports = function(content, sourceMap, meta) {
  this.callback(null, someSyncOperation(content), sourceMap, meta)
  return // 当调用 callback() 时总是返回 undefined
}
```

<a name="VaBOQ"></a>

#### 异步 loader

```javascript
// 单个结果
module.exports = function(content, sourceMap, meta) {
  var callback = this.async()
  someAsyncOperation(content, function(err, result) {
    if (err) return callback(err)
    callback(null, result, sourceMap, meta)
  })
}

// 多个结果
module.exports = function(content, sourceMap, meta) {
  var callback = this.async()
  someAsyncOperation(content, function(err, result, sourceMap, meta) {
    if (err) return callback(err)
    callback(null, result, sourceMap, meta)
  })
} 
```

<a name="vHMjM"></a>

### 案例

<a name="ayqbZ"></a>

### mini 版 style-loader

```javascript
module.exports = function(source) {
  return `
    const styleTag = document.createElement('style');
    styleTag.innerHTML = ${source};
    document.head.appendChild(styleTag);
  `
}
```

<a name="IhnZi"></a>

### mini 版 css-loader

```javascript
module.exports = function(source) {
  return JSON.stringify(source);
}
```

<a name="Z1eP9"></a>

### mini 版 less-loader

```javascript
const less = require('less')
module.exports = function(source) {
  less.render(source, (err, input) => {
    let { css } = output;
    this.callback(err, css);
  })
}
```

<a name="g6BEx"></a>

### 编写一个 markdown-loader 实践

- `markdown`相关功能模块

```javascript
// md.js
const MarkdownIt = require("markdown-it")

// 内置插件列表
const MarkdownItPluginsList = [
  'markdown-it-sup',
  'markdown-it-sub',
  'markdown-it-mark',
  'markdown-it-kbd',
  'markdown-it-kbd',
  'markdown-it-underline',
  'markdown-it-deflist',
  'markdown-it-checkbox',
  'markdown-it-emoji',
  'markdown-it-task-checkbox',
  'markdown-it-container',
  'markdown-it-anchor',
  'markdown-it-table-of-contents',
  'markdown-it-footnote',
  'markdown-it-imsize',
  'markdown-it-implicit-figures',
  'markdown-it-attrs',
  'markdown-it-math',
  'markdown-it-plantuml',
  'markdown-it-ins',
  'markdown-it-abbr',
  'markdown-it-smartarrows'
]

// 默认 markdown 配置
const DEFAULT_MARKDOWN_OPTIONS = {
  html: false,
  xhtmlOut: false,
  breaks: false,
  langPrefix: '',
  linkify: false,
  typographer: false,
  quotes: '“”‘’',
  highlight: null,
  plugins: [],
  disable: [],
}

const createMd = (options = {}) => {
  const finalOptions = Object.assign({}, DEFAULT_MARKDOWN_OPTIONS, options)
  const { plugins, disable } = finalOptions
  
  delete finalOptions.plugins
  delete finalOptions.disable
  delete finalOptions.enable
  
  const md = new MarkdownIt(finalOptions)
  // 启动自定义插件
  plugins.forEach(plugName => {
    if (MarkdownItPluginsList.includes(plugName)) return
    MarkdownItPluginsList.push(plugName)
  })
  
  MarkdownItPluginList.forEach(plugName => {
    const plug = require(plugName)
    md.use(plug)
  })
  
  // 找不到不会报错
  md.disable(disable, true)
  return md
}
```

- `loader`功能模块

```javascript
const { getOptions } = require("loader-utils")
const validateOptions = require("schema-utils")
const createMd = require("./md")

// options 校验
const MarkdownLoaderOptionsSchema = {
  type: 'object',
  properties: {
    html: { type: 'boolean' },
    xhtmlOut: { type: 'boolean' },
    breaks: { type: 'boolean' },
    langPrefix: { type: 'string' },
    linkify: { type: 'boolean' },
    typographer: { type: 'boolean' },
    quotes: {
      anyOf: [{ type: 'string' }, { type: 'array', items: { type: 'string' } }],
    },
    highlight: { instanceof: 'Function' },
    plugins: { type: 'array', items: { type: 'string' } },
    disable: { type: 'array', items: { type: 'string' } },
  },
  additionalProperties: false
}

module.exports = function(source) {
  this.cacheable()
  const options = getOptions(this)
  validateOptions(MarkdownLoaderOptionsSchema, options, {
    name: 'MarkdownLoader',
    baseDataPath: 'options'
  })
  const md = createMd(options)
  return md.render(source)
}
```

- `webpack.config.js`

```javascript
const path = require("path")

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist")
  },
  module: {
    rules: [
      {
        test: /.md$/,
        use: [
          { 
            loader: "./markdown-webpack-loader",
            options: {
              html: true,
              plugins: [],
              disable: []
            }
          }
        ]
      }
    ]
  }
}
```

该`markdown-webpack-loader`的`options`除了支持所有的`markdown-it`初始化参数外，还额外支持`plugins`，添加额外本地插件，以及`disable`，禁用某些插件功能

***

<a name="bquBs"></a>

## plugin

- **原理**：`plugin` 就是插件，基于事件流框架 `tapable`，插件可以扩展 `webpack` 的功能，在 `webpack` 运行的生命周期中会广播出许多事件，`plugin` 可以监听这些事件，在合适的时机通过 `webpack` 提供的 API 改变输出结果
- **组成**：
  - 一个 `javaScript` 命名函数或 `javaScript` 类
  - 在插件函数的 `prototype`(原型) 上定义一个 `apply` 方法
  - 指定一个绑定到 `webpack` 自身的事件钩子(`hook`)
  - 处理 `webpack` 内部实例的特定数据
  - 功能完成后调用 `webpack` 提供的回调(`callback`)
- **概念**：`webpack plugin` 是一个具有 `apply` 属性的 `javaScript` 对象。`apply` 属性会被 `webpack compiler` 调用，并且 `compiler` 对象可能在整个编译生命周期内访问。`compilation` 模块会被 `compiler` 用来创建新的 `compilation` 对象(或新的 `build` 对象)。`compilation` 实例能够访问所有的模块和它们的依赖(大部分是循环依赖)。它会对应用程序依赖图中的所有模块，进行字面上的编译(`literal compilation`)。在编译阶段，模块会被加、封存、优化、分块、哈希和重新创建(`load`、`seal`、`optimize`、`chunk`、`hash`、`restore`)
  - 全局唯一`compiler` 实例：包含了 `webpack` 环境的所有配置信息，包含 `options`、`loaders`、`plugins`，在 `webpack` 启动时实例化，也可以理解为 `webpack` 的实例。`compiler` 模块是 `webpack` 的支柱引擎，它通过 `cli` 或 `node api` 传递所有选项，创建出一个 `compilation` 实例。它扩展(`extend`)自 `tapable` 类，以便于注册和调用插件。
  - `compilation` 钩子：包含当前模块资源，编译生成资源，`webpack` 在开发模式下运行的时候，每当监测到一个文件变化，就会创建一次新的 `compilation`。
- **执行顺序**：按照编译顺序执行
- **配置**：`plugin` 在 `plugins` 中单独配置，类型为数组，每一项都是一个 `plugin` 实例，参数都通过构造函数传入

***

<a name="OoObb"></a>

### 常用 plugin

- `webpack-bar`：自定义构建进度条
- `html-webpack-plugin`：可根据模版自动生成 `html` 代码，并自动引用 `css` 和 `js` 文件
- `mini-css-extract-plugin`：常用于 `css`包分离
- 压缩`css`代码、重复`css`代码去重复：
  - `optimize-css-assets-webpack-plugin`webpack5不适用
  - `css-minimizer-webpack-plugin`webpack5适用
- 压缩`js`
  - `uglifyjs-webpack-plugin`webpack5不适用，同时使用`thread-loader`来开启多进程并行压缩代码webpack5不适用
  - `terser-webpack-plugin`webpack5适用`parallel: true` 使用多进程并行压缩代码
- 在`webpack v4`开始，使用`optimization.splitChunks`替代 `webpack.optimize.CommonsChunkPlugin`，用于代码分包 `chunk`，公共资源拆分
- `clean-webpack-plugin`：使用此插件后，`webpack`打包后的`dist`目录中的所有文件将被删除一次
- `webpack-bundle-analyzer`：可视化分析包体积大小
- `webpack-dev-server`：使用 `webpack` 和提供实时重载的开发服务器
- `webpack-merge`：提取公共配置，减少重复配置代码
- `webpack-dashboard`：`webpack` 服务器仪表盘，更友好的展示相关的打包信息
- `compression-webpack-plugin`：开启 `gzip` 压缩
- `eslint-webpack-plugin`：用于查找和修复`javaScript`代码中的问题
- `speed-measure-webpack-plugin`：简称为`SMP`，分析出`webpack`打包过程中`loader`和`plugin`的耗时，有助于找到构建过程中的性能瓶颈
- `webpack`内置插件
  - `webpack.DefinePlugin`：编译时配置全局变量，对开发模式和生产模式的构建允许不同的行为非常有用(可配合 `.env` 使用)
  - `webpack.HotModuleReplacementPlugin`：热更新，即`HMR`。在配置了`devServer.hot: true`自动添加该插件
  - `webpack.DllPlugin`：配合`webpack.DllReferencePlugin`使用索引链接，配合`mailfest.json`来抽离第三方模块，常用于对静态不变的第三方库进行处理，从而大幅度提升构建速度。
  - `webpack.SourceMapDevToolPlugin`：实现了对 `sourceMap` 生成内容进行更细粒度的控制，也可以根据`devtool`配置选项的某些设置来自动启用 <a name="fmL0C"></a>

### plugin 中的常用对象及对应 hook

在 `webpack` 中哪些对象可以注册 `hook`

- `Compiler Hooks`
  - [environment](https://webpack.js.org/api/compiler-hooks/#environment)
  - [afterEnvironment](https://webpack.js.org/api/compiler-hooks/#afterenvironment)
  - [entryOption](https://webpack.js.org/api/compiler-hooks/#entryoption)
  - [afterPlugins](https://webpack.js.org/api/compiler-hooks/#afterplugins)
  - [afterResolvers](https://webpack.js.org/api/compiler-hooks/#afterresolvers)
  - [initialize](https://webpack.js.org/api/compiler-hooks/#initialize)
  - [beforeRun](https://webpack.js.org/api/compiler-hooks/#beforerun)
  - [run](https://webpack.js.org/api/compiler-hooks/#run)
  - [watchRun](https://webpack.js.org/api/compiler-hooks/#watchrun)
  - [normalModuleFactory](https://webpack.js.org/api/compiler-hooks/#normalmodulefactory)
  - [contextModuleFactory](https://webpack.js.org/api/compiler-hooks/#contextmodulefactory)
  - [beforeCompile](https://webpack.js.org/api/compiler-hooks/#beforecompile)
  - [compile](https://webpack.js.org/api/compiler-hooks/#compile)
  - [thisCompilation](https://webpack.js.org/api/compiler-hooks/#thiscompilation)
  - [compilation](https://webpack.js.org/api/compiler-hooks/#compilation)
  - [make](https://webpack.js.org/api/compiler-hooks/#make)
  - [afterCompile](https://webpack.js.org/api/compiler-hooks/#aftercompile)
  - [shouldEmit](https://webpack.js.org/api/compiler-hooks/#shouldemit)
  - [emit](https://webpack.js.org/api/compiler-hooks/#emit)
  - [afterEmit](https://webpack.js.org/api/compiler-hooks/#afteremit)
  - [assetEmitted](https://webpack.js.org/api/compiler-hooks/#assetemitted)
  - [done](https://webpack.js.org/api/compiler-hooks/#done)
  - [additionalPass](https://webpack.js.org/api/compiler-hooks/#additionalpass)
  - [failed](https://webpack.js.org/api/compiler-hooks/#failed)
  - [invalid](https://webpack.js.org/api/compiler-hooks/#invalid)
  - [watchClose](https://webpack.js.org/api/compiler-hooks/#watchclose)
  - [infrastructureLog](https://webpack.js.org/api/compiler-hooks/#infrastructurelog)
  - [log](https://webpack.js.org/api/compiler-hooks/#log)
- `Compilation Hook`
  - [buildModule](https://webpack.js.org/api/compilation-hooks/#buildmodule)
  - [rebuildModule](https://webpack.js.org/api/compilation-hooks/#rebuildmodule)
  - [failedModule](https://webpack.js.org/api/compilation-hooks/#failedmodule)
  - [succeedModule](https://webpack.js.org/api/compilation-hooks/#succeedmodule)
  - [finishModules](https://webpack.js.org/api/compilation-hooks/#finishmodules)
  - [finishRebuildingModule](https://webpack.js.org/api/compilation-hooks/#finishrebuildingmodule)
  - [seal](https://webpack.js.org/api/compilation-hooks/#seal)
  - [unseal](https://webpack.js.org/api/compilation-hooks/#unseal)
  - [optimizeDependencies](https://webpack.js.org/api/compilation-hooks/#optimizedependencies)
  - [afterOptimizeDependencies](https://webpack.js.org/api/compilation-hooks/#afteroptimizedependencies)
  - [optimize](https://webpack.js.org/api/compilation-hooks/#optimize)
  - [optimizeModules](https://webpack.js.org/api/compilation-hooks/#optimizemodules)
  - [afterOptimizeModules](https://webpack.js.org/api/compilation-hooks/#afteroptimizemodules)
  - [optimizeChunks](https://webpack.js.org/api/compilation-hooks/#optimizechunks)
  - [afterOptimizeChunks](https://webpack.js.org/api/compilation-hooks/#afteroptimizechunks)
  - [optimizeTree](https://webpack.js.org/api/compilation-hooks/#optimizetree)
  - [afterOptimizeTree](https://webpack.js.org/api/compilation-hooks/#afteroptimizetree)
  - [optimizeChunkModules](https://webpack.js.org/api/compilation-hooks/#optimizechunkmodules)
  - [afterOptimizeChunkModules](https://webpack.js.org/api/compilation-hooks/#afteroptimizechunkmodules)
  - [shouldRecord](https://webpack.js.org/api/compilation-hooks/#shouldrecord)
  - [reviveModules](https://webpack.js.org/api/compilation-hooks/#revivemodules)
  - [beforeModuleIds](https://webpack.js.org/api/compilation-hooks/#beforemoduleids)
  - [moduleIds](https://webpack.js.org/api/compilation-hooks/#moduleids)
  - [optimizeModuleIds](https://webpack.js.org/api/compilation-hooks/#optimizemoduleids)
  - [afterOptimizeModuleIds](https://webpack.js.org/api/compilation-hooks/#afteroptimizemoduleids)
  - [reviveChunks](https://webpack.js.org/api/compilation-hooks/#revivechunks)
  - [beforeChunkIds](https://webpack.js.org/api/compilation-hooks/#beforechunkids)
  - [chunkIds](https://webpack.js.org/api/compilation-hooks/#chunkids)
  - [optimizeChunkIds](https://webpack.js.org/api/compilation-hooks/#optimizechunkids)
  - [afterOptimizeChunkIds](https://webpack.js.org/api/compilation-hooks/#afteroptimizechunkids)
  - [recordModules](https://webpack.js.org/api/compilation-hooks/#recordmodules)
  - [recordChunks](https://webpack.js.org/api/compilation-hooks/#recordchunks)
  - [beforeModuleHash](https://webpack.js.org/api/compilation-hooks/#beforemodulehash)
  - [afterModuleHash](https://webpack.js.org/api/compilation-hooks/#aftermodulehash)
  - [beforeHash](https://webpack.js.org/api/compilation-hooks/#beforehash)
  - [afterHash](https://webpack.js.org/api/compilation-hooks/#afterhash)
  - [recordHash](https://webpack.js.org/api/compilation-hooks/#recordhash)
  - [record](https://webpack.js.org/api/compilation-hooks/#record)
  - [beforeModuleAssets](https://webpack.js.org/api/compilation-hooks/#beforemoduleassets)
  - [additionalChunkAssets](https://webpack.js.org/api/compilation-hooks/#additionalchunkassets)
  - [shouldGenerateChunkAssets](https://webpack.js.org/api/compilation-hooks/#shouldgeneratechunkassets)
  - [beforeChunkAssets](https://webpack.js.org/api/compilation-hooks/#beforechunkassets)
  - [additionalAssets](https://webpack.js.org/api/compilation-hooks/#additionalassets)
  - [optimizeChunkAssets](https://webpack.js.org/api/compilation-hooks/#optimizechunkassets)
  - [afterOptimizeChunkAssets](https://webpack.js.org/api/compilation-hooks/#afteroptimizechunkassets)
  - [optimizeAssets](https://webpack.js.org/api/compilation-hooks/#optimizeassets)
  - [afterOptimizeAssets](https://webpack.js.org/api/compilation-hooks/#afteroptimizeassets)
  - [processAssets](https://webpack.js.org/api/compilation-hooks/#processassets)
  - [afterProcessAssets](https://webpack.js.org/api/compilation-hooks/#afterprocessassets)
  - [needAdditionalSeal](https://webpack.js.org/api/compilation-hooks/#needadditionalseal)
  - [afterSeal](https://webpack.js.org/api/compilation-hooks/#afterseal)
  - [chunkHash](https://webpack.js.org/api/compilation-hooks/#chunkhash)
  - [moduleAsset](https://webpack.js.org/api/compilation-hooks/#moduleasset)
  - [chunkAsset](https://webpack.js.org/api/compilation-hooks/#chunkasset)
  - [assetPath](https://webpack.js.org/api/compilation-hooks/#assetpath)
  - [needAdditionalPass](https://webpack.js.org/api/compilation-hooks/#needadditionalpass)
  - [childCompiler](https://webpack.js.org/api/compilation-hooks/#childcompiler)
  - [normalModuleLoader](https://webpack.js.org/api/compilation-hooks/#normalmoduleloader)
- `ContextModuleFactory Hooks`
  - [beforeResolve](https://webpack.js.org/api/contextmodulefactory-hooks/#beforeresolve)
  - [afterResolve](https://webpack.js.org/api/contextmodulefactory-hooks/#afterresolve)
  - [contextModuleFiles](https://webpack.js.org/api/contextmodulefactory-hooks/#contextmodulefiles)
  - [alternativeRequests](https://webpack.js.org/api/contextmodulefactory-hooks/#alternativerequests)
- `JavascriptParser Hooks`
  - [evaluateTypeof](https://webpack.js.org/api/parser/#evaluatetypeof)
  - [evaluate](https://webpack.js.org/api/parser/#evaluate)
  - [evaluateIdentifier](https://webpack.js.org/api/parser/#evaluateidentifier)
  - [evaluateDefinedIdentifier](https://webpack.js.org/api/parser/#evaluatedefinedidentifier)
  - [evaluateCallExpressionMember](https://webpack.js.org/api/parser/#evaluatecallexpressionmember)
  - [statement](https://webpack.js.org/api/parser/#statement)
  - [statementIf](https://webpack.js.org/api/parser/#statementif)
  - [label](https://webpack.js.org/api/parser/#label)
  - [import](https://webpack.js.org/api/parser/#import)
  - [importSpecifier](https://webpack.js.org/api/parser/#importspecifier)
  - [export](https://webpack.js.org/api/parser/#export)
  - [exportImport](https://webpack.js.org/api/parser/#exportimport)
  - [exportDeclaration](https://webpack.js.org/api/parser/#exportdeclaration)
  - [exportExpression](https://webpack.js.org/api/parser/#exportexpression)
  - [exportSpecifier](https://webpack.js.org/api/parser/#exportspecifier)
  - [exportImportSpecifier](https://webpack.js.org/api/parser/#exportimportspecifier)
  - [varDeclaration](https://webpack.js.org/api/parser/#vardeclaration)
  - [varDeclarationLet](https://webpack.js.org/api/parser/#vardeclarationlet)
  - [varDeclarationConst](https://webpack.js.org/api/parser/#vardeclarationconst)
  - [varDeclarationVar](https://webpack.js.org/api/parser/#vardeclarationvar)
  - [canRename](https://webpack.js.org/api/parser/#canrename)
  - [rename](https://webpack.js.org/api/parser/#rename)
  - [assigned](https://webpack.js.org/api/parser/#assigned)
  - [assign](https://webpack.js.org/api/parser/#assign)
  - [typeof](https://webpack.js.org/api/parser/#typeof)
  - [call](https://webpack.js.org/api/parser/#call)
  - [callMemberChain](https://webpack.js.org/api/parser/#callmemberchain)
  - [new](https://webpack.js.org/api/parser/#new)
  - [expression](https://webpack.js.org/api/parser/#expression)
  - [expressionConditionalOperator](https://webpack.js.org/api/parser/#expressionconditionaloperator)
  - [program](https://webpack.js.org/api/parser/#program)
- `NormalModuleFactory Hooks`
  - [beforeResolve](https://webpack.js.org/api/normalmodulefactory-hooks/#beforeresolve)
  - [factorize](https://webpack.js.org/api/normalmodulefactory-hooks/#factorize)
  - [resolve](https://webpack.js.org/api/normalmodulefactory-hooks/#resolve)
  - [resolveForScheme](https://webpack.js.org/api/normalmodulefactory-hooks/#resolveforscheme)
  - [afterResolve](https://webpack.js.org/api/normalmodulefactory-hooks/#afterresolve)
  - [createModule](https://webpack.js.org/api/normalmodulefactory-hooks/#createmodule)
  - [module](https://webpack.js.org/api/normalmodulefactory-hooks/#module)
  - [createParser](https://webpack.js.org/api/normalmodulefactory-hooks/#createparser)
  - [parser](https://webpack.js.org/api/normalmodulefactory-hooks/#parser)
  - [createGenerator](https://webpack.js.org/api/normalmodulefactory-hooks/#creategenerator)
  - [generator](https://webpack.js.org/api/normalmodulefactory-hooks/#generator) <a name="k6FRY"></a>

### 插件的不同类型

`webpack` 插件可以按照它所注册的事件分成不同的类型。每一个事件钩子都预先定义为同步、异步、瀑布或并行钩子，钩子在内部用 `call/callAsync` 方法调用。支持的钩子清单或可被绑定的钩子清单，通常在 `this.hooks` 属性指定。

- 例如：

```javascript
this.hooks = {
  shouldEmit: new SyncBailHook(['compilation']),
};
```

表示唯一支持的钩子是 `shouldEmit` ，这是一个 `SyncBailHook` 类型的钩子，传入插件的唯一参数是 `compilation` 。
支持的各类型钩子： <a name="QXII3"></a>

### 同步 hooks

- `SyncHook`
  - 通过 `new SyncHook([params])` 定义。
  - 用 `tap` 方法绑定。
  - 用 `call(...params)` 方法调用。

- `Bail`(保险) `Hooks`
  - 通过 `new SyncBailHook([params])` 定义。
  - 用 `tap` 方法绑定。
  - 用 `call(...params)` 方法调用。
    :::info
    `Bail` 类型的钩子，其插件回调函数是串行调用，任意一个插件回调函数返回非 `undefined` 值，则停止执行插件，该值作为钩子的返回值。`optimizeChunks`，`optimizeChunkModules` 等很有用的事件都是 `SyncBailHooks` 。
    :::

- Waterfall(瀑布) Hooks
  - 通过 `new SyncWaterfallHook([params])` 定义。
  - 用 `tap` 方法绑定。
  - 用 `call(...params)` 方法调用。
    :::info
    该类型的插件是一个接一个串行执行，前一个的返回值作为后一个的入参。插件需要考虑执行的顺序，因为后一个插件必须接受前一个插件执行的结果作为入参。第一个插件的参数为 `init` 。 所以 `waterfall` 钩子至少要制定一个参数。这种模式用于和 `ModuleTemplate`，`ChunkTemplate` 等 `webpack` 模板相关的 `Tapable` 实例。
    ::: <a name="hLMyk"></a>

### 异步 hooks

- `Async Series`(异步串联) `Hook`
  - 通过 `new AsyncSeriesHook([params])`定义。
  - 用 `tap/tapAsync/tapPromise`方法绑定。
  - 用 `callAsync(...params)`方法调用。

- `Async waterfall`(异步瀑布) 插件会用 `waterfall` 方式异步应用
  - 通过 `new AsyncWaterfallHook([params])`定义。
  - 用 `tap/tapAsync/tapPromise`方法绑定。
  - 用 `callAsync(...params)`方法调用。
    :::info
    插件处理函数的参数为当前值，以及一个签名为 `(err: Error, nextValue: any) -> void`的 `callback`函数。调用时 `nextValue`是下一个处理函数的当前值。第一个处理函数的当前是只 `init`。当所有 `handler`都执行后，`callback`执行，参数为最后一个值。任一个 `handler`传入 `err`值，停止调用 `handler`且 `callback`被调用。 这种模式在 `before-resolve`和 `after-resolve`事件中常见。
    :::

- Async Series Bail
  - 通过 `new AsyncSeriesBailHook([params]) `定义。
  - 用 `tap/tapAsync/tapPromise` 方法绑定。
  - 用 `callAsync(...params)` 方法调用。

- Async Parallel
  - 通过 `new AsyncParallelHook([params])` 定义。
  - 用 `tap/tapAsync/tapPromise` 方法绑定。
  - 用 `callAsync(...params)` 方法调用。 <a name="jW5lj"></a>

### 案例

```javascript
// 一个 JavaScript 类
class MyExampleWebpackPlugin {
  // 在插件函数的 prototype 上定义一个 `apply` 方法，以 compiler 为参数
  apply(compiler) {
    // 指定一个挂载到 webpack 自身的事件钩子
    compiler.hooks.emit.tapAsync('MyExampleWebpackPLugin', (compilation, callback) => {
      console.log('这是一个 example plugin')
      console.log('资源单次构建的 compilation 对象',  compilation)
      // 用 webpack 提供的插件 api 处理构建过程
      compilation.addModule(/* ... */)
      callback()
    })
  }
}
```

- 案例一

```javascript
const { validate } = require("schema-utils")

// 选项对象的 schema
const schema = {
    type: "object",
    properties: {
        test: {
            type: "string"
        }
    }
}

class HelloWorldPlugin {
    constructor(options = {}) {
        validate(schema, options, {
            name: "hello world plugin",
            baseDataPath: "options"
        })
    }

    apply(compiler) {
        compiler.hooks.done.tap("hello world plugin", stats => {
            console.log("hello, world!")
        })
    }
}

module.exports = HelloWorldPlugin
```

- 案例二

```javascript
class HelloCompilationPlugin {
    apply(compiler) {
        // 指定一个挂载到 compilation 的钩子，回调函数的参数为 compilation
        compiler.hooks.compilation.tap('HelloCompilationPlugin', compilation => {
            // 现在可以通过 compilation 对象绑定各种钩子
            compilation.hooks.optimize.tap('HelloCompilationPlugin', () => {
                console.log('资源已优化完毕')
            })
        })
    }
}

module.exports = HelloCompilationPlugin
```

- 案例三(生成一个叫做 `assets.md` 的新文件；文件内容是所有构建生成的文件的列表)

```javascript
class FileListPlugin {
  static defaultOptions = {
    outputFile: 'assets.md',
  };

  // 需要传入自定义插件构造函数的任意选项
  //（这是自定义插件的公开API）
  constructor(options = {}) {
    // 在应用默认选项前，先应用用户指定选项
    // 合并后的选项暴露给插件方法
    // 记得在这里校验所有选项
    this.options = { ...FileListPlugin.defaultOptions, ...options };
  }

  apply(compiler) {
    const pluginName = FileListPlugin.name;

    // webpack 模块实例，可以通过 compiler 对象访问，
    // 这样确保使用的是模块的正确版本
    // （不要直接 require/import webpack）
    const { webpack } = compiler;

    // Compilation 对象提供了对一些有用常量的访问。
    const { Compilation } = webpack;

    // RawSource 是其中一种 “源码”("sources") 类型，
    // 用来在 compilation 中表示资源的源码
    const { RawSource } = webpack.sources;

    // 绑定到 “thisCompilation” 钩子，
    // 以便进一步绑定到 compilation 过程更早期的阶段
    compiler.hooks.thisCompilation.tap(pluginName, (compilation) => {
      // 绑定到资源处理流水线(assets processing pipeline)
      compilation.hooks.processAssets.tap(
        {
          name: pluginName,

          // 用某个靠后的资源处理阶段，
          // 确保所有资源已被插件添加到 compilation
          stage: Compilation.PROCESS_ASSETS_STAGE_SUMMARIZE,
        },
        (assets) => {
          // "assets" 是一个包含 compilation 中所有资源(assets)的对象。
          // 该对象的键是资源的路径，
          // 值是文件的源码

          // 遍历所有资源，
          // 生成 Markdown 文件的内容
          const content =
            '# In this build:\n\n' +
            Object.keys(assets)
              .map((filename) => `- ${filename}`)
              .join('\n');

          // 向 compilation 添加新的资源，
          // 这样 webpack 就会自动生成并输出到 output 目录
          compilation.emitAsset(
            this.options.outputFile,
            new RawSource(content)
          );
        }
      );
    });
  }
}

module.exports = { FileListPlugin };
```

```javascript
const { FileListPlugin } = require('./file-list-plugin.js');

// 在 webpack 配置中使用自定义的插件：
module.exports = {
  // …

  plugins: [
    // 添加插件，使用默认选项
    new FileListPlugin(),

    // 或者:

    // 使用任意支持的选项
    new FileListPlugin({
      outputFile: 'my-assets.md',
    }),
  ],
};
```

这样就生成了一个指定名称的 `markdown` 文件：

```javascript
# In this build:

- main.css
- main.js
- index.html
```

<a name="NLgvK"></a>

#### 异步编译插件

有些插件钩子是异步的。我们可以像同步方式一样用 `tap` 方法来绑定，也可以用 `tapAsync` 或 `tapPromise` 这两个异步方法来绑定。 <a name="PbqEc"></a>

##### tapAsync

当我们用 `tapAsync` 方法来绑定插件时，必须调用函数的最后一个参数 `callback` 指定的回调函数。

```javascript
class HelloAsyncPlugin {
  apply(compiler) {
    compiler.hooks.emit.tapAsync('HelloAsyncPlugin', (compilation, callback) => {
      setTimeout(() => {
        console.log('异步任务完成...')
        callback()
      }, 1000)
    })
  }
}
module.exports = HelloAsyncPlugin
```

<a name="i7T1y"></a>

##### tapPromise

当我们用 `tapPromise` 方法来绑定插件时，必须返回一个 `promise`，异步任务完成后 `resolve`

```javascript
class HelloAsyncPlugin {
  apply(compiler) {
    compiler.hooks.emit.tapPromise('HelloAsyncPlugin', compilation => {
      // 返回一个 promise，异步任务完成后 resolve
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          console.log('异步任务完成...')
          resolve()
        }, 1000)
      })
    })
  }
}
module.exports = HelloAsyncPlugin
```

<a name="ByCN1"></a>

### 编写一个支持 Gzip 和 Brotli 压缩插件实践

压缩核心功能依赖`Node zlib`模块处理编译后的资源即可

- `plugin`功能模块

```javascript
const zlib = require('zlib');

const DEFAULT_COMPRESS_OPTIONS = {
  deleteOriginalAssets: false, // 是否删除压缩前的文件，看情况配置
  algorithm: 'gzip', // 压缩算法，默认就是gzip
  test: /./, // 所有文件都压缩
  threshold: 1000, // 只处理比这个值大的资源。按字节计算
  minRatio: 0.8, // 只有压缩率比这个值小的资源才会被处理
};

class CompressPlugin {
  constructor(options = {}) {
    this.options = {
      ...DEFAULT_COMPRESS_OPTIONS,
      ...options,
    };
  }

  compressSync(buf) {
    const { algorithm } = this.options;
    switch (algorithm.toLowerCase()) {
      case 'brotli':
        return zlib.brotliCompressSync(buf);
      case 'gzip':
        return zlib.gzipSync(buf);
      default:
        return buf;
    }
  }

  getComporessedName(filename) {
    const { algorithm } = this.options;
    switch (algorithm.toLowerCase()) {
      case 'brotli':
        return `${filename}.br`;
      case 'gzip':
        return `${filename}.gz`;
      default:
        return filename;
    }
  }

  apply(compiler) {
    const { deleteOriginalAssets, test, threshold, minRatio } = this.options;

    compiler.hooks.emit.tap('CompressPlugin', compilation => {
      const assets = compilation.assets;
      Object.keys(assets).forEach(filename => {
        // 按文件名过滤，默认不过滤
        if (!test.test(filename)) return;
        const asset = assets[filename];

        // 只处理比这个值大的资源。按字节计算
        if (asset.size() <= threshold) return;

        // 不支持的压缩方式不处理
        const compressedAssetName = this.getComporessedName(filename);
        if (filename === compressedAssetName) return;
        const compressedAsset = this.compressSync(asset.source());

        // 返回文件大小， 按字节算
        const compressedAssetSize = Buffer.isBuffer(compressedAsset)
          ? Buffer.byteLength(compressedAsset, 'utf8')
          : compressedAsset.length;

        // 只有压缩率比这个值小的资源才会被处理
        if (compressedAssetSize / asset.size() >= minRatio) return;

        assets[compressedAssetName] = {
          // 返回文件内容
          source: () =>
            // fileContent 既可以是代表文本文件的字符串，也可以是代表二进制文件的 Buffer
            compressedAsset,
          // 返回文件大小， 按字节算
          size: () => compressedAssetSize,
        };
        // 是否删除压缩前的文件，看情况配置
        if (deleteOriginalAssets) {
          delete assets[filename];
        }
      });
    });
  }
}

module.exports = CompressPlugin;
```

- `webpack.config.js`

```javascript
const path = require("path")
const CompressPlugin = require("./compress-webpack-plugin")

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist")
  },
  plugins: [
    new CompressPlugin({
      deleteOriginalAssets: true, // 是否删除压缩前的文件，看情况配置，默认为 false
      algorithm: "brotli" // 压缩算法，默认是 gzip
    })
  ]
}
```
