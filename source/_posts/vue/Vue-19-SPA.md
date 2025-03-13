---
title: Vue-19-Vue路由（一）單頁應用程序
excerpt: 本篇會介紹單頁應用，路由的概念，以及安裝VueRouter的步驟。
tags: [Vue，SPA，VueRouter ]
categories: [Vue]
date: 2025-03-07 14:41:41
---

## 1. 前言
我們觀察公司官網，購物網站，個人部落格等等，其實可以發現頁面隨著我們的操作有兩個替換模式，

要麽同一個頁面，不刷新的情況下，只是頁面的一部分會更新；要麽刷新跳轉至指定頁面。

前者是單頁應用程序 - SPA（Single Page Application），後者則是多頁應用程序 - MPA（Multi Page Application）。

SPA的特點：

1. 整個應用只有一個頁面。
2. 在SPA中，單頁應用程序頁面之間的跳轉並不會導致整個頁面刷新。

擧個例子，我們購物網站流程的頁面，我們進下一步只會更新中間的頁面，導航沒有更懂，而且不會刷新整個頁面。

|||
|--|--|
| <img src="/img/Vue/Vue-19-2.png"> | <img src="/img/Vue/Vue-19-3.png"> |


<br><br>我們重點也是學習SPA。SPA和MPA的特點如下面的表格：

![](/img/Vue/Vue-19-1.png) 

## 2. 路由的概念
其實，SPA性能會這麽好是因爲内容都是按需加載的，但是要按需加載，必須有兩個核心：訪問路徑和組件的對應關係。

比如說路徑 `/shoppingCart` 應該只導入 `shoppingCart` 購物車相關的模塊，而不是導入其他的。

這種對應的關係實現的方式我們稱之爲<font size="5" color="#46A3FF">路由</font>。


## 3. Vue的路由 - VueRouter
Vue的路由可以在創建專案時就安裝，這裏就示範如何在手動安裝 VueRouter。

{% note danger %}
Vue版本不同，下載的VueRouter版本也不一樣！

Vue 2 使用 VueRouter 3； Vue 3.0 使用 VueRouter 4
{% endnote %}
<br>

1. 下載VueRouter
`npm i vue-router@3.6.5`
<br>

2. 新增文件夾： pages，把頁面都放進去：

比如新增A頁面和B頁面

`pageA.vue`
```vue
<template>
  <div>
    我是A頁面
  </div>
</template>
```
<br>

3. 設置router的index.js

新增文件夾：router， 新增一個 `index.js` ：

```js
import pageA from '../pages/pageA.vue'
import pageB from '../pages/pageB.vue'

import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter) // VueRouter插件實例化

const router = new VueRouter({
  routes: [
    {path: '/pageA', component: pageA},
    {path: '/pageB', component: pageB},
  ]
})

export default router
```
<br>


4. 在 `main.js` 中引入router的 `index.js` 文件

```js
import Vue from 'vue'
import App from './App.vue'

// 導入 router的index.js
import router from './router/index'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  // router實例挂載
  router
}).$mount('#app')
```
<br>

5. 在 `App.vue` 增加路徑，可以用 `#/pageA` 跳轉， `<router-view>` 顯示頁面的區域

```vue
<template>
  <div>
    我是主頁<br><br>
    <a href="#/pageA">A頁面</a><br><br>
    <a href="#/pageB">B頁面</a><br><br>
    <br>
    <router-view>顯示區域</router-view>
  </div>
</template>
```
<br>

我們可以測試一下：

點擊前
![](/img/Vue/Vue-19-4.png)

點擊后
![](/img/Vue/Vue-19-5.png)

就會發現路徑也會顯示出來，對應頁面也在網頁中顯示出來。

<br>
關於路由的基本設置就先到這裏，接下來會介紹更多Vue路由的知識，敬請期待。
<br>