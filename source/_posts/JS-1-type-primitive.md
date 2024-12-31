---
title: Javascript-1-數據類型(一)
excerpt: Javascript的數據類型可以分爲基本數據和引用數據兩類
tags: [Javascript, 數據類型]
categories: [Javascript]
date: 2024-12-30 16:28:13
---

# 1. Javascript數據類型 (一)
Javascript數據類型可以分爲簡單和複雜類型，本文會先討論簡單數據類型。

## 1.1. 簡單數據
又稱基本數據類型，包括 數字型(number),字符串型(string), 布爾型(boolean), 未定義型(undefined), 空類型(null)
<br>

### 1.1.1. 數字型 (number)
因爲Javascript是弱數據類型，所以變量只有賦值后才知道是哪種類型。

可以是整數，小數，正數，負數。

```javascript
let num  // Javascript
int num = 10  // Java, num的值必須是數字
```

計算過程中可能會出現`NaN`，即Not a Number，代表一個計算錯誤：
- 不正確的數學操作
- 未定義的數學操作

任何對NaN的操作都會返回NaN

```javascript
console.log('A' - 10)  // NaN
console.log(NaN - 10)  // NaN
```
<br>

### 1.1.2. 字符串型 (string)
一段字串，用單引號或雙引號括起來。推薦單引號爲主。

單引號和雙引號可以互相嵌套。必要時可以用轉義符。

```javascript
console.log('A是"Apple"的縮寫')  // A是"Apple"的縮寫
console.log('A是\'Apples\'的縮寫') // A是'Apples'的縮寫
```

字符串可以用加號`+`連接：
```javascript
console.log('A是' + 'Apple')  // A是Apple
```

#### 1.1.2.1 模板字符串
在字符串中，可以用反引號``括起來，用`${}`插入變量：
```javascript
let a = 'Apple'
console.log('A是' + `${a}`)  // A是Apple
```
<br>

### 1.1.3. 布爾型 (boolean)
只有兩個值：true和false。
<br>

### 1.1.4. 未定義型 (undefined)
會出現的原因主要是因爲聲明變量，但不賦值。

```javascript
let a 
console.log(a)  // undefined
```


### 1.1.5. 空類型 (null)
有聲明，有賦值，但值為空。

```javascript
let a = null
console.log(a)  // null
```

undefined與null的差別是：undefined沒有賦值，null是賦值了，但值為空。

所以兩者都進行運算時，結果會不一樣
```javascript
console.log(undefined + 1)  // NaN
console.log(null + 1)  // 1
```

#### null的作用
null主要應用場景是聲明一個變量，但對象還沒有創建好。等創建了賦值給它。
<br>

### 1.1.6. typeof操作符
typeof操作符可以判斷變量的數據類型：
```javascript
let a = 10
console.log(typeof a)  // number
```
<br>

### 1.1.7. 類型轉換
Javascript是弱數據類型，所以獲取的數據需要仔細判斷數據類型，尤其是Javascript的數據類型轉換。

#### 1.1.7.1. 隱式轉換
運算符執行時，系統内部會自動轉換數據類型，所以稱爲"隱"式轉換。

- 規則1：`+`只要一邊是字符串，另一個也會變成字符串，其他運算符則會轉成數字類型。

```javascript
console.log(2 + 2) // 4
console.log(2 + '2') // 22
console.log(2 - 2) // 0
console.log(2 - '2') // 0
console.log(+'2') // 2 (number)
```

{% note info %}
輸入框的内容是<font color="	#46A3FF">**字符串**</font>。在輸入框`input`接收的變量可以放`+`，把字符串轉換成數字。
{% endnote %}

#### 1.1.7.2. 顯式轉換
直接把數據類型轉換成其他數據類型，所以稱爲"顯"式轉換。

```javascript
let a = '123'
console.log(Number(a))  // 123

let a = 'A'
console.log(Number(a))  // NaN
```

小數也可以轉換成整數,`parseInt`會自動忽略非數字：
```javascript
let a = '123.12px'
console.log(parseInt(a))  // 123
console.log(parseFloat(a))  // 123.12
```