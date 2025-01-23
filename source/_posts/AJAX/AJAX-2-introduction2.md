---
title: AJAX-2-AJAX簡介（二）
excerpt: 本篇延續上一篇，討論AJAX的概念和技術（請求方法，請求錯誤和HTTP協議）。
tags: [Javascript, AJAX] 
categories: [Javascript]
date: 2025-01-23 12:57:04
---

本篇延續上一篇對於AJAX的概念，沒看過的朋友可以點擊：[AJAX-1- AJAX簡介 (一)](https://wooiseong.vercel.app/2025/01/23/AJAX/AJAX-1-introduction/#3-AJAX%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%AA%8D%E8%AD%98)


### 3.3. 請求方法
請求方法就是對服務器資源要執行的操作，我們在上一篇想要查詢資源，只是使用 `GET`，即可。

一般來説使用Axios時，默認是 `GET` ，是其他請求方法才需要添加 `method` 屬性。常用的請求方法有以下幾種：

- `GET`：從服務器獲取資源。
- `POST`：向服務器提交資源。
- `PUT`：更新服務器上的資源（全部數據）。
- `DELETE`：刪除服務器上的資源。
- `PATCH`：部分更新服務器上的資源（部分數據）。

使用的請求方法一般在後端的接口文件都會注明，我們前端跟著使用即可。
<br>

我們可以嘗試提交個人資料，先看看接口文件：
![](/img/AJAX/AJAX-2-1.png)
<br>

這個接口請求方法是 `POST` , 然後需要 `username` 和 `password` 的參數：

```html
<body>  
  <form action="javascript:;">
    <input class="name" type="text" placeholder="請輸入名字">
    <input class="password" type="password" placeholder="請輸入密碼">
    <button class="btn">提交</button>
  </form>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.7.8/axios.min.js"></script>
  <script>
    const name = document.querySelector('.name')
    const psword = document.querySelector('.password')
    const btn = document.querySelector('.btn')

    btn.addEventListener('click', () => {
      const username = name.value
      const password = psword.value
      axios({
        url: 'http://hmajax.itheima.net/api/register',
        method: 'POST',
        data: {
          username,
          password
        }
      }).then((result) => {
        console.log(result)
      })
    })

  </script>
</body>
```

![](/img/AJAX/AJAX-2-2.png)
<br>

### 3.4. 請求錯誤
一個賬號只可以申請一次，我們如果刷新頁面，再重新申請就會報錯：

![](/img/AJAX/AJAX-2-4.png)

我們可以看到有兩個報錯，

第一個是瀏覽器報錯

第二個是axios的報錯内容，把它展開會看到 `response` 部分有説明錯誤原因： 

`"{\"code\":10005,\"message\":\"账号被占用\",\"data\":null}"`

![](/img/AJAX/AJAX-2-3.png)
<br>

axios提供了 `catch` 的方法，讓我們可以像Javascript的 `try...catch` 的方式捕獲錯誤：

```javascript
  <script>
    const name = document.querySelector('.name')
    const psword = document.querySelector('.password')
    const btn = document.querySelector('.btn')

    btn.addEventListener('click', () => {
      const username = name.value
      const password = psword.value
      axios({
        url: 'http://hmajax.itheima.net/api/register',
        method: 'POST',
        data: {
          username,
          password
        }
      }).then((result) => {
        // console.log(result)
      }).catch(error => {
        console.log(error)
        console.log(error.response.data.message)
      })
    })
  </script>
```
![](/img/AJAX/AJAX-2-6.png)

我們從 `error` 看到 `error.response.data.message` 記錄了錯誤的信息。

我們便可以截取這個信息，并通過彈窗之類的方式提示用戶提交後臺的結果。
<br>

### 3.5. HTTP協議

### 3.5.1. 請求報文
我們發送請求到服務器，有需要遵循的HTTP協議，按照這個協議發送的請求叫請求報文。

裏面有幾個組成部分。我們可以在瀏覽器的 network， 點擊發送的 register 請求， 點擊 Headers ，就可看到請求報文。

一般都會有
- 請求行，顯示請求方法和遵循的協議（HTTP/1.1）

- 請求頭，携帶請求的相關設置，我們需要注意的是 Content-Type，在往後的學習會遇到不是 JSON 的格式。

- 請求頭后有一個空行，來分隔響應頭，空行後面的數據是返回給瀏覽器的資源。

![](/img/AJAX/AJAX-2-9.png)


另外也會有請求體，也就是我們提交的内容。

在瀏覽器被分類在 Payload 那裏,我們可以看到提交的數據是JSON的格式（雙開關引號）。

![](/img/AJAX/AJAX-2-8.png)


### 3.5.1. 相應報文
既然有請求報文，也會有從服務器按照HTTP協議要求，返回給瀏覽器的内容。

![](/img/AJAX/AJAX-2-10.png)

![](/img/AJAX/AJAX-2-11.png)

一般上響應報文會有一個三個數字組成的響應狀態碼，來表明請求的結果：

- 1xx：表示請求已經接收，需要繼續處理。
- 2xx：表示請求成功。
- 3xx：表示重定向。
- 4xx：表示客戶端錯誤。
- 5xx：表示服務器錯誤。

像我們遇到的是 `400 Bad Request` 因爲我們重複提交賬號，所以服務器不能讓數據提交上去，返回一個 `400` 。

