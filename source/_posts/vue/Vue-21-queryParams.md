---
title: Vue-21-Vue路由（三）路由傳參
excerpt: 本篇會介紹如果利用路由在頁面跳轉的時候把參數傳遞至下一個頁面。
tags: [Vue， VueRouter， query， params]
categories: [Vue]
date: 2025-03-07 15:54:46
---

我們在上一篇提到路由的一些設定，這一篇是路由最重要的篇章，

會提到如何在路由跳轉的時候帶上參數給下一頁。

## 1. 路徑傳參數
### 1.1. 路徑跳轉

我們在傳遞參數時，可以用 `$router.push` 路徑的時候携帶參數

### 1.1.1. 查詢參數傳參
當前頁面：
```html
<div @click=$router.push(`/detail?id=${item.id}`)>點擊我跳轉</div>
```
<br>

下一頁面：
```js
export default {
  created(){
    console.log(this.$route.query.id)
  }
}
```
<br>

### 1.1.2. 動態路由傳參

如果使用這種方式傳參，需要先更改路由的地址，在 `/detail` 的後面加上`id`

router > index.js
```js
{
  path: '/detail/:id',
  component: Detail
}
```
<br>

當前頁面：
```html
<div @click=$router.push(`/detail/${item.id}`)>點擊我跳轉</div>
```
<br>

下一頁面：
```js
export default {
  created(){
    console.log(this.$route.params.id)
  }
}
```
<br>
<br>
那這兩種有傳參方式有什麽特別呢？
<br><br>

查詢參數，一般適合需要傳遞很多參數的類型；動態路由傳參適合傳遞單個參數。

所以可以根據參數的多寡來決定想要使用的方式。
