---
title: weakMap 和 weakSet
url: https://www.yuque.com/wcywxq/mxunh7/gc9gp1
---

<a name="Wg2NB"></a>

## weakMap

- 只接受对象作为键名(null 除外)，不接受其他类型的值作为键名
- 键名是弱引用，键值是正常引用。所以键名的弱引用不会被计入垃圾回收机制，键值正常进行垃圾回收
- 无法进行遍历 <a name="e8gH8"></a>

## weakSet

- 成员只能是对象
- weakSet 中的对象都是弱引用，不会被计入垃圾回收机制。因此，若其他对象都不再引用该对象，那么会自动回收该对象所占用的内存。不容易造成内存泄漏
- 无法进行遍历
