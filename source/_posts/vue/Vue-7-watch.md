---
title: Vue-7-監聽的科技 ———— watch
tags: [Vue， watch]
categories: [Vue]
date: 2025-02-24 15:25:52
---

不知道各位有沒有用過Google translate的服務呢？

當我們在左邊輸入内容，右邊的翻譯就會即時出現。在Vue我們可以實現這個功能，不過需要藉助`watch`這個功能。

![](/img/Vue/Vue-7-1.png)

## 1. watch的概念
`watch` 是Vue提供的一個屬性，主要是爲了監視數據變化，

在出現變化後執行一些操作或者執行一些功能。

### 1.1. 監視簡單數據（簡寫形式）
我們可以用上面的即時翻譯做例子：

```html
<body>  
  <div id="app">
    <div>
      <div>我是原字</div>
      <input v-model="oriText" type="text">
    </div>
    <br>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      oriText: '',
    },
    watch: {
      'oriText' (newValue, oldValue){
        console.log(newValue, oldValue)
      }
    },
    methods: {
    },
  })
  </script>
</body>
```

![](/img/Vue/Vue-7-2.png)

`watch` 的屬性會變成函數名，有兩個形參，第一個是新值，第二是舊值，

因此，我們寫 `code` 四個字會逐字被打印，變成 `code cod`
<br>

{% note warning %}
在寫watch時，有一個點要注意：

- `watch` 裏面要監視的屬性必須和 `data` 聲明一樣
{% endnote %}
<br>

### 1.2. 監視複雜數據 （完整形式）
問題來了：如果我們要監視的屬性 `oriText` 裏面還有幾個屬性，

比如選擇翻譯的語言 `oriText.language`，

我們總不能一個個監視吧？我們就需要用完整形式來監視這類複雜數據了。
<br>

我們需要額外添加配置項： `deep`

```html
<body>  
  <div id="app">
    <div>
      <div>我是原字</div>
      <input v-model="oriText.text" type="text">
      <select v-model="oriText.language">
        <option value="EN">EN</option>
        <option value="CN">CN</option>
        <option value="FN">FN</option>
      </select>
    </div>
    <br>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

  <script>
  const app = new Vue({
    el: '#app',
    data: {
      oriText: {
        language: 'EN',
        text: ''
      },
    },
    watch: {
      oriText: {
        deep: true,
        handler(newValue, oldValue){
          console.log(newValue, oldValue)
        }
      }
    },
    methods: {
    },
  })
  </script>
</body>
```
<br>

當我們寫一個字，`c`
![](/img/Vue/Vue-7-3.png)
<br>


當我們選擇一個語言，`CN`
![](/img/Vue/Vue-7-4.png)

<br>

值得注意的是，我們在 `handler` 獲取的形參就只有新值一個了，舊值的值也和新值一樣。
<br>

如果我們想要在進入頁面時就立刻執行數據的監視，我們可以添加 `immediate: true` 這個選項。

```js
 watch: {
      oriText: {
        deep: true,
        immediate: true,
        handler(newValue, oldValue){
          console.log(newValue, oldValue)
        }
      }
    },
```
<br>

那這個 `watch` 通常的應用場景在哪裏呢？

在用戶瀏覽頁面，產生數據變化時，比如一個商品加入購物車，

這個 `watch` 就可以幫助我們即時知道購物車的變化，馬上執行相應的業務邏輯。
<br>