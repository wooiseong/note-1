---
title: Javascript-22-日期與時間相關（二）延時函數
excerpt: 本篇討論日期和時間相關的操作。
tags: [Javascript] 
categories: [Javascript]
date: 2025-01-14 11:21:10
---

上一篇[Javascript-21-日期與時間相關（一）日期對象與間歇函數](https://wooiseong.vercel.app/2025/01/13/JS-21-time/),

我們討論了定時器-間歇函數 (setInterval) 的寫法，
<br>

間歇函數適合特定時間執行函數，但不能延遲執行。

所以還需要學習定時器-延時函數 (setTimeout) 的寫法。
<br>

## 1.3. 定時器 - 延時函數

<font size="5">`setInterval(函數名, 等待時間)`</font>

*延時函數的第一個參數是函數名，不是函數！

*延時時間的單位是毫秒
<br>

我們可以試試看：
```javascript
function fn(){
  console.log('hi')
}

setTimeout(fn, 1000)
```

在前五秒的時候并不會打印 `hi`
![](/img/JS/JS-22-1.png) 

直到五秒后
![](/img/JS/JS-22-2.png) 

我們繼續等待多幾秒，會發現這個函數只執行一次后就不再執行。
<br>

在等待的當兒，如果我們想要清除延時函數，

和間歇函數一樣，我們可以使用 `clearTimeout` 函數來清除延時函數。

```javascript
  function fn(){
    console.log('hi')
  }

  let n = setTimeout(fn, 5000)

  //  定時器id
  console.log(n) // 1

  // 清除定時器
  clearInterval(n)
```
<br>

延時函數和間歇函數感覺很像，

我們要怎麽知道什麽時候使用哪一個呢？

- 延時函數
延時函數只是按照設定時間，延遲執行函數，
當執行函數后就不會再執行。<br><br>適合只需要執行一次的情況，比如5秒後彈出廣告。
<br>

- 間歇函數
間歇函數會按照設定的時間間隔，
不斷的執行函數。<br><br>適合需要一段間隔時間再次執行的情況，比如更新時間。


