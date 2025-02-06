---
title: AJAX-5- Promise概念（一）Promise的語法
excerpt: 本篇討論Promise的概念，以及Promise處理異步任務的角色。
tags: [Javascript, Promise] 
categories: [Javascript]
date: 2025-02-05 20:04:12
---

我們在[Javascript-25-事件循環](https://wooiseong.vercel.app/2025/01/14/JS-25-eventLoop/) 有討論了任務分爲同步和異步任務。

在我們執行異步任務時，我們往往需要獲取執行成功或者失敗的結果,再根據這些結果，執行對應的函數，讓使用者知道結果。


因此，我們需要 `Promise` 對象來表示異步任務最終處理結果。

## 1.1. Promise語法

我們創建 `Promise` 后，可以調用 `then` 方法，其實參是一個回調函數。

我們可以接著使用 `catch` 來捕獲錯誤，實參也是一個回調函數。

當我們執行的異步任務（`setTimeout`）成功了， `Promise` 會返回一個結果，我們可以使用 `then` 來捕獲函數執行成功的結果。

```js
// 創建Promise對象
const p = new Promise((resolve,reject) => {
  setTimeout(() => {
    resolve('成功')
  },2000)
})

//獲取結果·
p.then(result => {
  console.log('成功嗎？',result) // 成功嗎？成功
}).catch(error => {
  console.log('失敗嗎？',error)
})
```
<br>

我們可以讓 `Promise` 返回失敗的結果試試看：
```js
// 創建Promise對象
const p = new Promise((resolve,reject) => {
  setTimeout(() => {
    reject('失敗')
  },2000)
})

//獲取結果·
p.then(result => {
  console.log('成功嗎？',result) 
}).catch(error => {
  console.log('失敗嗎？',error) // 失敗嗎？失敗
})
```
<br>

因此，通過 `Promise` 我們可以對異步任務的成功或者失敗進行處理。


## 1.2. Promise的三種狀態
`Promise` 有三種狀態，並只會處在其中一種狀態：

- 待定（pending）：初始狀態，既不是兌現，也不是拒絕狀態
- 已兌現（fulfilled）： 任務執行完成
- 已拒絕（rejected）： 任務執行失敗
<br>
<br>

`Promise` 對象一旦被兌現，或者拒絕就<font color="#46A3FF">**不可以改變狀態**</font>。
<br>

`resolve()` 被調用的時候，`Promise` 狀態會變成 `fulfilled`，並在 `then` 中獲取到 `resolve()` 的參數。

同樣的， `rejected()` 被調用的時候，`Promise` 狀態會變成 `rejected`，並在 `catch` 中獲取到 `rejected()` 的參數。
<br>

`Promise` 的狀態可以通過被創建的實例訪問到，我們用回上面的例子：
```js
// 創建Promise對象
const p = new Promise((resolve,reject) => {
  setTimeout(() => {
    resolve('成功')
  },2000)
})

console.log(p)
```

在還沒有執行 `setTimeout` 任務前， `Promise` 狀態是 `pending`：
![](/img/AJAX/AJAX-5-1.png)


成功執行完任務， `Promise` 狀態才會變成 `fulfilled`：
![](/img/AJAX/AJAX-5-2.png)
<br>

大家有注意到吧？我們使用的Axios，

其實就是 XMLHttpRequest 的封裝，並且也是使用 `Promise` 來處理異步任務的。


![](/img/AJAX/AJAX-5-3.png)
<br>

因此，我們在axios返回結果時，可以使用 `then` 和 `catch` 來處理異步任務的結果。

```javascript
axios({
  url: 'http://hmajax.itheima.net/api/province'
}).then((result) => {
  console.log(result)
  console.log(result.data.list)
})
```
<br>

`Promise` 如果只有涉及一兩個獨立的，不相互依靠的任務，當然沒問題，反之就會遇到回調函數地獄問題。

如果我們發第一個請求的回調函數，請求成功后作爲第二個請求的參數；

第二個請求的回調函數的參數作爲第三個請求的參數，

這樣的回調函數中嵌套回調函數，就被稱爲回調函數地獄。

![](/img/AJAX/AJAX-5-4.png)

這樣的寫法，萬一在其中的異步任務執行出錯會很難偵察，因爲你不知道哪一個才是發生錯誤的那個。
<br>

解決這個問題的方法會留到下一章再討論，敬請期待。
<br>