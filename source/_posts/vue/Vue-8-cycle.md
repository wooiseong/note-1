---
title: Vue-8-Vue生命周期
excerpt: 本篇介紹vue的生命周期和每個階段發生的事，讓我們更好掌握Vue引擎執行原理。
tags: [Vue， cycle]
categories: [Vue]
date: 2025-03-02 20:05:49
---


## 1. 前言
在我們創建一個Vue實例，直到銷毀是由一個過程的，這個過程被稱爲<font color="	#46A3FF">**Vue生命周期。**</font>

爲什麽我們需要知道Vue生命周期呢？因爲我們執行代碼，需要配合Vue生命周期來進行，這樣才能讓代碼在對應的階段執行。

Vue生命周期過程中，會運行一些函數，被稱爲生命周期鈎子，在鈎子裏運行我們的代碼。

## 2. 生命周期鈎子 

![](/img/Vue/Vue-8-1.png) 

我先簡略説解釋一下：

- `beforeCreate` 和 `created`

就是Vue實例還沒有被創建前，和創建後。

一般我們都是在 `created` 發送初始化渲染請求，請求後臺使用者的 `token`， 購買的商品列表等等。

- `beforeMount` 和 `mounted`

DOM渲染前和後。

我們需要在 `mounted` 才可以操作頁面的DOM。

- `beforeUpdate` 和 `updated`

修改數據前和後。

我們在 `updated` 這個階段，修改數據后，視圖會更新。一般這個部分會寫入數據更新後需要執行的代碼。


- `beforeDestroy` 和 `destroyed`

Vue實例銷毀前和後。

我們在 `beforeDestroy` 可以釋放Vue以外的資源，比如清除定時器和延時器。
<br><br>


## 3. 生命周期鈎子例子 
我們可以擧個例子來解釋生命周期函數
<br>

先從前四個生命周期來看，我們的數據 `this.count` 在 `created` 才可以被訪問到，

我們想要獲得頁面的DOM，在 `beforeMounted` ， DOM還沒有被渲染完成前，只會看到 `{{ count }}` ，直到 `mounted` DOM渲染完成，才會把 `{{ count }}` 替換成 `this.count` 的值 `100`。

```html
<body>  
  <div id="app">
    <button @click="count--">-</button>
    <span>{{ count }}</span>
    <button @click="count++">+</button>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      count: 100,
      title: '計數器'
    },
    beforeCreate(){
      console.log('beforeCreate', this.count)
    },
    created() {
      console.log('created', this.count)
    },
    beforeMount() {
      console.log('beforeMount', document.querySelector('span'))
    },
    mounted() {
      console.log('mounted', document.querySelector('span'))
    },
  })
  </script>
</body>
```

![](/img/Vue/Vue-8-2.png) 
<br>

接下來我們看後兩個生命周期，我們在 `beforeUpdate` 修改數據，其實只有 `data` 屬性的數據被修改，頁面的數字還是 `100` ，在 `updated` 才會看到數據已經被修改，但頁面還沒有更新，直到 `updated` 頁面才會更新。

至於 `beforeDestroy` 和 `destroyed` ，我們關閉頁面就看不到效果了。我們可以藉助 `app.$destroy()` 在調試窗口直接運行，就可以看到了。

```js
  <script>
  const app = new Vue({
    el: '#app',
    data: {
      count: 100,
      title: '計數器'
    },
    beforeCreate(){
      console.log('beforeCreate', this.count)
    },
    created() {
      console.log('created', this.count)
    },
    beforeMount() {
      console.log('beforeMount', document.querySelector('span'))
    },
    mounted() {
      console.log('mounted', document.querySelector('span'))
    },
    beforeUpdate() {
      console.log('beforeUpdate', document.querySelector('span').innerHTML)
    },
    updated(){
      console.log('updated', document.querySelector('span').innerHTML)
    },
    beforeDestroy() {
      console.log('beforeDestroy')
    },
    destroyed() {
      console.log('destroyed')
    }
  })
  </script>
```

![](/img/Vue/Vue-8-3.png) 
<br>

現階段各位對於什麽時候運用生命周期函數可能覺得混亂，這是正常的，等各位開始寫Vue專案時就會清晰很多了。
<br>



