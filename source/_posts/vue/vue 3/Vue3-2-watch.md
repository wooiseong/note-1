---
title: Vue3-1-Vue 2的未來 --- Vue3的登場 (二)
excerpt: 本篇繼續介紹Vue 3新增的特點，和Vue 2相比需要注意的地方。
tags: [Vue， Vue3]
categories: [Vue]
date: 2025-04-13 13:45:41
---

這一篇延續上一篇 [Vue3-1-Vue 2的未來 --- Vue3的登場 (一)](https://wooiseong.vercel.app/2025/04/13/vue/vue%203/Vue3-2-watch/),會繼續講解 Vue 3 和Vue 2相比特別的内容。

## 7. Vue 3的 `watch`

`watch` 的寫法和原來的差不多，可以傳一個數組的方式，當成 `watch` 的第一個參數,同時監聽不同的數據：

```vue
<script setup>
import { ref, watch } from 'vue'

const count = ref('')
const nickname = ref('')
const info = ref({
  name: 'Wooiseong',
  age: 18
})

const changeCount = () => {
  count.value = 1
}

const changeName = () => {
  nickname.value = 'John'
}

watch([count, nickname], (newVal, oldVal) => {
  console.log(newVal, oldVal)
}, {
  immediate: true,
  deep: true
})

</script>

<template>
  <div @click="changeCount">點擊我獲得數量 {{ count }} </div>
  <div @click="changeName">點擊我獲得名字 {{ nickname }}</div>
</template>
```

點擊前
![](/img/Vue/Vue3/Vue3-2-1.png) 

點擊後
![](/img/Vue/Vue3/Vue3-2-2.png) 


如果我只是想要監聽某個對象的屬性呢？比如 `info.age` ？

我們可以這麽寫：

```vue
watch(() => info.value.age, (newVal, oldVal) => {
  console.log(newVal, oldVal)
})
```

## 8. Vue3的生命周期
其實和Vue 2相比， Vue 3的生命周期都是一樣的，只是可能字眼換了一點而已。

![](/img/Vue/Vue3/Vue3-2-3.png)

只是需要注意的是，如果要使用生命周期鈎子函數，需要特別引入，才可以使用：

比如使用 `onMounted`：

```js
<script setup>
import { ref, onMounted } from 'vue'

const count = ref('')

onMounted(() => {
  console.log('onMounted使用')
})

</script>
```

## 9. Vue3的組件傳遞

### 9.1 父傳子
Vue 3的組件傳遞跟Vue 2的差不多，只是因爲沒有開啓 `props` 的地方， 

所以需要用宏編譯器 `defineProps` 在子組件接收從父組件傳遞的信息：

父組件：
```vue
<script setup>
import SonA from '@/components/SonA.vue'
import { ref } from 'vue'

const money = ref(1000)

</script>

<template>
  我是父組件，我的子組件在下面<br><br>
  <SonA car="寶馬" :money="money"></SonA>
</template>
```

子組件：
```vue
<script setup>

const props = defineProps({
  car: String,
  money: Number
})

</script>

<template>
  <div>我是組件A: 我收到的消息是： {{ car }}, 價格是 {{ money }}</div>
</template>
```

### 9.2 子傳父
子傳父也是用 `emit` ，但需要借用 `defineEmits` 接收將要觸發的事件，才可以傳遞數據給父組件。

子組件
```vue
<script setup>

const props = defineProps({
  car: String,
  money: Number
})

// 用defineEmits來定義子組件觸發的事件
const emit = defineEmits(['changeMsg'])
const sendMsg = () => {
  emit('changeMsg', 1000000)
}

</script>

<template>
  <div>我是組件A: 我收到的消息是： {{ car }}, 價格是 {{ money }}</div>
  <div><button @click="sendMsg">點擊發送花費</button></div>
</template>
```

父組件
```vue
<script setup>
import SonA from '@/components/SonA.vue'
import { ref } from 'vue'

const money = ref(1000)

// 接收子組件傳過來的值
const msg = ref('')
const receiveMsg = (newMsg) => {
  msg.value = newMsg
}

</script>

<template>
  我是父組件，我的子組件在下面 {{ msg }} <br><br>
  <SonA car="寶馬" :money="money" @changeMsg="receiveMsg"></SonA>
</template>

```

點擊前
![](/img/Vue/Vue3/Vue3-2-4.png)

點擊后
![](/img/Vue/Vue3/Vue3-2-5.png)


## 10. Vue3的DOM操作
Vue 3 因爲沒有 `this` ， 可以直接操作 `ref` 綁定的那個變量：

```vue
<script setup>
import { ref, onMounted } from 'vue'

const inp = ref(null)

console.log(inp.value)

onMounted(() => {
  console.log(inp.value)
})

</script>

<template>
  <input ref="inp" type="text" placeholder="我是輸入框">
</template>

```

![](/img/Vue/Vue3/Vue3-2-6.png)


不過必須記得要 `onMounted` 才可以渲染完DOM， DOM的操作才可以進行。


默認情況下， `script setup` 組件内部的屬性和方法是不對外開放的，

如果父組件要訪問子組件的DOM呢？我們可以用 `defineExpose`

這個Vue 3新加的，主要可以讓組件外部使用組件内的變量和方法：

子組件
```vue
<script setup>
import { ref } from 'vue'

const msg = ref('hello')

defineExpose({
    msg
})

</script>

<template>
  <div>我是子組件</div>
</template>
```

父組件
```vue
<script setup>
import { ref } from 'vue'
import SonA from '@/components/SonA.vue'

const testRef = ref(null)

const newCount = ref('')

const getCount = () => {
  newCount.value = testRef.value.count
}

</script>

<template>
我是父組件，我收到SonA暴露的值是 <button @click="getCount">點擊我獲取</button>

{{ newCount }}
<br>
<br>
 <SonA ref="testRef"></SonA>
</template>

```

![](/img/Vue/Vue3/Vue3-2-7.png)

## 11. Vue 3的 `provide` 和 `inject`

Vue 3 的 `provide` 和 `inject` 也是需要額外引入，寫法跟Vue 2的差不多：

`provide` 的組件
```vue
<script setup>
import { ref, provide } from 'vue'
import SonA from '@/components/SonA.vue'

const count = ref(100)

provide('counting', count )

</script>

<template>
<div>我是父組件，我provide的是 {{ count }}</div>

<SonA></SonA>
<br>
</template>
```


`inject` 的組件
```vue
<script setup>
import { inject } from 'vue'

const receivedCount = inject('counting')
</script>

<template>
  <div>我是子組件，我inject接收的是 {{ receivedCount }}</div>
</template>
```

![](/img/Vue/Vue3/Vue3-2-8.png)


## 12. Vue 3 的 `defienOptions`
原來的Vue 2我們可以使用和 `setup` 平級的屬性，如 `props` 和 `components`，

因爲 `script setup` 這種寫法的關係，已經無法添加平級的屬性。

雖然我們可以用宏編譯器 `defineProps` 和 `defineEmits`， 其他的屬性還是無法添加。額外用個 `script` 也很不方便。

`defineOptions` 可以解決這個問題,它可以定義 `Options API`的選項，除了 `expose, slots` 和上面提到的 `props` 和 `emits` ，

因爲可以用 `defineXXX` 來解決。

```js
<script setup>
defineOptions({
  name: 'App'
})

</script>
```

## 13. Vue 3 的 `defineModel`
一般我們子組件的數據都必須由父組件 `props` 傳給子組件，子組件 `emits` 回父組件，

但現在有更方便的 `defineModels` 可以在子組件包裹從父組傳來的數據，直接在子組件修改即可。

父組件：
```vue
<script setup>
import { ref } from 'vue'
import SonA from '@/components/SonA.vue'

const count = ref(100)
</script>

<template>
父組件的數字： {{ count }}<br><br>

子組件的按鈕
<SonA v-model="count"></SonA>
</template>

```

子組件：
```vue
<script setup>
const model = defineModel()
const update = () => {
  model.value++
}
</script>

<template>
  <button @click="update">{{ model }}</button>
</template>
```

