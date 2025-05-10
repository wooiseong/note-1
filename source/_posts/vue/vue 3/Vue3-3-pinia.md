---
title: Vue3-3-pinia ———— 比Vuex更進步的狀態管理工具
excerpt: 本篇介紹類似Vuex的狀態管理工具, pinia， 在Vue 3的專案使用。
tags: [Vue， Vue3, pinia]
categories: [Vue]
date: 2025-04-13 16:13:58
---

我們之前使用的Vuex，編碼風格和Vue 2的選項式API是非常接近的，

在Vue 3，我們更推薦用新一代狀態管理工具 ———— pinia。

pinia的特點如下：

![](/img/Vue/Vue3/Vue3-3-1.png) 


## 1. 安裝pinia

1. npm下載pinia的包
`npm install pinia`

2. 在main.js中引入pinia
```js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(pinia)
app.mount('#app')
```

## 2. 使用pinia
### 2.1. 開pinia倉庫
開倉庫的方式和Vuex的差不多：

比如我在src開一個store文件夾

```js
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(100)

  const addOn = () => count.value + 1

  return {
    count,
    addOn
  }
})
```

然後在需要使用的組件引入倉庫，使用倉庫中的數據：

```js
<script setup>
import { useCounterStore } from '@/store/counter'

const counterStore = useCounterStore()

</script>

<template>
<div>倉庫的數據：{{ counterStore.count }}</div>
</template>
```

![](/img/Vue/Vue3/Vue3-3-2.png) 


### 2.2. 使用pinia倉庫

異步操作的話，因爲沒有了 `Actions` ， 所以可以直接在倉庫中寫異步操作的代碼，也不用在組件使用 `dispatch` ：

store/counter.js
```js
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(100)

  const addOn = () => count.value++

  const addOnDelay = () => {
    setTimeout(() => {
      addOn()
    }, 1000)
  }

  return {
    count,
    addOn,
    addOnDelay
  }
})
```

App.vue
```vue
<script setup>
import { useCounterStore } from '@/store/counter'

const counterStore = useCounterStore()

</script>

<template>
<div>倉庫的數據：{{ counterStore.count }}</div>
<div><button @click="counterStore.addOnDelay">一秒后加1</button> {{ counterStore.count }}</div>
</template>
```

![](/img/Vue/Vue3/Vue3-3-3.png) 

### 2.3. 解構倉庫中的數據
大家還記得我們在Vuex中，訪問倉庫，除了直接訪問之外，可以用特別工具比如 `mapState` ，

來映射 `state` 保存的數據。

在pinia中，我們直接可以解構倉庫的數據，比如解構出 `count`：

```vue
<script setup>
import { useCounterStore } from '@/store/counter'

const counterStore = useCounterStore()

const { count } = counterStore

</script>

<template>
<div>倉庫的數據：{{ count }}</div>
<div><button @click="counterStore.addOnDelay">一秒后加1</button> {{ count }}</div>
</template>
```

![](/img/Vue/Vue3/Vue3-3-4.png) 

咦？怎麽丟失 `count` 的響應式了，點擊新增按鈕，數據沒有變化？

因爲倉庫的數據是 `reactive` 進行包裝的，用 `proxy` 劫持監聽數據（屬性和方法）的變化，

解構相等於賦值給新的變量，就是新的屬性和方法了，

解決方法很簡單，只要用 `storetoRef` 包裝被解構的倉庫就可以了。

```vue
<script setup>
import { useCounterStore } from '@/store/counter'
import { storeToRefs } from 'pinia'

const counterStore = useCounterStore()

const { count } = storeToRefs(counterStore)

</script>

<template>
<div>倉庫的數據：{{ count }}</div>
<div><button @click="counterStore.addOnDelay">一秒后加1</button> {{ count }}</div>
</template>

```

{% note info %}
只有屬性需要用 `storeToRefs` 包裝，方法不用。
{% endnote %}


### 2.4. pinia持久化插件
我們可以用 `persistedstate` 的插件，來實現數據持久化：

1. 安裝插件
`npm install pinia-plugin-persistedstate`

2. 在main.js中引入插件
```js
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import persist from 'pinia-plugin-persistedstate'
import App from './App.vue'

const pinia = createPinia()
const app = createApp(App)

app.use(pinia.use(persist))
app.mount('#app')
```

3. 在倉庫中使用插件 `persist:true` 在本地 `localStorage`保存數據
```js
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useCounterStore = defineStore('counter', () => {
  const count = ref(100)

  const addOn = () => count.value++

  const addOnDelay = () => {
    setTimeout(() => {
      addOn()
    }, 1000)
  }

  return {
    count,
    addOn,
    addOnDelay
  }
},{
  persist: true
})
```

我們也可以修改 `localStorage` 保存的名字：
```js
{
  persist: {
    key: 'countStore'
  }
}
```

或者特定要保存的數據，比如我們要保存 `count` 而已：
```js
{
  persist: {
    key: 'countStore',
    paths: ['count']
  }
}
```

![](/img/Vue/Vue3/Vue3-3-5.png) 