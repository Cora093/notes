## vue笔记

### 快速入门

#### 1.声明式渲染

`reactive()`和`ref()`用于声明动态绑定的对象

1. `reactive()`只适用于对象 (包括数组和内置类型，如 `Map` 和 `Set`)

   ```js
   import { reactive } from 'vue'
   
   const counter = reactive({
     count: 0
   })
   
   console.log(counter.count) // 0
   counter.count++
   ```

2.  `ref()` 则可以接受任何值类型。`ref` 会返回一个包裹对象，并在 `.value` 属性下暴露内部值。

   ```js
   import { ref } from 'vue'
   
   const message = ref('Hello World!')
   
   console.log(message.value) // "Hello World!"
   message.value = 'Changed'
   ```

   注意使用`{{}}`引用ref时可以省略.value

   但更改ref时不可以省略

#### 2.Attribute 绑定

v-bind:绑定属性(可简写为:)

```js
<div v-bind:id="dynamicId"></div>
//简写
<div :id="dynamicId"></div>
```

#### 3.事件监听

v-on:监听事件(可简写为@)

```js
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  // 更新组件状态
  count.value++
}
</script>

<template>
  <!-- v-on绑定事件 -->
  <button v-on:click="increment">{{ count }}</button>
  <!-- 简写 -->
  <button @click="increment">{{ count }}</button>
</template>
```

#### 4.表单绑定

v-model绑定表单

```js
<script setup>
import { ref } from 'vue'
const text = ref('')
</script>

<template>
  <input v-model="text" placeholder="Type here">
  <p>{{ text }}</p>
</template>
```

#### 5.条件渲染

v-if v-else 控制条件渲染

```js
<script setup>
import { ref } from 'vue'

const awesome = ref(true)

function toggle() {
  // ...
  awesome.value = awesome.value===true?false:true
}
</script>

<template>
  <button @click="toggle">toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</template>
```

#### 6.列表渲染

v-for列表渲染

```js
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>
```

数组添加和删除元素
![image-20230818161736576](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230818161736576.png)

#### 7.计算属性

`computed`将一个函数定义为计算属性，该函数将在相关依赖数据**发生变化时**自动重新计算，并返回计算后的新属性值。

```js
const total = computed(() => {
      return count.value * price; // 计算商品总价，依赖count和price
    });
```



#### 8.生命周期和模板引用

有时我们会不可避免地需要手动操作 DOM。这时我们需要使用**模板引用**——也就是指向模板中一个 DOM 元素的 ref。

![image-20230818164249918](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230818164249918.png)

#### 9.侦听器

watch侦听器 对象改变时执行回调函数

```js
watch(count, (newCount) => {  
    // 没错，console.log() 是一个副作用  
    console.log(`new count is: ${newCount}`) 
    })
```

#### 10.引入组件

![image-20230818165918643](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230818165918643.png)



#### 11.Props

父组件传递动态值到子组件

![image-20230818170658451](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230818170658451.png)



#### 12.Emits

子组件向父组件触发事件

![image-20230818171130145](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230818171130145.png)



#### 13.插槽

![image-20230818171519893](https://cora-typora-test-2023.oss-cn-shanghai.aliyuncs.com/pics/image-20230818171519893.png)



