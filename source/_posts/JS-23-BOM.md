---
title: Javascript-23-BOM介紹
excerpt: 本篇討論BOM的定義，以及屬於BOM的對象。
tags: [Javascript, Date, Time] 
categories: [Javascript]
date: 2025-01-14 12:06:10
---

## 1.1. 什麽是BOM

BOM的全名是Browser Object Model,也叫瀏覽器對象糢型。

大家還記得DOM樹嗎？

![](/img/JS/JS-10-4.png)
<br>

DOM樹描述了DOM的結構，而DOM其實只是BOM的一個分支。

![](/img/JS/JS-23-1.png)

從上面的結構來看， `window` 是Javascript中頂級對象，

也是全局對象。

也就是説向我們用過的方法，比如 `console.log` , `document` , `alert()` ，

這些都是 `window` 的方法。
<br>

`var` 定義在全局作用域的變量，函數都會變成 `window` 的屬性和方法。

當然也包括我們曾經用過的DOM的方法，

比如 `document.querySelector`,

實際的寫法應該是 `window.document.querySelector`

只是在調用時是可以省略 `window`。

不信？我們可以測測看：

```javascript
console.log(document.querySelector === 
window.document.querySelector) // true
```

在DOM相關篇章，我們已經瞭解了 `document`對象的屬性和方法，

接下來會討論BOM下面的三個對象： `location` 對象 , `navigator` 對象 , `history` 對象 。

### 1.1.1. location對象
和跳轉或者URL地址相關。

- location href
和CSS的功能差不多，我們可以用Javascript的方式讓頁面跳轉至其其他URL地址：

```javascript
  function fn (){
    location.href = 'https://www.google.com/'
  }

  setTimeout(fn,5000)
```

我們原來的頁面：
![](/img/JS/JS-23-2.png)

5秒後的頁面：
![](/img/JS/JS-23-3.png)
<br>

- location search
這個方法可以保存URL地址携帶的參數，也就是符號 `?` 後面部分。

```html
<body>
  <form>
    <input type="text" name="username">
  </form>
  <script>
    console.log(location.search)
  </script>
</body>
```

當我們在輸入框輸入 `123456`， 並 `Enter` 之後，

URL部分會携帶 `?username=123456` 這樣的參數，

而 `location.search` 可以捕捉這樣的參數，再加以處理。
<br>
<br>


### 1.1.2. navigation對象
navigator對象記錄瀏覽器自身的相關信息。

我們可以打印 `console.log(navigator)` 看看：
![](/img/JS/JS-23-5.png)

裏面有很多對象，我們最常用的是 `userAgent`，

我們打印 `console.log(navigator.userAgent)`，

![](/img/JS/JS-23-6.png)


這一字符串是做什麽用的呢？
<br>

`userAgent` 被稱爲使用者代理，是網站與伺服器用來辨識使用者行為的標記。

當網站瀏覽器打開時， `userAgent`就會產生，并以 `User-Agent Header`的形式發送給伺服器，伺服器就知道使用者瀏覽器裝置信息。

細節可以參考這篇：[UserAgent 是什麼？網站如何用它辨識身分？](https://simular.co/blog/post/what-is-useragent-and-how-to-use-it)
<br>
<br>

### 1.1.3. history對象
這個對象主要管理歷史記錄，該對象與瀏覽器地址欄的操作是相對應的。

- `history.back()` - 後退上一頁

- `history.forward()` - 前進下一頁

- `history.go(參數)` - 參數是整數，`1` 代表前進下一頁; `-1`代表後退上一頁
<br>
<br>