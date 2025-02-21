---
title: Vue-3-基礎語法（二）
excerpt: 本篇會延續上一篇，介紹Vue的基本語法（v-bind, v-for等等）。
tags: [Vue]
categories: [Vue]
date: 2025-02-13 15:31:04
---

本篇延續上一篇 [Vue-2-基礎語法（一）](https://notebook-olive.vercel.app/2025/02/06/vue/Vue-2-Command1/)，介紹更多Vue的基本語法。
<br>

### 1.6. v-bind
這個指令用來動態設置html的標簽屬性，比如 `src` ， `title` 和 `url` 等等。

```html
<body>
  <div id="app">
    <a v-bind:href="url1">google鏈接</a>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/
  vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      url1: 'https://www.google.com/'
    }
  })
  </script>
</body>
```

`v-bind` 後面跟上要綁定的 屬性，比如 `url` ， 後面用 `=` 賦值就可以了，寫法就變成： `v-bind: url="url1"`

我們也可以用 `:` 縮寫成： `:url="url1"`
<br>

我本人比較推薦縮寫的語法，因爲比較簡潔，而且對我來説 `:` 和 `=` 放在一起會感覺很混亂。
<br>

### 1.7. v-for
這個是vue系列很常用的指令，效果和 `forEach` 和 `map` 類似，就是遍歷循環，渲染在頁面上。

```html
<body>
  <div id="app">
    <ul>
      <li v-for="(item,index) in list" :key="item.id">{{ item }}</li>
    </ul>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/
  vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      list: [
        {id: 1, name: 'a'},
        {id: 2, name: 'b'},
        {id: 3, name: 'c'}
      ]
    }
  })
  </script>
</body>
```

![](/img/Vue/Vue-3-1.png) 

`v-for` 有兩個參數，第一個是數組的内容，第二是數組的索引號。

如果不需要索引號就不用聲明出來。
<br>

{% note info %}
你知道爲什麽 `v-for` 後面通常會綁定 `item.id` 為 `key` 的值嗎？

因爲 `v-for` 默認原地修改元素。假如 上面的第一個元素 `a` 有一個樣式 `color: red`，

當這個 `a` 被刪除，其實`color: red` 還是被保留下來給第二個 `b`， 因爲沒有唯一標識。所以VueJS不知道這個樣式是屬於誰的，被刪掉的元素跟這個沒有關聯。

因此，一般渲染都需要把貨品的 `id` 綁定成 `key` 。

{% endnote %}
<br>

### 1.8. v-model
這個是Vue和Angular才有的功能，也是Vue真香的原因之一：
<font size="5" color="	#46A3FF">**實現雙向數據綁定。**</font>
<br>

`v-model` 給表單元素使用的：輸入的内容可以在 `data` 接收； `data` 的數據可以直接渲染在表單元素上。


```html
<body>
  <div id="app">
    <input type="text" v-model="name" @input="text()">
  </div>

  <script src="https://cdn.jsdelivr.net/npm/
  vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      name: 'haha' 
    },
    methods: {
      text(){
        console.log(this.name)
      }
    }
  })
  </script>
</body>
```
<br>

我們在 `data` 設定 `name` 的值是 `haha` ， 輸入框的值就會變成 `haha` ， 這是數據綁定在表單元素上；

![](/img/Vue/Vue-3-2.png) 
<br>

我們更改表單數據，比如在 `haha` 後面加 `1` ，也可以監聽到 `name` 的值，這是表單收集的數據被及時接收。

![](/img/Vue/Vue-3-3.png)