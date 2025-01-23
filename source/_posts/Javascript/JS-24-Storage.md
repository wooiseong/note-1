---
title: Javascript-24-本地儲存介紹
excerpt: 本篇討論BOM的定義，以及屬於BOM的對象。
tags: [Javascript, localStorage] 
categories: [Javascript]
date: 2025-01-14 14:01:56
---

## 1.1. 前言
我們在登入頁面，或者填寫重要的表格時，一刷就會不見。

實際上，Javscript提供了將數據永久保存在本地的方法。

除非手動刪除，否則關閉頁面都會存在。

其中最常用的是 `localStorage`。


## 1.2. localStorage 
這是一種本地存儲數據的方法。

特性：
- 多窗口共享本地數據
- 以鍵值對的形式保存 (就是一個key存一個數值)

我們可以對數據進行特定操作。

### 1.2.1. 簡單數據
- 儲存數據<br>
<font size="5">`localStorage.setItem(鍵，值)`</font>

我們來試試看：
```javascript
localStorage.setItem('name', 'pink')
```
保存的數據可以在瀏覽器 `Application` 部分找到：

![](/img/JS/JS-24-1.png) 
<br>

- 獲取數據
如果我們想要使用已經保存的數據：<br>
<font size="5">`localStorage.getItem(鍵)`</font>

```javascript
// 獲取數據
const nameLocal = localStorage.getItem('name')

// 打印變量
console.log(nameLocal)  // pink
```
<br>

- 移除數據
如果我們想要移除已經保存的數據：
<font size="5">`localStorage.removeItem(鍵)`</font>

```javascript
// 移除數據
localStorage.removeItem('name')
```

### 1.2.2. 複雜數據
簡單數據可以直接以字符串的形式保存。

但是複雜數據不能。


- 保存數據
我們需要其他的方法保存：<font color="#46A3FF">**把複雜數據轉成JSON字符串保存**</font>。<br>JSON字符串是什麽這裏先跳過，在以後的篇章會再詳談。<br><br><font size="5">`localStorage.setItem(鍵, JSON.stringify(值))`</font>

```javascript
// 儲存數據
const obj = {
  name: 'pink',
  age: 18
}
localStorage.setItem('obj', JSON.stringify(obj))
```
![](/img/JS/JS-24-2.png) 


- 獲取數據
需要將JSON字符串以 `JSON.parse` 轉換成對象才可以使用。<br>
<font size="5">`JSON.parse(localStorage.getItem('obj'))`</font>

```javascript
// 獲取數據
const objLocal = JSON.parse(localStorage.getItem('obj'))

console.log(objLocal) // { name: 'pink', age: 18 }
```

- 移除數據
和簡單數據的寫法一樣：
```javascript
// 移除數據
localStorage.removeItem('obj')
```

