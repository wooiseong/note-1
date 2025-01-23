---
title: Javascript-8-變量傳説（三）變量提升
excerpt: Javascript使用var和let/const的差別，在於var聲明變量會造成變量提升。
tags: [Javascript, 變量提升] 
categories: [Javascript]
date: 2025-01-04 21:04:02
---

關於函數的基本介紹可以看回這篇：[Javascript-4-那個函數展開的領域 —— 作用域](https://wooiseong.vercel.app/2025/01/02/JS-4-scope/)。

## 1.1. 變量提升
我們如果都先訪問代碼，再聲明變量，會得到什麽結果呢？

```javascript
console.log(num)
var num = 1 // undefined
```

```javascript
console.log(num2)
let num2 = 2 // ReferenceError: Cannot access 
// 'num2' before initialization
```

```javascript
console.log(num3)
const num3 = 3 // ReferenceError: Cannot access 
// 'num3' before initialization
```
<br>

如果用`let`和 `const`先訪問變量，會得到上述報錯，

翻譯過來就是“無法訪問沒有聲明的變量”。代碼是由上而下執行的，如果還沒有變量存在，當然不能被訪問。

而 `var`聲明變量，得到的不是報錯，而是 `undefined` ？？？

這是因爲系統會把所有 `var` 聲明的變量提升到當前作用域的最前面。

上面的代碼，都是寫在全局作用域的，所以變量被提升到了全局作用域的最前面。這個現象被稱爲變量提升。

<font color="#46A3FF">**但是只提升聲明，不提升賦值**</font>。

所以 `var` 聲明的 `num` 被變量提升，可以被訪問，但是因爲沒有提升賦值( `1` )，所以是 `undefined`。

如果是函數作用域呢？同理
```javascript
function fn() {
  console.log(num) 
  var num = 1 
}
fn() // undefined
```

`var` 聲明的 `num` 被變量提升至當前作用域，函數作用域， 但不提升賦值。

理論上，按照邏輯順序，應該是先聲明和賦值變量，才訪問它們。

使用 `var`, 除了對於初學者很難適應外，也很容易有bug。

所以從ES6開始引入使用 `let`和`const`聲明變量。


## 1.2. 函數提升
函數提升，和變量提升類似。函數在聲明前，可以被調用。

```javascript
fn() // 1
function fn() {
  let num = 1
  console.log(num) 
}
```
雖然 `fn` 函數在聲明前就已經調用，這樣并不會報錯，因爲函數在聲明前，會被提升到作用域的最前面。
<br>

但是如果函數被賦值到一個變量，則會報錯：
```javascript
fn() // Uncaught ReferenceError: fn is not defined
fn =function () {
  let num = 1
  console.log(num) 
}
```
還記得我提過的只提升變量，不提升賦值嗎？

把一個匿名函數賦值給變量 `fn` ，并不能在聲明函數前調用函數。

總結來説，函數表達式不存在函數提升的現象。但函數提升使一般具名函數的聲明調用更靈活。
<br>