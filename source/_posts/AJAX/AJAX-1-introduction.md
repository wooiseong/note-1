---
title: AJAX-1- AJAX簡介 (一)
excerpt: 本篇討論AJAX的概念和技術(URL和參數)。
tags: [Javascript, AJAX] 
categories: [Javascript]
date: 2025-01-23 09:14:01
---

我們學習了 HTML, CSS和Javascript， 已經有足夠能力做基本的靜態網站了。但這個網站的内容是 “死” 的，因爲資料都是無法更新的。

因此，我們有需要跟後臺要數據，這個要數據的通信方法就是AJAX。

## 1. AJAX簡介
AJAX的全名是Asynchronous JavaScript and XML。

簡單來說，AJAX就是通過XMLHttpRequest對象從服務器獲取數據的方法。

發送的數據可以是JSON, XML, HTML, Text, Image等。

AJAX最大的特性是 “異步” ，就是説可以在不刷新頁面的情況下和服務器通信，更新數據或者頁面。


## 2. AJAX的封裝 - axios
我們可以用XMLHttpRequest來實現AJAX，但這個方法比較麻煩，我們可以使用第三方庫來簡化AJAX的實現。

Axios是一個已經組好通信相關設置的函式庫。我們會在接下來的學習，先用axios，再瞭解AJAX底層原理。

1. 首先插入axios的[函式庫](https://cdnjs.com/libraries/axios/1.7.8)
```javascript
<script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.7.8/axios.min.js"></script>
```


2. axios的語法
```javascript
axios({
  url: 地址
}).then((數據) => {
  // 得到數據後的處理
})
``` 
<br>

在還沒有嘗試前，我先説明我使用的是[黑馬程序員的數據接口](https://apifox.com/apidoc/shared-1b0dd84f-faa8-435d-b355-5a8a329e34a8/api-87683399)，有時有變動而無法訪問，但多數情況下是可以使用的。

![](/img/AJAX/AJAX-1-1.png)

這是平臺接口，工作時的接口數據可能不會長這樣。我就簡單介紹一下：

- 最上面有一個地址，就是我們的接口地址。

- 綠色的 `GET` 是指請求方法。

- 右下角有返回響應后應該有的數據内容和結構。
<br>


我們可以試試發送我們的第一個AJAX請求吧！
```javascript
axios({
  url: 'http://hmajax.itheima.net/api/province'
}).then((result) => {
  console.log(result)
  console.log(result.data.list)
})
```
![](/img/AJAX/AJAX-1-2.png)

我們可以看到返回來的數據是一個對象，我們的數據通常在 `data` 的部分。

我們可以看到 `data` 的部分有一個 `list` ，是一個數組，這才是我們要的數據， 因此我們可以用 `result.data.list` 的方式獲取城市清單。

當然這也是看後臺返回的數據結構，這個需要和後端工程師溝通才知道他們返回的數據結構是什麼樣的。

## 3. AJAX的基本認識
### 3.1. URL
URL是統一資源定位符（Uniform Resource Locator）， 俗稱網址，是因特網標准的資源地址。

用於訪問網絡的資源。

URL的組成是協議，域名和資源路徑：
![](/img/AJAX/AJAX-1-3.png)

- 我們可以看到 `http://` ，這個是http協議，是超文本傳輸協議，規定瀏覽器和服務器之間的傳輸數據的格式。這個格式在我們後面會詳談。

- 域名是服務器在瀏覽器的方位，就比如google有google的服務器, 蝦皮有蝦皮的服務器。有了域名，我們才知道要跟哪一個服務器要資料。

- 資源路徑是要訪問的資源在服務器的訪問路徑，比如上面的例子，就是訪問 `/api/province` 的資源。


### 3.2. 參數
參數是URL中?後面的部分，用於向服務器傳遞需要的數據。

我們在google輸入“貓咪”后，原來的URL `www.google.com` 會變成：`https://www.google.com/search?q=%E8%B2%93%...`

這個 `q=參數` 就是我們輸入的搜索詞轉化的内容。

一般在後臺給的接口文件對參數有一定要求：

![](/img/AJAX/AJAX-1-4.png)

比如獲取城市列表前，需要一個字符串為參數的值，這個字符串是省份的名字，不但參數數據種類 `string` 要正確，參數的名字 `pname` 也必須正確 。

如果這個參數<font color="#46A3FF">**是必須的話，就表示沒有這個參數就不能得到想要的後臺的數據。**</font>

參數可以添加在 `params: {}` 這個屬性上。
<br>

我們可以試試看：
```javascript
// 沒有參數
axios({
  url: 'http://hmajax.itheima.net/api/city',
  }).then((result) => {
  console.log(result)
  })

// 有參數
axios({
 url: 'http://hmajax.itheima.net/api/city',
 params: {
  pname: '北京'
 }
}).then((result) => {
  console.log(result)
})
```
![](/img/AJAX/AJAX-1-5.png)

我們可以看到沒有的參數的組別 `result.data.list` 就是一個空數組；

只有輸入了對的參數名，對的參數數據類型，并且後臺有這個參數的數據才會返回數據。
<br>

AJAX除了這些，還有一些需要學習的基礎知識，下篇會繼續細談，敬請期待。
<br>
