---
title: Javascript-39- 節能環保的魔法 ———— 防抖與節流  
excerpt: 本篇討論節省Javascript性能的方法：防抖和節流。
tags: [Javascript, debounce, throttle] 
categories: [Javascript]
date: 2025-01-22 13:51:26

---

## 1. 前言

我們以爲後端才講效能，其實前端做的頁面是用戶第一個看到的東西，

如果頁面加載很慢，操作很卡，用戶可能會直接放棄使用我們的產品，哪怕頁面看起來非常的酷炫。

有兩個可以減少Javascript引擎性能消耗的技巧： 防抖（debounce）和節流（throttle）必須要掌握。

## 2. 防抖
防抖的定義是<font color="#46A3FF">**單位時間内，頻繁觸發事件，只執行最後一次**</font>。

在單位時間裏，最後一次之前執行的函數都會被清除，只保留最後一次執行。

舉例來説，我們有一個搜索框，我們每打一個字觸發 `input` 事件，

如果我們輸入一個詞可能就觸發20-30次事件！還不包括刪除和空格。

但我們往往都只需要最後一次完整輸入的那個關鍵詞去提交而已。

因此，我們可以利用防抖，2秒内，不管你輸入多少次，都只會觸發一次事件。直到超過了兩秒，才會執行往後的請求等後續操作。

舉個例子：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box {
      width: 100px;
      height: 100px;
      background-color: pink;
    }
  </style>
</head>

<body>
  <div class="box">1</div>
  
  <script>
    const box = document.querySelector('.box')
    let i = 1

    function moving(){
      box.innerHTML = i++
    }

    box.addEventListener('mousemove', moving)
  </script>
</body>
</html>
```
![](/img/JS/JS-39-1.png)

我每經過粉紅色盒子， `mousemove` 被觸發， `moving` 函數被調用。

我從左到右經過盒子，就已經執行了47次 `moving` 函數。其實我只需要在鼠標經過盒子，執行一次函數就夠了。

我們可以用 `lodash` 的防抖或者手寫防抖函數來處理：

### 2.1. lodash 
`lodash` 的設置可以看[Javascript-37- 深淺拷貝 > 3.2. lodash](https://wooiseong.vercel.app/2025/01/20/JS-37-copy/)
<br>

`lodash` 的 ` _.debounce`函數語法如下:

<font size="5">`_.debounce(函數,延遲毫秒數)`</font>
<br>

```javascript
  <script>
    const box = document.querySelector('.box')
    let i = 1

    function moving(){
      box.innerHTML = i++
    }

    box.addEventListener('mousemove', _.debounce(moving,500))
  </script>
```
![](/img/JS/JS-39-2.png)

我們在盒子裏滑了一次，過了半秒，函數才會被執行一次。

因此，我們無論滑多少次，只要停留超過半秒，函數才會被執行。


### 2.2. 手寫防抖
我們可以自己手寫一個防抖函數，也可以瞭解底層實現的原理。

底層的原理其實是依靠定時器完成的。

1. 聲明定時器的變量
2. 每次觸發事件看有沒有定時器，有就清除
3. 沒有定時器就開啓定時器，存到變量裏面
4. 定時器執行函數


```javascript
function debounce(fn,t){
  let timer

  return function(){
    if (timer) clearTimeout(timer)

    timer = setTimeout(function(){
      fn()
    },t)
  }
}

box.addEventListener('mousemove', debounce(moving,500))
```
<br>

## 3. 節流
節流的定義是<font color="#46A3FF">**單位時間内，頻繁觸發事件，只執行一次**</font>。

這樣的事件適合需要頻繁觸發的，比如頁面尺寸縮放,滾動條滾動和鼠標移動。

比如滾動條事件，我們其實是想要滾動條一拉，3秒才觸發一次廣告，不是一拉廣告就不斷閃現。

這時可以利用節流，不斷觸發事件多頻繁，每隔3秒才會執行廣告彈出的函數。

用回上面的例子：鼠標在盒子移動，不管移動多少次，都是每半秒才 `+1` ，

我們一樣可以利用 `lodash` 和手寫節流的方法處理：

### 3.1. lodash 
`lodash` 的 ` _.throttle`函數語法如下:

<font size="5">`_.deb(函數,延遲毫秒數)`</font>
<br>

```javascript
  <script>
    const box = document.querySelector('.box')
    let i = 1

    function moving(){
      box.innerHTML = i++
    }

    box.addEventListener('mousemove', _.throttle(moving,500))
  </script>
```
![](/img/JS/JS-39-3.png)

雖然我在1秒滑了10次，但函數只執行了三次，也就是説無論在指定時間内滑多少次，都只會指定時間到了才執行一次函數。


### 3.2. 手寫節流
我們可以自己手寫一個節流函數，也可以瞭解底層實現的原理。

底層的原理其實也是依靠定時器完成的。

1. 聲明定時器的變量
2. 每次觸發事件看有沒有定時器，有就不再開新的定時器
3. 沒有定時器就開啓定時器，存到變量裏面
4. 定時器執行函數，裏面要把定時器清空

```javascript
function throttle(fn,t){
  let timer = null
  return function(){
    if (!timer){
      timer = setTiemout(function(){
        fn()
        timer = null
      },t)
    }
  }
}

box.addEventListener('mousemove', throttle(moving,500))
```
<br>