---
title: Javascript-37- 深淺拷貝 
excerpt: 本篇討論Javascript深淺拷貝的區別以及作用。
tags: [Javascript, copy] 
categories: [Javascript]
date: 2025-01-20 13:01:02
---


## 1. 前言
我們在[Javascript-9-變量傳説（四）let和const的選擇](https://wooiseong.vercel.app/2025/01/04/JS-9-storage/)有解釋簡單和複雜（引用）數據在内存保存數據的方式。

在我們拷貝對象數據時，常遇到的問題是：新拷貝的對象的增刪改查，會直接影響原數據。

因爲我們複製的是一個在棧的地址，而這個地址指向同一個堆的數據。

```javascript
const obj = {
  uname: 'A',
  age: 18
}

const o = obj
o.age = 20

console.log(obj.age) // 20
```
![](/img/JS/JS-37-1.png)


要解決這個問題，我們需要對深淺拷貝有一定認識。

## 2. 淺拷貝
### 2.1. 拷貝對象
- 我們之前學過的 `Object` 内置構造函數的方法： `Object.assign`
- 展開運算符 `{...obj}`

### 2.2. 拷貝數組
- `Array` 内置構造函數的方法： `Array.prototype.concat()`
- 展開運算符 `{...arr}`
<br>
<br>

淺拷貝一定程度可以解決上面的問題，對於對象第一層的比屬性和方法，修改拷貝了的數據不會被影響，但再往裏一層會受到影響。

```javascript
const obj = {
  uname: 'pink',
  age: 18,
  family: {
    baby: 'Baby'
  }
}

// 淺拷貝
const o = {}
Object.assign(o, obj)

o.age = 20
console.log(o.age) // 20
console.log(obj.age) // 18


o.family.baby = 'Dad'
console.log(o.family) // {baby: NotBaby}
console.log(obj.family) // {baby: NotBaby}
```

我們可以看到第一層的 `o.age` 修改沒問題，但是對象裏的對象 `o.family` 就會被修改。

這是因爲淺拷貝複是把第一層的對象的屬性和方法的值直接拷貝給新變量，

比如 `obj.uname` 的 值 `pink` 直接拷貝給 `o.uname`，

但是再裏一層的 `family` 只是拷貝地址，所以 `obj.family` 和 `o.family` 的地址還是同一個。

![](/img/JS/JS-37-2.png)
<br>

總結來説，淺拷貝只適合單一層的對象和數組拷貝，如果該引用數據屬性和方法還有包裹屬性和方法就需要用深拷貝。
<br>

## 3. 深拷貝
深拷貝直接拷貝對象，不是地址，可以分爲三種方法：

### 3.1. 遞歸函數 (Recursion)
遞歸函數就是在函數内部調用自己，和循環效果類似。

```javascript
let i = 1

function fn(){
  console.log(i)

  if (i >=6){
    return
  }
  i++
  fn()
}

fn()
```

在遞歸函數通常要加 `return` ，不然會很容易發生類似 `for` 的問題無限調用執行，叫做 “棧溢出” (stack overflow) 的錯誤。
<br>

那遞歸函數怎麽實現深拷貝呢？

```javascript
const obj = {
  uname: 'pink',
  age: 18,
  hobby: ['乒乓球', '足球'],
  family: {
    baby: '小pink'
  }
}
const o = {}

function deepCopy(newObj, oldObj) {
  for (let k in oldObj) {
    if (oldObj[k] instanceof Array) {
      newObj[k] = []
      deepCopy(newObj[k], oldObj[k])
    } else if (oldObj[k] instanceof Object) {
      newObj[k] = {}
      deepCopy(newObj[k], oldObj[k])
    }
    else {
      newObj[k] = oldObj[k]
    }
  }
}
```


在判斷被拷貝數據是數組還是對象時，必須先判斷是不是數組，

因爲萬物皆對象, `obj instanceof Object = true` ， 就不會執行數組的拷貝了。
<br>

### 3.2. lodash
上面的方法很長，也不好記，我們可以使用已經寫好的Javascript的庫 lodash。

1. 首先我們登入 [Lodash官方網站](https://lodash.com/) 下載 `lodash.js` 或者用 `npm` 安裝。

2. 引入`Lodash` (`<script src="路徑"></script>`)

3. 用 `_.cloneDeep(被拷貝對象)` 來深拷貝

```javascript
const obj = {
  uname: 'pink',
  age: 18,
  hobby: ['乒乓球', '足球'],
  family: {
    baby: '小pink'
  }
}

const o = _.cloneDeep(obj)
```
<br>

### 3.3. JSON.stringify()
還有一种方法是不需要使用函式庫的，就是 `JSON` 轉換法。

1. `JSON.stringify` 把對象變成簡單數據類型的string

2. 原來在堆的數據變成在棧的JSON string，簡單數據了。

3. `JSON.parse` 再把這個JSON string變成對象。這時，這個對象會產生新的地址，地址指向新的數據。

```javascript
const obj = {
  uname: 'pink',
  age: 18,
  hobby: ['乒乓球', '足球'],
  family: {
    baby: '小pink'
  }
}

const o = JSON.parse(JSON.stringify(obj))
```
<br>