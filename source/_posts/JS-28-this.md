---
title: Javascript-28-this的傳説(一)
excerpt: 本篇討論Javascript環境對象的概念，以及this的運用。
tags: [Javascript, this] 
categories: [Javascript]
date: 2025-01-15 15:47:01
---

## 1.1. 前言
我們在之前的監聽事件學習了事件執行的函數内部有一個事件對象 `e`

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

函數除了綁定事件有事件對象，

其實函數内部的環境也有一個環境對象：<font color="#46A3FF">**`this`，代表著當前函數運行時所處的環境**</font>。
<br>
我們可以嘗試打印一個函數的 `this` ：

```javascript
function fn(){
  console.log(this)
  console.log(window)
}
fn()
```

![](/img/JS/JS-27-1.png) 

當我們打印 `this`， 它的内容和 `window` 是一樣的。

因爲 `this` 指向函數的調用者。

我們提到過 `window` 是頂級對象，所以實際寫法是 `window.fn()`。
<br>

如果是上面的 `input` 的例子呢？

```html
<body>
  <input type="text" class="texting" placeholder="請輸入">

  <script>
    const input = document.querySelector('input')

    //按下按鍵
    input.addEventListener('keydown', function(){
      console.log(this)
    })
  </script>
</body>
```

![](/img/JS/JS-27-2.png) 

`this` 就會指向函數的調用者，也就是 `input` 元素。
<br>
{% note info %}
`this` 的粗略判斷原則就是 【誰調用我，我就指向誰】 
{% endnote %}
<br>

因此，我們如果想要改 `input` 的樣式 ，可以不用寫 `input.style.backgroundColor = 'red'`，

可以直接寫 `this.style.backgroundColor = 'red'`。

```html
<body>
  <input type="text" class="texting" placeholder="請輸入">

  <script>
    const input = document.querySelector('input')

    //按下按鍵
    input.addEventListener('keydown', function(){
      this.style.backgroundColor = 'red'
    })
  </script>
</body>
```
打字前：
![](/img/JS/JS-27-3.png) 
<br>

打字后：
![](/img/JS/JS-27-4.png) 

