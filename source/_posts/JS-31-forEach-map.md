---
title: Javascript-31-遍歷的藝術 ———— forEach 和 map
excerpt: 本篇討論Javascript用於遍歷的方法： forEach和map。
tags: [Javascript, forEach, map] 
categories: [Javascript]
date: 2025-01-17 08:40:08
---

## 1. 前言
在編程中，遍歷是最長需要執行的功能之一。

因爲一些方法只能執行第一次，因此把方法執行至每一個數組的元素就變得非常重要。

擧個例子,我要打印數組的索引號和元素的值：
```javascript
const arr = ['A','B','C']

for (let i = 0; i < arr.length ;i++){
  console.log('索引號是', i, '元素是', arr[i])
}
```

![](/img/JS/JS-31-1.png)
<br>

問題來了，如果我們執行的不是打印 `console.log()` 這麽簡單的方法，

而是封裝好的函數，那麽這樣的遍歷方法就無法滿足需求了。
<br>

for loop包 for loop是一個方法，但這樣的方法容易造成性能的問題，而且，萬一一個for loop有問題就造成了整個程式的錯誤。

我們有兩種常用於遍歷的方法： `forEach` 和 `map` 。
<br>

## 2. 遍歷的方法

### 2.1. forEach
這個方法用於調用數組的每個元素，將元素傳遞給回調函數。

語法如下：
<font size="5">`數組.forEach(function(當前數組元素，當前元素索引號){})`</font>
<br>

`forEach`的回調函數會接受兩個形參： `item`, `index`。

我們可以嘗試這樣使用：
```javascript
const arr = ['A','B','C']

arr.forEach(function(item, index){
  console.log('索引號是', index, '元素是',item)
  console.log(item + 'haha')
})
```
![](/img/JS/JS-31-2.png)

{% note danger %}
`forEach` 參數 `item` ，也就是數組元素一定要寫；`index` 是可選的。
{% endnote %}
<br>

### 2.2. map
`map` 是另一種遍歷方法，和 `forEach` 一樣可以遍歷數組，返回新的數組。

<font size="5">`const 新數組 = 數組.map(function(當前數組元素，當前元素索引號){})`</font>
<br>

```javascript
const arr = ['A','B','C']

const newArr = arr.map(function(item, index){
  return item + 'haha'
})

console.log(newArr)
```

![](/img/JS/JS-31-3.png)
<br>

{% note info %}
`map` 與 `forEach` 最大的不同是 `map` 會返回新數組，

`forEach` 是直接改動原有數組，并不會返回新數組。
{% endnote %}
<br>

因此，`map` 個人是比較推薦使用的，因爲有些情況新舊數組都需要保留。
<br>