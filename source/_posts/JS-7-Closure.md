---
title: Javascript-7-變量傳説（二）閉包
excerpt: Javascript閉包是一個函數對周圍狀態的引用綁在一起，内層函數訪問到外層函數的作用域。
tags: [Javascript, 閉包] 
categories: [Javascript]
date: 2025-01-04 20:09:41
---

關於作用域的介紹可以看回這篇: [Javascript-4-那個函數展開的領域 —— 作用域](https://wooiseong.vercel.app/2025/01/02/JS-4-scope/)。

關於函數的垃圾回收機制可以看回這篇：[Javascript-6-變量傳説（一）垃圾回收機制](https://wooiseong.vercel.app/2025/01/04/JS-6-GarbageCollection/)。

## 1.1. 什麽是閉包 (Closure)
從上面兩篇，我們可以知道作用域存在于函數中，裏面的變量在函數執行完就銷毀。

如果我們想要外部訪問函數的變量，該怎麼辦呢？

我們可以使用閉包。閉包是一個函數對周圍狀態的引用綁在一起，内層函數訪問到外層函數的作用域。

簡單來説， 閉包 = 内層函數 + 外層函數的變量
```javascript
function outer() {
    let a = 1
    function inner() {
      console.log(a)
    }
    return inner
}
const fn = outer()
fn() //1
```

外層函數是 `outer`的， 内層函數是 `inner`。外層函數的變量是 `let a = 1`。

當我們想要從外部訪問外層函數的那個變量時，我們可以 `return inner`函數，那個有訪問它外層 `let a = 1`的函數。

當我們調用 `fn`時，相等於調用 `inner` 函數，因爲 `outer() = inner`，

這樣可以封閉 `outer`變量，從外部訪問它。實現數據的私有。

試想想：如果 `let a = 1` 直接聲明在外面，成爲全局變量，那很容易就被修改。
<br>

```javascript
let a = 2
function outer() {
    let a = 1
    function inner() {
      console.log(a)
    }
    return inner
}
const fn = outer()
fn() //1
```
比如說，我們聲明一個跟 `a` 一樣的變量，並賦值。`fn()` 結果依然是 `1`，

因爲閉包讓 `outer`的 `let a = 1`被局部作用域“圈”在自己的局部作用域，外部 `fn` 的訪問不會影響這個 `a` 的值。 
<br>

{% note info %}
閉包允許將函數與其所操作的某些數據（環境）綁定在一起，
這樣就可以在這些數據不變的情況下重複使用這些函數。
{% endnote %}
<br>

在瞭解了閉包的作用后，下一篇會解答在“Javascript-6-變量傳説（一）垃圾回收機制”的問題，即聲明變量的位置和能否訪問的關係。

敬請期待。