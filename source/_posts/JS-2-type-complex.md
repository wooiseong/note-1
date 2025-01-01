---
title: Javascript-2-數據類型(二) 複雜數據(數組和對象)
excerpt: 銜接上一篇簡單數據類型，本文會討論複雜數據類型，包括基本認識，操作方法等。
tags: [Javascript, 數據類型]
categories: [Javascript]
date: 2024-12-31 11:00:00
---

上一篇討論了Javascript簡單數據類型，想瞭解的朋友可以點擊[Javascript-1-數據類型(一) 簡單數據](https://wooiseong.vercel.app/2024/12/30/JS-1-type-primitive/)。

## 1.2. 複雜數據 (complex type)
複雜數據類型，又被稱為引用數據類型，包括數組，和對象。和簡單數據不同，在于值保存的方法不同。在下一篇會再詳細比較兩者的不同。
<br>

### 1.2.1. 數組 (Array)
數組是一種按照順序保存數據的數據類型。

數組保存的一個個數據叫做<font color="#46A3FF">**元素**</font>，聲明語法有兩種：
```javascript
let arrA = new Array(1, 2, 3)
let arr = [1, 2, 3]
```

#### 1.2.1.1. 數組操作 -> 查
從中，我們可以利用一些方法獲取裏面的數據：
- 數組長度 `arr.length` 
- 索引/下標(從0開始) `arr[0]`

```javascript
let arr = [1, 2, 3]
console.log(arr.length) // 3
console.log(arr[0]) // 1
```

想要遍歷每個數組，可以使用for loop。後面還會學習到forEach和map等更便利的方法。

```javascript
let arr = [1, 2, 3]
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i])
}
```

#### 1.2.1.2. 數組操作 -> 改
直接把值傳給寫著索引的數組 `arr[0]` ，就可以修改該元素(`1`變更成`4`)。

```javascript
let arr = [1, 2, 3]
arr[0] = 4
console.log(arr) // [4, 2, 3]
```

#### 1.2.1.3. 數組操作 -> 增
有兩種方法：
- push 往數組最後面添加元素, 返回新數組
- unshift 往數組最前面添加元素，返回新數組

```javascript
// push 方法
let arr = [1, 2, 3]
arr.push(4)
console.log(arr) // [1, 2, 3, 4]

// unshift方法
let arrA = [1, 2, 3]
arrA.unshift(4)
console.log(arrA) // [4, 1, 2, 3]
```

#### 1.2.1.4. 數組操作 -> 刪
有三種方法：
- pop 往數組最後面添加元素, 返回新數組
- shift 往數組最前面添加元素，返回新數組
- splice 刪除指定位置的元素，返回被刪除的元素 `arr.splice(start, deleteCount)` 。
 `start` : 指定開始位置，<font color="#46A3FF">**本身開始刪除**</font>， `deleteCount` : 指定刪除的數量，沒有值就是 `start` 開始刪除到最後。

```javascript
// pop 方法
let arr = [1, 2, 3]
arr.pop()
console.log(arr) // [1, 2]

// shift方法
let arrA = [1, 2, 3]
arrA.shift()
console.log(arrA) // [2, 3]

// splice方法
let arrB = [1, 2, 3]
arrB.splice(1, 2)
console.log(arrB) // [1]
```
<br>

### 1.2.2. 對象 (Object)
對象是一種無序的數據集合，可以詳細描述某個事物。和數組不同的就是，數組是<font color="#46A3FF">**有序的**</font>。

聲明語法有兩種，以第二種爲主：
```javascript
let objA = new Object()
let obj = {}
```
<br>

對象可以保存屬性 `name` , `age` 和方法 `sayHi` ：
```javascript
let obj = {
  name: 'Ada',
  age: 18,
  sayHi: function() {
    console.log('Hi')
  }
}
```

{% note info %}
屬性和方法都是<font color="#46A3FF">**成對出現**</font>。屬性保存的是值，方法保存的是函數。
{% endnote %}

#### 1.2.2.1. 對象操作 -> 查
有兩種方法獲取屬性的值：

```javascript
let obj = {
  name: 'Ada',
  age: 18,
  sayHi: function() {
    console.log('Hi')
  }
}

// 方法1
console.log(obj.name) // Ada
// 方法2
console.log(obj['name']) // 18
```

{% note info %}
一般是第一種方法1居多，但屬性名有 `-` ，比如 `fake-name` ，就必須用 `obj[屬性名]` 這種寫法。
{% endnote %}

#### 1.2.1.2. 對象操作 -> 改
直接把新值傳達給指定對象的屬性，比如`age`即可

```javascript
let obj = {
  name: 'Ada',
  age: 18,
  sayHi: function() {
    console.log('Hi')
  }
}

obj.age = 20
console.log(obj.age) // 20
```


#### 1.2.1.3. 對象操作 -> 增
直接把值傳達給對象的新屬性，比如 `gender` 即可

```javascript
let obj = {
  name: 'Ada',
  age: 18,
  sayHi: function() {
    console.log('Hi')
  }
}

obj.gender = 'male'
console.log(obj.gender) // male
```

#### 1.2.1.4. 對象操作 -> 刪
非嚴格模式下可以使用 `delete` 刪除屬性和保存的值，但嚴格模式下不行。

```javascript
let obj = {
  name: 'Ada',
  age: 18,
  sayHi: function() {
    console.log('Hi')
  }
}

delete obj.age
console.log(obj) // { name: 'Ada', sayHi: ƒ }
```

#### 1.2.1.5. 遍歷對象
遍歷對象中的每個元素，可以使用 `for(let k in obj)` 的方式， `k` 是元素名， `obj[k]` 是屬性名存的值

```javascript
let obj = {
  name: 'Ada',
  age: 18,
  sayHi: function() {
    console.log('Hi')
  }
}

for (let k in obj) {
  console.log(k, obj[k])
}
// name Ada
// age 18
//  sayHi ƒ () { console.log('Hi') }
```

#### 1.2.1.6. 内置對象
Javascript内部也有提供對象，包含屬性和方法使用：

```javascript
// 屬性
console.log(Math.PI) //3.141592653589793


// 方法
console.log(Math.ceil(1.54)) // 2 (向上取整)
console.log(Math.floor(1.54)) // 1 (向下取整)

// 複數的正數1.5是四捨五入（2），負數是入比較‘大’的數（-1>-2），也就是-1
console.log(Math.round(1.54)) // 2 (四捨五入)
console.log(Math.round(1.55)) // 2 (四捨五入)
console.log(Math.round(-1.54)) // -1 (負值，取比較大的數)
console.log(Math.round(-1.55)) // -2

console.log(Math.max(1，2，3，4，5)) // 5 (最大值)
console.log(Math.min(1，2，3，4，5)) // 1 (最小值)

```


<br>
還有一個複雜數據是函數 (function)，會在下一篇繼續討論，敬請期待。
<br>
<br>