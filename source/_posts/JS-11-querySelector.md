---
title: Javascript-11-獲取DOM
excerpt: 本篇會討論獲取DOM的方法和注意事項。
tags: [Javascript, API, DOM] 
categories: [Javascript]
date: 2025-01-05 20:33:02
---

上一篇提到了DOM的基本結構，接下來會討論如何獲取DOM。

## 1.1. 獲取單個DOM元素
獲取單個DOM元素有很多方法，這裏討論比較常用的幾個方法。

### 1.1.1. className獲取DOM
```html
<body>
  <div class="testing">123</div>
  <script>
    const div = document.querySelector('.testing')
  </script>
</body>
```

DOM對象 `document` 有一個方法叫 `querySelector`，可以把className（類） `testing` 當成參數，便可以獲取DOM元素。

一般我們會把獲得的DOM元素賦值給一個變量，比如上面的 `div` ，我們再操作那個變量。這樣寫會比較簡潔。

### 1.1.2. HTML標簽獲取DOM
```html
<body>
  <div class="testing">123</div>
  <script>
    const div = document.querySelector('div')
  </script>
</body>
```

這個方法是可以把HTML標簽 `div` 當成`document.querySelector`的參數，便可以獲取DOM元素。

但如果網頁很多同樣的HTML標簽，這個獲取方法容易遇到問題：
```html
<body>
  <div class="testing">123</div>
  <div class="testing2">456<div>
  <script>
    const div = document.querySelector('div')
    console.log(div) // <div class="testing">123</div>
  </script>
</body>
```

`querySelector` 方法，只可以獲取第一個符合條件的DOM （條件是有 `testing` 這個className），

如果有多個符合條件的DOM，只會獲取第一個。

所以不能獲取 `<div class="testing2">456<div>`。

只要獲取第二個 `div` 標簽，其實是可以辦到的，後面會學到。

className比起標簽獲取DOM，特異性更高。

我們可以給那個HTML標簽唯一的className，所以一般用className獲取DOM。

### 1.1.3. id標簽獲取DOM
除了上面兩種方法，還有一種是id獲取DOM：
```html
<body>
  <div id="testing">123</div>
  <script>
    const div = document.querySelector('#testing')
  </script>
</body>
```
這個方法是可以把id標簽 `testing` 當成`document.querySelector`的參數，便可以獲取DOM元素。

這種id獲取DOM的方法一般比較少用。
<br>

{% note danger %}
className前面需要加 `.`，不然無法獲取DOM。

用HTML標簽，標簽名前面則不用加 `.`。

用id標簽，id名前面需要加 `#`。
{% endnote %}
<br>

## 1.2. 獲取多個DOM元素
如果我們要獲取多個DOM元素，可以使用 `querySelectorAll` 方法。

### 1.2.1. className獲取多個DOM元素
```html
<body>
  <div class="testing">123</div>
  <div class="testing">456</div>
  <script>
    const divs = document.querySelectorAll('.testing')
    console.log(divs)
  </script>
</body>
```
![](/img/JS/JS-11-1.png)  

`querySelectorAll` 會返回一個 `NodeList` ，也被稱爲僞數組。

這個僞數組類似數組，裏面會有所有符合條件的DOM元素集合（條件是都有 `testing` 這個className），但是它並不是真正的數組。
<br>
{% note info %}
僞數組和數組共同點： 都有 `length` 和 `index`。

僞數組和數組不同點： 僞數組沒有數組的方法，比如 `push`、`pop`、`shift`、`unshift`、`map`、`filter`、`reduce` 等等。
{% endnote %}
<br>

此外，僞數組裏的每個DOM元素也是不可以直接修改屬性的，需要通過遍歷的方式一個一個修改。下一篇會再詳談。
<br>
<br>
我們已經討論了獲取DOM元素的方法，下一步就是修改DOM元素。

關於怎麽修改，可以修改什麽部分，下一篇會討論，敬請期待。








