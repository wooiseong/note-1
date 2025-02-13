---
title: Vue-2-基礎語法（一）
excerpt: 本篇會介紹Vue的基本語法（插值表達式，v-show,v-bind等）。
tags: [Vue]
categories: [Vue]
date: 2025-02-06 14:43:12
---

這篇會開始學習一些基本的Vue語法：

## 1. Vue語法

### 1.1. 插值表達式
語法跟我們學Javascript的模板字符串類似，就是在網頁渲染變量的值。

語法：<font size="5">`{{ 表達式 }}`</font>

```html
<body>  

  <div id="app">
    {{ count }}
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16
  /dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      count: 234
    }
  })
  </script>
</body>
```

{% note danger %}
使用插值表達式有三個注意事項：
- 數據必須聲名存在
- 數據不支持語句，如 `if`，`while` 等等
- 標簽屬性，如 `title` 不可以使用插值表達式，比如
```javascript
<p title="{{ titleForm }}"></p>
```

{% endnote %}

### 1.2. v-html
這個Vue指令類似 `innerHTML` 的效果，可以安插html標簽。
```html
<body>  

  <div id="app">
    <div v-html="msg"></div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16
  /dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      msg: `<a href="https://www.google.com/">Google連接</a>`
    }
  })
  </script>
</body>
```
![](/img/Vue/Vue-2-1.png) 

### 1.3. v-show和v-if
從字面上看就是滿足條件就會顯示，不滿足條件就會隱藏，差異在於 `v-show` 只是隱藏，而 `v-if` 是直接移除標籤。

```html
  <div id="app">
    <div v-show="flag">v-show展示</div>
    <div v-show="flag2">v-show隱藏</div>
    <div v-if="flag">v-if展示</div>
    <div v-if="flag2">v-if隱藏</div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      flag: true,
      flag2: false
    }
  })
  </script>
</body>
```

![](/img/Vue/Vue-2-2.png) 

我們可以看到 `v-show` 如果值是 `false` ，只是在標簽添加 `display="none"` 的屬性，標簽還在；

`v-if` 如果條件不滿足，就是整個標籤都不會被渲染。

性能來説，肯定是 `v-if` 比較好，適合那些不用頻繁切換場景，比如點擊登入切換表單元素；

`v-show` 適合頻繁切換的場景，比如點擊分類頁面就換成對應的商品。


### 1.4. v-else和v-else-if
通常是配合 `v-if` 一起使用的。和我們學習的 `if` 和 `else` 類似，

`v-else` 應用在只有兩種情景，多種就用 `v-else-if`

我們可以試試看：
```html
<body>  
  <div id="app">
    <p v-if="gender===1">男生</p>
    <p v-else>女生</p>

    <p v-if="score >=80">分數甲等</p>
    <p v-else-if="score >=50">分數乙等</p>
    <p v-else>分數丙等</p>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      gender: 2,
      score: 50
    }
  })
  </script>
</body>
```
![](/img/Vue/Vue-2-3.png) 


### 1.5. v-on
作用是綁定事件，就是我們學習的 `addEventListener`， 我們可以直接在html標簽綁定事件，

然後在Vue的methods提供函數。

語法：<font size="5">`v-on:事件名 = "函數"`</font>

`v-on` 可以直接簡寫成<font size="5"> `@事件名="函數"`</font>
<br>

```html
<body>  
  <div id="app">
    <div @click="addCount">{{count}}</div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      count: 0
    },
    methods: {
      addCount(){
        this.count++
      }
    }
  })
  </script>
</body>
```

![](/img/Vue/Vue-2-4.png) 

我們點擊數字兩次， `click` 事件被觸發兩次，所以也就會觸發 `addCount()` 兩次。
<br>

{% note info %}
在操作 `data` 的變量時，我們不能直接寫`count`，就變成全局變量了，就會找不到了。

我們要的是實例的 `count` ， `this` 指向實例 `app` 本身，所以我們需要寫成 `this.count`

直接寫 `app.count` 當然也是可以，但萬一我們的這個方法不是只給這個實例使用，換了實例就要重寫成另一個調用這個函數的實例，就不夠靈活了。
{% endnote %}
<br>

#### 1.5.1. 傳參數
如果我們需要傳參數，很簡單，只需要在綁定事件的函數名後面括號寫參數，像一般函數傳參即可。
<br>

例如上面的例子，可以成爲 `addCount(5)` 傳入數字 `5`

```html
<body>  
  <div id="app">
    <div @click="addCount(5)">{{count}}</div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      count: 0
    },
    methods: {
       addCount(num){
        app.count = app.count + num
      }
    }
  })
  </script>
</body>
```
<br>
<br>
關於vue指令就先分享這些，下一章會分享剩下的指令，敬請期待吧!
<br>
<br>