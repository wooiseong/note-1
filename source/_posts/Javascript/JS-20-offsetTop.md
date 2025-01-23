---
title: Javascript-20-監聽風雲（六）頁面尺寸事件
excerpt: 本篇討論除了前幾篇討論的事件，最近添加的頁面尺寸事件。
tags: [Javascript, API, DOM, EventListener， OffsetTop] 
categories: [Javascript]
date: 2025-01-12 21:25:04
---

這篇和頁面滾動事件有關，還沒看過的朋友可以看上一篇：[Javascript-19-監聽風雲（五）頁面加載，滾動事件](https://wooiseong.vercel.app/2025/01/12/JS-19-mouseEvent/)


## 1.3. 頁面尺寸事件
這個事件主要在調整視窗的時候觸發：

```javascript
window.addEventListener('resize', function(){
  console.log('視窗大小改變')
})
```

調整視窗前：
![](/img/JS/JS-20-1.png) 

調整視窗后：
![](/img/JS/JS-20-2.png) 
<br>

## 1.4. 頁面元素寬高獲取
獲取元素的寬高有兩種方式。

### 1.4.1. client系列
這個系列可以獲得相對視窗的元素之

寬 `clientWidth` ；高 `clientHeight`

這個系列獲取的，包括元素自身設置的寬高，和 `padding`，但不包括 `border`，只讀屬性。

![](/img/JS/JS-20-3.png) 

### 1.4.2. offset系列
這個系列可以獲得相對視窗的元素之

寬 `offsetWidth` ；高 `offsetHeight`

這個系列獲取的，包括元素自身設置的寬高，`padding` 和 `border`，只讀屬性。

![](/img/JS/JS-20-4.png) 

我們來個例子吧！比較容易理解：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box{
      width: 100px;
      height: 100px;
      padding: 20px;
      border: 5px solid black;
      background-color: pink;
    }
  </style>
</head>
<body>
  <div class="box">我是盒子</div>
  <script>
    const div = document.querySelector('div')
    console.log(div.clientWidth)
    console.log(div.offsetWidth)
  </script>
</body>
</html>
```

![](/img/JS/JS-20-5.png) 

`clientWidth` 結果是 `140`， 因爲 `100px` + `20px` + `20px`。


`offsetWidth` 結果是 `150`, 因爲還有加上左右的 `5px` 的 `border`。


## 1.5. 頁面元素位置獲取
在上篇和這篇，我們瞭解了頁面滾動事件發生時，

我們怎麽獲取被卷去的位置，

比如我們想要頁面滾動超過 `100` ，也就是導航欄目結束就安插廣告，

雖然我們可以設定 `document.documentElement.scrollTop > 100`，就執行 `scroll` 綁定的函數，

但萬一導航欄目位置前面加插一個新元素， 

導航欄目就不在 `100`的位置了，所以這裏我們需要一個更靈活的獲取被卷去的位置的方法。
<br>
也就是需要<font color="#46A3FF" size="5">**獲取DOM元素所在的位置**</font>。
<br>

我們可以使用`offset`系列的 `offsetTop` 和 `offsetLeft`，

它們分別是相對父級的上和左的距離。
<br>

我们把上面的例子稍微改一下，箱子加個 `100px` ：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box{
      width: 100px;
      height: 100px;
      padding: 20px;
      margin: 100px;
      border: 5px solid black;
      background-color: pink;
    }
  </style>
</head>
<body>
  <div class="box">我是盒子</div>
  <script>
    const div = document.querySelector('div')
    console.log(div.offsetTop)
  </script>
</body>
</html>
```

![](/img/JS/JS-20-6.png)


`offsetTop`的結果是 `100` ， 因爲`margin-top`，會把箱子移至相對視窗的 `100px` 的位置。

沒有父級元素，默認以 `<body>` 為相對定位，
<br>

如果有父級元素，就以它開始計算子元素相對的距離：
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box{
      width: 100px;
      height: 100px;
      padding: 20px;
      margin: 100px;
      border: 5px solid black;
      background-color: pink;
      position: relative;
    }
    .box_son{
      width: 10px;
      height: 10px;
      background-color: red;
      position: absolute;
      top: 30px;
      left: 30px;
    }
  </style>
</head>
<body>
  <div class="box">
    <div class="box_son"></div>
  </div>
  <script>
    const box = document.querySelector('.box')
    const box_son = document.querySelector('.box_son')

    // 大箱子offsetTop
    console.log(box.offsetTop)
    // 小箱子offsetTop
    console.log(box_son.offsetTop)
  </script>
</body>
</html>
```
![](/img/JS/JS-20-7.png)

紅色小箱子（子元素）距離粉紅色大箱子上邊是 `30px` ，

因爲有使用定位 `position` ， 所以小箱子的位置會從大箱子上方開始計算，

所以小箱子的 `offsetTop` 還是 `30px`。

一般形況下，我們使用 `offsetTop` 的頻率比 `offsetLeft` 還多.
<br>

把我們上篇和這篇的内容做一個總結：

![](/img/JS/JS-20-8.png)