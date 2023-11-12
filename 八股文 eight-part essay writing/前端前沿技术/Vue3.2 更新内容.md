---
title: Vue3.2 更新内容
url: https://www.yuque.com/wcywxq/mxunh7/ia89dn
---

- SSR：服务端渲染优化。@vue/server-renderer 包加了一个ES模块创建，与 Node.js 解耦，使在非 Node 环境用 @vue/serve-render做服务端渲染成为可能，比如(Workers、Service Workers)
- New SFC Features：新的单文件组件特性
- Web Components：自定义 web 组件。这个我们平时很少用到，但是应该知道
- Effect Scope API： effect 作用域，用来直接控制响应式副作用的释放时间(computed 和 watchers)。这是底层库的更新，开发不用关心，但是应该知道
- Performance Improvements：性能提升。这是内部的提升，跟开发无关 <a name="yTINw"></a>

## 新特性和 api

<a name="RTMSh"></a>

### 新的 sfc 单文件组件的特性

可以在 style 标签里使用 v-bind，如下：

```vue
<template>
	<div>{{ color }}</div>
	<button @click='color = color === "red" ? "green" : "red"'>按钮</button>
</template>
<script setup>
	import { ref } from 'vue'
  const color = ref('ref')
</script>
<style scoped>
  div {
  	color: v-bind(color)
  }
</style>
```

<a name="pzSNV"></a>

### 部分 api 改变

<a name="ZsUCP"></a>

### 废弃 useContext

```javascript
import { useContext } from 'vue'
const { expose, slots, emit, attrs } = useContext()
```

<a name="H2Rjg"></a>

### 新增 useAttrs、useSlots、defineExpose、defineEmits

```javascript
import { useAttrs, useSlots } from 'vue'
const attrs = useAttrs()
const slots = useSlots()

defineExpose({
	name: 'zhangsan',
  someMethod() {
  	console.log('这是子组件的方法')
  }
})

defineEmits(['getName', 'myClick'])
```

<a name="AreWG"></a>

### defineProps 变更

```javascript
defineProps(['name'])
// 或者
defineProps({
	name: String
})
// 或者
defineProps({
	name: {
  	type: String,
    default: ''
  }
})
// 或者
const props = defineProps({
	... // 和上面相同
})
console.log(props.name)
```

<a name="CwX4K"></a>

### 顶级 await

不需要再用 async 就可以直接使用 await，这样默认会把组件的 setup 变成 async setup，像下面这样

```vue
<script setup lang='ts'>
	const post = await fetch('/api/post/xxx')
  	.then(res => res.json())
</script>
```

最终会转换为下面这样

```vue
<script lang='ts'>
	import { defineComponent, withAsyncContext } from 'vue'
  export default defineComponent({
  	async setup() {
    	const post = await withAsyncContext(
      	fetch('/api/post/xxx').then(res => res.json())
      )
      return { post } 
    }
  })
</script>
```

<a name="Sz9Bm"></a>

### 新增 withDefaults

在 ts 中，像下面这样定义 props 是不能设置默认值的

```typescript
interface Props {
	name: string
  age: number
}
defineProps<Props>()
```

加入 withDefaults 之后就可以指定默认值，像下面这样

```typescript
import { withDefaults } from 'vue'
interface Props {
	name: string
  age: number
}
const props = withDefaults(defineProps<Props>(), {
	name: 'zhangsan',
  age: 20
})
```

<a name="J1mLi"></a>

### 自定义 web 组件

通过 defineCustomElement 方法创建原生自定义组件。也可以通过这种方式发布组件出去供第三方使用

```javascript
// main.js
import { defineCustomElement } from 'vue'
const MyCustomElement = defineCustomElement({
	// 通过 vue 组件选项
  props: ['foo'],
  render() {
  	return h('div', `my-custom-element: ${this.foo}`)
  },
  // 仅适用于 defineCustomElement，css 将被注入到 shadow root
  style: [`div { border: 1px solid red }`]
})

customElements.define('my-custom-element', MyCustomElement)

// vite.config.js
export default defineConfig({
	plugins: [
  	vue({
    	template: {
      	compilerOptions: {
        	// vue 将跳过 my-custom-element 解析
          isCustomElement: tag => tag === 'my-custom-element'
        }
      }
    })
  ]
})

// 使用
<my-custom-element foo='foo' />
```

<a name="ERYkH"></a>

## 新增指令 v-memo

该指令可以缓存模版中的一部分，从而提升速度。例如：大量 v-for 的列表，只创建一次，就不会再更新了，直接用缓存，就是用内存换时间。

下面这样组件重新渲染时，如果 valueA 和 valueB 没有变化，div 将跳过此组件和其子组件的所有更新

```vue
<div v-memo='[valueA, valueB]'>
	...
</div>
```

还有像下面这样，部分缓存。需要注意的是：在 v-memo 里面不能用 v-for

```vue
<div v-for='item in list' :key='item.id' v-memo='[item.id === selected]'>
	...
</div>
```

<a name="TUZAr"></a>

## 性能提升

响应式的优化

- 更高效的 ref 实现，读取提升约 260%，写入提升约 50%
- 依赖收集速度提升约 4%
- 减少内存消耗约 17%

模版编译优化

- 创建元素 vnodes 速度提升约 200%
