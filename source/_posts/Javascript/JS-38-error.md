---
title: Javascript-38- 錯誤捕獲
excerpt: 本篇討論Javascript捕獲代碼執行時產生的錯誤的方法。
tags: [Javascript, try, catch, error] 
categories: [Javascript]
date: 2025-01-22 12:49:17
---

## 1. 前言
在我們寫代碼的時候，還沒有跑測試前，我們通常都需要確保代碼可以正常運行。

這時我們可以使用Javascript的方法，捕獲錯誤，執行捕獲錯誤后的代碼，方便我們理清和修復錯誤。

## 2. throw
這個方法會在代碼發生異常馬上抛出錯誤信息，終止下面的代碼運行。

我們也可以使用 `Error` 對象配合 `throw` 使用，記錄詳細的錯誤信息。

舉個例子，一個函數在沒有參數的時候需要抛出錯誤，阻止往下計算：
```javascript
 function count (x,y){
  return x + y
 }

console.log(count()) //NaN
```
<br>

我們可以使用 `throw` 方法：

```javascript
function count (x,y){
  if (!x + !y){
    throw '沒有參數！！！'
    // throw new Error('沒有參數！！！')
  }
  return x + y
 }

console.log(count()) 
```

對應 `throw '沒有參數！！！'`
![](/img/JS/JS-38-1.png)

對應 `throw new Error('沒有參數！！！')`
![](/img/JS/JS-38-2.png)

## 3. try...catch
`throw` 只可以抛出錯誤。如果我們要捕獲錯誤的話可以使用 `try` (試試) ， `catch` (捕獲) 和 `finally` (最後) 組合。
<br>

舉例來説，我寫錯代碼： `document.querySelector('.p')`的 `p` 不應該有 `.` 。
<br>

可能發生錯誤的代碼放在 `try {}` 中， 一有錯誤就會停止執行，

接著錯誤被 `catch (error) {}` 捕捉到，

通常下面會有一個 `finally {}` ， 執行那些不管有沒有錯誤都會執行的代碼。
<br>

```html
<body>
  <p>123</p>
  <script>

  function redChange (){
    try {
      console.log('我在代碼前面')
      const p = document.querySelector('.p')
      p.style.color = 'red'
      console.log('我在代碼後面')
    } catch (error){
      console.log(error)

    }
    finally {
      console.log('finally裏面照樣執行')
    }
    console.log('finally外面')
  }

  redChange()
  </script>
</body>
```
<br>

相信大家有注意到 `fianlly {}` 外面的代碼有錯誤還會執行，

因此通常在 `catch (error) {}` 的方法裏會加上 `return` 終止後面再執行的代碼。
<br>

```html
<body>
  <p>123</p>
  <script>

  function redChange (){
    try {
      console.log('我在代碼前面')
      const p = document.querySelector('.p')
      p.style.color = 'red'
      console.log('我在代碼後面')
    } catch (error){
      console.log(error)
      // 加上 return
      return
    }
    finally {
      console.log('finally裏面照樣執行')
    }
    console.log('finally外面')
  }

  redChange()
  </script>
</body>
```
<br>
<br>

這個捕獲錯誤的概念，很常應用在往後我們用ajax接收請求時，

產生一些錯誤，比如後臺伺服器錯誤，還有發送請求的時候的錯誤等，

我們都可以用 `try` 和 `catch` 來捕獲錯誤，然後進行相應的處理。
<br>
<br>