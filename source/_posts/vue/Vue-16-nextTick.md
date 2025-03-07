---
title: Vue-16-$nextTick ———— 爲了Vue異步更新而生
excerpt: 本篇會介紹$nextTick在DOM更新後發揮的作用。
tags: [Vue， $nextTick]
categories: [Vue]
date: 2025-03-07 12:46:45
---

我們上一篇提到可以用 `ref` 和 `$refs` 操作DOM元素，我們今天就來看一個經常遇到的問題：

我們想要在點擊按鈕後，出現輸入框，然後控制這個DOM顯示聚焦(focus)。

![](/img/Vue/Vue-16-1.png) 
<br>

```vue
<template>
  <div>
    <div v-if="isShow">
      <input ref="textInput" type="text" placeholder="請填寫">
    </div>

    <div v-else>
      <button @click="handleInput">點擊打開</button>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      isShow: false
    }
  },
  methods: {
    handleInput(){
      this.isShow = true
      this.$nextTick(() => {
        this.$refs.textInput.focus()
      })
    }
  }
}
</script>
```
<br>

我們可以發現點擊后，并沒有聚焦，還報錯了。

![](/img/Vue/Vue-16-2.png) 
<br>

顯然是找不到 `textInput` 的DOM。也就是 `v-if` 條件滿足了輸入框顯示條件，但DOM好像沒有生成。

這是因爲Vue爲了節省性能，是異步更新DOM的。就是説 `isShow` 的值改變，DOM也不會馬上就更新，

因此你要在 `handleInput` 事件馬上執行DOM操作時會報錯。
<br>

我們可以用 `this.$nextTick` 幫助我們解決這問題：

```js
handleInput(){
  this.isShow = true
  this.$nextTick(() => {
    this.$refs.textInput.focus()
  })
}
```

這個語法會等DOM更新後才執行裏面的代碼。


![](/img/Vue/Vue-16-3.png) 
<br>

當然，要解決問題不是只有一個方法，我們也可以直接把代碼放在異步任務裏面，也就是 `setTimeout` ， DOM更新完一秒后執行輸入框聚焦的任務。

但缺點就是，這樣會延遲一秒，還是 `this.$nextTick` 能更精準一些。
<br>