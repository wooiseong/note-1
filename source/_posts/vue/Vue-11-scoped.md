---
title: Vue-11-組件樣式衝突
excerpt: 本篇解釋發生組件之間樣式衝突的問題，以及解決方案。
tags: [Vue， scoped]
categories: [Vue]
date: 2025-03-05 15:29:24
---

我們在上一篇[Vue-10-組件化開發](https://notebook-olive.vercel.app/2025/03/05/vue/Vue-10-components/)討論了局部和全局注冊組建的方式。

如果我們想要在 `HmButton` 加上紅色的邊框，我們如果直接在 `<style>` 的 `div` 寫樣式寫會怎麽樣呢？

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
div {
  border: 1px solid red;
}
</style>

```

我們對 `div` 標簽的樣式進行修改，結果是這樣的：

![](/img/Vue/Vue-11-3.png) 

我們對`HmButton` 的 `div` 的樣式，是默認作用于全局的，也會影響其他的組件和頁面，

因此，我們可以在組件的 `<style>` 加上 `scoped` ， 就可以讓樣式作用于當前組件。

```html

<style scoped>
div {
  border: 1px solid red;
}
</style>
```

![](/img/Vue/Vue-11-2.png) 
<br>

那麽原理是什麽呢？

我們加上 `scoped` 之後，當前的所有元素都會加上一個自定義屬性 `data-v-hash` 值（紅色箭頭）

在 css 選擇器後面會被自動處理，添加屬性選擇器。這樣就可以避免組件和頁面css選擇器相同造成衝突了。

![](/img/Vue/Vue-11-4.png)

