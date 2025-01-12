---
title: Javascript-18-監聽風雲（四）事件委托，事件解綁
excerpt: 本篇討論事件委托和事件解綁的概念和應用。
tags: [Javascript, API, DOM, EventListener] 
categories: [Javascript]
date: 2025-01-11 19:54:30
---

本篇和事件流有關係，沒看過的朋友可以先復習：[監聽風雲（三）事件流，事件捕獲和事件冒泡](https://wooiseong.vercel.app/2025/01/10/JS-17-eventflow/)

## 1.1. 事件委托
我們在上一篇已經討論了事件冒泡的影響，以及阻止事件冒泡的方法。

但事件冒泡應用得當，是可以幫我們節省代碼的。

我們想象一個情況，我們有5個 `<li>`，點擊時會變紅色，

我們希望點擊一個 `<li>`， 其他 `<li>` 都會變紅。
<br>

固然可以使用 `for`， 便利所有的 `<li>` 都變紅。

缺點也很明顯：性能消耗問題，尤其是很多 `<li>` 的時候。
<br>

我們可以使用事件委托：

把事件綁定父級元素上，點擊子元素會事件冒泡到父級元素上。
<br>

點擊前：
```html
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
  </ul>

  <script>
    const ul = document.querySelector('ul')

    // 把事件委托給父級元素
    ul.addEventListener('click', function() {
      ul.style.color = 'red'
    })
  </script>
</body>
```
![](/img/JS/JS-18-1.png) 
<br>

點擊任一 `<li>` 後：
![](/img/JS/JS-18-2.png) 
<br>


## 1.2. 事件解綁
我們有一個方法可以讓事件不會再觸發： `removeEventListener`。

這個方法和 `addEventListener` 類似，也是兩個參數，第一個是事件名稱，第二個是事件處理函數。


還沒有事件解綁前，我們點擊：
```html
<body>
  <button class="btn">點擊我</button>

  <script>
    const btn = document.querySelector('.btn')

    // 聲明事件處理函數
    const fn = function(){
      console.log('點擊了')
    }

    // 事件監聽
    btn.addEventListener('click', fn)
  </script>
</body>
```
![](/img/JS/JS-18-3.png)
<br>


事件解綁后，我們點擊：
```html
<body>
  <button class="btn">點擊我</button>

  <script>
    const btn = document.querySelector('.btn')

    // 聲明事件處理函數
    const fn = function(){
      console.log('點擊了')
    }

    // 事件監聽
    btn.addEventListener('click', fn)

    // 事件解綁
    btn.removeEventListener('click', fn)
  </script>
</body>
```
![](/img/JS/JS-18-4.png)
<br>

你會注意到，我們一般寫事件處理函數，

都是不會在外面聲明的，這是因爲

<font color="#46A3FF" size="5">**匿名函數不能被事件解綁**</font>。

在書寫時，要特別注意。

