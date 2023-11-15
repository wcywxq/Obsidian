---
title: 简易版 vue2
url: https://www.yuque.com/wcywxq/pgkpvh/indxog
---

<a name="QZZzJ"></a>

## vue.js

> 入口

```javascript
import Observer from './observer.js'
import Compiler from './compiler.js'

/**
 * 包括 vue 构造函数，接收各种配置参数等
 */
export default class Vue {
	constructor(options = {}) {
  	this.$options = options
    this.$data = options.data
    this.$methods = options.methods
    
    // 获取根元素，并存储到 vue 实例
    this.initRootElement(options)
    // 利用 Object.defineProperty 将 data 的属性注入到 vue 实例
    this._proxyData(this.$data)
    
    // 实例化 Observer 对象，监听数据变化
    new Observer(this.$data)
    
    // 实例化 Compiler 对象，解析指令和模版表达式
    new Compiler(this)
  }
  
  // 获取根元素，并存储到 vue 实例，简单检查以下传入的 el 是否合规
  initRootElement(options) {
  	if (typeof options.el === 'string') {
    	this.$el = document.querySelector(options.el)
    } else if (options.el instanceof HTMLElement) {
    	this.$el = options.el
    }
    
    if (!this.$el) {
    	throw new Error('传入的 el 不合法，请传入 css 选择器或 HTMLELement')
    }
  }
  
  // 利用 object.definedProperty 将 data 的属性注入到 vue 实例中
  _proxyData(data) {
  	Object.keys(data).forEach(key => {
    	Object.defineProperty(this, key, {
      	enumerable: true.
        configurable: true,
        get() {
      		return data[key]
      	},
        set(newVal) {
        	if (data[key] === newVal) return
          data[key] = newVal
        }
      })
    })
  }
}
```

<a name="Rr6Bw"></a>

## complier.js

> 解析模版和指令

```javascript
import Watcher from './watcher.js'

export default class Compiler {
	constructor(vm) {
  	this.el = vm.$el
    this.vm = vm
    this.methods = vm.$methods
    
    // 编译模版
    this.compile(vm.$el)
  }
  
  // 编译模版
  compile(el) {
  	const childNodes = el.childNodes
    Array.from(childNodes).forEach(node => {
    	if (this.isTextNode(node)) {
      	// 编译文本节点
        this.compileText(node)
      } else if (this.isElementNode(node)) {
      	// 元素节点
        this.compileElement(node)
      }
      // 如果存在子节点，则递归调用
      if (node.childNodes && node.childNodes.length) {
      	this.compile(node)
      }
    })
  }
  
  // 编译文本节点
  compileText(node) {
  	const reg = /\{\{(.*?)\}\}/g
    const value = node.textContent
    
    if (reg.test(value)) {
      const key = RegExp.$1.trim() // 获取变量名
      node.textContent = value.replace(reg, this.vm[key])
      
      // 响应式更新
      new Watcher(this.vm, key, newVal => {
      	node.textContent = newVal
      })
    }
  }
  
  // 编译元素节点
  compileElement(node) {
  	if (node.attributes.length) {
    	Array.from(node.attributes).forEach(attr => {
      	// 遍历元素节点的所有属性
        const attrName = attr.name 
        
        // v-model v-html v-on:click
        if (this.isDirective(attrName)) {
        	let directiveName = attrName.indexOf(':') > 1
          	? attrName.subStr(5)
          	: attrName.subStr(2)
          let key = attr.value
          // 更新元素节点
          this.update(node, key, directiveName)
        }
      })
    }
  }
  
  // 更新元素节点
  update(node, key, directiveName) {
  	const fn = this[`${directiveName}Updater`]
    fn && fn.call(this, node, this.vm[key], key, directiveName)
  }
  
  // 解析 v-text
  textUpdater(node, value, key) {
  	node.textContent = value
    new Watcher(this.vm, key, newVal => {
    	node.value = newVal
    })
  }
  
  // 解析 v-model
  modelUpdater(node, value, key) {
  	node.value = value
    new Watcher(this.vm, key, newVal => {
    	node.value = newVal
    })
    // 更新值，双向绑定
    node.addEventListener('input', () => {
    	this.vm[key] = node.value
    })
  }
  
  // 解析 v-html
  htmlUpdater(node, value, key) {
  	node.innerHTML = value
    new Watcher(this.vm, key, newVal => {
    	node.innerHTML = newVal
    })
  }
  
  // 解析 v-on
  clickUpdater(node, value, key, directiveName) {
  	node.addEventListener(directiveName, this.methods[key])
  }
  
  // 判断元素属性是否是指令
  isDirective(attrName) {
  	return attrName.startsWith('v-')
  }
  
  // 判断是否为文本节点
  isTextNode(node) {
  	return node.nodeType === 3
  }
  
  // 判断是否为元素节点
  isElementNode(node) {
  	return node.nodeType === 1
  }
}
```

<a name="glz6o"></a>

## dep.js

> 用于收集当前响应式对象的依赖关系，每个响应式对象都有一个 dep 实例

```javascript
export default class Dep {
	constructor() {
  	// 存储所有观察者
    this.watchers = []
  }
  
  // 添加观察者
  addWatcher(watcher) {
  	if (watcher && watcher.update) {
    	this.watchers.push(watcher)
    }
  }
  
  // 发送通知
  notify() {
  	this.watchers.forEach(watcher => {
    	watcher.update()
    })
  }
}
```

<a name="YoNpk"></a>

## observer.js

> 数据劫持实现，递归遍历所有属性

```javascript
import Dep from './dep.js'

export default class Observer {
	constructor(data) {
  	this.traverse(data)
  }
  
  // 递归遍历 data 里的所有属性
  traverse(data) {
  	if (!data || typeof data !== 'object') return
    Object.keys(data).forEach(key => {
    	this.defineReactive(data, key, data[key])
    })
  }
  
  // 数据劫持, 给传入的数据设置 getter / setter
  defineReactive(obj, key, val) {
  	this.traverse(val)
    
    const _self = this
    // 这里实例化，为了在 setter 中调用 dep.notify()
    const dep = new Dep()
    
    Object.defineProperty(obj, key, {
    	enumerable: true,
      configurable: true,
      get() {
      	// 获取的时候做依赖收集
        // Dep.target 是为了在此处添加依赖收集用的，添加之后就可以删除了，所以为 null
        Dep.target && dep.addWatcher(Dep.target)
        return val
      },
      set(newVal) {
      	if (val === newVal) return
        val = newVal
        // 设置的时候可能设置了一个对象，因此需要递归
        this.traverse(newVal)
        // 发起更新通知
        dep.notify()
      }
    })
  }
}
```

<a name="vGEKS"></a>

## watcher.js

> 负责依赖收集，收到的是一个个 Dep 类的实例化对象，需要有 update 方法

```javascript
import Dep from './dep.js'

export default class Watcher {
  /**
   * @params vm Vue 实例
   * @params key data 属性名
   * @params cb 负责更新视图的回调
   */
	constructor(vm, key, cb) {
  	this.vm = vm
    this.key = key
    this.cb = cb
    
    // 同一时间只维持一个 watcher
    Dep.target = this
    
    // 触发 get 方法，在 get 方法里去做一些操作
    this.oldVal = vm[key]
    
    // 为了避免重复添加 watcher，将其设置为 null
    Dep.target = null
  }
  
  // 当数据变化时，更新视图
  update() {
  	// 需要判断新旧两个值的关系
    let newVal = this.vm[this.key]
    if (this.oldVal === newVal) return
    this.cb(newVal)
  }
}
```
