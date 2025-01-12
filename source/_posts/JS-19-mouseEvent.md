---
title: Javascript-19-監聽風雲（五）頁面加載，滾動事件
excerpt: 本篇討論除了前幾篇討論的事件， 這次討論頁面加載和滾動事件。
tags: [Javascript, API, DOM, EventListener， Scroll] 
categories: [Javascript]
date: 2025-01-12 16:36:45
---

我們前幾篇已經討論了常用的事件，

還有幾類事件可以幫助我們建立更强的網頁交互。

## 1.1. 頁面加載事件 
顧名思義就是加載外部的資源，比如圖片，

加載完畢就觸發的事件。

這個事件主要目的：
- 需要加載完資源，才可以觸發的事件，比如廣告
- 以前的代碼把 `<script>` 寫在 `<head>` 中， 裏面有DOM操作 （比如變字體顔色）會報錯， 因爲在 `<head>` HTML標簽還沒有被加載。
<br>

語法如下：

`window.addEventListener('load', function(){})`

這個事件綁定在 `window` 對象上。
```html
<body>
  <div>
    <img src="https://i.ibb.co/zVg9D8g/xiong-web-Logo.png" alt="">
  </div>

  <script>
    // 頁面加載事件
    window.addEventListener('load', function () {
      console.log('圖片加載了')
    })
  </script>
</body>
```

![](/img/JS/JS-19-1.png) 
<br>

如果我們還想要更早，在圖片等資源還沒有加載就觸發事件呢？

我們可以使用 `DOMContentLoaded`。

```html
<body>
  <div>
    <button>點擊我</button>
  </div>

  <script>
    // HTML文檔加載完后執行
    document.addEventListener('DOMContentLoaded', function () {
      const btn = document.querySelector('button')
      btn.addEventListener('click', function () {
        console.log('加載button了')
      })
    })
    
  </script>
</body>
```

頁面加載后，點擊按鈕:
![](/img/JS/JS-19-2.png) 
<br>


## 1.2. 頁面滾動事件 
超過頁面的網頁，右邊通常會有一個滾動條。

當我們拉著滾動條，網頁滾動時，我們可以觸發頁面滾動事件。

舉個例子，當我們把MOMO首頁滾動條往下拉時，

中間的搜索框會移動去導航欄目。
![](/img/JS/JS-19-4.png) 

往下拉時：
![](/img/JS/JS-19-3.png) 
<br>

我們可以試試看：
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body {
      height: 2000px;
    }
  </style>
</head>
<body>
  <div>
    <button>點擊我</button>
  </div>

  <script>
    // 滾動事件
    window.addEventListener('scroll', function() {
      console.log('滾動了')
    })

  </script>
</body>
</html>
```

開始滾動時：
![](/img/JS/JS-19-5.png) 
<br>

如果我們想要知道的不是視窗滾動，而是某元素滾動事件可以嗎？

當然可以。

我們也可以使用 `scroll` 事件，直接綁定在DOM元素上。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .texting {
      height: 100px;
      overflow: scroll;
    }
  </style>
</head>
<body>
  <div class="texting">
    123<br>
    123<br>
    123<br>
    123<br>
    123<br>
    123<br>
  </div>

  <script>
    // 滾動事件
    const txt = document.querySelector('.texting')
    txt.addEventListener('scroll', function() {
      console.log('滾動了')
    })

  </script>
</body>
</html>
```

![](/img/JS/JS-19-6.png) 
<br>

### 1.2.1. 頁面滾動- 元素 scrollTop 屬性 
但是，我們看到的通常是滾動條到特定位置，

事件才會被觸發，而不是一滾動就觸發。

也就是説，我們往下滾動，“滾出的部分” 決定什麽時候可以觸發事件。

這個“滾出的部分”，就是 `scrollTop`。
![](/img/JS/JS-19-7.png) 

用上面的例子，我們可以獲取 `txt` 的 `scrollTop`
```javascript
  <script>
    // 滾動事件
    const txt = document.querySelector('.texting')
    txt.addEventListener('scroll', function() {
      console.log(txt.scrollTop)
    })

  </script>
```

![](/img/JS/JS-19-8.png) 

打印的數字，就是往上被卷去的 `txt` 頭部，

越往上卷，被卷去的部分越多，數字越大。

這些數字是單純的數字，沒有 `px` 等單位。
<br>

### 1.2.2. 頁面滾動- 視窗 scrollTop 屬性 
我們也可以獲取視窗被卷去的部分。

`document` 有一個屬性 `documentElement.scrollTop`，

我們可以用這個屬性獲得視窗卷軸往下被卷去的部分：
```javascript
    window.addEventListener('scroll', function() {
      console.log(document.documentElement.scrollTop)
    })
```
![](/img/JS/JS-19-9.png) 
<br>

除了，上述事件，還有一個調整視窗大小才會觸發的事件，下一篇會再詳談，敬請期待。
<!-- 這樣一來，我們可以借由 `document.documentElement.scrollTop`，獲取用戶滾動到某處的位置，

當用戶滾動到某DOM元素的位置(被卷去的頭部 `offsetTop`)，

對那個DOM元素執行 `scroll` 綁定的函數。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    body{
      height: 2000px
    }
    .texting {
      font-size: 50px;
      background-color: pink;
      margin-top: 300px;
    }
  </style>
</head>
<body>
  <div class="texting">经过我就會變紅</div>

  <script>
    const texting = document.querySelector('.texting')
    window.addEventListener('scroll', function(){
      if (document.documentElement.scrollTop > texting.offsetTop){
        // 執行 texting字體變red
        texting.style.color = 'red'
      }
    })
  </script>
</body>
</html>
```

滾動到 `texting`之前：
![](/img/JS/JS-19-10.png) 
<br>

滾動到 `texting`之後：
![](/img/JS/JS-19-11.png) 
<br>

一般上，我們使用縱軸的滾動條比較多，橫軸的很少，

因此， `offset`



關於尺寸事件，其實還有一個縮放視窗相關的 -->