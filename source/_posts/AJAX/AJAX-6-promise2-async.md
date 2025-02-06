---
title: AJAX-6- Promise概念（二）鏈式調用Promise和async-await語法
excerpt: 本篇討論解決回調函數地獄問題的方法，即鏈式調用Promise和async-await語法。
tags: [Javascript, Promise, async, await] 
categories: [Javascript]
date: 2025-02-05 21:10:02
---


上一篇我們討論了回調函數地獄問題，也就是Axios的回調函數中，嵌套下一個Axios回調函數，很難偵測錯誤。

爲了解決這個問題，我們有兩個解決方案：鏈式調用 `Promise` 和使用 `async-await` 語法。

## 1. 鏈式調用 `Promise`
`Promise` 對象内置 `then`方法，因此，我們可以把請求的結果放在 `then` 方法中，並 `return` 一個新的 `Promise`對象。

只有第一個 `Promise`對象執行的結果是成功的，才會調用 `then` 方法，並返回新的 `Promise` 對象。

![](/img/AJAX/AJAX-6-1.png)

我們來試試看：
```javascript
// 創建Promise對象
const p = new Promise((resolve,reject) => {
  setTimeout(() => {
    resolve('成功')
  },2000)
})

const p2 = p.then(result1 => {
  console.log(result1) // 成功

  //返回新的Promise對象
  return new Promise((resolve,reject)=> {
    resolve(result1 + '再次成功')
  })
})

p2.then(result2 => {
  console.log(result2) // 成功再次成功
})
```
<br>

這樣一來，我們就可以很清楚看到兩個異步任務的關係，只要第一個 `Promise` 是 `rejected`， 就不會執行 `then` 方法，也就不會 `return` 新的 `Promise` 對象。
<br>

這樣寫是不是還是讓你覺得麻煩？還有更方便的 `async-await` 可以使用。

## 2. `async-await` 語法
`async` 函數是 `AsyncFunction` 構造函數的實例。我們在函數前面使用關鍵字 `async` ，搭配 `await` 在 `Promise`對象前面，也就是 `await` 取代 `then` 方法，

就可以等 `Promise` 返回結果，再繼續執行後續的程式碼。
<br>

這樣一來，就可以用更簡潔的方式寫基於 `Promise` 的異步任務。
<br>

```javascript
function p(){
  console.log('成功')
  return 1
}

function p2(){
  console.log('再次成功')
  return 2
}

async function fn(){
  const result1 = await p() // 成功
  console.log(result1) // 1
  const result2 = await p2() // 再次成功
  console.log(result2) // 2
}

fn()
```

`await` 取代了 `then` 方法，當 `p()` 執行完了， `await` 會直接返回執行結果，我們可以放在 `result1` 變量中，然後繼續執行後續的程式碼。
<br>
<br>

這個方法在Axios蠻常用的，因爲為Axios本身就是一個 `Promise` 對象，所以可以直接使用 `await` 來等待請求的結果。

再把結果交給下一個 `Promise` 對象，這樣就可以避免回調函數地獄問題了。
<br>

```javascript
async function getData(){
const province = await axios({url:
'http://hmajax.itheima.net/api/province'})
const beijing = province.data.list[0] // 北京
const city = await axios({url:
'http://hmajax.itheima.net/api/city',params:{pname: beijing}})
console.log(city)
}

getData()
```
![](/img/AJAX/AJAX-6-2.png)

我們用 `await` 就可以把第一個Axios請求的成功結果給 `province` ，

再用 `province` 中的 `data.list[0]` （北京） 當成第二個Axios的 `pname` 參數去請求後臺。

這樣寫就會比原來的鏈式調用簡潔很多。

### 2.1. 捕獲 `asycn-await` 的錯誤
鏈式調用可以用 `Promise` 的 `catch` 方法捕獲錯誤，`async-await` 則可以搭配 `try...catch` 捕獲錯誤（[可以這裏重溫語法](https://wooiseong.vercel.app/2025/01/22/JS-38-error/)）。

一樣用上面的例子
```javascript
async function getData(){
  try{
    //寫錯第一個請求地址的url
    const province = await axios({url:
    'http://hmajax.itheima.net/api/provincee'})
    const beijing = province.data.list[0] // 北京
    const city = await axios({url:
    'http://hmajax.itheima.net/api/city',params:{pname: beijing}})
    console.log(city)
  } catch(error){
    console.dir(error)
  }
}

getData()
```
![](/img/AJAX/AJAX-6-3.png)

當我們故意寫錯請求地址的url，`catch` 方法可以捕獲到錯誤，並且把錯誤的資訊顯示在控制台。

當錯誤發生時，就不會執行錯誤以下的代碼，所以在控制臺我們并不會看到第二個Axios請求的結果。
<br>