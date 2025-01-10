---
title: Javascript-15-監聽風雲（一）什麽是事件監聽
excerpt: 本篇開始討論發生在網頁上的事件以及JS是如何監聽的。
tags: [Javascript, API, DOM] 
categories: [Javascript]
date: 2025-01-09 21:12:39
---

## 1.1. 前言
不知道大家有沒有用過google translate服務，

當我們在左邊輸入一個語言，右邊會自動“出現”翻譯後的語言。

就好像有一個人在那個輸入框後面“接收”和“回饋”我們的操作。

![](/img/JS/JS-15-1.png)

Javascript就是那個“人”，監聽事件，作出反應。

什麽是事件呢？

就是在瀏覽器在運行時發生的動作或者狀態，比如點擊、滑鼠移動、鍵盤輸入等等。

什麽是監聽事件呢？

Javascript是事件驅動的程式語言。

當瀏覽器載入網頁時。Javascript的很多針對事件作出的相應的程式碼也會被讀取，但不會執行。直到事件被觸發，才會執行。

事件監聽就是讓Javascript檢測是否有事件觸發，

一旦有事件觸發，就會觸發對應的程式碼。

比如上面google translate，當輸入事件被觸發時，

一個向後臺發送的翻譯請求會被觸發和執行。

結果再被返回，然後顯示在右邊的輸入框。

## 1.2. 事件監聽的語法
事件監聽的語法如下：

```javascript
DOM對象.addEventListener(事件類型, 執行函數);
```

事件監聽三要素：
- 事件源： 被事件觸發的DOM元素
- 事件類型： 用什麽方式觸發，比如點擊click、滑鼠移動mousemove
- 事件調執行數：觸發后要做什麽事
<br>
<br>

那我們不妨來試試看吧！

```html
<body>
  <button class="btn">123</button>

  <script>
    const btn = document.querySelector('.btn')
    btn.addEventListener('click', function(){
      alert('CLICK ME')
    })
  </script>
</body>
```

![](/img/JS/JS-15-2.png)

在上面的代碼：
- 事件源： `btn`成爲監聽對象
- 事件類型： `btn`被點擊
- 事件調執行數：觸發后調用封裝 `alert` 了的匿名函數

{% note info %}
同一個DOM元素可以被綁定多個事件監聼。
{% endnote %}

## 1.3. 事件類型
根據事件類型，可以分爲4類：

### 1.3.1 鼠標事件
- click 點擊
- mouseenter 鼠標經過
- mouseleave 鼠標離開

點擊我們討論過了，那麼mouseenter和mouseleave呢？

顧名思義分別就是鼠標懸停在事件源和離開事件源會觸發的情況。

```html
<body>
  <button class="btn">123</button>

  <script>
    const btn = document.querySelector('.btn')

    //鼠標經過
    btn.addEventListener('mouseenter', function(){
      console.log('鼠標進來')
    })

    //鼠標離開
    btn.addEventListener('mouseenter', function(){
      console.log('鼠標離開')
    })
  </script>
</body>
```
鼠標進來
![](/img/JS/JS-15-3.png)

鼠標離開
![](/img/JS/JS-15-4.png)

<br>

### 1.3.2 焦點事件
- focus 獲得焦點
- blur 失去焦點

在輸入文本框，我們點擊時，就是獲得焦點，光標出現。

當我們點擊其他地方時，光標消失，失去焦點。

```html
<body>
  <input type="text" class="texting" placeholder="請輸入">

  <script>
    const texting = document.querySelector('.texting')

    //獲得焦點
    texting.addEventListener('focus', function(){
      console.log('獲得焦點')
    })

    //失去焦點
    texting.addEventListener('blur', function(){
      console.log('失去焦點')
    })
  </script>
</body>
```
點擊文本框
![](/img/JS/JS-15-5.png)

點擊其他地方
![](/img/JS/JS-15-6.png)

這類事件比較常用在輸入文本時，

獲得焦點讓文本框有一些樣式，告知用戶正在輸入。
<br>

### 1.3.3 鍵盤事件
- keydown 鍵盤按下
- keyup 鍵盤擡起

鍵盤事件，主要監聽鍵盤的按鍵操作。就像我們打游戲時，

按 `Enter` 不放就會凝聚炮彈，按得越久，炮彈威力越強。

放開 `Enter` 就會發射炮彈。

```html
<body>
  <input type="text" class="texting" placeholder="請輸入">

  <script>
    const texting = document.querySelector('.texting')

    //獲得焦點
    texting.addEventListener('keyup', function(){
      console.log('我按鍵了')
    })

    //失去焦點
    texting.addEventListener('keydown', function(){
      console.log('我放開鍵了')
    })
  </script>
</body>
```

![](/img/JS/JS-15-7.png)

當我按著 `Enter` 不放，就會一直觸發 `keydown` 事件，直到我放開爲止。

在放開時，會觸發 `keyup` 事件。
<br>

### 1.3.4 文本事件
- input 用戶輸入

主要是監聽用戶輸入内容事件。

在 1.1前言 提到的google translate例子

使用的就是文本輸入事件。

這也是一個表單很常用的事件，因爲我們可能會需要監聽用戶輸入。

對用戶輸入的内容進行校驗。

我們監聽文本事件時，也可以使用 `input.value` 獲取用戶輸入的内容。

用 `input.value.length` 獲取用戶輸入内容長度。

```html
<body>
  <input type="text" class="texting" placeholder="請輸入">

  <script>
    const texting = document.querySelector('input')

    //獲得焦點
    texting.addEventListener('input', function(){
      console.log(input.value)
    })
  </script>
</body>
```

![](/img/JS/JS-15-8.png)

只要有輸入内容事件，每個字都會被監聽到。
<br>

{% note danger %}
盡量把 `input` 事件綁定在HTML標簽 `<input>` 的DOM元素上，而不是className或者id獲取的DOM元素，

因爲`input.value` 只可以在 `<input>`獲取的DOM元素訪問到。
{% endnote %}