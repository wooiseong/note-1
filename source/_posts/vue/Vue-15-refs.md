---
title: Vue-15-獲取和控制DOM的法寶： ref 和 $refs
excerpt: 本篇會介紹在Vue獲取和控制DOM元素： `ref` 和 `$refs` 。
tags: [Vue， refs， $refs]
categories: [Vue]
date: 2025-03-07 10:40:12
---

還記得我們曾經學習過JS，要用 `document.querySelector` 獲取HTML的DOM元素嗎？

不記得傳送門： [Javascript-11-獲取DOM元素](http://localhost:4000/2025/01/05/JS-11-querySelector/)

Vue也有方法可以獲取DOM元素哦！這就是 `ref` 和 `$refs` 。

你可能會說我直接用JS的老方法不就行了？但這種方法會產生問題：

假如父組件和子組件有相同類名： chatBox，我們在子組件獲取元素就會有問題，因爲

`document.querySelector` 的查找範圍是<font size="5" color="#46A3FF">**整個頁面，父組件和子組件都是查找範圍。**</font>

因此我們可以藉助 `ref` 和 `$refs` 幫助我們獲取指定的DOM元素：

1. 把 `ref="標簽名"` 加在想要獲取DOM的HTML標簽

2. 在 `script` 的 `mounted` 函數可以用 `this.$refs.標簽名` 操作DOM元素

```vue
<template>
  <div>
    <div ref="chatBox">I am chatBox</div>
  </div>
</template>

<script>

export default {
  mounted(){
    console.log(this.$refs.chatBox)
  }
}
</script>
```

![](/img/Vue/Vue-15-1.png) 
<br>

{% note info %}
因爲DOM渲染完成是 `mounted` 之後，因爲獲取和操作DOM一般都是在 `mouunted` 這個函數裏面。
{% endnote %}





