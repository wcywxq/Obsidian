---
title: ts 关键字间的区别
url: https://www.yuque.com/wcywxq/mxunh7/sagma3
---

<a name="no2WV"></a>

## const 和 readonly 的区别

1. `const` 用于变量，`readonly` 用于属性
2. `const` 在运行时检查，`readonly` 在编译时检查
3. 使用 `const` 变量保存的数组，可以使用 `push`、`pop` 等方法。但是如果使用 `ReadonlyArray<number>` 声明的数组不能使用 `push`、`pop` 等方法 <a name="d1ION"></a>

## type 和 interface 的区别

1. 类型别名 `type` 可以为任何类型引入名称。例如基本类型，联合类型等
2. 类型别名 `type` 不支持继承
3. 类型别名 `type` 不会创建一个真正的名字
4. 类型别名 `type` 无法被实现(`implements`)，而接口 `interface` 可以被派生类实现
5. 类型别名 `type` 重名时编译器会抛出错误，接口 `interface` 重名时会产生合并
6. 类型别名 `type` 无法在函数上挂载属性，接口 `interface` 可以

```typescript
interface FuncWithAttachment {
  (params: string): boolean
  someProperty: number
}
const testFunc: FuncWithAttachment = {}
const result = testFunc('mike') // 无类型提醒
testFunc.someProperty = 3 // 有类型提醒
```

<a name="eEY0e"></a>

## implements 和 extends 的区别

- `extends`, 子类会继承父类的所有属性和方法。
- `implements`，使用 `implements` 关键字的类将需要实现需要实现的类的所有属性和方法。 <a name="Q4E2a"></a>

## 枚举 和 object 的区别

- 枚举可以通过枚举的名称，获取枚举的值。也可以通过枚举的值获取枚举的名称。
- `object` 只能通过 `key` 获取 `value`
- 数字枚举在不指定初始值的情况下，枚举值会从 `0` 开始递增。
- 虽然在运行时，枚举是一个真实存在的对象。但是使用 `keyof` 时的行为却和普通对象不一致。必须使用 `keyof typeof` 才可以获取枚举所有属性名。 <a name="dfnPF"></a>

## never 和 void 的区别

- `never`、`never` 表示永远不存在的类型。比如一个函数总是抛出错误，而没有返回值。或者一个函数内部有死循环，永远不会有返回值。函数的返回值就是 `never` 类型。
- `void`，没有显示的返回值的函数返回值为 `void` 类型。如果一个变量为 `void` 类型，只能赋予 `undefined` 或者 `null`。 <a name="Hl0D8"></a>

## any、never、unknown、null\&undefined、void 的区别

- `any`：动态的变量类型(失去了类型检查的作用)
- `never`：永不存在的值的类型。例如：`never`类型是那些总是会抛出异常或根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型。
- `unknown`：任何类型的值都可以赋给 `unknown`类型，但是`unknown`类型的值只能赋给`unknown`本身和`any`类型。与`any`类型不同的是，`unknown`类型可以接受任意类型赋值，但是`unknown`类型赋值给其他类型前，必须被断言。
- `null & undefined`：默认情况下 `null`和`undefined`是所有类型的子类型。就是说你可以把`null`和`undefined`赋值给`number`类型的变量。当你指定了`--strictNullChecks`标记，`null`和`undefined`只能赋值给`void`和它们自己
- `void`：没有任何类型。
