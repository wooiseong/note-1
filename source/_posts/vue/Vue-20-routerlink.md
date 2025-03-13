---
title: Vue-20-Vue路由（二）router-link的應用
excerpt: 本篇會介紹router-link的使用方式。
tags: [Vue， VueRouter， router-link]
categories: [Vue]
date: 2025-03-07 15:54:33
---


## 1. 路由
我們看Vue專案有時會發現跳轉路由不一定是使用 `a` 標簽，更多是看到 `router-link`的標簽：

```vue
<router-link to="/pageA">A頁面</router-link>
```

這兩種標簽的差別是什麽呢？
<br>
- `router-link`不需要在路徑加 `#` ，因為它會自動幫我們加上。
<br>

- 實現高亮


### 1.1 router-link實現高亮
用上一個例子，如果我們要實現點擊高亮，可以給那個標簽的父類加 `.router-link-active`

```vue
<template>
  <div>
    我是主頁<br><br>
    <div class="pageLink">
      <router-link to="/pageA">A頁面</router-link><br><br>
      <router-link to="/pageB">B頁面</router-link><br><br>
    </div>
    <br>
    <router-view>顯示區域</router-view>
  </div>
</template>

<script>
export default {
}
</script>

<style scoped>
.pageLink a.router-link-active {
  color: red;
}
</style>
```
<br>

點擊就會實現高亮了：

![](/img/Vue/Vue-20-1.png) 

這種路徑匹配方式是模糊匹配，就是只要 `/pageA/A` , `/pageA/B` 等等，只要有 `/pageA` 就會匹配成功。

如果要路徑一樣才能實現高亮的話可以使用 `router-link-exact-active`
<br>

### 1.2 定制高亮類名

這些類名很長，Vue有提供一個方法給我們修改高亮類名：

1. 在router/index.js：

```js
const router = new VueRouter({
  routes: [
    {path: '/pageA', component: pageA},
    {path: '/pageB', component: pageB},
  ],
  linkActiveClass: 'activePro',
  linkExactActiveClass: 'exact-activePro'
})
```

2. 在樣式上面：

```vue
<style scoped>
.pageLink a.activePro {
  color: red;
}
</style>
```

## 2. 路由相關
### 2.1. 重新定向
我們的路由常會遇到兩種情況，可以在router/index.js解決：

- 在指定路徑重新定向去另一頁面
比如我們要用戶一登錄頁面就馬上跳去 `/home` 頁面，可以用 `redirect` 做到

```js
const router = new VueRouter({
  routes: [
    {path: '/', redirect: '/pageA'},
    {path: '/pageA', component: pageA},
    {path: '/pageB', component: pageB},
  ],
})
```
<br>

- 路徑都不匹配，導去404頁面
我們可以用 `path: '*', component: NotFind` ， 放在最下面的路徑

```js
const router = new VueRouter({
  routes: [
    {path: '/', redirect: '/pageA'},
    {path: '/pageA', component: pageA},
    {path: '/pageB', component: pageB},
    { path: '*', component: NotFind }
  ],
})
```
<br>

### 2.2 路由跳轉方式
VueRouter提供兩種路由跳轉方式，分別是path路徑跳轉和name命名路由跳轉的方式。

#### 2.2.1 path路徑跳轉
我們之前學的都是path路徑跳轉，

在邏輯只需要 `this.$router.push({路徑})` 即可
<br>

假如我們的 router>index.js 的路徑是這樣：
```js
const router = new VueRouter({
  routes: [
    {path: '/pageA', component: pageA},
    {path: '/pageB', component: pageB},
  ],
})
```

我們在頁面可以提供這樣的邏輯：

```vue
<template>
  <div>
    我是主頁<br><br>
    <button @click="toPageA">跳轉A頁面</button>
    <br>
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  methods: {
    toPageA() {
        this.$router.push({
          path: '/pageA'
      })
    }
  }
}
</script>
```
<br>

#### 2.2.2 name命名路由跳轉
上面的方法很簡單，適合路徑比較沒有這麽長的情況。如果很長，最好用命名路由的方式比較簡潔。


我們的 router>index.js 的路徑要改成這樣：
```js
const router = new VueRouter({
  routes: [
    {name: 'pageA', path: '/pageA', component: pageA},
    {name: 'pageB', path: '/pageB', component: pageB},
  ],
})
```

在頁面就是這樣寫：
```vue
<script>
export default {
  methods: {
    toPageA() {
      this.$router.push({
        name: 'pageA'
      })
    }
  }
}
</script>
```
<br>
<br>
至於這兩種跳轉方式要如何傳遞參數，
<br><br>

我們會留到下一期講，敬請期待。
<br>