---
title: Javascript-21-日期與時間相關（一）日期對象
excerpt: 本篇討論日期和時間相關的操作。
tags: [Javascript, Date, Time] 
categories: [Javascript]
date: 2025-01-13 15:08:32
---

我們在上一個單元討論了DOM的操作，接下來就可以進入DOM的單元。

在進入BOM的部分，一些内容和時間或者日期相關，因此特別開幾篇來討論。

## 1.1. 日期對象
Javascript可以使用 `new` 關鍵字實例化時間對象。

關於 `new` 和實例化先不用深究，後面會再詳談。

實例化日期對象的語法：

```javascript
const date = new Date()
console.log(date) // Mon Jan 13 2025 15:45:53 GMT+0800 (台北標準時間)
```
<br>

日期對象會返回當下的時間等數據，

但這個數據不能直接使用。

不過我們可以使用日期對象原有的方法：

![](/img/JS/JS-21-1.png) 
<br>

舉例來説，我們可以使用 `getMonth()` 方法
```javascript
const date = new Date()
console.log(date.getMonth()) // 0
```
<br>

咦，這個月不是1月嗎？爲什麽返回 `0` ？
<br>

因爲返回的值只有 0-6， 因此 `getMonth()` 和 `getDay()` 需要特別加 `1` ，

才是真正的結果。

### 1.1.1. 時間戳
我們在購物網站常常可以看到倒計時效果，
![](/img/JS/JS-21-2.jpg) 

有的還會顯示倒數的計時器。

倒數，

是未來的時間減現在的時間。

可是我們不能直接計算未來的時間，所以需要藉助時間戳。

時間戳是指<font color="#46A3FF">**從1970年1月1日零點零分零秒到當前時間的總秒數**</font>。

我們有三種獲得現在的時間戳的方法,分別是

`getTime()`， `+newDate()` ， `Date.now()`

```javascript
  // getTime() 
  const date = new Date();
  console.log(date.getTime()) //1736756677688

  // +newDate()
  console.log(+new Date()) //1736756677688

  // Date.now()
  console.log(Date.now()) //1736756677688
```

但需要注意的是， `Date.now` 只可以獲得當前時間戳，未來的不能獲得。

所以要做倒計時，只可以使用 `getTime()` 和 `+newDate()` 。

`+newDate()` 可以不用實例化，又方便記憶，所以推薦使用。
<br>

我們只需要把 `未來時間戳` - `現在時間戳` = `剩餘時間`

那個剩餘時間再轉換成小時，分鐘和秒數就是倒計時了。

```javascript
// 現在時間戳
const now = +new Date()

// 明天時間戳
const tomorrow = +new Date('2025-1-14 00:00:00')

// 倒計時時長
const duration = tomorrow - now
console.log(duration)  // 26450243
```

這個 `26450243` 就是明天到現在的時長了。

## 1.2. 定時器 - 間歇函數
倒計時的製作必然和定時器有關係，那什麽是定時器呢？

定時器是指每隔一段時間就執行一次函數。
<br>

<font size="5">`setInterval(函數名, 間隔時間)`</font>


*間歇函數的第一個參數是函數名，不是函數！

*間隔時間的單位是毫秒


<br>
擧個例子：

```javascript
function fn(){
  console.log('hi')
}

setInterval(fn, 1000)
```
![](/img/JS/JS-21-3.png) 

這是執行函數7秒之後的畫面。每隔1秒就會執行一次 `fn` 函數。

如果我不管它，它會一直執行下去。

因此，間歇函數通常會在滿足特定條件後，停止執行。
<br>

那我們如何停止間歇函數呢？

每個定時器都有一個獨特的id編號。

我們可以把id編號傳入 `clearInterval()` 函數，就可以停止間歇函數了。

```javascript
  function fn(){
    console.log('hi')
  }

  let n = setInterval(fn, 1000)

  //  定時器id
  console.log(n) // 1

  // 清除定時器
  clearInterval(n)
```
<br>

回到我們的倒計時，我們可以這樣寫：
```javascript
  function fn(){
    const now = +new Date()
    const tomorrow = +new Date('2025-1-14 00:00:00')
    const duration = tomorrow - now
    const h = Math.floor(duration / 1000 / 60 / 60)
    const m = Math.floor(duration / 1000 / 60 % 60)
    const s = Math.floor(duration / 1000 % 60)
    console.log(`${h}時${m}分${s}秒`)
  }

  setInterval(fn, 1000)
```
![](/img/JS/JS-21-4.png) 

我們可以看到，時間會一秒一秒的減少。

直到 `00時00分00秒` ，倒計時就結束了。
<br>

細心的你肯定也發現：間歇函數有一秒的延遲才會執行。

如果我們需要馬上執行函數，可以在間歇函數之前先調用一次函數。
```javascript
  function fn(){
    const now = +new Date()
    const tomorrow = +new Date('2025-1-14 00:00:00')
    const duration = tomorrow - now
    const h = Math.floor(duration / 1000 / 60 / 60)
    const m = Math.floor(duration / 1000 / 60 % 60)
    const s = Math.floor(duration / 1000 % 60)
    console.log(`${h}時${m}分${s}秒`)
  }

  // 先調用函數一次
  fn()

  // 調用間歇函數
  setInterval(fn, 1000)
```

<br>
如果我想不要馬上執行定時器，

在登入頁面5秒后才開始計時呢？

方法留到下一篇再介紹，敬請期待。