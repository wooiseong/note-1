---
title: Javascript-12-操作DOM元素
excerpt: 本篇會討論操作DOM元素内容和屬性的方法。
tags: [Javascript, API, DOM] 
categories: [Javascript]
date: 2025-01-06 09:39:25
---

上一篇提到了獲取DOM元素的方法，接下來會討論如何操作DOM元素。

建議先看過上一篇再來：[Javascript-11-獲取DOM元素](https://wooiseong.vercel.app/2025/01/05/JS-11-querySelector/)

## 1. 前言
獲取了DOM，我們可以對它進行操作，例如修改內容、修改屬性等。

還記得我們曾經説過

<font color="#46A3FF">**瀏覽器根據上面提到的HTML標簽，生成一個個的DOM對象**</font>。

我們操作DOM對象,就是操作HTML標簽。

我們操作DOM，有幾個方面。

## 2. 修改DOM元素屬性

### 2.1. 修改innerText 和 innerHTML屬性
如果我們想要修改HTML標簽裏的内容，比如

`div`裏的内容 `123` 變成 `456`

```html
<body>
  <div class="testing">123</div>

  <script>
    
  </script>
</body>
```

![](/img/JS/JS-12-1.png)  
<br>

我們可以通過操作`innerText`屬性，去修改 `div`標簽的内容：
```html
<body>
  <div class="testing">123</div>

  <script>
    const div = document.querySelector('.testing')
    div.innerText = '456'
  </script>
</body>
```
![](/img/JS/JS-12-2.png)  

網頁變成456。
<br>

但是 `innerText` 只可以操作<font color="#46A3FF">**純文本内容**</font>，不解析標簽。

我們可以用另一個屬性 `innerHTML`。
<br>

假如我們要加一個 `<strong>`標簽：
```html
<body>
  <div class="testing">123</div>

  <script>
    const div = document.querySelector('.testing')
    div.innerHTML = '<strong>456</strong>'
  </script>
</body>
```
![](/img/JS/JS-12-3.png) 

那個 `<strong>`就可以被解析成功。`456` 被加粗了。
<br>

`innerHTML`可以像 `innerText` 操作文本，還可以添加標簽，所以一般更鼓勵用 `innerHTML` 修改内容。
<br>

### 2.2. 修改常用屬性
還記得我們學習HTML標簽時，一些標簽自帶屬性？比如 `<img>`的 `src` 和 `alt` 屬性。或者 `<a>` 的 `href` 屬性。

```html
<body>
    <img src="https://i.ibb.co/j8ZQGgb/bootstrap.jpg" alt="" 
    class="imgEdit">
</body>
```
![](/img/JS/JS-12-4.png) 

通過DOM操作，我們也可以修改這些HTML標簽自帶的屬性，

比如我們把 `img` 的 `src` 的值直接替換成其他圖片地址：
```html
<body>
  <img src="https://i.ibb.co/j8ZQGgb/bootstrap.jpg" alt="" 
  class="imgEdit">

  <script>
    const img = document.querySelector('.imgEdit')
    img.src = "https://i.ibb.co/xXvjX8W/4abdba977d932814a46555e52f8ca789.jpg"
  </script>
</body>
```
![](/img/JS/JS-12-5.png) 
<br>

### 2.3. 修改樣式
我們在學習CSS時，學過如何修改元素的樣式，比如 `color`、`background-color`、`font-size` 等。

舉例來説，我們要修改一段字的字體顔色和大小：
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .testing {
      color: red;
      font-size: 100px;
    }
  </style>
</head>
<body>

  <div class="testing">123</div>
  
</body>
</html>
```
![](/img/JS/JS-12-6.png)
<br>

我們會在 `style`標簽裏設置 `div` 的樣式。

Javascript同樣也可以通過幾種方法修改CSS樣式。
<br>

#### 2.3.1. 通過 style
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .testing {
      color: red;
      font-size: 100px;
    }
  </style>
</head>
<body>

  <div class="testing">123</div>
  
  <script>
    const div = document.querySelector('.testing')
    div.style.color = 'blue'
  </script>

</body>
</html>
```
![](/img/JS/JS-12-7.png)

我們可以直接修改 `style` 屬性，來修改樣式。

把值以字符串的方式賦值給DOM元素的 `style` 屬性，

也就是 `div.style.color` = `blue` 。
<br>

{% note warning %}
有幾個需要注意的事項：
1. 如果是修改需要單位的樣式屬性，比如 `font-size`，我們需要加上單位，比如 `px`。

2. 如果有屬性有 `-` ,就是用小駝峰命名法，比如 `font-size` 寫成 `fontSize`
{% endnote %}

```javascript
  <script>
    const div = document.querySelector('.testing')
    div.style.fontSize = '200px'
  </script>
```
<br>

這種修改方式適合需要比較單純和少量修改的樣式。
<br>

#### 2.3.2. 通過className
![](/img/JS/JS-12-9.png) ![](/img/JS/JS-12-6.png)

Javascript可以把 `className` 加給html屬性，讓它獲得樣式。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .testing {
      color: red;
      font-size: 100px;
    }
    .additional {
      color: pink;
    }
  </style>
</head>
<body>

  <div class="testing">123</div>
  
  <script>
    const div = document.querySelector('.testing')
    div.classList.add('additional')
  </script>
</body>
</html>
```
![](/img/JS/JS-12-8.png)

原來 `testing`的紅色，會被 `additional` 的粉紅色覆蓋掉。`additional` 沒有 `font-size`，就會維持原來的樣式。
<br>

#### 2.3.3. 通過classList
使用添加 `className` 的方式，

的確是可以很方便把某組樣式直接加給DOM元素。

但問題在於，新的 `className` 容易覆蓋以前的。最好的辦法是以 `classList` 的方式追加和刪除 `className` 。

什麽是 `classList` ?

`classList` 是DOM元素的屬性集合，會顯示這個HTML標簽上所有的 `className`。
 ```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .testing {
      color: red;
      font-size: 100px;
    }
    .additional {
      color: pink;
    }
  </style>
</head>
<body>

  <div class="testing">123</div>
  
  <script>
    const div = document.querySelector('.testing')
    div.classList.add('additional')
    console.log(div.classList)
  </script>
</body>
</html>
```
![](/img/JS/JS-12-10.png)
<br>

因此，除了我們在 “2.3.2. 通過className” 的 `add`，方法，其實還有 `remove` 和 `toggle` 方法。
 ```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .testing {
      color: red;
      font-size: 100px;
    }
    .additional {
      color: pink;
    }
  </style>
</head>
<body>

  <div class="testing">123</div>
  
  <script>
    const div = document.querySelector('.testing')
    div.classList.add('additional')
    div.classList.remove('testing')
    console.log(div.classList)
  </script>
</body>
</html>
```
![](/img/JS/JS-12-11.png)

通過 `remove` 便可以移除原來加上的 `testing`。
<br>

{% note warning %}
`classList` 操作的 `className` 和 `document.querySelector`輸入的參數不一樣，名字前面是沒有 `.` 的！

 ```javascript
  const div = document.querySelector('.testing')
  div.classList.add('additional')
 ```

{% endnote %}
<br>


還有`toggle`的部分，會留到下一篇再討論，敬請期待。

