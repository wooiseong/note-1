---
title: Javascript-16-監聽風雲（二）事件對象和常見屬性
excerpt: 本篇討論事件對象的基本概念，包括其屬性，以及應用的場景。
tags: [Javascript, API, DOM, EventListener] 
categories: [Javascript]
date: 2025-01-10 15:14:46
---

還沒看過事件監聽介紹的朋友可以先看看上一篇 [Javascript-15-監聽風雲（一）什麽是事件監聽](https://wooiseong.vercel.app/2025/01/09/JS-15-eventIntro/)

## 1.1. 事件對象
給一個DOM元素綁定事件時，事件觸發時的相關信息都在事件對象裏。

我們可以通過這些信息，對發生的事件作出更好的應對。


### 1.1.1. 鍵盤事件
舉例來説，我們在一個論壇，編輯文本后，鍵盤敲擊 `Shift`，文本是不會提交的，

唯有敲擊 `Enter`， 才會讓文本提交上去。

Javascript是如何分辨我們`keydown` 事件用了什麽鍵呢？

```html
<body>
  <input type="text" class="texting" placeholder="請輸入">

  <script>
    const input = document.querySelector('input')

    //按下按鍵
    input.addEventListener('keydown', function(e){
      console.log(e)
    })
  </script>
</body>
```

事件綁定的函數，是有參數的。

第一個參數就是<font color="#46A3FF">**事件對象**</font>，我們通常命名為 `event`, `ev` 或者 `e`。

如果我按 `Shift`, 再按 `Enter`

![](/img/JS/JS-16-1.png) 

事件對象有一個屬性 `key`， 裏面就有我們打印的鍵值。

我們獲取 `key`屬性的值，只需要 `e.key` 即可。

除了這個屬性，還有 `isTrust`， `altKey` 等等屬性可以參考。


### 1.1.2. 鼠標事件
還有一個常常使用對象屬性的事件是鼠標事件。

如果我們三個HTML標簽，分別是 `<p>` ， `<span>` ， `<a>` ， 請問我怎麽知道我點了哪一個呢？

```html
<body>
  <div>
    <p>我是p</p>

    <span>我是span</span>
    <br>
    <br>
    <a href="#">我是a</a>
  </div>

</body>
```

![](/img/JS/JS-16-2.png) 
<br>

我們可以用事件屬性的 `e.target.tagName` ，返回一個HTML名字的字符串。

```html
<body>
  <div>
    <p>我是p</p>

    <span>我是span</span>
    <br>
    <br>
    <a href="#">我是a</a>
  </div>

  <script>
    const div = document.querySelector('div')

    // 鼠標點擊事件
    div.addEventListener('click', function(e){
      console.log(e.target.tagName)
    })
  </script>
</body>
```
![](/img/JS/JS-16-3.png) 

當我們依次點擊三個標簽時，就會返回HTML標簽字符串，

這樣我們就知道我們的鼠標到底點擊哪一個，

點擊對的才會執行綁定的函數。
<br>

{% note warning %}
`e.target.tagName` 返回的是大寫的HTML標簽名稱，如 `DIV`， `SPAN`， `A`。

對比時要注意。
{% endnote %}
