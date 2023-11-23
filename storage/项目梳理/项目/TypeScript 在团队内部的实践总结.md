---
title: TypeScript 在团队内部的实践总结
url: https://www.yuque.com/wcywxq/aggvdq/bp0lie
---

<a name="64c6f2f3"></a>

## TS 的特性

JavaScript：

- 解释型脚本语⾔，⽆需额外编译，便可在浏览器等环境中解释执⾏。

TypeScript：

- ⾯向对象的编程语⾔，增加了静态类型、接⼝、泛型、类型注解等。
- 通过静态类型检测，更容易编写健壮性强的代码，更适合⼤型项⽬开发。
- 浏览器、Node 等环境⽆法直接运⾏ TypeScript，需编译成 JavaScript 后运⾏。

<a name="52b8c184"></a>

## 优点

- 丰富的类型系统：基本类型、接口、类、函数、泛型、枚举......
- 强大的类型推导能力：支持类型推导，命名空间和模块，装饰器等特性
- 完善的生态支持：tsc/bable 编译器、IDE、eslint、[@types ](/types)

<a name="a38829fa"></a>

## 前后端通信之痛

我之前所在的部门，一个化妆品电商品牌前端团队，N 个前端，M 个品牌，2N 个后端，总共几百人，几乎每天都在不停的开发、联调中，电商业务需求多、迭代块、细节多，一款品牌电商经过历史迭代、多⼈多团队维护，存在代码结构混乱、逻辑耦合极⾼，单⼀⽂件动辄上千⾏，代码可读性、可维护性均较差；基于 JavaScript 设计的数据结构、数据传递等⾮常复杂，导致线上⼩问题层出不穷，排查修复成本⾼；另外还有是由于每个研发同事的水平、代码风格、做事风格的不同，也难以保持 API 和字段单词的规范和统一，更别说所有开发人员保证实时更新 API 文档了。

<a name="2372b182"></a>

### ⽬标

- 项目太大，在长期的迭代中，代码风格不同，组件、函数升级，参数不兼容
- 前后端接口字段不一致等
- 接口文档与代码不一致等

基于以上一些项目维护中的痛点，决定想要实现通过对项⽬代码的基础能⼒、业务逻辑⼩步快跑式的拆解，降低项⽬代码的复杂度，并形成团队内⼀致的⼯程规范、编码规范等，提升项⽬代码的可读性、可维护性。

通过 TypeScript 等相关技术栈引⼊，梳理并重构业务代码数据结构、数据传递等逻辑，通过静态类型校验，增加编译时排雷的概率，降低运⾏时出错的⻛险。

维持项⽬技术新度，偿还历史项⽬的技术债务，提升团队同学的技术热情与技术能⼒。

<a name="f47adc3a"></a>

## TS 在欧莱雅电商业务中的迁移实践

<a name="74c68b51"></a>

### 迁移原则

![image.png](../assets/bp0lie/1647788931569-ff27bf88-cbf2-426b-a218-f9b25d290548.png)

<a name="0f6433a9"></a>

### 实践⽅案

Monorepo (pnpm + TypeScript) + esbuild + Changesets

<a name="83527ca9"></a>

### ⽅案设计说明

以小程序为例，考虑到电商务的代码体量及复杂性，比如兰蔻、YSL、娇兰等电商小程序至少 100+页面，妄图一个小程序一个主包吞下整个电商全流程不太现实。所以，我们采取的⽅案是按照功能属性拆分代码并渐进式改造升级。第⼀步，我们对项⽬中"基础能⼒"部分拆分；第⼆步，我们对项⽬中"业务逻辑"与"视图渲染"拆分。

基础能⼒部分，独⽴功能模块众多，因此采⽤ Monorepo 模式整合。出于构建速度的考量，对 TypeScript 的 Dev 环境与 Prod 环境构建使⽤ Vite/webpack + esbuild (团队种老的电商多租户平台还是在用 Vue2 + elementui，新电商后台已全面改用 vite + Vue3/React)。基础能⼒部分，功能相对独⽴、业务属性弱，在渐进式改造升级的过程中以依赖包的形式集成⼊原⼯程，所以使⽤轻量化、社区活跃度⾼的 Changesets 来迭代维护依赖包更新。

<a name="0f6433a9-1"></a>

### 实践⽅案

在 toB 的 PC 端项目方面，⼯程结构：

```typescript
 .changeset                 -- 基础库版本管理配置
 dist                       -- 基础库production产物
 packages
    - loreal-sign           -- 商城登录基础库
    - loreal-ec             -- EC电商通用基础库
    - loreal-axios          -- 二次封装ec请求库
    - loreal-trace          -- 电商埋点SDK及其工具
    - loreal-utils          -- EC电商工具链
 pnpm-workspace.yaml        --
 tsconfig.json              -- dev模式 TypeScritp配置文件
 tsconfig.type-check.json   -- tsc模式 TypeScript配置文件，用于prod模式构建时做并行类型检查
 vite.config.ts             -- vite dev模式配置文件
 vite.prod-config.ts        -- vite prod模式配置文件
```

在 toC 的小程序端项目方面，⼯程结构：

```typescript
  miniprogram
    - api                   -- API治理中心
    - assets                -- 静态资源
    - components            -- loreal电商组件
    - config                -- 与APPID有关的配置信息
    - packages              -- 分包
    - pages                 -- 主包
    - plugins               -- 插件
    - utils                 -- 工具函数，包括wxs
  model
    - api                   -- api接口的types文件、插件的types
    index.d.ts
  node_modules
  typings
    - types                 -- 微信自身api的一些types
    index.d.ts
- package.json
- project.config.json
- project.private.config.json
- tsconfig.json
```

<a name="eaebb74e"></a>

## TS 迁移过程中的踩坑记录

<a name="161c52fb"></a>

### 1. 对于 toB 的项目，Vite 对库模式开发⽀持不友好

踩坑：Vite 新⽣事物，虽说迭代挺快，但不⾜也挺明显。如，Vite 对多⻚应⽤构建配置友好， 但对 Monorepo 库模式配置能⼒不⾜，需要编程式（Vite -> build()）解决构建需求。

<a name="20b26e2f"></a>

### 2. esbuild ⽆类型检查能⼒及声明⽂件⽣成能⼒

踩坑：选择 Vite 很⼤⼀部分因素是它使⽤ esbuild 做构建，且 esbuild 内置了 TypeScript 编译能⼒， 但 esbuild 不提供类型检查与声明⽂件⽣成。所以，最终的⽅案是，ESBuild 构建同时使⽤ TSC 编译器并⾏的完成类型检查与声明⽂件⽣成。

<a name="f634ebdd"></a>

### 3. TSConfig 配置⽂件⾃定义问题

踩坑：仅 Prod 模式就需要 2 个 TS 配置⽂件。TSC 命令⽐较⽅便指定，但 Vite 暴露的 build 函数（执⾏ esbuild 构建）基本⽆说明⽂档。撸 Vite 源码找⾃定义配置⽅式，再结合 esbuild 官⽅⽂档

<a name="5b099d89"></a>

## TS 代码迁移技巧

<a name="5b2bd130"></a>

### 1.不使用 TS 提供的语法糖（除非已经纳入了 ECMAScript 标准）

enum、重载、 public, privaete, protected, readonly 修饰符、方法装饰器，访问器装饰器，属性装饰器等

<a name="a163a782"></a>

### 2.定义类型时尽量使用交叉类型（不使用 extends），Utility 定义 Types

交叉类型和 Mixins 有一点区别：交叉类型只是一个类型声明，用于类型约束；Mixins 会给类增加成员，new 对象时，对象会包含增加的成员属性。 同时 TS 在全局内置了很多 Utility Types,可以极大的提高我们开发效率。

<a name="e44e7faa"></a>

### 3.公共类型放在 src/types 或者 types 下，方便以后统一处理

在日常开发中可能会经常用到 webpack 的路径别名，比如: import xxx from '@/path/to/name'，如果编辑器不做任何配置的话，编译器不会给任何路径提示，更不会给你语法提示。这里有个小技巧，基于 tsconfig.json 的 baseUrl 和 paths 这两个字段，配置好这两个字段后，.ts 文件里不但有了路径提示，还会跟踪到该路径进行语法提示。
如果把 tsconfig.json 重命名成 jsconfig.json，.js 文件里也可以享受到路径别名提示和语法提示。

:::info
如果把 tsconfig.json 重命名成 jsconfig.json，.js 文件里也可以享受到路径别名提示和语法提示。
:::

<a name="f0b6dbea"></a>

### 4.需要时才定义类型，尽量使用类型推断，非复用（简单）类型使用字面量定义

在 ts 中，代码实现中的 typeof 关键词能够帮助 ts 判断出变量的基本类型:

```typescript
function fn(x: string | number) {
    if (typeof x === "string") {
        // x is string
        return x.length
    } else {
        // x is number
        // .....
    }
}
```

instanceof 关键词能够帮助 ts 判断出构造函数的类型;在条件判断中，ts 会自动对 null 和 undefined 进行类型保护,如果我们已经知道的参数不为空，可以使用 ! 来手动标记.

```typescript
function fn2(x?: string) {
    return x!.length
}
```

<a name="853984fd"></a>

### 5.暂时不能解决的使用 TsFixme & [@ts-ignore ](/ts-ignore)

在开发过程中，我们都会不时地写一些 // TODO: 和 // FIXME: 注释。有时我们这样做是因为我们知道代码可以做得更好，但暂时不确定如何做，有时由于 deadline 而没有时间编写最佳解决方案，而有时我们只是想着手处理更紧急的事情，这时我们只需在代码中标识一个 // TODO: or // FIXME 提示自己以便在将来某一天再处理。

```typescript
/**
 * 手动获取用户在天猫旗舰店的会员等级和积分
 * @returns {any | *} 返回会员等级和积分; FIXME: 此处类型较为复杂，先用 any 代替，有空再补
 */
```

<a name="91fa4fd0"></a>

## TS 迁移流程

<a name="f2faa7eb"></a>

### 1.先培训

因为一些常用的 JS 写法，在 TS 里面需要稍微变通一下才能使用，比如我们常用的 promise.all 这种方法，在 TypeScript 中，当你这么用 Promise.all 时，会遇到类型检查错误的问题。

```typescript
Promise.all(Promise < XX > [])
```

因为 lib.es6.d.ts 中，对 Promise.all 这么定义

```typescript
all<TAll>(values: Iterable<TAll | PromiseLike<TAll>>): Promise<TAll[]>;
```

所以，对上述案例，TAll 被自动识别为了 Promise；而实际上，TAll 应该是 XX。 该定义文件的 PromiseLike 跟 Promise 似乎没什么关系，所以没能自动识别。 所以使用时，如果 Promise.all 传入的是一个数组，那么建议的用法是强制制定类型，如下。

```typescript
Promise.all<XX>(Promise < XX > [])
```

又比如：解决 ts-node 中使用 symlinks 时引用 node\_modules 报错的问题 preserveSymlinks 在 TypeScripts 中默认为 true 而在 NodeJS 中默认为 false

```typescript
ts-node --preserve-symlinks index.ts
```

<a name="715c7290"></a>

### 2.项目种常用类型补全，建立标准，服务于全体应用

前后端共享代码和类型定义，全程代码提示和类型报错

<a name="9855570a"></a>

### 3.小范围试验，确保转换过程简单、安全和自动化

先在欧莱雅、兰蔻、薇姿项目中重构改造，在一些重要电商里先行试水，随后在整个电商事业群里推广。改造的范围包括但不限于：

1. core utils 迁移
2. 选取核心文件进行迁移

<a name="e87638f2"></a>

### 4.总结试验结果并优化

为了减轻这种风险，我们需要一个规范的流程来转换文件，其不会引入回归，也不会诱导工程师去做多余的事情。这个流程还要能快速执行。

我们确定了一个分为两部分的流程：首先自动转换 CoffeeScript 文件，然后立即手动添加基本类型注解和与 linter 相关的更改。关键在于抵制（不管是什么方式）重构代码的诱惑。这样一来，转换工作就成为了简单、遵循安全规则的机械活动，不会影响运行时行为。

<a name="ffd31892"></a>

### 5.大范围推广

1. 新页面新文件、工具函数必须使用 ts，老文件修改超过 40% 需用 ts 重写
2. 使用 snippet 模板
3. code review

<a name="c22810c9"></a>

## 收益&总结

<a name="25c14cba"></a>

### 收益：开发体验必须明显改善

要让整个团队都参与进来，必须让开发人员体会到，他们在编写 TypeScript 时会更有效率。如果团队只是将迁移看作是从一种语法换成另一种差不多的语法，他们永远都不会产生认同感。如果迁移之后他们的日常工作效率并没有提升，那么就算工程师们往往更喜欢编写类型化代码，也敌不过旧习惯的巨大惯性。

将工具链和配置作为优先事项来对待。大多数开发人员使用的编辑器就是那么几种而已，因此我们创建了可以直接使用的编辑器配置，添加了调试配置，从而可以轻松设置断点和单步执行代码。

最后，我们整理了一套取得共识的 linting 规则，这些规则使我们能够在整个组织中以统一的样式编写代码，并让开发人员对迁移行动更加满意。

当团队开始看到这些转换工作的成果时，整个项目也就得到了认可，前进动力也会更足了。当我们的工程师开始将类型化数据访问视为必不可少的工具后，他们就能更好地意识到，代码库的其它部分也会平稳地转换完毕。

<a name="25f9c7fa"></a>

### 总结

⽬前，电商业务渐进式改造升级，第⼀部分“基础能⼒”的拆分进⼊尾声，初步拆分出“账号登录”、“⽹络请求”、“数据埋点”等模块。通过“基础能⼒”模块的拆分，降低了原有代码的冗余度，使相关功能模块的逻辑清晰化、内聚化，可维护性增强；通过 TypeScript 语⾔的使⽤，使复杂的数据结构类型化、规范化，能更好的应对业务体量的增⻓，提升产品运⾏时的稳定性；同时，新的技术栈及技术⽅案的引⼊，切实提升了团队同学的技术能⼒，营造了团队良好的技术氛围。

全面拥抱 Typescript，不仅仅是增加了类型，而是编程方式的转变。

变量 => 类型 => 结构

面向过程 => 面向接口
