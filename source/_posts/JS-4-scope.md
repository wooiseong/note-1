---
title: Javascript-4-那個函數展開的領域 ———— 作用域
excerpt: 學習函數，就必須要知道函數變量訪問的原則，才不會造成變量污染或者訪問不到變量的囧況。
tags: [Javascript, 函數, 作用域] 
categories: [Javascript]
date: 2025-01-02 21:08:27
---

關於函數的基本介紹可以看回這篇：[Javascript-3-數據類型(三) 複雜數據(函數)](http://localhost:4000/2025/01/01/JS-3-complex-md/)。

# 1.1. 前言
當我們開始使用函數時，常常遇到兩種報錯：

（一）想要訪問for loop裏的`i`
```javascript
for (let i = 0; i < 10; i++) {
  console.log('wawawa')
}
console.log(i)
```
本來想在函數外訪問函數内部變量，結果無法顯示，反而報錯：
![](/img/JS/JS-4-1.png)  

（二） 訪問函數裏的變量
```javascript
function fn(){
  let num = 1
}
fn()
console.log(num)
```
![](/img/JS/JS-4-2.png)  

原話翻譯上面的報錯，就是`i`和`num`這兩個變量沒有被聲明，所以無法訪問。

咦，在for loop和函數不是已經聲明了嗎？為什麼還會報錯呢？這和<u>作用域</u>有關。

一段代碼不是總有效和可用的，限定這個可用性的代碼範圍的就是<font color="#46A3FF">**作用域**</font>。

要理解這個概念，可以通過《咒術迴戰》的領域展開。

![「新陰流·簡易領域」](/img/JS/JS-4-3.jpg) 












