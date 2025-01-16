---
title: Javascript-30- Javascript的省力魔法 ———— 解構與賦值
excerpt: 本篇討論如何改變this的指向。一共有三個方法： call(), apply() 和 bind()。
tags: [Javascript, destructing assignment] 
categories: [Javascript]
date: 2025-01-16 20:23:20
---

## 1. 前言
ES6的引進改變了Javascript的寫法，除了我們學過的箭頭函數，

還有一個方便省力的新語法是解構賦值（Destructiion Assigment）

我們來看一個情況：
```javascript
const arr = [3,5,6]

const a = arr[0]
const b = arr[1]
const c = arr[2]
```

我們想要給數組的每個變量賦值給新的變量，

我們就需要一個一個操作，如果這個數組有很多元素，我們一個個寫就會顯得代碼冗長，還容易賦值給錯的變量。

我們可以用解構賦值的方法：
```javascript
const [a,b,c] = [3,5,6]

console.log(a) // 3
console.log(b) // 5
console.log(c) // 6
```

## 2. 解構賦值介紹
解構賦值就是將數組的單元值批量賦值給變量的簡潔語法。

`=` 左邊是聲明的變量，右邊數組的單元值便會一個一個 “解構” ，按照順序進行 “賦值” 操作

{% note warning %}
1. 解構賦值前如果有代碼，必須加 `;` 。
2. 要解構賦值的變量要使用 `let` 賦值；用 `const` 賦值的變量不能解構賦值。
{% endnote %}

## 3. 解構類型

### 3.1. 數組的解構賦值
- 默認賦值
我們知道解構賦值需要一對一把右邊變量解構賦值給左邊的變量，

就會出現兩種情況：

1. 左邊變量比右邊多的情況，可能造成左邊變量為 `undefined` 。

```javascript
const [a,b,c,d] = [3,5,6]
console.log(d) // undefined
```

我們可以給左邊的變量默認值，沒有解構賦值就沿用默認值：

```javascript
const [a,b,c,d=12] = [3,5,6]
console.log(d) // 12
```
<br>
2. 右邊變量比左邊多的情況，或者不確定原來有多少單元值
<br>
<br>

我們可以用剩餘參數 `...args` 的方法：
```javascript
const [a,b,c, ...args] = [3,5,6,7,19]
console.log(args) // [7,19]
```

- 多維數組
解構賦值也支持多維數組,數組可以直接被解構賦值給左邊的變量：
```javascript
const [a,b,c] = [3,[5,6],7]
console.log(a) // 3
console.log(b) // [5,6]
console.log(c) // 7
```

### 3.2. 對象的解構賦值
對象的解構和數組的很相似，就是把右邊的屬性和方法解構賦值給 `=` 左邊的變量。

對象屬性的值會被賦值給與屬性名相同的變量。

擧個例子：
```javascript
    const user = {
      name: 'A',
      age: 19
    }

    // 解構賦值給的對象屬性名字相同
    const {name, age} = user
    console.log(name, age) // A 19

    // 解構賦值給的對象屬性名字不相同
    const {name1, age1} = user
    console.log(name1, age1) // undefined undefined
```

當對象中找不到與變量名一樣的屬性時，變量會變成 `undefined` 。
<br>

如果我就不想要原來的變量名呢？可以在解構賦值時，另外再改。

比如把 `name` 改成 `myName` ：

```javascript
const user = {
  name: 'A',
  age: 19
}

// 解構賦值給的對象屬性名字相同
const {name: myName, age} = user
console.log(myName, age) // A 19
```
<br>

如果複雜一點的對象數組呢？我們也一樣可以直接用解構賦值操作：
```javascript
    const user = [{
      name: {
        mother: 'A',
        father: 'B',
        son: 'C'
      },
      age: 19
    }]

    // 解構賦值給的對象屬性名字相同
    const [{ name: { mother, father, son }, age }] = user
    console.log(mother, father, son, age) // A B C 19
```
<br>

我們學習解構賦值，最重要的應用就是<font color="#46A3FF">**從後臺撈取數據，渲染頁面**</font>。

我們從後臺拿到的數據，通常就是對象數組的形式。

我們拿到的數據，只需要渲染的部分。我們就可以利用解構賦值來操作。
<br>

用回上面的例子：
```javascript
const user = [{
  name: {
    mother: 'A',
    father: 'B',
    son: 'C'
  },
  age: 19
}]

//渲染函數
function renderData([data]){
  console.log(data)
  console.log(data.name)
  console.log(data.name.mother)
}

renderData(user)
```
![](/img/JS/JS-30-1.png)

我們可以直接把 `user` 在 `renderData` 的形參就解構賦值給 `data` （ `[data]` 的形式）。

我們便可以以 `對象.屬性` 的形式，比如 `data.name` 和 `data.name.mother` 的方式渲染我們要的資料了。
<br>

是不是覺得這樣寫非常方便！

如果現在還不瞭解的話沒關係，我們在AJAX技術，串接API時會聊到。到時你就會知道解構賦值的好了😊
<br>
