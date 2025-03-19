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