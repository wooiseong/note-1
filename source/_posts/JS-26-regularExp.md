---
title: Javascript-26-正則表達式
excerpt: 本篇討論Javascript匹配字符串組合的模式，也就是正則表達式。
tags: [Javascript, Regular Expression] 
categories: [Javascript]
date: 2025-01-14 21:36:57
---

## 1.1. 前言

在填寫表單時，我們如果輸入不合法的資訊，就會被阻擋，而且表單不會提交。

![](/img/JS/JS-26-1.png) 

校驗是前端非常重要的工作，因爲如果表單輸入的内容不合法，

表單傳到後端卻不能往後處理，其實會增加後端的負擔。
<br>

Javascript有一套匹配字符串組合的模式，

白話來說，電子郵件的格式是 `xxx@mail.com`,而 `xxx` 是英文和數字的組合。

不符合這樣匹配規則的輸入内容就是不合法内容。

這個匹配規則就是正則表達式。
<br>

## 2.1. 正則表達式 （Regular Expression）

<font size="5">**`const 正則變量名 = /表達式/`**</font>
<br>

正則表達式可以使用 `test()` 方法來進行驗證

<font size="5">**`正則變量名.test(被驗證變量)`**</font>
<br>

```javascript
    const str = '前端的内容包含CSS和Javascript'

    // 定義正則表達式
    const reg1 = /Javascript/
    const reg2 = /JS/

    // 是否匹配
    const result1 = reg1.test(str)
    console.log(result1) // true

    const result2 = reg2.test(str)
    console.log(result2) // false
```

正則表達式返回值是 `true` 或者 `false`， 分別代表匹配，或者不匹配。
<br>

## 2.2. 檢索 （Execute）
我們除了需要正則表達式是否匹配的結果，

也需要檢索出符合正則表達式的字符串。

我們可以使用 `exec()` ，在指定字符串中執行搜索匹配，

如果匹配成功會返回一個包含符合搜索内容數組。

```javascript
const str = '前端的工作和前端的學習息息相關'

const reg = /前端/
console.log(reg.exec(str))
```
![](/img/JS/JS-26-2.png)

數組會有被成功檢索的字符串的相關信息。

相信大家也注意到了，這個數組只能返回被檢索的第一個字符串，

上面的字符串有兩個“前端” 字眼，但只有第一個被返回了。

我們需要元子符的幫助。


## 2.3. 元字符 （Metacharacters）
元字符也是字符的一種，但它們是有特殊含義的字符，

可以提高我們正則表達式的匹配能力。

參考文檔可以看：[Character classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions/Character_classes)

元字符根據類別可以分類爲三種： 邊界符， 量詞和字符類

### 2.3.1. 邊界符
規定開頭和結尾必須用什麽。

`^` 表示匹配行首的文本

`$` 表示匹配行尾的文本
<br>

如果兩個一起使用，就表示頭尾都必須匹配。

```javascript
const str = '前端相關'

const reg1 = /^前端/
console.log(reg1.test(str)) // true

const reg2 = /相關$/
console.log(reg2.test(str)) // true

const reg3 = /^前端$/
console.log(reg3.test(str)) // false

const reg4 = /^前端相關$/
console.log(reg4.test(str)) // true
```



### 2.3.2.量詞
表示可重複次數

![](/img/JS/JS-26-3.png)

```javascript
const str = ''

// * 類似 >=0 次
const reg1 = /前*/
console.log(reg1.test(str)) // true

// + 類似 >=1 次
const reg2 = /前+/
console.log(reg2.test(str)) // false

// ? 類似 0 或者 1 次
const reg3 = /前?/
console.log(reg3.test(str)) // true
```

### 2.3.3. 字符類
定義匹配的字符種類，比如英文，數字等

`[abc]` 只選一個符合的 `a` ，`b` ， `c`

`[a-z]` 只選一個符合的小寫英文字母

`[A-Z]` 只選一個符合的大寫英文字母

`[0-9]` 只選一個符合的0-9的數字
<br>

```javascript
const str = 'xab'

const reg1 = /[abc]/
console.log(reg1.test(str)) //true

const reg2 = /[a-z]/
console.log(reg2.test(str)) //true

const reg3 = /[A-Z]/
console.log(reg3.test(str)) //false 

const reg4 = /[0-9]/
console.log(reg4.test(str)) //false
```
<br>

以上的規則都可以組合在一起，比如：

`const reg4 = /[A-Z][a-z][0-9]/` 匹配大小寫英文字和數字


我們這樣寫的確是可以的，但這些規則實際上有更簡潔的寫法：
![](/img/JS/JS-26-4.png)

### 2.3.4. 修飾符
約束執行的行爲，比如是否區分大小寫等。

寫在是寫在正則表達式的後面：

<font size="5">`/正則表達式/修飾符`</font>
<br>

`i` 是 `ignore` ，正則表達式不區分大小寫

```javascript
// 沒有 i 區分大小寫
const reg1 = /Alice/
console.log(reg1.test(str)) // false

// 有 i 不區分大小寫
const reg2 = /Alice/i
console.log(reg2.test(str)) // true
```
<br>

`g` 是 `global` ，正則表達式匹配所有滿足條件的字符串

回到 “2.2. 檢索” 的問題, `exec` 只能檢索第一個匹配的字符串，

配合 `g` 可以檢索所有匹配的字符串了。

```javascript
  const reg = /Alice*/g
  const str = 'My name is Alice, I love Alice'
  let array

  while ((array = reg.exec(str)) !== null) {
  console.log(`Found ${array[0]}. Next starts at ${reg.lastIndex}.`)
  }

```
![](/img/JS/JS-26-5.png)


### 2.3.5. 替换文本
正則表達式除了查找匹配結果外，也可以把匹配的字符串替換成想要的文本。

<font size="5">`字符串.replace(/正則表達式/, '替換文本)'`</font>
<br>

```javascript
const str = 'My name is Alice, I love Alice'
const reg = /Alice/g
const strReplaced = str.replace(reg, 'Bob')
console.log(strReplaced) // My name is Bob, I love Bob
```
<br>
<br>