---
tags:
  - programming
  - vue
description: Vue 在 SFC 中如何申明变量
---
```js
<script setup>  
import { reactive } from 'vue';  
  
// 使用 reactive 定义一个响应式对象  
const state = reactive({  
	count: 0,  
	name: 'Alice'  
});  
  
function increment() {  
	state.count++;  
}  
</script>
```
## 区别
在 Vue 中，ref、reactive 和 computed 都是用于处理响应式数据的工具，但它们有各自不同的用途和行为。

ref：

ref 用于创建一个响应式的引用对象，它通常用于包装单个值或 DOM 元素。
通过 ref 创建的响应式数据可以通过 .value 来访问和修改。
ref 在模板中可以直接使用，不需要通过 .value 访问。

reactive：

reactive 用于创建一个响应式对象，它接受一个普通对象并返回一个响应式代理。
与 ref 不同，reactive 创建的对象不需要通过 .value 来访问和修改属性。
reactive 更适合处理复杂的数据结构，如对象或数组。

computed：

computed 用于创建计算属性，它根据依赖的数据进行计算并返回一个响应式的结果。
计算属性是基于它们的依赖进行缓存的，只有当依赖的数据发生变化时，计算属性才会重新计算。
computed 通常用于处理复杂逻辑或需要缓存的计算结果。

总结：

ref 主要用于单个值的响应式处理。
reactive 主要用于对象的响应式处理。
computed 主要用于基于依赖的计算属性的处理。

在 Vue 3 中，这些工具提供了更灵活和强大的响应式数据处理能力。