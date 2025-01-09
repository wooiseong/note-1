---
title: Javascript-13-操作DOM元素（二）
excerpt: 本篇會延續上一篇，繼續討論操作DOM元素的方法。
tags: [Javascript, API, DOM] 
categories: [Javascript]
date: 2025-01-06 14:56:42
---

上一篇提到了操作DOM元素的方法，包括對 `classList` 的操作， `add`， `remove` 和 `toggle`。本篇繼續討論 `toggle`。

上一篇傳送門：[Javascript-12-操作DOM元素（一）](https://wooiseong.vercel.app/2025/01/06/JS-12-controlDOM/)

### 2.3. 修改樣式
#### 2.3.3. 通過classList
 ```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .active {
      width: 100px;
      height: 100px;
      background-color: red;
    }
  </style>
</head>
<body>

  <div class="box active">123</div>

</body>
</html>
```

![](/img/JS/JS-13-1.png)

當我們想要移除 `active` 的 `className` ，我們可以使用 `remove`把它移除。

但我們又想要加回去呢？

一些情況下，我們可能希望一個HTML標簽在有 `active`和 沒有 `active` 之間切換。

我們可以不用 `remove` 和 `add` 來來回回使用。
<br>

<font size="5">**直接用 `toggle` 就行了！**</font>
<br>

`toggle` 就像一個開關紐, 

<font color="#46A3FF">**HTML標簽有這個 `className` 我就移除，沒有我就添加**</font>

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" 
  content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .active {
      width: 100px;
      height: 100px;
      background-color: red;
    }
  </style>
</head>
<body>

  <div class="box active">123</div>

  <script>
    const box = document.querySelector('.box')
    box.classList.toggle('active')
  </script>

</body>
</html>
```

![](/img/JS/JS-13-2.png)

當我們執行 `box.classList.toggle('active')` 時，

`div` 標簽有 `active`， 所以 `active` 會被移除。
<br>

### 2.4. 修改表單樣式
表單也很多情況需要修改屬性，

比如表單沒填寫，“提交” 按鈕會被禁止提交，直到表單填寫完成。

表單自帶的屬性，`value` 和 `type` 可以利用DOM修改。

```html
<body>

  <input type="text" placeholder="請輸入名字" class="testing">

</body>
```
![](/img/JS/JS-13-3.png)
<br>

我們可以修改 `input` 的值
```html
<body>

  <input type="text" placeholder="請輸入名字" class="testing">

  <script>
    const testing = document.querySelector('.testing')
    testing.value = '123'
  </script>

</body>
```
![](/img/JS/JS-13-4.png)
<br>

也可以修改 `input` 的類型
```html
<body>

  <input type="text" placeholder="請輸入名字" class="testing">

  <script>
    const testing = document.querySelector('.testing')
    testing.type = 'radio'
  </script>

</body>
```
![](/img/JS/JS-13-5.png)
<br>

除了這些，有布爾值的屬性也可以修改，比如 `disabled`， `checked`， `selected`。

```html
<body>

  <button>點擊我</button>

</body>
```
![](/img/JS/JS-13-6.png)
<br>

`<button>` 有一個 `disabled` 屬性，值默認是 `false`。我們可以直接用布爾值來修改：


```html
<body>

  <button>點擊我</button>

  <script>
    const btn = document.querySelector('button')
    btn.disabled = true
  </script>
</body>
```
![](/img/JS/JS-13-7.png)


### 2.5. 自定義屬性
我們知道HTML標簽自帶一些屬性，比如 `input` 自帶 `type` 和 `placeholder` 屬性。

我們還可以添加自定義屬性， `data-自定義="值"` 這樣方式。
```html
<body>
  <div data-id="1">1</div>
  <div data-id="2">2</div>
  <div data-id="3">3</div>
  <div data-id="4">4</div>
  <div data-id="5">5</div>
</body>
```

這樣每個 `div` 綁定有特殊值的自定義屬性。

```javascript
const div = document.querySelector('div')
console.log(div) // <div data-id="1">1</div>
console.log(div.dataset.id) // 1
```

`document.querySelector` 只會選取第一個符合條件的DOM元素。

`div.dataset.id` 就會打印 `data-id` 上面的值 `1`。
<br>

如果我要選取其他的自定義屬性呢？
```javascript
const div = document.querySelector('div[data-id="2"]')
console.log(div) // <div data-id="2">2</div>
```
用 `標簽名[data-自定義="值"]` 這樣方式選取。

自定義屬性的名字是自定的，而且綁定的值也是自定且具唯一性。

因此大大提高操作DOM的方便性。



