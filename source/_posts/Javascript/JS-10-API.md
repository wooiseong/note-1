---
title: Javascript-10-Javascript操作的旋律 —— DOM BOM, DOM DOM BOM？
excerpt: 本篇會仔細闡述Javascript使用let和const的時機。
tags: [Javascript, API, DOM, BOM] 
categories: [Javascript]
date: 2025-01-05 19:06:39
---

## 1.1. 前言
我們在前9篇，已經學習了什麽基礎的Javascript語言基礎，但只是Javascript的一部分。這個語言基礎是基於一個叫 ECMAScript 的語言規範，有興趣的朋友可以多家瞭解 [JavaScript 核心语言（ECMAScript）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/JavaScript_technologies_overview)。

Javascript更大的價值，在於它可以負責網頁的邏輯。
<br>

比如輪播圖，沒有Javascript，是無法切換頁面，或者控制它的播放的時機的。
![](/img/JS/JS-10-1.png) 
<br>
Javascript實際上並不能直接控制瀏覽器，而是它可以通過瀏覽器提供的網絡應用程式介面（Web Application Programming Interface-Web API）來操作的。

Web API可以分爲頁面文檔對象模型（Document Object Model-DOM）和瀏覽器對象模型（Browser Object Model-BOM）。

![](/img/JS/JS-10-2.png) 

我們接下來會慢慢瞭解這兩者的内容。

## 1.2. DOM
DOM是瀏覽器一套專門用來操作網頁内容的功能。

主要作用是實現網頁内容特效和實現用戶交互。

比如以上的輪播圖例子，我只要點擊左右的 “<” 和 “>” (紅色箭頭處)，畫面就會切換成上一頁或者下一頁的輪播圖：

![](/img/JS/JS-10-3.png) 

### 1.2.1. DOM樹
在瞭解Javascript操作DOM前，我們可以先把HTML文檔以樹狀結構直觀表現出來，這樣的結構就被稱爲DOM樹。

在學習HTML和CSS的時候，不知道你有沒有留意過HTML文件的結構，都會有`<head>`，`<body>`等標簽，加上我們之後加上去的`div`, `a`和其他HTML標簽。

![](/img/JS/JS-10-4.png) 

DOM很直觀標簽標簽和標簽之間的關係。Javascript就是通過這些HTML標簽去操作網頁的。

### 1.2.2. DOM對象
瀏覽器根據上面提到的HTML標簽，生成一個個的DOM對象。

當我們修改對象的屬性，就會映射在HTML標簽身上。

我們在瀏覽器通過 `document.querySelector` 獲取 `<div>123</div>`這個HTML標簽（這個方法先不用去瞭解，後面會學到）

```html
<body>
  <div>123</div>

  <script>
    const div = document.querySelector('div')
    console.dir(div)
  </script>

</body>
```
![](/img/JS/JS-10-5.png) 

我們打印會看到這個DOM身上所有屬性（`div` 標簽下的那些），

<font color="#46A3FF">**我們操作這些DOM對象的屬性，把它們當成一般對象來處理**</font>，就是DOM的核心思想。

在DOM樹，我們可以發現最上面的是 `document`，這個DOM對象包括了網頁所有的内容，

比如 `document.write()`

或者 `document.querySelector()`

這些都是挂載在 `document` 的方法。我們可以利用這些方法來對網頁做一些操作。
<br>
<br>
確切應該怎麽用Javascript操作DOM呢？我們下一篇會提到，敬請期待。