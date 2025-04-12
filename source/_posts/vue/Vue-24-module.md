---
title: Vue-24-創建多個Vuex模塊
excerpt: 本篇會延續上一篇，探討Vuex創建模塊的方法，方便日後維護。
tags: [Vue， Vuex]
categories: [Vue]
date: 2025-04-10 16:33:57
---

我們在前幾篇討論了Vuex的基本使用方式，但爲了更好維護不同種類的數據，分成不同的模塊管理是更理想的。

## 1.創建子模塊

步驟1：在store目錄下創建modules文件夾，例如user.js

步驟2： 在user.js中創建模塊
```js
const state = {
  userInfo: {
    name: '张三',
    age: 18
  }
}
const mutations = {}
const actions = {}
const getters = {}

export default {
  state,
  mutations,
  actions,
  getters
}

```

步驟3： 在store/index.js引入,用 `modules`挂載

這一步就是相等於把子模塊挂在到 `index.js` 的 `state` 上

```js
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'

Vue.use(Vuex)

const store = new Vuex.Store({
  strict: true, // 严格模式
  modules: {
    user
  }
})

export default store

```


## 2.訪問子模塊的數據
### 2.1.通過模塊名訪問 `$store.state.模塊名.xxx`

```vue
<template>
  <div id="app">
    <div>store模塊user的數據：{{ $store.state.user.userInfo.name }}</div>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>
```

![](/img/Vue/Vue-24-1.png) 

### 2.2. 通過 `mapState` 映射
上面的方式的寫法比較長，我們也可以用 `mapState` 直接映射子模塊的所有數據，

比如我們要映射 `user` 子模塊的數據：

```vue
<template>
  <div id="app">
    <div>store模塊user的數據：{{ $store.state.user.userInfo.name }}</div>
    <div>store模塊user的數據的年齡： {{ user.userInfo.age }}</div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'App',
  computed: {
    ...mapState(['user'])
  }
}
</script>
```

![](/img/Vue/Vue-24-2.png) 
<br>

如果我們想要直接映射 `user` 子模塊的 `userInfo` 呢？也是可以的，但需要開啓命名空間，

因爲子模塊中的 `state` 等等都是全局挂載，開啓命名空間才會挂在在子模塊。

需要在子模塊寫 `namespaced: true` ：

user.js
```js
const state = {
  userInfo: {
    name: '张三',
    age: 18
  }
}
const mutations = {}
const actions = {}
const getters = {}

export default {
  namespaced: true,
  state,
  mutations,
  actions,
  getters
}
```

在組件就可以 `...mapState('模塊名'， ['模塊屬性和方法'])` 來訪問了：

```vue
<template>
  <div id="app">
    <div>store模塊user的數據：{{ $store.state.user.userInfo.name }}</div>
    <div>store模塊user的數據的年齡： {{ user.userInfo.age }}</div>
    <div>store模塊直接訪問userInfo： {{ userInfo }}</div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
export default {
  name: 'App',
  computed: {
    ...mapState(['user']),
    ...mapState('user', ['userInfo'])
  }
}
</script>
```

![](/img/Vue/Vue-24-3.png) 

這樣就可以更準確地訪問需要的子模塊的特定屬性和方法了。

其他的 `Getters`, `Mutations`, `Actions` 也一樣可以直接訪問或者映射，這裡就不一一介紹了。

總結就是：

在 `<script>` 裏面的寫法：

1. State
直接訪問：
`this.$store.state.user.userInfo.name`<br><br>映射：
`...mapState('user', ['userInfo'])`

2. Getters
直接訪問：
`this.$store.getters['user/UpperCaseName']`<br><br>映射：
`...mapState('user', ['UpperCaseName'])`

3. Mutations
直接訪問：
`this.$store.commit('user/setUser', { name: 'xiao' })`<br><br>映射：
`...mapState('user', ['setUser'])`

4. Actions
直接訪問：
`this.$store.dispatch('user/setUserSecond', { name: 'xiao' })`<br><br>映射：
`...mapState('user', ['setUserSecond'])`
<br>

你可能會好奇爲什麽 `state` 以外都是 ['子模塊/屬性方法']` 來訪問,

因爲他們以對象保存的方法是跟 `state` 不一樣的：

比如在 user.js 子模塊我們定義：

```js
const state = {
  userInfo: {
    name: 'aaa',
    age: 18
  }
}
const mutations = {
}
const actions = {}
const getters = {
    UpperCaseName (state) {
    state.userInfo.name.toUpperCase()
  }
}

export default {
  namespaced: true,
  state,
  mutations,
  actions,
  getters
}
```

在組件我們打印 `this.$store.state` 和 `this.$store.getters` 會看到：

![](/img/Vue/Vue-24-4.png) 

因爲保存的鍵名不一樣，所以我們在引用的時候需要也別注意這一點。
