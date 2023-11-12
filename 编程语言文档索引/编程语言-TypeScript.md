---
title: 编程语言-TypeScript
url: https://www.yuque.com/wcywxq/rddgev/gchdc2
---

<a name="93922108"></a>

## 一、 基础类型

<a name="db5daa39"></a>

### 1.1 类型断言

> 有时候你会遇到这样的情况，你会比 `TypeScript` 更了解某个值的详细信息。通常这会发生在你清楚的知道一个实体具有比它现有类型更确切的类型。类型断言好比其它语言里的类型转换，但是不尽兴特殊的数据检查和解构。它没有运行时的影响，只是在编译阶段起作用。

- 类型断言有两种形式

- 第一种：尖括号语法 (当在 `TypeScript` 里使用 `JSX` 时，只有 `as` 语言断言是被允许的)

```typescript
let someValue: any = "this is a string";
let strLength: number = (<string>someValue).length;
```

- 第二种：`as` 语法

```typescript
let _someValue: any = "this is a string";
let _strLength: number = (_someValue as string).length;
```

<a name="9b3125b6"></a>

### 1.2 基础类型

<a name="791c716a"></a>

#### 1.2.1 布尔值

```typescript
let isDone: boolean = false;
```

<a name="136743cf"></a>

#### 1.2.2 数字

> 与 `javascript` 一样，`TypeScript` 里的所有数字都是浮点数。这些浮点数的类型是 `number`。 除了支持十进制和十六进制字面量， `TypeScript` 还支持 `ECMA2015` 中引入的二进制和八进制字面量。

```typescript
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
let binaryLiteral: number = 0b1010;
let octalLiteral: number = 0o744;
```

<a name="f617dd89"></a>

#### 1.2.3 字符串

```typescript
let strName: string = 'bob';
```

<a name="98f85b5f"></a>

#### 1.2.4 数组

> `TypeScript` 像 `JavaScript` 一样可以操作数组元素。有两种方法定义数组。

- 第一种：可以在元素类型后面接上 `[]`;

```typescript
let list1: number[] = [1, 2, 3];
```

- 第二种：使用数组泛型, `Array<元素类型>`

```typescript
let list2: Array<number> = [1, 2, 3];
```

<a name="3b1d1e80"></a>

#### 1.2.5 元组 Tuple

- 元组类型允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。

```typescript
let x: [string, number];
x = ["hello", 10]; // => Ok
x = [10 ,"hello"]; // => Error
```

- 当访问一个已知索引的元素，会得到正确的类型

```typescript
console.log(x[0].substr(1)); // => Ok
console.log(x[1].substr(1)); // => Error
```

- 当访问一个越界的元素，会使用联合类型替代

```typescript
x[3] = "world"; // Ok, 字符串可以赋值给(string | number)类型
console.log(x[5].toString()); // Ok, "string"和"number"都有toString方法
x[6] = true; // Error, 布尔不是(string | number)类型
```

<a name="643b18d7"></a>

#### 1.2.6 枚举

- `enum` 类型是对 `JavaScript` 标准数据类型的一个补充。像 `C#` 等其他语言一样，使用枚举类型可以为一组数值赋予友好的名字

```typescript
enum Color { Red, Green, Blue }
let c: Color = Color.Green;
```

- 默认情况下，从 `0` 开始为元素编号。你也可以手动的指定成员的数值。

```typescript
enum Color1 { Red = 1, Green, Blue }
let c1: Color1 = Color1.Green;
```

- 或者全部采用赋值

```typescript
enum Color2 { Red = 1, Green = 2, Blue = 4 }
let c2: Color2 = Color2.Green;
```

- 枚举类型提供的一个便利是你可以由枚举的值得到它的名字。

```typescript
enum Color3 { Red = 1, Green, Blue }
let colorName: string = Color3[2];
console.log(colorName);
```

<a name="639e2222"></a>

#### 1.2.7 Any

- 有时候，我们想要为那些在编程阶段还不清楚类型的变量指定一个类型。这些值可能来自于动态的内容，比如来自用户输入或第三方库。这种情况下，我们不希望类型检查器对这些值进行检查而是让它们通过编译阶段的检查。那么我们可以使用 `any` 类型来标记这些变量

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // Ok
```

- 在对现有代码进行改写的时候，`any` 类型是十分有用的，它允许你在编译时可选择地包含或移除类型检查。你可能认为 `Object` 有相似的作用，就像在其他语言中那样。但是 `Object` 类型的变量只是允许你给他赋任意值，但是却不能够在它上面调用任意的方法，即使它真有这些方法。

```typescript
let notSure1: any = 4;
notSure1.ifItExists(); // Ok
notSure1.toFixed(); // Ok
let prettySure: Object = 4;
// prettySure.toFixed(); // Error, Property 'toFixed' doesn't exist on type 'Object'.
```

- 当你只知道一部分数据类型时，`any` 类型也是有用的。

```typescript
let list: any[] = [1, true, 'free'];
list[1] = 100;
```

<a name="341e40c9"></a>

#### 1.2.8 Void

- 某种程度上来说，`void` 类型像是与 `any` 类型相反，它表示没有任何类型。当一个函数没有返回值时，你通常会见到其返回值类型是 `void`。

```typescript
function warnUser(): void {
    console.log("This is my warning message");
}
```

- 声明一个 `void` 类型的变量没有什么大用，因为你只能为它赋予 `undefined` 和 `null`

```typescript
let unusbale: void = undefined;
```

<a name="6d6ec9c6"></a>

#### 1.2.9 Null 和 Undefined

- `TypeScript` 里，`undefined` 和 `null` 两者各有自己的类型分别叫做 `undefined` 和 `null`。和 `void` 相似，它们本身的类型用处不是很大。

```typescript
let u: undefined = undefined;
let n: null = null;
```

- 默认情况下 `null` 和 `undefined` 是所有类型的子类型。就是说你可以把 `null` 和 `undefined` 赋值给 `number` 类型的变量。
- 然而，当你指定了一个 `--strictNullChecks` 标记，`null` 和 `undefined` 只能赋值给 `void` 和它们各自。这能避免很多常见的问题。也许在某处你想传入一个 `string` 或 `null` 或 `undefined`，你可以使用联合类型 `string | null | undefined`。

<a name="3593923e"></a>

#### 1.2.10 Never

- `never` 类型表示的是那些永不存在的值的类型。例如：`never` 类型是那些总会抛出异常或根本不会有返回值的函数表达式或箭头函数表达式的返回值类型；
- 变量也可能是 `never` 类型，当它们被永不为真的类型保护所约束时。
- `never` 类型是任何类型的子类型，也可以赋值给任何类型；

> 然而，没有类型是 `never` 的子类型或可以赋值给 `never` 类型(除了 `never` 本身之外)。即使 `any` 也不可以赋值给 `never`。

```typescript
// 返回 never 的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}
// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}
// 返回 never 的函数必须存在法达到的终点
function infiniteLoop(): never {
    while (true) {}
}
```

<a name="5ebc50e4"></a>

#### 1.2.11 Object

- `Object` 表示非原始类型，也就是除 `number`, `string`, `boolean`, `symbol`, `null` 或 `undefined` 之外的类型。
- 使用 `Object` 类型，就可以更好的表示像 `Object.create` 这样的 `API`。

```typescript
declare function create(o: object | null): void;
create({ prop: 0 }); // Ok
create(null); // Ok
create(42); //Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

<a name="6df43059"></a>

## 二、变量声明

<a name="7e9e013a"></a>

### 2.1 介绍

> `let` 和 `const` 是 `JavaScript` 里相对较新的变量声明方式。 像我们之前提到过的， `let` 在很多方面与 `var` 是相似的，但是可以帮助大家避免在 `JavaScript` 里常见一些问题。 `const` 是对 `let` 的一个增强，它能阻止对一个变量再次赋值。
> 因为 `TypeScript` 是 `JavaScript` 的超集，所以它本身就支持 `let` 和 `const` 。 下面我们会详细说明这些新的声明方式以及为什么推荐使用它们来代替 `var`。
> 如果你之前使用 `JavaScript` 时没有特别在意，那么这节内容会唤起你的回忆。 如果你已经对 `var` 声明的怪异之处了如指掌，那么你可以轻松地略过这节。

<a name="c902ae77"></a>

### 2.2 var 声明

> 一直以来我们都是通过 `var` 关键字定义 `JavaScript` 变量。

```typescript
var a = 10;
```

> 大家都能理解，这里定义了一个名为 `a` 值为 `10` 的变量。
> 我们也可以在函数内部定义变量：

```typescript
function f() {
    var message = "Hello, world!";
        return message;
}
```

> 并且我们也可以在其它函数内部访问相同的变量。

```typescript
function f() {
    var a = 10;
    return function g() {
        var b = a + 1;
        return b;
    }
}

var g = f();
g(); // returns 11;
```

> 上面的例子里，`g` 可以获取到 `f` 函数里定义的 `a` 变量。 每当 `g` 被调用时，它都可以访问到 `f` 里的 `a` 变量。 即使当 `g` 在 `f` 已经执行完后才被调用，它仍然可以访问及修改 `a`。

```typescript
function f() {
    var a = 1;

    a = 2;
    var b = g();
    a = 3;

    return b;

    function g() {
        return a;
    }
}

f(); // returns 2
```

<a name="1b0e94c3"></a>

### 2.3 作用域规则

> 对于熟悉其它语言的人来说，`var` 声明有些奇怪的作用域规则。 看下面的例子：

```typescript
function f(shouldInitialize: boolean) {
    if (shouldInitialize) {
        var x = 10;
    }
        return x;
}

f(true);  // returns '10'
f(false); // returns 'undefined'
```

> 有些读者可能要多看几遍这个例子。 变量 `x` 是定义在 `if` 语句里面\_，但是我们却可以在语句的外面访问它。 这是因为 `var` 声明可以在包含它的函数，模块，命名空间或全局作用域内部任何位置被访问（我们后面会详细介绍），包含它的代码块对此没有什么影响。 有些人称此为 `var` 作用域或函数作用域。函数参数也使用函数作用域。
> 这些作用域规则可能会引发一些错误。 其中之一就是，多次声明同一个变量并不会报错：

```typescript
function sumMatrix(matrix: number[][]) {
    var sum = 0;
    for (var i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (var i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }
        return sum;
}
```

> 这里很容易看出一些问题，里层的 `for` 循环会覆盖变量 `i`，因为所有 `i` 都引用相同的函数作用域内的变量。 有经验的开发者们很清楚，这些问题可能在代码审查时漏掉，引发无穷的麻烦。

<a name="0ecd59b5"></a>

### 2.4 捕获变量怪异之处

> 快速的猜一下下面的代码会返回什么：

```typescript
for (var i = 0; i < 10; i++) {
    setTimeout(function() { console.log(i); }, 100 * i);
}
```

> 介绍一下，`setTimeout` 会在若干毫秒的延时后执行一个函数（等待其它代码执行完毕）。
> 好吧，看一下结果：

```typescript
10
10
10
10
10
10
10
10
10
10
```

> 很多 `JavaScript` 程序员对这种行为已经很熟悉了，但如果你很不解，你并不是一个人。 大多数人期望输出结果是这样：

```typescript
0
1
2
3
4
5
6
7
8
9
```

> 还记得我们上面提到的捕获变量吗？
> 我们传给 `setTimeout` 的每一个函数表达式实际上都引用了相同作用域里的同一个 `i`。
> 让我们花点时间思考一下这是为什么。 `setTimeout` 在若干毫秒后执行一个函数，并且是在 `for` 循环结束后。 `for` 循环结束后，`i` 的值为 `10`。 所以当函数被调用的时候，它会打印出 `10！`
> 一个通常的解决方法是使用立即执行的函数表达式（`IIFE`）来捕获每次迭代时 `i` 的值：

```typescript
for (var i = 0; i < 10; i++) {
    // capture the current state of 'i'
    // by invoking a function with its current value
    (function(i) {
        setTimeout(function() { console.log(i); }, 100 * i);
    })(i);
}
```

> 这种奇怪的形式我们已经司空见惯了。 参数 `i` 会覆盖 `for` 循环里的i，但是因为我们起了同样的名字，所以我们不用怎么改 `for` 循环体里的代码。

<a name="5e9d6f56"></a>

### 2.5 let 声明

> 现在你已经知道了 `var` 存在一些问题，这恰好说明了为什么用 `let` 语句来声明变量。 除了名字不同外， let与 `var` 的写法一致。

```typescript
let hello = "Hello!";
```

> 主要的区别不在语法上，而是语义，我们接下来会深入研究。

<a name="58aad62c"></a>

### 2.6 块作用域

> 当用 `let` 声明一个变量，它使用的是词法作用域或块作用域。 不同于使用 `var` 声明的变量那样可以在包含它们的函数外访问，块作用域变量在包含它们的块或 `for` 循环之外是不能访问的。

```typescript
function f(input: boolean) {
    let a = 100;
    if (input) {
        // Still okay to reference 'a'
        let b = a + 1;
        return b;
    }

    // Error: 'b' doesn't exist here
    return b;
}
```

> 这里我们定义了`2`个变量 `a` 和 `b` 。 `a` 的作用域是 `f` 函数体内，而 `b` 的作用域是 `if` 语句块里。
> 在 `catch` 语句里声明的变量也具有同样的作用域规则。

```typescript
try {
    throw "oh no!";
}
catch (e) {
    console.log("Oh well.");
}

// Error: 'e' doesn't exist here
console.log(e);
```

> 拥有块级作用域的变量的另一个特点是，它们不能在被声明之前读或写。 虽然这些变量始终“存在”于它们的作用域里，但在直到声明它的代码之前的区域都属于 暂时性死区。 它只是用来说明我们不能在 `let` 语句之前访问它们，幸运的是 `TypeScript` 可以告诉我们这些信息。

```typescript
a++; // illegal to use 'a' before it's declared;
let a;
```

> 注意一点，我们仍然可以在一个拥有块作用域变量被声明前获取它。 只是我们不能在变量声明前去调用那个函数。 如果生成代码目标为 `ES2015`，现代的运行时会抛出一个错误；然而，现今 `TypeScript` 是不会报错的。

```typescript
function foo() {
    // okay to capture 'a'
    return a;
}

// 不能在'a'被声明前调用'foo'
// 运行时应该抛出错误
foo();

let a;
```

> 关于暂时性死区的更多信息，查看这里 [Mozilla Developer Network.](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let)

<a name="4141f88a"></a>

### 2.7 重定义及屏蔽

> 我们提过使用 `var` 声明时，它不在乎你声明多少次；你只会得到 `1` 个。

```typescript
function f(x) {
    var x;
    var x;
    if (true) {
        var x;
    }
}
```

> 在上面的例子里，所有 `x` 的声明实际上都引用一个相同的 `x`，并且这是完全有效的代码。 这经常会成为`bug` 的来源。 好的是， `let` 声明就不会这么宽松了。

```typescript
let x = 10;
let x = 20; // 错误，不能在1个作用域里多次声明`x
```

> 并不是要求两个均是块级作用域的声明 `TypeScript` 才会给出一个错误的警告。

```typescript
function f(x) {
    let x = 100; // error: interferes with parameter declaration
}

function g() {
    let x = 100;
    var x = 100; // error: can't have both declarations of 'x'
}
```

> 并不是说块级作用域变量不能用函数作用域变量来声明。 而是块级作用域变量需要在明显不同的块里声明。

```typescript
function f(condition, x) {
    if (condition) {
        let x = 100;
        return x;
    }
        return x;
}

f(false, 0); // returns 0
f(true, 0);  // returns 100
```

> 在一个嵌套作用域里引入一个新名字的行为称做屏蔽。 它是一把双刃剑，它可能会不小心地引入新问题，同时也可能会解决一些错误。 例如，假设我们现在用 `let` 重写之前的 `sumMatrix` 函数。

```typescript
function sumMatrix(matrix: number[][]) {
    let sum = 0;
    for (let i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (let i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }
        return sum;
}
```

> 这个版本的循环能得到正确的结果，因为内层循环的i可以屏蔽掉外层循环的 `i`。
> 通常来讲应该避免使用屏蔽，因为我们需要写出清晰的代码。 同时也有些场景适合利用它，你需要好好打算一下。

<a name="416ef34b"></a>

### 2.8 块级作用域变量的获取

> 在我们最初谈及获取用 `var` 声明的变量时，我们简略地探究了一下在获取到了变量之后它的行为是怎样的。 直观地讲，每次进入一个作用域时，它创建了一个变量的 环境。 就算作用域内代码已经执行完毕，这个环境与其捕获的变量依然存在。

```typescript
function theCityThatAlwaysSleeps() {
    let getCity;
    if (true) {
        let city = "Seattle";
        getCity = function() {
            return city;
        }
    }
    return getCity();
}
```

> 因为我们已经在 `city` 的环境里获取到了 `city` ，所以就算if语句执行结束后我们仍然可以访问它。
> 回想一下前面 `setTimeout` 的例子，我们最后需要使用立即执行的函数表达式来获取每次 `for` 循环迭代里的状态。 实际上，我们做的是为获取到的变量创建了一个新的变量环境。 这样做挺痛苦的，但是幸运的是，你不必在 `TypeScript` 里这样做了。
> 当 `let` 声明出现在循环体里时拥有完全不同的行为。 不仅是在循环里引入了一个新的变量环境，而是针对 每次迭代都会创建这样一个新作用域。 这就是我们在使用立即执行的函数表达式时做的事，所以在 `setTimeout` 例子里我们仅使用 `let` 声明就可以了。

```typescript
for (let i = 0; i < 10 ; i++) {
    setTimeout(function() {console.log(i); }, 100 * i);
}
```

> 会输出与预料一致的结果：

```typescript
0
1
2
3
4
5
6
7
8
9
```

<a name="d62b9d1e"></a>

### 2.9 const 声明

> `const` 声明是声明变量的另一种方式。

```typescript
const numLivesForCat = 9;
```

> 它们与 `let` 声明相似，但是就像它的名字所表达的，它们被赋值后不能再改变。 换句话说，它们拥有与 `let` 相同的作用域规则，但是不能对它们重新赋值。
> 这很好理解，它们引用的值是不可变的。

```typescript
const numLivesForCat = 9;
const kitty = {
    name: "Aurora",
    numLives: numLivesForCat,
}

// Error
kitty = {
    name: "Danielle",
    numLives: numLivesForCat
};

// all "okay"
kitty.name = "Rory";
kitty.name = "Kitty";
kitty.name = "Cat";
kitty.numLives--;
```

> 除非你使用特殊的方法去避免，实际上 `const` 变量的内部状态是可修改的。 幸运的是，`TypeScript` 允许你将对象的成员设置成只读的。 接口一章有详细说明。

<a name="20bd7689"></a>

### 2.10 let vs. const

> 现在我们有两种作用域相似的声明方式，我们自然会问到底应该使用哪个。 与大多数泛泛的问题一样，答案是：依情况而定。
> 使用最小特权原则，所有变量除了你计划去修改的都应该使用 `const`。 基本原则就是如果一个变量不需要对它写入，那么其它使用这些代码的人也不能够写入它们，并且要思考为什么会需要对这些变量重新赋值。 使用 `const` 也可以让我们更容易的推测数据的流动。
> 跟据你的自己判断，如果合适的话，与团队成员商议一下。
> 这个手册大部分地方都使用了 `let` 声明。

<a name="90cfe94d"></a>

### 2.11 解构

> `Another TypeScript` 已经可以解析其它 `ECMAScript 2015` 特性了。 完整列表请参见 [the article on the Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)。 本章，我们将给出一个简短的概述。

<a name="ea620f42"></a>

#### 2.11.1 解构数组

> 最简单的解构莫过于数组的解构赋值了：

```typescript
let input = [1, 2];
let [first, second] = input;
console.log(first); // outputs 1
console.log(second); // outputs 2
```

> 这创建了 `2` 个命名变量 `first` 和 `second`。 相当于使用了索引，但更为方便：

```typescript
first = input[0];
second = input[1];
```

> 解构作用于已声明的变量会更好：

```typescript
// swap variables
[first, second] = [second, first];
```

> 作用于函数参数：

```typescript
function f([first, second]: [number, number]) {
    console.log(first);
    console.log(second);
}
f(input);
```

> 你可以在数组里使用 `...` 语法创建剩余变量：

```typescript
let [first, ...rest] = [1, 2, 3, 4];
console.log(first); // outputs 1
console.log(rest); // outputs [ 2, 3, 4 ]
```

> 当然，由于是 `JavaScript`, 你可以忽略你不关心的尾随元素：

```typescript
let [first] = [1, 2, 3, 4];
console.log(first); // outputs 1
```

> 或其它元素：

```typescript
let [, second, , fourth] = [1, 2, 3, 4];
```

<a name="8662d63b"></a>

#### 2.11.2 对象解构

> 你也可以解构对象：

```typescript
let o = {
    a: "foo",
    b: 12,
    c: "bar"
};
let { a, b } = o;
```

> 这通过 `o.a and o.b` 创建了 `a` 和 `b` 。 注意，如果你不需要 `c` 你可以忽略它。
> 就像数组解构，你可以用没有声明的赋值：

```typescript
({ a, b } = { a: "baz", b: 101 });
```

> 注意，我们需要用括号将它括起来，因为 `Javascript` 通常会将以 `{` 起始的语句解析为一个块。
> 你可以在对象里使用 `...` 语法创建剩余变量：

```typescript
let { a, ...passthrough } = o;
let total = passthrough.b + passthrough.c.length;
```

<a name="f5c86fdc"></a>

#### 2.11.3 属性重命名

> 你也可以给属性以不同的名字：

```typescript
let { a: newName1, b: newName2 } = o;
```

> 这里的语法开始变得混乱。 你可以将 `a: newName1` 读做 "`a` 作为 `newName1`"。 方向是从左到右，好像你写成了以下样子：

```typescript
let newName1 = o.a;
let newName2 = o.b;
```

> 令人困惑的是，这里的冒号不是指示类型的。 如果你想指定它的类型， 仍然需要在其后写上完整的模式。

```typescript
let {a, b}: {a: string, b: number} = o;
```

<a name="bd7bedc2"></a>

#### 2.11.4 默认值

> 默认值可以让你在属性为 `undefined` 时使用缺省值：

```typescript
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject;
}
```

> 现在，即使 `b` 为 `undefined` ， `keepWholeObject` 函数的变量 `wholeObject` 的属性 `a` 和 `b` 都会有值。

<a name="bfcba61e"></a>

#### 2.11.5 函数声明

> 解构也能用于函数声明。 看以下简单的情况：

```typescript
type C = { a: string, b?: number }
function f({ a, b }: C): void {
    // ...
}
```

> 但是，通常情况下更多的是指定默认值，解构默认值有些棘手。 首先，你需要在默认值之前设置其格式。

```typescript
function f({ a="", b=0 } = {}): void {
    // ...
}
f();
```

> 上面的代码是一个类型推断的例子，将在本手册后文介绍。
> 其次，你需要知道在解构属性上给予一个默认或可选的属性用来替换主初始化列表。 要知道 `C` 的定义有一个 `b` 可选属性：

```typescript
function f({ a, b = 0 } = { a: "" }): void {
    // ...
}
f({ a: "yes" }); // ok, default b = 0
f(); // ok, default to {a: ""}, which then defaults b = 0
f({}); // error, 'a' is required if you supply an argument
```

> 要小心使用解构。 从前面的例子可以看出，就算是最简单的解构表达式也是难以理解的。 尤其当存在深层嵌套解构的时候，就算这时没有堆叠在一起的重命名，默认值和类型注解，也是令人难以理解的。 解构表达式要尽量保持小而简单。 你自己也可以直接使用解构将会生成的赋值表达式。

<a name="b5261991"></a>

#### 2.11.6 展开

> 展开操作符正与解构相反。 它允许你将一个数组展开为另一个数组，或将一个对象展开为另一个对象。 例如：

```typescript
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5];
```

> 这会令 `bothPlus` 的值为 `[0, 1, 2, 3, 4, 5]`。 展开操作创建了 `first` 和 `second` 的一份浅拷贝。 它们不会被展开操作所改变。
> 你还可以展开对象：

```typescript
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { ...defaults, food: "rich" };
```

> `search` 的值为 `{ food: "rich", price: "$$", ambiance: "noisy" }`。 对象的展开比数组的展开要复杂的多。 像数组展开一样，它是从左至右进行处理，但结果仍为对象。 这就意味着出现在展开对象后面的属性会覆盖前面的属性。 因此，如果我们修改上面的例子，在结尾处进行展开的话：

```typescript
let defaults = { food: "spicy", price: "$$", ambiance: "noisy" };
let search = { food: "rich", ...defaults };
```

> 那么，`defaults` 里的 `food` 属性会重写 `food: "rich"`，在这里这并不是我们想要的结果。
> 对象展开还有其它一些意想不到的限制。 首先，它仅包含对象 [自身的可枚举属性](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Enumerability_and_ownership_of_properties)。 大体上是说当你展开一个对象实例时，你会丢失其方法：

```typescript
class C {
  p = 12;
  m() {
  }
}
let c = new C();
let clone = { ...c };
clone.p; // ok
clone.m(); // error!
```

> 其次，`TypeScript` 编译器不允许展开泛型函数上的类型参数。 这个特性会在 `TypeScript` 的未来版本中考虑实现。

<a name="27c36696"></a>

## 三、接口

<a name="fea87294"></a>

### 3.1 实现一个简单的接口

```typescript
interface LabelledValue {
    label: string;
}
function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}
let myObj = { size: 10, label: "size 10 Object" };
printLabel(myObj);
```

<a name="4e4a9ff4"></a>

### 3.2 可选属性

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}
function createConfig(config: SquareConfig): { color: string, area: number} {
    let newSquare = { color: "white", area: 100 };
    if(config.color) {
        newSquare.color = config.color;
    }
    if(config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}
let mySquare = createConfig({ color: "black", width: 20 });
console.log(mySquare);
```

<a name="a707e5a2"></a>

### 3.3 只读属性

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error! 因为属性是只读的
```

<a name="ebf8a97a"></a>

### 3.4 额外的属性检查

> 问题：将" *可选属性* "与" `option bags` "模式相结合而引发

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}
function createConfig(config: SquareConfig): { color: string, area: number } {
    let newSquare = { color: "white", area: 100 };
    if(config.color) {
        newSquare.color = config.color;
    }
    if(config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}
```

> `let mySquare = createConfig({ colour: "red", width: 100 }); //报错`

- 解决方案一：采用类型断言

```typescript
let mySquare = createConfig({width: 100, opacity: 0.5} as SquareConfig);
```

- 解决方案二：采用添加一个字符串索引签名(最佳方案)

```typescript
interface _SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

- 解决方案三：变量赋值

<a name="36a89cec"></a>

### 3.5 函数类型

<a name="067908ba"></a>

#### 3.5.1 函数类型

```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    let result = source.search(subString);
    return result > -1;
};
```

<a name="9392e223"></a>

#### 3.5.2 函数类型

```typescript
let add: (baseValue: number, increment: number) => number = (x, y) => x + y;

let add2 = (x: number, y: number): number => x + y;

let buildNameFun: (fname: string, ...rest: Array<string>) => string =
    (firstName: string, ...restOfName: Array<string>) =>
        `${firstName}, ${restOfName.join(' ')}`;

let fn = (a: number, b: number): number => a + b;
```

<a name="52aed2a1"></a>

#### 3.5.3 函数类型

```typescript
interface func {
    (x: string, y: string): boolean;
}
let func: func = (x: string, y: string) => {
    return x.search(y) > -1;
};
```

<a name="b5146a38"></a>

### 3.6 可索引类型

- 例如：`a[10]` 或 `ageMap["daniel"]`
- 可索引类型具有一个 索引签名，它描述了对象索引的类型，还有相应的索引返回值类型

```typescript
interface StringArray {
  [index: number]: string;
}
let myArray: StringArray;
myArray = ["Bob", "Fred"];
let myStr: string = myArray[0];
console.log(myStr);
```

> `Ts` 支持两种索引签名：字符串 和 数字。可以同时使用两种类型的索引，但数字索引的返回值必须是字符串索引返回值类型的子类型。

<a name="4173419a"></a>

### 3.7 类类型

> 接口描述了类的公共部分，而不是类的公有和私有两部分。它不会帮你检查类是否具有某些私有成员

```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date); // 在接口中描述的方法
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) { // 在类中的具体实现
        this.currentTime = d;
    }
    constructor(h: number, m: number) {
    }
}
```

> 当操作类的时候，我们需要知道类是具有两个类型的：*静态部分的类型* 和 *实例的类型*。
> 当用构造器签名去定义一个接口并试图定义一个类去实现这个接口时会得到一个错误。
> 因为当一个类实现了一个接口时，只对其实例部分进行类型检查。`constructor` 存在于类的静态部分，所以不在检查的范围内。

- 因此应当直接操作 *类的静态部分*

```typescript
interface err_ClockConstructor {
    new (hour: number, minute: number);
}
class err_Clock implements err_ClockConstructor {
    currentTime: Date;
    constructor(h: number, m : number) {}
}
```

- 下面是实现对 *静态类型* 的检查工作

> 因为 `createClock` 的第一个参数是 `ClockConstructor` 类型，在 `createClock(AnalogClock, 7, 32)` 里，会检查 `AnalogClock` 是否符合构造函数签名

```typescript
interface ClockConstructor {
    new(hour: number, minute: number): ClockInterface
}

interface ClockInterface {
    tick();
}

function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) {
    }

    tick() {
        console.log("beep beep")
    }
}

class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) {
    }

    tick() {
        console.log("tick tick")
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
console.log(digital);
console.log(analog);
```

<a name="243828cd"></a>

### 3.8 接口继承

- 和类一样，接口也可以相互继承。可以灵活地将接口分割到可重用的模块里。

```typescript
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
```

- 一个接口可以继承多个接口，创建出多个接口的合成接口

```typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```

- 目的: `=>` 灵活地将接口分割到可重用的模块中

```typescript
interface Lamp {
    lampOn(): void;

    lampOff(): void;
}

interface wx {
    wxNumber: number;

    showWxNumber(): string;
}

interface Photo extends Lamp, wx {
    photo(): string;
}

class HuaWeiPhone implements Photo {
    public wxNumber: number;

    photo(): string {
        return "华为手机"
    }

    lampOn(): void { }

    lampOff(): void { }
    constructor(wxNumber: number) {
        this.wxNumber = wxNumber;
    }

    showWxNumber(): string {
        return "我的微信号：123";
    }
}
let huaWeiPhone = new HuaWeiPhone(12345678910);
console.log(huaWeiPhone.showWxNumber());
console.log(huaWeiPhone.photo());
```

<a name="a21224fd"></a>

### 3.9 混合类型

> 一个对象可以同时做为函数和对象使用，并带有额外的属性

```typescript
interface Counter {
    (start: number): string;

    interval: number;

    reset(): void;
}

function getCounter(): Counter {
    let counter = <Counter>function (start: number) {
        // return start + 'string';
    };
    counter.interval = 123;
    counter.reset = function () {
    };
    return counter;
}

let c = getCounter();
c(10);
c.interval = 5.0;
c.reset();
```

<a name="47001b6f"></a>

### 3.10 接口继承类

1. 当接口继承了一个类类型时，它会继承类的成员但不包括其实现。
2. 就好像接口声明了所有类中存在的成员，但并没有提供具体实现一样。
3. 接口同样会继承到类的 `private` 和 `protected` 成员。
4. 这意味着当你创建了一个接口继承了一个拥有私有或受保护的成员的类时，这个接口类型只能被这个类或其子类所实现( `implement` )。
5. 当你有一个庞大的继承接口时这很有用，但要指出的是你的代码只在子类拥有特定属性时起作用。这个子类除了继承至基类外与基类没有任何关系。

```typescript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control implements SelectableControl {
    select(): void { }
}

class TextBox extends Control {
    select(): void { }
}

// 错误："Image"类型缺少"state"属性
class Image implements SelectableControl {
    select(): void { }
}
class Location { }
```

> 在上面的例子中，`SelectableControl` 包含了 `Control` 的所有成员。包括私有成员 `state`。因为 `state` 是私有成员，所以只能够是 `Control` 的子类们才能实现 `SelectableControl` 接口，因为只有 `Control` 的子类才能够拥有一个声明于 `Control` 的私有成员 `state`，这对私有成员的兼容性是必需的。
> 在 `Control` 类内部，是允许通过 `SelectableControl` 的实例来访问私有成员 `state` 的。实际上，`SelectableControl` 接口和拥有 `select` 方法的 `Control` 类是一样的。`Button` 和 `TextBox` 类是 `SelectableControl` 的子类(因为它们都继承自 `Control` 并有 `select` 方法，但 `Image` 和 `Location` 类并不是这样的。)

<a name="95a4742a"></a>

### 3.11 接口继承接口

> 目的: `=>` 灵活地将接口分割到可重用的模块中

```typescript
interface Lamp {
    lampOn(): void;
    lampOff(): void;
}
interface wx {
    wxNumber: number;
    showWxNumber(): string;
}

interface Photo extends Lamp, wx {
    photo(): string
}

class HuaWeiPhone implements Photo {
    public wxNumber: number;
    photo(): string {
        return '华为手机'
    }
    lampOn() {}
    lampOff() {}
    constructor(wxNumber: number) {
        this.wxNumber = wxNumber;
    }
    showWxNumber() {
        return '我的微信号是：123';
    }
}

let huaWeiPhone = new HuaWeiPhone(13100970071);
console.log(huaWeiPhone.showWxNumber());
console.log(huaWeiPhone.photo());
```

<a name="ed5c2744"></a>

### 3.12 类接口实现

> 手机类是一个大类
> 华为是手机类下的一个类
> 华为手机有拍照和闪光灯功能，照相机也有拍照和闪光灯功能
> 因此华为手机和照相机的公共特性就是拍照和闪光灯
> 所以通过关键字 `implements` 来标识提取出来的接口

```typescript
// 拍照
interface Photo {
    photo(): string;
}
// 闪光灯
interface Lamp {
    lampOn(): void;
    lampOff(): void;
}

class Phone { }

class HuaWei extends Phone implements Photo, Lamp {
    photo(): string {
        return '华为拍照';
    }
    lampOn() { }
    lampOff() { }
}

class DigitalCamera implements Photo, Lamp {
    photo(): string {
        return '照相机拍照'
    }
    lampOn() { }
    lampOff() { }
}
```

<a name="d695b0ea"></a>

## 四、类

<a name="948629ce"></a>

### 4.1 创建一个基本类

```typescript
class Gretter {
    public gretting: string;
    constructor(message: string) {
        this.gretting = message;
    }
    public greet() {
        return `Hello, ${this.gretting}`;
    }
}

let gretter: Gretter = new Gretter('world!');
console.log(gretter);
```

<a name="a64f2a4e"></a>

### 4.2 继承

```typescript
class Animal {
    name: string;
    constructor(theName: string) {
        this.name = theName;
    }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`)
    }
}

class Snake extends Animal{
    constructor(name: string) {
        super(name);
    }
    move(distanceInMeters = 5){
        console.log('Slithering...');
        super.move(distanceInMeters);
    }
}

class Horse extends Animal{
    constructor(name: string) {
        super(name);
    }
    move(distanceInMeters = 45) {
        console.log('Galloping');
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

<a name="83795918"></a>

### 4.3 public 关键字

> 默认为 `public`
> 在 `TypeScript` 中,成员都默认为 `public`

```typescript
class Animal_public {
    public name: string;
    public constructor(theName: string) {
        this.name = theName;
    }
    public move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

<a name="79771f39"></a>

### 4.4 private 关键字

> 理解 `private`
>
> 1. 当成员被标记成 `private` 时，它就不能在声明它的类的外部访问
> 2. 如果其中一个类型里包含一个 `private` 成员，那么只有当另外一个类型中也存在这样一个 `private` 成员， 并且它们都是来自同一处声明时，我们才认为这两个类型是兼容的。

```typescript
class Animal_private {
    private name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}
const cat = new Animal_private("Cat");
// cat.name; // 错误：'name'是私有的

class Animal_private_1 {
    private name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}
class Rhino extends Animal_private_1{
    constructor() {
        super("Rhino");
    }
}
class Employee {
    private name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}
let animal = new Animal_private_1("Goat");
let rhino = new Rhino();
let employee = new Employee("Bob");

animal = rhino;
// animal = employee; //错误：Animal与Employee不兼容
```

<a name="24002237"></a>

### 4.5 protected 关键字

- 理解 `protected`

> `protected` 修饰符与 `private` 修饰符的行为很相似，但是有一点不同，`protected` 成员在派生类中仍然可以访问。

```typescript
class Person {
    protected name: string;
    constructor(name: string) {
        this.name = name;
    }
}
class _Employee extends Person {
    private department: string;
    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }
    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}`;
    }
}
let howard = new _Employee("Howard", "Sales");
console.log(howard.getElevatorPitch());
// console.log(howard.name); // 错误，因为 name 无法在外部访问，但是可以在派生类中访问
```

- 构造函数也可以被标记成 `protected`

> 这意味着这个类不能在包含它的类外被实例化。但是能被继承。

```typescript
class __Person {
    protected name: string;
    protected constructor(theName: string) {
        this.name = theName;
    }
}
// __Employee 能够继承 Person
class __Employee extends __Person{
    private department: string;
    constructor(name: string, department: string) {
        super(name);
        this.department = department;
    }
    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`
    }
}
let __howard = new __Employee("Howard", "Sales");
// let __join = new __Person("John");// 错误：'Person'的构造函数是被保护的
```

<a name="1f9011a6"></a>

### 4.6 readonly 修饰符

- 你可以使用 `readonly` 关键字将属性设置为只读的。只读属性必须在声明时或构造函数里被初始化

```typescript
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor(theName: string) {
        this.name = theName;
    }
}
let dad = new Octopus("Man with the 8 strong legs");
// dad.name = "Man with the 3-piece suit";// 错误，name是只读的
```

- 参数属性

> 1. 参数属性可以方便地让我们在一个地方定义并初始化一个成员。
> 2. 参数属性通过给构造函数参数前面添加一个访问限定符来声明。
> 3. 使用 `private` 限定一个参数属性会声明并初始化一个私有成员；对于 `public` 和 `protected` 来说也是一样。

```typescript
class _Octopus {
    readonly numberOfLegs: number = 9;
    constructor(readonly name: string) {}
}
```

<a name="39bbc1cd"></a>

### 4.7 存取器 get & set

> 存取器 支持 `es5+`，不支持 `es3`

1. `TypeScript` 支持通过 `getters/setters` 来截取对对象成员的访问。它能帮助你有效的控制对对象成员的访问。
2. 没有存取器的例子

```typescript
class _Employee {
    fullName: string
}
let _employee = new _Employee();
_employee.fullName = "Bob Smith";
if(_employee.fullName) {
    console.log(_employee.fullName);
}
```

3. 拥有存取器的例子

> 1. 我们可以修改一下密码，来验证一下存取器是否是工作的。当密码不对时，会提示我们没有权限去修改员工。
> 2. 只带有 `get` 不带有 `set` 的存取器自动被推断为 `readonly`。
> 3. 这在从代码生成 `.d.ts` 文件时是有帮助的，因为利用这个属性的用户会看到不允许够改变它的值。

```typescript
let passcode = "secret passcode";
class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if(passcode && passcode === "secret passcode") {
            this._fullName = newName;
        } else {
            console.log("Error: Unauthorized update of employee!")
        }
    }
}
let employee = new Employee();
employee.fullName = "Bob smith";
if(employee.fullName) {
    console.log(employee.fullName)
}
```

<a name="849de0d3"></a>

### 4.8 静态属性

> 我们也可以创建类的静态成员，这些属性存在于类本身上面而不是类的实例上。

```typescript
class Gird {
    public static origin = {x: 0, y: 0};
    public calculateDistanceFromOrigin(point: {x: number; y: number}) {
        let xDist = (point.x - Gird.origin.x);
        let yDist = (point.y - Gird.origin.y);
        return Math.sqrt(xDist * xDist + yDist * yDist) / this.scale;
    }
    public constructor(public scale: number) { }
}
console.log(Gird.origin.x);
let grid1 = new Gird(1.0);
let gird2 = new Gird(5.0);
console.log(grid1.calculateDistanceFromOrigin({x: 10, y: 10}));
console.log(gird2.calculateDistanceFromOrigin({x: 10, y: 10}));
```

<a name="ae74a689"></a>

### 4.9 抽象类

1. 抽象类作为其他派生类的基类使用，它们一般不会直接被实例化。
2. 不同于接口，抽象类可以包含成员的实现细节
3. `abstract` 关键字是用于定义抽象类和在抽象类内部定义抽象方法

```typescript
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log('roaming the earth ...')
    }
}
```

4. 抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。
5. 抽象方法的语法于接口方法相似，二者都是定义方法签名但不包含方法体。然而抽象方法必须包含 `abstract` 关键字并且可以包含访问修饰符

```typescript
abstract class Department {
    constructor(public name: string) {
    }
    printName(): void {
        console.log(`Department name: ${this.name}`)
    }
    abstract printMeeting(): void; // 必须在派生类中实现
}
class AccountingDepartment extends Department{
    constructor() {
        super('Accounting and Auditing');
    }
    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }
    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}
let department: Department; // 允许创建一个对抽象类的引用
// department = new Departmemnt(); // 错误，不能创建一个抽象类的实例
department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值
department.printName();
department.printMeeting();
// department.generateReports(); // 错误，方法在声明的抽象类中不存在
```

<a name="21e8d1ea"></a>

### 4.10 高级技巧(构造函数)

> 当在 `TypeScript` 里声明了一个类的时候，实际上声明了很多东西。首先就是类的实例类型

```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return 'Hello,' + this.greeting;
    }
}
let greeter: Greeter; // =>意思：Greeter类的实例的类型是Greeter
greeter = new Greeter("world");
console.log(greeter.greet());
```

> 可以认为类具有 *实例部分* 与 *静态部分*

```typescript
class _Greeter {
    static standardGreeting = "Hello, there";
    greeting: string;
    greet() {
        if(this.greeting) {
            return "Hello," + this.greeting;
        } else {
            return _Greeter.standardGreeting;
        }
    }
}
let _greeter: _Greeter;
_greeter = new _Greeter();
console.log(_greeter.greet());
```

> 此处创建了一个 `greeterMaker` 的变量，这个变量保存了这个 *类* 或者说保存了 *类构造函数*。然后使用 `typeof _Greeter`，意思是取 `Greeter` 类的类型，而不是实例的类型。或者更确切的说，"告诉我 `Greeter` 标识符的类型"，也就是构造函数的类型。这个类型包含了类的所有静态成员和构造函数。

```typescript
let _greeterMaker: typeof _Greeter = _Greeter;
_greeterMaker.standardGreeting = "Hey there!";
let _greeter2: _Greeter = new _greeterMaker();
console.log(_greeter2.greet());
```

<a name="c5d8f768"></a>

### 4.11 高级技巧(把类当作接口使用)

> 类定义会创建两个东西：*类的实例类型* 和 *一个构造函数*。
> 因为类可以创建出类型，所以能够在允许使用接口的地方使用类

```typescript
class Point {
    x: number;
    y: number;
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 4};
```

<a name="46a4436d"></a>

## 五、函数

<a name="c99a3256"></a>

### 5.1 介绍

> 函数是 `JavaScript` 应用程序的基础。它帮助你实现抽象层，模拟层，信息隐藏和模块。在 `TypeScript` 里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义 行为的地方。`TypeScript` 为 `JavaScript` 函数添加了额外的功能，让我们可以更容易的使用。

<a name="7ab937ef"></a>

### 5.2 函数类型

<a name="6b5dc147"></a>

#### 5.2.1 为函数定义类型

> 我们可以给每个参数添加类型之后再为函数本身添加返回值类型。

> `TypeScript` 能够根据返回语句自动推断出返回值类型，因此我们通常省略它。

```typescript
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function (x: number, y: number): number {
    return x + y;
};
```

<a name="8dd21769"></a>

#### 5.2.2 书写完整的函数类型

```typescript
let myAdd1: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

> 函数类型分为两个部分：*参数类型* 和 *返回值类型*。当写出完整函数类型的时候，这两部分都是需要的。我们以参数列表的形式写出参数类型，为每一个参数指定一个名字和类型。这个名字只是为了增加可读性。

> 1. 只要参数类型是匹配的，那么就认为它是有效的函数类型，而不在乎参数名是否正确。
> 2. 第二部分是返回值类型。对于返回值，我们在函数和返回值类型之间使用 `=>` 符号，使之清晰明了。  如之前提到的，返回值类型是函数类型的必要部分，如果函数没有返回任何值，你也必须指定返回值类型为 `void`,而不能留空。

> 函数的类型只是由参数类型和返回值组成的。函数中使用的捕获变量不会体现在类型里。实际上，这些变量是函数的隐藏状态并不是组成 `API` 的一部分。

- 我们也可以这么写：

```typescript
let myAdd2: (baseValue: number, increment: number) => number = function (x: number, y: number): number {
  return x + y;
};
```

<a name="52d16392"></a>

#### 5.2.3 推断类型

> 在尝试下面这个例子的时候，你会发现如果你在赋值语句的一边指定了类型但是另一边没有类型的话，TypeScript 编译器会自动识别出类型。这叫做："按上下文归类"，是类型推论的一种。它帮助我们更好地为程序指定类型。

```typescript
// myAdd has the full function type
let myAdd3 = function(x: number, y: number): number { return x + y };
// The parameters `x` and `y` have the type number
let myAdd4: (baseValue: number, increment: number) =number = function (x, y) {
  return x + y;
};
```

<a name="70e15cde"></a>

### 5.3 可选参数和默认参数

> `TypeScript` 里的每个函数参数都是必须的。这不是指不能传递 `null` 或 `undefined` 作为参数，而是说编译器检查用户是否为每个参数都传入了值。编译器还会假设只有这些参数会被传递进函数。

1. 简短地说，传递给一个函数的参数个数必须与函数期望的参数个数一致。

```typescript
function buildName(firstName: string, lastName: string) {
    return firstName + ' ' + lastName;
}
// let result1 =  buildName("Bob"); // error
// let result2 = buildName("Bob", "Jack", "Sr."); // error
let result3 = buildName("Bob", "Jack");
```

2. `JavaScript`里，每个参数都是可选的，可传可不传。没传参的时候，它的值就是 `undefined`。在 `TypeScript` 里我们可以在参数旁边使用 `?` 实现可选参数的功能。
3. 注意：可选参数必须跟在必选参数后面，不能放在前面

```typescript
function buildName1(firstName: string, lastName?: string) {
    if(lastName) {
        return firstName + " " + lastName;
    } else {
        return firstName;
    }
}
let result_1 = buildName1("Bob");
let result_2 = buildName1("Bob", "Jack");
// let result_3 = buildName1("Bob", "Jack", "Sr."); // error
```

4. 可以设置默认初始化的参数
5. 在所有必须参数后面的带默认初始化的参数都是可选的，与可选参数一样，在调用函数的时候可以省略。也就是说，可选参数与末尾的默认参数共享参数类型。

```typescript
function buildName2(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}
```

6. 与普通可选参数参数不同的是，带默认值的参数不需要放在必须参数的后面。如果带默认值的参数出现在必须参数的前面，用户必须明确的传入 `undefined` 值来获得默认值。

```typescript
function buildName3(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}
// let result__1 = buildName3("Bob"); // error
// let result__2 = buildName3("Bob", "Jack", "Sr."); // error
let result__3 = buildName3("Bob", "Jack");
let result__4 = buildName3(undefined, "Jack");
```

<a name="a6a0420c"></a>

### 5.4 剩余参数

> 必要参数，默认参数 和 可选参数 有个共同点：它们表示某一个参数。有时，你想同时操作多个参数，或者你并不知道会有多少参数传进来。在 `JavaScript` 里，你可以使用 `arguments` 来访问所有传入的参数。在 `TypeScript` 里，你可以把所有参数收集到一个变量里

```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

let employeeName = buildName("Jose", "Smith", "Bob", "Alice");
```

> 剩余参数会被当作个数不限的可选参数。可以一个都没有，也可以有任意个。编译器创建参数数组，名字是你在省略号后面给定的名字，你可以在函数体内使用这个数组。

```typescript
function buildName1(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}

let buildNameFun: (fName: string, ...rest: string[]) => string = buildName1;
```

> `this` 参数。在使用了 `--noImplicitThis` 标记之后，`this` 类型可能会是 `any`。此时需要提供一个显示的 `this` 参数。

```typescript
interface Card {
    suit: string;
    card: number;
}

interface Deck {
    suits: string[];
    cards: number[];

    createCardPicker(this: Deck): () => Card;
}

let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker(): (this: Deck) => Card {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);

            return {suit: this.suits[pickedSuit], card: pickedCard % 13};
        }
    }
};
let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();
console.log(`card: ${pickedCard.card} of ${pickedCard.suit}`);
```

<a name="2f907283"></a>

## 六、泛型

<a name="1e911178"></a>

### 6.1 介绍

> 在软件工程中，我们不仅要创建一致的定义良好的 `API`，同时也要考虑可重用性。组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。
> 在像 `C#` 和 `Java` 这样的语言中，可以使用 `泛型` 来创建可重用组件，一个组件可以支持多种类型的数据。这样用户就可以以自己的数据类型来使用组件。
> 除了泛型接口，我们还可以创建泛型类。注意，无法创建泛型枚举和泛型命名空间。

<a name="6ed33fa9"></a>

### 6.2 泛型之 hello world

> 创建一个 `identity` 函数，这个函数会返回任何传入它的值。

- 如果不使用泛型，函数可能是：

```typescript
function identity(arg: number): number {
    return arg;
}
```

- 也可以使用 `any` 类型定义函数，但是这样可能会导致传入的类型与返回的类型不相同。

```typescript
function identity1(arg: any): any {
    return arg;
}
```

> 因此，我们需要一种方法使返回值的类型与传入参数的类型是相同的。

- 这里我们使用了类型变量，它是一种特殊的变量，只能用于表示类型而不是值。

```typescript
function identity2<T>(arg: T): T {
    return arg;
}
```

- 使用方式一：传入所有的参数，包括类型参数

```typescript
let output = identity2<string>("myString");
```

- 使用方式二(更普遍)：利用了类型推论-即编译器会根据传入的参数自动地帮助我们确定 `T` 的类型。

```typescript
let output2 = identity2("myString");
```

<a name="93c0e432"></a>

### 6.3 使用泛型变量

> 使用泛型创建像 `identity` 这样的函数时，编译器要求你在函数体必须正确的使用这个通用的类型。换句话说，你必须把这些参数当作是任意或所有类型。

- 如果我们想打印出 `arg` 的长度，也许会这样做。

```typescript
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length); // 错误，T 类型也许不会有.length属性
    return arg;
}
```

- 如果我们像操作 `T` 类型的数组

```typescript
function loggingIdentity<T>(arg: T[]): T[] {
    console.log(arg.length);
    return arg;
}
function loggingIdentity2<T>(arg: Array<T>): Array<T> {
    console.log(arg.length);
    return arg;
}
```

<a name="02666da9"></a>

### 6.4 泛型类型(泛型接口)

1. 泛型函数的类型和非泛型函数的类型一样，首先列出类型参数，类似于函数声明。

```typescript
function identity<T>(arg: T): T {
    return arg;
}

let myIdentity: <T>(arg: T) => T = identity;
```

2. 我们也可以为类型中的泛型类型参数使用不同的名称，只要在数量上和使用方式上能对应上就可以。

```typescript
function identity1<T>(arg: T): T {
    return arg;
}

let myIdentity1: <U>(arg: U) => U = identity;
```

3. 我们还可以使用带有调用签名的对象字面量来定义泛型函数：

```typescript
function identity2<T>(arg: T): T {
    return arg;
}

let myIdentity2: { <T>(arg: T): T } = identity;
```

4. 这引导我们去写第一个泛型接口了。我们把上面例子里的对象字面量拿出来做为一个接口：

```typescript
interface GenericIdentityFn {
    <T>(arg: T): T
}

function identity3<T>(arg: T): T {
    return arg;
}

let myIdentity3: GenericIdentityFn = identity3;
```

5. 我们可能想把泛型参数当作整个接口的一个参数。这样我们就能清楚的知道使用的具体是哪个泛型类型(比如：`Dictionary<string>` 而不只是 `Dictionary`)。这样接口里的其它成员也能知道这个参数的类型了。

```typescript
interface _GenericIdentityFn<T> {
    (arg: T): T;
}

function _identity<T>(arg: T): T {
    return arg;
}

let _myIdentity: _GenericIdentityFn<number> = _identity;
```

> 注意，我们的示例做了少许改动。不再描述泛型函数，而是把非泛型函数签名作为泛型类型的一部分。

> 当我们使用 `GenericIdentityFn` 的时候，还得传入一个类型参数来指定泛型类型(这里是：`number`)，从而锁定之后代码里使用的类型。

> 对于描述哪部分类型属于泛型部分来说，理解何时把参数 "放在调用签名里" 和何时 "放在接口上" 是很有帮助的。

<a name="9242e893"></a>

### 6.5 泛型类

> 泛型类和泛型接口差不多。泛型类使用 `<>` 括起泛型类型，跟在类名后面。类分为两部分：静态部分 和 实例部分。泛型类指的是实例部分的类型，所以类的静态属性不能使用这个泛型类型。

> `enericNumber` 类的使用是非常直观的，并没有什么去限制它只能使用 `number` 类型。

- 也可以使用字符串或其它更复杂的类型。

```typescript
class GenericNumber<T> {
zeroValue: T;
add: (x: T, y: T) => T
}
let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = (x, y) => x + y;
```

> 与接口一样，直接把泛型类型放在类后面，可以帮助我们确认类的所有属性都在使用相同的类型。

```typescript
let stringNumeric = new GenericNumber<string>();
stringNumeric.zeroValue = " ";
stringNumeric.add = (x, y) => x + y;
console.log(stringNumeric.add(stringNumeric.zeroValue, "test"));
```

<a name="67b504a8"></a>

### 6.6 泛型约束

> 我们应该记得之前的一个例子，我们有时候想要操作某类型的一组值，并且我们知道这组值具有什么样的属性。

> 在 `loggingIdentity` 例子中，我们想访问 `arg` 的 `length` 属性，但是编译器并不能证明每种类型都有 length 属性。

```typescript
function loggingIdentity<T>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

> 相比与操作 `any` 所有类型，我们想要限制函数去处理任意带有 `.length` 属性的所有类型。只要传入的类型有这个属性，我们就允许，就是说至少包含这一属性。为此，我们需要列出对于 `T` 的约束要求。

> 为此，我们需要定义一个接口来描述约束条件。创建一个包含 `.length` 属性的接口，使用这个接口和 `extends` 关键字来实现约束。

```typescript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

- 此时这个泛型函数被定义了约束。因此它不再适用于任意类型：

```typescript
loggingIdentity(3); // error
```

- 我们需要传入符合约束类型的值，必须包含的属性。

```typescript
loggingIdentity({length: 10, value: 3});
```

<a name="213a7592"></a>

### 6.7 在泛型约束中使用类型参数

> 你可以声明一个类型参数，且它被另一个类型参数所约束。例如，现在我们需想要用属性名从对象里获取这个属性。并且我们想要确保这个属性存在于对象 `obj` 上，因此我们需要在这两个类型之间使用约束。

```typescript
function getProperty<T, K extends keyof T>(obj: T, key: K) {
    return obj[key];
}
let x = {a: 1, b: 2, c: 3, d: 4};
getProperty(x, "a");
getProperty(x, "m"); // error
```

<a name="44be19e5"></a>

### 6.8 在泛型里使用类类型

> 在 `TypeScript` 使用泛型创建工厂函数时，需要引用构造函数的类类型。

```typescript
function create<T>(c: { new(): T }): T {
    return new c();
}
```

> 下面是一个更高级的例子，使用原型属性推断并约束构造函数与类实例的关系。

```typescript
class BeeKeeper {
    hasMask: boolean;
}

class ZooKeeper {
    nameTag: string;
}

class Animal {
    numLegs: number;
}

class Bee extends Animal {
    keeper: BeeKeeper;
}

class Lion extends Animal {
    keeper: ZooKeeper;
}

function createInstance<A extends Animal>(c: new() => A): A {
    return new c();
}

createInstance(Lion).keeper.nameTag;
createInstance(Bee).keeper.hasMask;
```

<a name="390ce393"></a>

### 6.9 多个类型变量

```typescript
function info<S, N>(name: S, age: N): [S, N] {
    return [name, age];
}

console.log(info("pr", 10));
console.log(info<string, number>('pr', 18));
```

<a name="b4d36e89"></a>

## 七、枚举

<a name="1a830e28"></a>

### 7.1 介绍

> 使用枚举我们可以定义一些带有名字的常量。使用枚举可以清晰地表达意图或创建一组有区别的用例。`TypeScript` 支持数字的和基于字符串的枚举。

<a name="a0a9026e"></a>

### 7.2 数字枚举

```typescript
enum Direction {
    Up = 1,
    Down,
    Left,
    Right
}
```

> 如上，我们定义了一个数字枚举，`Up`使用初始化为 `1`，其余成员会从 `1` 开始自动增长。 换句话说，`Direction.Up` 的值为 `1`，`Down` 值为 `2`，`Left` 值为 `3`，`Right` 值为 `4`。

1. 我们还可以完全不使用初始化器

```typescript
enum Direction2 {
    Up,
    Down,
    Left,
    Right
}
```

> 现在，`Up` 的值为 `0`，`Down` 的值为 `1` 等等。 当我们不在乎成员的值的时候，这种自增长的行为是很有用处的，但是要注意每个枚举成员的值都是不同的。

2. 使用枚举很简单：通过枚举的属性来访问枚举成员，和枚举的名字来访问枚举类型：

```typescript
enum _Response {
    No = 0,
    Yes = 1,
}

function respond(recipient: string, message: _Response): void {

}

respond("Princess Caroline", _Response.Yes);
```

3. 数字枚举可以被混入到计算过的和常量成员（如下所示）。

> 简短地说，不带初始化器的枚举或者被放在第一的位置，或者被放在使用了数字常量或其它常量初始化了的枚举后面。换句话说，下面的情况是不被允许的：

```typescript
enum E {
    A = getSomeValue(),
    B, // error! 'A' is not constant-initialized, so 'B' needs an initializer
}
```

<a name="77b96580"></a>

### 7.3 字符串枚举

> 字符串枚举的概念很简单，但是有细微的运行时的差别。在一个字符串枚举里，每个成员都必须使用字符串字面量，或另外一个字符串枚举成员进行初始化。

```typescript
enum Direction {
    Up = "UP",
    Down = "DOWN",
    Left = "LEFT",
    Right = "RIGHT"
}
```

> 由于字符串枚举没有自增长的行为，字符串枚举可以很好的序列化。换句话说，如果你正在调试并且必须要读一个数字枚举的运行时的值，这个值通常是很难读的 - 它并不能表达有用的信息（尽管反向映射会有所帮助），字符串枚举允许你提供一个运行时有意义的并且可读的值，独立于枚举成员的名字。

<a name="79d5d7a6"></a>

### 7.4 异构枚举

> 从技术的角度来说，枚举可以混合字符串和数字成员，但是你似乎并不会这么做：

```typescript
enum BooleanLikeHeterogeneousEnum {
    No = 0,
    Yes = "YES",
}
```

> 除非你真的想要利用 `JavaScript` 运行时的行为，否则我们不建议这样做。

<a name="9cd7c4ca"></a>

### 7.5 计算的和常量成员

> 每个枚举成员都带有一个值，它可以是常量或计算出来的。当满足如下条件时，枚举成员被当作是常量

1. 它是枚举的第一个成员且没有初始化器，这种情况下它被赋予值 `0`：

```typescript
enum E { X }
```

2. 它不带有初始化器且它之前的枚举成员是一个数字常量。这种情况下，当前枚举成员的值为它上一个枚举成员的值加 `1`。

```typescript
enum E1 { X, Y, Z }
enum E2 { A = 1, B, C }
```

3. 枚举成员使用 常量枚举表达式进行初始化。常量枚举表达式是 TypeScript 表达式的子集，它可以在编译阶段求值。

> 当一个表达式满足下面条件之一时，它就是一个常量枚举表达式：
>
> 1. 一个枚举表达式字面量（主要是字符串字面量或数字字面量）
> 2. 一个对之前定义的常量枚举成员的引用（可以是在不同的枚举类型中定义的）
> 3. 带括号的常量枚举表达式
> 4. 一元运算符 +，-，～ 其中之一应用在了常量枚举表达式
> 5. 常量枚举表达式作为二元运算符 `+，-，*，/，%，<<，>>，>>>，&，|，^` 的操作对象

> 若常量枚举表达式求值后为 `NaN` 或 `Infinity`，则会在编译阶段报错。

> 所有其它情况的枚举成员被当作是需要计算得出的值

```typescript
enum FileAccess {
    // constant numbers
    None,
    Read = 1 << 1,
    White = 1 << 2,
    ReadWhite = Read | White,
    // computed number
    G = '123'.length,
}
```

<a name="52ad235a"></a>

### 7.6 联合枚举与枚举成员的类型

> 存在一种特殊的非计算的常量枚举成员的子集：字面量枚举成员。字面量枚举成员是指不带有初始值的常量枚举成员，或者是被初始化为：
>
> 1. 任何字符串字面量(例如：`"foo", "bar", "baz"`)
> 2. 任何数字字面量(例如：`1, 100`)
> 3. 应用了一元 - 符号的数字字面量(例如：`-1, -100`)  当所有枚举成员都拥有字面量枚举值时，它就有了一种特殊的含义。

> 首先，枚举成员成为了类型！例如，我们可以说某些成员只能是枚举成员的值：

```typescript
enum ShapeKind {
    Circle,
    Square,
}

interface Circle {
    kind: ShapeKind.Circle,
    radius: number,
}

interface Square {
    kind: ShapeKind.Square,
    sideLength: number,
}

let c: Circle = {
    kind: ShapeKind.Square, // error
    radius: 100,
};
```

> 另一个变化是枚举类型本身变成了每个枚举成员的联合。虽然我们还没有讨论 联合类型 。但你只要知道通过联合枚举，类型系统能够利用这样一个事实，它可以知道枚举里的值的集合。因此，`TypeScript` 能够捕获在比较值的时候犯的愚蠢的错误。例如：

```typescript
enum E {
    Foo,
    Bar,
}
function f(x: E){
    if(x !== E.Foo || x !== E.Bar) {
        // // Error! Operator '!==' cannot be applied to types 'E.Foo' and 'E.Bar'.
    }
}
```

> 这个例子里，我们先检查 `x` 是否不是 `E.Foo`。 如果通过了这个检查，然后 `||` 会发生短路效果，
> `if` 语句体里的内容会被执行。 然而，这个检查没有通过，那么 x 则只能为 `E.Foo`，因此没理由再去检查它是否为 `E.Bar`。

<a name="8d4339da"></a>

### 7.7 运行时的枚举

- 枚举是在运行时真正存在的对象。

```typescript
enum E { X, Y, Z }
```

- 可以传递给函数么？

```typescript
function f(obj: {X:number}) {
    return obj.X
}
```

- 没问题，因为 `"E"` 包含一个数值型属性 `"X"`

```typescript
f(E);
```

<a name="11d070c7"></a>

### 7.8 编译时的枚举

> 尽管一个枚举是在运行时真正存在的对象，但 `keyof` 关键字的行为与其作用在对象上时有所不同。应该使用 `keyof` `typeof` 来获取一个表示枚举里所有字符串 `key` 的类型。

```typescript
enum LogLevel {
    ERROR, WARN, INFO, DEBUG
}
```

- 等同于：

```typescript
type LogLevelStrings = 'ERROR' | 'WARN' | 'INFO' | 'DEBUG'; type LogLevelStrings = keyof typeof LogLevel;
```

```typescript
function printImportant(key: LogLevelStrings, message: string) {
    const num = LogLevel[key];
    if(num <= LogLevel.WARN) {
        console.log("Log Level key is: ", key);
        console.log("Log Level value is: ", num);
        console.log("Log Level message is ", message);
    }
}
printImportant("ERROR", "This is a message");
```

<a name="19bcfadd"></a>

### 7.9 反向映射

> 除了创建一个以属性名作为对象成员的对象外，数字枚举成员还具有了反向映射，从枚举值到枚举名字。

- 例如在下面的例子中：

```typescript
enum _Enum {
  A
}
let _a = _Enum.A;
let _nameOfA = _Enum[_a]; // "A"
```

- `TypeScript` 可能会将这段代码编译为下面的 `JavaScript`：

```typescript
var Enum;
(function (Enum) {
  Enum[Enum["A"] = 0] = "A";
})(Enum || (Enum = {}));
var a = Enum.A;
var nameOfA = Enum[a]; // "A"
```

> 生成的代码中，枚举类型被编译成一个对象，它包含了正向映射（`name -> value`）和反向映射（`value -> name`）。

> 引用枚举成员总会生成为对属性访问并且永远也不会内联代码。

> 要注意的是不会为字符串枚举成员生成反向映射。

<a name="fb69a593"></a>

### 7.10 const 枚举

> 大多数情况下，枚举是十分有效的方案。然而在某些情况下需求很严格。为了避免在额外生成的代码上的开销和额外的非直接的对枚举成员的访问，我们可以使用 `const` 枚举。

> 常量枚举通过在枚举上使用 `const` 修饰符来定义。

```typescript
const enum Enum {
    A = 1,
    B = A * 2,
}
```

> 常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除。常量枚举成员在使用的地方会被内联进来。 之所以可以这么做是因为，常量枚举不允许包含计算成员。

```typescript
const enum Directions {
    Up,
    Down,
    Left,
    Right,
}
let directions = [Directions.Up, Directions.Down, Directions.Left, Directions.Right];
// 生成后的代码为：
var directions1 = [0 /* Up */, 1 /* Down */, 2 /* Left */, 3 /* Right */];
```

<a name="3698efa0"></a>

### 7.11 外部枚举

> 外部枚举 用来描述已经存在的枚举类型的形状。

```typescript
declare enum Enum {
    A = 1,
    B,
    C = 2,
}
```

> 外部枚举 和 非外部枚举 之间有一个重要的区别，在正常的枚举里，没有初始化方法的成员被当成常量成员。

> 对于非常量的外部枚举而言，没有初始化方法时被当做需要经过计算的。

<a name="02247d28"></a>

## 八、类型别名

<a name="184d67c0"></a>

### 8.1 类型别名

1. 创建别名需要使用关键字 `type`
2. 使用别名通常用在有 联合类型 的场景下

> 注意：不要混淆了 `TypeScript` 中的 `=>`; 和 `ES6` 中的 `=>` 在 `TypeScript` 的类型定义中，`=>` 用来表示函数的定义。(左边是输入类型，需要用括号扩起来，右边是输出类型)；在 `ES6` 中，`=>` 叫做箭头函数。

```typescript
type Name = string;
type ShowName = () => string;
type NameOrShowName = Name | ShowName;

const getName = (name: NameOrShowName) => {
    if(typeof name === 'string') {
        return name;
    }
    return name();
};

let showName = () => 'pr is a boy';
console.log(getName("pr"));
console.log(getName(showName()));
```

<a name="660804ff"></a>

### 8.2 字符串字面量类型

```typescript
type EventNames = 'click' | 'scroll' | 'mousemove';
const handleEvent: (a: Element, b: EventNames) => string = (ele: Element, event: EventNames) => {
    return `${ele} ${event}`
}
handleEvent(document.getElementById('header'), 'scroll');
handleEvent(document.getElementById('footer'), 'click');
```

<a name="c4652a90"></a>

## 九、类型推论

<a name="7ad4decf"></a>

### 9.1 基础

> 在 `TypeScript` 里，在有些没有明确指出类型的地方，类型推论会帮助提供类型。如下面的例子：

```typescript
let x = 3;
```

> 变量 `x` 的类型被推断为数字。这种推断发生在初始化变量和成员，设置默认参数和决定函数返回值时。

> 大多数情况下，类型推论是直接了当地。

<a name="4b2ff140"></a>

### 9.2 最佳通用类型

> 当需要从几个表达式中推断类型的时候，会使用这些表达式的类型来推断出一个最合适的通用类型。

```typescript
let x = [0, 1, null];
```

> 为了推断 `x` 的类型，我们必须考虑所有元素的类型。这里有两种选择: `number` 和 `null`。计算通用类型算法会考虑所有的候选类型，并给出一个兼容所有候选类型的类型。

> 由于最终的通用类型取自候选类型，有些时候候选类型共享相同的通用类型，但是却没有一个类型能做为所有候选类型的类型。

```typescript
let zoo = [new Rhino(), new Elephant(), new Snake()];
```

> 这里，我们想让 `zoo` 被推断为 `Animal[]` 类型，但是这个数组里没有对象是 `Animal` 类型的，因此不能推断出这个结果。

> 为了更正，当候选类型不能使用的时候我们需要明确的指出类型：

```typescript
let zoo: Animal[] = [new Rhino(), new Elephant(), new Snake()];
```

> 如果没有找到最佳通用类型的话，类型推断的结果为联合数组类型，`(Rhino | Elephant | Snake)[]`。

<a name="18512c90"></a>

### 9.3 上下文归类

1. `TypeScript` 类型推论也可能按照相反的方向进行，这被叫做"上下文归类"。按上下文归类会发生在表达式类型与所处的位置相关时。

> 在下面这个例子里，`TypeScript` 类型检查器会使用 `Window.onmousedown` 函数的类型来推断右边函数表达式的类型。所以它能够推断出 `mouseEvent` 参数的类型中包含了 `button` 属性而不包含 `kangaroo` 属性。

```typescript
window.onmousedown = function (mouseEvent) {
    console.log(mouseEvent.button); // <- Ok
    console.log(mouseEvent.kangaroo); // <- error
};
```

2. `TypeScript` 还能够很好地推断出其它上下文中的类型。

```typescript
window.onscroll = function(uiEvent) {
    console.log(uiEvent.button); // Error
};
```

> 上面的函数被赋给 `window.onscroll``，TypeScript` 能够知道 `uiEvent` 是 `UIEvent`，而不是 `MouseEvent。UIEvent` 对象不包含 `button` 属性，因此 `TypeScript` 会报错。

1. 如果这个函数不是在上下文归类的位置上，那么这个函数的参数类型将隐式的成为 `any` 类型，而且也不会报错(除非你开启了 `--noImplicitAny` 选项)

```typescript
const handler = function(uiEvent) {
    console.log(uiEvent.button); // <- Ok
};
```

- 我们也可以明确地为函数参数类型赋值来覆写上下文类型：

```typescript
window.onscroll = function (uiEvent: any) {
    console.log(uiEvent.button); // <- Now, no error is given
};
```

> 但这段代码会打印 `undefined`，因为 `uiEvent` 并不包含 `button` 属性。

> 上下文归类会在很多情况下使用到。通常包含函数的参数，赋值表达式的右边，类型断言，对象成员和数组字面量和返回值语句。

- 上下文类型也会做为最佳通用类型的候选类型。比如：

```typescript
function createZoo(): Animal[] {
  return [new Rhino(), new Elephant(), new Snake()];
}
```

> 上面这个例子里，最佳通用类型有 `4` 个候选者：`Animal, Rhino, Elephant, Snake`。当然，`Animal` 会被作为最佳通用类型。

<a name="c3bd22d8"></a>

## 十、类型兼容性

<a name="e7c49304"></a>

### 10.1 介绍

> `TypeScript` 里的兼容性是基于结构子类型的。结构类型是一种只使用其成员来描述类型的方式。它正好与名义( `nominal` )类型形成对比。(译者注：在基于名义类型的类型系统中，数据类型的兼容性或等价性是通过明确的声明/或类型的名称来决定的。这与结构性类型系统不同，它是基于类型的组成结构，且不要求明确地声明。)看下面的例子：

```typescript
interface Named {
    name: string;
}

class Person {
    name: string;
}

let p: Named;
// Ok, because of structural typing
p = new Person();
```

> 在使用基于名义类型的断言，比如 `C#` 或 `Java` 中，这段代码会报错， 因为 `Person` 类没有明确说明其实现了 `Named` 接口。
> `TypeScript` 的结构性子类型是根据 `JavaScript` 代码的典型写法来设计的。 因为 `JavaScript` 里广泛地使用匿名对象，例如函数表达式和对象字面量， 所以使用结构类型系统来描述这些类型比使用名义类型系统更好。

<a name="ee63b42d"></a>

### 10.2 关于可靠性的注意事项

> `TypeScript` 的类型系统允许某些在编译阶段无法确认其安全性的操作。当一个类型系统具此属性时，被当做是“不可靠”的。TypeScript 允许这种不可靠行为的发生是经过仔细考虑的。通过这篇文章，我们会解释什么时候会发生这种情况和其有利的一面。

<a name="917c9ed1"></a>

### 10.3 开始

- `TypeScript` 结构化类型系统的基本规则是，如果 `x` 要兼容 `y`，那么 `y` 至少具有与 `x` 相同的属性。比如：

```typescript
interface Named {
    name: string;
}

let x: Named;
// y's inferred type is { name: string; location: string }
let y = {name: "Alice", location: "Seattle"};
x = y;
```

> 这里要检查 `y` 是否能赋值给 `x`，编译器检查 `x` 中的每个属性，看是否能在 `y` 中也找到对应属性。

> 在这个例子中，`y` 必须包含名字是 `name` 的 `string` 类型成员。`y` 满足条件，因此赋值正确。

- 检查函数参数时使用相同的规则

```typescript
function greet(n: Named) {
    console.log("Hello, " + n.name);
}

greet(y); // OK
```

> 注意，`y` 有个额外的 `location` 属性，但这不会引发错误。
> 只有目标类型（这里是 `Named`）的成员会被一一检查是否兼容。
> 这个比较过程是递归进行的，检查每个成员及子成员。

<a name="b2a7841d"></a>

### 10.4 比较两个函数

1. 相对来讲，在比较原始类型和对象类型的时候是比较容易理解的，问题是如何判断两个函数是兼容的。

- 下面我们从两个简单的函数入手，它们仅是参数列表略有不同：

```typescript
let x = (a: number) => 0;
let y = (b: number, s: string) => 0;

y = x; // <- Ok
// x = y; // <- Error
```

> 要查看 `x` 是否能赋值给 `y`，首先看它们的参数列表。 `x` 的每个参数必须能在 `y` 里找到对应类型的参数。注意的是参数的名字相同与否无所谓，只看它们的类型。 这里，`x` 的每个参数在 `y` 中都能找到对应的参数，所以允许赋值。第二个赋值错误，因为 `y` 有个必需的第二个参数，但是 `x` 并没有，所以不允许赋值。
> 你可能会疑惑为什么允许忽略参数，像例子 `y = x` 中那样。 原因是忽略额外的参数在 `JavaScript` 里是很常见的。
> 例如，`Array.forEach` 给回调函数传 `3` 个参数：数组元素，索引 和 整个数组。 尽管如此，传入一个只使用第一个参数的回调函数也是很有用的：

```typescript
let items = [1, 2, 3];
// Don't force these extra arguments
items.forEach((item, index, array) => console.log(item));
// Should be Ok
items.forEach(item => console.log(item));
```

2. 下面来看看如何处理返回值类型，创建两个仅是返回值类型不同的函数。

```typescript
let x1 = () => ({name: "Alice"});
let y1 = () => ({name: "Alice", location: "Seattle"});

x1 = y1; // Ok
y1 = x1; // Error, because x() lacks a location property
```

3. 类型系统强制源函数的返回值类型必须是目标函数返回值类型的子类型。

<a name="9dbccb20"></a>

### 10.5 函数参数双向协变

> 当比较函数参数类型时，只有当源函数参数能够赋值给目标函数或者反过来时才能赋值成功。这是不稳定的，因为调用者可能传入了一个具有更精确类型信息的函数，但是调用这个传入的函数的时候却使用了不是那么精确的类型信息。实际上，这极少会发声错误，并且能够实现很多 `JavaScript` 里的常见模式。

- 例如：

```typescript
enum EventType {Mouse, Keyboard}

interface Event {
    timestamp: number;
}

interface MouseEvent extends Event {
    x1: number;
    y1: number
}

interface keyEvent extends Event {
    keyCode: number;
}

function listenEvent(eventType: EventType, handler: (n: Event) => void) {
    /* …… */
}

// Unsound, but useful and common 不完整，但是常用常见
listenEvent(EventType.Mouse, (e: MouseEvent) => console.log(`${e.x1},${e.y1}`));

// Undesirable alternatives in presence of soundness 完整但是不受欢迎
listenEvent(EventType.Mouse, (e: Event) => console.log(`${(<MouseEvent>e).x1},${(<MouseEvent>e).y1}`));

listenEvent(EventType.Mouse, <(e: Event) => void>((e: MouseEvent) => console.log(`${e.x1},${e.y1}`)));
// Still disallowed (clear error). Type safety enforced for wholly incompatible types
// 仍然不允许（清除错误）。完全不兼容类型所强制规定的类型安全
// listenEvent(EventType.Mouse, (e: number) => console.log(e));
```

<a name="540171b4"></a>

### 10.6 可选参数和剩余参数

1. 比较函数兼容性的时候，可选参数与必须参数是可互换的。源类型上有额外的可选参数不是错误，目标类型的可选参数在源类型里没有对应的参数也不是错误。
2. 当一个函数有剩余参数时，它被当做无限个可选参数。
3. 这对于类型系统来说是不稳定的，但从运行时的角度来看，可选参数一般来说是不强制的，因为对于大多数函数来说相当于传递了一些 undefined 。

> 有一个好的例子，常见的函数接收一个回调函数并用对于程序员来说是可预知的参数 但对类型系统来说是不确定的参数来调用：

```typescript
function invokeLater(args: any[], callback: (...args: any[]) => void) {
    /* ... Invoke callback with 'args' ... 通过args调用callback */
}

// Unsound - invokeLater "might" provide any number of arguments
// 不健全的是 - 被调用时候可能会提供任意数量的参数
invokeLater([1, 2], (x, y) => console.log(x + "," + y));

// Confusing (x and y are actually required) and unrecoverable
// 令我们困惑(是否需要 x 和 y) 并且 不可恢复
invokeLater([1, 2], (x?, y?) => console.log(x + "," + y));
```

<a name="a6c7e468"></a>

### 10.7 函数重载

> 对于有重载的函数，源函数的每个重载都要在目标函数上找到对应的函数签名。这确保了目标函数可以在所有源函数可调用的地方调用。

<a name="ec380ec3"></a>

### 10.8 枚举

> 枚举类型与数字类型兼容，并且数字类型与枚举类型兼容。不同枚举类型之间是不兼容的。

- 比如：

```typescript
enum Status { Ready, Waiting }

enum Color { Red, Blue, Green }

let _status = Status.Ready;
// _status = Color.Green; // Error
```

<a name="1bfb6001"></a>

### 10.9 类

> 类与对象字面量和接口差不多，但是有一点不同：类的静态部分和实例部分的类型。比较两个类类型的对象时，只有实例的成员会被比较。静态成员和构造函数不在比较的范围内。

```typescript
class Animal {
    feet: number;

    constructor(name: string, numFeet: number, c: boolean) {
    }
}

class Size {
    feet: number;

    constructor(numFeet: number) {
    }
}

let a: Animal;
let s: Size;
a = s; //Ok
s = a; //Ok

/*  */
```

<a name="404e0617"></a>

### 10.10 类的私有和受保护成员

> 类的私有成员和受保护成员会影响兼容性。 当检查类实例的兼容时，如果目标类型包含一个私有成员，那么源类型必须包含来自同一个类的这个私有成员。同样地，这条规则也适用于包含受保护成员实例的类型检查。 这允许子类赋值给父类，但是不能赋值给其它有同样类型的类。

<a name="d15f908d"></a>

### 10.11 泛型

> 因为 `TypeScript` 是结构性的类型系统，类型参数只影响使用其做为类型一部分的结果类型。

- 比如

```typescript
interface Empty<T> {
}

let x: Empty<number>;
let y: Empty<string>;

x = y;// Ok, because y matches structure of x
```

> 上面代码里，`x` 和 `y` 是兼容的，因为它们的结构使用类型参数时，并没有什么不同。

- 把这个例子改变一下，增加一个成员，就能看出是如何工作的了。

```typescript
interface NotEmpty<T> {
    data: T
}

let x1: NotEmpty<number>;
let y1: NotEmpty<string>;
x1 = y1; // Error，because x and y are not compatible
```

> 在这里，泛型类型在使用时就好比不是一个泛型类型。

> 对于没指定泛型类型的泛型参数时，会把所有泛型参数当成 `any` 比较。然后用结果类型进行比较，就像上面第一个例子。

- 比如：

```typescript
let identity = function<T>(x: T): T {
  // ...
  return;
};
let reverse = function<U>(y: U): U {
  // ...
  return;
};
identity = reverse; // Ok, because (x: any) => any matches (y: any) => any
```

<a name="f340a9aa"></a>

### 10.12 高级主题

- 子类型和赋值

> 目前为止，我们使用了"兼容性"，它在语言规范里没有定义，在 `TypeScript` 里，有两种兼容性：子类型 和 赋值。它们的不同点在于：赋值扩展了子类型兼容性，增加了一些规则，允许和 `any` 来回赋值，以及 enum 和对应数字值之间的来回赋值。
> 语言里的不同地方分别使用了它们之中的机制。实际上，类型兼容性是由赋值兼容性来控制的，即使在 `implements` 和 `extends` 语句也不例外。

<a name="b38970c5"></a>

## 十一、高级类型

<a name="43502200"></a>

### 11.1 交叉类型

> 交叉类型 是将多个类型合并为一个类型。这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。例如，`Person & Serializable & Loggable` 同时是 `Person` 和 `Serializable` 和 `Loggable`。就是说这个类型的对象同时拥有了这三种类型的成员。
> 我们大多数是在混入( `mixins` )或其它不适合典型面向对象模型的地方看到交叉类型的使用。(在 `JavaScript` 里发生这种情况的场合很多！)

<a name="f5d38cb9"></a>

#### 11.1.1 `ReadOnly` 的实现

```typescript
type ReadOnly<T> = {
    readOnly [P in keyof T]: T[P];
}
```

<a name="ab97727b"></a>

#### 11.1.2 `Partial` 的实现

```typescript
type Partial<T> = {
    [P in keyof T]?: T[P]
}
```

<a name="316b007c"></a>

#### 11.1.3 `Pick` 的实现

```typescript
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
}
```

<a name="1e098569"></a>

#### 11.1.4 `Record` 的实现

```typescript
type Record<K extends string, T> = {
    [P in K]: T;
}
```

> 下面是如何创建混入的一个简单的例子(`"target": "es5"`):

```typescript
function extend<First, Second>(first: First, second: Second): First & Second {
    const result: Partial<First & Second>= {};
    for (const prop in first) {
        if (first.hasOwnProperty(prop)) {
            (<First>result)[prop] = first[prop];
        }
    }
    for (const prop in second) {
        if (second.hasOwnProperty(prop)) {
            (<Second>result)[prop] = second[prop];
        }
    }
    return <First & Second>result;
}
>
class Person {
    constructor(public name: string) {
    }
}
interface Loggable {
    log(name: string): void;
}
class ConsoleLogger implements Loggable {
    log(name) {
        console.log(`Hello, I'm ${name}`)
    }
}
const jim = extend(new Person("Jim"), ConsoleLogger.prototype);
let c = jim.log(jim.name);
console.log(c);
```

<a name="313c1801"></a>

### 11.2 联合类型

> 联合类型 和 交叉类型 很有关联，但是使用上却完全不同。偶尔你会遇到这种情况，一个代码库希望传入 `number` 或 `string` 类型的参数。例如下面的函数：

```typescript
/**
 * Takes a string and adds "padding" to the left.
 * If 'padding' is a string, then 'padding' is appended to the left side.
 * If 'padding' is a number, then that number of spaces is added to the left side.
 */
function padLeft(value: string, padding: any) {
    if (typeof padding === 'number') {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === 'string') {
        return padding + value;
    }
    throw new Error(`Expected string or number, got '${padding}'.`)
}

padLeft("Hello World", 4);
```

> `padLeft` 存在一个问题，`padding` 参数的类型指定成了 `any`。这就是说我们可以传入一个既不是 `number` 也不是 `string` 类型的参数，但是 `TypeScript` 却不会报错。
>
> ```typescript
> ```

let indentedString = padLeft("Hello World", true); // 编译阶段通过，运行时报错。

    >  在传统的面向对象语言里，我们可能会将这两种类型抽象成有层级的类型。这么做显然是非常清晰的，但同时也存在了过度设计。`padLeft` 原始版本的好处之一就是允许我们传入原始类型。这样做的话使用起来既简单又方便。如果我们就是想使用已经存在的函数的话，这种新的方式就不适用了。代替 `any`，我们可以使用 联合类型 作为 `padding` 的参数。


    ```typescript
    /**
     * Takes a string and adds "padding" to the left.
     * If 'padding' is a string, then 'padding' is appended to the left side.
     * If 'padding' is a number, then that number of spaces is added to the left side.
     */
    function padLeft1(value: string, padding: string | number) {
        // ...
    }

    // let indentedString1 = padLeft1("Hello world", true); // errors during compilation

> 联合类型 表示一个值可以是几种类型之一。我们使用竖线(`|`)分隔每个类型，所以 `number | string | boolean` 表示一个值可能是 `number`, `string`, `boolean`

```typescript
interface Bird {
    fly();

    lagEggs();
}

interface Fish {
    swim();

    layEggs();
}

function getSmallPet(): Fish | Bird {
    // ...
}
let pet = getSmallPet();
pet.layEggs(); // <- ok
// pet.swim(); // <- errors
```

> 这里的联合类型可能有点复杂，但是你很容易就习惯了。如果一个值的类型是 `A | B`，我们能够确定的是它包含了 `A` 和 `B` 中共有的成员。这个例子里，`Bird` 具有一个 `fly` 成员。我们不能确定一个 `Bird | Fish` 类型的变量是否有 `fly` 方法。如果变量在运行时是 `Fish` 类型，那么调用 `pet.fly()` 就出错了。

<a name="66bc1a64"></a>

### 11.3 类型守卫与类型区分

> 联合类型适用于那些值可以为不同类型的情况。但当我们想确切地了解是否为 `Fish` 时怎么办？`JavaScript` 里常用来区分 `2` 个可能值的方法是检查成员是否存在。如之前提及的，我们只能访问联合类型中共同拥有的成员。

```typescript
interface Fish { swim(); layEggs(); }
interface Bird { fly(); layEggs(); }
function getSmallPet(): Fish | Bird {
    return;
}
let pet = getSmallPet();
```

> 每一个访问的成员都会报错

```typescript
if(pet.swim) {
    pet.swim();
} else if(pet.fly) {
    pet.fly();
}
```

> 为了让这段代码工作，我们需要使用类型断言

```typescript
if((<Fish>pet).swim) {
    (<Fish>pet).swim();
} else if((< Bird>pet).fly) {
    (<Bird>pet).fly();
}
```

<a name="01c7e9f2"></a>

### 11.4 用户自定义的类型守卫

> 这里我们注意到不得不多次使用类型断言。假若我们一旦检查过类型，就能在之后的每个分支里清楚地知道 `pet` 的类型的话就好了。

> `TypeScript` 里的类型守卫机制让它成为了现实。类型守卫就是一些表达式，它们会在运行时检查以确保在某个作用域里的类型。要想定义一个类型守卫，我们只需要简单地定义一个函数，它的返回值是一个类型谓词：

```typescript
interface Fish {
    swim();

    layEggs();
}

interface Bird {
    fly();

    layEggs();
}

function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim !== undefined;
}
```

> 在这个例子里，`pet is Fish` 就是类型谓词。谓词为 `parameterName is Type` 这种形式， `parameterName` 必须是来自于当前函数签名里的一个参数名。
> 每当使用一些变量调用 `isFish` `时，TypeScript` 会将变量缩减为那个具体的类型，只要这个类型与变量的原始类型是兼容的
> `swim` 和 `fly` 的调用都没有问题了

```typescript
if(isFish(pet)) {
    pet.swim();
} else {
    pet.fly();
}
```

> 注意 `TypeScript` 不仅知道在 `if` 分支里 `pet` 是 `Fish` 类型；它还清楚在 `else` 分之里，一定不是 `Fish` 类型，一定是 Bird 类型。

<a name="4223f923"></a>

### 11.5 typeof 类型守卫

> 现在我们回过头来看看怎么使用联合类型书写 `padLeft` 代码。 我们可以像下面这样利用类型断言来写：

```typescript
function isNumber(x: any): x is number {
    return typeof x === "number";
}

function isString(x: any): x is string {
    return typeof x === "string";
}

function padLeft(value: string, padding: string | number) {
    if (isNumber(padding)) {
        return Array(padding + 1).join(" ") + value;
    }
    if (isString(padding)) {
        return padding + value;
    }
    throw new Error(`Expected string or number, go ${padding}.`);
}
```

> 然而，必须要定义一个函数来判断类型是否是原始类型，这太痛苦了。幸运的是，现在我们不必将 `typeof x === "number"` 抽象成一个函数，因为 `TypeScript` 可以将它识别为一个类型守卫。也就是说，我们可以直接在代码里检查类型了。

```typescript
function newPadLeft(value: string, padding: string | number) {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    }
    throw new Error(`Expected string or number, go ${padding}.`);
}
```

> 这些 `typeof` 类型守卫只有两种形式能被识别：`typeof v === "typename"` 和 `typeof v !== "typename"`, `"typename"` 必须是 `"number"`, `"string"`, `"boolean"` 或 `"symbol"`。但是 `TypeScript` 并不会阻止你与其它字符串比较，语言不会把那些表达式识别为类型守卫。

<a name="fe616cc0"></a>

### 11.6 instanceof 类型守卫

> `instanceof` 类型守卫是通过构造函数来细化类型的一种方式。比如，我们借鉴一下之前字符串填充的例子：

```typescript
interface Padder {
    getPaddingString(): string
}

class SpaceRepeatingPadder implements Padder {
    constructor(private numSpaces: number) {
    }

    getPaddingString() {
        return Array(this.numSpaces + 1).join(" ");
    }
}

class StringPadder implements Padder {
    constructor(private value: string) {
    }

    getPaddingString() {
        return this.value;
    }
}

function getRandomPadder() {
    return Math.random() < 0.5 ?
        new SpaceRepeatingPadder(4) :
        new StringPadder(" ")
}

// 类型为 SpaceRepeatingPadder | StringPadder
let padder: Padder = getRandomPadder();

if (padder instanceof SpaceRepeatingPadder) {
    padder; // 类型细化为 'SpaceRepeatingPadder'
}
if (padder instanceof StringPadder) {
    padder; // 类型细化为 'StringPadder'
}
```

> `instanceof` 的右侧要求是一个构造函数，`TypeScript` 将细化为:

1. 此构造函数的 `prototype` 属性的类型，如果它的类型不为 `any` 的话
2. 标签签名所返回的类型的联合

> 以此顺序。

<a name="2645976f"></a>

### 11.7 可为 null 的类型

> `TypeScript` 具有两种特殊的类型，`null` 和 `undefined` ，它们分别具有值 `null` 和 `undefined`。默认情况下，类型检查器会认为 `null` 和 `undefined` 可以赋值给任何类型。`null` 和 `undefined` 是所有其它类型的一个有效值。这意味着，你阻止不了将它们赋值给其它类型，就算是你想要阻止这种情况也不行。

> `--strictNullChecks` 标记可以解决此错误：当你声明一个变量时，它不会自动地包含 `null` 或 `undefined`。

- 你可以使用联合类型明确的包含它们：\`\`\`typescript
  let s = "foo";
  s = null; // --strictNullChecks模式下 错误， 'null' 不能赋值给 'string'
  let sn: string | null = "bar";
  sn = null; // Ok
  sn = undefined; // --strictNullChecks模式下 error, 因为'undefined'不能赋值给'string | null'

````


>  注意：按照 `JavaScript` `的语义，TypeScript` 会把 `null` 和 `undefined` 区别对待。


> `string | null`，`string | undefined` 和 `string | undefined | null` 是不同的类型。


<a name="b1f3ffa5"></a>
### 11.8 可选参数和可选属性

- 使用了 `--strictNullChecks` ，可选参数会被自动地加上 `| undefined`:

```typescript
function f(x: number, y?: number) {
    return x + (y || 0);
}
f(1, 2);
f(1);
f(1, undefined);
f(1, null); // error, 'null' is not assignable to 'number | undefined'
````

- 可选属性也会有同样的处理

```typescript
class C {
    a: number;
    b?: number;
}
let c = new C();
c.a = 12;
c.a = undefined; // error, 'undefined' is not assignable to 'number'
c.b = 13;
c.b = undefined; // Ok
c.b = null; // error, 'null' is not assignable to 'number | undefined'
```

<a name="aab6e77c"></a>

### 11.9 类型守卫和类型断言

> 由于可以为 `null` 的类型是通过联合类型实现，那么你需要使用类型守卫来去除 `null`。幸运地是这与在 `JavaScript` 里写的代码一致：

```typescript
function f(sn: string | null): string {
    if(sn == null) {
        return "default";
    } else {
        return sn;
    }
}
```

> 这里很明显地去除了 `null`, 你也可以使用短路运算符:

```typescript
function f1(sn: string | null): string {
    return sn || "default";
}
```

> 如果编译器不能够去除 `null` 或 `undefined`，你可以使用类型断言手动去除。语法是添加 `!` 后缀：`identifier!` 从 `identifier` 的类型里去除了 `null` 和 `undefined`：

```typescript
function broken(name: string | null): string {
    function postfix(epihet: string) {
        return name.charAt(0) + '. the' + epihet; // error, 'name' is possibly null
    }
    name = name || "Bob";
    return postfix("great");
}

function fixed(name: string | null): string {
    function postfix(epihet: string) {
        return name!.charAt(0) + '. the' + epihet; // Ok
    }
    name = name || "Bob";
    return postfix("great");
}
```

> 本例使用了嵌套函数，因为编译器无法去除嵌套函数的 `null` (除非是立即调用的函数表达式)。因为它无法跟踪所有对嵌套函数的调用，尤其是你将内层函数作为外层函数的返回值。如果无法知道函数在哪里被调用，就无法知道调用时 `name` 的类型。

<a name="cb08a7ca"></a>

### 11.10 类型别名

1. 类型别名会给一个类型起个新名字。类型别名有时和接口很像，但是可以作用于 原始值，联合类型，元组 以及其它任何你需要手写的类型。

```typescript
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;

function getName(n: NameOrResolver): Name {
    if (typeof n === 'string') {
        return n;
    } else {
        return n();
    }
}
```

2. 起别名不会新建一个类型 - 它创建了一个新名字来引用那个类型。

3.给原始类型起别名通常没有什么用，尽管可以作为文档的一种形式使用。

4. 同接口一样，类型别名也可以是泛型 - 我们可以添加类型参数并且在别名声明的右侧传入。

```typescript
type Container<T> = { value: T };
```

5. 我们也可以使用类型别名来在属性里引用自己：

```typescript
type Tree<T>= {
    value: T;
    left: Tree<T>;
    right: Tree<T>;
}
```

6. 与交叉类型一起使用，我们可以创建出一些十分稀奇古怪的类型

```typescript
type LinkedList<T> = T & { next: LinkedList<T> };

interface Person {
    name: string;
}

var people: LinkedList<Person>;
var s = people.name;
var s = people.next.name;
var s = people.next.next.name;
var s = people.next.next.next.name;
```

> 然而，类型别名不能出现在声明右侧的任何地方

```typescript
// type Yikes = Array<Yikes>; // error
```

<a name="8df349d9"></a>

### 11.11 接口 vs 类型别名

> 像我们提到的，类型别名可以像接口一样；然而，仍有一些细微差别。

1. 接口创建了一个新的名字，可以在其它任何地方使用。类型别名并不创建新名字 - 比如，错误信息就不会使用别名。

> 在下面的示例代码里，在编译器中将鼠标悬停在 `interfaced` 上，显示它返回的是 `Interface`，但悬停在 `aliased` 上时，显示的却是对象字面量类型。

```typescript
type Alias = { num: number };

interface Interface {
    num: number;
}

declare function aliased(arg: Alias): Alias;

declare function interfaced(arg: Interface): Interface;
```

2. 另一个重要区别是类型别名不能被 `extends` 和 `implements` (自己也不能 `extends` 和 `implements` 其它类型)。因为 软件中的对象应该对于扩展是开放的，但是对于修改是封闭的，你应该尽量去使用接口代替类型别名。
3. 另一方面，如果你无法通过接口来描述一个类型并且需要使用 联合类型 或 元组类型，这时通常会使用类型别名。

<a name="0f475581"></a>

### 11.12 字符串字面量类型

> 字符串字面量类型允许你指定字符串必须的固定值。在实际应用中，字符串字面量类型可以与联合类型，类型守卫和类型别名很好的配合。通过结合使用这些特性，你可以实现类似枚举类型的字符串。

```typescript
type Easing = "ease-in" | "ease-out" | "ease-in-out";

class UIElement {
    animate(dx: number, dy: number, easing: Easing) {
        if (easing === "ease-in") {
            // ...
        } else if (easing === "ease-out") {
            // ...
        } else if (easing === "ease-in-out") {
            // ...
        } else {
            // error! should not pass null or undefined.
        }
    }
}

let button = new UIElement();
button.animate(0, 0, "ease-in");
// button.animate(0, 0,"uneasy"); // error: "uneasy" is not allowed here
```

- 你只能从三种允许的字符中选择其一来作为参数传递，传入其它值则会产生错误。

```typescript
// Argument of type '"uneasy"' is not assignable to parameter of type '"ease-in" | "ease-out" | "ease-in-out"'
```

- 字符串字面量类型还可以用于区分函数重载

```typescript
function createElement(tagName: "img"): HTMLImageElement;
function createELement(tagName: "input"): HTMLInputElement;
// ... more overloads ...
function createElement(tagName: "string"): Element {
    // ... code goes here ...
}
```

<a name="33283dc2"></a>

### 11.13 数字字面量类型

- `TypeScript` 还具有数字字面量类型

```typescript
function rollDice(): 1 | 2 | 3 | 4 | 5 | 6 {
    // ...
}
```

- 我们很少直接这样使用，但它们可以用在缩小范围调试 `bug` 的时候。

```typescript
function foo(x: number) {
    if(x !== 1 || x !== 2) {
        // ~~~~~~
        // Operator '!==' cannot be applied to types '1' and '2'
    }
}
```

> 换句话说，当 `x` 与 `2` 进行比较的时候，它的值必须为 `1`，这就意味着上面的比较检查是非法的。

<a name="e2f0a7b6"></a>

### 11.14 枚举成员类型

> 如我们在枚举一节里提到的，当每个枚举成员都是用字面量初始化的时候枚举成员是具有类型的。在我们谈及“单例类型”的时候，多数是指 枚举成员类型 和 数字/字符串 字面量类型，尽管大多数用户会互换使用“单例类型”和“字面量类型”。

<a name="9ed54232"></a>

### 11.15 可辨识联合

> 你可以合并 单例类型，联合类型，类型守卫 和 类型别名 来创建一个叫做 可辨识联合 的高级模式，它也称作 标签联合 或 代数数据类型。可辨识联合 在函数式编程里很有用处。一些语言会自动地为你辨识联合；而 `TypeScript` 则基于已有的 `JavaScript` 模式，。它具有 `3` 个要素：

1. 具有普通的单例类型属性 - 可辨识的特征
2. 一个类型别名包含了那些类型的联合 - 联合。
3. 此属性上的类型守卫

```typescript
interface Square {
    kind: "square";
    size: number;
}

interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}

interface Circle {
    kind: "circle";
    radius: number;
}
```

> 首先我们声明了将要联合的接口。每个接口都有 `kind` 属性但有不同的字符串字面量类型。`kind` 属性称作 可辨识的特征或标签。其它的属性则特定于各个接口。注意，目前各个接口间是没有联系的。

- 下面我们将它们联合到一起。

```typescript
type Shape = Square | Rectangle | Circle;
```

- 现在我们使用可辨识联合

```typescript
function area(s: Shape) {
switch (s.kind) {
    case "square":
        return s.size * s.size;
    case "rectangle":
        return s.width * s.height;
    case "circle":
        return Math.PI * s.radius ** 2;
}
}
let areaSquare = area({kind: "square", size: 12});
let areaRectangle = area({kind: "rectangle", width: 14, height: 13});
let areaCircle = area({kind: "circle", radius: 5});
console.log(areaSquare, areaRectangle, areaCircle);
```

<a name="c681b741"></a>

### 11.16 完整性检查

> 当没有涵盖所有 可辨识联合 的变化时，我们想让编译器可以通知我们。比如，如果我们添加了 `Triangle` 和 `Shape`，我们同时还需要更新 `area`：

```typescript
interface Square {
    kind: "square",
    size: number
}

interface Rectangle {
    kind: "rectangle",
    width: number,
    height: number
}

interface Circle {
    kind: "circle",
    radius: number
}

// interface Triangle { kind: "triangle", bottom: number, height: number }

type Shape = Square | Rectangle | Circle;

function area(s: Shape) {
    switch (s.kind) {
        case "square":
            return s.size * s.size;
        case "rectangle":
            return s.width * s.height;
        case "circle":
            return Math.PI * s.radius ** 2;
    }
    // should error here - we didn't handle case "triangle"
}
```

- 有两种方式可以实现。

1. 首先是启用 `--strictNullChecks` 并且指定一个返回值类型:

```typescript
function area1(s: Shape): number {
 switch (s.kind) {
     case "square":
         return s.size * s.size;
     case "rectangle":
         return s.height * s.width;
     case "circle":
         return Math.PI * s.radius ** 2;
 }
}
```

> 因为 `switch` 没有包含所有情况，所以 `TypeScript` 认为这个函数有时候会返回 `undefined`。如果你明确地指定了返回值类型为 `number`，那么你会看到一个错误，因为实际上返回值类型为 `number | undefined`。然而，这种方法存在些微妙之处且 `--strictNullChecks` 对旧代码支持不好。

2. 第二种方法使用 `never` 类型，编译器用它来进行完整性检查

```typescript
function assertNever(x: never): never {
 throw new Error("Unexpected object: " + x);
}
>
function area2(s: Shape) {
 switch (s.kind) {
     case "square":
         return s.size * s.size;
     case "rectangle":
         return s.width * s.height;
     case "circle":
         return Math.PI * s.radius ** 2;
     default:
         return assertNever(s); // error here if there are missing cases
 }
}
```

> 这里，`assertNever` 检查 `s` 是否为 `never` 类型 - 即为除去所有可能情况后剩下的类型。如果你忘记了某个 `case`，那么 `s` 将具有一个真实的类型并且你会得到一个错误。这种方式需要你定义一个额外的函数，但是在你忘记某个 `case` 的时候也更加明显。

<a name="7ac7e947"></a>

### 11.17 多态的 this 类型

> 多态的 `this` 类型表示的是某个包含类或接口的子类型。这被称作是 `F-bounded` 多态性。它能很容易的表现连贯接口间的继承。

- 比如，在计算器的例子里，在每个操作之后都返回 `this` 类型。

```typescript
class BasicCalculator {
    public constructor(protected value: number = 0) {
    }

    public currentValue(): number {
        return this.value;
    }

    public add(operand: number): this {
        this.value += operand;
        return this;
    }

    public multiply(operand: number): this {
        this.value *= operand;
        return this;
    }

    //...other operations go here...
}

let v = new BasicCalculator(2)
    .multiply(5)
    .add(1)
    .currentValue();
console.log(v);
```

> 由于这个类使用了 `this` 类型，你可以继承它，新的类可以直接使用之前的方法，不需要做任何改变。

```typescript
class ScientificCalculator extends BasicCalculator {
    public constructor(value = 0) {
        super(value);
    }

    public sin() {
        this.value = Math.sin(this.value);
        return this;
    }

    // ... other operations go here ...
}

let v1 = new ScientificCalculator(2)
    .multiply(5)
    .sin()
    .add(1)
    .currentValue();
console.log(v1);
```

> 如果没有 `this` 类型，`ScientificCalculator` 就不能够在继承 `BasicCalculator` 的同时还保持接口的连贯性。`multiply` 将会返回 `BasicCalculator`，它并没有 `sin` 方法。然而，使用 `this` 类型，`multiply` 会返回 `this`，在这里就是 `ScientificCalculator`。

<a name="89f63e26"></a>

### 11.18 索引类型

> 使用索引类型，编译器就能够检查使用了动态属性名的代码。

- 例如，一个常见的 `JavaScript` 模式是从对象中选取属性的子集。

```typescript
function js_pluck(o, names) {
  return names.map(n => o[n]);
}
```

- 下面是如何在 `TypeScript` 里使用此函数，通过 索引类型查询和 索引访问 操作符：

```typescript
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
    return names.map(n => o[n]);
}

interface Person {
    name: string;
    age: number;
}

let person: Person = {
    name: "Jarid",
    age: 35,
};
let strings: string[] = pluck(person, ["name"]);
console.log(strings);
```

> 编译器会检查 `name` 是否真的是 `Person` 的一个属性。本例还引入了几个新的类型操作符。

<a name="269a1874"></a>

#### 11.18.1 首先是 `keyof T`，索引类型查询操作符。

> 对于任何类型 `T` ，`keyof T` 的结果为 `T` 上已知的公共属性名的联合。

- 例如：

```typescript
let personProps: keyof Person; // 'name' | 'age'
```

> `keyof Person` 是完全可以与 `'name' \| 'age'` 互相替换的。不同的是如果你添加了其它的属性到 `Person`, 例如 `address: string`，那么`keyof Person` 会自动变为 `'name' | 'age' | 'address'`。你可以像 `pluck` 函数这类上下文里使用 `keyof` ，因为在使用之前你并不清楚可能出现的属性名。但编译器会检查你是否传入了正确的属性名给 `pluck`：

```typescript
// pluck(person, ['age', 'unknown']); // error, 'unknown' is not in 'name' | 'age'
```

<a name="94f85507"></a>

#### 11.18.2 第二个操作符是 `T[K]`, 索引访问操作符。

> 在这里，类型语法反应了表达式语法。这意味着 `person['name']` 具有类型 `Person['name']` -- 在我们的例子里则为 `string` 类型。然而，就像索引类型查询一样，你可以在普通的上下文里使用 `T[K]`，这正是它的强大所在。你只要确保类型变量 `K extends keyof T` 就可以了。

- 例如下面 `getProperty` 函数的例子：

```typescript
function getProperty<T, K extends keyof T>(o: T, name: K): T[K] {
return o[name]; // o[name] is of type T[K]
}
```

> `getProperty` 里的 `o: T` 和 `name: K`，意味着 `o[name]: T[K]`。当你返回 `T[K]` 的结果，

> 编译器会是实例化键的真实类型，因此 `getProperty` 的返回值类型会随着你需要的属性改变。

```typescript
let _name: string = getProperty(person, "name");
let _age: number = getProperty(person, "age");
// let unknown = getProperty(person, "unknown"); // error, 'unknown' is not in 'name' | 'age'
console.log(_name, " ", _age);
```

<a name="d836d6c1"></a>

### 11.19 索引类型和字符串索引签名

> `keyof` 和 `T[K]` 与字符串索引签名进行交互。如果你有一个带有字符串索引签名的类型，那么 `keyof T` 会是 `string`。并且 `T[string]` 为索引签名的类型。

```typescript
interface Dictionary<T>{
    [key: string]: T
}
let keys: keyof Dictionary<number>; // string
let value: Dictionary<number>["foo"]; // number
```

<a name="fef370a3"></a>

### 11.20 映射类型

- 一个常见的任务是将一个已知的类型每个属性都变为可选的。

```typescript
interface PersonPartial {
    name?: string;
    age?: number;
}
```

- 或者我们只想要一个只读版本

```typescript
interface PersonReadonly {
    readonly name: string;
    readonly age: number;
}
```

> 这在 `JavaScript` 里经常出现。`TypeScript` 提供了从旧类型中创建新类型的一种方式 `--` 映射类型。在映射类型里，新类型以相同的形式去转换旧类型里每个属性。假如，你可以令每个属性成为 `readonly` 类型或可选的。

- 下面是一些例子：

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}
type Partial<T> = {
    [P in keyof T]?: T[P];
}
```

- 像下面这样使用

```typescript
interface Person { }
type PersonPartial = Partial; type PersonReadonly = Readonly;
```

> 需要注意的是，这个语法描述的是类型而非成员。若想添加额外的成员，则可以使用 交叉类型。

- 这样使用

```typescript
type PartialWithNewMember<T> = {
    [P in keyof T]?: T[P];
} & { newMember: boolean; }
```

- 不要这样使用

```typescript
type PartialWithNewMember<T> = {
  [P in keyof T]?: T[P];
  newMember: boolean;
}
```

> 下面来看看最简单的映射类型和它的组成部分

```typescript
type Keys = 'option1' | 'option2';
type Flags = { [K in Keys]: boolean; }
```

> 它的语法与索引签名的语法类型，内部使用了 `for...in`。具有三个部分：

1. 类型变量 `K`，它会依次绑定到每个属性。
2. 字符串字面量联合的 `Keys`，它包含了要迭代的属性名的集合。
3. 属性的结果类型。

> 在上面这个简单的例子里，`Keys` 是硬编码的属性列表并且属性类型永远是 `boolean`，因此这个映射类型

- 等同于：

```typescript
type _FLags = {
    option1: boolean;
    option2: boolean;
}
```

> 在真正的应用里，可能不同于上面的 `Readonly` 或 `Partial`。它们会基于一些已存在的类型，且按照一定的方式转换字段。这就是 `keyof` 和 `索引访问类型` 要做的事情。

```typescript
type NullablePerson = { [P in keyof Person]: Person[P] | null }
type PartialPerson = { [P in keyof Person]?: Person[P] }
```

> 但它更有用的地方是可以有一些通用版本。

```typescript
type Nullable<T> = { [P in keyof T]: T[P] | null }
type Partial1<T> = { [P in keyof T]?: T[P] }
```

> 在这些例子里，属性列表是 `keyof T` 且结果类型是 `T[P]` 的变体。这是使用通用映射类型的一个好模版。因为这类转换是同态的，映射只作用于 `T` 的属性而没有其它的。编译器知道在添加任何新属性之前可以拷贝所有存在的属性修饰符。假如，假设 `Person.name` 是只读的，那么 `Partial1<Person>.name` 也将是只读的且为可选的。

> 下面是另一个例子，`T[P]` 被包装在 `Proxy<T>` 类里：

```typescript
    type Proxy<T> = {
        get(): T;
        set(value: T): void;
    }
    type Proxify<T> = {
        [P in keyof T]: Proxy<T[P]>;
    }
    function proxify<T>(o: T): Proxify<T> {
        // ...wrap proxies
    }
    let proxyProps = proxify(props);
```

> 注意 `Readonly<T>` 和 `Partial<T>` 用处不小，因此它们于 `pick` 和 `Record` 一同被包含进了 `TypeScript` 的标准库里：

```typescript
type Pick<T, K extends keyof T>= {
    [P in K]: T[P]
}
type Record<K extends keyof any, T>= {
    [P in K]: T;
}
```

> `Readonly`, `Partial` 和 `Pick` 是同态的，但 `Record` 不是。因为 `Record` 并不需要输入类型来拷贝属性，所以它不属于同态：

```typescript
type ThreeStringProps = Record<'prop1' | 'prop2' | 'prop3', string>
```

> 非同态类型本质上会创建新的属性，因此它们不会从它处拷贝属性修饰符。

<a name="044a2658"></a>

### 11.21 由映射类型进行判断

> 现在你了解了如何包装一个类型的属性，那么接下来就是如何拆包。其实这也非常容易：

```typescript
type Proxify<T> = {
    get(): T;
    set(value: T): void;
}

function unproxify<T>(t: Proxify<T>): T {
    let result = {} as T;
    for (const k in t) {
        result[k] = t[k].get();
    }
    return result;
}
// let originalProps = unproxify(proxyProps);
```

> 注意这个拆包推断只适用于同态的映射类型。如果映射类型不是同态的，那么需要给拆包函数一个明确的类型参数。

<a name="5323833e"></a>

### 11.22 有条件类型

> `TypeScript 2.8` 引入了有条件类型，它能够表示非统一的类型。有条件的类型会以一个条件表达式进行类型关系检测，从而在两种类型中选择其一：

```typescript
T extends U ? X : Y
```

> 上面的类型意思是：若 `T` 能够赋值给 `U`，那么类型是 `X`，否则为 `Y`

> 有条件的类型 `T extends U ? X : Y` 或者解析为 `X`，或者解析为 `Y`，再或者延迟解析，因为它可能依赖一个或多个类型变量。若 `T` 或 `U` 包含类型参数，那么是否解析为 `X` 或 `Y` 或推迟，取决于类型系统是否有足够的信息来确定 `T` 总是可以赋值给 `U`。

- 下面是一些类型可以被立即解析的例子：

```typescript
declare function f<T extends boolean>(x: T): T extends true ? string : number;

// Type is 'string' | 'number'
let x = f(Math.random() < 0.5);
```

> 另外一个例子涉及 `TypeName` 类型别名，它使用了嵌套了有条件类型

```typescript
type TypeName<T> =
    T extends string ? "string" :
        T extends number ? "number" :
            T extends boolean ? "boolean" :
                T extends undefined ? "undefined" :
                    T extends Function ? "function" : "object";

type T0 = TypeName<string>; // "string"
type T1 = TypeName<"a">; // "string"
type T2 = TypeName<true>; // "boolean"
type T3 = TypeName<() => void>; // "function"
type T4 = TypeName<string[]>; // "object"
```

- 下面是一个有条件类型被推迟解析的例子

```typescript
interface Foo {
    propA: boolean;
    propB: boolean;
}
declare function f<T>(x: T): T extends Foo ? string : number;
function foo<U>(x: U) {
    // Has type 'U extends Foo ? string : number'
    let a = f(x);
    // This assignment is allowed though!
    let b: string | number = a;
}
```

> 这里，`a` 变量含有未确定的有条件类型。当有另一段代码调用 `foo`，它会用其它类型替换 `U`，`TypeScript` 将重新计算有条件类型，决定它是否可以选择一个分支。与此同时，我们可以将有条件类型赋值给其它类型，只要有条件类型的每个分支，都可以赋值给目标类型。因此在我们的例子里，我们可以将 `U extends Foo ? string : number` 赋值给 `string | number`，因为不管这个有条件类型最终结果是什么，它只能是 `string | number`。

<a name="1a7b1a41"></a>

### 11.23 分布式有条件类型

> 如果有条件类型里待检查的类型是 `naked type parameter` ，那么它也被称为"分布式有条件类型"。分布式有条件类型在实例化时会自动分发成联合类型。例如：实例化 `T extends U ? X : Y`，`T` 的类型为 `A | B | C`，会被解析为 `(A extends U ? X : Y) | (B extends U ? X : Y) | (C extends U ? X : Y)`。

- 例子：\`\`\`typescript
  type TypeName<T> =
  T extends string ? "string" :
  T extends number ? "number" :
  T extends boolean ? "boolean" :
  T extends undefined ? "undefined" :
  T extends Function ? "function" : "object";
  type T10 = TypeName\<string | (() => void)>; // "string" | "function"
  type T11 = TypeName\<string | string\[] | undefined>; // "string" | "object" | "undefined"
  type T12 = TypeName\<string\[] | number\[]>; // "object"

````


>  在 `T extends U ? X : Y` 的实例化里，对 `T` 的引用被解析为联合类型的一部分(比如，`T` 指向某一单个部分，在有条件类型分布到联合类型之后)。此外，在 `X` 内对 `T` 的引用有一个附加的类型参数约束 `U`(例如，`T` 被当成在 `X` 内可赋值给 `U`)。


- 例子：

```typescript
type BoxedValue<T> = { value: T };
type BoxedArray<T> = { array: T[] };
type Boxed<T> = T extends any[] ? BoxedArray<T[number]> : BoxedValue<T>;

type T20 = Boxed<string>; // BoxedValue<string>;
type T21 = Boxed<number[]>; // BoxedArray<number>;
type T22 = Boxed<string | number[]>; // BoxedValue<string> | BoxedArray<number>;
````

<a name="582039f4"></a>

#### 11.23.1 有条件类型的分布式的属性可以方便地用来过滤联合类型：

```typescript
type Diff<T, U> = T extends U ? never : T; // Remove types from T that are assignable to U(从 T 中删除可分配给 U 的类型)
type Filter<T, U> = T extends U ? T : never; // Remove types from T that are not assignable to U(从 T 中删除不可赋值给 U 的类型)

type T30 = Diff<"a" | "b" | "c" | "d", "a" | "c" | "f">; // "b" | "d"
type T31 = Filter<"a" | "b" | "c" | "d", "a" | "c" | "f">; // "a" | "c"
type T32 = Diff<string | number | (() => void), Function>; // string | number
type T33 = Filter<string | number | (() => void), Function>; // () => void

type _NonNullable<T> = Diff<T, null | undefined>; // Remove null and undefined from T

type T34 = _NonNullable<string | number | undefined>; // string | number
type T35 = _NonNullable<string | string[] | null | undefined>; // string | string[]

function f1<T>(x: T, y: _NonNullable<T>) {
    x = y; // Ok
    // y = x ;// Error
}
function f2<T extends string | undefined>(x: T, y: _NonNullable<T>) {
    x = y; // Ok
    // y = x; //Error
    // let s1: string = x; // Error
    let s2: string = y; // Ok
}
```

<a name="24724157"></a>

#### 11.23.2 有条件类型与映射类型结合时特别有用

```typescript
type FunctionPropertyNames<T> = {[K in keyof T]: T[K] extends Function ? K : never}[keyof T];
type FunctionProperties<T> = Pick<T, FunctionPropertyNames<T>>;

type NonFunctionPropertyNames<T> = {[K in keyof T]: T[K] extends Function ? never : K}[keyof T];
type NonFunctionProperties<T> = Pick<T, NonFunctionPropertyNames<T>>;

interface Part {
    id: number;
    name: string;
    subparts: Part[];
    updatePart(newName: string): void;
}

type T40 = FunctionPropertyNames<Part>; // "updatePart"
type T41 = NonFunctionPropertyNames<Part>; // "id" | "name" | "subparts"
type T42 = FunctionProperties<Part>; // { updatePart(newName: string): void }
type T43 = NonFunctionProperties<Part>; // { id: number, name: string, suparts: Part[] }
```

<a name="93092da5"></a>

#### 11.23.3 与联合类型和可交叉类型相似，有条件类型不允许递归地引用自己。

- 比如下面的错误：

```typescript
// type ElementType<T> = T extends any[] ? ElementType<T[number]>: T; // Error
```

<a name="01fe1300"></a>

### 11.24 有条件类型中的类型推断

> 现在有条件类型的 `extends` 子语句中，允许出现 `infer` 声明，它会引入一个待推断的类型变量。这个推断的类型变量可以在有条件类型的 `true` 分支中被引用。允许出现多个同类型变量的 `infer`。

> 例如：下面代码会提取函数类型的返回值

```typescript
type _ReturnType<T> = T extends (...args: any[]) => infer R ? R : any;
```

> 有条件类型 可以嵌套来构成一系列的匹配模式，按顺序进行求值：

```typescript
type Unpacked<T> =
    T extends (infer U)[] ? U :
        T extends (...args: any[]) => infer U ? U :
            T extends Promise<infer U> ? U :
                T;

type T0 = Unpacked<string>; // string
type T1 = Unpacked<string[]>; // string
type T2 = Unpacked<() => string>; // string
type T3 = Unpacked<Promise<string>>; // string
type T4 = Unpacked<Promise<string>[]>; // Promise<string>
type T5 = Unpacked<Unpacked<Promise<string>[]>>; // string
```

> 下面的例子解释了在协变位置上，同一个类型变量的多个候选类型会被推断为联合类型：

```typescript
type Foo<T> = T extends { a: infer U, b: infer U } ? U : never;
type T10 = Foo<{ a: string, b: string }>; // string
type T11 = Foo<{ a: string, b: number }>; // string | number
```

> 相似地，在抗变位置上同一个类型变量的多个候选类型会被推断为交叉类型：
>
> ```typescript
> ```

type Bar<T> = T extends { a: (x: infer U) => void, b: (x: infer U) => void } ? U : never;
type T20 = Bar<{ a: (x: string) => void, b: (x: string) => void }>; // string
type T21 = Bar<{ a: (x: string) => void, b: (x: number) => void }>; // string & number

````


> 当推断具有多个调用签名(例如函数重载类型)的类型时，用最后的签名(大概是最自由的包含所有情况的签名) 进行推断。无法根据参数类型列表来进行解析重载。


```typescript
declare function foo(x: string): number;
declare function foo(x: number): string;
declare function foo(x: string | number): string | number;

type T30 = ReturnType<typeof foo>; // string | number
````

> 无法在正常类型参数的约束子语句中使用 `infer` 声明：

```typescript
// type ReturnType1<T extends (...args: any[]) => infer R> = R; // Error
```

> 但是，可以这样达到同样的效果，在约束里删掉类型变量，用 有条件类型 来替换：

```typescript
type AnyFunction = (...args: any[]) => any;
type ReturnType2<T extends AnyFunction> =
    T extends (...args: any[]) => infer R ? R : any;
```

> 实际上，就是 `ReturnType`

```typescript
type __ReturnType<T extends (...args: any) => any> =
    T extends (...args: any) => infer R ? R : never;
```

<a name="be7aba91"></a>

### 11.25 预定义的有条件类型

> `TypeScript 2.8` 在 `lib.d.ts` 里增加了一些预定义的有条件类型

> 1. `Exclude<T, U>` -- 从 `T` 中剔除可以赋值给 `U` 的类型
> 2. `Extract<T, U>` -- 提取 `T` 中可以赋值给 `U` 的类型
> 3. `NonNullable<T>` -- 从 `T` 中剔除 `null` 和 `undefined`
> 4. `ReturnType<T>` -- 获取函数返回值类型
> 5. `InstanceType<T>` -- 获取构造函数类型的实例类型

- `Example`:

```typescript
type T00 = Exclude<"a" | "b" | "c" | "d", "a" | "c" | "f">; // "b" | "d"
type T01 = Extract<"a" | "b" | "c" | "d", "a" | "c" | "f">; // "a" | "c"

type T02 = Exclude<string | number | (() => void), Function>; // "string" | "number"
type T03 = Extract<string | number | (() => void), Function>; // () => void

type T04 = NonNullable<string | number | undefined>; // string | number
type T05 = NonNullable<(() => string) | string[] | null | undefined>; // (() => string) | string[]

function f1(s: string) {
    return { a: 1, b: s }
}

class C {
    x = 0;
    y = 0;
}

type T10 = ReturnType<() => string>; // string
type T11 = ReturnType<(s: string) => void>; // void
type T12 = ReturnType<(<T>() => T)>; // {}
type T13 = ReturnType<(<T extends U, U extends number[]>() => T)>; // number[]
type T14 = ReturnType<typeof f1>; // { a: number, b: string }
type T15 = ReturnType<any>; // any
type T16 = ReturnType<never>; // never
// type T17 = ReturnType<string>; // Error
// type T18 = ReturnType<Function>; // Error

type T20 = InstanceType<typeof C>; // C
type T21 = InstanceType<any>; // any
type T22 = InstanceType<never>; // never
// type T23 = InstanceType<string>; // Error
// type T24 = InstanceType<Function>; // Error
```

> 注意：`Exclude` 类型是建议的 `Diff` 类型的一种实现，我们使用 `Exclude` 这个名字是为了避免破坏已经定义了的 `Diff` 的代码，并且我们感觉这个名字能更好地表达类型的语义。我们没有增加 `Omit<T, K>` 类型，因为它可以很容易的用 `Pick<T, Exclude<keyof T, K>>` 来表示

<a name="1ec49334"></a>

## 十二、实用工具类型

> `TypeScript` 提供一些工具类型来帮助常见的类型转换。这些类型是全局可见的。

<a name="f20a1a75"></a>

### 12.1 Partial 局部的

> 构造类型 `T`, 并将它所有的属性设置成可选的，它的返回类型表示输入类型的所有子类型。

- 例子：

```typescript
interface PartialTodo {
    title: string;
    description: string;
}

function updateTodo(todo: PartialTodo, fieldsToUpdate: Partial<PartialTodo>) {
    return {...todo, ...fieldsToUpdate}
}

const partialTodo1 = {
    title: 'organize desk',
    description: 'clear clutter'
};

const partialTodo2 = updateTodo(partialTodo1, {
    description: 'throw out trash'
});
```

<a name="361c6538"></a>

### 12.2 Readonly 只读的

> 构造类型 T**，并将它所有的属性设置为** readonly**，也就是说构造出的类型的属性不能被再次赋值**

- 例子：

```typescript
interface ReadonlyTodo {
    title: string;
}

const readonlyTodo: Readonly<ReadonlyTodo> = {
    title: 'Delete inactive users'
};

readonlyTodo.title = 'Hello'; // Error: cannot reassign a readonly property

// Object.freeze
function freeze<T>(obj: T): Readonly<T>;
```

<a name="e88ab836"></a>

### 12.3 Record\<T, K> 记录

> 构造一个类型，其属性名的类型为 `K`，属性值的类型为 `T`。这个工具可用来将某个类型的属性映射到另一个类型上。

- 例子：

```typescript
interface PageInfo {
    title: string;
}

type Page = 'home' | 'about' | 'contact';

const x: Record<Page, PageInfo> = {
    about: {title: 'about'},
    contact: {title: 'concat'},
    home: {title: 'home'}
};
```

<a name="9648f3d0"></a>

### 12.4 Pick\<T, K> 挑选

> 从类型 `T` 中挑选部分属性 `K` 来构造类型

```typescript
interface PickTodo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Pick<PickTodo, 'title' | 'completed'>

const pickTodo: TodoPreview = {
    title: 'Clean room',
    completed: false,
};
```

<a name="98b0c13f"></a>

### 12.5 Exclude\<T, U> 剔除

> 从类型 `T` 中剔除所有可以赋值给 `U` 的属性，然后构造一个类型。

- 例子：

```typescript
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // "c"
type T2 = Exclude<string | number | (() => void), Function>; // string | number
```

<a name="22cd24be"></a>

### 12.6 Extract\<T, U> 提取

> 从类型 `T` 中提取所有可以赋值给 `U` 的类型，然后构造一个类型。

```typescript
type T01 = Extract<"a" | "b" | "c", "a" | "f">; // "a"
type T02 = Extract<string | number | (() => void), Function>; // () => void
```

<a name="211a3f63"></a>

### 12.7 NonNullable

> 从类型 `T` 中剔除 `null` 和 `undefined`，然后构造一个类型

- 例子：

```typescript
type T10 = NonNullable<string | number | undefined>; // string | number
type T11 = NonNullable<string[] | null | undefined>; // string[]
```

<a name="3be91cd5"></a>

### 12.8 ReturnType 返回值类型

> 由函数类型 `T` 的返回值类型构造一个类型

- 例子：

```typescript
function f1(s: string) {
    return {a: 1, b: s}
}

type _T0 = ReturnType<() => string>; // string
type _T1 = ReturnType<(s: string) => void>; // void
type _T2 = ReturnType<(<T>() => T)>; // {}
type _T3 = ReturnType<(<T extends U, U extends number[]>() => T)>; // number[]
type _T4 = ReturnType<typeof f1>; // { a: number, b: string }
type _T5 = ReturnType<any>; // any
type _T6 = ReturnType<never>; // never
type _T7 = ReturnType<string>; // Error
type _T8 = ReturnType<Function>; // Error
```

<a name="ab8b802e"></a>

### 12.9 InstanceType 构造函数 实例类型

> 由构造函数类型 `T` 的实例类型构造一个类型。

- 例子：

```typescript
class C {
    x = 0;
    y = 0;
}

type _T01 = InstanceType<typeof C>; // C
type _T02 = InstanceType<any>; // any
type _T03 = InstanceType<never>; // never
type _T04 = InstanceType<string>; // Error
type _T05 = InstanceType<Function>; // Error
```

<a name="f7fecab1"></a>

### 12.10 Required

> 构造一个类型，使类型 `T` 的所有属性为 `required`

- 例子：

```typescript
interface Props {
    a?: number;
    b?: string;
}

const obj: Props = {a: 5}; // Ok
const obj2: Required<Props> = { a: 5 }; // Error, property 'b' missing
```

<a name="7a98afe5"></a>

### 12.11 ThisType this

> 这个工具不会返回一个转换后的类型。它作为上下文的 `this` 类型的一个标记。
> 注意：若想要使用此类型，必须启用 `--noImplicitThis`

- 例子：

```typescript
// Compile with --noImplicitThis
type ObjectDescriptor<D, M> = {
    data?: D;
    methods?: M & ThisType<D & M>; // Type of 'this' in methods is D & M
}

function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
    let data: object = desc.data || {};
    let methods: object = desc.methods || {};
    return {...data, ...methods} as D & M;
}

let _obj = makeObject({
    data: {x: 0, y: 0},
    methods: {
        moveBy(dx: number, dy: number) {
            this.x += dx; // Strongly typed this
            this.y += dy; // Strongly typed this
        }
    }
});
_obj.x = 10;
_obj.y = 10;
_obj.moveBy(5, 5);
```

> 上面例子中，`makeObject` 参数里的 `methods` 对象具有一个上下文类型 `ThisType<D & M>`，因此 `methods` 对象的方法里 this 的类型为 `{ x: number, y: number } & { moveBy(dx: number, dy: number): number }`。
> 在 `lib.d.ts` 里，`ThisType<T>` 标识接口是个简单的空接口声明。除了在被识别为对象字面量的上下文类型之外，这个接口与一般的空接口没有什么不同。

<a name="dfe65560"></a>

## 十三、Symbols

> 介绍：自 `ECMAScript 2015` 起，`symbol` 成为了一种新的原生类型，就像 `number` 和 `string` 一样。

<a name="90ec2a20"></a>

### 13.1 `symbol` 类型的值是通过 `Symbol` 构造函数创建的。

```typescript
let sym1 = Symbol();
let sym2 = Symbol("key"); // 可选的字符串 key
```

<a name="93ca9317"></a>

### 13.2 `Symbols` 是不可改变且唯一的

```typescript
let sym3 = Symbol("key");
let sym4 = Symbol("key");
sym3 === sym4; // false symbols 是唯一的
```

<a name="8e96c998"></a>

### 13.3 像字符串一样，`symbols` 也可以被用作对象属性的键。

```typescript
const sym = Symbol();
let obj = {
    [sym]: "value"
};
console.log(obj[sym]); // "value"
```

<a name="5234c568"></a>

### 13.4 `Symbols` 也可以与计算出的属性名相结合来声明对象的属性和类成员。

```typescript
const getClassNameSymbol = Symbol();

class C {
    [getClassNameSymbol]() {
        return "C";
    }
}

let c = new C();
let className = c[getClassNameSymbol](); // "C"
```

<a name="c188a030"></a>

### 13.5 众所周知的 `Symbols`

> 除了用户定义的 `Symbols`，还有一些已经众所周知的内置 `symbols`。内置 `symbols` 用来表示语言内部的行为。
> 以下为这些 `symbols` 的列表：

- `Symbol.hasInstance`

> 该方法会被 `instanceof` 运算符调用。构造器对象用来识别一个对象是否是其实例。

- `Symbol.isConcatSpreadable`

> 布尔值，表示当在一个对象上调用 `Array.prototype.concat` 时，这个对象的数组元素是否可展开。

- `Symbol.iterator`

> 方法，被 `for-of` 语句调用，。返回对象的默认迭代器

- `Symbol.match`

> 方法，被 `String.prototype.match` 调用。正则表达式用来匹配字符串

- `Symbol.replace`

> 方法，被 `String.prototype.replac`e 调用。正则表达式用来替换字符串中匹配的子串。

- `Symbol.search`

> 方法，被 `String.prototype.search` 调用。正则表达式返回被匹配部分在字符串中的索引。

- `Symbol.species`

> 函数值，为一个构造函数。用来创建派生对象。

- `Symbol.split`

> 方法，被 `String.prototype.split` 调用。正则表达式用来分割字符串。

- `Symbol.toPrimitive`

> 方法，被 `ToPrimitive` 抽象操作调用。把对象转换为相应的原始值。

- `Symbol.toStringTag`

> 方法，被内置方法 `Object.prototype.toString` 调用。返回创建对象时默认的字符串描述。

- `Symbol.unscopables`

> 对象，它自己拥有的属性会被 `with` 作用域排除在外。

<a name="67757bf1"></a>

## 十四、迭代器和生成器

- 可迭代性

> 当一个对象实现了 `Symbol.iterator` 属性时，我们认为它是可迭代的。一些内置的类型如 `Array`，`Map`，`Set`，`String`，`Int32Array`，`Uint32Array` 等都已经实现了各自的 `Symbol.iterator`。对象上的 `Symbol.iterator` 函数负责返回供迭代的值。

<a name="c2b50e3a"></a>

### 14.1 for ... of 语句

> `for ... of` 会遍历可迭代对象，调用对象上的 `Symbol.iterator` 方法。

- 下面是在数组上使用 `for ... of` 的简单例子

```typescript
let someArray = [1, "string", false];

for (let entry of someArray) {
    console.log(entry); // 1, "string", false
}
```

<a name="55c9b2cb"></a>

### 14.2 for ... of Vs for ... in 语句

> `for ... of` 和 `for ... in` 均可迭代一个列表；但是用于迭代的值却不同，`for ... in` 迭代的是对象的 `键` 的列表，而 `for ... of` 则迭代对象的键对应的值。

- 下面的例子展示了两者之间的区别：

```typescript
let list = [4, 5, 6];
for (let i in list) {
    console.log(i); // "0" "1" "2"
}
for (let i of list) {
    console.log(i); // "4" "5" "6"
}
```

> 另一个区别是 `for ... in` 可以操作任何对象；它提供了查看对象属性的一种方法。但是 `for ... of` 关注于迭代对象的值。内置对象 `Map` 和 `Set` 已经实现了 `Symbol.iterator` 方法，让我们可以访问它们保存的值。

```typescript
let pets = new Set(["Cat", "Dog", "Hamster"]);
pets["species"] = "mammals";
for (let pet in pets) {
    console.log(pet);
}
for (let pet of pets) {
    console.log(pet); // "Cat", "Dog", "Hamster"
}
```

<a name="c3655367"></a>

### 14.3 代码生成目标为 `ES5` 和 `ES3`

> 当生成目标为 `ES5` 或 `ES3`，迭代器只允许在 `Array` 类型上使用。在非数组值上使用 `for ... of` 语句会得到一个错误，就算这些非数组值已经实现了 `Symbol.iterator` 属性。

> 编译器会生成一个简单的 `for` 循环做为 `for ... of` 循环，比如：

```typescript
let numbers = [1, 2, 3];
for(let num of numbers) {
  console.log(num);
}
// 生成的代码为：
var number = [1, 2, 3];
for(var _i = 0; _i < numbers.length; _i++) {
  var num = numbers[_i];
  console.log(num);
}
```

<a name="714a4885"></a>

### 14.4 目标为 `ECMAScript 2015` 或更高

> 当目标为兼容 `ECMAScript 2015` 的引擎时，编译器会生成相应引擎的 `for ... of` 内置迭代器实现方式。

<a name="2dfe9cc1"></a>

## 十五、模块

<a name="11338c92"></a>

### 15.1 关于术语的一点说明

> 请务必注意一点：`TypeScript 1.5` 里术语名已经发生了变化。"`内部模块`" 现在被称作 "`命名空间`"。"`外部模块`" 现在则简称为 "`模块`"。这是为了与 `ECMAScript 2015` 里的术语保持已一致， (也就是说 `module X` { 相当于现在推荐的写法 `namespace X` })

<a name="9f322f1f"></a>

### 15.2 介绍

1. 从 `ECMAScript 2015` 开始，`JavaScript` 引入了模块的概念。`TypeScript` 也沿用这个概念。
2. 模块在其自身的作用域里执行，而不是在全局作用域；这意味着定义在一个模块里的变量，函数，类等等在模块外部是不可见的，除非你明确地使用 `export` 形式之一导出它们。相反，如果想使用其它模块导出的 `变量`，`函数`，`类`，`接口` 等的时候，你必须要导入它们，可以使用 `import` 形式之一。
3. 模块是自声明的；两个模块之间的关系是通过在文件级别上使用 `import` 和 `exports` 建立的。
4. 模块使用模块加载器去导入其它的模块。在运行时，模块加载器的作用是在执行此模块代码前去查找并执行这个模块的所有依赖。大家最熟知的 `JavaScript` 模块加载器是服务与 `Node.js` 的 `CommonJS` 和 服务于 `Web` 应用的 `Require.js`。
5. `TypeScript` 和 `ECMAScript 2015` 一样，任何包含顶级 `import` 或者 `export` 的文件都被当成一个模块。相反地，如果一个文件不带有顶级的 `import` 或者 `export` 声明，那么它的内容将被视为全局可见的 (因此对模块也是可见的)。

<a name="e660906a"></a>

### 15.3 导出

<a name="ae52d3de"></a>

#### 15.3.1. 导出声明

> 任何声明( 比如 `变量`，`函数`，`类`，`类型别名` 或 `接口` )都能够通过添加 `export` 关键字来导出。

```typescript
export interface StringValidator { 
    isAcceptable(s: string): boolean; 
}
export const numberRegexp = /^[0-9]+$/;
export class ZipCodeValidator implements StringValidator { 
    isAcceptable(s: string): boolean { 
        return s.length === 5 && numberRegexp.test(s); 
    } 
}
```

<a name="5c56b80f"></a>

#### 15.3.2 导出语句

> 导出语句很便利，因为我们可能需要对导出的部分重命名，所以上面的例子可以这样改写：

```typescript
class ZipCodeValidator1 implements StringValidator {
    isAcceptable(s: string): boolean {
        return s.length === 5 && numberRegexp.test(s);
    }
}
export { ZipCodeValidator1 };
export { ZipCodeValidator1 as mainValidator };
```

<a name="0110fd9e"></a>

#### 15.3.3 重新导出

> 我们经常会去扩展其它模块，并且只导出那个模块的那部分内容。

> 重新导出功能并不会在当前模块导入那个模块或定义一个新的局部变量。

```typescript
export class ParseIntBasedZipCodeValidator {
    isAcceptable(s: string) {
        return s.length === 5 && parseInt()
    }
};
// 导出原先的验证器但做了重命名
export { ZipCodeValidator as RegExpBasedZipCodeValidator } from "./ZipCodeValidator" ;
```

> 或者一个模块可以包裹多个模块，并把它们导出的内容联合在一起，通过语法：`export * from "module"`

```typescript
export * from "./StringValidator";
export * from "./LetterOnlyValidator";
export * from "./ZipCodeValidator";
```

<a name="f9527380"></a>

### 15.4 导入

> 模块的导入操作与导出一样简单，可以使用以下 `import` 形式之一来导入其它模块中的导出内容。

<a name="b4ce1054"></a>

#### 15.4.1 导入一个模块中的某个导出内容

```typescript
import { ZipCodeValidator } from './ZipCodeValidator';
let myValidator = new ZipCodeValidator();
```

> 可以对导入内容重命名

```typescript
import { ZipCodeValidator as ZCV } from "./ZipCodeValidator";
let myValidator = new ZCV();
```

<a name="1ee1e563"></a>

#### 15.4.2 将整个模块导入到一个变量，并通过它来访问模块的导出部分

```typescript
import * as validator from './ZipCodeValidator';
let myValidator = new validator.ZipCodeValidator();
```

<a name="b16692b8"></a>

#### 15.4.3 具有副作用的导入模块

> 尽管不推荐这么做，一些模块会设置一些全局状态供其它模块使用。这些模块可能没有任何的导出或用户根本

> 就不关注它的导出。使用下面的方法来导入这些模块。

```typescript
import './my-module.js';
```

<a name="ed445659"></a>

### 15.5 默认导出

> 每个模块都可以有一个 `default` 导出。默认导出使用 `default` 关键字标记；并且一个模块只能够有一个 `default` 导出。需要使用 一种特殊的导入形式来导入 `default` 导出。

> `default` 导出十分便利。比如，像 `JQuery` 这样的类库可能有一个默认导出 `jQuery` 或 `$`，并且我们基本上也会使用同样的名字 `jQuery` 或 `$` 导出 `JQuery`。

- `JQuery.d.ts`

```typescript
declare let $: JQuery;
export default $;
```

- `App.ts`

```typescript
import $ from "JQuery";
$("button.continue").html("Next step ...");
```

> 类和函数声明可以直接被标记为默认导出。标记为默认导出的类和函数的名字是可以省略的。

- `ZipCodeValidator.ts`

```typescript
export default class ZipCodeValidator {
  static numberRegExp = /^[0-9]+$/;
  isAcceptable(s: string) {
      return s.length === 5 && ZipCodeValidator.numberRegExp.test(s);
  }
}
```

- `Test.ts`

```typescript
import validator from './ZipCodeValidator';
let myValidator = new validator();
```

> `或者`

- `StaticZipCodeValidator.ts`

```typescript
const numberRegExp = /^[0-9]+$/;
export default function (s: string) {
    return s.length === 5 && numberRegExp.test(s);
}
```

- `Test.ts`

```typescript
import validate from './StaticZipCodeValidator';
let strings = ["Hello", "98052", "101"];
// use function validate
strings.forEach(s => {
    console.log(`"${s}" ${validate(s)} ? " matches" : " does not match"`);
});
```

> `default` 导出也可以是一个值

- `OneTwoThree.ts`

```typescript
export default "123";
```

- `Log.ts`

```typescript
import num from './OneTwoThree';
console.log(num); // 123
```

> `export =` 和 `import = require()`

1. `CommonJS` 和 `AMD` 的环境里都有一个 `exports` 变量，这个变量包含了一个模块的所有导出内容。
2. `CommonJS` 和 `AMD` 的 `exports` 都可以被赋值为一个对象，这种情况下其作用就类似于 es6 语法里的默认导出，即 `export default` 语法了。虽然作用相似，但是 `export default` 语法并不能兼容 `CommonJS` 和 `AMD` 的 `exports`。
3. 为了支持 `CommonJS` 和 `AMD` 的 `exports`，`TypeScript` 提供了 `export =` 语法。
4. `export =` 语法定义一个模块的导出对象。这里的对象一词指的是类，接口，命名空间，函数或枚举。
5. 若使用 `export =` 导出一个模块，则必须使用 `TypeScript` 的特定语法 `import module = require('module')` 来导入此模块。

- `ZipCodeValidator.ts`

```typescript
let numberRegExp = /^[0-9]+$/;

class ZipCodeValidator {
    isAcceptable(s: string) {
        return s.length === 5 && numberRegExp.test(s);
    }
}

export = ZipCodeValidator; // 重点
```

- `Test.ts`

```typescript
import zip = require("./ZipCodeValidator"); // 重点

let strings = ["Hello", "98502", "101"];
let validator = new zip();
strings.forEach(s => {
    console.log(`"${s}" - ${validator.isAcceptable(s) ? "matches" : "does not match"}`)
});
```

<a name="c9bf80e2"></a>

### 15.6 生成模块代码

> 根据编译时指定的模块目标参数，编译器会生成相应的供 `Node.js\(CommonJS\)，Require.js\(AMD\)`，`UMD`，`SystemJS` 或 `ECMAScript 2015 native module\(ES6\)` 模块加载系统使用的代码。

> 想要了解生成代码中 `define`，`require` 和 `register` 的意义，需要参考相应的模块加载器的文档。

- `SimpleModule.ts`

```typescript
import m = require("mod");
export let t = m.something + 1;
```

- `AMD / RequireJS SimpleModule.js`

```typescript
define(["require", "exports", "./mod"], function (require, exports, mod_1) {
  exports.t = mod_1.something + 1;
});
```

- `CommonJS / Node SimpleModule.js`

```typescript
import mod_1 = require("./mod");
exports.t = mod_1.something + 1;
```

- `UMD SimpleModule.js`

```typescript
(function (factory) {
  if (typeof module === "object" && typeof module.exports === "object") {
      let v = factory(require, exports);
      if (v !== undefined) module.exports = v;
  } else if (typeof define === "function" && define.amd) {
      define(["require", "exports", "./mod"], factory);
  }
})(function (require, exports) {
  let mod_1 = require("./mod");
  exports.t = mod_1.something + 1;
});
```

- `System SimpleModule.js`

```typescript
System.register(["./mod"], function(exports_1) {
  let mod_1;
  let t;
  return {
      setters: [
          function (mod_1_1) {
              mod_1 = mod_1_1;
          }
      ],
      execute: function() {
          exports_1("t", t = mod_1.something + 1);
      }
  }
});
```

- `Native ECMAScript 2015 modules SimpleModule.js`

```typescript
import { something } from "./mod";
export let t = something + 1;
```

<a name="9c147ce5"></a>

### 15.7 简单示例

> 下面我们来整理一下前面的验证器实现，每个模块只有一个命名的导出。

> 为了编译，我们必须要在命令行上指定一个模块目标。对于 `Node.js` 来说，使用 `--module commonjs`；对于 `Require.js` 来说，使用 `--module amd`。

- 比如：

```typescript
tsc --module commonjs Test.ts
```

> 编译完成后，每个模块会生成一个单独的 `.js` 文件。好比使用了 `reference` 标签，编译器会根据 `import` 语句编译相应的文件。

- `Validation.ts`

```typescript
export interface StringValidator {
  isAcceptable(s: string): boolean;
}
```

- `LetterOnlyValidator.ts`

```typescript
import { StringValidator } from './Validation';
const lettersRegExp = /^[A-Za-z]+$/;

export class LetterOnlyValidator implements StringValidator{
  isAcceptable(s: string): boolean {
      return lettersRegExp.test(s);
  }
}
```

- `ZipCodeValidator.ts`

```typescript
import { StringValidator } from './Validation';
const numberRegExp = /^[0-9]+$/;

export class ZipCodeValidator implements StringValidator{
  isAcceptable(s: string): boolean {
      return s.length === 5 && numberRegExp.test(s);
  }
}
```

- `Test.ts`

```typescript
import {StringValidator} from './Validation';
import {ZipCodeValidator} from './ZipCodeValidator';
import {LetterOnlyValidator} from './LetterOnlyValidator';

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: StringValidator; } = {};
validators["ZIP code"] = new ZipCodeValidator();
validators["Letters only"] = new LetterOnlyValidator();

// Show whether each string passed each validator
strings.forEach(s => {
  for (let name in validators) {
      console.log(`"${s}" - ${validators[name].isAcceptable(s) ? "matches" : "does not match"} ${name}`)
  }
});
```

<a name="4bdadecf"></a>

### 15.8 可选的模块加载和其它高级加载场景

1. 有时候，你只想在某种条件下才加载某个模块。在 `TypeScript` 里，使用下面的方式来实现它和其它的高级加载场景，我们可以直接调用模块加载器并且保证类型完全。
2. 编译器会检测是否每个模块都会在生成的 `JavaScript` 中用到。如果一个模块标识符只在类型注解部分使用，并且完全没有在表达式中使用时，就不会生成 `require` 这个模块的代码。省略掉没有用到的引用对性能提升是很有益的，并同时提供了选择性加载模块的能力。
3. 这种模式的核心是 `import id = require("...")` 语句可以让我们访问模块导出的类型。模块加载器会被动态调用(通过 `require`)，就像下面的 `if` 代码块里那样。它利用了省略引用的优化，所以模块只在被需要时加载。为了让这个模块工作，一定要注意 `import` 定义的标识符只能在表示类型处使用(不能在会转换成 `JavaScript` 的地方。)
4. 为了确保类型安全性，我们可以使用 `typeof` 关键字。`typeof` 关键字，当在表示类型的地方使用时，会得出一个类型值，这里就表示模块的类型。

- `示例 1`. `Node.js` 里的动态模块加载

```typescript
declare function require(moduleName: string): any;
import {ZipCodeValidator as Zip} from './ZipCodeValidator';
if (needZipValidator) {
    let ZipCodeValidator: typeof Zip = require('./ZipCodeValidator');
    let validator = new ZipCodeValidator();
    if (validator.isAcceptable("...")) {
        /!* ... *!/
    }
}
```

- `示例 2`. `require.js` 里的动态模块加载

```typescript
declare function require(moduleNames: string[], onLoad: (...args: any[]) => void): void;
import * as Zip from './ZipCodeValidator';
if(needZipValidator) {
    require(['./ZipCodeValidator'], (ZipCodeValidator: typeof Zip) => {
        let validator = new ZipCodeValidator.ZipCodeValidator();
        if(validator.isAcceptable("...")) {
            /!* ... *!/
        }
    })
}
```

- `示例 3`. `System.js` 里的动态模块加载

```typescript
declare const System: any;
import {ZipCodeValidator as Zip} from "./ZipCodeValidator";
if (needZipValidator) {
    System.import("./ZipCodeValidator").then((ZipCodeValidator: typeof Zip) => {
        var x = new ZipCodeValidator();
        if (x.isAcceptable("...")) {
            /!*...*!/
        }
    })
}
```

<a name="ecc37eab"></a>

### 15.9 使用其它的 JavaScript 库

> 要想描述非 `TypeScript` 编写的类库的类型，我们需要声明类库所暴露的 `API`。

> 我们叫它声明因为它不是'外部程序'的具体实现，它们通常是在 `.d.ts` 文件里定义的。类似于 `C/C++` 里的 `.h` 文件。

<a name="187e32d6"></a>

#### 15.9.1 外部模块

> 在 `Node.js` 里大部分工作是通过加载一个或多个模块实现的。我们可以使用顶级的 `export` 声明来为每个模块都定义一个 `.d.ts` 文件，但最好还是写在一个大的 `.d.ts` 文件里。我们使用与构造一个外部命名空间相似的方法，但是这里使用 `module` 关键字并且把名字用引号括起来，方便之后 `import`。

- `例如：`
- `node.d.ts(simplified except)`

```typescript
declare module "url" {
    export interface Url {
        protocol?: string;
        hostname?: string;
        pathname?: string;
    }

    export function parse(urlStr: string, parseQueryString?, slashesDenoteHost?): Url;
}

declare module "path" {
    export function normalize(p: string): string;
    export function join(...paths: any[]): string;
    export let sep: string;
}
```

> 现在我们可以 `/// <reference> node.d.ts` 并且使用 `import url = require('url')`；或 `import * as URl from "url"` 加载模块。\`

```typescript
/// <reference path="node.d.ts">
import * as URL from 'url';

let myUrl = URL.parse("http://www.typescriptlang.org");
```

<a name="cf19a535"></a>

#### 15.9.2 外部模块简写

> 假如你不想在使用一个新模块之前花时间去编写声明，你可以采用声明的简写形式以便能够快速使用它。

> `declarations.d.ts`

```typescript
declare module "hot-new-module";
```

- 简写模块里所有导出类型将是 `any`

```typescript
import x, { y } from "hot-new-module";
x(y);
```

<a name="ca52ca4d"></a>

#### 15.9.3 模块声明通配符

> 某些模块加载器如 `SystemJS` 和 `AMD` 支持导入非 `JavaScript` 内容。它们通常会使用一个前缀或后缀来表示特殊的加载语法，模块声明通配符可以用来表示这些情况。

```typescript
declare module "*!text" {
    const content: string;
    export default content;
}
```

- `some do it the other way around`

```typescript
declare module "json!*" {
  const value: any;
  export default value;
}
```

- 现在你可以就导入匹配 `"!text"` 或 `"json!"` 的内容了。

```typescript
import fileContent from "./xyz.txt!txt";
import data from "json!http://example.com/data.json";
console.log(data, fileContent);
```

<a name="26a1d78e"></a>

#### 15.9.4 `UMD` 模块

> 有些模块被设计成兼容多个模块加载器，或者不使用模块加载器(全局变量)。它们以 `UMD` 模块为代表。这些库可以通过导入的形式或全局变量的形式访问。

- `例如：`
- `math-lib.d.ts`

```typescript
export function isPrime(x: number): boolean;
export as namespace mathLib;
```

- 之后，这个库可以在某个模块里通过导入来使用:

```typescript
import { isPrime } from "math-lib";
isPrime(2);
mathLib.isPrime(2);
```

- 它同样可以通过全局变量的形式使用，但只能在某个脚本(指不带有模块导入或导出的脚本文件)里。

```typescript
mathLib.isPrime(2)
```

<a name="5fa7a16e"></a>

### 15.10 创建模块结构指导

<a name="c9ee722d"></a>

#### 15.10.1 尽可能地在顶层导出

1. 用户应该更容易地使用你模块导出的内容。嵌套层次过多会变得难以处理，因此仔细考虑一下如何组织你的代码。
2. 从你的模块中导出一个命名空间就是一个增加嵌套的例子。虽然命名空间有时候有它们的用处，在使用模块的时候它们额外地增加了一层。这对用户来说是很不方便的并且通常是多余的。
3. 导出类的静态方法也有同样的问题 - 这个类本身就增加了一层嵌套。除非它能方便地表述或便于清晰使用，否则请考虑直接导出一个辅助方法。

<a name="2bfd4b14"></a>

#### 15.10.2 如果仅导出单个 `class` 或 `function`，使用 `export default`

> 就像" `在顶层导出` "帮助减少用户使用的难度，一个默认的导出也能起到这个效果。如果一个模块就是为了导出特定的内容，那么你应该考虑使用一个默认导出。这会令模块的导入和使用变得些许简单，比如：

- `MyClass.ts`

```typescript
export default class SomeType {
    constructor() { /!* ... *!/ }
}
```

- `MyFunc.ts`

```typescript
export default function getThing() {
    return 'thing';
}
```

- `Consumer.ts`

```typescript
import t from './MyClass';
import f from './MyFunc';
let x = new t();
console.log(f());
```

> 对用户来说这是最理想的。他们可以随意命名导入模块的类型 (本例为 `t` ) 并且不需要多余的 ( `.` ) 来找到相关对象。

<a name="c34b9298"></a>

#### 15.10.3 如果要导出多个对象，把它们放在顶层里导出

- `MyThings.ts`

```typescript
export class SomeType {/!*...*!/}
export function someFunc() {
    /*...*/
}
```

<a name="8922dfd9"></a>

#### 15.10.4 (相反地，当导入的时候)明确地列出导入的名字。

- `Consumer.ts`

```typescript
import { SomeType, someFunc } from './MyThings';
let x = new SomeType();
let y = someFunc();
```

<a name="ff27525a"></a>

#### 15.10.5 使用命名空间导入模式当你要导出大量内容的时候

- `MyLargeModule.ts`

```typescript
export class Dog {}
export class Cat {}
export class Tree {}
export class Flower {}
```

- `Consumer.ts`

```typescript
import * as myLargeModule from './MyLargeModule.ts';
let x = new myLargeModule.Dog();
```

<a name="b0b9be21"></a>

#### 15.10.6 使用重新导出进行扩展

> 你可能经常需要扩展一个模块的功能。`JS` 里常用的一个模式是 `JQuery` 那样去扩展原对象。如我们之前提到的，模块不会像全局命名空间那样去合并。推荐的方案是 不要去改变原来的对象，而是导出一个新的实体来提供新的功能。

> 假设 `Calculator.ts` 模块里定义了一个简单的计算器实现。这个模块同样提供了一个辅助函数来测试计算器的功能，通过传入一系列输入的字符串并在最后给出结果。

- `Calculator.ts`

```typescript
export class Calculator {
    private current = 0;
    private memory = 0;
    private operator: string;

    protected static processDigit(digit: string, currentValue: number) {
        if (digit >= "0" && digit <= "9") {
            return currentValue * 10 + (digit.charCodeAt(0) - "0".charCodeAt(0));
        }
    }

    protected static processOperator(operator: string) {
        if (["+", "-", "*", "/"].indexOf(operator) >= 0) {
            return operator;
        }
    }

    protected evaluateOperator(operator: string, left: number, right: number): number {
        switch (this.operator) {
            case "+":
                return left + right;
            case "-":
                return left - right;
            case "*":
                return left * right;
            case "/":
                return left / right;
        }
    }

    private evaluate() {
        if (this.operator) {
            this.memory = this.evaluateOperator(this.operator, this.memory, this.current);
        } else {
            this.memory = this.current;
        }
        this.current = 0;
    }

    public handleChar(char: string) {
        if (char === "=") {
            this.evaluate();
            return;
        } else {
            let value = Calculator.processDigit(char, this.current);
            if (value !== undefined) {
                this.current = value;
                return;
            } else {
                let value = Calculator.processOperator(char);
                if (value !== undefined) {
                    this.evaluate();
                    this.operator = value;
                    return;
                }
            }
        }
        throw new Error(`Unsupported input: '${char}'`);
    }

    public getResult() {
        return this.memory;
    }
}

export function test(c: Calculator, input: string) {
    for (let i = 0; i < input.length; i++) {
        c.handleChar(input[i]);
    }
    console.log(`result of '${input}' is '${c.getResult()}'`);
}
```

- 下面使用导出的 `test` 函数来测试计算器
- `TestCalculator.ts`

```typescript
import { Calculator, test } from './Calculator';
let c = new Calculator();
test(c, "1+2*33/11="); // prints 9
```

- 现在扩展它，添加支持输入其它进制(十进制以外)。
- `ProgrammerCalculator.ts`

```typescript
import { Calculator } from './Calculator';
class ProgrammerCalculator extends Calculator {
    static digits = ["0", "1", "2", "3", "4", "5", "6", "7",
        "8", "9", "A", "B", "C", "D", "E", "F"];
    constructor(public base: number) {
        super();
        const maxBase = ProgrammerCalculator.digits.length;
        if (base <= 0 || base > maxBase) {
            throw new Error(`base has to be within 0 to ${maxBase} inclusive`);
        }
    }
    protected processDigit(digit: string, currentValue: number) {
        if (ProgrammerCalculator.digits.indexOf(digit) >= 0) {
            return currentValue * this.base + ProgrammerCalculator.digits.indexOf(digit);
        }
    }
}
export { ProgrammerCalculator as Calculator };
export { test } from "./Calculator";
```

> 新的 `ProgrammerCalculator` 模块导出的API与原先的 `Calculator` 模块很相似，但却没有改变原模块里的对象。

- `TestProgrammerCalculator.ts`

```typescript
import { Calculator, test } from './ProgrammerCalculator';
let c = new Calculator(2);
test(c, "001+010="); // prints 3
```

<a name="8b525970"></a>

#### 15.10.7 模块里不要使用命名空间

> 当初次进入基于模块的开发模式时，可能总会控制不住要将导出包裹在一个命名空间里。模块具有其自己的作用域，并且只有导出的声明才会在模块外部可见。记住这点，命名空间在使用模块时几乎没什么价值。

> 在组织方面，命名空间对于在全局作用域内对逻辑上相关的对象和类型分组是很便利的。例如，在 `C\#` 里，你会从 `System.Collections` 里找到所有集合的类型。通过将类型有层次地组织在命名空间里，可以方便用户找到与使用那些类型。然而，模块本身已经存在于文件系统之中，这是必须的。我们必须通过路径和文件名找到它们，这已经提供了一种逻辑上的组织形式。我们可以创建 `/collections/generic/` 文件夹，把相应模块放在这里。

> 命名空间对解决全局作用域里命名冲突来说是很重要的。比如，你有一个 `My.Application.Customer.AddFrom` 和 `My.Application.Order.AddForm` `--` 两个类型的名字相同，但命名空间不同。然而，这对于模块来说却不是一个问题。在一个模块里，没有理由两个对象拥有同一个名字。从模块的使用角度来说，使用者会挑出它们用来引用模块的名字，所以也没有理由发声重名的情况。

<a name="1425a6c3"></a>

#### 15.10.8 危险信号

> 以下均为模块结构上的危险信号。重新检查以确保你没有在对模块使用命名空间。

1. 文件的顶层声明是 `export namespace Foo {...}` ( 删除 `Foo` 并把所有内容向上层移动一层)
2. 文件只有一个 `export class` 或 `export function` (考虑使用 `export default`)
3. 多个文件的顶层具有同样的 `export namespace Foo` {( 不要以为这些会合并到一个 `Foo` 中! )}

<a name="380d0fdb"></a>

## 十六、命名空间

<a name="30562b06"></a>

### 16.1 介绍

> 这篇文章描述了如何在 `TypeScript` 里使用命名空间(之前叫做"内部模块")来组织你的代码。就像我们在术语说明里提到的那样，`"内部模块"` 现在叫做 `"命名空间"`。另外，任何使用 `module` 关键字来声明一个内部模块的地方都应该使用 `namespace` 关键字来替换。这就避免了让新的使用者被相似的名称所迷惑。

- `第一步`

> 我们先来写一段程序并将在整篇文章中都使用这个例子。我们定义几个简单的字符串验证器，

> 假设你会使用它们来验证表单里的用户输入或验证外部数据。

- `所有验证器都放在一个文件里`

```typescript
interface StringValidator {
    isAcceptable(s: string): boolean;
}

let letterRegExp = /^[A-Za-z]$/;
let numberRegExp = /^[0-9]+$/;

class LetterOnlyValidator implements StringValidator {
    isAcceptable(s: string): boolean {
        return letterRegExp.test(s);
    }
}

class ZipCodeValidator implements StringValidator {
    isAcceptable(s: string): boolean {
        return s.length === 5 && numberRegExp.test(s);
    }
}

let strings = ["Hello", "98052", "101"];
let validators: { [s: string]: StringValidator; } = {};
validators["ZIP code"] = new ZipCodeValidator();
validators["Letters Only"] = new LetterOnlyValidator();
// show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        let isMatch = validators[name].isAcceptable(s);
        console.log(`'${s}' ${isMatch ? "matches" : "does not match"} '${name}'.`)
    }
}
```

<a name="b3afce6f"></a>

### 16.2 命名空间

> 随着更多验证器的加入，我们需要一种手段来组织代码，以便于在记录它们类型的同时还不用担心与其它对象产生命名冲突。因此，我们把验证器包裹到一个命名空间里内，而不是把它们放在全局空间下。

> 下面的例子里，把所有与验证器相关的类型都放到一个叫做 `Validation` 的命名空间里。因为我们想让这些接口和类在命名空间之外也是可访问的，所以需要使用 `export`。相反的，变量 `LetterRegExp` 和 `numberRegExp` 是实现的细节，不需要导出，因此它们在命名空间外是不能访问的。在文件末尾的测试代码里，由于是在命名空间之外访问的，因此需要限定类型的名称，比如 `Validation.LettersOnlyValidator`。

- 使用命名空间的验证器

```typescript
namespace Validation {
  export interface StringValidator {
      isAcceptable(s: string): boolean;
  }

  const LetterRegExp = /^[A-Za-z]+$/;
  const numberRegExp = /^[0-9]+$/;

  export class LetterOnlyValidator implements StringValidator {
      isAcceptable(s: string): boolean {
          return LetterRegExp.test(s);
      }
  }

  export class ZipCodeValidator implements StringValidator {
      isAcceptable(s: string): boolean {
          return s.length === 5 && numberRegExp.test(s);
      }
  }
}
let strings = ["Hello", "98052", "101"];
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP Code"] = new Validation.ZipCodeValidator();
validators["Letters Only"] = new Validation.LetterOnlyValidator();
for (let s of strings) {
  for (let name in validators) {
      console.log(`'${s}' ${validators[name].isAcceptable(s) ? "matches" : "does not match"} '${name}'.`)
  }
}
```

<a name="58fed524"></a>

### 16.3 分离到多文件

> 当应用变得越来越大时，我们需要将代码分离到不同的文件中以方便维护。

<a name="10e70c65"></a>

#### 16.3.1 多文件中的命名空间

> 现在，我们把 `Validation` 命名空间分割成多个文件。尽管是不同的文件，它们仍是同一个命名空间，并且在使用的时候就如同它们在一个文件中定义的一样。因为不同文件之间存在依赖关系，所以我们加入了引用标签来告诉编译器文件之间的关联。

- `Validation.ts`

```typescript
namespace Validation {
  export interface StringValidator {
      isAcceptable(s: string): boolean;
  }
}
```

- `LetterOnlyValidator.ts`

```typescript
/// <reference path="Validation.ts" />;
namespace Validation {
  const lettersRegExp = /^[A-Za-z]+$/;
  export class LettersOnlyValidator implements StringValidator {
      isAcceptable(s: string): boolean {
          return lettersRegExp.test(s);
      }
  }
}
```

- `ZipCodeValidator.ts`

```typescript
/// <reference path="Validation.ts" />
namespace Validation {
    const numberRegExp = /^[0-9]+$/;
    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string): boolean {
            return s.length === 5 && numberRegExp.test(s);
        }
    }
}
```

- `Test.ts`

```typescript
/// <reference path="Validation.ts" />
/// <reference path="LettersOnlyValidator.ts" />
/// <reference path="ZipCodeValidator.ts" />

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```

> 当涉及到多文件时，我们必须确保所有编译后的代码都被加载了。我们有两种方式。

1. 第一种方式，把所有的输入文件编译为一个输出文件，需要使用 `--outFile` 标记： `tsc --outFile sample.js Test.ts`  编译器会根据源码里的引用标签自动地对输出进行排序。你也可以单独地指定每个文件。 `tsc --outFile sample.js Validation.ts LetterOnlyValidator.ts ZipCodeValidator.ts Test.ts`
2. 第二种方式，我们可以编写每一个文件(默认方式)，那么每个源文件都会对应生成一个 `JavaScript` 文件。然后，在页面上通过 `<script>` 标签把所有生成的 `JavaScript` 文件按正确的顺序引进来，比如：

```html
MyTestPage.html (excerpt)
<script src="Validation.js" type="text/javascript" />
<script src="LettersOnlyValidator.js" type="text/javascript" />
<script src="ZipCodeValidator.js" type="text/javascript" />
<script src="Test.js" type="text/javascript" />
```

<a name="122ae5a1"></a>

### 16.4 别名

> 另一种简化命名空间操作的方法是用 `import q = x.y.z` 给常用的对象起一个短的名字。不要与用来加载模块的 `import x = require('name')` 语法弄混了，这里的语法是为指定的符号创建一个别名。你可以用这种方法为任意标识符创建别名，也包括导入的模块中的对象。

```typescript
namespace Shapes {
    export namespace Polygons {
        export class Triangle { }
        export class Square { }
    }
}
import Polygons = Shapes.Polygons;
let sq = new Polygons.Square(); // Same as "new Shapes.Polygons.Square()"
```

> 注意：我们并没有使用 `require` 关键字，而是直接使用导入符号的限定名赋值。这与使用 `var` 相似，但它还适用于类型和导入的具有命名空间含义的符号。重要的是，对于值来讲，`import` 会生成与原始符号不同的引用，所以改变别名的 `var` 值并不会影响原始变量的值。

<a name="8806d13d"></a>

### 16.5 使用其它的 JavaScript 库

> 为了描述不是用 `TypeScript` 编写的类库的类型，我们需要声明类库导出的 `API`。由于大部分程序库只提供少数的顶级对象，命名空间是用来表示它们的一个好办法。我们称其为声明是因为它不是外部程序的具体实现。我们通常在 `.d.ts` 里写这些声明。如果你熟悉 `C/C+`+，你可以把它们当作 `.h` 文件。

> 外部命名空间 (`declare namespace =>`; 声明全局对象)

> 流行的程序库 `D3` 在全局对象 `d3` 里定义它的功能。因为这个库通过一个`<script>` 标签加载 (不是通过模块加载器)，它的声明文件使用内部模块来定义它的类型。为了让 `TypeScript` 编译器识别它的类型，我们使用外部命名空间声明。

- 比如，我们可以像下面这样写
- `D3.d.ts`

```typescript
declare namespace D3 {
  export interface Selectors {
      select: {
          (selector: string): Selection;
          (element: EventTarget): Selection;
      };
  }

  export interface Event {
      x: number;
      y: number;
  }

  export interface Base extends Selectors {
      event: Event;
  }
}
declare var d3: D3.Base;
```

<a name="71cb65fb"></a>

## 十七、命名空间和模块

<a name="936b9393"></a>

### 17.1 介绍

> 这篇文章将概括介绍在 `TypeScript` 里使用模块与命名空间来组织代码的方法。我们也会谈及命名空间和模块的高级使用场景，和在使用它们的过程中常见的陷阱。

<a name="684cda1b"></a>

### 17.2 使用命名空间

> 命名空间是位于全局命名空间下的一个普通的带有名字的 `JavaScript` 对象。这令命名空间十分容易使用。它们可以在多文件中同时使用，并通过 `--outFile` 结合在一起。命名空间是帮你组织 `Web` 应用不错的方式，你可以把所有依赖都放在 `HTML` 页面的 `<script>` 标签里。但就像其它的全局命名空间污染一样，它很难去识别组件之间的依赖关系，尤其是在大型应用中。

<a name="9989be36"></a>

### 17.3 使用模块

1. 像命名空间一样，模块可以包含代码和声明。不同的模块可以声明它的依赖。
2. 模块会把依赖添加到模块加载器上(像是 `CommonJS` / `RequireJS`)。对于小型的 JS 应用来说可能没必要，但是对于大型应用，这一点点的花费会带来长久的模块化和可维护性上的便利。模块也提供了更好的代码重用，更强的封闭性以及更好的使用工具进行优化。
3. 对于 `Node.js` 应用来说，模块是默认并推荐的组织代码的方式。
4. 从 `ECMAScript 2015` 开始，模块成为了语言内置的部分，应该会被所有正常的解释引擎所支持。

> 因此，对于新项目来说推荐使用模块作为组织代码的方式。

<a name="99a5e39f"></a>

### 17.4 命名空间和模块的陷阱

> 这部分我们会描述常见的命名空间和模块的使用陷阱和如何其避免它们。

<a name="8fac78ac"></a>

#### 17.4.1 对模块使用 `/// <reference>`

> 一个常见的错误是使用 `/// <reference>` 引用模块文件，应该使用 `import`。要理解这之间的差别，我们首先应该弄清编译器是如何根据 `import` 路径(例如：`import x from "..."`；或 `import x = require("...")` 里面的 `...`，等等)来定位模块的类型信息的。

> 编译器首先尝试去查找相应路径下的 `.ts`，`.tsx` 再或者 `.d.ts`。如果这些文件都找不到，编译器会查找外部模块声明。回想一下，它们是在 `.d.ts` 文件里声明的。

- `myModules.d.ts`

```typescript
// In a .d.ts file or .ts file that is not a module
declare module "SomeModule" {
    export function fn(): string;
}
```

- `myOtherModule.ts`

```typescript
/// <reference path="myModules.d.ts">
import * as m from "SomeModule";
```

> 这里的引用标签指定了外来模块的位置。这就是一些 `TypeScript` 例子中引用 `node.d.ts` 的方法。

<a name="53d926d7"></a>

#### 17.4.2 不必要的命名空间

> 如果你想把命名空间转换为模块，它可能会像下面这个文件一样。

- `shapes.ts`

```typescript
export namespace Shapes {
    export class Triangle {
        /* ... */
    }
    export class Square {
        /* ... */
    }
}
```

> 顶层的模块 `Shapes` 包裹了 `Triangle` 和 `Square`。对于使用它的人来说这是令人迷惑和讨厌的。

- `shapeConsumer.ts`

```typescript
import * as shapes from './shapes';
let t = new shapes.Shapes.Triangle(); // shapes.Shapes?
```

> `TypeScript` 里模块的一个特点是不同的模块永远也不会在相同的作用域内使用相同的名字。因为使用模块的人会为它们命名，所以完全没有必要把导出的符号包裹在一个命名空间里。再次重申，不应该对模块使用命名空间。使用命名空间是为了提供逻辑分组和避免命名冲突。模块文件本身已经是一个逻辑分组，并且它的名字是由导入这个模块的代码指定，所以没有必要为导出的对象增加额外的模块层。

- 下面是改进的例子：
- `shape.ts`

```typescript
export class Triangle { /* ... */}
export class Square {/* ... */}
```

- `shapeConsumer.ts`

```typescript
import * as shapes from "./shapes";
let t = new shapes.Triangle();
```

<a name="0dd89fe9"></a>

#### 17.4.3 模块的取舍

> 就像每个 `JS` 文件对应一个模块一样，`TypeScript` 里模块文件与生成的 `JS` 文件也是一一对应的。这会产生一种影响，根据你指定的目标模块系统的不同，你可能无法连接多个模块源文件。例如当目标模块系统为 `commonjs` 或 `umd` 时，无法使用 `outFile` 选项，但是 `TypeScript 1.8` 以上的版本能够使用 `outFile` 当目标为 `amd` 或 `system`。

<a name="ea22207b"></a>

## 十八、模块解析

<a name="4c2c35f2"></a>

### 18.1 说明

1. 模块解析是指编译器在查找导入模块内容时所遵循的流程。假设有一个导入语句 `import { a } from "moduleA"`; 为了去检查任何对 `a` 的使用，编译器需要准确的知道它表示什么，并且需要检查它的定义 `moduleA`。
2. 这时候，编译器会有个疑问“ `moduleA` 的结构是怎样的？” 这听上去很简单，但 moduleA 可能在你写的某个 .ts/.tsx 文件里或者在你的代码所依赖的 `.d.ts` 里。
3. 首先，编译器会尝试定位表示导入模块的文件。编译器会遵循以下二种策略之一： `Classic` 或 `Node`。这些策略会告诉编译器到哪里去查找 `moduleA`。
4. 如果上面的解析失败了并且模块名是非相对的（且是在" `moduleA` "的情况下），编译器会尝试定位一个外部模块声明。我们接下来会讲到非相对导入。

`5`. 最后，如果编译器还是不能解析这个模块，它会记录一个错误。 在这种情况下，错误可能为 `error TS2307: Cannot find module 'moduleA'.`

<a name="2c139a05"></a>

### 18.2 相对 vs. 非相对模块导入

> 根据模块引用是相对的还是非相对的，模块导入会以不同的方式解析。

1. 相对导入是以 `/，./` 或 `../` 开头的。 下面是一些例子：

```typescript
import Entry from "./components/Entry";
import { Default } from "../constants/http";
import "/mod";
```

2. 所有其它形式的导入被当作非相对的。下面是一些例子：

```typescript
import * as $ from "JQuery";
import { Component } from "@angular/core";
```

> 相对导入在解析时是相对于导入它的文件，并不能解析为一个外部模块声明。你应该为你自己写的模块使用相对导入，这样能确保它们在运行时的相对位置。

> 非相对模块的导入可以相对于 baseUrl 或 通过下文会讲到的路径映射来解析。它们还可以被解析成 外部模块声明。

> 使用非相对路径来导入你的外部依赖。

<a name="f5c8f754"></a>

### 18.3 模块解析策略

> 共有两种可用的模块解析策略：`Node` 和 `Classic`。 你可以使用 `--moduleResolution` 标记来指定使用哪种模块解析策略。若未指定，那么在使用了 -`-module AMD \| System \| ES2015` 时的默认值为 `Classic`，其它情况时则为 `Node`。

<a name="92b0f11e"></a>

#### 18.3.1 Classic

> 这种策略在以前是 `TypeScript` 默认的解析策略。 现在，它存在的理由主要是为了向后兼容。

> 相对导入的模块是相对于导入它的文件进行解析的。 因此 `/root/src/folder/A.ts` 文件里的 `import { b } from "./moduleB"` 会使用下面的查找流程：

1. /root/src/folder/moduleB.ts\`\`
2. /root/src/folder/moduleB.d.ts\`\`

> 对于非相对模块的导入，编译器则会从包含导入文件的目录开始依次向上级目录遍历，尝试定位匹配的声明文件。

- `比如：`

> 有一个对 `moduleB` 的非相对导入 `import { b } from "moduleB"`，它是在 `/root/src/folder/A.ts` 文件里，会以如下的方式来定位"`moduleB`"：

> 1. `/root/src/folder/moduleB.ts`
> 2. `/root/src/folder/moduleB.d.ts`
> 3. `/root/src/moduleB.ts`
> 4. `/root/src/moduleB.d.ts`
> 5. `/root/moduleB.ts`
> 6. `/root/moduleB.d.ts`
> 7. `/moduleB.ts`
> 8. `/moduleB.d.ts`

<a name="5b1a9d31"></a>

#### 18.3.2 Node

> 这个解析策略试图在运行时模仿 `Node.js` 模块解析机制。完整的 `Node.js` 解析算法可以在 `Node.js module documentation` 找到。

- Node.js 如何解析模块

> 为了理解 `TypeScript` 编译依照的解析步骤，先弄明白`Node.js`模块是非常重要的。

> 通常，在 `Node.js` 里导入是通过 `require` 函数调用进行的。 `Node.js` 会根据 `require` 是相对路径还是非相对路径做出不同的行为。

> 相对路径很简单。 例如，假设有一个文件路径为 `/root/src/moduleA.js`，包含了一个导入 `var x = require("./moduleB")`;

- Node.js 以下面的顺序解析这个导入

1. 检查 `/root/src/moduleB.js` 文件是否存在。
2. 检查 `/root/src/moduleB` 目录是否包含一个 `package.json` 文件，且 `package.json` 文件指定了一个"`main`"模块。

> 在我们的例子里，如果 `Node.js` 发现文件 `/root/src/moduleB/package.json` 包含了 `{ "main": "lib/mainModule.js" }`，那么 `Node.js` 会引用 `/root/src/moduleB/lib/mainModule.js`。

3. 检查 `/root/src/moduleB` 目录是否包含一个 `index.js` 文件。 这个文件会被隐式地当作那个文件夹下的"`main`"模块。

你可以阅读 `Node.js` 文档了解更多详细信息：[file modules](https://nodejs.org/api/modules.html#modules_file_modules) 和 [folder modules](https://nodejs.org/api/modules.html#modules_folders_as_modules)。

> 但是，非相对模块名的解析是个完全不同的过程。`Node` 会在一个特殊的文件夹 `node\_modules` 里查找你的模块。`node\_modules` 可能与当前文件在同一级目录下，或者在上层目录里。`Node` 会向上级目录遍历，查找每个 `node\_modules`直到它找到要加载的模块。
> 还是用上面例子，但假设 `/root/src/moduleA.js` 里使用的是非相对路径导入 `var x = require("moduleB")`;

- `Node` 则会以下面的顺序去解析 `moduleB`，直到有一个匹配上。

> 1.`/root/src/node_modules/moduleB.js`
> 2.`/root/src/node_modules/moduleB/package.json (如果指定了"main"属性)`
> 3.`/root/src/node_modules/moduleB/index.js`
> 4.`/root/node_modules/moduleB.js`
> 5.`/root/node_modules/moduleB/package.json (如果指定了"main"属性)`
> 6.`/root/node_modules/moduleB/index.js`
> 7.`/node_modules/moduleB.js`
> 8.`/node_modules/moduleB/package.json (如果指定了"main"属性)`
> 9.`/node_modules/moduleB/index.js`

> 注意 `Node.js` 在步骤（`4`）和（`7`）会向上跳一级目录。
> 你可以阅读 `Node.js` 文档了解更多详细信息：[loading modules from node\_modules](https://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders)。

- TypeScript 如何解析模块

> `TypeScript` 是模仿 `Node.js` 运行时的解析策略来在编译阶段定位模块定义文件。因此，`TypeScript` 在 `Node` 解析逻辑基础上增加了 TypeScript源文件的扩展名（ `.ts`，`.tsx` 和 `.d.ts`）。同时，`TypeScript` 在 `package.json` 里使用字段"`types`"来表示类似"`main`"的意义 `-` 编译器会使用它来找到要使用的"`main`"定义文件。

> 比如，有一个导入语句 `import { b } from "./moduleB"` 在 `/root/src/moduleA.ts` 里，会以下面的流程来定位 `"./moduleB"` ：
>
> 1. `/root/src/moduleB.ts`
> 2. `/root/src/moduleB.tsx`
> 3. `/root/src/moduleB.d.ts`
> 4. `/root/src/moduleB/package.json (如果指定了"types"属性)`
> 5. `/root/src/moduleB/index.ts`
> 6. `/root/src/moduleB/index.tsx`
> 7. `/root/src/moduleB/index.d.ts`

> 回想一下 `Node.js` 先查找 `moduleB.js` 文件，然后是合适的 `package.json`，再之后是 `index.js`。

> 类似地，非相对的导入会遵循 `Node.js` 的解析逻辑，首先查找文件，然后是合适的文件夹。因此 `/root/src/moduleA.ts` 件里的 `import { b } from "moduleB"` 会以下面的查找顺序解析：
>
> 1. `/root/src/node_modules/moduleB.ts`
> 2. `/root/src/node_modules/moduleB.tsx`
> 3. `/root/src/node_modules/moduleB.d.ts`
> 4. `/root/src/node_modules/moduleB/package.json (如果指定了"types"属性)`
> 5. `/root/src/node_modules/moduleB/index.ts`
> 6. `/root/src/node_modules/moduleB/index.tsx`
> 7. `/root/src/node_modules/moduleB/index.d.ts`
> 8. `/root/node_modules/moduleB.ts`
> 9. `/root/node_modules/moduleB.tsx`
> 10. `/root/node_modules/moduleB.d.ts`
> 11. `/root/node_modules/moduleB/package.json (如果指定了"types"属性)`
> 12. `/root/node_modules/moduleB/index.ts`
> 13. `/root/node_modules/moduleB/index.tsx`
> 14. `/root/node_modules/moduleB/index.d.ts`
> 15. `/node_modules/moduleB.ts`
> 16. `/node_modules/moduleB.tsx`
> 17. `/node_modules/moduleB.d.ts`
> 18. `/node_modules/moduleB/package.json (如果指定了"types"属性)`
> 19. `/node_modules/moduleB/index.ts`
> 20. `/node_modules/moduleB/index.tsx`
> 21. `/node_modules/moduleB/index.d.ts`

> 不要被这里步骤的数量吓到 `-` `TypeScript` 只是在步骤（`8`）和（`15`）向上跳了两次目录。 这并不比 `Node.js` 里的流程复杂。

<a name="4c9a4392"></a>

### 18.4 附加的模块解析标记

> 1. 有时工程源码结构与输出结构不同。 通常是要经过一系统的构建步骤最后生成输出。 它们包括将 `.ts` 编译成 `.js`，将不同位置的依赖拷贝至一个输出位置。 最终结果就是运行时的模块名与包含它们声明的源文件里的模块名不同。 或者最终输出文件里的模块路径与编译时的源文件路径不同了。
> 2. `TypeScript` 编译器有一些额外的标记用来通知编译器在源码编译成最终输出的过程中都发生了哪个转换。
> 3. 有一点要特别注意的是编译器不会进行这些转换操作；它只是利用这些信息来指导模块的导入。

- `Base URL`

> 在利用 `AMD` 模块加载器的应用里使用 `baseUrl` 是常见做法，它要求在运行时模块都被放到了一个文件夹里。这些模块的源码可以在不同的目录下，但是构建脚本会将它们集中到一起。

> 设置 `baseUrl` 来告诉编译器到哪里去查找模块。 所有 `非相对模块` 导入都会被当做相对于 `baseUrl`。

- `baseUrl` 的值由以下两者之一决定：

> 1. 命令行中 `baseUrl` 的值（如果给定的路径是相对的，那么将相对于当前路径进行计算）
> 2. `tsconfig.json`里的 `baseUrl` 属性（如果给定的路径是相对的，那么将相对于‘`tsconfig.json`’路径进行计算）

> 注意相对模块的导入不会被设置的 `baseUrl` 所影响，因为它们总是相对于导入它们的文件。

> 阅读更多关于 `baseUrl` 的信息 [RequireJS](https://requirejs.org/docs/api.html#config-baseUrl) 和 [`SystemJS`](https://github.com/systemjs/systemjs/blob/master/docs/config-api.md#baseurl)。

<a name="57294489"></a>

### 18.5 路径映射

> 有时模块不是直接放在 `baseUrl` 下面。 比如，充分 "`jquery`"模块地导入，在运行时可能被解释为"`node\_modules/jquery/dist/jquery.slim.min.js`"。 加载器使用映射配置来将模块名映射到运行时的文件，查看 [RequireJs documentation](https://requirejs.org/docs/api.html#config-paths) 和 [SystemJS documentation](https://github.com/systemjs/systemjs/blob/master/docs/config-api.md#paths)。

> `TypeScript` 编译器通过使用 `tsconfig.json` 文件里的 "`paths`" 来支持这样的声明映射。 下面是一个如何指定 `jquery` 的"`paths`"的例子。

```json
{
    "compilerOptions": {
        "baseUrl": ".", // This must be specified if "paths" is.
            "paths": {
            "jquery": ["node_modules/jquery/dist/jquery"] // 此处映射是相对于"baseUrl"
        }
    }
}
```

> 请注意"`paths`"是相对于"`baseUrl`"进行解析。 如果 "`baseUrl`" 被设置成了除"`.`"外的其它值，比如 `tsconfig.json` 所在的目录，那么映射必须要做相应的改变。如果你在上例中设置了 `"baseUrl": "./src"`，那么 `jquery` 应该映射到 `"../node\_modules/jquery/dist/jquery"`。

> 通过"`paths`"我们还可以指定复杂的映射，包括指定多个回退位置。 假设在一个工程配置里，有一些模块位于一处，而其它的则在另个的位置。 构建过程会将它们集中至一处。 工程结构可能如下：

```typescript
projectRoot
├── folder1
│   ├── file1.ts (imports 'folder1/file2' and 'folder2/file3')
│   └── file2.ts
├── generated
│   ├── folder1
│   └── folder2
│       └── file3.ts
└── tsconfig.json
```

- 相应的 `tsconfig.json` 文件如下：

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "*": [
        "*",
        "generated/*"
      ]
    }
  }
}
```

> 它告诉编译器所有匹配 `"*"`（所有的值）模式的模块导入会在以下两个位置查找：
>
> 1. `"*"`： 表示名字不发生改变，所以映射为  `=>` `/`
> 2. "`generated/`"表示模块名添加了“`generated`”前缀，所以映射为 `<moduleName> => <baseUrl>/generated/<moduleName>`

- `按照这个逻辑，编译器将会如下尝试解析这两个导入：`

> (i). 导入`'folder1/file2'`
>
> 1. 匹配 `'*'` 模式且通配符捕获到整个名字。
> 2. 尝试列表里的第一个替换：`'*' -> folder1/file2`。
> 3. 替换结果为非相对名 - 与 `baseUrl` 合并 `-> projectRoot/folder1/file2.ts`。
> 4. 文件存在。完成。

(ii). 导入`'folder2/file3'`

> 1. 匹配 `'*'` 模式且通配符捕获到整个名字。
> 2. 尝试列表里的第一个替换：`'*' -> folder2/file3`。
> 3. 替换结果为非相对名 - 与 `baseUrl` 合并 `-> projectRoot/folder2/file3.ts`。
> 4. 文件不存在，跳到第二个替换。
> 5. 第二个替换：`'generated/' -> generated/folder2/file3`。
> 6. 替换结果为非相对名 - 与 `baseUrl` 合并 `-&gt; projectRoot/generated/folder2/file3.ts`。
> 7. 文件存在。完成。

- 利用 `rootDirs` 指定虚拟目录

> 有时多个目录下的工程源文件在编译时会进行合并放在某个输出目录下。 这可以看做一些源目录创建了一个“虚拟”目录。  利用 `rootDirs`，可以告诉编译器生成这个虚拟目录的 `roots`； 因此编译器可以在“虚拟”目录下解析相对模块导入，就好像它们被合并在了一起一样。

> 比如，有下面的工程结构：

```typescript
src
 └── views
     └── view1.ts (imports './template1')
     └── view2.ts

 generated
 └── templates
         └── views
             └── template1.ts (imports './view2')
```

> `src/views` 里的文件是用于控制UI的用户代码。 `generated/templates` 是 `UI` 模版，在构建时通过模版生成器自动生成。构建中的一步会将 `/src/views` 和 `/generated/templates/views` 的输出拷贝到同一个目录下。 在运行时，视图可以假设它的模版与它同在一个目录下，因此可以使用相对导入 `"./template"`。

> 可以使用"`rootDirs`"来告诉编译器。"`rootDirs`"指定了一个 `roots` 列表，列表里的内容会在运行时被合并。

> 因此，针对这个例子， `tsconfig.json` 如下：

```json
{
  "compilerOptions": {
    "rootDirs": [
      "src/views",
      "generated/templates/views"
    ]
  }
}
```

> 每当编译器在某一 `rootDirs` 的子目录下发现了相对模块导入，它就会尝试从每一个 `rootDirs` 中导入。

> `rootDirs` 的灵活性不仅仅局限于其指定了要在逻辑上合并的物理目录列表。它提供的数组可以包含任意数量的任何名字的目录，不论它们是否存在。这允许编译器以类型安全的方式处理复杂捆绑( `bundles` )和运行时的特性，比如条件引入和工程特定的加载器插件。

> 设想这样一个国际化的场景，构建工具自动插入特定的路径记号来生成针对不同区域的捆绑，比如将 `\#{locale}` 做为相对模块路径 `./\#{locale}/messages` 的一部分。在这个假定的设置下，工具会枚举支持的区域，将抽像的路径映射成 `./zh/messages`，`./de/messages` 等。

> 假设每个模块都会导出一个字符串的数组。比如 `./zh/messages` 可能包含：

```typescript
export default [
    "您好吗",
    "很高兴认识你"
];
```

> 利用 `rootDirs` 我们可以让编译器了解这个映射关系，从而也允许编译器能够安全地解析 `./\#{locale}/messages`，就算这个目录永远都不存在。比如，使用下面的 `tsconfig.json`：

```json
{
  "compilerOptions": {
    "rootDirs": [
      "src/zh",
      "src/de",
      "src/#{locale}"
    ]
  }
}
```

> 编译器现在可以将 `import messages from './#{locale}/messages'` 解析为 `import messages from './zh/messages'` 用做工具支持的目的，并允许在开发时不必了解区域信息。

<a name="57d39e78"></a>

### 18.6 跟踪模块解析

> 如之前讨论，编译器在解析模块时可能访问当前文件夹外的文件。 这会导致很难诊断模块为什么没有被解析，或解析到了错误的位置。 通过 `--traceResolution` 启用编译器的模块解析跟踪，它会告诉我们在模块解析过程中发生了什么。

> 假设我们有一个使用了 `typescript` 模块的简单应用。`app.ts` 里有一个这样的导入 `import * as ts from "typescript"`。

```typescript
│   tsconfig.json
├───node_modules
│   └───typescript
│       └───lib
│               typescript.d.ts
└───src
        app.ts
```

> 使用 `--traceResolution` 调用编译器。

```typescript
tsc --traceResolution
```

> 输出结果如下：

```typescript
======== Resolving module 'typescript' from 'src/app.ts'. ========
Module resolution kind is not specified, using 'NodeJs'.
Loading module 'typescript' from 'node_modules' folder.
File 'src/node_modules/typescript.ts' does not exist.
File 'src/node_modules/typescript.tsx' does not exist.
File 'src/node_modules/typescript.d.ts' does not exist.
File 'src/node_modules/typescript/package.json' does not exist.
File 'node_modules/typescript.ts' does not exist.
File 'node_modules/typescript.tsx' does not exist.
File 'node_modules/typescript.d.ts' does not exist.
Found 'package.json' at 'node_modules/typescript/package.json'.
'package.json' has 'types' field './lib/typescript.d.ts' that references 'node_modules/typescript/lib/typescript.d.ts'.
File 'node_modules/typescript/lib/typescript.d.ts' exist - use it as a module resolution result.
======== Module name 'typescript' was successfully resolved to 'node_modules/typescript/lib/typescript.d.ts'. ========
```

> 需要留意的地方：

> 1. 导入的名字及位置 `======== Resolving module 'typescript' from 'src/app.ts'. ========`
> 2. 编译器使用的策略 `Module resolution kind is not specified, using 'NodeJs'.`
> 3. 从 `npm` 加载 `types` `'package.json' has 'types' field './lib/typescript.d.ts' that references 'node\_modules/typescript/lib/typescript.d.ts'.`
> 4. 最终结果 `======== Module name 'typescript' was successfully resolved to 'node\_modules/typescript/lib/typescript.d.ts'. ========`

- 使用 `--noResolve`

> 正常来讲编译器会在开始编译之前解析模块导入。 每当它成功地解析了对一个文件 `import`，这个文件被会加到一个文件列表里，以供编译器稍后处理。`--noResolve` 编译选项告诉编译器不要添加任何不是在命令行上传入的文件到编译列表。 编译器仍然会尝试解析模块，但是只要没有指定这个文件，那么它就不会被包含在内。

> `比如`:

- `app.ts`

```typescript
import * as A from "moduleA" // OK, moduleA passed on the command-line
import * as B from "moduleB" // Error TS2307: Cannot find module 'moduleB'.
```

```typescript
tsc app.ts moduleA.ts --noResolve
```

> 使用 `--noResolve` 编译 `app.ts`：

> 1. 可能正确找到 `moduleA`，因为它在命令行上指定了。
> 2. 找不到 `moduleB`，因为没有在命令行上传递。

<a name="b51bf333"></a>

### 18.7 常见问题

- 为什么在 `exclude` 列表里的模块还会被编译器使用

> `tsconfig.json` 将文件夹转变一个“工程” 如果不指定任何 “`exclude`” 或 “`files`”，文件夹里的所有文件包括 `tsconfig.json` 和 所有的子目录 都会在编译列表里。 如果你想利用 “`exclude`”排除某些文件，甚至你想指定所有要编译的文件列表，请使用“`files`”。

> 有些是被 `tsconfig.json` 自动加入的。 它不会涉及到上面讨论的模块解析。 如果编译器识别出一个文件是模块导入目标，它就会加到编译列表里，不管它是否被排除了。

> 因此，要从编译列表中排除一个文件，你需要在排除它的同时，还要排除所有对它进行 `import` 或使用了 `/// <reference path="..." />` 指令的文件。

<a name="545fa643"></a>

## 十九、声明合并

<a name="e8d98d10"></a>

### 19.1 介绍

> `TypeScript` 中有些独特的概念可以在类型层面上描述 `JavaScript` 对象的模型。这其中尤其独特的一个例子 "声明合并" 的概念。

> 理解了这个概念，将有助于操作现有的 `JavaScript` 代码。同时，也会有助于理解更多高级抽象的概念。

> 对本文件来讲，"`声明合并`" 是指编译器将针对同一个名字的两个独立声明合并为单一声明。合并后的声明同时拥有原先两个声明的特性。任何数量的声明都可被合并；不局限于两个声明。

<a name="4087bca2"></a>

### 19.2 基础概念

> `TypeScript` 中的声明会创建以下三种实体之一：`命名空间`，`类型` 或 `值`。创建命名空间的声明会新建一个命名空间，它包含了用(.)符号来访问时使用的名字。创建类型的声明是：用声明的模型创建一个类型并绑定到给定的名字上。
> 最后，创建值的声明会创建在 `JavaScript` 输出中看到的值。

| Declaration Type | Namespace | Type | Value |
| :--- | :--- | :--- | :--- |
| `Namespace` | X |  | X |
| `Class` |  | X | X |
| `Enum` |  | X | X |
| `Interface` |  | X |  |
| `Type Alias` |  | X |  |
| `Function` |  |  | X |
| `Variable` |  |  | X |

> 理解每个声明创建了什么，有助于理解当声明合并时，有哪些东西被合并了。

<a name="c468378c"></a>

### 19.3 合并接口

> 最简单最常见的声明合并类型是接口合并。从根本上说，合并的机制是把双方的成员放到一个同名的接口里。

```typescript
interface Box { height: number; width: number; }
interface Box { scale: number; }
let box: Box = { height: 5, width: 6, scale: 10 };
```

> 接口的非函数成员应该是唯一的。如果它们不是唯一的，那么它们必须是相同的类型。如果两个接口中同时声明了同名的非函数成员且它们的类型不同，则编译器会报错。

> 对于函数成员，每个同名函数声明都会被当成这个函数的一个重载。同时需要注意，当接口 `A` 与后来的接口 `A` 合并时，后面的接口具有更高的优先级。

```typescript
// 如下例所示：
interface Cloner {
    clone(animal: Animal): Animal;
}
interface Cloner {
    clone(animal: Sheep): Sheep;
}
interface Cloner {
    clone(animal: Dog): Dog;
    clone(animal: Cat): Cat;
}
// 这三个接口合并成一个声明：
interface Cloner {
    clone(animal: Dog): Dog;
    clone(animal: Cat): Cat;
    clone(animal: Sheep): Sheep;
    clone(animal: Animal): Animal;
}
```

> 注意每组接口里的声明顺序保持不变，但各组接口之间的顺序是后来的接口重载出现在靠前的位置。

> 这个规则有一个例外是当出现特殊的函数签名时。如果签名里有一个参数的类型是单一的字符串字面量(比如，不是字符串字面量的联合类型)，那么它将会被提升到重载列表的最顶端。

```typescript
// 比如，下面的接口会合并到一起
interface Document {
    createElement(tagName: any): ELement;
}
interface Document {
    createElement(tagName: "div"): HTMLDivElement;
    createElement(tagName: "span"): HTMLSpanElement;
}
interface Document {
    createElement(tagName: string): HTMLElement;
    createElement(tagName: "canvas"): HTMLCanvasElement;
}
// 合并后的 Document 将会像下面这样:
interface Document {
    createElement(tagName: "canvas"): HTMLCanvasElement;
    createElement(tagName: "div"): HTMLDivElement;
    createElement(tagName: "span"): HTMLSpanElement;
    createElement(tagName: string): HTMLElement;
    createElement(tagName: any): Element;
}
```

<a name="9d7dcfd8"></a>

### 19.4 合并命名空间

1. 与接口相似，同名的命名空间也会合并其成员。命名空间会创建出命名空间和值，我们需要知道这两者都是怎么合并的。
2. 对于命名空间的合并，模块导出的同名接口进行合并，构成单一命名空间内含合并后的接口。
3. 对于命名空间里值的合并，如果当前已经存在给定名字的命名空间，那么后来的命名空间的导出成员会被加到已经存在的那个模块里。

```typescript
// Animals 声明合并示例：
namespace Animals {
    export class Zebra { }
}
namespace Animals {
    export interface Legged {
        numberOfLegs: number;
    }
    export class Dog { }
}
// 等同于：
namespace Animals {
    export interface Legged {
        numberOfLegs: number;
    }
    export class Zebra { }
    export class Dog { }
}
```

> 除了这些合并外，你还需要了解非导出成员是如何处理的。非导出成员仅在其原有的(合并前的)命名空间内可见。这就是说合并之后，从其它命名空间合并进来的成员无法访问非导出成员。

- 下面提供了清晰的说明：

```typescript
namespace Animal {
    let haveMuscles = true;
    export function animalsHaveMuscles() {
        return haveMuscles;
    }
}
namespace Animal {
    export function doAnimalsHaveMuscles() {
        return haveMuscles; // // Error, because haveMuscles is not accessible here
    }
}
```

> 因为 `haveMuscles` 并没有导出，只有 `animalsHaveMuscles` 函数共享了原始未合并的命名空间可以访问这个变量。
> `doAnimalsHaveMuscles` 函数虽是合并命名空间的一部分，但是访问不了未导出的成员。

<a name="9c7e14b1"></a>

### 19.5 命名空间与类和函数和枚举类型合并

> 命名空间可以与其它类型的声明合并。只要命名空间的定义符合将要合并类型的定义。合并结果包含两者的声明类型。
> `TypeScript` 使用这个功能去实现一些 `JavaScript` 里的设计模式。

<a name="619f5dd6"></a>

#### 19.5.1 合并命名空间和类

> 这让我们可以表示内部类

```typescript
class Album {
    label: Album.AlbumLabel;
}

namespace Album {
    export class AlbumLabel { }
}
```

> `合并规则` 与上面 `合并命名空间` 小节里讲的规则一致，我们必须导出 `AlbumLabel` 类，好让合并的类能访问。

> `合并结果`是一个类并带有一个内部类。你也可以使用命名空间为类增加一些静态属性。

> 除了内部类的模式，你在 `JavaScript` 里，创建一个函数稍后扩展它增加一些属性也是很常见的。`TypeScript` 使用声明合并来达到这个目的并保证类型安全。

```typescript
function buildLabel(name: string): string {
    return buildLabel.prefix + name + buildLabel.suffix;
}
namespace buildLabel {
    export let suffix = '';
    export let prefix = 'Hello, ';
}
console.log(buildLabel("Sam Smith"));

// 相似的，命名空间可以用来扩展枚举类型
enum Color {
    red = 1, green = 2, blue = 4
}
namespace Color {
    export function mixColor(colorName: string) {
        if(colorName === "yellow") {
            return Color.red + Color.green;
        } else if(colorName === "white") {
            return Color.red + Color.green + Color.blue;
        } else if(colorName === "magenta") {
            return Color.red + Color.blue;
        } else if(colorName === "cyan") {
            return Color.green + Color.blue;
        }
    }
}
```

<a name="2a844ef0"></a>

#### 19.5.2 非法的合并

> `TypeScript` 并非允许所有的合并。目前，类不能与其它类或变量合并。想要了解如何模仿类的合并，请参照 [TypeScript 混入](https://github.com/magicwangxuanqi/typeScript-document/tree/364ec42f004be1686f97e1b02f97c1cf1fb2ba02/Mixins%E6%B7%B7%E5%85%A5/README.md)。

<a name="a69a1df6"></a>

### 19.6 模块扩展

> 虽然 `JavaScript` 不支持合并，但你可以为导入的对象打补丁以更新它们。

- 让我们考察一下这个玩具性的示例：

```typescript
// observable.js
export class Observable<T> {
    // ... implementation left as an exercise for the reader...
}

// map.js
import { Observable } from './observable'
Observable.prototype.map = function () {
    // ... another exercise for the reader
};
```

> 它也可以很好地工作在 `TypeScript` 中，但编译器对 `Observable.prototype.map` 一无所知。你可以使用扩展模块来将它告诉编译器。

```typescript
// observable.ts stays the same
// map.ts
import { Observable } from './observable';
declare module "./observable" {
    interface Observable<T> {
        map<U>(f: (x: T) => U): Observable<U>
    }
}
Observable.prototype.map = function (f) {
    // ... another exercise for the reader
};

// consumer.ts
import {Observable} from './observable';
import "./map";
let o: Observable<number>;
o.map(x => x.toFixed());
```

> 模块名的解析和用 `import / export` 解析模块标识符的方式是一致的。 更多信息请参考 [Modules](https://github.com/magicwangxuanqi/typeScript-document/tree/364ec42f004be1686f97e1b02f97c1cf1fb2ba02/%E6%A8%A1%E5%9D%97/README.md)。当这些声明在扩展中合并时，就好像在原始位置被声明了一样。 但是，你不能在扩展中声明新的顶级声明(仅可以扩展模块中已经存在的声明)。

<a name="cc50d8ff"></a>

### 19.7 全局扩展

> 你也可以在模块内部添加声明到全局作用域中

> `observable.ts`

```typescript
export class Observable<T> {
    // ... still no implementation ...
}
declare global {
    interface Array<T> {
        toObservable(): Observable<T>;
    }
}
Array.prototype.toObservable = function() {
    // ...
};
```

> 全局扩展和与模块扩展的行为和限制是相同的。

<a name="79edeb2d"></a>

## 二十、JSX

<a name="b3c2bc25"></a>

### 20.1 介绍

> `JSX` 是一种嵌入式的类似 `XML` 的语法。它可以被转换成合法的 `JavaScript`，尽管转换的语义是根据不同的实现而定的。
> `JSX` 因 `React` 框架而流行，但也存在其它的实现。`TypeScript` 支持内嵌，类型检查以及将 `JSX` 直接编译为 JavaScript。

<a name="bdc7c5aa"></a>

### 20.2 基本用法

- 想要使用 `JSX` 必须做两件事：

> 1. 给文件一个 `.tsx` 扩展名
> 2. 启用 `jsx` 选项

- `TypeScript` 具有三种 `JSX` 模式：`preserve`, `react` 和 `react-native`。这些模式只在代码生成阶段起作用 - 类型检查并不受影响。

> 1. 在 `preserve` 模式下生成代码中会保留 `JSX` 以供后续的转换操作使用( `比如：Babel` )。另外，输出文件会带有 `.jsx` 扩展名。
> 2. `react` 模式会生成 `React.createElement`，在使用前不需要再进行转换操作了，输出文件的扩展名为 `.js`。
> 3. `react-native` 相当于 `preserve`，它也保留了所有的 `JSX`，但是输出文件的扩展名是 `.js`。

| 模式 | 输入 | 输出 | 输出文件扩展名 |
| :--- | :--- | :--- | :--- |
| preserve | `<div />` | `<div />` | `.js` |
| react | `<div />` | `React.createElement("div")` | `.js` |
| react-native | `<div />` | `<div />` | `.js` |

> 你可以通过在命令行里使用 `--jsx` 标记或 `tsconfig.json` 里的选项来指定模式。
> 注意：`React` 标识符是写死的硬编码，所以你必须保证 `React`（大写的`R`）是可用的。

<a name="139495d4"></a>

### 20.3 as 操作符

> 回想一下怎么写类型断言

```typescript
var foo = <foo>bar;
```

> 这里断言 `bar` 变量是 `foo` 类型的。 因为 `TypeScript` 也使用尖括号来表示类型断言，在结合 `JSX` 的语法后将带来解析上的困难。因此，`TypeScript` 在 `.tsx` 文件里禁用了使用尖括号的类型断言。

> 由于不能够在 `.tsx` 文件里使用上述语法，因此我们应该使用另一个类型断言操作符：`as`。

- 上面的例子可以很容易地使用 `as` 操作符改写：

```typescript
var foo = bar as foo;
```

> `as` 操作符在 `.ts` 和 `.tsx` 里都可用，并且与尖括号类型断言行为是等价的。

<a name="3f92b47a"></a>

### 20.4 类型检查

> 为了理解 `JSX` 的类型检查，你必须首先理解 `固有元素` 与 `基于值的元素` 之间的区别。假设有这样一个 `JSX` 表达式 `<expr />`，`expr` 可能引用环境自带的某些东西(比如，在 `DOM` 环境里的 `div` 或 `span`)或者是你自定义的组件。这是非常重要的。原因有如下两点：

1. 对于 `React`，固有元素会生成字符串 (`React.createElement("div")` )，然后由你自定义的组件却不会生成( `React.createElement(MyComponent)`)。
2. 传入 `JSX` 元素里的属性类型的查找方式不同。固有元素总以一个小写字母开头，基于值的元素总是以一个大写字母开头。

<a name="017bb901"></a>

#### 20.4.1 固有元素

> 固有元素使用特殊的接口 `JSX.IntrinsicElements` 来查找。默认地，如果这个接口没有指定，会全部通过，不对固有元素进行类型检查。然而，如果这个接口存在，那么固有元素的名字需要在 `JSX.IntrinsicElements` 接口的属性里查找。

- 例如：

```jsx
declare namespace JSX {
    interface IntrinsicElements {
        foo: any;
    }
}
<foo />; // 正确
<bar />; // 错误
```

> 在上例中，\`\`\`没有问题，但是\`\`会报错，因为它没在 JSX.IntrinsicElements\` 里指定。

> 注意：你也可以在 `JSX.IntrinsicElements` 上指定一个用来捕获所有字符串索引：

```jsx
declare namespace JSX {
    interface IntrinsicElements {
        [elemName: string]: any;
    }
}
```

<a name="040532f3"></a>

#### 20.4.2 基于值的元素

> 基于值的元素会简单的在它所在的作用域里按标识符查找。

```jsx
import MyComponent from "./myComponent";
<MyComponent /> // 正确
<SomeOtherComponent /> // 错误
```

> 有两种方式可以定义基于值的元素：
>
> 1. 无状态函数组件 (`SFC`)
> 2. 类组件

> 由于这两种基于值的元素在 `JSX` 表达式里无法区分，因此 `TypeScript` 首先会尝试将表达式作为无状态函数组件进行解析。如果解析成功，那么 `TypeScript` 就完成了表达式到其声明的解析操作。如果按照无状态函数组件解析失败，那么 `TypeScript` 会继续尝试以类组件的形式进行解析。如果依旧失败，那么将输出一个错误。

<a name="86291de0"></a>

##### (1) 无状态函数组件

> 正如其名，组件被定义成 `JavaScript` 函数，它的第一个参数是 `props` 对象。`TypeScript` 会强制它的返回值可以赋值给 `JSX.Element`。

```jsx
interface FooProp {
    name: string;
    X: number;
    Y: number;
}
declare function AnotherComponent(prop: { name: string });
function ComponentFoo(prop: FooProp) {
    return <AnotherComponent name={prop.name} />
}
const Button = (prop: { value: string }, context: { color: string }) => <button>;
```

> 由于无状态函数组件是简单的 `JavaScript` 函数，所以我们还可以利用函数重载。

```jsx
interface ClickableProps {
    children: JSX.Element[] | JSX.Element;
}
interface HomeProps extends ClickableProps {
    home: JSX.Element;
}
interface SlideProps extends ClickableProps {
    side: JSX.Element | string;
}
function MainButton(prop: HomeProps): JSX.Element;
function MainButton(prop: SlideProps): JSX.Element {
    // ...
}
```

<a name="013dfe55"></a>

##### (2) 类组件

> 1. 我们可以定义类组件的类型。然而，我们首先最好弄懂两个新的术语：`元素类的类型` 和 `元素实例的类型`。
> 2. 现在有 `<Expr />`，元素类的类型为 `Expr` 的类型。所以在上面的例子里，如果 `MyComponent` 是 `ES6` 的类，那么类类型就是类的构造函数和静态部分。如果 `MyComponent` 是个工厂函数，类类型为这个函数。
> 3. 一旦建立起了类类型，实例类型由构造器或调用签名(如果存在的话)的返回值的联合构成。再次说明，在 `ES6` 类的情况下，实例类型为这个类的实例的类型，并且如果是工厂函数，实例类型为这个函数返回值类型。

```jsx
class MyComponent {
    render() {}
}
// 使用构造签名
var myComponent = new MyComponent();
// 元素类的类型 => MyComponent;
// 元素实例的类型 => { render: () => void }
```

```jsx
function MyFactoryFunction() {
    return {
        render: () => {}
    }
}
// 使用调用签名
var myComponent = MyFactoryFunction();
// 元素类的类型 => FactoryFunction
// 元素实例的类型 => { render: () => void }
```

> 元素的实例类型很有趣，因为它必须赋值给 `JSX.ElementClass` 或 `抛出一个错误`。默认的 `JSX.ElementClass` 为 `{}`，但是它可以被扩展用来限制 `JSX` 的类型以符合相应的接口。

```jsx
declare namespace JSX {
    interface ElementClass {
        render: any
    }
}

class MyComponent {
    render() {}
}
function MyFactoryFunction() {
    return {
        render: () => {}
    }
}
<MyComponent />; // 正确
<MyFactoryFunction />; // 正确

class NotAValidComponent {}
function NotAValidFactoryFunction() {
    return {}
}
<NotAValidComponent />; // 错误
<NotAValidFactoryFunction />; // 错误
```

<a name="14aa68e0"></a>

### 20.5 属性类型检查

1. 属性类型检查的第一步是 确定元素类型。这在 `固有元素` 和 `基于值的元素` 之间稍有不同。
2. 对于固有元素，这是 `JSX.IntrinsicElements` 属性的类型。

```jsx
declare namespace JSX {
    interface IntrinsicElements {
        foo: {
            bar?: boolean;
        }
    }
}
// `foo` 的元素属性类型为 `{bar?: boolean}`
<foo bar />;
```

3. 对于基于值的元素，就稍微复杂些。它取决于先前确定的在元素实例类型上的某个属性的类型。至于该使用哪个属性来确定类型取决于 `JSX.ElementAttributesProperty`。它应该使用单一的属性来定义。这个属性名之后会被使用。`TypeScript 2.8`，如果未指定 `JSX.ElementAttributesProperty`，那么将使用 `类元素构造函数` 或 `SFC` 调用的第一个参数类型。

```jsx
declare namespace JSX {
  interface ElementAttributesProperty {
      props; // 指定用来使用的属性名
  }
}
class MyComponent {
  // 在元素实例上指定类型
  props: {
      foo?: string;
  }
}
// `MyComponent` 的元素属性类型为 `{foo?: string}`
<MyComponent foo="bar" />;
```

4. 元素属性类型用于 `JSX` 里进行属性的类型检查。支持可选属性和必须属性

```jsx
declare namespace JSX {
    interface IntrinsicElements {
        foo: {
            requiredProp: string;
            optionalProp?: number;
        }
    }
}
<foo requiredProp="bar" />; // 正确
<foo requiredProp="bar" optionalProp={0} />; // 正确
<foo />; // 错误，缺少requiredProp
<foo requiredProp={0} />; // 错误，requiredProp 应该是字符串
<foo requiredProp="bar" unknownProp />; // 错误，unknownProp 属性不存在
<foo requiredProp="bar" some-unknown-prop />; // 正确，`some-unknown-prop` 不是合法的标识符
```

> 注意：如果一个属性名不是个合法的 `JS` 标识符(像 `data-*` 属性)，并且它没出现在元素属性类型里时不会当作一个错误。

> 另外，`JSX` 还会使用 `JSX.IntrinsicAttributes` 接口来指定额外的属性，这些额外的属性通常不会被组件的 `props` 或 `arguments` 使用 - 比如 `React` 里的 `key`。还有，`JSX.IntrinsicClassAttributes<T>` 泛型类型也可以用来做同样的事情。这里的泛型参数表示类实例类型。在 `React` 里，它用来允许 `Ref<T>` 类型上的 `ref` 属性。通常来讲，这些接口上的所有属性都是可选的，除非你想要用户在每个 `JSX` 标签上都提供一些属性。

> 延展操作符也可以使用

```jsx
var props = { requiredProp: 'bar' }; ; // 正确
var badProps = {};
<foo {...badProps} />; // 错误
```

<a name="e1fd36c4"></a>

### 20.6 子孙类型检查

> 从 `TypeScript 2.3` 开始，我们引入了 `children` 类型检查。`children` 是元素属性(`attribute`)类型的一个特殊属性(`property`)，子 `JSXExpression` 将会被插入到属性里。与使用 `JSX.ElementAttributesProperty` 来决定 `props` 名类似，我们可以利用 `JSX.ElementChildrenAttribute` 来决定 `children` 名。`JSX.ElementChildrenAttribute` 应该被声明在单一的属性(`property`)里。

```typescript
declare namespace JSX {
    interface ElementChildrenAttribute {
        children: {}; // specify children name to use
    }
}
```

> 如不特殊指定子孙的类型，我们将使用 `React typings` 里的默认类型

```jsx
<div>
  <h1>Hello</h1>
</div>;

<div>
  <h1>Hello</h1>
  World
</div>;

const CustomComp = (props) => <div>props.children</div>
<CustomComp>
  <div>Hello World</div>
  {"This is just a JS expression..." + 1000}
</CustomComp>
```

```jsx
interface PropsType {
  children: JSX.Element;
  name: string;
}
class Component extends React.Component<PropsType, {}> {
  render() {
      return (
          <h2>
              {this.props.children}
          </h2>
      )
  }
}
// Ok
<Component>
  <h1>Hello World</h1>
</Component>

// Error: children is of type JSX.Element not array of JSX.Element
<Component>
  <h1>Hello World</h1>
  <h2>Hello World</h2>
</Component>

// Error: children is of type JSX.Element not array of JSX.Element or string.
<Component>
  <h1>Hello</h1>
  World
</Component>
```

<a name="5b0e78dc"></a>

### 20.7 JSX 结果类型

> 默认地 `JSX` 表达式结果的类型为 `any`。你可以自定义这个类型，通过指定 `JSX.Element` 接口。然而，不能够从接口里检索元素，属性或 `JSX` 的子元素的类型信息。它是一个黑盒。

<a name="caecee5a"></a>

### 20.8 嵌入的表达式

> `JSX` 允许你使用 `{}` 标签来内嵌表达式

```tsx
var a = <div>
    {['foo', 'bar'].map(i => <span>{i / 2}</span>)}
</div>;
// 上面的代码产生一个错误，因为你不能用数字来除以一个字符串。输出如下，若你使用了 preserve 选项
var a = <div>
    {['foo', 'bar'].map(function(i) { return <span>{i / 2}</span>; })}
</div>
```

<a name="c36d0ea9"></a>

### 20.9 react 整合

> 要想一起使用 `JSX` 和 `React`，你应该使用 `React` 类型定义。这些类型声明定义了 `JSX` 合适命名空间来使用 `React`。

```jsx
/// <reference path="react.d.ts" />
interface Props {
  foo: string;
}
class MyComponent extends React.Component<Props, {}> {
  render() {
      return (
          <span>{this.props.foo}</span>
      )
  }
}
<MyComponent foo="bar" />; // 正确
<MyComponent foo={0} />; // 错误
```

<a name="8a6296c2"></a>

### 20.10 工厂函数

> `jsx`：`react` 编译选项使用的工厂函数是可以配置的。可以使用 `jsxFactory` 命令行选项，或内联的 `@jsx` 注释指令在每个文件上设置。比如，给 `createElement` 设置 `jsxFactory`，`<div />` 会使用 `createElement("div")` 来生成，而不是 `React.createElement("div")`。

```jsx
// 注释指令可以像下面这样使用：(在 TypeScript 2.8 里)
import preact = require("preact");
/* @jsx preact.h */
const x = <div/>;

// 生成
const preact = require("preact");
const x = preact.h("div", null);
```

> 工厂函数的选择同样会影响 `JSX` 命名空间的查找(类型检查)。如果工厂函数使用 `React.createElement` 定义(默认)，编译器会先检查 `React.JSX`，之后才检查全局的 `JSX`。如果工厂函数定义为 `h`，那么在检查全局的 `JSX` 之前先检查 `h.JSX`。

<a name="e0a191e7"></a>

## 二十一、装饰器

<a name="9f1bde89"></a>

### 21.1 介绍

> 随着 `TypeScript` 和 `ES6` 里引入了类，在一些场景下我们需要额外的特定来支持标注或修改类及其成员。装饰器(`Decorators`)为我们在类的声明及成员上通过元编程语法添加标注提供了一种方式。`JavaScript` 里的装饰器目前处在 建议征集的第二阶段，但在 `TypeScript` 里已作为一项实验性特性予以支持。
> 注意：装饰器是一项实验性特性，在未来的版本中可能会发生改变
> 若要启用实验性的装饰器特性，你必须在命令行或 `tsconfig.json` 里启用 `experimentalDecorators` 编译器选项。
> 命令行： `tsc --target ES5 --experimentalDecorators`

- tsconfig.json

```json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true
    }
}
```

<a name="2a712a5b"></a>

### 21.2 装饰器

> 装饰器是一种特殊的声明，它能够被附加到 `类声明`、`方法`、`访问符`、`属性或参数上`。装饰器使用 `@expression` 这种形式，`expression` 求值后必须为一个函数，它会在运行时被调用，被装饰器的声明信息作为参数传入。
> 例如，有一个 `@sealed` 装饰器，我们会这样定义 `sealed` 函数

```typescript
function sealed(target) {
    // do something with target
}
```

> 注意：后面类装饰器小节里有一个更加详细的例子。

<a name="d9500669"></a>

### 21.3 装饰器工厂

> 如果我们要定制一个修饰器如何应用到一个声明上，我们得写一个装饰器工厂函数。装饰器工厂就是一个简单的函数，它返回一个表达式，以供装饰器在运行时调用。

- 我们可以通过下面的方式来写一个装饰器工厂函数：

```typescript
function color(value: string) { // 这是一个装饰器工厂
  return function (target) { // 这是装饰器
      // do something with "target" and "value" ...
  }
}
```

> 注意：下面方法装饰器小节里有一个更加详细的例子。

<a name="0d39ca26"></a>

### 21.4 装饰器组合

> 多个装饰器可以同时应用到一个声明上，就像下面的实例：

1. 书写在同一行：

```typescript
@f @g x
```

2. 书写在多行上：

```typescript
@f
@g
x
```

> 当多个装饰器应用于一个声明上，它们求值方式与 复合函数 相似。在这个模型下，当复合 f 和 g 时，复合的结果 `\(f∘g\)\(x\)` 等同于 `f\(g\(x\)\)`。

> 同样的，在 `TypeScript` 里，当多个装饰器应用在一个声明上时会进行如下步骤的操作： `1`. 由上至下依次对装饰器表达式求值 `2`. 求值的结果会被当作函数，由下至上依次被调用。

```typescript
// 如果我们使用 装饰器工厂 的话，可以通过下面的例子来观察它们求值的顺序：
function f() {
    console.log("f(): evaluated");
    return function(target, prototypeKey: string, descriptor: PropertyDescriptor) {
        console.log("f(): called");
    }
}
function g() {
    console.log("g(): evaluated");
    return function(target, prototypeKey: string, descriptor: PropertyDescriptor) {
        console.log("g(): called");
    }
}
class C {
    @f()
    @g()
    method() {}
}
```

```typescript
// 在控制台里会打印出如下结果：
f(): evaluated
g(): evaluated
g(): called
f(): called
```

<a name="fb55aec8"></a>

### 21.5 装饰器求值

> 类中不同声明上的装饰器将按以下规定的顺序应用：

- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个实例成员。
- 参数装饰器，然后依次是方法装饰器，访问符装饰器，或属性装饰器应用到每个静态成员。
- 参数装饰器应用到构造函数。
- 类装饰器应用到类。

<a name="645ee8a9"></a>

### 21.6 类装饰器

1.类装饰器在类声明之前被声明(紧靠着类声明)。类装饰器应用于类构造函数，可以用来监视修改或替换类定义。类装饰器不能用在声明文件中( `.d.ts` )，也不能用在任何外部上下文中(比如 `declare` 的类)。

2. 类装饰器表达式会在运行时当作函数被调用，类的构造函数作为其唯一的参数。
3. 如果类装饰器返回一个值，它会使用提供的构造函数来替换类的声明。

> 注意：如果你要返回一个新的构造函数，你必须注意处理好原来的原型链。在运行时的装饰器调用逻辑中 不会为你做这些。
> 下面是使用类装饰器( `@sealed` )的例子，应用在 `Greeter` 类：

```typescript
@sealed
class Greeter1 {
    greeting: string;

    constructor(message: string) {
        this.greeting = message;
    }

    greet() {
        return "Hello, " + this.greeting;
    }
}

// 我们可以这样定义 @sealed 装饰器
function sealed(constructor: Function) {
    Object.seal(constructor);
    Object.seal(constructor.prototype);
}
```

> 当 `@sealed` 被执行的时候，它将密闭此类的构造函数和原型。(注：参见 `Object.seal` )

- 下面是一个重载构造函数的例子：

```typescript
function classDecorator<T extends { new(...args: any[]): {} }>(constructor: T) {
    return class extends constructor {
        newProperty = "new property";
        hello = "override";
    }
}

@classDecorator
class Greeter2 {
    property = "property";
    hello: string;

    constructor(m: string) {
        this.hello = m;
    }
}

console.log(new Greeter2("world"));
```

<a name="00bcdba7"></a>

### 21.7 方法装饰器

> 方法装饰器声明在一个方法的声明之前(紧靠着方法声明)。它会被应用到方法的 `属性描述符` 上，可以用来监视，修改或者替换方法定义。方法装饰器不能用在声明文件 `.d.ts` 上，重载或者任何外部上下文(比如 `declare` 的类)中。

> 方法装饰器表达式会在运行时当作函数被调用，传入下列 `3` 个参数：
>
> 1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
> 2. 成员的名字
> 3. 成员的属性修饰符

> 注意：如果代码输出目标代码小于 `ES5`，属性描述符 将会是 `undefined`。

> 如果方法装饰器返回一个值，它会被用作方法的 属性描述符。

> 注意：如果代码输出目标版本小于 `ES5`，返回值会被忽略。

- 下面是一个方法装饰器 (`@enumerable`) 的例子，应用于 `Greeter` 类的方法上。

```typescript
class Greeter {
  greeting: string;
  constructor(message: string) {
      this.greeting = message;
  }

  @enumerable(false)
  greet() {
      return "Hello, " + this.greeting;
  }
}
```

- 我们可以用下面的函数声明来定义 `@enumerable` 装饰器：

```typescript
function enumerable(value: boolean) {
  return function(target: any, property: string, descriptor: PropertyDescriptor) {
      descriptor.enumerable = value;
  }
}
```

> 这里的 `@enumerable(false)` 是一个装饰器工厂，当装饰器 `@enumerable(false)` 被调用时，它会修改属性描述符的 `enumerable` 属性。

<a name="e4eb644d"></a>

### 21.8 访问器装饰器

> 访问器装饰器声明在一个访问器的声明之前(紧靠着访问器声明)。访问器装饰器 应用于访问器的 `属性描述符` 并且可以用来监视，修改或替换一个访问器的定义。访问器装饰器不能用在声明文件中( `.d.ts` )，或者任何外部上下文(比如 `declare` 的类)里。

> 注意：`TypeScript` 不允许同时装饰一个成员的 `get` 和 `set` 访问器。取而代之的是，一个成员的所有装饰的必须应用在文档顺序的第一个访问器上。这是因为，在装饰器应用于一个属性描述符时，它联合了 `get` 和 `set` 访问器，而不是分开声明的。

> 访问器装饰器表达式会在运行时当作函数被调用，传入下列 `3` 个参数：
>
> 1. 对于静态成员来说是类的构造函数，对于实例成员来说是类的原型对象。
> 2. 成员的名字
> 3. 成员的属性描述符

> 注意：如果代码输出目标版本小于 `ES5`，`Property Descriptor` 将会是 `undefined`。

> 如果访问器装饰器返回一个值，它会被用作方法的 `属性描述符` 注意：如果代码输出目标版本小于 `ES5` 返回值会被忽略。

- 下面是使用了访问器装饰器 (`@configurable`) 的例子，应用于 `Point` 类的成员上。

```typescript
class Point {
  private _x: number;
  private _y: number;
  constructor(x: number, y: number) {
      this._x = x;
      this._y = y;
  }

  @configurable(false)
  get x() { return this._x; }

  @configurable(false)
  get y() { return this._y; }
}
let p: Point = new Point(1, 2);
console.log(p);
```

- 我们可以通过如下函数声明来定义 `@configurable` 装饰器：

```typescript
function configurable(value: boolean) {
  return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
      descriptor.configurable = value;
  }
}
```

<a name="660a6ed4"></a>

### 21.9 属性装饰器

> 属性装饰器声明在一个属性声明之前(紧靠着属性声明)。属性装饰器不能用在声明文件中（`.d.ts`），或者任何外部上下文（比如 `declare` 的类）里。

> 属性装饰器表达式会在运行时当作函数被调用，传入下列 `2` 个参数：
>
> 1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
> 2. 成员的名字。

> 注意：`属性描述符` 不会作为参数传入属性装饰器，这与 `TypeScript` 是如何初始化属性装饰器有关。因为目前没有办法在定义一个原型对象的成员时描述一个实例属性，并且没办法监视或修改一个属性的初始化方法。返回值也会被忽略。因此，属性描述符只能用来监视类中是否声明了某个名字的属性。

- 我们可以用它来记录这个属性的元数据，如下例所示：

```typescript
class Greeter {
    @format("Hello, %s")
    greeting: string;
>
    constructor(message: string) {
        this.greeting = message;
    }
>
    greet() {
        let formatString = getFormat(this, "greeting");
        return formatString.replace("%s", this.greeting);
    }
}
>
let g: Greeter = new Greeter("鼓励");
console.log(g);
>
// 然后定义 @format 装饰器 和 getFormat 函数
import "reflect-metadata";
>
const formatMetadataKey = Symbol("format");
>
function format(formatString: string) {
    return Reflect.metadata(formatMetadataKey, formatString);
}
>
function getFormat(target: any, propertyKey: string) {
    return Reflect.getMetadata(formatMetadataKey, target, propertyKey);
}
```

> 这个 `@format("Hello, %s")` 装饰器是个 `装饰器工厂`。当 `@format("Hello, %s")` 被调用时，它添加一条这个属性的元数据，通过 `reflect-metadata` 库里的 `Reflect.metadata` 函数。当 `getFormat` 被调用时，它读取格式的元数据。

> 注意：这个例子需要使用 `reflect-metadata` 库。查看 元数据 了解 `reflect-metadata` 库更详细的信息。

<a name="8e1c48a1"></a>

### 21.10 参数装饰器

> `参数装饰器` 声明在一个参数声明之前(紧靠着参数声明)。参数装饰器应用于 `类构造函数` 或 `方法声明`。参数装饰器 不能用在声明文件（`.d.ts`），重载或其它外部上下文（比如 `declare` 的类）里。
> `参数装饰器` 表达式会在运行时当作函数被调用，传入下列 `3` 个参数：
>
> 1. 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
> 2. 成员的名字
> 3. 参数在函数参数列表的索引

> 注意：参数装饰器只能用来监视一个方法的参数是否被传入。

> 参数装饰器的返回值会被忽略

- 下面定义了参数装饰器( `@required` )并应用于 `Greeter` 类方法的一个参数：

```typescript
class Greeter {
  greeting: string;

  constructor(message: string) {
      this.greeting = message;
  }

  @validate
  greet(@required name: string) {
      return "Hello " + name + ", " + this.greeting;
  }
}
```

- 然后我们使用下面的函数定义 `@required` 和 `@validate` 装饰器

```typescript
import "reflect-metadata";

const requiredMetadataKey = Symbol("required");

function required(target: Object, propertyKey: string | symbol, parameterIndex: number) {
    let existingRequiredParameters: number[] =
        Reflect.getOwnMetadata(requiredMetadataKey, target, propertyKey) || [];
    existingRequiredParameters.push(parameterIndex);
    Reflect.defineMetadata(requiredMetadataKey, existingRequiredParameters, target, propertyKey);
}

function validate(target: any, propertyName: string, descriptor: TypedPropertyDescriptor<Function>) {
    let method = descriptor.value;
    descriptor.value = function () {
        let requiredParameters: number[] =
            Reflect.getOwnMetadata(requiredMetadataKey, target, propertyName);
        if (requiredParameters) {
            for (let parameterIndex of requiredParameters) {
                if (parameterIndex >= arguments.length || arguments[parameterIndex] === undefined) {
                    throw new Error("Missing required argument.")
                }
            }
        }
        return method.apply(this, arguments);
    }
}
```

> `@required` 装饰器添加了元数据实体把参数标记为必须的。
> `@validate` 装饰器把 `greet` 方法包裹在一个函数里，在调用原先的函数前验证函数参数。
> `注意：`这个例子使用了 `reflect-metadata` 库。 查看 元数据 了解 `reflect-metadata` 库的更多信息。

<a name="51ead784"></a>

### 21.11 元数据

> 一部分例子使用了 `reflect-metadata` 库来支持实现性的 `metadata API`。这个库还不是 `ECMAScript\( JavaScript \)` 标准的一部分。然而，当装饰器被 `ECMAScript` 官方标准采纳后，这些扩展也将被推荐给 `ECMAScript` 以采纳。

- 你可以通过 `npm` 安装这个库

```bash
npm i reflect-metadata --save
```

> `TypeScript` 支持为带有装饰器的声明生成元数据。你需要在 `命令行` 或 `tsconfig.json` 里启用 `emitDecoratorMetadata` 编译器选项。

- 命令行：

```bash
tsc --target ES5 --experimentalDecorators --emitDecoratorMetadata
```

- `tsconfig.json`

```json
{
    "compilerOptions": {
        "target": "ES5",
        "experimentalDecorators": true,
        "emitDecoratorMetadata": true
    }
}
```

> 当启用了，只要 `reflect-metadata` 库被引入了，设计阶段添加的类型信息可以在运行时使用。

- 如下例所示：

```typescript
import "reflect-metadata";

class Point {
    x: number;
    y: number;
}

class Line {
    private _p0: Point;
    private _p1: Point;

    @validate
    set p0(value: Point) {
        this._p0 = value;
    }

    get p0() {
        return this._p0;
    }

    @validate
    set p1(value: Point) {
        this._p1 = value;
    }

    get p1() {
        return this._p1;
    }
}

function validate<T>(target: any, propertyKey: string, descriptor: TypedPropertyDescriptor<T>) {
    let set = descriptor.set;
    descriptor.set = function (value: T) {
        let type = Reflect.getMetadata("design:type", target, propertyKey);
        if (!(value instanceof type)) {
            throw new TypeError("Invalid type.")
        }
        set(value);
    }
}
```

- `TypeScript` 编译器可以通过 `@Reflect.metadata` 装饰器注入设计阶段的类型信息。你可以认为它相当于下面的 `TypeScript：`

```typescript
class Line1 {
    private _p0: Point;
    private _p1: Point;

    @validate
    @Reflect.metadata("design:type", Point)
    set p0(value: Point) {
        this._p0 = value;
    }

    get p0() {
        return this._p0;
    }

    @validate
    @Reflect.metadata("design:type", Point)
    set p1(value: Point) {
        this._p1 = value;
    }

    get p1() {
        return this._p1;
    }
}
```

> 注意： 装饰器元数据是个实验性的特性并且可能在以后的版本中发声破坏性的改变。

<a name="9b88289e"></a>

### 21.12 Reflect-metadata 库

```typescript
namespace Reflect {
    // 用于装饰器
    metadata(k, v): (target, property?) => void
    // 在对象上面定义元数据
    defineMetadata(k, v, o, p?): void
    // 是否存在元数据
    hasMetadata(k, o, p?): boolean
    hasOwnMetadata(k, o, p?): boolean
    // 获取元数据
    getMetadata(k, o, p?): any
    getOwnMetadata(k, o, p?): any
    // 获取所有元数据的 Key
    getMetadataKeys(o, p?): any[]
    getOwnMetadataKeys(o, p?): any[]
    // 删除元数据
    deleteMetadata(k, o, p?): boolean
}
```

<a name="5994b05d"></a>

#### 21.12.1 安装

<a name="d3a69416"></a>

##### Metadata Reflection API

- [Detailed proposal][Metadata-Spec]

1. Installation

```bash
npm install reflect-metadata
```

2. Background

- Decorators add the ability to augment a class and its members as the class is defined, through a declarative syntax.
- Traceur attaches annotations to a static property on the class.
- Languages like C# (.NET), and Java support attributes or annotations that add metadata to types, along with a reflective API for reading metadata.

3. Goals

- A number of use cases (Composition/Dependency Injection, Runtime Type Assertions, Reflection/Mirroring, Testing) want the ability to add additional metadata to a class in a consistent manner.
- A consistent approach is needed for various tools and libraries to be able to reason over metadata.
- Metadata-producing decorators (nee. "Annotations") need to be generally composable with mutating decorators.
- Metadata should be available not only on an object but also through a Proxy, with related traps.
- Defining new metadata-producing decorators should not be arduous or over-complex for a developer.
- Metadata should be consistent with other language and runtime features of ECMAScript.

4. Syntax

- Declarative definition of metadata:

```javascript
class C {
  @Reflect.metadata(metadataKey, metadataValue)
  method() {
  }
}
```

- Imperative definition of metadata:

```javascript
Reflect.defineMetadata(metadataKey, metadataValue, C.prototype, "method");
```

- Imperative introspection of metadata:

```javascript
let obj = new C();
let metadataValue = Reflect.getMetadata(metadataKey, obj, "method");
```

5. Semantics

- Object has a new \[\[Metadata]] internal property that will contain a Map whose keys are property keys (or `undefined` ) and whose values are Maps of metadata keys to metadata values.
- Object will have a number of new internal methods for \[\[DefineOwnMetadata]], \[\[GetOwnMetadata]], \[\[HasOwnMetadata]], etc.
  - These internal methods can be overridden by a Proxy to support additional traps.
  - These internal methods will by default call a set of abstract operations to define and read metadata.
- The Reflect object will expose the MOP operations to allow imperative access to metadata.
- Metadata defined on class declaration *C* is stored in *C*.\[\[Metadata]], with `undefined` as the key.
- Metadata defined on static members of class declaration *C* are stored in *C*.\[\[Metadata]], with the property key as the key.
- Metadata defined on instance members of class declaration *C* are stored in *C*.prototype.\[\[Metadata]], with the property key as the key.

1. API

```javascript
// define metadata on an object or property
Reflect.defineMetadata(metadataKey, metadataValue, target);
Reflect.defineMetadata(metadataKey, metadataValue, target, propertyKey);

// check for presence of a metadata key on the prototype chain of an object or property
let result = Reflect.hasMetadata(metadataKey, target);
let result = Reflect.hasMetadata(metadataKey, target, propertyKey);

// check for presence of an own metadata key of an object or property
let result = Reflect.hasOwnMetadata(metadataKey, target);
let result = Reflect.hasOwnMetadata(metadataKey, target, propertyKey);

// get metadata value of a metadata key on the prototype chain of an object or property
let result = Reflect.getMetadata(metadataKey, target);
let result = Reflect.getMetadata(metadataKey, target, propertyKey);

// get metadata value of an own metadata key of an object or property
let result = Reflect.getOwnMetadata(metadataKey, target);
let result = Reflect.getOwnMetadata(metadataKey, target, propertyKey);

// get all metadata keys on the prototype chain of an object or property
let result = Reflect.getMetadataKeys(target);
let result = Reflect.getMetadataKeys(target, propertyKey);

// get all own metadata keys of an object or property
let result = Reflect.getOwnMetadataKeys(target);
let result = Reflect.getOwnMetadataKeys(target, propertyKey);

// delete metadata from an object or property
let result = Reflect.deleteMetadata(metadataKey, target);
let result = Reflect.deleteMetadata(metadataKey, target, propertyKey);

// apply metadata via a decorator to a constructor
@Reflect.metadata(metadataKey, metadataValue)
class C {
  // apply metadata via a decorator to a method (property)
  @Reflect.metadata(metadataKey, metadataValue)
  method() {
  }
}
```

7. Alternatives

- Use properties rather than a separate API.
  - Obvious downside is that this can be a lot of code:

```javascript
function ParamTypes(...types) {
  return (target, propertyKey) => {
    const symParamTypes = Symbol.for("design:paramtypes");
    if (propertyKey === undefined) {
      target[symParamTypes] = types;
    }
    else {
      const symProperties = Symbol.for("design:properties");
      let properties, property;
      if (Object.prototype.hasOwnProperty.call(target, symProperties)) {
        properties = target[symProperties];
      }
      else {
        properties = target[symProperties] = {};
      }
      if (Object.prototype.hasOwnProperty.call(properties, propertyKey)) {
        property = properties[propertyKey];
      }
      else {
        property = properties[propertyKey] = {};
      }
      property[symParamTypes] = types;
    }
  };
}
```

8. Notes

- Though it may seem counterintuitive, the methods on Reflect place the parameters for the metadata key and metadata value before the target or property key. This is due to the fact that the property key is the only optional parameter in the argument list. This also makes the methods easier to curry with Function#bind. This also helps reduce the overall footprint and complexity of a metadata-producing decorator that could target both a class or a property:

```javascript
function ParamTypes(...types) {
  // as propertyKey is effectively optional, its easier to use here
  return (target, propertyKey) => { Reflect.defineMetadata("design:paramtypes", types, target, propertyKey); }

  // vs. having multiple overloads with the target and key in the front:
  //
  // return (target, propertyKey) => {
  //    if (propertyKey === undefined) {
  //      Reflect.defineMetadata(target, "design:paramtypes", types);
  //    }
  //    else {
  //      Reflect.defineMetadata(target, propertyKey, "design:paramtypes", types);
  //    }
  // }
  //
  // vs. having a different methods for the class or a property:
  //
  // return (target, propertyKey) => {
  //    if (propertyKey === undefined) {
  //      Reflect.defineMetadata(target, "design:paramtypes", types);
  //    }
  //    else {
  //      Reflect.definePropertyMetadata(target, propertyKey, "design:paramtypes", types);
  //    }
  // }
}
```

- To enable experimental support for metadata decorators in your TypeScript project, you must add `"experimentalDecorators": true` to your tsconfig.json file.
- To enable experimental support for auto-generated type metadata in your TypeScript project, you must add `"emitDecoratorMetadata": true` to your tsconfig.json file.
  - Please note that auto-generated type metadata may have issues with circular or forward references for types.

9. Issues

- A poorly written mutating decorator for a class constructor could cause metadata to become lost if the prototype chain is not maintained. Though, not maintaining the prototype chain in a mutating decorator for a class constructor would have other negative side effects as well. [@rbuckton ](/rbuckton)
  - This is mitigated if the mutating decorator returns a class expression that extends from the target, or returns a proxy for the decorator. [@rbuckton ](/rbuckton)
- Metadata for a method is attached to the class (or prototype) via the property key. It would not then be available if trying to read metadata on the function of the method (e.g. "tearing-off" the method from the class). [@rbuckton ](/rbuckton)

[Metadata-Spec]: [https://rbuckton.github.io/reflect-metadata]\(https://rbuckton.github.io/reflect-metadata\)

<a name="d537846c"></a>

## 二十二、Mixins 混入

<a name="205253f1"></a>

### 22.1 介绍

> 除了传统的面向对象继承方式，还流行一种通用可重用组件创建类的方式，就是联合另一个简单类的代码。
> 你可能在 `Scala` 等语言里对 `mixins` 及其特性已经很熟悉了，但它在 `JavaScript` 中也是很流行的。

<a name="70e9f031"></a>

### 22.2 混入示例

> 下面的代码演示了如何在 `TypeScript` 里使用混入。后面我们还会解释这段代码是如何工作的。

```typescript
// Disposable Mixin
class Disposable {
    isDisposed: boolean;
    dispose() {
        this.isDisposed = true;
    }
}

// Activatable Mixin
class Activatable {
    isActive: boolean;
    activate() {
        this.isActive = true;
    }
    deactivate() {
        this.isActive = false;
    }
}

class SmartObject implements Disposable, Activatable {
    constructor() {
        setInterval(() => console.log(this.isActive + ":" + this.isDisposed), 500)
    }

    interact() {
        this.activate()
    }

    // Disposable
    isDisposed: boolean = false;
    dispose: () => void;
    // Activatable
    isActive: boolean = false;
    activate: () => void;
    deactivate: () => void;
}

applyMixins(SmartObject, [Disposable, Activatable]);
let smartObj = new SmartObject();
setTimeout(() => smartObj.interact(), 1000);

////////////////////////////////////////
// In your runtime library somewhere
////////////////////////////////////////

function applyMixins(derivedCtor: any, baseCtors: any[]) {
    baseCtors.forEach(baseCtor => {
        Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
            derivedCtor.prototype[name] = baseCtor.prototype[name]
        })
    })
}
```

<a name="456b65ec"></a>

### 22.3 理解这个例子

1. 代码里首先定义了两个类，它们将做为 `mixins`。可以看到每个类都只定义了一个特定的行为或功能。稍后我们使用它们来创建一个新类，同时具有这两种功能。

```typescript
    // Disposable Mixin
    class Disponsable {
        isDisponsed: boolean;
        dispose() {
            this.isDisponsed = true;
        }
    }

    // Activatable Mixin
    class Activatable {
        isActive: boolean;
        activate() {
            this.isActive = true;
        }
        deactivate() {
            this.isActive = false;
        }
    }
```

2. 下面创建一个类，结合了这两个 `mixins`。 下面来看一下具体是怎么操作的：

```typescript
class SmartObject implements Disposable, Activatable { }
```

3. 首先应该注意到的是，没使用 `extends` 而是使用 `implements`。 把类当成了接口，仅使用 `Disposable` 和 `Activatable` 的类型而非其实现。 这意味着我们需要在类里面实现接口。但是这是我们在用 `mixin` 时想避免的。
4. 我们可以这么做来达到目的，为将要 `mixin` 进来的属性方法创建出占位属性。这告诉编译器这些成员在运行时是可用的。 这样就能使用 `mixin` 带来的便利，虽说需要提前定义一些占位属性。

```typescript
// Disposable
  isDisposed: boolean = false;
  dispose: () => void;
// Activatable
  isActive: boolean = false;
  activate: () => void;
  deactivate: () => void;
```

5. 最后，把 `mixins` 混入定义的类，完成全部实现部分。

```typescript
applyMixins(SmartObject, [Disposable, Activatable]);
```

6. 最后，创建这个帮助函数，帮我们做混入操作。 它会遍历 `mixins` 上的所有属性，并复制到目标上去，把之前的占位属性替换成真正的实现代码。

```typescript
function applyMixins(derivedCtor: any, baseCtors: any[]) {
    baseCtors.forEach(baseCtor => {
        Object.getOwnPropertyNames(baseCtor.prototype).forEach(name => {
            derivedCtor.prototype[name] = baseCtor.prototype[name];
        })
    });
}
```

<a name="842964ab"></a>

## 二十三、三斜线指令

- 三斜线指令是包含单个 `XML` 标签的单行注释。注释内容会做为编译器指令使用。
- 三斜线指令仅可放在包含它的文件最顶端。一个三斜线指令的前面只能出现单行或多行注释，这包括其它的三斜线指令。如果它们出现在一个语句或声明之后，那么它们会被当作普通的单行注释，并且不具有特殊的含义。

```typescript
/// <reference path="..." />
/// <reference path="..." /> 指令是三斜线指令中最常见的一种。 它用于声明文件间的 依赖。
```

- 三斜线引用告诉编译器在编译过程中要引入的额外文件。
- 当使用 `--out` 或 `--outFile` 时，它也可以做为调整输出内容顺序的一种方法。文件在输出文件内容中的位置与经过预处理后的输入顺序一致。

<a name="4803f8d5"></a>

### 23.1 预处理输入文件

> 编译器会对输入文件进行预处理来解析所有三斜线引用指令。在这个过程中，额外的文件会加到编译过程中。
> 这个过程会以一些根文件开始；它们是在命令行中指定的文件或是在 `tsconfig.json` 中的 `"files"` 列表里的文件。这些根文件按指定的顺序进行预处理。在一个文件被加入列表前，它包含的所有三斜线引用都要被处理，还有它们包含的目标。三斜线引用以它们在文件里出现的顺序，使用深度优先的方式解析。
>  一个三斜线引用路径是相对于包含它的文件的，如果不是根文件。

<a name="a93afbae"></a>

### 23.2 错误

> 引用不存在的文件会报错。一个文件用三斜线指令引用自己会报错。

<a name="d0bea2cf"></a>

### 23.3 使用 `--noResolve`

> 如果指定了 `--noResolve` 编译选项，三斜线指令会被忽略；它们不会增加新文件，也不会改变给定文件的顺序。

```typescript
/// <reference types="..." />
```

- 与 `/// <reference path="..." />` 指令相似，这个指令是用来声明依赖的；一个 `/// <reference types="..." />` 指令则声明了对某个包的依赖。
- 对这些包的名字的解析与在 `import` 语句里对模块名的解析类似。可以简单地把三斜线类型引用指令当做 `import` 声明的包。
- 例如，把 `/// <reference types="node" />`引入到声明文件，表明这个文件使用了`@types/node/index.d.ts` 里面声明的名字；并且，这个包需要在编译阶段与声明文件一起被包含进来。
- 仅当在你需要写一个 `.d.ts` 文件时才使用这个指令。
- 对于那些在编译阶段生成的声明文件，编译器会自动地添加 `/// <reference types="..." />`；当且仅当结果文件中使用了引用的包里的声明时才会在生成的声明文件里添加  `/// <reference types="..." />` 语句。
- 若要在 `.ts` 文件里声明一个对 `@types` 包的依赖，使用 `--types` 命令行选项 或在 `tsconfig.json` 里指定。查看 在 `tsconfig.json` 里使用 `@types`，`typeRoots` 和 `types` 了解详情。
- `/// <reference no-default-lib="true"/>` 这个指令把一个文件标记成默认库。你会在 `lib.d.ts` 文件和它不同的变体的顶端看到这个注释。这个指令告诉编译器在编译过程中不要包含这个默认库（比如，`lib.d.ts`）。 这与在命令行上使用 -`-noLib` 相似。
- 还要注意，当传递了 `--skipDefaultLibCheck` 时，编译器只会忽略检查带有 `/// <reference no-default-lib="true"/>` 的文件。

<a name="b9dc8450"></a>

### 23.4 `<amd-module />`

> 默认情况下生成的 `AMD` 模块 都是匿名的。但是，当一些工具需要处理生成的模块时会产生问题，比如 `r.js。amd-module` 指令允许给编译器传入一个可选的模块名：

- `amdModule.ts`

```typescript
/// <amd-module name="NamedModule" />
export class C {

}
```

> 这会将 `NamedModule` 传入到 `AMD define` 函数里：

- `amdModule.js`

```typescript
define("NamedModule", ["require", "exports"], function(require, exports) {
  var C = (function() {
    function C() {

    }
    return C;
  })();
  exports.C = C;
});
```

<a name="1aa1471a"></a>

### 23.5 `<amd-dependency />`

- 注意：这个指令被废弃了。使用 `import "moduleName";` 语句代替。

```typescript
/// <amd-dependency path="x" />

// 告诉编译器有一个非 TypeScript 模块依赖需要被注入，做为目标模块 require 调用的一部分。
```

- `amd-dependency` 指令也可以带一个可选的 `name` 属性；它允许我们为 `amd-dependency` 传入一个可选名字：

```typescript
/// <amd-dependency path="legacy/moduleA" name="moduleA"/>
declare var moduleA:MyType
moduleA.callStuff()
```

- 生成的 `JavaScript` 代码：

```typescript
define(["require", "exports", "legacy/moduleA"], function (require, exports, moduleA) {
    moduleA.callStuff()
});
```

<a name="62f0a9dd"></a>

## 二十四、JavaScript 文件类型检查

<a name="66689ffa"></a>

### 24.1 JavaScript 文件类型检查

> `TypeScript 2.3` 以后的版本支持使用 `--checkJs` 对 `.js` 文件进行类型检查和错误提示。

> 你可以通过添加 `// @ts-nocheck` 注释来忽略类型检查；相反，你可以通过去掉 `--checkJs` 设置并添加一个 `// @ts-nocheck` 注释来选择检查某些 `.js` 文件。你还可以用 `// @ts-ignore` 来忽略本行的错误。如果你使用了 `tsconfig.json`，`JS` 检查将遵照一些严格检查标记，如 `noImplicitAny`，`strictNullChecks` 等。但因为 `JS` 检查是相对宽松的，在使用严格标记时可能会有些出乎意料的情况。

- 对比 `.js`文件和 `.ts` 文件在类型检查上的差异，有如下几点需要注意

<a name="9249efab"></a>

#### (1) 用 `JSDoc` 类型表示类型信息

> `.js` 文件里，类型可以和在 `.ts` 文件里一样被推断出来。同样地，当类型不能被推断时，它们可以通过 `JSDoc` 来指定，就好比在 `.ts` 文件里那样。如同 `TypeScript`，`--noImplicitAny` 会在编译器无法推断类型的位置报错。（除了对象字面量的情况；后面会详细介绍）`JSDoc` 注解修饰的声明会被设置为这个声明的类型。比如：

```typescript
/** @type {number} */
var x;
x = 0;      // OK
x = false;  // Error: boolean is not assignable to number
```

> 你可以在这里找到所有 `JSDoc` 支持的模式，[JSDoc 文档](https://github.com/Microsoft/TypeScript/wiki/JSDoc-support-in-JavaScript)。

<a name="a8137367"></a>

#### (2) 属性的推断来自于类内的赋值语句

> `ES2015` 没提供声明类属性的方法。属性是动态赋值的，就像对象字面量一样。在 `.js` 文件里，编译器从类内部的属性赋值语句来推断属性类型。属性的类型是在构造函数里赋的值的类型，除非它没在构造函数里定义或者在构造函数里是 `undefined` 或 `null`。若是这种情况，类型将会是所有赋值的类型的联合类型。在构造函数里定义的属性会被认为是一直存在的，然而那些在方法，存取器里定义的属性被当成可选的。

```typescript
class C {
    constructor() {
        this.constructorOnly = 0
        this.constructorUnknown = undefined
    }
    method() {
        this.constructorOnly = false // error, constructorOnly is a number
        this.constructorUnknown = "plunkbat" // ok, constructorUnknown is string | undefined
        this.methodOnly = 'ok'  // ok, but y could also be undefined
    }
    method2() {
        this.methodOnly = true  // also, ok, y's type is string | boolean | undefined
    }
}
```

> 如果一个属性从没在类内设置过，它们会被当成未知的。

> 如果类的属性只是读取用的，那么就在构造函数里用 `JSDoc` 声明它的类型。如果它稍后会被初始化，你甚至都不需要在构造函数里给它赋值：

```typescript
class C {
    constructor() {
        /** @type {name | undefined} */
        this.prop = undefined;
        /** @type {number | undefined} */
        this.count;
    }
}
let c = new C();
c.prop = 0; // Ok
c.count = "string"; // Error: string is not assignable to number | undefined
```

<a name="9a85ec40"></a>

#### (3) 构造函数等同于类

> `ES2015` 以前，`Javascript` 使用构造函数代替类。编译器支持这种模式并能够将构造函数识别为 `ES2015` 的类。属性类型推断机制和上面介绍的一致。

```typescript
function C() {
    this.constructorOnly = 0;
    this.constructorUnknown = undefined
}
C.prototype.method = function() {
    this.constructorOnly = false; // error
    this.constructorUnknown = "plunkbat" // OK, the type is string | undefined
};
```

<a name="a25a4140"></a>

#### (4) 支持 `CommonJS` 模块

> 在 `.js` 文件里，`TypeScript` 能识别出 `CommonJS` 模块。对 `exports` 和 `module.exports` 的赋值被识别为导出声明。相似地，`require` 函数调用被识别为模块导入。例如：

```typescript
// same as `import module 'fs'`
const fs = require('fs');
// same as `export function readFile`
module.exports.readFile = function(f) {
    return fs.readFileSync(f);
};
```

> 对 `JavaScript` 文件里模块语法的支持比在 `TypeScript` 里宽泛多了。 大部分的赋值和声明方式都是允许的。

<a name="df2ae1da"></a>

#### (5) 类，函数和对象字面量是命名空间

```typescript
// .js 文件里的类是命名空间。它可以用于嵌套类，比如：
    class C { }
    C.D = class {};
// ES2015 之前的代码，它可以用来模拟静态方法。
    function Outer() {
        this.y = 2;
    }
    Outer.Inner = function() {
        this.yy = 2;
    }
// 它还可以用于创建简单的命名空间：
    var ns = {}
    ns.C = class { }
    ns.func = function() { }
// 同时还支持其它的变化
    // 立即调用的函数表达式
    var ns = (function(n) {
        return n | {};
    })();
    ns.COUNT = 1

// defaulting to global
var assign = assign || function() {
    // code goes here
}
assign.extra = 1;
```

<a name="2116a0b6"></a>

#### (6) 对象字面量是开放的

- `.ts` 文件里，用对象字面量初始化一个变量的同时也给它声明了类型。 新的成员不能再被添加到对象字面量中。这个规则在 `.js` 文件里被放宽了；对象字面量具有开放的类型，允许添加并访问原先没有定义的属性。例如：

```typescript
var obj = { a: 1 };
obj.b = 2;  // Allowed
```

- 对象字面量的表现就好比具有一个默认的索引签名 `\[x:string\]: any`，它们可以被当成开放的映射而不是封闭的对象。
- 与其它 `JS` 检查行为相似，这种行为可以通过指定 `JSDoc` 类型来改变，例如：

```typescript
/** @type {{ a: number }} */
var obj = { a: 1 };
obj.b = 2;  // Error, type {a: number} does not have property b
```

<a name="38f54f92"></a>

#### (7) null undefined 和 空数组的类型是 any 或 any\[]

> 任何用 `null`，`undefined` 初始化的变量，参数或属性，它们的类型是 `any`，就算是在严格 `null` 检查模式下。任何用 `[]` 初始化的变量，参数或属性，它们的类型是 `any[]`，就算是在严格 `null` 检查模式下。 唯一的例外是像上面那样有多个初始化器的属性。

```typescript
function Foo(i = null) {
    if (!i) i = 1;
    var j = undefined;
    j = 2;
    this.l = [];
}
var foo = new Foo();
foo.l.push(foo.i);
foo.l.push("end");
```

<a name="d3a114e7"></a>

#### (8) 函数参数是默认可选的

> 由于在 `ES2015` 之前无法指定可选参数，因此 `.js` 文件里所有函数参数都被当做是可选的。 使用比预期少的参数调用函数是允许的。

> 需要注意的一点是，使用过多的参数调用函数会得到一个错误。

- 例如：

```typescript
function bar(a, b) {
    console.log(a + " " + b);
}
bar(1);       // OK, second argument considered optional
bar(1, 2);
bar(1, 2, 3); // Error, too many arguments
```

- 使用 `JSDoc` 注解的函数会被从这条规则里移除。使用 `JSDoc` 可选参数语法来表示可选性。

```typescript
// 比如：
/**
 * @param {string} [somebody] - Somebody's name.
  */
function sayHello(somebody) {
    if (!somebody) {
        somebody = 'John Doe';
    }
    console.log('Hello ' + somebody);
}
sayHello();
```

<a name="7e27d164"></a>

#### (9) 由 `arguments` 推断出的 `var-args` 参数声明

> 如果一个函数的函数体内有对 `arguments` 的引用，那么这个函数会隐式地被认为具有一个 `var-arg` 参数（比如: `(...arg: any[]) =>; any)`）。使用 `JSDoc` 的 `var-arg` 语法来指定 `arguments` 的类型。

```typescript
/** @param {...number} args */
function sum(/* numbers */) {
    var total = 0
    for (var i = 0; i < arguments.length; i++) {
        total += arguments[i]
    }
    return total
}
```

<a name="30f29d14"></a>

#### (10) 未指定的类型参数默认为 `any`

> 由于 `JavaScript` 里没有一种自然的语法来指定泛型参数，因此未指定的参数类型默认为 `any`。

<a name="0f75a8b4"></a>

#### (11) 在 `extends` 语句中：

> 例如，`React.Component` 被定义成具有两个类型参数，`Props` 和 `State`。 在一个 `.js` 文件里，没有一个合法的方式在 `extends` 语句里指定它们。默认地参数类型为 `any`：

```typescript
import { Component } from "react";

class MyComponent extends Component {
    render() {
        this.props.b; // Allowed, since this.props is of type any
    }
}
```

> 使用 `JSDoc` 的 `@augments` 来明确地指定类型。

- 例如：

```typescript
import { Component } from "react";
/**
 * @augments {Component<{a: number}, State>}
 */
 class MyComponent extends Component {
    render() {
        this.props.b; // Error: b does not exist on {a:number}
    }
}
```

<a name="d851d303"></a>

#### (12) 在 JSDoc 引用中

```typescript
/** @type{Array} */
var x = [];

x.push(1);        // OK
x.push("string"); // OK, x is of type Array<any>

/** @type{Array.<number>} */
var y = [];

y.push(1);        // OK
y.push("string"); // Error, string is not assignable to number
```

<a name="41c49c9c"></a>

#### (13) 在函数调用中

> 泛型函数的调用使用 `arguments` 来推断泛型参数。有时候，这个流程不能够推断出类型，大多是因为缺少推断的源；在这种情况下，类型参数类型默认为 `any`。

```typescript
// 例如：
var p = new Promise((resolve, reject) => { reject() });
p; // Promise<any>;
```

<a name="ef23eb58"></a>

### 24.2 支持的 JSDoc

> 面的列表列出了当前所支持的 `JSDoc` 注解，你可以用它们在 `JavaScript` 文件里添加类型信息。 注意，没有在下面列出的标记（例如 `@async`）都是还不支持的。

```typescript
1.  @type
2.  @param (or @arg or @argument)
3.  @returns (or @return)
4.  @typedef
5.  @callback
6.  @template
7.  @class (or @constructor)
8.  @this
9.  @extends (or @augments)
10. @enum
```

> 它们代表的意义与 `usejsdoc.org` 上面给出的通常是一致的或者是它的超集。 下面的代码描述了它们的区别并给出了一些示例。

<a name="efd77a7d"></a>

#### (1) [@type ](/type)

> 可以使用 `@type` 标记并引用一个类型名称（原始类型，`TypeScript` 里声明的类型，或在 `JSDoc` 里 `@typedef` 标记指定的）可以使用任何 `TypeScript` 类型和大多数 `JSDoc` 类型。

```typescript
/** @type {string} */
var s;

/** @type {Window} */
var win;

/** @type {PromiseLike<string>} */
var promisedString;

// You can specify an HTML Element with DOM properties
/** @type {HTMLElement} */

var myElement = document.querySelector(selector);
element.dataset.myData = '';
```

- `@type` 可以指定联合类型—例如，`string` 和 `boolean` 类型的联合。

```typescript
/** @type {{string | boolean}} */
var sb;
```

- 注意：括号是可选的

```typescript
/** @type {string | boolean} */
var sb;
```

- 有多种方式来指定数组类型

```typescript
/** @type {number[]} */
var ns;
/** @type {Array.<number>} */
var nds;
/** @type {Array<number>} */
var nas;
```

- 还可以指定对象字面量类型。例如，一个带有 `a(字符串)` 和 `b(数字)` 属性的对象，使用下面的语法：

```typescript
/** @type {{a: string, b: number}} */
var var9;
```

- 可以使用字符串和数字索引签名来指定 `map-like` 和 `array-like` 的对象，使用标准的 `JSDoc` 语法或者 `TypeScript` 语法。

```typescript
/**
 * A map-like object that maps arbitrary `string` properties to `number`s.
 *
 * @type {Object.<string, number>}
 * */
var stringToNumber;
/** @type {Object.<number, object>} */
var arrayLike;
```

- 这两个类型与 `TypeScript` 里的 `{ [x: string]: number }` 和 `{ [x: number]: any }` 是等同的。 编译器能识别出这两种语法。
- 可以使用 `TypeScript` 或 `Closure` 语法指定函数类型。

```typescript
/** @type {function(string, boolean): number} Closure syntax */
var sbn;
/** @type {(s: string, b: boolean) => number} Typescript syntax */
var sbn2;
```

- 或者直接使用未指定的 `Function` 类型：

```typescript
  /** @type {Function} */
  var fn7;
  /** @type {function} */
  var fn6;
```

- `Closure` 的其它类型也可以使用：

```typescript
/** @type {*} - can be 'any' type */
var star;
/** @type {?} - unknown type (same as 'any') */
var question;
```

<a name="7bdf7965"></a>

#### (2) 转换

> `TypeScript` 借鉴了 `Closure` 里的转换语法。在括号表达式前面使用 `@type` 标记，可以将一种类型转换成另一种类型

```typescript
/**
 * @type {number | string}
 */
var numberOrString = Math.random() < 0.5 ? "hello" : 100;
var typeAssertedNumber = /** @type {number} */ (numberOrString)
```

<a name="1e8ba8a6"></a>

#### (3) 导入类型

> 可以使用导入类型从其它文件中导入声明。 这个语法是 `TypeScript` 特有的，与 `JSDoc` 标准不同：

```typescript
/**
 * @param p { import("./a").Pet }
  */
function walk(p) {
    console.log(`Walking ${p.name}...`);
}
```

> 导入类型也可以使用在类型别名声明中：

```typescript
/**
 * @typedef Pet { import("./a").Pet }
 */

/**
 * @type {Pet}
 */
var myPet;
myPet.name;
```

> 导入类型可以用在从模块中得到一个值的类型。

```typescript
/**
 * @type {typeof import("./a").x }
 */
var x = require("./a").x;
```

<a name="8621467c"></a>

#### (4) [@param ](/param) 和 [@returns ](/returns)

> `@param` 语法 和 `@type` 相同，但增加了一个参数名。 使用 `[]` 可以把参数声明为可选的：

```typescript
 // Parameters may be declared in a variety of syntactic forms
 /**
  * @param {string}  p1 - A string param.
  * @param {string=} p2 - An optional param (Closure syntax)
  * @param {string} [p3] - Another optional param (JSDoc syntax).
  * @param {string} [p4="test"] - An optional param with a default value
  * @return {string} This is the result
  */
function stringsStringStrings(p1, p2, p3, p4){
  // TODO
}
```

> 函数的返回值类型也是类似的：

```typescript
/**
 * @return {PromiseLike<string>}
 */
function ps(){}

/**
 * @returns {{ a: string, b: number }} - May use '@returns' as well as '@return'
 */
function ab(){}
```

<a name="b8a8d369"></a>

#### (5) @typedef, @callback, 和 [@param ](/param)

> `@typedef` 可以用来声明复杂类型。和 `@param` 类似的语法。

```typescript
/**
 * @typedef {Object} SpecialType - creates a new type named 'SpecialType'
 * @property {string} prop1 - a string property of SpecialType
 * @property {number} prop2 - a number property of SpecialType
 * @property {number=} prop3 - an optional number property of SpecialType
 * @prop {number} [prop4] - an optional number property of SpecialType
 * @prop {number} [prop5=42] - an optional number property of SpecialType with default
 */
/** @type {SpecialType} */
var specialTypeObject;
```

> 可以在第一行上使用 `object` 或 `Object`。

```typescript
/**
 * @typedef {object} SpecialType1 - creates a new type named 'SpecialType'
 * @property {string} prop1 - a string property of SpecialType
 * @property {number} prop2 - a number property of SpecialType
 * @property {number=} prop3 - an optional number property of SpecialType
 */
/** @type {SpecialType1} */
var specialTypeObject1;
```

> `@param` 允许使用相似的语法。 注意，嵌套的属性名必须使用参数名做为前缀：

```typescript
/**
* @param {Object} options - The shape is the same as SpecialType above
* @param {string} options.prop1
* @param {number} options.prop2
* @param {number=} options.prop3
* @param {number} [options.prop4]
* @param {number} [options.prop5=42]
*/
function special(options) {
   return (options.prop4 || 1001) + options.prop5;
}
```

> `@callback` 与 `@typedef` 相似，但它指定函数类型而不是对象类型：

```typescript
/**
 * @callback Predicate
 * @param {string} data
 * @param {number} [index]
 * @returns {boolean}
 */
/** @type {Predicate} */
const ok = s => !(s.length % 2);
```

> 当然，所有这些类型都可以使用 `TypeScript` 的语法 `@typedef` 在一行上声明：

```typescript
/** @typedef {{ prop1: string, prop2: string, prop3?: number }} SpecialType */
/** @typedef {(data: string, index?: number) => boolean} Predicate */
```

<a name="6f1f282c"></a>

#### (6) [@template ](/template)

> 使用 `@template` 声明泛型：

```typescript
/**
 * @template T
 * @param {T} p1 - A generic parameter that flows through to the return type
 * @return {T}
 */
function id(x){ return x }
```

> 用逗号或多个标记来声明多个类型参数：

```typescript
/**
 * @template T,U,V
 * @template W,X
 */
```

> 还可以在参数名前指定类型约束。只有列表的第一项类型参数会被约束:

```typescript
/**
 * @template {string} K - K must be a string or string literal
  * @template {{ serious(): string }} Seriousalizable - must have a serious method
  * @param {K} key
  * @param {Seriousalizable} object
  */
function seriousalize(key, object) {
    // ????
}
```

<a name="d6b34431"></a>

#### (7) [@constructor ](/constructor)

> 编译器通过 `this` 属性的赋值来推断构造函数, 但你可以让检查更严格提示更友好, 你可以添加一个 `@constructor` 标记:

```typescript
/**
 * @constructor
 * @param {number} data
 */
function C(data) {
    this.size = 0;
    this.initialize(data); // Should error, initializer expects a string
}
/**
 * @param {string} s
 */
C.prototype.initialize = function (s) {
    this.size = s.length
}

var c = new C(0);
var result = C(1); // C should only be called with new
```

> 通过 `@constructor`, `this` 将在构造函数`C`里被检查，因此你在 `initialize` 方法里得到一个提示，如果你传入一个数字你还将得到一个错误提示。如果你直接调用`C`而不是构造它，也会得到一个错误。

> 不幸的是，这意味着那些既能构造也能直接调用的构造函数不能使用 `@constructor`。

<a name="e57f3e4a"></a>

#### (8) [@this ](/this)

> 编译器通常可以通过上下文来推断出 `this` 的类型。但你可以使用 `@this` 来明确指定它的类型：

```typescript
/**
 * @this {HTMLElement}
 * @param {*} e
 */
function callbackForLater(e) {
    this.clientHeight = parseInt(e) // should be fine!
}
```

<a name="167b9a03"></a>

#### (9) [@extends ](/extends)

> 当 `JavaScript` 类继承了一个基类, 无处指定类型参数的类型, 而 `@extends` 标记提供了这样一种方式:

```typescript
/**
 * @template T
 * @extends {Set<T>}
 */
class SortableSet extends Set {
    // ...
}
```

> 注意 `@extends` 只作用于类。当前，无法实现构造函数继承类的情况。

<a name="95e8dbfc"></a>

#### (10) [@enum ](/enum)

> `@enum` 标记允许你创建一个对象字面量, 它的成员都有确定的类型。不同于 `JavaScript` 里大多数的对象字面量，它不允许添加额外成员。

```typescript
/** @enum {number} */
const JSDocState = {
    BeginningOfLine: 0,
    SawAsterisk: 1,
    SavingComments: 2,
}
```

> 注意 `@enum` 与 `TypeScript` 的 `@enum` 大不相同, 它更加简单。然而，不同于 `TypeScript` 的枚举, `@enum` 可以是任何类型:

```typescript
/** @enum {function(number): number} */
const Math = {
    add1: n => n + 1,
    id: n => -n,
    sub1: n => n - 1,
}
```

<a name="61eaa4d9"></a>

#### (11) 更多示例

```tsx
var someObj = {
    /**
     * @param {string} param1 - Docs on property assignments work
     */
    x: function(param1){}
};

/**
 * As do docs on variable assignments
 * @return {Window}
 */
let someFunc = function(){};

/**
 * And class methods
 * @param {string} greeting The greeting to use
 */
Foo.prototype.sayHi = (greeting) => console.log("Hi!");

/**
 * And arrow functions expressions
 * @param {number} x - A multiplier
 */
let myArrow = x => x * x;

/**
 * Which means it works for stateless function components in JSX too
 * @param {{a: string, b: number}} test - Some param
 */
var sfc = (test) => <div>{test.a.charAt(0)}</div>;

/**
 * A parameter can be a class constructor, using Closure syntax.
 *
 * @param {{new(...args: any[]): object}} C - The class to register
 */
function registerClass(C) {}

/**
 * @param {...string} p1 - A 'rest' arg (array) of strings. (treated as 'any')
 */
function fn10(p1){}

/**
 * @param {...string} p1 - A 'rest' arg (array) of strings. (treated as 'any')
 */
function fn9(p1) {
    return p1.join();
}
```

<a name="ced660bd"></a>

#### (12) 已知不支持的模式

> 在值空间中将对象视为类型是不可以的, 除非对象创建了类型, 如构造函数。

```typescript
function aNormalFunction() {

}
/**
 * @type {aNormalFunction}
 */
var wrong;
/**
 * Use 'typeof' instead:
 * @type {typeof aNormalFunction}
 */
var right;
```

> 对象字面量属性上的 `=` 后缀不能指定这个属性是可选的:

```typescript
/**
 * @type {{ a: string, b: number= }}
 */
var wrong;
/**
 * Use postfix question on the property name instead:
 * @type {{ a: string, b?: number }}
 */
var right;
```

> `Nullable` 类型只在启用了 `strictNullChecks` 检查时才启作用:

```typescript
/**
 * @type {?number}
 * With strictNullChecks: true -- number | null
 * With strictNullChecks: off  -- number
 */
var nullable;
```

> `Non-nullable` 类型没有意义, 以其原类型对待:

```typescript
/**
 * @type {!number}
 * Just has type number
 */
var normal;
```

> 不同于 `JSDoc` 类型系统, `TypeScript` 只允许将类型标记为包不包含 `null`。`Non-nullable` -- 如果启用了 `strictNullChecks`，那么 `number` 是 `非null` 的。 如果没有启用，那么 `number` 是可以为 `null` 的。
