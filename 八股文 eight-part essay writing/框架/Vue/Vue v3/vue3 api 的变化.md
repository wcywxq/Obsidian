---
title: vue3 api 的变化
url: https://www.yuque.com/wcywxq/mxunh7/cgy1qw
---

<a name="ANZGw"></a>

## watch 的变化

- vue3 中的 watch 函数第一个参数为响应式对象，有 getter/setter 的 effect 函数，或者这些类型数组；第二个参数为数据变化时的回调；第三个函数为 watchOption，提供是否立即监听和是否深度监听的配置
- vue3 可以多次使用 watch 方法

```javascript
watch(data, () => {})
watch(() => data.name, () => {})
watch([data, name], () => {})
```

- vue3 的 setup 中不存在 this 对象，监听路由需要使用 vue-router 提供的 useRoute，vue2 则是在 watch 对象里添加 '$route' 进行监听

```vue
<script>
	import { useRoute } from 'vue-router'
  // setup
  const route = useRoute()
  const userData = ref()
  // 当参数更改时获取用户信息
  watch(() => route.params, () => {})
</script>
```

<a name="kTbgM"></a>

## vue3 中 watchEffect 与 watch 的异同

不同点：

- watchEffect 立即执行传入的函数，在初始化时自动收集依赖，并在其依赖变更时重新运行该函数
- watchEffect 无法获取原值，只能得到变化后的值

相同点：
二者都可以通过 StopHandle.stop 函数手动停止监听

```javascript
const stopHandle = watchEffect()
stopHandle.stop()
```

<a name="V0h8X"></a>

## v-model 的变化

<a name="cfxNZ"></a>

### vue2 的 v-model

v-model 通过在数据修改时，通知父级节点实现数据的双向绑定。其实是一个语法糖当满足以下两个条件时，可实现自定义组件的v-model：

- 子组件受控接收 prop
- 数据修改时触发 event 把新的数据提交给父组件

```vue
<script>
export default {
	props: {
  	value: String
  },
  model: {
    prop: 'value',
    event: 'change'
  }
}
</script>
```

<a name="tGMtG"></a>

### vue3 的 v-model

vue3 中升级了 v-model 的用法，通过 update:modelValue 来触发事件，prop 也被更改为 modelValue。还在一个组件上支持多个 v-model，如新增 v-model:lastName 等。

```javascript
props: {
  modelValue: String,
}
emits: ['update:modelValue'],
methods: {
  changeTitle() {
    this.$emit('update:modelValue', title)
  }
}
```
