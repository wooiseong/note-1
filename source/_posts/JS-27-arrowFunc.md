---
title: Javascript-27-箭頭函數
excerpt: 本篇討論Javascript箭頭函數的概念以及語法。
tags: [Javascript, arrow function] 
categories: [Javascript]
date: 2025-01-15 12:47:34
---

## 1.1. 前言
箭頭函數（arrow function）是ES6中引入的新的函數寫法。這種寫法在React和Vue等框架非常常見。

寫法比原來的函數變得更簡潔。

## 1.2. 箭頭函數語法
普通函數的寫法：
<font size="5">`const 變量 = function (){ 函數體 } `</font>
<br>

箭頭函數的寫法：
<font size="5">``const 變量 = () => { 函數體 } ``</font>
<br>

舉個實際例子：
```javascript
// 普通函數
const fn = function (){
  console.log('hi')
}

// 箭頭函數
const fnArrow = () => {
  console.log('hi')
}
```
<br>

有幾個規則需要特別記住的：
1. 如果形參只有一個，可以省略 `()`
```javascript
const fnArrow = x => {
  console.log(x)
}
fnArrow(1) // 1
```
<br>


2. 如果函數體只有一行代碼，可以省略 `()`， 而且這一行代碼執行結果會作為函數的返回值
```javascript
// 只有一行代碼可以不用 ()，而且帶有return效果
const fnArrow = x => x + 1
console.log(fnArrow(1)) // 2


//代碼寫在()，必須自己return，不然會是undefined
const fnArrow1 = x => {
  x + 1
}
console.log(fnArrow1(1)) // undefined
```
<br>

3. 箭頭函數可以直接返回對象，<font color="#EA0000">**但函數體一定要寫 `()`**</font>
```javascript
const fnArrow = name => ({name: name})
console.log(fnArrow('Roy')) // {name: "Roy"}
```
<br>

4. 普通函數的動態參數 `arguments`， 在箭頭函數不存在，但可以使用 剩餘參數 `...args`<br>
當我們想要傳實參，但不確定有多少個，或者應該賦值給哪一個形參，
普通函數我們可以用函數原有的屬性 `arguments` ：<br><br>
```javascript
const fn = function(){
  console.log(arguments)
}

fn(1,2,3)
```
![](/img/JS/JS-28-1.png)
箭頭函數沒有 `arguments` 屬性，但有一個剩餘參數方法 `...args` 可以替代
```javascript
const fnArrow = (...args) => {
  console.log(args) 
}

fnArrow(1,2,3)
```
![](/img/JS/JS-28-2.png) 
<br>
<br>

箭頭函數用處是替代一般寫匿名函數的地方，比如綁定事件執行的匿名函數。

還有一個更重要的用處是改變環境變量 `this` 的應用方式。
<br>
那什麽是 `this` 呢？ 我們下一章會詳談，敬請期待。
<br>

