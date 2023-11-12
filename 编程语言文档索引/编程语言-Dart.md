---
title: 编程语言-Dart
url: https://www.yuque.com/wcywxq/rddgev/thzcqg
---

<a name="a2578a9d"></a>

# Dart语言入门教程

[**官方网站：https://dart.dev/**](https://dart.dev/)

[**中文网站：http://dart.goodev.org/**](http://dart.goodev.org/)

<a name="63554eb7"></a>

## 一、 安装

获取 `dart SDK`

```bash
// 稳定版
brew tap dart-lang/dart
brew install dart
```

```bash
// 最新版
brew install dart --devel
```

```bash
// 升级
brew upgrade dart
```

```bash
// 安装稳定版
brew unlink dart
brew install dart
```

```bash
// 安装最新版
brew upgrade --force dart -- --level
```

```bash
// 查看版本
brew info dart
```

<a name="4e1cfe64"></a>

## 二、 介绍

`main` 方法：程序入口

`print` 方法：可以在控制台输出内容

<a name="93d6d18f"></a>

## 三、 变量与常量

<a name="a15d0d16"></a>

### 3.1 变量

使用 `var` 声明变量，可赋予不同类型的值。

未初始化时，默认值为 `null`。

使用 `final` 关键字声明一个只能赋值一次的变量。

<a name="a2c425db"></a>

### 3.2 常量

使用 `const` 声明常量

使用 `const` 声明的必须是编译器的常量 `=&gt;` 在编译阶段就可以确定它的值

<a name="b6691530"></a>

## 四、 数据类型

<a name="23be9bf8"></a>

### 4.1 数值型：`Number` (简写：`num`)

- 子类型：

1. 整型 `Int`
2. 浮点型 `double`

- 数值型操作：

**运算符：** `+`、`-`、`*`、`/`、`～/`(**取整**)、`%`(**取余**)

**常用属性：** `isNaN` (**是否是非数字**)、`isEven` (**是否是偶数**)、`isOdd` (**是否是奇数**)

**常用方法：**

1. `abs()` 绝对值

2. `round()` 四舍五入

3. `floor()` 取不大于它的整数，向下取整

4. `ceil()` 取不小于于它的整数，向上取整

5. `toInt()` 浮点型转整形

6. `toDouble()` 整形转浮点型

**注意:** `0.0 /0.0 => NAN`

<a name="d0c6a326"></a>

### 4.2 字符串：`String`

- 字符串的使用

1. 使用单引号，双引号创建字符串
2. 使用三个引号或双引号创建多行字符串
3. 使用 `r` 创建原始 `raw` 字符串. `=&gt;`

```dart
String str = 'Hello \n World!';
print(str); // 输出换行形式的 Hello World

String str1 = r'Hello \n World!';
print(str1); // 输出 Hello \n World
```

- 字符串操作

**运算符:** `+`、`*`(**重复次数**)、`==`、`[]`(**取字符**)

**插值表达式：** `${expression}`

```dart
int a = 1;
int b = 2;
print('a + b = ${a + b}');
print('a=$a');
```

**常用属性**：`length`、`isEmpty`、`isNotEmpty`

**常用方法**：

1、`contains()`(是否包含)

2、`subString()`(截取一段字符串，参数1:开始位置；参数2:结束位置(不包括))

3、`startsWith()`(是否以一个字符串开头)

4、`endsWith()`(是否以一个字符串结尾)

5、`indexOf()`(是否包含一个字符，返回这个字符的下标)

6、`lastIndexOf()`(是否包含一个字符，倒序返回这个字符的下标)

7、`toLowerCase()`(转换小写)

8、`toUpperCase()`(转换大写)

9、`trim()`(截取空格)

10、`trimLeft()`(截取左边空格)

11、`trimRight()`(截取右边空格)

12、`split()`(分割字符串)

13、`replaceXXX()`(替换)

<a name="8ca0f9f4"></a>

### 4.3 布尔型：`Boolean`

<a name="7a6f4b82"></a>

### 4.4 列表：`List`

- `**List**`**(数组)的创建**

**创建 **`**List**`**：**

```dart
var list = [1, 2, 3];
```

**创建不可变的 List：**

```dart
var list = const [1, 2, 3];
```

**构造创建：**

```dart
var list = new List();
```

- **常用操作**

`[].length`

`add()`、`insert()` **用来添加元素**

`remove()`、`clear()` **用来删除元素，其中 **`**clear**`** 是清空整个 **`**list**`

`indexOf()`、`lastIndexOf` **获取 **`**list**`** 中元素的位置**

`sort()` **排序，可以传递参数，参数是要传递的方法，默认按照 **`**ASCII**`** 码来进行排序。**

`sublist()` **获取子 **`**list**`

`shuffle()` **打乱(随机打乱 )**

`asMap()`  **将 **`**list**`** 转换为 **`**map**`

`forEach()` **循环一个 **`**list**`**，括号中传递的是一个方法。** `list.forEach(print);`

`generate` (生成集合的长度, 迭代器回调函数)**集合的生成函数**

<a name="851523de"></a>

### 4.5 键值对：`Map`

- `Map` 的创建

> 创建 Map

```dart
var language = {'first': 'Dart', 'second': 'Java'};
```

> 创建不可变 Map

```dart
var language = const {'first': 'Dart', 'second': 'Java'};
```

> 构造创建

```dart
var language = new Map();
```

- `Map` 常用操作

1、`[].length`

2、`isEmpty()`、`isNotEmpty()` **是否为空**

3、`keys,values` **获取 map 所有的键和所有的值**

4、`containsKey()` **是否包含某个键**

5、`containsValue()` **是否包含某个值**

6、`remove()` **移除某个元素**

7、`forEach()` **循环，传入两个方法**

```dart
void main() {
  var map = {'first': 'Dart', 'Second': 'Java', 'Third': 'Python'};
  map.forEach(f);
}
void f(key, value) {
  print("key=$key, value=$value");
}
```

8、 `map()` `**map**`** 的遍历方式，接受一个回调函数作为参数，返回一个新 **`**map**`

```dart
void main() {
    Map age = {"zhangsan": 18, "lisi": 20};
    Map age2 = age.map(f);
    print(age2); // {18: "zhangsan", 20: "lisi"};
}
MapEntry f(k, v) {
    return MapEntry(v, k); // 调换 k, v 的值
}
```

<a name="24728d0d"></a>

### 4.6 `Runes, Symbols`

<a name="8dfe3165"></a>

### 4.7 `dynamic =&gt`; 动态类型

```dart
void main() {
  var list = new List<dynamic>();
  list.add(1);
  list.add("hello");
  list.add(true);
  print(list);
}
```

<a name="8e7deab9"></a>

### 4.8 `dynamic, var, Object` 的区别

*dynamic* 是动态类型，如果使用其声明一种类型，则仍旧可以为其声明另一种类型；通常不直接使用。

*var* 是关键字，如果使用其声明一种类型，不可为其声明另一种类型，但可改变同类型的值；

*Obejct* 是基类，用其声明的变量只能使用 ***Object*** 类所提供的方法

<a name="b47821f1"></a>

## 五、 运算符

<a name="014f3b72"></a>

### 5.1 算数运算符

- **加减乘除**: `+`，`-`，`*`，`/`，`～/`，`%`。
- **递增递减:** ·`++var`，`var++`，`--var`，`var--`。

<a name="7f1403d6"></a>

### 5.2 关系运算符

- **运算符:** `==`，`!=`，`>`，`<`，`>=`，`<=`
- **判断内容是否相同使用 **`**==**`

<a name="10766857"></a>

### 5.3 逻辑运算符

- **运算符：** `!`，`&&`，`||`
- **针对布尔类型运算**

<a name="34b8e337"></a>

### 5.4 赋值运算符

- **基础运算符：** `=`，`??=`**(如果左边的变量没有值，就将右侧的值赋给它；如果左边变量有值，右边的值无效。)**
- **复合运算符：**`+=`，`-=`，`*=`，`/=`，`%=`，`~/=`

<a name="b18b30d3"></a>

### 5.5 条件表达式

- **三目运算符：** `condition ? expr1 : expr2`
- `**??**`**运算符：** `expr1 ?? expr2`(**如果第一个表达式为空，则使用第二个表达式，否则直接使用第一个表达式的值.**)

<a name="bd3fc3e3"></a>

## 六、 控制流语句

<a name="22af5e7e"></a>

### 6.1 条件语句

- **if语句**
- **if...else if 语句**
- **if...else if...else 语句**

<a name="2d6cdeb7"></a>

### 6.2 循环语句

- **for循环**
- **for...in 循环**
- **while循环**
- **do...while 循环**
- **break 和 continue**
  - **break 用于终止循环**
  - **continue 用于跳出当前循环**
- **switch...case 语句**
  - **比较类型:** **num, String, 编译器常量, 对象, 枚举**
  - **非空 case 必须有一个 break**
  - **default 关键字来处理默认情况**
  - **continue 跳转标签**

<a name="1a05b0c4"></a>

## 七、 方法

<a name="11ad6452"></a>

### 7.1 方法定义

**方法定义：**

    返回类型 方法名 (参数1，参数2 ...) {
        方法体
        return 返回值
    }

**方法特性：**

`1、` 方法也是对象，并且有具体类型 `Function`。

`2、` 返回值类型，参数类型都可以省略。

`3、` 箭头语法：`=> expr` 是 `{return expr;}`缩写。只适用于 **一个表达式**。

`4、` 方法都有返回值。如果没有指定，默认 `return null` 最后一句执行。

<a name="a73dcf54"></a>

### 7.2 可选参数

- 可选命名参数：`{param1, param2, ...}`

```dart
void main() {
  printPerson("张三");// name=张三, age=null, gender=null
  printPerson("张三", age: 20); // name=张三, age=20, gender=null
  printPerson("张三", age: 20, gender: "Male"); // name=张三, age=20, gender="Male"
  printPerson("张三", gender: "Male"); // name=张三, age=null, gender="Male"
}

printPerson(String name, { int age, String gender }) {
  print('name=$name, age=$age, gender=$gender'); 
}
```

- 可选位置参数：`[param1, param2, ...]`

```dart
void main() {
  printPerson("李四");// name=李四, age=null, gender=null
  printPerson("李四", 18);// name=李四, age=18, gender=null
  printPerson("李四", 18, "男"); // name=李四, age=18, gender=男
}

printPerson(String name, [int age, String gender]) {
  print('name=$name, age=$age, gender=$gender');
}
```

- 如果存在具体参数，可选参数声明，必须在参数后面

<a name="3d374fc1"></a>

### 7.3 默认参数值

- 使用 `=` 在 **可选参数** 指定默认值

```dart
void main() {
  printPerson("张三"); // name=张三, age=30, gender=女
  printPerson("张三", age: 20); // name=张三, age=20, gender=女
  printPerson("张三", gender: "男"); // name=张三, age=30, gender=男
}

printPerson(String name, { int age = 30, String gender = "女" }) {
  print('name=$name, age=$age, gender=$gender');
}
```

- 默认值只能是编译时常量

<a name="da4cf90f"></a>

### 7.4 方法对象

- 方法可以作为对象赋值给其它变量

```dart
void main() {
  // var func = printHello;
  Function func = printHello;
  func();
}

void printHello() {
  print("Hello");
}
```

- 方法可作为参数传递给其它方法

```dart
void main() {
  var list = [1, 2, 3, 4];
  list.forEach(print);
}

// 或者
void main() {
  var list1 = ["h", "e", "l", "l", "o"];
  print(listTimes(list1, times));
}

List listTimes(List list, String f(str)) {
  for(var index = 0; index < list.length; index++) {
    list[index] = f(list[index]);
  }
  return list;
}

String times(str) {
  return str * 3;
}
```

<a name="588eca3a"></a>

### 7.5 匿名方法

如何定义：

```dart
(参数1，参数2，...) {
    方法体...
    return 返回值
}
```

匿名方法的特性：

- 可赋值给变量，通过变量进行调用

```dart
void main() {
  // 第一种
  var func = (str) {
    print("Hello---$str");
  };
  func(30);
  // 第二种
  (() {
      print("Test");
  })()
}
```

- 可在其它方法中直接调用或传递给其它方法

```dart
void main() {
  var list1 = ["h", "e", "l", "l", "o"];
  var result = listTimes(list1, (str) {return str * 3;});
  // var result = listTimes(list1, (str) => str * 3);
  print(result);
}

List listTimes(List list, f(str)) {
  for(var index = 0; index < list.length; index++) {
    list[index] = f(list[index]);
  }
  return list;
}
```

```dart
void main() {
  var list2 = ["h", "e", "l", "l", "o"];
  print(listTimes2(list2));
}
List listTimes2(List list) {
  var func = (str) { return str * 3; };
  for(var i = 0; i < list.length; i++) {
    list[i] = func(list[i]);
  }
  return list;
}
```

注意：匿名方法不能直接定义在外面

<a name="7a56f110"></a>

### 7.6 闭包

- 闭包是一个方法(对象)
- 闭包定义在其它方法内部
- 闭包能够访问外部方法内的局部变量，并持有其状态

```dart
void main() {
  var func = a();
  func();
  func();
  func();
  func();
}
a() {
  int count = 0;
  printCount() {
    print(count++);
  };
  return printCount;
}
```

```dart
// 使用匿名方法的闭包
void main() {
  var func = a();
  func();
  func();
  func();
  func();
}

a() {
  int count = 0;
  return () {
    print(count++);
  };
}
```

<a name="0e7e494d"></a>

## 八、 `Dart` 面向对象编程

<a name="842be82b"></a>

### 8.1 类于对象

1. 使用关键字 `class` 声明一个类
2. 使用关键字 `new` 创建一个对象，`new` 可省略
3. 所有对象都继承于 `Object` 类型

<a name="8cadb31c"></a>

### 8.2 属性和方法

1. 属性默认会生成 `getter` 和 `setter` 方法
2. 使用 `final` 声明的属性只有 `getter` 方法（只可读不可写）
3. 属性和方法是通过 `.` 访问
4. 方法不能被重栽

<a name="bb4c9d85"></a>

### 8.3 类及成员可见性

1. `Dart` 中的可见性是以 `library(库)` 为单位
2. 默认情况下，每一个 `Dart` 文件就是一个库
3. 使用 `_` 表示库的私有性

```dart
// person.dart
class _Person {
  String name;
  int age;
  // final String address;

  void work() {
    print("name is $name, age is $age");
  }
}

// class_and_object.dart
import 'person.dart';

void main() {
  // var person = new Person();
  var person = _Person(); // error
  person.name = "Tom";
  person.age = 20;
  print(person.name); 
  person.work();

  // print(person.address);
}
```

4. 使用 ***import*** 导入库

<a name="3d0dd0b5"></a>

### 8.4 计算属性

1. 顾名思义，计算属性的值是通过计算而来，本身不存储值
2. 计算属性赋值，其实是通过计算转换到其它实例变量

```dart
void main() {
  var rect = new Rectangle();
  rect.width = 10;
  rect.height = 20;
  print(rect.area); // 200;
  
  // 已知面积求宽度
  rect.area = 200;
  print(rect.width); // 10.0
}

class Rectangle {
  num width, height;
  // 获取计算属性的值
  num get area => width * height;
  // num get area {
  //   return width * height;
  // }
  // 设置计算属性值
  set area(value){
    width = value / 20;
  }
}
```

<a name="34a314fc"></a>

### 8.5 构造方法

1. 如果没有自定义构造方法，则会有个默认构造方法
2. 如果存在自定义构造方法，则默认构造方法无效

```dart
void main() {
  var person = new Person("Tom", 20);
}
class Person {
  String name;
  int age;

  // final String gender;
  Person(String name, int age) {
    this.name = name;
    this.age = age;
  }
  void work() {
    print("work...");
  }
}
```

**语法糖**：在构造方法执行之前对属性进行赋值

```dart
void main() {
  var person = new Person("Tom", 20, "Male");
}
class Person {
  String name;
  int age;

  final String gender;
  Person(this.name, this.age, this.gender); // 语法糖
  void work() {
    print("work...");
  }
}
```

3. 构造方法不能重载

<a name="28dec681"></a>

### 8.6 命名构造方法

1. 使用命名构造方法，可以实现多个构造方法
2. 使用 **类名.方法** 的形式实现

```dart
void main() {
  var person = new Person.width("Tom");
}
class Person {
  String name;
  Person.width(String name) {
    this.name = name;
  }
  void work() {
    print("work...");
  }
}
```

<a name="d6b162f0"></a>

### 8.7 常量构造方法

1. 如果类是不可变状态，可以把对象定义为编译时常量
2. 使用 `const` 声明构造方法，并且所有变量都为 `fianl`
3. 使用 `const` 声明对象，可以省略

```dart
void main() {
  // const person = const Person("张三", 20, "Male");
  const person = Person("张三", 20, "Male");
}

class Person {
  final String name;
  final int age;
  final String gender;
  const Person(this.name, this.age, this.gender);
  void work() {
    print("work");
  }
}
```

<a name="1b266566"></a>

### 8.8 工厂构造方法

1. 工厂构造方法类似于设计模式中的工厂模式
2. 在构造方法前添加关键字 `factory` 实现一个工厂构造方法
3. 在工厂构造方法中可返回对象

```dart
class Logger {
  final String name;
  static final Map<String, Logger> _cache = <String, Logger> {};

  factory Logger(String name) {
    return Logger._internal('Dart'); // 可以返回
  }
  Logger._internal(this.name);
  void log(String msg) {
    print(msg);
  }
}
```

- ***命名工厂构造方法***（***factory 类名.方法名***）

它可以有返回值，而且不需要将类的 ***final*** 变量作为参数，是提供一种灵活获取类对象的方式

```dart
class Student {
  factory Student._stu(Student stu) {
    return Student(stu._school, stu.name, stu.age);
  }
}
```

<a name="2da81770"></a>

### 8.9 初始化列表常用于设置 (`fianl` 变量的值)

1. 初始化列表会在构造方法体执行之前执行
2. 使用逗号分隔初始化表达式
3. 初始化列表常用于设置 `final` 变量的值

```dart
void main() {
  var person = new Person("Tom", 20, "Male");
}
class Person {
  String name;
  int age;
  final String gender;

  Person(this.name, this.age, this.gender);
  // 初始化列表
  Person.withMap(Map map): gender = map["gender"] {
    this.name = map["name"];
    this.age = map["age"];
  }
  // 或者
  Person.withMap1(Map map): name = map["name"], age = map["age"], gender = map["gender"];
  void work() {
    print("work");
  }
}
```

<a name="0ce482f2"></a>

### 8.10 静态成员

1. 使用 `static` 关键字来实现类级别的变量和函数（不再属于对象级别）
2. 静态成员不能访问非静态成员，非静态成员可以访问静态成员
3. 类中的常量需要使用 `static const` 声明

```dart
void main() {
  var page = new Page();
  // page.scrollDown();
  Page.scrollDown();
}
class Page {
  // 添加常量
  static const int maxAge = 10;
  
  static int currentPage = 1;
  static void scrollDown() {
    currentPage = 1;
    print("scrollDown");
  }
  void scrollUp() {
    currentPage++;
    print("scrollUp");
  }
}
```

<a name="539800c4"></a>

### 8.11 对象操作符

1. 条件成员访问：`?.`（如果该操作符前边不为空，则继续向后执行，否则不继续执行）
2. 类型转换：`as`

```dart
void main() {
  var person;
  person = "";
  person = new Person();
  (person as Person).work();
}
class Person {
  String name;
  int age;
  void work() {
    print("work..."); 
  }
}
```

3. 是否指定类型：`is`，`is!`

```dart
void main() {
  var person;
  person = "";
  person = new Person();
  if(person is Person) {
    person.work();
  }
}
class Person {
  String name;
  int age;
  void work() {
    print("work..."); 
  }
}
```

4. 级联操作：`..`

```dart
void main() {
  var person = new Person();
  person..name = "Tom" ..age = 20 ..work();
  // 等价于
  person.name = "Tom";
  person.age = 20;
  person.work();
}
class Person {
  String name;
  int age;
  void work() {
    print("work..."); 
  }
}
```

<a name="a3818357"></a>

### 8.11 对象 `call` 方法

- 如果类实现了 `call()` 方法，则该类的对象可以作为方法使用

```dart
void main() {
  var person = new Person();
  // person("张三", 30);
  print(person("张三", 30));
}
class Person {
  String name;
  int age;
  // void call(String name, int age) {
  //   print("Name is $name, Age is $age");
  // }
  String call(String name, int age) {
    return "Name is $name, Age is $age";
  }
}
```

<a name="a58aed29"></a>

## 九、面向对象扩展

- 继承，继承中的构造方法
- 抽象类
- 接口
- `Mixins`，操作符的覆写（操作符/运算符重载）

<a name="cd14c6c5"></a>

### 9.1 继承

1. 使用关键字 `extends` 继承一个类
2. 子类会继承父类可见的属性和方法（私有属性无法继承），不会继承构造方法
3. 子类能够复写父类的方法、`getter` 和 `setter`
4. 单继承，多态性（例如可以重写 `toString` 方法）

```dart
// person.dart
class Person {
  String name;
  int age;
  String _birthday;
  bool get isAudit => age > 10;
  void run() {
    print("Person run");
  }
}
```

- [***@override ***](/override)*** *** 表示下面的计算属性或方法是从父类中复写过来的，并不是自己的
- `**super.run();**`  `_**super**_` 表示在子类中调用父类的方法，相当于 `_**this**_`

```dart
// student.dart
import "person.dart";

void main() {
  var student = new Student();
  student.study();
  student.name = "Tom";
  student.age = 16;
  print(student.isAudit);
  student.run();
}
class Student extends Person {
  void study() {
    print("Student study..."); 
  }
  // @override 表示下面的计算属性或方法是从父类中复写过来的，并不是自己的
  @override
  bool get isAudit => age > 15;

  @override
  void run() {
    // super.run(); // super表示在子类中调用父类的方法 
    print("Student run...");
  }
}
```

- 多态

```dart
// 子类的实例可以赋值给父类的一个应用
void main() {
    Person person = new Student();
    if(person is Student) {
      person.study();
    }
}
```

<a name="0b9e8a5f"></a>

### 9.2 继承中的构造方法

- 继承中的构造方法

1. 子类中的构造方法默认会调用父类的无名无参的构造方法
2. 如果父类没有无名无参的构造方法，则需要显示调用父类构造方法
3. 在构造方法参数后使用 `:` 显示调用父类构造方法

```dart
void main() {
  var student = new Student("Tom");
  print(student.name);
}
class Person {
  String name;
  Person(this.name);
  Person.withName(this.name);
}
class Student extends Person {
  int age;

//  Student(String name) : super(name);
  Student(String name) : super.withName(name);
}
```

- 构造方法的执行顺序

1. 父类的构造方法在子类构造方法体开始执行的位置调用
2. 如果有初始化列表，初始化列表会在父类构造方法之前执行

```dart
void main() {
  var student = new Student("Tom", "Male");
  print(student.name);
}
class Person {
  String name;
  Person(this.name);
  Person.withName(this.name);
}
class Student extends Person {
  int age;
  final String gender;
//  Student(String name) : super(name);
  // 初始化列表必须放在显示调用父类构造方法的前面
  Student(String name, String genderName) : gender = genderName, super.withName(name);
}
```

<a name="7fc58712"></a>

### 9.3 抽象类

1. 抽象类使用关键字 `abstract` 表示，不能直接被实例化
2. 抽象方法不用 `abstract` 修饰，无实现
3. 抽象类可以没有抽象方法
4. 有抽象方法的类一定得声明为抽象类

抽象类更多用来作为接口使用

```dart
void main() {
  var person = new Student();
  person.run();
}
abstract class Person {
  void run();
}
class Student extends Person {
  @override
  void run() {
    print("run...");
  }
}
```

<a name="08f04307"></a>

### 9.4 接口

1. 在 `dart` 中，类和接口是统一的，类就是接口
2. 每个类都隐式的定义了一个包含所有实例成员的接口
3. 如果是复用已有类的实现，使用继承（`extends`）
4. 如果只是使用已有类的外在行为(一些行为)，则使用接口（`implements`）

```dart
void main() {
  var student = new Student();
}
class Person {
  String name;
  int get age => 18;
  void run() {
    print("Person run...");
  }
}
class Student implements Person {
  @override
  String name;

  @override
  // TODO: implement age
  int get age => 15;

  @override
  void run() {
    // TODO: implement run
  }
}
```

- 更好的写法，利用抽象类

```dart
void main() {
  var student = new Student();
  student.run();
}
abstract class Person {
  void run();
}
class Student implements Person {
  @override
  void run() {
    print("Student run...");
  }
}
```

<a name="cc3f95f0"></a>

### 9.5 `Mixins`

1. `Mixins` 类似于多继承，是在多类继承中重用一个类代码的方式

```dart
void main() {
  var d = new D();
  d.a();
  d.b();
  d.c();
}

class A {
  void a() {
    print("A.a()...");
  }
}
class B {
  void a() {
    print("B.a()...");
  }
  void b() {
    print("B.b()...");
  }
}
class C {
  void a() {
    print("C.a()...");
  }
  void b() {
    print("C.b()...");
  }
  void c() {
    print("C.c()...");
  }
}
// 必须先有继承，才能使用 Mixins；如果使用 Mixins 的几个类中有相同的方法，则处于最后一个的方法优先被调用
class D extends A with B, C {

}
```

2. 作为 `Mixin` 的类不能有显示声明构造方法
3. 作为 `Mixin` 的类只能继承自 `Object`
4. 使用关键字 `with` 连接一个或多个 `Mixin`

如果是由其它类组装而来的，没有自己的属性或方法，则可以简写

```dart
void main() {

}
abstract class Engine {
  void work();
}
class OilEngine implements Engine {
  @override
  void work() {
    print("Work with qil...");
  }
}
class ElectricEngine implements Engine {
  @override
  void work() {
    print("Work with electric...");
  }
}
class Tyre {
  String name;
  void run() {}
}
// 如果是由其它类组装而来的，没有自己的属性或方法，则可以简写
class Car = Tyre with ElectricEngine;
// 完整写法
//class Car extends Tyre with ElectricEngine {}
class Bus = Tyre with OilEngine;
```

<a name="13db85ad"></a>

### 9.6 操作符覆写(重载运算符/运算符重载)

1. 覆写操作符需要在类中定义

```dart
返回类型 operator 操作符 (参数1, 参数2, ...) {
    实现体...
    return 返回值
}
```

2. 如果覆写 `==` ，还需要覆写对象的 `hashCode` `getter` 方法
3. 可覆写的操作符
   | ***<*** | ***+*** | ***|*** | ***\[]*** |
   | --- | --- | --- | --- |
   | ***>*** | ***/*** | ***^*** | ***\[]=*** |
   | ***<=*** | ***~/*** | ***&*** | ***~*** |
   | ***>=*** | ******* | ***<<*** | ***==*** |
   | ***-*** | ***%*** | >> |  |

```dart
void main() {
  var person1 = new Person(18);
  var person2 = new Person(20);
  var person3 = new Person(20);
  print(person1 > person2);
  print(person1["age"]);
  print(person2 == person3);
}

class Person {
  int age;
  Person(this.age);

  // 覆写 >
  bool operator >(Person person) {
    return this.age > person.age;
  }
  // 覆写 []
  int operator [](String str) {
    if("age" == str) {
      return age;
    }
    return 0;
  }
  // 覆写等号，重写hashCode，右键打开generate选项然后选择
  @override
  bool operator ==(Object other) =>
      identical(this, other) ||
          other is Person &&
              runtimeType == other.runtimeType &&
              age == other.age;

  @override
  int get hashCode => age.hashCode;
}
```

<a name="65d308b2"></a>

## 十、枚举

- 枚举

1. 枚举是一种有穷序列集的数据类型
2. 使用关键字 `enum` 定义一个枚举
3. 常用于代替常量，控制语句等

```dart
void main() {
  var currentSeason = Season.spring;
  print(currentSeason.index);
  switch(currentSeason) {
    case Season.spring:
      print("1-3月");
      break;
    case Season.summer:
      print("4-6月");
      break;
    case Season.autumn:
      print("7-9月");
      break;
    case Season.winter:
      print("10-12月");
      break;
  }
}
enum Season {
  spring,
  summer,
  autumn,
  winter
}
```

- `Dart` 枚举特性

1. `index` 从 `0` 开始，依次累加
2. 不能指定原始值
3. 不能添加方法

<a name="8f5e2369"></a>

## 十一、泛型

- 泛型

1. `Dart` 中的类型是可选的，可使用泛型限定类型
2. 使用泛型能够有效的减少代码重复

- 泛型的使用

1. 类的泛型

```dart
void main() {
  var utils = new Utils<String>();
  utils.put("element");
}
class Utils<T> {
  T element;
  void put(T element) {
    this.element = element;
  }
}
```

2. 方法的泛型

```dart
void main() {
  var utils = new Utils();
  utils.put<String>("element");
}
class Utils {
  void put<T>(T element) {
    print(element);
  }
}
```

<a name="87ad53d5"></a>

## 十二、库

- 常用库

1. `Dart web` 应用通常使用 `dart:html` 库
2. `dart:core` 库定义了 `num`, `int`, 和 `double` 类，这些类 定义一些操作数字的基础功能。
3. 异步编程通常使用回调函数，但是 `Dart` 提供了另外的 选择： [***Future***](https://api.dartlang.org/stable/dart-async/Future-class.html) 和 [***Stream***](https://api.dartlang.org/stable/dart-async/Stream-class.html) 对象。 `Future` 和 `JavaScript` 中的 `Promise` 类似，代表在将来某个时刻会返回一个 结果。`Stream` 是一种用来获取一些列数据的方式，例如 `事件流`。 `Future`, `Stream`, 以及其他异步操作的类在 [***dart:async***](https://api.dartlang.org/stable/dart-async/dart-async-library.html) 库中。
4. `Math` 库提供了常见的数学运算功能，例如 `sine` 和 `cosine`， `最大值`、`最小值等`，还有各种常量 例如 `pi` 和 `e` 等。`Math` 库中 的大部分函数都是顶级方法。导入 `dart:math` 就可以使用 `Math` 库了

- 如果导入的两个库具有冲突的标识符，则可以使用库的前缀来区分

```dart
import 'package:lib1/lib1.dart';
import 'package:lib2/lib2.dart' as lib2;
// ...
Element element1 = new Element();           // Uses Element from lib1.
lib2.Element element2 = new lib2.Element(); // Uses Element from lib2.
```

- 导入库的一部分

如果你只使用库的一部分功能，则可以选择需要导入的 内容。例如：

```dart
// Import only foo.
import 'package:lib1/lib1.dart' show foo;

// Import all names EXCEPT foo.
import 'package:lib2/lib2.dart' hide foo;
```

- 延迟载入库

`Deferred loading` (也称之为 `lazy loading`) 可以让应用在需要的时候再 加载库。 下面是一些使用延迟加载库的场景：

- 减少 `APP` 的启动时间。
- 执行 `A/B` 测试，例如 尝试各种算法的 不同实现。
- 加载很少使用的功能，例如可选的屏幕和对话框。

要延迟加载一个库，需要先使用 `deferred as` 来 导入：

```dart
import 'package:deferred/hello.dart' deferred as hello;
```

当需要使用的时候，使用库标识符调用 `loadLibrary()` 函数来加载库：

```dart
greet() async {
  await hello.loadLibrary();
  hello.printGreeting();
}
```

在前面的代码， 使用 `await` 关键字暂停代码执行一直到库加载完成。 关于 `async` 和 `await` 的更多信息请参考 [***异步支持***](http://dart.goodev.org/guides/language/language-tour#asynchrony-support)。

在一个库上你可以多次调用 `loadLibrary()` 函数。 但是该库只是载入一次。

使用延迟加载库的时候，请注意一下问题：

- 延迟加载库的常量在导入的时候是不可用的。 只有当库加载完毕的时候，库中常量才可以使用。
- 在导入文件的时候无法使用延迟库中的类型。 如果你需要使用类型，则考虑把接口类型移动到另外一个库中， 让两个库都分别导入这个接口库。
- ***Dart*** 隐含的把 `loadLibrary()` 函数导入到使用 `deferred as` 的命名空间中。 `loadLibrary()` 方法返回一个 [***Future***](http://dart.goodev.org/guides/libraries/library-tour#future)。

<a name="be7a8956"></a>

## 十三、异步支持

`Dart` 库中有很多返回 `Future` 或者 `Stream` 对象的方法。 这些方法是 *异步的*： 这些函数在设置完基本的操作 后就返回了， 而无需等待操作执行完成。 例如读取一个文件，在打开文件后就返回了。

有两种方式可以使用 `Future` 对象中的 数据：

- 使用 `async` 和 `await`
- 使用 [***Future API***](http://dart.goodev.org/guides/libraries/library-tour#future)

同样，从 `Stream` 中获取数据也有两种方式：

- 使用 `async` 和一个 异步 `for` 循环 (`await for`)
- 使用 [***Stream API***](http://dart.goodev.org/guides/libraries/library-tour#stream)

**1.** 要使用 `await`，其方法必须带有 `async` 关键字：

```dart
checkVersion() async {
  var version = await lookUpVersion();
  if (version == expectedVersion) {
    // Do something.
  } else {
    // Do something else.
  }
}
```

**2.** 可以使用 `try`, `catch`, 和 `finally` 来处理使用 `await` 的异常：

```dart
try {
  server = await HttpServer.bind(InternetAddress.LOOPBACK_IP_V4, 4044);
} catch (e) {
  // React to inability to bind to the port...
}
```

<a name="a1db95b8"></a>

### 13.1 声明异步方法

一个 `async` 方法是函数体被标记为 `async` 的方法。 虽然异步方法的执行可能需要一定时间，但是 异步方法立刻返回 - 在方法体还没执行之前就返回了。

```dart
checkVersion() async {
  // ...
}

lookUpVersion() async => /* ... */;
```

在一个方法上添加 `async` 关键字，则这个方法返回值为 `Future`。 例如，下面是一个返回字符串 的同步方法：

```dart
String lookUpVersionSync() => '1.0.0';
```

如果使用 `async` 关键字，则该方法 返回一个 `Future`，并且 认为该函数是一个耗时的操作。

```dart
Future<String> lookUpVersion() async => '1.0.0';
```

有时候，你的算法要求调用很多异步方法，并且等待 所有方法完成后再继续执行。使用 `[Future.wait()](https://api.dartlang.org/stable/dart-async/Future/wait.html)` 这个静态函数来管理多个 `Future` 并等待所有 `Future` 执行完成。

```dart
Future deleteDone = deleteLotsOfFiles();
Future copyDone = copyLotsOfFiles();
Future checksumDone = checksumLotsOfOtherFiles();

Future.wait([deleteDone, copyDone, checksumDone])
    .then((List values) {
      print('Done with all the long steps');
    });
```

<a name="172c3272"></a>

### 13.2 使用 `await` 表达式

在一个异步方法内可以使用多次 `await` 表达式。 例如，下面的示例使用了三次 `await` 表达式 来执行相关的功能：

```dart
var entrypoint = await findEntrypoint();
var exitCode = await runExecutable(entrypoint, args);
await flushThenExit(exitCode);
```

在 `await expression` 中， `expression` 的返回值通常是一个 `Future`； 如果返回的值不是 `Future`，则 `Dart` 会自动把该值放到 `Future` 中返回。 `Future` 对象代表返回一个对象的承 (`promise`)。 `await expression` 执行的结果为这个返回的对象。 `await expression` 会阻塞住，直到需要的对象返回为止。

**如果 **`**await**`** 无法正常使用，请确保是在一个 **`**async**`** 方法中。** 例如要在 `main()` 方法中使用 `await`， 则 `main()` 方法的函数体必须标记为 `async`：

```dart
main() async {
  checkVersion();
  print('In main: version is ${await lookUpVersion()}');
}
```

<a name="3f70affd"></a>

### 13.3 在循环中使用异步

异步 `for` 循环具有如下的形式：

```dart
await for (variable declaration in expression) {
  // Executes each time the stream emits a value.
}
```

上面 `expression` 返回的值必须是 `Stream` 类型的。 执行流程如下：

1. 等待直到 ***stream*** 返回一个数据
2. 使用 `stream` 返回的参数 执行 `for` 循环代码，
3. 重复执行 `1` 和 `2` 直到 `stream` 数据返回完毕。

使用 `break` 或者 `return` 语句可以 停止接收 `stream` 的数据， 这样就跳出了 `for` 循环并且 从 `stream` 上取消注册了。

**如果异步 **`**for**`** 循环不能正常工作， 确保是在一个 **`**async**`** 方法中使用。** 例如，要想在 `main()` 方法中使用异步 `for` 循环，则需要把 `main()` 方法的函数体标记为 `async`：

```dart
main() async {
  ...
  await for (var request in requestServer) {
    handleRequest(request);
  }
  ...
}
```
