---
title: Javascript-5-無名的函數 —— 匿名函數
excerpt: 匿名函數，就是沒有名字的函數。本篇會討論在調用匿名函數的注意事項。
tags: [Javascript, 匿名函數]
categories: [Javascript]
date: 2025-01-04 11:07:01
---

關於函數的基本介紹可以看回這篇：[Javascript-3-數據類型(三) 複雜數據(函數)](https://wooiseong.vercel.app/2025/01/01/JS-3-complex-md/)。

## 1.1 前言

我們在上面這篇學習了基本函數語法：
```javascript
function add(x,y) {
  console.log(x + y)
}

// 調用函數
add(10,20) //30
```

我們需要調用函數，就直接寫函數名字加實參（如有） `add(10,20)` 。
<br>

你知道嗎？有一種函數是無名的，被稱為匿名函數（Anonymous Functions）。
```javascript
let fn = function (){
  console.log('Hello World')
}
```
<br>

問題來了：如果匿名沒有名字，要怎麽調用呢？🧐

別急，下面就會解答。匿名函數分爲函數表達式和立即執行函數。

## 1.2 匿名函數 - 函數表達式
匿名函數會賦值給一個變量，通過那個變量就可以調用。

```javascript
let a = function (x,y) {
  console.log(x + y)
}
a(1,2) //3
```
<br>

爲什麽會需要賦值給一個變量來調用呢？直接寫具名函數不久好了？

在未來我們學Javascript應用程式界面的時候 （Web API），就會用到。
```javascript
let btn = document.querySelector('button')
btn.onlick = function () {
  console.log('Hello World')
}
```
`btn` 綁定 `onclick` 事件，調用匿名函數。這裏的函數通常用匿名函數，不會用具名函數。


## 1.3 匿名函數 - 立即執行函數
Javascipt立即執行函數，顧名思義就是立即執行的函數，有兩種寫法：

```javascript
// 第一種： 大括號小括號 (function(){})();
(function (x,y) {
  console.log(x + y)
})(1,2); //3

// 第二種： 一個大括號 (function(){}());
(function (x,y) {
  console.log(x + y)
}(1,2)); //3
```

兩種寫法都是可以的，本人比較喜歡第一種，容易記。

{% note danger %}
立即執行函數的前面（如果前面有代碼）和後面必須要加一個分號 `;` ，否則會報錯。

沒有分號，系統以爲立即執行函數後面的代碼還是它的一部分：
{% endnote %}

![](/img/JS/JS-5-1.png) 

立即執行函數，除了可以立即執行，更重要的是它可以<font color="#46A3FF">**避免變量污染**</font>。

如果現在有兩個函數，都需要同一個名字的變量，直接聲明兩次是會報錯的：
![](/img/JS/JS-5-2.png) 

解決辦法是可以用把變量寫在立即執行函數裏。

```javascript
(function () {
  let num = 1;
  console.log(num) // 1
})();  
(function () {
  let num = 2;
  console.log(num) // 2
})();
```
<br>

每個立即執行函數形成獨立的局部作用域。

因此相同名字的變量在不同的立即執行函數不會相互衝突，也不會出現變量污染的情況。

