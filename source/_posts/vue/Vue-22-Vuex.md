---
title: Vue-21-Vuex————管理狀態的倉庫
excerpt: 本篇會介紹Vuex的概念，以及使用方式。
tags: [Vue， Vuex]
categories: [Vue]
date: 2025-03-13 16:20:43
---

在組件通信篇[Vue-12-組件之間的通信（一）](https://notebook-olive.vercel.app/2025/03/05/vue/Vue-12-props/) ， 相信大家初學都是一片混亂。

如果我們的項目簡單還好，如果很複雜就會很難維護。比如登入后的個人信息，幾乎是每個組件都會用到的。我們一直 `provide`和` inject` 也不是辦法。

在需要多個組件共享狀態，以及多個組件共同維護一份數據的情況下，

Vuex就是你的答案。
<br>

## 1. Vuex的基礎概念

Vuex是一個類似插件的狀態管理工具，類似記憶倉庫。任何組件都可以從倉庫獲取想要的數據。

Vuex的優勢：
- 共同維護一份數據，數據集中化管理
- 響應式變化
- 操作簡潔，裏面提供了一些函數可以簡化我們的代碼

![](/img/Vue/Vue-22-1.png) 

## 2. Vuex的安裝

1. 安裝Vuex
```
npm install vuex@next --save
```

2. 項目引入Vuex
打開一個store文件夾，開一個 “index.js” 文件

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

3. 創建倉庫

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store()

export default store
```

4. 將倉庫引入到main.js
main.js

```js
import store from '@/store/index'

new Vue({
  render: h => h(App),
  store
}).$mount('#app')

```

## 3. Vuex的狀態
### 3.1. state狀態
這個狀態是Vuex保存的數據，我們要訪問可以通過兩種方式：

store直接訪問，或者通過輔助函數

#### 3.1.1. 直接訪問store

我們先在store/index.js定義 `state` 的資料：
```js
const store = new Vuex.Store({
  state: {
    title: '我是資料',
    content: '我是内容'
  },
})

export default store
```

在App.vue的文件：
```vue
<template>
  <div id="app">
    <div>取出資料： {{ $store.state.title }}</div>
  </div>
</template>

<script>
export default {
  name: 'App',
  created () {
    console.log(this.$store.state.title)
  }
}
</script>
```

如果是在JS模塊，比如main.js，就直接寫 `store.state.title` 就可以了。

#### 3.1.2. 輔助函數訪問store
如果我們的數據很少，當然可以直接訪問store，

但是state裏面的數據很多很深，就要寫很長，不方便。

我們可以用 `mapState` 把store的數據自動映射組建的 `computed` 裏面。

我們在組件的需要引入 `mapState` ，然後在 `computed` 使用：
```vue
<template>
  <div id="app">
    <div>取出資料： {{ title }}</div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'App',
  created () {
    console.log(this.title)
  },
  computed: {
    ...mapState(['title', 'content'])
  }
}
</script>
```
<br><br>
關於其他的狀態，就留到下一篇再討論，敬請期待。
<br>