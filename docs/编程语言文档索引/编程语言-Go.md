---
title: 编程语言-Go
url: https://www.yuque.com/wcywxq/rddgev/nfs1ot
---

<a name="yAoV2"></a>

## 一、go 基础语法

<a name="dd9ab5ef"></a>

### 1.1 标识符

一个标识符实际上就是一个或是多个字母( `A~Z` 和 `a~z` )数字( `0~9` )、下划线 `_` 组成的序列，但是第一个字符必须是字母或下划线而不能是数字。

<a name="61cfde35"></a>

### 1.2 关键字

| **`break`** | **`default`** | **`func`** | **`interface`** | **`select`** |
| :--- | :--- | :--- | :--- | :--- |
| **`case`** | **`defer`** | **`go`** | **`map`** | **`struct`** |
| **`chan`** | **`else`** | **`goto`** | **`package`** | **`switch`** |
| **`const`** | **`fallthrough`** | **`if`** | **`range`** | **`type`** |
| **`continue`** | **`for`** | **`import`** | **`return`** | **`var`** |

<a name="63122753"></a>

### 1.3 预定义标识符

| **`append`** | **`bool`** | **`byte`** | **`cap`** | **`close`** |
| :--- | :--- | :--- | :--- | :--- |
| **`complex`** | **`complex64`** | **`complex128`** | **`uint16`** | **`copy`** |
| **`false`** | **`float32`** | **`float64`** | **`imag`** | **`int`** |
| **`int8`** | **`int16`** | **`uint32`** | **`int32`** | **`int64`** |
| **`iota`** | **`len`** | **`make`** | **`new`** | **`nil`** |
| **`panic`** | **`uint64`** | **`print`** | **`println`** | **`real`** |
| **`recover`** | **`string`** | **`true`** | **`uint`** | **`uint8`** |
| **`uintptr`** |  |  |  |  |

<a name="ea3d9bb8"></a>

## 二、go 语言数据类型

***1.*** 在 `Go` 语言中，数据类型用于声明函数和变量

***2.*** 数据类型的出现是为了把数据分成所需内存大小不同的数据，编程的时候需要用大数据的时候才需要申请大内存，好处是可以充分利用内存

<a name="86820623"></a>

### **2.1** **布尔型**

布尔型的值只可以是常量 `true` 或者 `false`。一个简单的例子：

```go
var b bool = true
```

<a name="f4044004"></a>

### **2.2** **字符串类型**

<a name="1e974315"></a>

#### **2.2.1 说明**

字符串就是一串固定长度的字符连接起来的字符序列。

`Go` 的字符串是由单个字节连接起来的。`Go` 语言的字符串的字节使用 `UTF-8` 编码标识 `Unicode` 文本。

<a name="ac58c795"></a>

#### **2.2.2 常见转义符**

| 转义符 | 含义 |
| :--- | :--- |
| `\r` | 回车符（返回行首） |
| `\n` | 换行符（直接跳到下一行的同列位置） |
| `\t` | 制表符 |
| `\'` | 单引号 |
| `\"` | 双引号 |
| `\\` | 反斜杠 |

<a name="6924feae"></a>

#### **2.2.3 单引号**

在 `go` 语言中表示 `golang` 中的 `rune(int32)` 类型，单引号里面是 **单个字符**，对应的值为该字符的 `ASCII` 值。

<a name="13617051"></a>

#### **2.2.4 双引号**

在 `go` 语言中双引号里面可以是单个字符也可以是字符串，双引号里面可以有转义字符，如 `\n`、`\r` 等，对应 `go` 语言中的 `string` 类型。

<a name="90b28f1c"></a>

#### **2.2.5 反引号**

用来定义多行字符串字面量，不支持转义。

```go
const str = `
    first,
    second,
    third
`
fmt.Println(str)
```

<a name="6f04c464"></a>

#### **2.2.6 字符**

字符串中的每一个元素叫做“字符”，分为以下两种：

1. `uint8（byte）类型`

代表 ASCII 码的一个字符

1. `rune（int32）类型`

用来处理中文、日文或者其它复合字符

```go
var a byte = 'a'
fmt.Printf("%d %T\n", a, a) // 97 unint8

var b rune = '你'
fmt.Printf("%d %T\n", b, b) // 20320 int32
```

<a name="70b5ed08"></a>

### **2.3** **派生类型**

<a name="f11461b5"></a>

#### **2.3.1 指针类型(`Pointer`)**

<a name="66e0ccea"></a>

#### **2.3.2 数组类型**

<a name="45f780b7"></a>

#### **2.3.3 结构化类型(`struct`)**

<a name="11469010"></a>

#### **2.3.4 `Channel`类型**

<a name="74ebc7b3"></a>

#### **2.3.5 函数类型**

<a name="c4d0da8d"></a>

#### **2.3.6 切片类型**

<a name="f30684d4"></a>

#### **2.3.7 接口类型（`interface`）**

<a name="8cb02fe1"></a>

#### **2.3.8 `Map` 类型**

<a name="62fa7932"></a>

### 2.4 **数字类型**

<a name="d0943a5f"></a>

#### **2.4.1 说明**

整型 `int` 和浮点型 `float32`、`float64`，`Go` 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。

<a name="b493adfe"></a>

#### **2.4.2 数字类型**

| 序号 | 类型 | 描述 |
| :--- | :--- | :--- |
| 1 | `uint8` | 无符号 8 位整型 (0 到 255) |
| 2 | `uint16` | 无符号 16 位整型 (0 到 65535) |
| 3 | `uint32` | 无符号 32 位整型 (0 到 4294967295) |
| 4 | `uint64` | 无符号 64 位整型 (0 到 18446744073709551615) |
| 5 | `int8` | 有符号 8 位整型 (-128 到 127) |
| 6 | `int16` | 有符号 16 位整型 (-32768 到 32767) |
| 7 | `int32` | 有符号 32 位整型 (-2147483648 到 2147483647) |
| 8 | `int64` | 有符号 64 位整型 (-9223372036854775808 到 9223372036854775807) |

<a name="71a84db5"></a>

#### **2.4.3 浮点类型**

| 序号 | 类型 | 描述 |
| :--- | :--- | :--- |
| 1 | `float32` | `IEEE-754` 32位浮点型数 |
| 2 | `float64` | `IEEE-754` 64位浮点型数 |
| 3 | `complex64` | `32` 位实数和虚数 |
| 4 | `complex128` | `64` 位实数和虚数 |

```go
package main
import (
    "fmt"
    "math"
)
func main() {
    fmt.Printf("%f\n", math.Pi) // 按默认宽度和精度输出
    fmt.Printf("%.2f\n", math.Pi) // 按默认宽度，2位精度输出
}
```

<a name="4c5c8bb1"></a>

#### **2.4.4 其它数字类型**

| 序号 | 类型 | 描述 |
| :--- | :--- | :--- |
| 1 | `byte` | 类似 uint8 |
| 2 | `rune` | 类似 int32 |
| 3 | `uint` | 32 或 64 位 |
| 4 | `int` | 与 uint 一样大小 |
| 5 | `uintptr` | 无符号整型，用于存放一个指针 |

<a name="8cb74b7b"></a>

## 三、go 语言变量

<a name="14dba8e5"></a>

### 3.1 变量声明

<a name="1be554ca"></a>

#### **3.1.1 声明变量的一般形式**

```go
   var a int // 整型
   var b string // 字符串
   var c []float32 // 32位浮点切片，浮点切片表示由多个浮点类型组成的数据结构
   var d func() bool // 返回布尔类型的函数变量
   var e struct { // 声明一个结构体类型的变量，这个结构体拥有一个整型的 x 字段
       x int
   }
```

<a name="280b15ef"></a>

#### **3.1.2 变量声明（第一种）**

指定变量类型，如果没有初始化，则变量默认为零值。

- 数值类型(包括 `complex64/128` ) 为 `0`
- 布尔类型为 `false`
- 字符串为 `""`(空字符串)
- 以下几种类型为 `nil`

```go
var a *int
var a []int
var map[string] int
var a chan int
var a func(string) int
var a error // error 是接口
```

```go
// var v_name v_type
// v_name = value

// 零值就是变量没有做初始化时系统默认设置的值
package main
import "fmt"
func main() {
    // 声明一个变量并初始化
    var a = "RUNNOB"
    fmt.Println(a)

    // 没有初始化就为零
    var b int
    fmt.Println(b)

    // bool 零值为 false
    var c bool
    fmt.Println(c)

    var i1 int
    var f1 float64
    var b1 bool
    var s1 string
    fmt.Printf("%v %v %v %q\n", i1, f1, b1, s1)
}
```

<a name="8f57739b"></a>

#### **3.1.3 变量声明（第二种）**

根据值自行判定变量类型

```go
// var v_name = value
package main
import "fmt"
func main() {
    var d = true
    fmt.Println(d)
}
```

<a name="36fe1dea"></a>

#### **3.1.4 变量声明（第三种）**

省略 `var`，注意 `:=` 左侧如果没有声明新的变量，就产生编译错误

```go
// v_name := value

var intVal int
intVal := 1 // 这时候会产生编译错误
intVal, intVal1 := 1, 2 // 此时不会产生编译错误，因为有声明新的变量，因为 := 是一个声明语句

// 可以将 var f string = "Runoob" 简写为 f := "Runoob"
package main
import "fmt"
func main() {
    f := "Runoob"
    fmt.Println(f)
}
```

<a name="e25a4bcb"></a>

### 3.2 多变量声明

```go
//类型相同多个变量, 非全局变量
var vname1, vname2, vname3 type
vname1, vname2, vname3 = v1, v2, v3

var vname1, vname2, vname3 = v1, v2, v3 // 和 python 很像,不需要显示声明类型，自动推断

vname1, vname2, vname3 := v1, v2, v3 // 出现在 := 左侧的变量不应该是已经被声明过的，否则会导致编译错误


// 这种因式分解关键字的写法一般用于声明全局变量
var (
    vname1 v_type1
    vname2 v_type2
)
```

```go
package main

var x, y int
var (  // 这种因式分解关键字的写法一般用于声明全局变量
    a int
    b bool
)

var c, d int = 1, 2
var e, f = 123, "hello"

// 这种不带声明格式的只能在函数体中出现
// g, h := 123, "hello"

func main(){
    g, h := 123, "hello"
    println(x, y, a, b, c, d, e, f, g, h)
}
```

<a name="9578130a"></a>

### 3.3 值类型和引用类型

<a name="a8b407c5"></a>

#### **3.3.1 值类型**

1. 所有像  `int`、`float`、`bool` 和  `string` 这些基本类型都属于值类型，使用这些类型的变量直接指向存在内存中的值
2. 当使用等号 `=` 将一个变量的值赋值给另一个变量时，如：`j = i`，实际上是在内存中将 `i` 的值进行拷贝
3. 你可以通过 `&i` 来获取变量  `i`  的内存地址，例如：`0xf840000040`（每次的地址都可能不一样）。值类型的变量的值存储在栈中。
4. 内存地址会根据机器的不同而有所不同，甚至相同的程序在不同的机器上执行后也会有不同的内存地址。因为每台机器可能有不同的存储器布局，并且位置分配也可能不同。

<a name="52a647f8"></a>

#### **3.3.2 引用类型**

1. 更复杂的数据通常会需要使用多个字，这些数据一般使用引用类型保存。
2. 一个引用类型的变量 `r1` 存储的是 **`r1` 的值所在的内存地址（数字）**，或 **内存地址中第一个字所在的位置**。这个内存地址为称之为**指针**，**这个指针实际上也被存在另外的某一个字中**。
3. 同一个引用类型的指针指向的多个字可以是在连续的内存地址中（**内存布局是连续的**），这也是计算效率最高的一种存储形式；也可以将这些字分散存放在内存中，**每个字都指示了下一个字所在的内存地址**。
4. 当使用赋值语句 `r2 = r1` 时，只有引用（地址）被复制。如果 `r1` 的值被改变了，那么这个值的所有引用都会指向被修改后的内容，在这个例子中，`r2` 也会受到影响。
5. 简短形式，使用 `:=` 赋值操作符
   - 我们知道可以在变量的初始化时省略变量的类型而由系统自动推断，声明语句写上 `var` 关键字其实是显得有些多余了，因此我们可以将它们简写为 `a := 50` 或  `b := false`。`a` 和 `b` 的类型（`int` 和 `bool`）将由编译器自动推断。
6. 这是使用变量的首选形式，但是它只能被用在函数体内，而不可以用于全局变量的声明与赋值。使用操作符 `:=` 可以高效地创建一个新的变量，称之为 **初始化声明**。
7. 注意事项
   - 如果在相同的代码块中，我们不可以再次对于相同名称的变量使用初始化声明，例如：`a := 20` 就是不被允许的，编译器会提示错误 `no new variables on left side of :=`，但是 `a = 20` 是可以的，因为这是给相同的变量赋予一个新的值。
   - 如果你在定义变量 `a` 之前使用它，则会得到编译错误 `undefined: a`。
   - 如果你声明了一个局部变量却没有在相同的代码块中使用它，同样会得到编译错误，例如下面这个例子当中的变量 `a`：\`\`\`go
     // 尝试编译这段代码将得到错误 a declared and not used。
     package main
     import "fmt"
     func main() {
     var a string = "abc"
     fmt.Println("hello, world")
     }

````

   - 此外，单纯地给 `a` 赋值也是不够的，这个值必须被使用，所以使用`fmt.Println("hello, world", a)`会移除错误。
   - 但是全局变量是允许声明但不使用。 同一类型的多个变量可以声明在同一行，如：```go
var a, b, c int
````

- 多变量可以在同一行进行赋值，如：\`\`\`go
  var a, b int
  var c string
  a, b, c = 5, 7, "abc"

````

   - 上面这行假设了变量 `a`，`b`和 `c`都已经被声明，**否则的话应该这样使用**：```go
a, b, c := 5, 7, "abc"
````

- 右边的这些值以相同的顺序赋值给左边的变量，所以 `a` 的值是 `5`， `b`的值是 `7`，`c` 的值是 `"abc"`。\[**这被称为 `并行` 或 `同时` 赋值。**]如果你想要交换两个变量的值，则可以简单地使用 **`a, b = b, a`**，两个变量的类型必须是相同。
- 空白标识符 `_` 也被用于**抛弃值**，如值 `5` 在：`_, b = 5, 7` 中被抛弃。`_`实际上是一个只写变量，你不能得到它的值。这样做是因为 `Go` 语言中你必须使用所有被声明的变量，但有时你并不需要使用从一个函数得到的所有返回值。
- 并行赋值也被用于当一个函数返回多个返回值时，比如这里的 `val` 和错误 `err` 是通过调用 `Func1` 函数同时得到：`val, err = Func1(var1)`。

<a name="45c12816"></a>

## 四、go 语言常量

<a name="3759d033"></a>

### 4.1 const

常量是一个简单值的标识符，在程序运行时，不会被修改的量。

常量中的数据类型只可以是 **布尔型**、**数字型**（**整数型**、**浮点型** 和 **复数**）和 **字符串型**。

常量的定义格式：

```go
const identifier [type] = value
```

你可以省略类型说明符\[`type`]，因为编译器可以根据变量的值来推断其类型

- 显式类型定义：`const b string = "abc"`
- 隐式类型定义：`const b = "abc"`

多个相同类型的声明可以简写为： `const c_name1, c_name2 = value1, value2`

以下实例演示了常量的应用：

```go
package main
import "fmt"
func main() {
    const LENGTH int = 10
    const WIDTH int = 5
    var area int
    const a, b, c = 1, false, "str" // 多重赋值

    area = WIDTH * LENGTH
    fmt.Printf("面积为：%d", area)
    println()
    println(a, b, c)
}
```

常量还可以用作枚举：

```go
const (
    unknown = 0
    Female = 1
    Male = 2
)
```

数字 `0`、`1` 和 `2` 分别代表 `未知性别`、`女性` 和 `男性`

常量可以用 `len()`、`cap()`、`unsafe.Sizeof()` 函数计算表达式的值。常量表达式中，函数必须是内置函数，否则编译不过：

```go
package main
import "unsafe"
const (
    a = "abc"
    b = len(a)
    c = unsafe.Sizeof(a)
)
func main() {
    println(a, b, c)
}
```

<a name="557fa36b"></a>

### 4.2 iota

`iota`, 特殊常量，可以认为是一个可以被编译器修改的常量

`iota` 在 `const` 关键字出现时将被重置为 `0`(`const` 内部的第一行之前)，`const` 中每新增一行常量声明将使 `iota` 计数一次（`iota` 可理解为 `const` 语句块中的行索引）

`iota` 可以被用作枚举值：

```go
const (
    a = iota
    b = iota
    c = iota
)
```

第一个 `iota` 等于 `0`，每当 `iota` 在新的一行被使用时，它的值都会自动加 `1`；所以 `a=0`，`b=1`，`c=2` 可以简写为如下形式：

```go
const (
    a = iota
    b
    c
)
```

<a name="ee4acefe"></a>

#### **4.2.1 iota 用法**

```go
package main
import "fmt"
func main() {
    const (
        a = iota // 0
        b // 1
        c // 2
        d = "ha" // 独立值，iota += 1
        e // "ha" iota += 1
        f = 100 // iota += 1
        g // 100 iota += 1
        h = iota // 7，恢复计数
        i // 8
    )
    fmt.Println(a, b, c, d, e, f, g, h, i)
}
```

- 再看个有趣的的 `iota` 实例：

```go
package main

import "fmt"
const (
    i=1<<iota
    j=3<<iota
    k
    l
)

func main() {
    fmt.Println("i=",i)
    fmt.Println("j=",j)
    fmt.Println("k=",k)
    fmt.Println("l=",l)
}
```

`iota` 表示从 `0` 开始自动加 `1`，所以 `i=1<<0`, `j=3<<1`（`<<`表示左移的意思），即：`i=1`, `j=6`，这没问题，关键在 `k` 和 `l`，从输出结果看 `k=3<<2`，`l=3<<3`。

- 简单表述：
  - `i=1`：左移 `0` 位,不变仍为 `1`;
  - `j=3`：左移 `1` 位,变为二进制 `110`, 即 `6`;
  - `k=3`：左移 `2` 位,变为二进制 `1100`, 即 `12`;
  - `l=3`：左移 `3` 位,变为二进制 `11000`,即 `24`。

<a name="107e5cba"></a>

## 五、go 语言运算符

运算符用于在程序运行时执行数学或逻辑运算

| 序号 | `go`语言内置的运算符 |
| :--- | :--- |
| `1` | 算数运算符 |
| `2` | 关系运算符 |
| `3` | 逻辑运算符 |
| `4` | 位运算符 |
| `5` | 赋值运算符 |
| `6` | 其它运算符 |

<a name="014f3b72"></a>

### 5.1 算数运算符

| 运算符 | 描述 |
| :--- | :--- |
| `+` | 相加 |
| `-` | 相减 |
| `*` | 相乘 |
| `/` | 相除 |
| `%` | 求余 |
| `++` | 自增 |
| `--` | 自减 |

<a name="7f1403d6"></a>

### 5.2 关系运算符

| 运算符 | 描述 |
| :--- | :--- |
| `==` | 检查两个值是否相等，如果相等返回 `True` 否则返回 `False`。 |
| `!=` | 检查两个值是否不相等，如果不相等返回 `True` 否则返回 `False`。 |
| `>` | 检查左边值是否大于右边值，如果是返回 `True` 否则返回 `False`。 |
| `<` | 检查左边值是否小于右边值，如果是返回 `True`否则返回 `False`。 |
| `>=` | 检查左边值是否大于等于右边值，如果是返回 `True` 否则返回 `False`。 |
| `<=` | 检查左边值是否小于等于右边值，如果是返回 `True` 否则返回 `False`。 |

<a name="10766857"></a>

### 5.3 逻辑运算符

| 运算符 | 描述 |
| :--- | :--- |
| `&&` | 逻辑 `AND` 运算符。 如果两边的操作数都是 `True`，则条件 `True`，否则为 `False`。 |
| `||` | 逻辑 `OR` 运算符。 如果两边的操作数有一个 `True`，则条件 `True`，否则为 `False`。 |
| `!` | 逻辑 `NOT` 运算符。 如果条件为 `True`，则逻辑 `NOT` 条件 `False`，否则为 `True`。 |

<a name="98bbf776"></a>

### 5.4 位运算符

位运算符对整数在内存中的二进制位进行操作。下表列出了位运算符 `&`, `|`, 和 `^` 的计算：

| `p` | `q` | `p & q` | `p | q` | `p ^ q` |
| :--- | :--- | :--- | :--- | :--- |
| `0` | `0` | `0` | `0` | `0` |
| `0` | `1` | `0` | `1` | `1` |
| `1` | `1` | `1` | `1` | `0` |
| `1` | `0` | `0` | `1` | `1` |

假定 `A = 60`; `B = 13`; 其二进制数转换为：

```go
A = 0011 1100

B = 0000 1101

-----------------

A & B = 0000 1100

A | B = 0011 1101

A ^ B = 0011 0001
```

`Go` 语言支持的位运算符如下表所示。假定 `A 为 60`，`B 为 13`：

| 运算符 | 描述 |
| :--- | :--- |
| `&` | 按位与运算符`"&"`是双目运算符。 其功能是参与运算的两数各对应的二进位相与。 |
| `|` | 按位或运算符`"|"`是双目运算符。 其功能是参与运算的两数各对应的二进位相或 |
| `^` | 按位异或运算符`"^"`是双目运算符。 其功能是参与运算的两数各对应的二进位相异或，当两对应的二进位相异时，结果为1。 |
| `<<` | 左移运算符`"<<"`是双目运算符。左移`n`位就是乘以`2`的`n`次方。 其功能把`"<<"`左边的运算数的各二进位全部左移若干位，由`"<<"`右边的数指定移动的位数，高位丢弃，低位补`0`。 |
| `>>` | 右移运算符`">>"`是双目运算符。右移`n`位就是除以`2`的`n`次方。 其功能是把`">>"`左边的运算数的各二进位全部右移若干位，`">>"`右边的数指定移动的位数。 |

<a name="87c98392"></a>

### 5.5 赋值运算符

| 运算符 | 描述 | 实例 |
| :--- | :--- | :--- |
| `=` | 简单的赋值运算符，将一个表达式的值赋给一个左值 | `C = A + B` 将 `A + B` 表达式结果赋值给 `C` |
| `+=` | 相加后再赋值 | `C += A` 等于 `C = C + A` |
| `-=` | 相减后再赋值 | `C -= A` 等于`C = C - A` |
| `*=` | 相乘后再赋值 | `C *= A` 等于 `C = C * A` |
| `/=` | 相除后再赋值 | `C /= A` 等于 `C = C / A` |
| `%=` | 求余后再赋值 | `C %= A` 等于 `C = C % A` |
| `<<=` | 左移后赋值 | `C <<= 2` 等于 `C = C << 2` |
| `>>=` | 右移后赋值 | `C >>= 2` 等于 `C = C >> 2` |
| `&=` | 按位与后赋值 | `C &= 2` 等于 `C = C & 2` |
| `^=` | 按位异或后赋值 | `C ^= 2` 等于 `C = C ^ 2` |
| `\|=` | 按位或后赋值 | `C \|= 2` 等于 `C = C \| 2` |

<a name="4efb2255"></a>

### 5.6 其它运算符

| 运算符 | 描述 | 实例 |
| :--- | :--- | :--- |
| `&` | 返回变量存储地址 | `&a`; 将给出变量的实际地址。 |
| `*` | 指针变量。 | `*a`; 是一个指针变量 |

<a name="6b66a1d6"></a>

### 5.7 运算符优先级

有些运算符拥有较高的优先级，二元运算符的运算方向均是从左至右。下表列出了所有运算符以及它们的优先级，由上至下代表优先级由高到低：

| 优先级 | 运算符 |
| :--- | :--- |
| `5` | `* / % << >> & &^` |
| `4` | `+ - | ^` |
| `3` | `== != < <= > >=` |
| `2` | `&&` |
| `1` | `||` |

可以通过使用括号来临时提升某个表达式的整体运算优先级。

<a name="6a01f0e1"></a>

## 六、go 语言条件语句

以下为常用条件语句：

| 语句 | 描述 |
| :--- | :--- |
| `if` 语句 | `if` 语句 由一个布尔表达式后紧跟一个或多个语句组成。 |
| `if...else` 语句 | `if` 语句 后可以使用可选的 `else` 语句, `else` 语句中的表达式在布尔表达式为 `false` 时执行。 |
| `if` 嵌套语句 | 你可以在 `if` 或 `else if` 语句中嵌入一个或多个 `if` 或 `else if` 语句。 |
| `switch` 语句 | `switch` 语句用于基于不同条件执行不同动作。 |
| `select` 语句 | `select` 语句类似于 `switch` 语句，但是 `select` 会随机执行一个可运行的 `case`。如果没有 `case` 可运行，它将阻塞，直到有 `case` 可运行。 |

<a name="e25feddf"></a>

### 6.1 `select` 语句

- 语法
  - 每个 `case` 都必须是一个 **通信**
  - 所有 `channel` 表达式都会被求值
  - 所有被发送的表达式都会被求值
  - 如果任意某个通信可以进行，它就执行，其它被忽略
  - 如果有多个 `case` 都可以运行，`select` 会随机公平地选出一个执行。其他不会执行。
    否则：
    - 如果有 `default` 子句，则执行该语句
    - 如果没有 `default` 子句，`select` 将阻塞，直到某个通信可以运行；`Go` 不会重新对 `channel` 或值进行求值

```go
select {
    case communication clause :
        statement(s);
    case communication clause :
        statement(s);
    default :
        statement(s);
}
```

例子1：

```go
package main

import "fmt"

func main() {
	var c1, c2, c3 chan int
	var i1, i2 int
	select {
	case i1 = <-c1: // 从 Channel c1 中接收数据，并将值赋给 i1
		fmt.Printf("received ", i1, " from c1\n")
	case c2 <- i2: // 发送 i2 到 Channel c2 中
		fmt.Printf("sent ", i2, " to c2\n")
	case i3, ok := (<-c3): // same as: i3, ok := <-c3 ; 从 Channel c3 中接收数据，并将值赋给 i3, ok
		if ok {
			fmt.Printf("received ", i3, " from c3\n")
		} else {
			fmt.Printf("c3 is closed\n")
		}
	default:
		fmt.Printf("no communication\n")
	}
}
```

例子2：

```go
package main

import "fmt"

func fibonacci(c, quit chan int) {
    x, y := 0, 1
    for {
        select {
            case c <- x:
                x, y = y, x + y
            case <-quit:
                fmt.Println("quit")
                return
        }
    }
}

func main() {
    c := make(chan int)
    quit := make(chan int)
    go func() {
        for i := 0; i < 10; i++ {
            fmt.Println(<-c)
        }
        quit <- 0
    }()
    fibonacci(c, quit)
}
```

<a name="443f5d8b"></a>

## 七、go 语言循环语句

| 循环类型 | 描述 |
| :--- | :--- |
| `for` 循环 | 重复执行语句块 |
| 循环嵌套 | 在 `for` 循环中嵌套一个或多个 `for` 循环 |

<a name="60d9ca6d"></a>

### 7.1 `for` 循环

```go
package main
import "fmt"
func main() {
    var b int = 15
    var a int
    numbers := [6]int{1, 2, 3, 5}
    // for 循环 
    for a := 0; a < 10; a++ {
        fmt.Printf("a 的值为：%d\n", a)
    }
    for a < b {
        a++
        fmt.Printf("a 的值为：%d\n", a)
    }
    for i, x := range numbers {
        fmt.Printf("第 %d 位 x 的值 = %d\n", i, x)
    }
}
```

<a name="615380f2"></a>

#### 7.1.1 类似 `c` 语言的 `for` 循环

```go
for init; condition; post { }
```

<a name="7c2097f7"></a>

#### 7.1.2 类似 `c` 语言的 `while`

```go
for condition { }
```

<a name="1800ae65"></a>

#### 7.1.3 类似 `c` 语言的 `for(;;)`

```go
for { }
```

<a name="628e736c"></a>

#### 7.1.4 `range` 格式

`for` 循环的 `range` 格式可以对 `slice`、`map`、数组、字符串等进行迭代操作

```go
for key, value := range oldMap {
    newMap[key] = value
}
```

<a name="57251dda"></a>

#### 7.1.5 循环嵌套

```go
package main

import "fmt"

func main() {
	var i, j int

	for i = 2; i < 100; i++ {
		for j = 2; j <= (i / j); j++ {
			if i%j == 0 {
				break
			}
		}
		if j > (i / j) {
			fmt.Printf("%d 是素数\n", i)
		}
	}
}
```

<a name="d7494ad7"></a>

### 7.2 循环控制语句

循环控制语句可以控制循环体内语句的执行过程。

`GO` 语言支持以下几种循环控制语句：

| 控制语句 | 描述 |
| :--- | :--- |
| `break` 语句 | 经常用于中断当前 `for` 循环或跳出 `switch` 语句 |
| `continue` 语句 | 跳过当前循环的剩余语句，然后继续进行下一轮循环。 |
| `goto` 语句 | 将控制转移到被标记的语句。 |

<a name="3aee1d27"></a>

#### 7.2.1 `break` 语句

```go
// 变量大于 15 时跳出循环
package main
import "fmt"
func main() {
    var a int = 10
    for a < 20 {
        fmt.Printf("a 的值为：%d \n", a);
        a++
        if a > 15 {
            break;
        }
    }
}
```

<a name="9036e021"></a>

#### 7.2.2 `continue` 语句

```go
// 在变量 a 等于 15 的时候跳过本次循环执行下一次循环：
package main 
import "fmt"
func main() {
    var a int = 10
    for a < 20 {
        if a == 15 {
            a = a + 1
            continue
        }
        fmt.Printf("a 的值为 ：%d\n", a)
        a++
    }
}
```

<a name="8321efb8"></a>

#### 7.2.3 `goto` 语句

`Go` 语言的 `goto` 语句可以无条件地转移到过程中指定的行。

`goto` 语句通常与条件语句配合使用。可用来实现条件转移， 构成循环，跳出循环体等功能。

但是，在结构化程序设计中一般不主张使用 `goto` 语句， 以免造成程序流程的混乱，使理解和调试程序都产生困难。

- 语法格式

```go
goto label;
..
.
label: statement
```

```go
// 在变量 a 等于 15 的时候跳过本次循环并回到循环的开始语句 Loop 处
package main

import "fmt"

func main() {
    var a int = 10
    // 循环
    LOOP: for a < 20 {
        if a == 15 {
            // 跳过迭代
            a = a + 1
            goto LOOP
        }
        fmt.Printf("a 的值为 ： %d\n", a)
        a++
    }
}
```

<a name="bea5001b"></a>

### 7.3 无限循环

如果循环中条件语句永远不为 `false` 则会进行无限循环，我们可以通过 `for` 循环语句中只设置一个条件表达式来执行无限循环：

```go
package main

import "fmt"

func main() {
    for true {
        fmt.Printf("这是无限循环。\n")
    }
}
```

<a name="77fda0e3"></a>

## 八、go 语言函数

<a name="4954d328"></a>

### 8.1 函数定义

```go
// Go 语言函数定义格式如下：
func function_name( [parameter list] ) [return_types] { 函数体 }
```

> 函数定义解析：
>
> - `func`：函数由 `func` 开始声明
> - `function_name`：函数名称、函数名和参数列表一起构成了函数签名
> - `parameter list`：参数列表，参数就像一个占位符，当函数被调用时，你可以将值传递给参数，这个值被称为实际参数。参数列表指定的是参数类型、顺序、及参数个数。参数是可选的，也就是说函数也可以不包含参数
> - `return_types`：返回类型，函数返回一列值。`return_types` 是该列值的数据类型。有些功能不需要返回值，这种情况下 `return_types` 不是必须的
> - 函数体：函数定义的代码集合

```go
func max(num1, num2 int) int {
    var result int
    if num1 > num2 {
        result = num1
    } else {
        result = num2
    }
    return result
}
```

<a name="de53eba0"></a>

### 8.2 函数调用

```go
package main

import "fmt"

func main() {
    var a int = 100
    var b int = 200
    var ret int

    ret = max(a, b)
    fmt.Printf("最大值是：%d\n", ret)
}

func max(num1, num2 int) int {
    var result int

    if num1 > num2 {
        result = num1
    } else {
        result = num2
    }
}
```

<a name="c30d4cb2"></a>

### 8.3 函数返回多个值

```go
package main

import "fmt"

func swap(x, y string) (string, string) {
    return y, x
}

func main() {
    a, b := swap("Google", "Runoob")
    fmt.Println(a, b)
}
```

<a name="248664af"></a>

### 8.4 函数参数

函数如果使用参数，该变量可称为函数的形参。

形参就像定义在函数体内的局部变量。

调用函数，可以通过两种方式来传递参数：

| 传递类型 | 描述 |
| :--- | :--- |
| 值传递 | 值传递是指在调用函数时将实际参数复制一份传递到函数中，这样在函数中如果对参数进行修改，将不会影响到实际参数。 |
| 引用传递 | 引用传递是指在调用函数时将实际参数的地址传递到函数中，那么在函数中对参数所进行的修改，将影响到实际参数。 |

默认情况下，`Go` 语言使用的是值传递，即在调用过程中不会影响到实际参数。

<a name="69c33c34"></a>

### 8.5 函数用法

| 函数用法 | 描述 |
| :--- | :--- |
| 函数作为另外一个函数的实参 | 函数定义后可作为另外一个函数的实参数传入 |
| 闭包 | 闭包是匿名函数，可在动态编程中使用 |
| 方法 | 方法就是一个包含了接受者的函数 |

<a name="42186c39"></a>

#### 8.5.1 函数作为另外一个函数的实参

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    // 声明函数变量
    getSquareBoot := func(x float64) float64 {
        return math.Sqrt(x)
    }

    // 使用函数
    fmt.Println(getSquareBoot(9))
}
```

```go
package main

import "fmt"

// 声明一个函数类型
type cb func(int) int

func main() { 
    testCallBack(1, callBack)
    testCallBack(2, func(x int) int {
        fmt.Printf("我是回调，x：%d\n", x)
        return x
    })
}

func testCallBack(x int, f cb) {
    f(x)
}

func callBack(x int) int {
    fmt.Printf("我是回调，x：%d\n", x)
    return x
}
```

<a name="951efa63"></a>

#### 8.5.2 闭包

`Go` 语言支持匿名函数，可作为闭包。匿名函数是一个"内联"语句或表达式。匿名函数的优越性在于可以直接使用函数内的变量，不必申明。

以下实例中，我们创建了函数 `getSequence()` ，返回另外一个函数。该函数的目的是在闭包中递增 `i` 变量，代码如下：

```go
package main

import "fmt"

func getSequence() func() int {
    i := 0
    return func() int {
        i += 1
        return i
    }
}

func main() {
    nextNumber := getSequence()

    fmt.Println(nextNumber())
    fmt.Println(nextNumber())
    fmt.Println(nextNumber())

    nextNumber1 := getSequence()
    fmt.Println(nextNumber1())
    fmt.Println(nextNumber1())
}
```

带参数的闭包函数调用

```go
package main

import "fmt"

func main() {
    add_func := add(1, 2)
    fmt.Println(add_func())
    fmt.Println(add_func())
    fmt.Println(add_func())
}

func add(x1, x2 int) func() (int, int) {
    i := 0
    return func() (int, int) {
        i++
        return i, x1 + x2
    }
}
```

闭包带参数

```go
package main

import "fmt"

func main() {
    add_func := add(1, 2)
    fmt.Println(add_func(1, 1))
    fmt.Println(add_func(0, 0))
    fmt.Println(add_func(2, 2))
}
// 闭包使用方法
func add(x1, x2 int) func(x3 int, x4 int) (int, int, int) {
    i := 0 
    return func(x3 int, x4 int) (int, int, int) {
        i++
        return i, x1 + x2, x3 + x4
    }
}
```

<a name="22903c1a"></a>

#### 8.5.3 方法

`Go` 语言中同时有函数和方法。一个方法就是一个包含了接受者的函数，接受者可以是 **命名类型** 或者 **结构体类型的一个值** 或者是 **一个指针**。所有给定类型的方法属于该类型的方法集。语法格式如下：

```go
func (variable_name variable_data_type) function_name() [return_type]{
   /* 函数体*/
}
```

下面定义一个结构体类型和该类型的一个方法：

```go
package main

import (
    "fmt"
)

// 定义结构体
type Circle struct {
    radius float64
}

func main() {
    var c1 Circle
    c1.radius = 10.00
    fmt.Println("圆的面积 = ", c1.getArea())
}
// 该 method 属于 Circle 类型对象的方法
func (c Circle) getArea() float64 {
    // c.radius 即为 Circle 类型对象中的属性
    return 3.14 * c.radius * c.radius
}
```

<a name="ca3147fa"></a>

## 九、go 语言数组

`Go` 语言提供了数组类型的数据结构。

数组是具有相同唯一类型的一组已编号且长度固定的数据项序列，这种类型可以是任意的原始类型例如整形、字符串或者自定义类型。

相对于去声明 number0, number1, ..., number99 的变量，使用数组形式 numbers\[0], numbers\[1] ..., numbers\[99] 更加方便且易于扩展。

数组元素可以通过索引（位置）来读取（或者修改），索引从 0 开始，第一个元素索引为 0，第二个索引为 1，以此类推。

![](assets/nfs1ot/goarray.png)

<a name="a1d3a967"></a>

### 9.1 声明数组

> `Go` 语言数组声明需要指定元素类型及元素个数，**语法格式** 如下：
>
> ```go
> ```

var variable\_name \[SIZE] variable\_type

    > 以上为一维数组的定义方式。例如以下定义了数组 `balance` 长度为 `10` 类型为 `float32：`
    > ```go
    var balance [10] float32

<a name="58253869"></a>

### 9.2 初始化数组

> 以下演示了数组初始化：
>
> ```go
> ```

var balance = \[5]float32{1000.0, 2.0, 3.4, 7.0, 50.0}

    > 初始化数组中 `{}` 中的元素个数**不能大于** `[]` 中的数字。
    > 如果忽略 `[]` 中的数字不设置数组大小，`Go` 语言会根据元素的个数来设置数组的大小：
    > ```go
    var balance = [...]float32{1000.0, 2.0, 3.4, 7.0, 50.0}

> 该实例与上面的实例是一样的，虽然没有设置数组的大小。
>
> ```go
> ```

balance\[4] = 50.0

    > 以上实例读取了第五个元素。数组元素可以通过索引（位置）来读取（或者修改），索引从 `0` 开始，第一个元素索引为 `0`，第二个索引为 `1`，以此类推。
    > ![](https://www.runoob.com/wp-content/uploads/2015/06/array_presentation.jpg#align=left&display=inline&height=67&originHeight=67&originWidth=465&status=done&style=none&width=465)


    <a name="2c53a7a6"></a>
    ### 9.3 访问数组元素

    > 数组元素可以通过索引（位置）来读取。格式为数组名后加中括号，中括号中为索引的值。例如：
    > ```go
    var salary float32 = balance[9]

> 以上实例读取了数组 `balance` 第 `10` 个元素的值。
> 以下演示了数组完整操作（声明、赋值、访问）的实例：

```go
 package main
 import "fmt"
 func main() {
    var n [10] int // n 是一个长度为 10 的数组
    var i, j int
    // 为数组 n 初始化元素
    for i = 0; i < 10; i++ {
        n[i] = i + 100 // 设置元素为 i + 100
    }
    // 输出每个数组元素的值
    for j = 0; j < 10; j++ {
        fmt.Printf("Element[%d] = %d\n", j, n[j])
    }
}
```

<a name="cadae226"></a>

### 9.4 更多内容

数组对 `Go` 语言来说是非常重要的，以下我们将介绍数组更多的内容：

| 内容 | 描述 |
| :=== | :=== |
| 多维数组 | `Go` 语言支持多维数组，最简单的多维数组是二维数组 |
| 向函数传递数组 | 你可以向函数传递数组参数 |

<a name="7cdd727d"></a>

#### 9.4.1 `Go` 语言多维数组

`Go` 语言支持多维数组，以下为常用的多维数组声明方式：

> ```go
> ```

var variable\_name \[SIZE1]\[SIZE2]...\[SIZEN] variable\_type

````


以下实例声明了三维的整型数组：

> ```go
var threedim [5][10][4]int
````

<a name="1ac91a5f"></a>

##### (1) 二维数组

二维数组是最简单的多维数组，二维数组本质上是由一维数组组成的。二维数组定义方式如下：

> ```go
> ```

var arrayName \[ x ]\[ y ] variable\_type

````


`variable_type` 为 `Go` 语言的数据类型，`arrayName` 为数组名，二维数组可认为是一个表格，`x` 为行，`y` 为列，下图演示了一个二维数组 `a` 为三行四列：

二维数组中的元素可通过 `a[ i ][ j ]` 来访问。

![](https://www.runoob.com/wp-content/uploads/2015/06/two_dimensional_arrays.jpg#align=left&display=inline&height=125&originHeight=125&originWidth=427&status=done&style=none&width=427)

<a name="eb99eecc"></a>
##### (2) 初始化二维数组

多维数组可通过大括号来初始值。以下实例为一个 `3` 行 `4` 列的二维数组：

```go
a = [3][4]int{  
 {0, 1, 2, 3} ,   /*  第一行索引为 0 */
 {4, 5, 6, 7} ,   /*  第二行索引为 1 */
 {8, 9, 10, 11},   /* 第三行索引为 2 */
}
````

> 注意：以上代码中倒数第二行的 `}` 必须要有逗号，因为最后一行的 `}` 不能单独一行，也可以写成这样：

```go
a = [3][4]int{  
 {0, 1, 2, 3} ,   /*  第一行索引为 0 */
 {4, 5, 6, 7} ,   /*  第二行索引为 1 */
 {8, 9, 10, 11}}   /* 第三行索引为 2 */
```

<a name="26dd8aed"></a>

##### (3) 访问二维数组

二维数组通过指定坐标来访问。如数组中的行索引与列索引，例如：

```go
val := a[2][3]
或
var value int = a[2][3]
```

以上实例访问了二维数组 `val` 第三行的第四个元素。

二维数组可以使用循环嵌套来输出元素：

```go
package main
import "fmt"
func main() {
    // 数组 5行2列
    var a = [5][2]int{{0,0}, {1,2}, {2,4}, {3,6}, {4,8}}
    var i, j int

    // 输出数组元素
    for i = 0; i < 5; i++ {
        for j = 0; j < 2; j++ {
            fmt.Printf("a[%d]{%d] = %d\n", i, j, a[i][j])
        }
    }
}
```

<a name="37b2d57e"></a>

#### 9.4.2 `Go` 语言向函数传递数组

如果你想向函数传递数组参数，你需要在函数定义时，声明形参为数组，我们可以通过以下两种方式来声明：

- 方式一

形参设定数组大小：

```go
void myFunction(param [10]int) {
    ...
}
```

- 方式二

形参未设定数组大小：

```go
void myFunction(param []int) {
    ...
}
```

- 实例

让我们看下以下实例，实例中函数接收整型数组参数，另一个参数指定了数组元素的个数，并返回平均值：

```go
func getAverage(arr []int, size int) float32 {
    var i int
    var avg, sum float32
    for i = 0; i < size; i++ {
        sum += arr[i]
    }
    avg = sum / size
    return avg
}
```

接下来我们来调用这个函数：

```go
package main
import "fmt"
func main() {
    // 数组长度为 5
    var balance = [5]int{1000, 2, 3, 17, 50}
    var avg float32

    // 数组作为参数传递给函数
    avg = getAverage(banance, 5)

    // 输出的平均值
    fmt.Printf("平均值为：%f", avg) //  214.399994
}
func getAverage(arr [5]int, size int) float32 {
    var i, sum int
    var avg float32
    for i = 0; i < size; i++ {
        sum += arr[i]
    }
    avg = float32(sum) / float32(size)
    return avg
}
```

以上实例中我们使用的形参并未设定数组大小。

浮点数计算输出有一定的偏差，你也可以转整型来设置精度。

```go
package main
import (
    "fmt"
)
func main() {
    a := 1.69
    b := 1.7
    c := a * b
    fmt.Println(c)
}
```

设置固定精度：

```go
package main
import (
    "fmt"
)
func main() {
    a := 1690
    b := 1700
    c := a * b
    fmt.Println(c)
    fmt.Println(float64(c) / 1000000)
}
```

<a name="994a4da0"></a>

## 十、Go 语言指针

`Go` 语言中指针是很容易学习的，`Go` 语言中使用指针可以更简单的执行一些任务。

接下来让我们来一步步学习 `Go` 语言指针。

我们都知道，**变量** 是一种使用方便的占位符，用于引用计算机内存地址。

`Go` 语言的取地址符是 `&`，放到一个变量前使用就会返回相应变量的内存地址。

以下实例演示了变量在内存中地址：

```go
package main
import "fmt"
func main() {
    var a int = 10
    fmt.Printf("变量的地址：%x\n", &a) // 20818a220
}
```

<a name="60acedc5"></a>

### 10.1 什么是指针

一个指针变量指向了一个值的内存地址

类似于变量和常量，在使用指针前你需要声明指针。指针的声明格式如下：

```go
var var_name *var_type
```

`var-type` 为指针类型，`var_name` 为指针变量名，`*` 号用于指定变量是作为一个指针。以下是有效的指针声明：

```go
var ip *int // 指向整型
var fp *float32 // 指向浮点型
```

<a name="9415fd40"></a>

### 10.2 如何使用指针

指针使用流程：

- 定义指针变量
- 为指针变量赋值
- 访问指针变量中指向地址的值

在指针类型前面加上 `*` 号（前缀）来获取指针所指向的内容。

```go
package main
import "fmt"
func main() {
    var a int = 20
    var ip *int
    ip = &a
    fmt.Printf("a 的变量地址为：%x\n", &a)
    fmt.Printf("ip 变量储存的地址为：%x\n", ip)
    fmt.Printf("*ip 变量的值：%d\n", *ip)
}
```

<a name="ecc0d6e4"></a>

### 10.3 Go 空指针

当一个指针被定义后没有分配到任何变量时，它的值为 `nil`

`nil` 指针也称为空指针

`nil` 在概念上与其它语言的 `null`、`None`、`nil`、`NULL` 一样，都指代零值或空值

一个指针变量通常缩写为 `ptr`

```go
package main

import "fmt"

func main() {
    var ptr *int
    fmt.Printf("ptr 的值为：%x\n", ptr)
}
```

空指针判断

```go
if(ptr != nil) // ptr 不是空指针
if(ptr == nil) // ptr 是空指针
```

<a name="ced4ceb5"></a>

### 10.4 Go 指针更多内容

| 内容 | 描述 |
| :--- | :--- |
| `Go` 指针数组 | 你可以定义一个指针数组来存储地址 |
| `Go` 指向指针的指针 | `Go` 支持指向指针的指针 |
| `Go` 向函数传递指针参数 | 通过引用或地址传参，在函数调用时可以改变其值 |

<a name="4b66198d"></a>

#### 10.4.1 Go 指针数组

在我们了解指针数组前，先看个实例，定义了长度为 `3` 的整型数组：

```go
package main

import "fmt"

const MAX int = 3

func main() {
    a := []int{10, 100, 200}
    var i int

    for i = 0; i < MAX; i++ {
        fmt.Printf("a[%d] = %d\n", i, a[i])
    }
}
```

有一种情况，我们可能需要保存数组，这样我们就需要使用到指针。

以下声明了整型指针数组：

```go
var ptr [MAX]*int
```

`ptr` 为整型指针数组。因此每个元素都指向了一个值。以下实例的三个整数将存储在指针数组中：

```go
package main

import "fmt"

const MAX int = 3

func main() {
    a := []int{10, 100, 200}
    var i int
    var ptr [MAX]*int

    for i = 0; i < MAX; i++ {
        ptr[i] = &a[i] // 整数地址赋值给指针数组
    }

    for i = 0; i < MAX; i++ {
        fmt.Printf("a[%d] = %d\n", i, *ptr[i])
    }
}

// a[0] = 10
// a[1] = 100
// a[2] = 200
```

注意：创建指针数组的时候，不适合用 `range` 循环。

<a name="c947c93f"></a>

#### 10.4.2 Go 指向指针的指针

如果一个指针变量存放的又是另一个指针变量的地址，则称这个指针变量为指向指针的指针变量。

当定义一个指向指针的指针变量时，第一个指针存放第二个指针的地址，第二个指针存放变量的地址：

![](assets/nfs1ot/pointer_to_pointer.jpg)

指向指针的指针变量声明格式如下：

```go
var ptr **int
```

以上指向指针的指针变量为整型。

访问指向指针的指针变量值需要使用两个 `*` 号，如下所示：

```go
package main

import "fmt"

func main() {
    var a int
    var ptr *int
    var pptr **int

    a = 3000

    // 指向 ptr 地址
    ptr = &a
    // 指向指针 ptr 地址
    pptr = &ptr

    // 获取 pptr 的值
    fmt.Printf("变量 a = %d\n", a)
    fmt.Printf("指针变量 *ptr = %d\n", *ptr)
    fmt.Printf("指向指针的变量 **pptr = %d\n", **pptr)
}

// 变量 a = 3000
// 指针变量 *ptr = 3000
// 指向指针的指针变量 **pptr = 3000
```

多次指向的指针即在相应的指针前增加 `*`

<a name="d2b40cda"></a>

#### 10.4.3 Go 向函数传递指针参数

`Go` 语言允许向函数传递指针，只需要在函数定义的参数上设置为指针类型即可。

以下实例演示了如何向函数传递指针，并在函数调用后修改函数内的值：

```go
package main 

import "fmt"

func main() {
    var a int = 100
    var b int = 200

    fmt.Printf("交换前a的值：%d\n", a)
    fmt.Printf("交换前b的值：%d\n", b)

    
    // 调用函数用于交换值
    // &a 指向 a 变量的地址
    // &b 指向 b 变量的地址

    swap(&a, &b)

    fmt.Printf("交换后a的值：%d\n", a)
    fmt.Printf("交换后b的值：%d\n", b)
}

func swap(x *int, y *int) {
    var temp int
    temp = *x // 保存 x 地址的值
    *x = *y // 将 y 赋值给 x
    *y = temp // 将 temp 赋值给 y

    // 简洁写法 *x, *y = *y, *x
}

// 交换前 a 的值 : 100
// 交换前 b 的值 : 200
// 交换后 a 的值 : 200
// 交换后 b 的值 : 100
```

<a name="3a201d9b"></a>

## 十一、Go 语言结构体

`Go` 语言中数组可以存储同一类型的数据，但在结构体中我们可以为 **不同项定义不同的数据类型**。

结构体是由一系列 **具有相同类型** 或 **不同类型的数据** 构成的数据集合。

结构体表示一项记录，比如保存图书馆的书籍记录，每本书有以下属性：

- `Title` ：标题
- `Author` ： 作者
- `Subject`：学科
- `ID`：书籍 `ID`

<a name="e37ccfc1"></a>

### 11.1 定义结构体

结构体定义需要使用 `type` 和 `struct` 语句。`struct` 语句定义一个新的数据类型，结构体中有一个或多个成员。`type` 语句设定了结构体的名称。结构体的格式如下：

```go
type struct_variable_type struct {
    member definition
    member definition
    ...
    member definition
}
```

一旦定义了结构体类型，它就能用于变量的声明，语法格式如下：

```go
variable_name := structure_variable_type {value1, value2, value3, ..., valuen}
或
variable_name := structure_variable_type { key1: value1, key2: value2..., keyn: valuen}
```

```go
package main

import "fmt"

type Books struct {
	title   string
	author  string
	subject string
	book_id int
}

func main() {
	// 创建一个新的结构体
	fmt.Println(Books{"Go 语言", "www.google.com", "Go 语言介绍", 1234567})

	// 也可以使用 key => value 格式
	fmt.Println(Books{title: "Go 语言", author: "www.google.com", subject: "Go 语言介绍", book_id: 1234567})

	// 忽略的字段为 0 或 空
	fmt.Println(Books{title: "Go 语言", author: "www.google.com"})
}

// {Go 语言 www.runoob.com Go 语言教程 6495407}
// {Go 语言 www.runoob.com Go 语言教程 6495407}
// {Go 语言 www.runoob.com  0}
```

<a name="bad2b15a"></a>

### 11.2 访问结构体成员

如果要访问结构体成员，需要使用点号 `.` 操作符，格式为：

```go
结构体.成员名
```

结构体类型变量使用 `struct` 关键字定义，实例如下：

```go
package main

import "fmt"

type Books struct {
   title string
   author string
   subject string
   book_id int
}

func main() {
   var Book1 Books        /* 声明 Book1 为 Books 类型 */
   var Book2 Books        /* 声明 Book2 为 Books 类型 */

   /* book 1 描述 */
   Book1.title = "Go 语言"
   Book1.author = "www.google.com"
   Book1.subject = "Go 语言介绍"
   Book1.book_id = 6495407

   /* book 2 描述 */
   Book2.title = "Python 教程"
   Book2.author = "www.google.com"
   Book2.subject = "Python 语言介绍"
   Book2.book_id = 6495700

   /* 打印 Book1 信息 */
   fmt.Printf( "Book 1 title : %s\n", Book1.title)
   fmt.Printf( "Book 1 author : %s\n", Book1.author)
   fmt.Printf( "Book 1 subject : %s\n", Book1.subject)
   fmt.Printf( "Book 1 book_id : %d\n", Book1.book_id)

   /* 打印 Book2 信息 */
   fmt.Printf( "Book 2 title : %s\n", Book2.title)
   fmt.Printf( "Book 2 author : %s\n", Book2.author)
   fmt.Printf( "Book 2 subject : %s\n", Book2.subject)
   fmt.Printf( "Book 2 book_id : %d\n", Book2.book_id)
}

// Book 1 title : Go 语言
// Book 1 author : www.google.com
// Book 1 subject : Go 语言说明
// Book 1 book_id : 6495407
// Book 2 title : Python 教程
// Book 2 author : www.google.com
// Book 2 subject : Python 语言说明
// Book 2 book_id : 6495700
```

<a name="3b9c2b91"></a>

### 11.3 结构体作为函数参数

你可以像其他数据类型一样将结构体类型作为参数传递给函数。并以以上实例的方式访问结构体变量：

```go
package main

import "fmt"

type Books struct {
    title string
    author string
    subject string
    book_id int
}

func main() {
    var Book1 Books // 声明 Book1 为 Books 类型
    var Book2 Books // 声明 Book2 为 Books 类型

    // book1描述
    Book1.title = "Go 语言"
    Book1.author = "www.google.com"
    Book1.subject = "Go 语言介绍"
    Book1.book_id = 6495407

    /* book 2 描述 */
    Book2.title = "Python 教程"
    Book2.author = "www.google.com"
    Book2.subject = "Python 语言介绍"
    Book2.book_id = 6495700
    
    printBook(Book1)
    printBook(Book2)
}

func printBook(book Books) {
    fmt.Printf( "Book title : %s\n", book.title)
    fmt.Printf( "Book author : %s\n", book.author)
    fmt.Printf( "Book subject : %s\n", book.subject)
    fmt.Printf( "Book book_id : %d\n", book.book_id)
}

// Book title : Go 语言
// Book author : www.google.com
// Book subject : Go 语言说明
// Book book_id : 6495407
// Book title : Python 教程
// Book author : www.google.com
// Book subject : Python 语言说明
// Book book_id : 6495700
```

<a name="9b9abe70"></a>

### 11.4 结构体指针

你可以定义指向结构体的指针类似于其他指针变量，格式如下：

```go
var struct_pointer *Books
```

以上定义的指针变量可以存储结构体变量的地址。查看结构体变量地址，可以将 & 符号放置于结构体变量前：

```go
struct_pointer = &Book1
```

使用结构体指针访问结构体成员，使用 `"."` 操作符：

```go
struct_pointer.title
```

接下来让我们使用结构体指针重写以上实例，代码如下：

```go
package main

import "fmt"

type Books struct {
    title string
    author string
    subject string
    book_id int
}

func main() {
    var Book1 Books // 声明 Book1 为 Books 类型
    var Book2 Books // 声明 Book2 为 Books 类型

    // book1描述
    Book1.title = "Go 语言"
    Book1.author = "www.google.com"
    Book1.subject = "Go 语言介绍"
    Book1.book_id = 6495407

    /* book 2 描述 */
    Book2.title = "Python 教程"
    Book2.author = "www.google.com"
    Book2.subject = "Python 语言介绍"
    Book2.book_id = 6495700
    
    printBook(&Book1)
    printBook(&Book2)
}

func printBook(book *Books) {
    fmt.Printf( "Book title : %s\n", book.title)
    fmt.Printf( "Book author : %s\n", book.author)
    fmt.Printf( "Book subject : %s\n", book.subject)
    fmt.Printf( "Book book_id : %d\n", book.book_id)
}

// Book title : Go 语言
// Book author : www.google.com
// Book subject : Go 语言说明
// Book book_id : 6495407
// Book title : Python 教程
// Book author : www.google.com
// Book subject : Python 语言说明
// Book book_id : 6495700
```

例子：

```go
package main

import "fmt"

type Books struct {
    title string
    author string
    subject string
    book_id int
}

func changeBook(book Books) {
    book.title = "book1_change"
}

func main() {
    var book1 Books
    book1.title = "book1"
    book1.author = "zuozhe"
    book1.book_id = 1
    changeBook(book1)
    fmt.Println(book1)
}
// {book1 zuozhe 1}
```

如果想在函数里面改变结构体数据内容，需要传入指针：

```go
package main

import "fmt"

type Books struct {
    title string
    author string
    subject string
    book_id int
}

func changeBook(book *Books) {
    book.title = "book1_change"
}

func main() {
    var book1 Books
    book1.title = "book1"
    book1.author = "zuozhe"
    book1.book_id = 1
    changeBook(&book1)
    fmt.Println(book1)
}
// {book1_change zuozhe  1}
```

<a name="d6d98421"></a>

### 11.5 "类" 的创建

`struct` 类似于 `java` 中的类，可以在 `struct` 中定义成员变量。

> ## 要访问成员变量，可以有两种方式：
>
> 1. 通过 `struct 成员.变量` 变量来访问
>
> - 2. 通过 `struct 指针.成员` 变量来访问

不需要通过 `getter`, `setter` 来设置访问权限。

```go
type Rect struct { // 定义矩形类
    x, y float64 // 类型只包含属性，并没有方法
    width, height float64
}
func (r *Rect) Area() float64 { // 为 Rect 类型绑定 Area 的方法，*Rect 为指针引用可以修改传入参数的值
    return r.width * r.height // 方法归于类型，不归属于具体的对象，声明该类型的对象即可调用该类型的方法
}
```

<a name="6c06e114"></a>

## 十二、Go 语言切片（slice）- 动态数组

`Go` 语言切片是对数组的抽象。

`Go` 数组的长度不可改变，在特定场景中这样的集合就不太适用，`Go` 中提供了一种灵活，功能强悍的内置类型切片(`"动态数组"`),与数组相比切片的长度是不固定的，可以追加元素，在追加时可能使切片的容量增大。

<a name="52ee6482"></a>

### 12.1 定义切片

你可以声明一个 **`未指定大小`** 的数组来定义切片：

```go
var identifier []type
```

切片不需要说明长度。

或使用 **`make()函数`** 来创建切片:

```go
var slice1 []type = make([]type, len)

也可以简写为：

slice1 := make([]type, len)
```

也可以指定容量，其中 `capacity` 为可选参数。

```go
make([]T, length, capacity)
```

这里 `len` 是 **`数组的长度`** 并且也是 **`切片的初始长度`**。

<a name="b22e35a9"></a>

### 12.2 切片初始化

直接初始化切片，`[]` 表示是切片类型，`{1,2,3}` 初始化值依次是 `1,2,3` .其 `cap=len=3`

```go
s := []int{1, 2, 3}
```

初始化切片 `s`,是数组 `arr` 的引用

```go
s := arr[:]
```

将 `arr` 中从下标 `startIndex` 到 `endIndex - 1` 下的元素创建为一个新的切片

```go
s := arr[startIndex:endIndex]
```

默认 `endIndex` 时将表示一直到 `arr` 的最后一个元素

```go
s := arr[startIndex:]
```

默认 `startIndex` 时将表示表示从 `arr` 的第一个元素开始

```go
s1 := arr[:endIndex]
```

通过切片 `s` 初始化切片 `s1`

```go
s1 := s[startIndex:endIndex]
```

通过内置函数 `make()` 初始化切片 `s`, `[]int` 标识为其元素类型为 `int` 的切片

```go
s := make([]int, len, cap)
```

<a name="b3e70d6b"></a>

### 12.3 `len()` 和 `cap()` 函数

切片是可索引的，并且可以由 `len()` 方法获取长度。

切片提供了计算容量的方法 `cap()` 可以 **`测量切片最长可以达到多少`**。

```go
package main

import "fmt"

func main() {
    var numbers = make([]int, 3, 5)
    printSlice(numbers)
}

func printSlice(x []int) {
    fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

// len=3 cap=5 slice=[0 0 0]
```

<a name="df678fa4"></a>

### 12.4 `nil` 空切片

一个切片在未初始化之前默认为 `nil`，长度为 `0`，实例如下：

```go
package main

import "fmt"

func main() {
    var numbers []int
    printSlice(numbers)
    if(numbers == nil) {
        fmt.Printf("切片是空的")
    }
}

func printSlice(x []int) {
    fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

// len=0 cap=0 slice=[]
// 切片是空的
```

<a name="5ce4d0f6"></a>

### 12.5 切片截取

可以通过设置下限及上限来设置截取切片 `[lower-bound:upper-bound]`，实例如下：

```go
package main

import "fmt"

func main() {
    // 创建切片
    numbers := []int{0, 1, 2, 3, 4, 5, 6, 7, 8}
    printSlice(numbers)

    // 打印原始切片
    fmt.Println("numbers == ", numbers)

    // 打印子切片从索引 1(包含) 到索引4(不包含)
    fmt.Println("numbers[1:4] == ", numbers[1:4])

    // 默认下限为 0
    fmt.Println("numbers[:3] == ", numbers[:3])

    // 默认上限为 len(s)
    fmt.Println("numbers[4:] == ", numbers[4:])

    numbers1 := make([] int, 0, 5)
    printSlice(numbers1)

    // 打印子切片从索引 0(包含) 到索引 2(不包含)
    numbers2 := numbers[:2]
    printSlice(numbers2)

    // 打印子切片从索引 2(包含) 到索引 5(不包含)
    numbers3 := numbers[2:5]
    printSlice(numbers3)
}

func printSlice(x []int) {
    fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

// len=9 cap=9 slice=[0 1 2 3 4 5 6 7 8]
// numbers == [0 1 2 3 4 5 6 7 8]
// numbers[1:4] == [1 2 3]
// numbers[:3] == [0 1 2]
// numbers[4:] == [4 5 6 7 8]
// len=0 cap=5 slice=[]
// len=2 cap=9 slice=[0 1]
// len=3 cap=7 slice=[2 3 4]
```

<a name="e90f35a4"></a>

### 12.6 `append()` 和 `copy()` 函数

如果想增加切片的容量，我们必须 **`创建一个新的更大的切片`** 并把 **`原分片的内容都拷贝过来`**。

下面的代码描述了从拷贝切片的 `copy 方法` 和 `向切片追加新元素的 append 方法`。

```go
package main

import "fmt"

func main() {
    var numbers []int
    printSlice(numbers)
    
    // 允许追加空切片
    numbers = append(numbers, 0)
    printSlice(numbers)

    // 向切片添加一个元素
    numbers = append(numbers, 1)
    printSlice(numbers)

    // 同时添加多个元素
    numbers = append(numbers, 2, 3, 4)
    printSlice(numbers)

    // 创建切片 numbers1 是之前切片的两倍容量
    numbers1 := make([] int, len(numbers), (cap(numbers)) * 2)

    // 拷贝 numbers 的内容到 numbers1
    copy(numbers1, numbers)
    printSlice(numbers1)
}

func printSlice(x []int){
   fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}

// len=0 cap=0 slice=[]
// len=1 cap=1 slice=[0]
// len=2 cap=2 slice=[0 1]
// len=5 cap=6 slice=[0 1 2 3 4]
// len=5 cap=12 slice=[0 1 2 3 4]
```

<a name="ca58a416"></a>

## 十三、Go 语言范围 (`range`)

`Go` 语言中 `range` 关键字用于 `for` 循环中 *`迭代数组(array)`* 、*`切片(slice)`*、*`通道(channel)`* 或 *`集合(map)`* 的元素。在 *`数组`* 和 *`切片`* 中它 *`返回元素的索引和索引对应的值`*，在 *`集合`* 中返回 *`key-value`* 对。

```go
package main

import "fmt"

func main() {
    // 这是我们使用 range 去求一个 slice 的和。使用数组跟这个很类似
    nums := []int{2, 3, 4}
    sum := 0
    for _, num := range nums {
        sum += num
    }
    fmt.Println("sum: ", sum)

    // 在数组上使用 range 将传入 index 和 值 两个变量。上面那个例子我们不需要使用该元素的序号，所以我们使用空白符"_"省略了。有时侯我们确实需要知道它的索引。
    for i, num := range nums {
        if num == 3 {
            fmt.Println("index: ", i)
        }
    }

    // range 也可以用在 map 的键值对上
    kvs := map[string] string{"a": "apple", "b": "banana"}
    for k, v := range kvs {
        fmt.Printf("%s -> %s\n", k, v)
    }

    // range 也可以用来枚举 Unicode 字符串。第一个参数是字符的索引，第二个值是字符(Unicode的值)本身
    for i, c::= range "go" {
        fmt.Println(i, c)
    }
}

// sum: 9
// index: 1
// a -> apple
// b -> banana
// 0 103
// 1 111
```

<a name="1592bb13"></a>

## 十四、Go 语言 Map(集合)

`Map` 是一种 ***无序的键值对的集合*** 。`Map` 最重要的一点是通过 `key` 来快速检索数据，`key` 类似于索引，指向数据的值。

`Map` 是一种集合，所以我们可以像 *`迭代数组`* 和 *`切片`* 那样迭代它。不过，`Map` 是无序的，我们无法决定它的返回顺序，这是因为 `Map` 是使用 `hash` 表来实现的。

<a name="352740a0"></a>

### 14.1 定义 `Map`

可以使用内建函数 `make` 也可以使用 `map` 关键字来定义 `Map`:

```go
// 声明变量，默认 map 是 nil
var map_variable map[key_data_type] value_data_type

// 使用 make 函数
map_variable := make(map[key_data_type] value_data_type)
```

如果不初始化 `map`，那么就会创建一个 `nil map`。`nil map` 不能用来存放键值对

下面实例演示了创建和使用 `map`:

```go
package main

import "fmt"

func main() {
    var countryCapticalMap map[string] string
    countryCapticalMap = make(map[string] string)

    // map 插入 key-value 时，各个国家对应的首都
    countryCapticalMap["France"] = "巴黎"
    countryCapticalMap["Italy"] = "罗马"
    countryCapticalMap["Japan"] = "日本"
    countryCapticalMap["India"] = "新德里"
    
    // 使用键输出 Map 值
    for country := range countryCapticalMap {
        fmt.Println(country, "首都是", countryCapticalMap[country])
    }

    // 查看元素在集合中是否存在
    captical, ok := countryCapticalMap["American"]
    fmt.Println(captical, ok)
    if(ok) {
        fmt.Println("American的首都是", captical)
    } else {
        fmt.Println("American的首都不存在")
    }
}

// France 首都是 巴黎
// Italy 首都是 罗马
// Japan 首都是 东京
// India  首都是 新德里
// American 的首都不存在
```

<a name="cebcc7fc"></a>

### 14.2 `delete()` 函数

`delete()` 函数用于删除集合的元素，参数为 `map` 和其对应的 `key`。

```go
package main

import "fmt"

func main() {
    // 创建 map
    countryCapitalMap := map[string]string{"France": "Paris", "Italy": "Rome", "Japan": "Tokyo", "India": "New delhi"}

    fmt.Println("原始 Map")

    // 打印 Map
    for country := range countryCapticalMap {
        fmt.Println(country, "首都是", countryCapticalMap[country])
    }

    // 删除元素
    delete(countryCapticalMap, "France")
    fmt.Println("法国条目被删除")

    fmt.Println("删除元素后 Map")

    // 打印 Map
    for country := range countryCapticalMap {
        fmt.Println(country, "首都是", countryCapticalMap[country])
    }
}
// 原始 Map
// India 首都是 New delhi
// France 首都是 Paris
// Italy 首都是 Rome
// Japan 首都是 Tokyo
// 法国条目被删除
// 删除元素后 Map
// Italy 首都是 Rome
// Japan 首都是 Tokyo
// India 首都是 New delhi
```

<a name="7d11b350"></a>

### 14.3 基于 `go` 实现简单的 `HashMap`

暂未做 `key` 值的校验

```go
package main

import "fmt"

type HashMap struct {
	key      string
	value    string
	hashCode int
	next     *HashMap
}

var table [16](*HashMap)

func initTable() {
	for i := range table {
		table[i] = &HashMap{"", "", i, nil}
	}
}

func getInstance() [16](*HashMap) {
	if table[0] == nil {
		initTable()
	}
	return table
}

func getHashCode(k string) int {
	if len(k) == 0 {
		return 0
	}
	var hashCode int = 0
	var lastIndex int = len(k) - 1
	for i := range k {
		if i == lastIndex {
			hashCode += int(k[i])
			break
		}
		hashCode += (hashCode + int(k[i])) * 31
	}
	return hashCode
}

func IndexTable(hashCode int) int {
	return hashCode % 16
}

func indexNode(hashCode int) int {
	return hashCode >> 4
}

func put(k string, v string) string {
	var hashCode = getHashCode(k)
	var thisNode = HashMap{k, v, hashCode, nil}

	var tableIndex = IndexTable(hashCode)
	var nodeIndex = indexNode(hashCode)

	var headPtr [16](*HashMap) = getInstance()
	var headNode = headPtr[tableIndex]

	if (*headNode).key == "" {
		*headNode = thisNode
		return ""
	}

	var lastNode *HashMap = headNode
	var nextNode *HashMap = (*headNode).next

	for nextNode != nil && (indexNode((*nextNode).hashCode) < nodeIndex) {
		lastNode = nextNode
		nextNode = (*nextNode).next
	}

	if (*lastNode).hashCode == thisNode.hashCode {
		var oldValue string = lastNode.value
		lastNode.value = thisNode.value
		return oldValue
	}

	if lastNode.hashCode < thisNode.hashCode {
		lastNode.next = &thisNode
	}

	if nextNode != nil {
		thisNode.next = nextNode
	}
	return ""
}

func get(k string) string {
	var hashCode = getHashCode(k)
	var tableIndex = IndexTable(hashCode)

	var headPtr [16](*HashMap) = getInstance()
	var node *HashMap = headPtr[tableIndex]

	if (*node).key == k {
		return (*node).value
	}

	for (*node).next != nil {
		if k == (*node).key {
			return (*node).value
		}
		node = (*node).next
	}
	return ""
}

// for example
func main() {
	getInstance()
	put("a", "a_put")
	put("b", "b_put")
	fmt.Println(get("a"))
	fmt.Println(get("b"))
	put("p", "p_put")
	fmt.Println(get("p"))
}

// a_put
// b_put
// p_put
```

<a name="f8f6c633"></a>

## 十五、Go 语言递归函数

递归，就是在运行的过程中调用自己。

语法格式如下：

```go
func recursion() {
   recursion() /* 函数调用自身 */
}

func main() {
   recursion()
}
```

`Go` 语言支持递归。但我们在使用递归时，开发者需要 **`设置退出条件`**，否则递归 **`将陷入无限循环中`**。

递归函数对于解决数学上的问题是非常有用的，就像 *`计算阶乘`*，生成 *`斐波那契数列`* 等。

<a name="46a9a9c6"></a>

### 15.1 阶乘

```go
package main 

import "fmt"

func Factorial(n uint64)(result uint64) {
    if n > 0 {
        result = n * Factorial(n - 1)
        return result
    }
    return 1
}

func main() {
    var i int = 15
    fmt.Printf("%d 的阶乘是 %d\n", i, Factorial(uint64(i)))
}

// 15 的阶乘是 1307674368000
```

<a name="1ac8696e"></a>

### 15.2 斐波那契数列

```go
package main

import "fmt"

func fibonacci(n int) int {
    if n < 2 {
        return n
    }
    return fibonacci(n - 2) + fibonacci(n - 1)
}

func main() {
    var i int
    for i = 0; i < 10; i++ {
        fmt.Printf("%d\t", fibonacci(i))
    }
}

// 0	1	1	2	3	5	8	13	21	34
```

<a name="baf11e4b"></a>

## 十六、Go 语言类型转换

类型转换用于将一种数据类型的变量转换为另外一种类型的变量。`Go` 语言类型转换基本格式如下：

```go
// type_name 为类型，expression 为表达式。
type_name(expression)
```

以下实例中将整型转化为浮点型，并计算结果，将结果赋值给浮点型变量：

```go
package main

import "fmt"

func main() {
    var sum int = 17
    var count int = 5
    var mean float32

    mean = float32(num) / float32(count)
    fmt.Printf("mean 的值为: %f\n",mean)
}
// mean 的值为: 3.400000
```

<a name="70f8fd73"></a>

## 十七、Go 语言接口

`Go` 语言提供了另外一种数据类型即接口，它把所有的具有共性的方法定义在一起，任何其他类型只要实现了这些方法就是实现了这个接口。

```go
// 定义接口
type interface_name interface {
    method_name1 [return_type]
    method_name2 [return_type]
    method_name3 [return_type]
    ...
    method_namen [return_type]
}
// 结构体
type struct_name struct {

}
// 实现接口方法
func (struct_name_variable struct_name) method_name1() [return_type] {
    // 方法实现
}
...
func (struct_name_variable struct_name) method_namen() [return_type] {
    // 方法实现
}
```

实例：

```go
package main

import "fmt"

type Phone interface {
    call()
}

type NokiaPhone struct {}

func (nokiaPhone NokiaPhone) call() {
    fmt.Println("I am Nokia, I can call you!")
}

type IPhone struct {}

func (iPhone IPhone) call() {
    fmt.Println("I am iPhone, I can call you")
}

func main() {
    var phone Phone

    phone = new(NokiaPhone)
    phone.call()

    phone = new(IPhone)
    phone.call()
}
```

在上面的例子中，我们定义了一个接口 `Phone`，接口里面有一个方法 `call()`。然后我们在 `main` `函数里面定义了一个Phone` 类型变量，并分别为之赋值为 `NokiaPhone` 和 `IPhone`。然后调用 `call()` 方法，输出结果如下：

```go
I am Nokia, I can call you!
I am iPhone, I can call you!
```

<a name="e9c604ab"></a>

## 十八、Go 错误处理

`Go` 语言通过 **`内置的错误接口`** 提供了非常简单的错误处理机制。

`error` 类型是一个接口类型，这是它的定义：

```go
type error interface {
    Error() string
}
```

我们可以在编码中通过实现 `error` 接口类型来生成错误信息。

函数通常在最后的返回值中返回错误信息。使用 `errors.New`  可返回一个错误信息：

```go
func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, errors.New("math: square root of negative number")
    }
    // 实现
}
```

在下面的例子中，我们在调用 `Sqrt` 的时候传递的一个负数，然后就得到了 `non-nil` 的 `error` 对象，将此对象与 `nil` 比较，结果为 `true`，所以 `fmt.Println` ( `fmt` 包在处理 `error` 时会调用 `Error` 方法)被调用，以输出错误，请看下面调用的示例代码：

```go
result, err := sqrt(-1)

if err != nil {
    fmt.Println(err)
}
```

实例：

```go
package main

import "fmt"

// 定义一个 DivideError 结构
type DivideError struct {
    dividee int
    divider int
}

// 实现 error 接口
func (de *DivideError) Error() string {
    strFormat := `
        Cannot proceed, the divider is zero.
        dividee: %d
        divider: 0
    `
    return fmt.Sprintf(strFormat, de.dividee)
}

// 定义 int 类型除法运算的函数
func Divide(varDividee int, varDivider int) (result int, errorMsg string) {
    if varDivider == 0 {
        dData := DivideError{
            dividee: varDividee,
            divider: varDivider,
        }
        errorMsg = dData.Error()
        return
    } else {
        return varDividee / varDivider, ""
    }
}

func main() {
    // 正常情况
    if result, errorMsg := Divide(100, 10); errorMsg == "" {
        fmt.Println("100 / 10 = ", result)
    }
    // 当被除数为零的时候会返回错误信息
    if _, errorMsg := Divide(100, 0); errorMsg != "" {
        fmt.Println("errorMsg is: ", errorMsg)
    }
}

// 100/10 =  10
// errorMsg is:  
//     Cannot proceed, the divider is zero.
//     dividee: 100
//     divider: 0
```

<a name="0b34be10"></a>

## 十九、Go 并发

`Go` 语言支持并发，我们只需要通过 `go` 关键字来开启 `goroutine` 即可。

`goroutine` 是轻量级线程，`goroutine` 的调度是由 `Golang` 运行时进行管理的。

`goroutine` 语法格式：

```go
go 函数名 { 参数列表 }
```

例如：

```go
go f(x, y, z)
```

开启一个新的 `goroutine`:

```go
f(x, y, z)
```

`Go` 允许使用 `go` 语句开启一个新的 **`运行期线程`**， 即 `goroutine`，以一个不同的、新创建的 `goroutine` 来执行一个函数。 同一个程序中的 **`所有 goroutine 共享同一个地址空间`**。

```go
package main

import (
    "fmt"
    "time"
)

func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Println(s)
    }
}

func main() {
    go say("world")
    say("hello")
}
```

执行以上代码，你会看到输出的 `hello` 和 `world` 是没有固定先后顺序。因为它们是两个 `goroutine` 在执行：

```go
// world
// hello
// hello
// world
// world
// hello
// hello
// world
// world
// hello
```

<a name="aff3472b"></a>

### 19.1 通道（`channel`）

通道（`channel`）是 **`用来传递数据的一个数据结构`**。

通道可用于两个 `goroutine` 之间通过传递一个指定类型的值来同步运行和通讯。操作符 `<-` 用于指定通道的方向，**`发送或接收`**。如果 **`未指定方向，则为双向通道`**。

```go
ch <- v // 把 v 发送到通道 ch
v := <-ch // 从 ch 接收数据，并把值赋给 v
```

声明一个通道很简单，我们使用 `chan` 关键字即可，通道在使用前必须先创建：

```go
ch := make(chan int)
```

**注意**：默认情况下，通道是不带缓冲区的。发送端发送数据，同时必须又接收相应的接收数据。
以下实例通过两个 `goroutine` 来计算数字之和，在 `goroutine` 完成计算后，它会计算两个结果的和：

```go
package main

import "fmt"

func sum(s []int, c chan int)  {
	sum := 0
	for _, v := range s {
		sum += v
	}
	// 把 sum 发送到通道 c
	c <- sum
}

func main()  {
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s) / 2], c)
	go sum(s[len(s) / 2:], c)
	// 从通道 c 中接收
	x, y := <-c, <-c
	fmt.Println(x, y, x + y)
}
```

<a name="21f47adc"></a>

### 19.2 通道缓冲区

通道可以设置缓冲区，通过 `make` 的第二个参数指定缓冲区大小：

```go
ch := make(chan int, 100)
```

带缓冲区的通道允许发送端的数据发送和接收端的数据获取处于 **`异步状态`** ，就是说 **`发送端发送的数据可以放在缓冲区里面，可以等待接收端去获取数据，而不是立刻需要接收端去获取数据`**。

不过由于 **`缓冲区的大小是有限的`**，所以还是必须有接收端来接收数据的，否则缓冲区一满，数据发送端就无法再发送数据了。

如果没有设置容量，或者容量设置为 `0`, 说明 `Channel` 没有缓存，只有 `sender` 和 `receiver` 都准备好了后，
它们的通讯( `communication` )才会发生( `Blocking` )。如果设置了缓存，就有可能不发生阻塞， 只有 `buffer` 满了后 `send` 才会阻塞，
而只有缓存空了后 `receive` 才会阻塞。一个 `nil channel` 不会通信。

注意：

- 如果通道不带缓冲，发送方会阻塞直到接收方从通道中接收了值。如果通道带缓冲，发送方则会阻塞直到发送的值被拷贝到缓冲区内；
- 如果缓冲区已满，则意味着需要等待直到某个接收方获取到一个值。接收方在有值可以接收之前会一直阻塞。

实例：

```go
package main

import "fmt"

func main()  {
	// 这里我们定义了一个可以存储整数类型的带缓冲通道
	// 缓冲区大小为 2
	ch := make(chan int, 2)

	// 因为 ch 是带缓冲的通道，我们可以同时发送两个数据
	// 而不用立刻需要去同步读取数据
	ch <- 1
	ch <- 2

	// 获取这两个数据
	fmt.Println(<-ch)
	fmt.Println(<-ch)
}

// 1
// 2
```

<a name="01e1d94b"></a>

### 19.3 `Go` 遍历通道与关闭通道

`Go` 通过 `range` 关键字来实现遍历读取到的数据，类似于与数组或切片。格式如下：

```go
v, ok := <-ch
```

例子：

```go
func main() {
    go func() {
        time.Sleep(1 * time.Hour)
    }()
    c := make(chan int)
    go func() {
        for i := 0; i < 10; i = i + 1 {
            c <- i
        }
        close(c)
    }()
    for i := range c {
        fmt.Println(i)
    }
    fmt.Println("Finished")
}
```

如果通道接收不到数据后 `ok` 就为 `false`，这时通道就可以使用 `close()` 函数来 **`关闭`**。

斐波那契序列：

```go
package main

import (
	"fmt"
)

func fibonacci(n int, c chan int)  {
	x, y := 0, 1
	for i := 0; i < n; i++ {
		c <- x
		x, y = y, x + y
	}
	close(c)
}

func main() {
	c := make(chan int, 10)
	// 此处 cap 函数返回的是缓冲区的长度
	go fibonacci(cap(c), c)
	/*
	 	range 函数遍历每个从通道接收到的数据，因为 c 在发送完 10 个数据之后就关闭了通道，所以
		这里我们 range 函数在接收到 10 个数据之后就结束了。如果上面的 c 通道不关闭，那么 range 函数
		就不会结束，从而在接收第 11 个数据的时候就阻塞了
	 */
	for i := range c {
		fmt.Println(i)
	}
}
```

<a name="61fb330f"></a>

### 19.4 `Channel` 类型

`Channel` 类型的定义格式如下：

```go
ChannelType = ( "chan" | "chan" "<-" | "<-" "chan" ) ElementType
```

它包括 **`三种类型的定义`** 。可选的 `<-` 代表 `channel` 的方向。如果没有指定方向，那么 `Channel` 就是双向的，既可以接收数据，也可以发送数据。

```go
chan T          // 可以接收的发送类型为 T 的数据
chan<- float64  // 只可以用来发送 float64 类型的数据
<-chan int      // 只可以用来接收 int 类型的数据
```

`<-` 总是优先和最左边的类型结合。

```go
chan<- chan int // 等价 chan<- (chan int)
chan<- <-chan int // 等价 chan<- (<-chan int)
<-chan <-chan int // 等价 <-chan (<-chan int)
chan (<-chan int)
```

你可以在多个 `goroutine` 从/往 一个 `channel` 中 `receive/send` 数据, 不必考虑额外的同步措施。

`Channel` 可以作为一个 **`先入先出(FIFO)`** 的队列，接收的数据和发送的数据的顺序是一致的。

`channel` 的 `receive` 支持 `多值赋值(multi-valued assignment)`，如

```go
v, ok := <-ch
```

可以使用一个 **`额外的返回参数`** 来检查 `channel` 是否关闭。

```go
x, ok := <-ch
x, ok = <-ch
var x, ok = <-ch
```

如果 `OK` 是 `false`，表明接收的 `x` 是产生的零值，这个 `channel` 被关闭了或者为空。

<a name="49c37d15"></a>

### 19.5 blocking

默认情况下，发送和接收会一直阻塞着，直到另一方准备好。这种方式可以用来在 `gororutine` 中进行同步，而不必使用 **`显示的锁`** 或者 **`条件变量`**。

如官方的例子中 `x, y := <-c, <-c` 这句会一直等待计算结果发送到 `channel` 中。

```go
package main

import "fmt"

func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // send sum to c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}
c := make(chan int)
    go sum(s[:len(s)/2], c)
    go sum(s[len(s)/2:], c)
    x, y := <-c, <-c // receive from c
    fmt.Println(x, y, x+y)
}
```

<a name="92548648"></a>

### 19.6 timeout

`select` 有很重要的一个应用就是 **`超时处理`**。 因为上面我们提到，如果没有 `case` 需要处理，`select` 语句就会一直阻塞着。
这时候我们可能就需要一个超时操作，用来处理超时的情况。

下面这个例子我们会在 `2` 秒后往 `channel c1` 中发送一个数据，但是 `select` 设置为 `1` 秒超时,因此我们会打印出 `timeout 1`,而不是 `result 1`。

其实它利用的是 `time.After` 方法，它返回一个类型为 `<-chan Time` 的单向的 `channel`，在指定的时间发送一个当前时间给返回的 `channel` 中。

```go
package main

import (
    "time"
    "fmt"
)

func main() {
    c1 := make(chan string, 1)
    go func() {
        time.Sleep(time.Second * 2)
        c <- "result 1"
    }()
    select {
        case res := <-c1:
            fmt.Println(res)
        case <-time.After(time.Second * 1):
            fmt.Println("timeout 1")
    }
}
```

<a name="083d10bf"></a>

### 19.7 `Timer` 和 `Ticker`

我们看一下关于时间的两个 `Channel`。

<a name="a8bd10e9"></a>

#### 19.7.1 `Timer`

`timer` 是一个定时器，代表未来的一个单一事件，你可以告诉 `timer` 你要等待多长时间，它提供一个 `Channel`，在将来的那个时间那个 `Channel` 提供了一个时间值。

下面的例子中第二行会阻塞 `2` 秒钟左右的时间，直到时间到了才会继续执行。

```go
timer1 := time.NewTimer(time.Second * 2)
<-timer1.C
fmt.Println("Timer 1 expired")
```

当然如果你只是想 **`单纯的等待`** 的话，可以使用 **`time.Sleep`** 来实现。

你还可以使用 **`timer.Stop 来停止计时器`**。

```go
    timer2 := time.NewTimer(time.Second)
    go func() {
        <-timer2.C
        fmt.Println("Timer 2 expired")
    }()
    stop2 := timer2.Stop()
    if stop2 {
        fmt.Println("Timer 2 stopped")
    }
```

<a name="8bb24a84"></a>

#### 19.7.2 `Ticker`

`ticker` 是一个定时触发的计时器，它会以一个间隔( `interval` )往 `Channel` 发送一个事件(当前时间)，
而 `Channel` 的接收者可以以固定的时间间隔从 `Channel` 中读取事件。

下面的例子中 `ticker` 每 `500` 毫秒触发一次，你可以观察输出的时间。

```go
ticker := time.NewTicker(time.Millisecond * 500)
go func() {
    for t := range ticker,C {
        fmt.Println("Tick at", t)
    }
}
```

类似 `timer`, `ticker` 也可以通过 `Stop` 方法来停止。一旦它停止，接收者不再会从 `channel` 中接收数据了。

<a name="237a5b8d"></a>

### 19.8 close

内建的 `close` 方法可以用来关闭 `channel`。

总结一下 `channel` 关闭后 `sender` 的 `receiver` 操作。

如果 `channel c` 已经被关闭,继续往它发送数据会导致 `panic: send on closed channel:`

```go
import "time"

func main() {
    go func() {
        time.Sleep(time.Hour)
    }()
    c := make(chan int, 10)
    c <- 1
    c <- 2
    close(c)
    c <- 3
}
```

但是从这个关闭的 `channel` 中不但可以读取出已发送的数据，还可以不断的读取零值:

```go
c := make(chan int, 10)
c <- 1
c <- 2
close(c)
fmt.Println(<-c) // 1
fmt.Println(<-c) // 2
fmt.Println(<-c) // 0
fmt.Println(<-c) // 0
```

但是如果通过 `range` 读取，`channel` 关闭后 `for` 循环会跳出：

```go
c := make(chan int, 10)
c <- 1
c <- 2
close(c)
for i := range c {
    fmt.Println(i)
}
```

通过 `i, ok := <-c` 可以查看 `Channel` 的状态，判断值是零值还是正常读取的值。

```go
c := make(chan int, 10)
close(c)
i, ok := <-c
fmt.Printf("%d, %t", i, ok) //0, false
```

<a name="79cd7fc6"></a>

### 19.9 同步

`channel` 可以用在 `goroutine` 之间的同步。

下面的例子中 `main goroutine` 通过 `done channel` 等待 `worker` 完成任务。 `worker` 做完任务后只需往 `channel` 发送一个数据就可以通知
`main goroutine` 任务完成。

```go
import (
    "fmt"
    "time"
)
func worker(done chan bool) {
    time.Sleep(time.Second)
    // 通知任务已完成
    done <- true
}
func main() {
    done := make(chan bool, 1)
    go worker(done)
    // 等待任务完成
    <-done
}
```

<a name="09583159"></a>

## 二十、占位符

定义示例类型和变量

```go
type Human struct {
    Name string
}
var people = Human{Name:"zhangsan"}
```

<a name="b48a4a38"></a>

### 20.1 普通占位符

| 占位符 | 说明 | 举例 | 输出 |
| :--- | :--- | :--- | :--- |
| `%v` | 相应值的默认格式。 | `Printf("%v", people)` | `{zhangsan}` |
| `%+v` | 打印结构体时，会添加字段名 | `Printf("%+v", people)` | `{Name:zhangsan}` |
| `%#v` | 相应值的 `Go` 语法表示 | `Printf("#v", people)` | `main.Human{Name:"zhangsan"}` |
| `%T` | 相应值的类型的 `Go` 语法表示 | `Printf("%T", people)` | `main.Human` |
| `%%` | 字面上的百分号，并非值的占位符 | `Printf("%%")` | `%` |

<a name="45c11863"></a>

### 20.2 布尔占位符

| 占位符 | 说明 | 举例 | 输出 |
| :--- | :--- | :--- | :--- |
| `%t` | `true` 或 `false` | `Printf("%t", true)` | `true` |

<a name="74762d15"></a>

### 20.3 整数占位符

| 占位符 | 说明 | 举例 | 输出 |
| :--- | :--- | :--- | :--- |
| `%b` | 二进制表示 | `Printf("%b", 5)` | `101` |
| `%c` | 相应 `Unicode` 码点所表示的字符 | `Printf("%c", 0x4E2D)` | 中 |
| `%d` | 十进制表示 | `Printf("%d", 0x12)` | `18` |
| `%o` | 八进制表示 | `Printf("%d", 10)` | `12` |
| `%q` | 单引号围绕的字符字面值，由 `Go` 语法安全地转义 | `Printf("%q", 0x4E2D)` | '中' |
| `%x` | 十六进制表示，字母形式为小写 `a-f` | `Printf("%x", 13)` | `d` |
| `%X` | 十六进制表示，字母形式为大写 `A-F` | `Printf("%x", 13)` | `D` |
| `%U` | `Unicode` 格式：`U+1234`，等同于 `"U+%04X"` | `Printf("%U", 0x4E2D)` | `U+4E2D` |

<a name="1664ce0e"></a>

### 20.4 浮点数和复数的组成部分（实部和虚部）

| 占位符 | 说明 | 举例 | 输出 |
| :--- | :--- | :--- | :--- |
| `%b` | 无小数部分的，指数为二的幂的科学计数法， 与 `strconv.FormatFloat` 的 `'b'` 转换格式一致。例如 `-123456p-78` |  |  |
| `%e` | 科学计数法，例如 `-1234.456e+78` | `Printf("%e", 10.2)` | `1.020000e+01` |
| `%E` | 科学计数法，例如 `-1234.456E+78` | `Printf("%e", 10.2)` | `1.020000E+01` |
| `%f` | 有小数点而无指数，例如 `123.456` | `Printf("%f", 10.2)` | `10.200000` |
| `%g` | 根据情况选择 `%e` 或 `%f` 以产生更紧凑的（无末尾的 `0`）输出 | `Printf("%g", 10.20)` | `10.2` |
| `%G` | 根据情况选择 `%E` 或 `%f` 以产生更紧凑的（无末尾的 `0`）输出 | `Printf("%G", 10.20+2i)` | `(10.2+2i)` |

<a name="2ba31772"></a>

### 20.5 字符串与字节切片

| 占位符 | 说明 | 举例 | 输出 |
| :--- | :--- | :--- | :--- |
| `%s` | 输出字符串表示（`string` 类型或 `[]byte`) | `Printf("%s", []byte("Go语言"))` | `Go` 语言 |
| `%q` | 双引号围绕的字符串，由 `Go` 语法安全地转义 | `Printf("%q", "Go语言")` | "`Go` 语言" |
| `%x` | 十六进制，小写字母，每字节两个字符 | `Printf("%x", "golang")` | `676f6c616e67` |
| `%X` | 十六进制，大写字母，每字节两个字符 | `Printf("%X", "golang")` | `676F6C616E67` |

<a name="4d2c0a56"></a>

### 20.6 指针

| 占位符 | 说明 | 举例 | 输出 |
| :--- | :--- | :--- | :--- |
| `%p` | 十六进制表示，前缀 `0x` | `Printf("%p", &people)` | `0x4f57f0` |

<a name="d4546e4a"></a>

### 20.5 其它标记

| 占位符 | 说明 | 举例 | 输出 |
| :--- | :--- | :--- | :--- |
| `+` | 总打印数值的正负号；对于 `%q（%+q）` 保证只输出 `ASCII` 编码的字符。 | `Printf("%+q", "中文")` | `"\u4e2d\u6587"` |
| `-` | 在右侧而非左侧填充空格（左对齐该区域） |  |  |
| `#` | 备用格式：为八进制添加前导 `0（%#o）` 为十六进制添加前导 `0x（%#x）` 或 `0X（%#X）`，为 `%p（%#p）` 去掉前导 `0x`； 如果可能的话，`%q（%#q）` 会打印原始 （即反引号围绕的）字符串； 如果是可打印字符，`%U（%#U）` 会写出该字符的 `Unicode` 编码形式（如字符 `x` 会被打印成 `U+0078 'x'`）。 | `Printf("%#U", '中')` | `U+4E2D` |
| `''` | (空格)为数值中省略的正负号留出空白（% d）； 以十六进制（% x, % X）打印字符串或切片时，在字节之间用空格隔开 |  |  |
| `0` | 填充前导的0而非空格；对于数字，这会将填充移到正负号之后 |  |  |
