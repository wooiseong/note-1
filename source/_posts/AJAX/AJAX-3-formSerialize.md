---
title: AJAX-3-收集表單資料的神器 ———— form-serialize插件
excerpt: 本篇討論使用form-serialize插件快速收集表單數據，提交至後臺。
tags: [Javascript, AJAX, form-serialize] 
categories: [Javascript]
date: 2025-01-23 15:37:54
---

## form-serialize插件介紹
表單數據提交是很常需要做的工作，只有幾個數據當然很容易就提交了。

如果是好幾項數據呢？每個都要用 `document.querySelector` 獲取其 `value` 屬性也太麻煩了！

我們可以利用插件幫我們直接收集全部數據： form-serialize。

1. 插入form-serialize.js
```javascript
<script src="
https://cdn.jsdelivr.net/npm/form-serialize@0.7.2/index.min.js
"></script>
```

2. 引入 form-serialize.js 文件

詳細内容看這份在npm的[官方説明](https://github.com/defunctzombie/form-serialize)。

可以使用 `npm` 或者直接下載引用。

![](/img/AJAX/AJAX-3-1.png)

{% note warning %}
如果有報錯：
`Uncaught ReferenceError: module is not defined`

就把文件的最後一行 `module.exports = serialize;` 注釋
{% endnote %}
<br>

3. 使用form-serialize插件收集數據
<font size="5">`serialize(表單DOM, { hash: true, empty: true })`</font>
<br>
<br>

拿上一篇當成例子：
```html
<body>  
  <form action="javascript:;" class="formSubmit">
    <input class="name" name="username" type="text" placeholder="請輸入名字">
    <input class="password" name="password" type="password" placeholder="請輸入密碼">
    <button class="btn">提交</button>
  </form>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.7.8/axios.min.js"></script>
  <script src="index.js"></script>
</body>
```
<br>

我們直接獲取表單的DOM元素，用`form-serialize`插件來收集數據。

當我們輸入完資料點擊提交后：

```javascript
  <script>
    const btn = document.querySelector('.btn')

    btn.addEventListener('click', () => {
      const form = document.querySelector('.formSubmit')
      const dataCollected = serialize(form, { hash: true, empty: true })
      console.log(dataCollected)   
      // {username: 'wooiseong96', password: '123456'}
    })
  </script>
```

打印的結果就直接是對象了，裏面有我們定義的屬性和值。

{% note info %}
- form-serialize依著 `input` 的 `name` 屬性收集數據，沒有這個屬性就無法收集數據。

- 我們可以命名 `name` 屬性時，直接依照接口文件要的參數名字，就不用更改這麽麻煩。 
{% endnote %}

我們發送的axios的 `data` 的内容本來就是對象，我們可以直接把 `dataCollected` 這個變量丟進去：
```javascript
  <script>
    const btn = document.querySelector('.btn')

    btn.addEventListener('click', () => {
      const form = document.querySelector('.formSubmit')
      const dataCollected = serialize(form, { hash: true, empty: true })
     //  console.log(dataCollected)  

      axios({
        url: 'http://hmajax.itheima.net/api/register',
        method: 'POST',
        data: dataCollected
      }).then((result) => {
        console.log(result)
      }).catch((error) => {
        console.log(error)
        console.log(error.response.data.message)
      }) 
    })
  </script>
```
<br>

我們説了這麽多，那 `hash` 和 `empty` 是什麽呢？

 `hash` 
- true: Javascript對象
- false: URL的查詢字符串

`empty` 
- true: 獲取空值
- false: 不獲取空值
<br>

以上面的例子，我們來試試看：
```javascript
// 輸入username和password
 const dataCollected = 
 serialize(form, { hash: true, empty: true }) 
 //{username: 'wooiseong', password: '123456'}

 const dataCollected = 
 serialize(form, { hash: false, empty: true }) 
 //username=wooiseong&password=123456


// 輸入一個空鍵
 const dataCollected = 
 serialize(form, { hash: true, empty: true }) 
 //{username: ' ', password: ' '}
 
 const dataCollected = 
 serialize(form, { hash: true, empty: false }) 
 //username=+&password=+
```