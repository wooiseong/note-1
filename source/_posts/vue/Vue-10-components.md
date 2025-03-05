---
title: Vue-10-組件化開發
excerpt: 本篇介紹如果編寫組件，讓項目可以更好維護
tags: [Vue， components]
categories: [Vue]
date: 2025-03-05 14:02:22
---

我們在上一篇[Vue-9-進入Vue工程化開發的世界！](https://notebook-olive.vercel.app/2025/03/02/vue/Vue-9-projectCreation/) 有介紹工程化開發的基礎，

既然可以在 `App.vue` 直接寫好模板，行爲和樣式， 爲什麽我們還要學組件化開發呢？ 

因為組件化開發讓我們可以將重複的代碼抽取出來，讓代碼更簡潔，更易於維護。

## 1. 局部注冊
我們在 `components` 的文件夾可以寫我們想要的組件代碼。

在需要的頁面引入，比如我只是 `App.vue` 需要引入組件，我就在 `App.vue` 引入和注冊那個組件，只有 `App.vue` 才可以使用那個組件

這種按需引入的方式被稱爲<font color="	#46A3FF" size="5">**局部注冊**</font>。

<br>

比如我們要做一個網頁，分別有頭部，内容和底部，我們可以切成三個模塊。
![](/img/Vue/Vue-10-1.png) 

1. 在 `components` 開三個文件：

- HmHeader.vue
- HmMain.vue
- HmFooter.vue
<br>

2. 在 `components` 的組件寫對應的内容：

- HmHeader.vue

```vue
<template>
  <div class="hm-header">我是頭部</div>
</template>

<script>
export default {
  
}
</script>

<style>
.hm-header {
  height: 50px;
  background-color: purple;
}
</style>
```
<br>

3. 在 `App.vue` 把内容清空，在 `<script>` 用 `import` 引入文件，文件名稱要和文件名一致，

4. 在  `<script>` 的 `components` 進行註冊，讓 `App.vue` 可以使用這個組件。

5. 在 `<template>` 使用組件，用 `useHmHeader` 來命名，這樣就可以在 `App.vue` 使用 `useHmHeader` 這個組件了。
<br>

```vue
<template>
  <div>
    <useHmHeader></useHmHeader>
    <useHmMain></useHmMain>
    <useHmFooter></useHmFooter>
  </div>
</template>

<script>
import HmHeader from '@/components/HmHeader.vue'
import HmMain from '@/components/HmMain.vue'
import HmFooter from '@/components/HmFooter.vue'

export default {
  components: {
    useHmHeader: HmHeader,
    useHmMain: HmMain,
    useHmFooter: HmFooter
  }
}
</script>

<style>
</style>
```
最終畫面：
![](/img/Vue/Vue-10-2.png) 
<br>

{% note info %}
在 `App.vue` 的 `components` 注冊組件時，如果命名和原來引入的都是同樣的，比如 `HmHeader` 

可以直接寫這個名稱，不用額外命名：

```js
export default {
  components: {
    HmHeader,
  }
}
```
{% endnote %}


## 2. 全局注冊
<font color="	#46A3FF" size="5">**全局注冊**</font>就是我們在 `main.js` 注冊組件，那個組件是所有的頁面都可以使用的。

比如我們要加入一個按鈕的組件

1. 寫一個 `HmButton.vue` 的文件

```vue
<template>
  <div>
    <button class="hm-button">點擊我</button>
  </div>
</template>

<script>
export default {
}
</script>


<style>
</style>

```
<br>

2. 在 `main.js` 引入組件，並且進行全局註冊 `component('組件名', 組件對象 )`

```js
import HmButton from '@/components/HmButton.vue'

Vue.component('useHmButton', HmButton)
```
<br>

3. 在需要的組件/頁面裏使用 `<useHmButton>`：

- `HmHeader.vue`

```html
<template>
  <div class="hm-header">我是頭部
    <useHmButton></useHmButton>
  </div>

</template>
```

![](/img/Vue/Vue-10-3.png) 
<br>


這樣就完成了組建的基本創建和使用了。
<br>