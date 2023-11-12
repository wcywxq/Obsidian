---
title: for...in 和 for...of 的区别
url: https://www.yuque.com/wcywxq/mxunh7/bqhdyg
---

<a name="iyDXd"></a>

## for ... in

1. 一般用于遍历对象的可枚举属性，以及对象从构造函数原型中继承的属性，对于每个不同的属性，语句都会被执行
2. 不适用于对数组的遍历，因为输出的顺序是不固定的
3. 如果迭代的对象的变量值是 null 或 undefined，for ... in 将不会执行循环体，建议在使用 for ... in 循环之前，先检查该对象的值是不是 null 或 undefined <a name="tOxKm"></a>

## for ... of

1. for ... of 是 ES6 新增的遍历方式，允许遍历一个含有 Iterator 接口的数据结构，并且返回各项的值
2. for ... of 语句在可迭代对象(包括：Array、Map、Set、String、TypedArray、arguments 等)上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句 <a name="vgJO4"></a>

## 异同

1. for ... in 适合遍历对象；for ... of 用于遍历数组
2. for ... in 获取的是对象的键名；for ... of 遍历获取的是键值
3. for ... in 会遍历对象的整个原型链，新能非常差不推荐使用；for ... of 只会遍历当前可迭代对象，不会遍历当前可迭代对象的原型链
4. 对于数组的遍历，for ... in 会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，for ... of 只返回数组下标对应的属性值
