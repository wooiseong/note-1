---
title: Javascript-6-變量傳説（一）垃圾回收機制
excerpt: Javascript變量的回收機制，對於如何有效使用變量非常重要。第一步是瞭解垃圾回收機制。
tags: [Javascript, 垃圾回收機制] 
categories: [Javascript]
date: 2025-01-04 16:16:48
---

關於函數的基本介紹可以看回這篇：[Javascript-4-那個函數展開的領域 —— 作用域](https://wooiseong.vercel.app/2025/01/02/JS-4-scope/)。

## 1.1. 前言
不知道你有沒有發現几個問題：

爲什麽for loop使用的變量是 `let` ，而不用 `var` 或則 `const` ？

```javascript
for (let i = 0; i < 10; i++) {
  console.log('wawawa')
}
```
<br>

爲什麽函數可以在聲明前訪問，變量不可以 ？
```javascript
fn() // 1
function fn(){
  let num = 1
  console.log(num) 
}

console.log(num) // Uncaught ReferenceError: 
// Cannot access 'num' before initialization
let num = 1
```

在這個篇章，會解答這些問題。

## 1.2. 垃圾查找機制
Javascript環境中，内存的分配有以下生命周期：
1. 内存分配： 聲明變量，函數和對象的時候，系統會自動分配内存
2. 内存使用： 讀寫内存，使用變量和函數等
3. 内存回收： 使用完畢，垃圾回收器自動回收不再使用的内存

全局變量一般不會回收，除了關閉頁面。而一般情況下，局部變量的值，不用了會自動被回收掉。

舉例來説，在函數聲明的變量，就會在函數執行完就銷毀。這解釋爲什麽外面無法訪問函數内部變量：
```javascript
function fn(){
  let num = 1
}
fn()
console.log(num)
```
![](/img/JS/JS-4-2.png)  

如果内存有些原因沒有釋放，就稱作<font color="#46A3FF">**内存泄漏**</font>

垃圾回收機制可以分爲：引用計數法和標記清楚法

### 1.2.1. 引用計數法
這種計數法看一個對象是否有指向它的引用，看的是“不再使用的對象”。

有的話一次引用會累加1，減少引用一次會減1。引用次數是0,會釋放内存。

但是，如果兩個對象相互引用（嵌套引用），即便不再使用。

垃圾回收器不會進行回收，導致内存泄漏。
```javascript
function fn(){
  let obj1 = {}
  let obj2 = {}
  obj1.a = obj2
  obj2.a = obj1
}
fn()
```


### 1.2.2. 標記清楚法
這種計數法的出現解決引用計數法的問題，看的是“無法達到的對象”。

系統從根部出發掃描内存對象，凡是從根部到達的，都還是要用的。

無法從根部出發觸及的對象會被標記為不再使用，接著進行回收。

![](/img/JS/JS-6-1.png)  

就如上面的那個 `fn` 函數，裏面的對象 `obj1` 和 `obj2` 雖然相互引用，但是它們就像上圖的紅色框的unreachables，

從根部 `<global>` 無法觸及，所以會被回收。
<br>

知道了作用域和這個變量回收機制，我們就會在下一篇說到Javascript閉包的概念。
<br>