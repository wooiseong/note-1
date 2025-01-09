---
title: Javascript-14-操作DOM元素（三）
excerpt: 本篇會討論進階的DOM操作，包括查找DOM,刪除和複製DOM節點。
tags: [Javascript, API, DOM] 
categories: [Javascript]
date: 2025-01-09 20:05:05
---

我們學習了獲取DOM，可以使用 `document.querySelector` ，

但是如果我們要的是與它關係的其他DOM呢？

## 1.1 查找父節點

 ```html
<body>
  <div class="dad">
    我是爸爸
    <div class="son">我是兒子</div>
  </div>

  <script>
    const son = document.querySelector('.son')
    console.log(son)
  </script>
</body>
```

![](/img/JS/JS-14-1.png)

<br>
我們獲取的是DOM元素是 `son` , 如果想要它的父級 `father` 的DOM元素呢？

可以這麽寫：

 ```html
<body>
  <div class="dad">
    我是爸爸
    <div class="son">我是兒子</div>
  </div>

  <script>
    const son = document.querySelector('.son')
    console.log(son.parentNode)
  </script>
</body>
```

![](/img/JS/JS-14-2.png)

這樣就可以獲取到 `son` 的父級DOM元素(包括其子元素)了。
<br>

## 1.2 增加節點
我們有時需要在一個子元素，

比如一個 `ul` 的 `li` ， 

在 `ul` 做了處理，就往後追加一個 `li` 的DOM元素。

 ```html
<body>
  <ul class="dad">
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>

  <script>
    const ul = document.querySelector('ul')
    console.log(ul)
  </script>
</body>
```
![](/img/JS/JS-14-4.png)
<br>

我們可以使用 `document.createElement` 創造新節點，

再用`appendChild` <font color="#46A3FF">**插入最後一個子元素後面**</font>。

```html
<body>
  <ul class="dad">
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>

  <script>
    const ul = document.querySelector('ul')
    // 增加新節點
    const li = document.createElement('li')
    li.innerText = '4'

    // 插入新節點
    ul.appendChild(li)
    console.log(ul)
  </script>
</body>
```

![](/img/JS/JS-14-3.png)
<br>

如果我們想要把新創造的DOM元素

插入到特定子節點，

可以使用 `inserBefore( 目標元素 , 哪個元素前 )`

```html
<body>
  <ul class="dad">
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>

  <script>
    const ul = document.querySelector('ul')
    // 增加新節點
    const li = document.createElement('li')
    li.innerText = '4'

    // 追加節點
    ul.appendChild(li, ul.children[1])
    console.log(ul)
  </script>
</body>
```
![](/img/JS/JS-14-5.png)
<br>

## 1.3 克隆節點
複製節點，可以使用 `cloneNode()` 方法，

`cloneNode()` 的括號裏面如果是

`true` 克隆節點，包括裏面的所有屬性和内容

`false`克隆節點的HTML標簽而已（默認）

```html
<body>
  <ul class="dad">
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>

  <script>
    const ul = document.querySelector('ul')
    // 增加新節點
    const li = ul.children[0].cloneNode()

    // 追加節點
    ul.appendChild(li)
    console.log(ul)
  </script>
</body>
```

![](/img/JS/JS-14-6.png)
<br>

如果輸入 `true` 為參數 

`const li = ul.children[0].cloneNode(true)`

結果變成
![](/img/JS/JS-14-7.png)
<br>

## 1.4 刪除節點
刪除節點一定要通過<font color="#46A3FF">**父元素**</font>。

如果沒有父子關係就不能刪除。

```html
<body>
  <ul class="dad">
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
</body>
```
![](/img/JS/JS-14-8.png)
<br>

刪除後就看不見第一個節點了。

```html
<body>
  <ul class="dad">
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>

  <script>
    const ul = document.querySelector('ul')
    // 刪除節點
    ul.removeChild(ul.children[0])
  </script>
</body>
```

![](/img/JS/JS-14-9.png)