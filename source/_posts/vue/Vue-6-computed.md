---
title: Vue-6-computed和methods，我該用哪一個？
excerpt: 本篇會分析 `computed` 和 'methods' 的差別和時機。
tags: [Vue， computed， methods]
categories: [Vue]
date: 2025-02-24 14:53:52
---

我們學習了 `computed`和 `methods` 的使用，兩者都可以做出計算的處理，

比如 `computed` 可以纍加購物車的物品總數， `methods` 同樣可以寫一個方法來執行這個功能。
<br>

差別在哪裏呢？
<br>

`computed`是封裝數據的處理，并得到一個結果，

但是 `methods` 的作用比較像是給 `app` 這個實例提供方法，用以處理業務邏輯。

比如我要點擊一個按鈕，然後執行一個方法，這個方法就可以寫在 `methods` 裏面。

因此， `methods` 通常要注冊事件。

而且，`methods` 在插值表達式需要以 `方法()` 寫； `computed`
<br>

另一個差別就是計算屬性會緩存計算的結果，再次使用就直接讀取緩存。只有依賴數據有變化才會自動重新計算再緩存。

我們可以舉個例子:
<br>

在 `computed` 計算年齡：
```html
<body>  
  <div id="app">
    <div>計算的年齡：{{ totalAge }}</div>
    <div>再次計算的年齡：{{ totalAge }}</div>
    <div>三次計算的年齡：{{ totalAge }}</div>
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
    },
    computed: {
      totalAge(){
        console.log('我執行一次')
        return this.list.reduce((prev,current) => prev + current.age, 0)
      }
    }
  })
  </script>
</body>
```
![](/img/Vue/Vue-6-1.png)
<br>

在 `methods` 計算年齡：
```html
<body>  
  <div id="app">
    <div>計算的年齡：{{ totalAgeFn() }}</div>
    <div>再次計算的年齡：{{ totalAgeFn() }}</div>
    <div>三次計算的年齡：{{ totalAgeFn() }}</div>
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
      totalAgeFn(){
        console.log('我執行一次')
        return this.list.reduce((prev,current) => prev + current.age, 0)
      }
    },
    computed: {
    }
  })
  </script>
</body>
```

![](/img/Vue/Vue-6-2.png)
<br>

大家可以看到 `computed` 只會執行 `totalAge` 一次， 得到的結果會被緩存，有被引用時就不用重新執行，

反之，如果把執行年齡計算的函數 `totalAgeFn()` 寫在 `methods` ， 只要在插值表達式引用一次， 就會執行一次。

試想想：如果我們有龐大的資料，需要頁面被反復引用，不是要重複執行非常多次？
<br>

<font size="4">因此，遇到需要計算的業務，我們可以直接給 `computed` 做，讓 `methods` 專心處理其他業務邏輯。</font>
<br>
