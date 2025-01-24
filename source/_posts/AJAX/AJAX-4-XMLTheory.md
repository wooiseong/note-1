---
title: AJAX-4- XMLHttpRequest的原理
excerpt: 本篇討論XMLHttpRequest的底層原理，幫助我們更快瞭解這個對象是如何與服務器交互
tags: [Javascript, AJAX, XMLHttpRequest] 
categories: [Javascript]
date: 2025-01-24 13:17:25
---

我們在第一篇[AJAX-1- AJAX簡介 (一)](https://wooiseong.vercel.app/2025/01/23/AJAX/AJAX-1-introduction/)已討論了AJAX的原理，但對於AJAX的實現，只知道和XMLHttpRequest（XHR）對象有關，

這一篇會討論其底層的原理。

你可能會懷疑說我們不是有axios了嗎？幹嘛還要學習XHR？

因爲axios包含XHR對象，内部一些代碼與服務器連接是無關的，會增加代碼打包的體積。

在只有一兩個需要與服務器交互的項目中，使用XHR對象即可。

## 1. 使用XHR對象
一共有4個步驟：
1. 創建XHR對象
2. 配置請求方法和URL等信息
3. 監聽 `loadned` 事件，接收相應結果
4. 發起請求


我們以第一篇的接口地址爲例子：
```javascript
// 1. 創建XHR對象
const xhr = new XMLHttpRequest()

// 2. 配置請求方法和URL等信息
xhr.open('GET', 'https://hmajax.itheima.net/api/province')

// 3. 監聽 `loadned` 事件，接收相應結果
xhr.addEventListener('load', () => {
  console.log(xhr.response)
})

// 4. 發起請求
xhr.send()
```
![](/img/AJAX/AJAX-4-1.png)

我們可以獲得和axios差不多的結果，但axios會自動把JSON字符串轉成對象，XHR方法不會。

我們還需要多做一步，把JSON字符串轉成對象：
```javascript
// 1. 創建XHR對象
const xhr = new XMLHttpRequest()

// 2. 配置請求方法和URL等信息
xhr.open('GET', 'https://hmajax.itheima.net/api/province')

// 3. 監聽 `loadned` 事件，接收相應結果
xhr.addEventListener('load', () => {
  console.log(xhr.response)

  // 將JSON字符串轉成對象
  const data = JSON.parse(xhr.response)
  console.log(data)
})

// 4. 發起請求
xhr.send()
```
![](/img/AJAX/AJAX-4-2.png)

## 1.1. XHR參數
XHR也可以携帶參數，但參數是查詢字符串（參數=數據）的方式，拼接在 `?` 後面。

然後把這一查詢字符串連接在URL後面。

```javascript
// 1. 創建XHR對象
const xhr = new XMLHttpRequest()

// 2. 配置請求方法和URL等信息
xhr.open('GET', 'http://hmajax.itheima.net/api/city?pname=北京')

// 3. 監聽 `loadned` 事件，接收相應結果
xhr.addEventListener('load', () => {

  // 將JSON字符串轉成對象
  const data = JSON.parse(xhr.response)
  console.log(data)
})

// 4. 發起請求
xhr.send()
```
<br>

啊可是萬一參數很多，總不能都寫在URL後面吧？

我們可以把參數以JSON字符串的形式傳輸，并且需要設置請求頭： 

步驟如下：
1. 設置請求頭 `xhr.setRequestHeader('Content-Type', 'application/json')`
2. 把參數轉換成JSON字符串 `JSON.stringify(參數)`
3. 把參數傳入 `send()` 方法中

我們拿[上一篇](https://wooiseong.vercel.app/2025/01/23/AJAX/AJAX-3-formSerialize/)的注冊請求爲例：

```html
<body>  
  <form action="javascript:;" class="formSubmit">
    <input class="name" name="username" type="text" placeholder="請輸入名字">
    <input class="password" name="password" type="password" placeholder="請輸入密碼">
    <button class="btn">提交</button>
  </form>

  <script src="index.js"></script>
  <script>
    const btn = document.querySelector('.btn')

    // 綁定點擊事件
    btn.addEventListener('click', () => {
      const form = document.querySelector('.formSubmit')
      const dataCollected = serialize(form, { hash: true, empty: true })

      const xhr = new XMLHttpRequest()
      xhr.open('POST', 'http://hmajax.itheima.net/api/register')
      xhr.addEventListener('load', () => {
          const data = JSON.parse(xhr.response)
          console.log(data)
      })

      // 設置請求頭
      xhr.setRequestHeader('Content-Type', 'application/json')

      // 把參數轉換成JSON字符串
      const userStr = JSON.stringify(dataCollected)

      // 傳入參數
      xhr.send(userStr)
    })
  </script>
</body>
```
![](/img/AJAX/AJAX-4-3.png)
<br>