---
title: new 和 Object.create
url: https://www.yuque.com/wcywxq/mxunh7/gyo93k
---

<a name="vUwby"></a>

## new 关键字

在使用 new 关键字创建对象的时候，new 关键字会帮助我们做以下四件事情：

1. 帮助我们创建一个空对象
2. 将新对象的原型 prototype 指向构造函数的原型 prototype
3. 执行构造函数, 改变 this，把构造函数的属性添加到新对象
4. 返回创建的新对象
   :::warning
   不能 new 一个箭头函数，因为箭头函数没有 this
   :::

```javascript
function newFactory(fn, ...args) {
  	// method1
  	const obj = new Object()
    obj.__proto__ = newFactory.prototype
  	newFactory.apply(obj, args)
    return obj
  
  	// method2
    const obj = Object.create(fn.prototype)
    fn.apply(obj, args)
  	return obj
}

// 测试用例
function Person(name, age) {
    this.name = name
    this.age = age
}

Person.prototype.say = function() {
    return this.name + ':' + this.age
}

const person = newFactory(Person, 'zhangsan', 20)

console.log(person, person.say())
```

<a name="twvte"></a>

## Object.create

定义：Object.create 方法创建一个新对象，使用现有的对象来提供新创建的对象的对象 proto
参数：Object.create(proto, propertiesObject)

- proto：传递一个现在有的对象，即新对象的原型对象(新创建的对象 **proto** 属性指向现有属性)。第一个参数 proto 的值为 null，那么创建出来的对象是一个 {}(空对象)并且没有原型
- propertiesObject：可选，给新对象添加新属性以及描述器。如果没有指定则创建一个 {}，有原型也有继承 Object.prototype 上的方法 <a name="m5UXH"></a>

## new Object 和 Object.create 区别

- new Object 继承内置对象 Object，而 Object.create 则是继承指定对象
- 可以通过 Object.create(null) 创建一个干净的对象，也就是没有原型，而 new Object() 创建的对象是 Object 的实例，原型永远指向 Object.prototype
