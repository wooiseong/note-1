---
title: Javascript-16-監聽風雲（三）事件流，事件捕獲和事件冒泡
excerpt: 本篇討論事件對象的基本概念，包括其屬性，以及應用的場景。
tags: [Javascript, API, DOM, EventListener] 
categories: [Javascript]
date: 2025-01-10 16:15:15
---

## 1.1. 前言
我們在上兩篇學習了事件發生需要的要素和事件對象。

接下來會更詳細瞭解事件發生的機制。

## 1.2. 事件流 (捕獲事件與冒泡事件)
事件流指的是事件完整執行過程的流動路徑。

![](/img/JS/JS-17-1.png) 

舉例來説，我們給一個 `div`標簽綁定點擊事件。

這個事件被觸發時時，實際上是有經過一個從最大的DOM對象(不清楚DOM樹結構的可以先看[這裏](http://localhost:4000/2025/01/05/JS-10-API/))，

也就是`document`父級往下，一層一層傳到我們綁定的 `div`。

這就是<font color="#46A3FF">**事件捕獲 (Event Capturing)**</font>。
<br>

當事件從我們綁定的 `div` 又會往上傳遞要觸發的函數。 從我們綁定的 `div`，一層一層往上傳到`document`。

這就是<font color="#46A3FF">**事件冒泡 (Event Bubbling)**</font>。
<br>

這裏有個簡單示範:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .grandfa{
      width: 200px;
      height: 200px;
      background-color: blue;
    }
    .fa{
      width: 100px;
      height: 100px;
      background-color: green;
    }
    .son{
      width: 50px;
      height: 50px;
      background-color: yellow;
    }
  </style>
</head>
<body>
  <div class="grandfa">
    <div class="fa">
      <div class="son"></div>
    </div>
  </div>

  <script>
    // 爺爺事件-藍
    const grandfa = document.querySelector('.grandfa')
    grandfa.addEventListener('click', function(){
      console.log('爷爺事件')
    })

    // 爸爸事件-綠
    const fa = document.querySelector('.fa')
    fa.addEventListener('click', function(){
      console.log('爸爸事件')
    })

    //孩子事件-黃
    const son = document.querySelector('.son')
    son.addEventListener('click', function(){
      console.log('孩子事件')
    })
  </script>

</body>
</html>
```
點擊黃色箱子時：
![](/img/JS/JS-17-2.png) 

HTML結構中，
- 爺爺`grandfa`，爸爸`fa`，孩子`son`三個`div`。
- 爺爺是<font color="blue">藍色箱子</font>， 
爸爸是<font color="green">綠色箱子</font>， 
孩子是<font color="yellow">黃色箱子</font>。


個別綁定點擊事件，

當我們點擊黃色箱子時，發現打印順序是 孩子 -> 爸爸 -> 爺爺。

顯然是觸發冒泡事件。

如果我們要看到捕獲事件呢？
<br>

我們的事件監聽，實際上還有第三個參數：
```javascript
DOM對象.addEventListener(事件類型, 執行函數， 是否捕獲事件);
```

一般默認是`false`，所以我們一般看不到這個捕獲事件的。

當我們在最後一個參數寫 `true` ，就可以看到捕獲事件了。
<br>

```javascript
  <script>
    // 爺爺事件-藍
    const grandfa = document.querySelector('.grandfa')
    grandfa.addEventListener('click', function(){
      console.log('爷爺事件')
    },true)

    // 爸爸事件-綠
    const fa = document.querySelector('.fa')
    fa.addEventListener('click', function(){
      console.log('爸爸事件')
    },true)

    //孩子事件-黃
    const son = document.querySelector('.son')
    son.addEventListener('click', function(){
      console.log('孩子事件')
    },true)
  </script>
```
點擊黃色箱子時：
![](/img/JS/JS-17-3.png) 

打印順序是 爺爺 -> 爸爸 -> 孩子。
<br>

一般情況下，冒泡事件比捕獲事件更常用。

基於上面的例子，事件冒泡可以這樣理解：

{% note info %}
一個DOM元素綁定事件被觸發時，

會依次向上調用所有父級的<font color="#46A3FF">**同名事件**</font>。
{% endnote %}
<br>

## 1.3. 阻止冒泡
雖然冒泡事件對於用一個子標簽的DOM元素綁定事件，

觸發時也順便觸發父標簽的同名事件，但很常造成不可預計困擾。

如果我們點擊黃色箱子，只觸發它的事件呢？

我麽可以用事件對象綁定一個方法： `事件對象.stopPropagation()`

```javascript
  <script>
    // 爺爺事件-藍
    const grandfa = document.querySelector('.grandfa')
    grandfa.addEventListener('click', function(e){
      console.log('爷爺事件')
      e.stopPropagation()
    })

    // 爸爸事件-綠
    const fa = document.querySelector('.fa')
    fa.addEventListener('click', function(e){
      console.log('爸爸事件')
      e.stopPropagation()
    })

    //孩子事件-黃
    const son = document.querySelector('.son')
    son.addEventListener('click', function(e){
      console.log('孩子事件')
      e.stopPropagation()
    })
  </script>
```
![](/img/JS/JS-17-4.png) 

這個 `e.Propagation()` 方法，也可以阻止捕獲事件。