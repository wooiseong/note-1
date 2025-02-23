---
title: Vue-5-計算屬性
excerpt: 本篇會介紹Vue的計算屬性，可以幫助我們處理數據處理相關業務。
tags: [Vue, computed]
categories: [Vue]
date: 2025-02-21 21:40:48
---

## 1. 計算屬性 (computed) 介紹 
計算屬性是Vue的一個專門用來計算數據的屬性。當依賴的值發生變化，會自動重新計算。
<br>

我們在 `computed: {}` 裏面可寫我們用於結算的屬性，比如求年齡的總數，`totalAge` ，

用插值表達式放在頁面中以表運算結果。
<br>

```html
<body>  
  <div id="app">
    <div>{{ totalAge }}</div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      list: [
        {name: 'A', age: 20},
        {name: 'B', age: 100},
      ] 
    },
    computed: {
      totalAge(){
        return this.list.reduce((prev,current) => prev + current.age, 0)
      }
    }
  })
  </script>
<body>
```

{% note danger %}
和 `methods :{}` 提供的函數不同，在標簽中不能寫 `totalAge()` ，不然不能使用。 

直接寫函數名 `totalAge` 即可。
{% endnote %}
<br>

可是這個寫法，其實是計算屬性的簡寫，因爲計算屬性<font color="#46A3FF" size="5">**只可以讀取訪問，不能修改**</font>。

什麽意思呢？拿上面的例子說，我們想要把年齡總數改成 `0` ， 寫一個函數 `changeAge` 放在 `methods` 直接修改是無法做到的。

```html
<body>  
  <div id="app">
    <button @click="changeAge">年齡總數歸零</button>
    <div>{{ totalAge }}</div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      list: [
        {name: 'A', age: 20},
        {name: 'B', age: 100},
      ] 
    },
    methods: {
      changeAge(){
        this.totalAge = 0 
      }
    },
    computed: {
      totalAge(){
        return this.list.reduce((prev,current) => prev + current.age, 0)
      }
    }
  })
  </script>
<body>
```

![](/img/Vue/Vue-5-1.png) 
<br>

如果我們想要改寫，就必須要寫計算屬性的完整寫法：`get()` 和 `set()`。

## 2. 計算屬性完整寫法

```js
  const app = new Vue({
    el: '#app',
    data: {
      list: [
        {name: 'A', age: 20},
        {name: 'B', age: 100},
      ] 
    },
    methods: {
      changeAge(){
        this.totalAge = 0
      }
    },
    computed: {
      totalAge: {
        get(){
          return this.list.reduce((prev,current) => prev + current.age, 0)
        },  
        set(value){
          console.log(value)
          this.list[0].age = value
          this.list[1].age = value
        }
      }
    }
  })
```

![](/img/Vue/Vue-5-2.png) 
<br>

`get()` 就是我們在簡寫的計算屬性裏的計算邏輯， 
<br>

`set()` 寫的是我們想要修改的修改邏輯，這個函數有一個形參 `value`，

這個 `value` 是我們想要更改的值，我們在 `methods` 已經定義想改的值 ` this.totalAge = 0` ,打印的 `value` 的值是 `0` 。
<br>
<br>

啊爲什麽我們不直接在 `methods` 裏面寫這些方法呢？<font size="5">😶</font>

在 `methods` 不也可以執行年齡總數計算？
<br>
關於這兩者的不同，會留到下一篇再討論，敬請期待。
<br>
<br>