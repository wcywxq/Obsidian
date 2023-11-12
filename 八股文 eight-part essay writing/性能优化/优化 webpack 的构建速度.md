---
title: 优化 webpack 的构建速度
url: https://www.yuque.com/wcywxq/mxunh7/aft935
---

1. **使用高版本的 **`**webpack**`** 和 **`**node.js**`
   - `webpack v4` 优点：
     - v8 带来的优化(`for...of` 代替了 `forEach`、`Map` 和 `Set` 代替了 `Object`、`includes` 代替了 `indexOf`)
     - 默认使用更快的 `md4 hash`算法，去替代`md5`
     - `webpack ast`可以直接从`loader`传递给 `ast`，减少解析时间
     - 使用字符串方法替代正则表达式
   - `webpack v5` 优点：

[webpack 5 和 webpack 4 的区别](https://front-tech.yuque.com/go/doc/71613217?view=doc_embed)

2. **多进程、多实例构建**
   - `thread-loader`官方推荐
   - `parallel-webpack`
   - `happypack`
3. `**JavaScript**`**代码多进程、多实例并行压缩**
   - `terser-webpack-plugin`：设置`parallel: true`
4. **图片压缩**
   - `image-minimizer-webpack-plugin`官方推荐
   - `image-webpack-loader`
5. **缩小打包作用域**
   - `exclude/include`：确定`loader`的作用范围
   - 优化`resolve.module`配置：指明第三方模块的绝对路径，以减少不必要的查找
   - 优化`resolve.mainFields`配置
   - 优化`resolve.extensions`配置
   - 对不需要解析的第三方库进行忽略
   - 合理利用`alias`
6. **提取页面公共资源**
   - `externals`：
     - 使用`externals`配置来提取常用库，将基础库通过`cdn`引入，不打入`bundle`中
   - `dll`webpack5不推荐：
     - 在`webpack v4`及以前，可以通过使用`webpack.DllPlugin`和`webpack.DllReferencePlugin`来拆分`bundles`，对模块进行预编译，从而大幅提升构建速度。(`dll: 动态链接库`)
     - 主要是利用，利用索引链接对`mainfest.json`的引用，通过`webpack.DllPlugin`来对引用但绝对不会修改的`npm`包进行预编译，再通过`webpack.DllReferencePlugin`将预编译的模块加载进来，避免反复编译浪费时间
     - 随着`webpack v4`版本自身编译加快，`dll`缓存作为优化方案便显得不再那么重要。在`webpack v5`中，提供的持久化缓存是比`dll`更优秀的解决方案。
7. **充分利用缓存提高二次构建速度**
   - `babel-loader`可以通过设置`cacheDirectory: true`选项来开启缓存配置，将`babel-loader`提速至少两倍，同时会将转译结果缓存到文件系统中。
   - `terser-webpack-plugin`：通过配置`minimizer`自定义`terser-webpack-plugin`配置
     - 在 `webpack4` 中，通过设置`cache: true`或者 `cache: 'path/to/cache'` 即指定缓存目录来开启缓存；
     - 在 `webpack5`中，通过设置`terserOptions.nameCache: JSON.parse(fs.readFileSync(/tmp/cache.json, "utf8"))`来指定缓存文件的映射
   - 在`webpack5`中，通过配置持久化缓存`cache.type: filesystem`和配置缓存路径`cache.cacheDirectory: path.resolve(__dirname, '.temp_cache')`来缓存生成的`webpack`模块和`chunk`，以改善构建速度。(当构建突然中断，二次构建时，将直接从缓存中拉取代码，整体提速 `90%`左右)
8. **多个入口时进行 **`**common chunk**`** 设置**
   - 在`webpack v4`之前，曾使用`common-webpack-plugin`来避免`webpack`图谱中父子关系关联之间的重复依赖问题
   - 在`webpack v4`开始，通过使用`optimization.splitChunks`来配置分包策略，进而分离公共资源，避免单个包体积过大等问题
9. **代码 **`**tree-shaking**`
   - 在打包过程中检测工程中没有使用过的模块并进行标记，在资源压缩时将他们从最终的 `bunlde`中去掉，在开发过程中尽可能的使用`es-module`的模块，提高`tree-shaking`效率
   - 禁用`babel-loader`的模块依赖解析功能，否则`webpack`接收到的就是转换过的`commonjs`形式的模块，无法进行`tree-shaking`
   - 使用`mini-css-extract-plugin`，默认情况下会生成使用`ES`模块语法的`JS`语法，这样对`tree-shaking`是有益的
10. **作用域提升 **`**scope-hoisting**`

- `scope-hoisting` 会分析出模块之间的依赖关系，尽可能的使用闭包，即把打包出来的模块合并到一个函数中，这样会减少冗余并加速
- 开启方式：`optimazation.concatenateModules: true`

11. **动态 **`**polyfill**`** 服务**

- 利用 `cdn`动态导入 `polyfill`

12. **关闭 **`**symlinks**`

- 若项目不使用 `npm link` 或者 `yarn link`，那么可以设置 `resolve.syslinks: false`，减少解析工作量

13. **不同的**`**devtool**`**设置，会导致性能差异**

- 在大多数情况下，最佳选择是`eval-cheap-module-source-map`
  - 设置`devtool: "eval-cheap-module-source-map"`即可

14. **输出结果不携带路径信息**

- 默认`webpack`会在输出的`bundle`中生成路径信息，将路径信息删除可小幅度提升构建速度
  - 设置`output.pathinfo: false`来关闭路径信息
