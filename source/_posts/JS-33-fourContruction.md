---
title: Javascript-33- 談對象（二） 内置構造函數 
excerpt: 本篇討論Javascript各種數據類型的内置構造函數。
tags: [Javascript, Contructor] 
categories: [Javascript]
date: 2025-01-18 20:15:35
---

上篇我們解説了構造函數的概念，以及對象成員的類別。沒看過的可以看[Javascript-32- 談對象（一） 創建對象 ](https://wooiseong.vercel.app/2025/01/17/JS-32-construction/)。

這一篇會討論Javascript各種數據類型的内置構造函數，以及它們的方法。

我們便可以用這些方法來幫助我們實現功能。


## 1. 包裝類型
基本類型，像是字符串，數值，布爾值等都有專門的構造函數，被稱爲包裝類型。

舉例來説
```javascript
const str = 'A'
console.log(str.length) // 1
```

如果説構造函數有靜態方法，構造函數創造的實例有實例方法，

那簡單數據類型哪裏來的 `length` 方法？

這是因爲當我們把 `A` 賦值給我們的變量 `str` 時，Javascript子底層已經把簡單數據用構造函數包裝成引用數據（對象）。

因此可以調用内置的 `length` 方法。

這是Javascript底層運作的方式：
```javascript
const str = 'A' // const str = new String('A')
console.log(str.length) // 1
```
<br>

## 2. 内置構造函數
### 2.1. Object
`Object` 有幾個常用靜態方法，我們可以來看看：

- `Object.keys` 獲取對象所有屬性 （鍵）
```javascript
const student = { name: 'A', age: 18 }

const arr = Object.keys(student)

console.log(arr) // ['name', 'age']
```
這個方法會返回一個數組，數組有所有對象的屬性。
<br>

- `Object.values` 獲取對象所有屬性 （鍵）的值
```javascript
const student = { name: 'A', age: 18 }

const arr = Object.values(student)

console.log(arr) // ['A', 18]
```
這個方法會返回一個數組，數組有所有對象的屬性的值。
<br>

- `Object.assign` 可以用於對象拷貝，<br>
<font size="5">`Object.assign(新對象，舊對象)`</font><br><br>

```javascript
const student = { name: 'A', age: 18 }
const newStudent = {}

Object.assign(newStudent,student)

console.log(newStudent) //  { name: 'A', age: 18 }
```
這個方法會返回被拷貝對象的新對象。
<br>

這個方法通常的應用場景，是給對象添加新屬性：

```javascript
const student = { name: 'A', age: 18 }

Object.assign(student, { gender: 'male' })

console.log(student) //  { name: 'A', age: 18, gender: 'male' }
```

### 2.2. Array
`Array` 的方法我們也曾經經常使用，比如 `forEach` 是我們之前使用過的方法。

除了 `forEach` ，有三個我們常用的方法：

- `reduce` 可以纍計處理的結果<br>
<font size="5">`arr.reduce(function(上次的值，當前的值){}, 起始值)`</font><br><br>


```javascript
const arr = [1,2,3,4]

const result = arr.reduce(function(prev, current){
  return prev + current
},0)

console.log(result) // 10
```

返回值是處理后的結果，在處理函數需要 `return` 把值返回。

第一次執行 `reduce`， `prev` 是 `0`，`current` 是 `1`，所以 `prev + current` 等於 `1`。

第二次執行 `reduce`，`prev` 是 `1`，`current` 是 `2`，所以 `prev + current` 等於 `3`。

以此類推，最後返回 `10`。
<br>
除了上面提到的方法，還有很多方法可以幫助我們實現功能：

![](/img/JS/JS-33-1.png)

### 2.3. String
字符串的方法我們之前也使用過，比如 `length` 或者 `replace` ， 

以下是常用的字符串方法整理：

![](/img/JS/JS-33-2.png)
<br>

- `split` 可以把字符串在特定地方切開，返回有切割後的元素的數組

```javascript
const str = 'I,am,A'
const strSplit = str.split(',')

console.log(strSplit) // ['I', 'am', 'A']
```
<br>

- `substring` 截取需要的字符串，返回截取後的字符串<br>
<font size="5">`str.substring(開始的索引號，結束的索引號)`</font><br>
*結束的索引號的字符串不包含在返回的字符串內
<br><br>

```javascript
const str = 'I,am,A'
const strSub = str.substring(0,2)

console.log(strSub) // 'I,'
```
<br>

### 2.4. Number 

`Number` 的構造函數，包含我們已經學過的 `Number.parseInt()` 和 `Number.parseFloat()` ，

可以參考這一篇：[Javascript-1-數據類型(一) 簡單數據 > 1.1.7. 類型轉換](http://localhost:4000/2024/12/30/JS-1-type-primitive/) 
<br>
