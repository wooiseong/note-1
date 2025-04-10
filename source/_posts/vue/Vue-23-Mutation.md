---
title: Vue-23-Vuex的mutation,actions和getters
excerpt: 本篇會延續上一篇，探討Vuex的mutation,actions和getters的作用和引用場景。
tags: [Vue， Vuex]
categories: [Vue]
date: 2025-03-19 16:20:34
---

延續上一篇的内容，本篇會繼續討論Vuex的各種狀態

### 3.2. mutations狀態
Vuex一樣也是遵循單項數據流模式，初學者可能會容易忘記這一點，我們可以在store/index.js開啓嚴格模式， `strict: true`

在組件直接修改 `state` 的數據會報錯：

```js
const store = new Vuex.Store({
  strict: true,
  state: {
    title: '我是資料',
    content: '我是内容'
  },
})

export default store
```


當我們要修改 `state` 的數據時，一樣需要從組件傳遞信息給store，

由store統一去修改數據。

這個修改store數據的方法就是 `mutations`


#### 3.2.1. 直接訪問store
那些修改的方法和 `state` 一樣寫在store/index.js，我們可以提供函數修改 `state` 的數據。

我們在store的 `mutations` 定義一個函數修改 `state.title` ：

store/index.js
```js
const store = new Vuex.Store({
  strict: true, // 严格模式
  state: {
    title: '我是資料',
    content: '我是内容'
  },
  getters: {
  },
  mutations: {
    changeTitle (state, text) {
      state.title = text
    }
  },
  actions: {
  },
  modules: {
  }
})
```

函數的第一個參數是 `state` 的變量；第二個參數是我們組件傳過來的參數。
<br>

app.vue
```vue
<script>
 methods: {
    changeTitleAction () {
      this.$store.commit('changeTitle', '換資料')
    }
  }
</script>
```

我們在需要的頁面，比如綁定一個點擊事件，用： ` this.$store.commit` 修改數據，第一個參數是 `mutations` 的方法名， 第二個參數是傳遞過去那個方法的實參。

點擊按鈕前：
![](/img/Vue/Vue-23-1.png) 


點擊按鈕後：
![](/img/Vue/Vue-23-2.png) 


#### 3.2.2. 輔助函數訪問store
和 `state` 一樣， `mutations` 可以用 `mapMutations` 把store的方法映射到組件的 `methods` 中。

我們在組件一樣引入 `mapMutations` , 直接把 `mutations` 的方法映射到組件的 `methods` 中。

我們就可以 `this.方法` 直接使用方法：

app.vue
```vue
<script>
import { mapState, mapMutations } from 'vuex'
export default {
  name: 'App',
  created () {
    console.log(this.title)
  },
  computed: {
    ...mapState(['title', 'content'])
  },
  methods: {
    ...mapMutations(['changeTitle']),
    changeTitleAction () {
      this.changeTitle('換資料')
    }
  }
}
</script>
```

### 3.3. actions狀態
`actions` 和 `mutations` 很像，只是前者可以處理異步操作，後者只可以處理同步操作。

#### 3.3.1. 直接訪問store

我們用上面的例子舉例：我要點擊后1秒才更換資料。

store/index.js
```js
const store = new Vuex.Store({
  strict: true, // 严格模式
  state: {
    title: '我是資料',
    content: '我是内容'
  },
  getters: {
  },
  mutations: {
    changeTitle (state, text) {
      state.title = text
    }
  },
  actions: {
    changeTitleDelay (state, text) {
      setTimeout(() => {
        state.commit('changeTitle', text)
      }, 1000)
    }
  },
  modules: {
  }
})
```

注意 `actions` 調用的方法還是需要在 `mutations` 寫好，用 `commit` 調用。

在組件用 `dispatch` 調用 `actions` 方法即可：

app.vue

```vue
<script>
import { mapState } from 'vuex'
export default {
  name: 'App',
  created () {
    console.log(this.title)
  },
  computed: {
    ...mapState(['title', 'content'])
  },
  methods: {
    changeTitleAction () {
      this.$store.dispatch('changeTitleDelay', '換資料')
    }
  }
}
</script>
```


#### 3.3.2. 輔助函數訪問store
一樣 `actions` 可以使用輔助函數 `mapActions` 來映射在組件的 `methods` 裏：


app.vue
```vue
<script>
import { mapState, mapActions } from 'vuex'
export default {
  name: 'App',
  created () {
    console.log(this.title)
  },
  computed: {
    ...mapState(['title', 'content'])
  },
  methods: {
    ...mapActions(['changeTitleDelay']),
    changeTitleAction () {
      this.changeTitleDelay('換資料')
    }
  }
}
</script>
```

### 3.4. getters狀態
`getters` 類似計算屬性。我們在 `state` 派生出一些狀態，是依賴 `state` 的，就可以用 `getters` 。

舉例來説，我們需要在 `store` 保存一個數組， 這個數組提供一個大於5的數據，我們可以在 `getters` 進行


#### 3.4.1. 直接訪問store

store/index.js
```js
const store = new Vuex.Store({
  strict: true, // 严格模式
  state: {
    title: '我是資料',
    content: '我是内容',
    list: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  },
  getters: {
    filterList (state) {
      return state.list.filter(item => item > 5)
    }
  }
})
```

在組件直接用 `$store.getters.方法` 就可以訪問 `getters` 的方法了

app.vue
```vue
<template>
  <div id="app">
    <div>取出資料： {{ title }}</div>
    <button @click="changeTitleAction">換資料</button>
    <div>原始資料： {{ list }}</div>
    <div>大於5的資料： {{ $store.getters.filterList }}</div>
  </div>
</template>
```

![](/img/Vue/Vue-23-3.png) 


#### 3.4.2. 輔助函數訪問store
我們可以用 `mapGetters` 輔助函數來映射 `getters` 的方法。

這個映射和 `mapState` 一樣都是在 `computed` 裏：

app.vue
```vue
<template>
  <div id="app">
    <div>取出資料： {{ title }}</div>
    <button @click="changeTitleAction">換資料</button>
    <div>原始資料： {{ list }}</div>
    <div>大於5的資料： {{ filterList }}</div>
  </div>
</template>

<script>
import { mapState, mapGetters, mapActions } from 'vuex'
export default {
  name: 'App',
  created () {
    console.log(this.title)
  },
  computed: {
    ...mapState(['title', 'content', 'list']),
    ...mapGetters(['filterList'])
  },
  methods: {
    ...mapActions(['changeTitleDelay']),
    changeTitleAction () {
      this.changeTitleDelay('換資料')
    }
  }
}
</script>
```

<br><br>

目前我們對於四種狀態的學習完成了！如果只有一個倉庫，的確是很容易管理。

但專案變大的時候，我們可能需要不同的倉庫去保存不同的狀態，這時候我們就需要 `modules` 了。

下次會詳細説明，敬請期待。
