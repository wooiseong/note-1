---
title: Javascript-28-this的傳説(一) 什麽是this
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
  console.log(window === this)
}
fn()
```

![](/img/JS/JS-27-1.png) 

當我們打印 `this`， 它的内容和 `window` 是一樣的。

因爲 `this` 指向函數的調用者。

我們提到過 `window` 是頂級對象，所以實際寫法是 `window.fn()`。

所以是 `window` 調用這個函數， `this` 指向 `window` 。
<br>

## 2.1. this的指向
我們接下來就學習各種情況下的 `this`

### 2.1.1. 普通函數
上面的 `input` 使用的是普通寫法的匿名函數，我們可以打印它的 `this` 看看：

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

直接寫 `this.style.backgroundColor = 'red'`。

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
<br>

### 2.1.2. 箭頭函數
我們把上面的 `input` 的匿名函數改寫成箭頭函數看看：
```html
<body>
  <input type="text" class="texting" placeholder="請輸入">

  <script>
    const input = document.querySelector('input')

    //按下按鍵
    input.addEventListener('keydown', () => {
      console.log(this)
    })
  </script>
</body>
```

![](/img/JS/JS-27-5.png) 

怎麽調用者變成 `window` 了， 不是應該是 `input` 嗎<font size="5">??? 🧐</font> 
<br>

這是因爲箭頭函數不會創建自己的 `this`， 它會沿用自己的作用域鏈上一層的 `this`。

`input` 外的上一層作用域就是 `window`，所以 `this` 指向 `window`。
<br>

我們可以拿嵌套的函數來試試看：
```javascript
const obj = {
  sayHi: function(){
    const count = () => {
      console.log(this)
    }
    count()
  }
}

obj.sayHi() // {sayHi: ƒ}
```

首先，`count` 是匿名函數，所以沒有作用域。

就像作用域鏈機制一樣，會往上一層找，找到 `sayHi` 這個函數。

`sayHi` 是普通函數，所以有作用域，是 `obj` 調用的，`sayHi` 的 `this` 是 `{sayHi: ƒ}`。

所以 `count` 的 `this` 才會是 `{sayHi: ƒ}` 。


### 2.1.3. 嚴格模式下的this
開發過程中，Javascript提供了嚴格模式的環境，只要寫上 `use strict` 即可。

在嚴格模式下，原來指向 `window` 的 `this` 會變成 `undefined` 。

```javascript
'use strict'

function fn(){
  console.log(this)
}

fn() // undefined
```

### 2.1.4. this的應用場景
從上述的討論，我們知道箭頭函數的 `this` 是沿用上一層的，

在需要使用上層 `this` 的地方很好用，

但在綁定事件的函數，需要 `this` 獲取被綁定的DOM元素，

箭頭函數就不適合。

在往後的學習，我們會慢慢掌握 `this` 的使用時機。
<br>

如果 `this` 真的需要改變，有方法嗎？

答案是有的，我們會在下一篇文章學習,敬請期待。
<br>
