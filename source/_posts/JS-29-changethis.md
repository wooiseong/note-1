---
title: Javascript-29- this的傳説（二）改變this
excerpt: 本篇討論如何改變this的指向。一共有三個方法： call(), apply() 和 bind()。
tags: [Javascript, this, call, apply, bind] 
categories: [Javascript]
date: 2025-01-16 19:42:20
---

我們在上一篇 [Javascript-28-this的傳説(一) 什麽是this](https://wooiseong.vercel.app/2025/01/15/JS-28-this/) 討論了 `this` 的概念。

我們在這一篇會討論如何改變 `this` 的指向。

## 1.1. call()
基本語法如下：
<font size="5">`函數.call(調用對象，參數)`</font>
<br>

```javascript
const obj = {
    name: 'A'
}

function fn(x,y){
  console.log(this) //{name: "A"}
  console.log(x+y) //3
}

//調用函數
fn.call(obj,1,2)
```
在調用 `fn` 時，我們用 `call` 方法，可以把 `this` 指向 `obj`。
<br>

## 2.1. apply()
這個方法和 `call()` 很像，差別是 `apply()` 傳入的參數是數組。

基本語法如下：
<font size="5">`函數.call(調用對象，[參數])`</font>
<br>

```javascript
const obj = {
    name: 'A'
}

function fn(x,y){
  console.log(this) //{name: "A"}
  console.log(x+y) //3
}

//調用函數
fn.apply(obj,[1,2])
```

注意的是，不像一般的函數的形參，

我們傳 `[1,2]`， `fn` 的形參要兩個，不是一個。

這樣 `1` 和 `2` 才會被單獨賦值給變量 `x` 和 `y` ，

否則只有第一個形參會被賦予數組第一個元素：

```javascript
const obj = {
    name: 'A'
}

function fn(x){
  console.log(this) //{name: "A"}
  console.log(x) //1
}

//調用函數
fn.apply(obj,[1,2])
```
<br>

## 3.1. bind()
上兩種方法都需要調用函數才可以改變函數 `this` 指向。

`bind()` 可以在不調用函數的情況下改變 `this` 指向。

基本語法如下：
<font size="5">`函數.bind(調用對象)`</font>
<br>

```javascript
const obj = {
    name: 'A'
}

function fn(){
  console.log(this)
}

const fnUse = fn.bind(obj)

//調用函數
fnUse() // { name: 'A' }
```

這個 `bind()` 不會調用 `fn` 。我們調用 `fnUse()` 才會調用 `fn` ，就會看到 `this` 的指向已經被改變成 `obj` 了。

這個方法一般可以使用在不需要調用函數，卻需要改變 `this` 的情況，

比如綁定事件的函數，裏面還包著 `setTimeout`，`setTimeout`裏需要綁定事件的DOM對象。

這時，可以用 `bind()` 在不調用 `setTimeout` 的情況下把`this` 指向目標DOM元素。