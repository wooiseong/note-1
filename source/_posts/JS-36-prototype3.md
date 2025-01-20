---
title: Javascript-36- Javascript的原型（三） 原型繼承和原型鏈 
excerpt: 本篇討論Javascript的原型繼承的特性和原型鏈的完整概念。
tags: [Javascript, Contructor, prototype, prototype chain] 
categories: [Javascript]
date: 2025-01-20 09:17:46
---

這篇涉及原型對象和對象原型，需要上一篇 [Javascript-34- Javascript的原型（二） 原型 ](https://wooiseong.vercel.app/2025/01/19/JS-35-prototype2/)的基礎。


## 7. 原型繼承
我們知道構造函數可裏面有屬性和方法。

如果我們有一個公共的屬性和方法，想要挂在構造函數 `A`上， 也想要挂在構造函數 `B` 上，我們可以利用原型繼承的方式,

把公共屬性和方法賦值于構造函數的 `prototype` 上：

```javascript
// 公共屬性
const person = { eyes: 2 }

// 構造函數
function Woman(){ }

console.log('原型繼承前的Woman原型',Woman.prototype)
console.log('原型繼承前的Woman原型constructor',Woman.prototype.constructor)


// Woman 原型繼承 person
Woman.prototype = person

// Woman 原型繼承需要指回 Woman
Woman.prototype.constructor = Woman

console.log('原型繼承后的Woman原型',Woman.prototype)
console.log('原型繼承后的Woman原型constructor',Woman.prototype.constructor)
```
![](/img/JS/JS-36-1.png)
<br>

可是這樣問題就來了：如果我另一個構造函數 `Man` 也繼承 `person` ，

我修改 `Man.prototype` 不就也等於修改 `person` ？ 因爲 `Man` 和 `Woman` 的 `prototype` 都是指向同一個 `person` ？

```javascript
// 公共屬性
const person = { eyes: 2 }

// 構造函數 Woman
function Woman(){ }

Woman.prototype = person
Woman.prototype.constructor = Woman

// 構造函數 Man
function Man(){}

Man.prototype = person
// 修改 eyes 數量
Man.prototype.eyes = 1

Man.prototype.constructor = Man

console.log(person)
console.log(Woman.prototype.eyes)
```
![](/img/JS/JS-36-2.png)

因爲 `person.eyes` 被 `Man.prototype.eyes` 覆蓋了，所以 `person.eyes` 的值變成了 `1` 。

連 `Woman.prototype.eyes` 也會收到影響。
<br>

還記得構造函數創建的實例對象每個都不一樣，互不影響嗎？

我們把公共屬性和方法包在一個公共構造函數 `Person` 就可以啦！
<br>

`Woman` 和 `Man` 這些構造函數要用 `Person` 的屬性和方法，

就用 `new Person()` （ `Person` 實例對象）賦值給這些構造函數的 `prototype` 。

```javascript
// 公共屬性
function Person (){ 
  this.eyes = 2 
}

// 構造函數 Woman
function Woman(){ }

Woman.prototype = new Person()
Woman.prototype.constructor = Woman

// 構造函數 Man
function Man(){}

Man.prototype = new Person()
// 修改 eyes 數量
Man.prototype.eyes = 1

Man.prototype.constructor = Man

console.log(Person)
console.log(Woman.prototype.eyes)
```
![](/img/JS/JS-36-3.png)

`Man.prototype.eyes` 修改數量，并不會影響 `Person` 和 `Woman.prototype.eyes` 。

濃縮成一行代碼就是：
<font size="5">`子構造函數.prototype = new 父構造函數 `</font>

## 8. 原型鏈
我們知道構造函數一定會有原型對象 `prototype` , 也會有 `prototype.constructor` 和 `prototype__proto__` 。

實例對象有 `__proto__` 指向構造函數的 `prototype` ，那構造函數的 `prototype__proto__` 會指向什麽呢？

我們來試試看：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

console.log(Star.prototype.__proto__)
console.log(Star.prototype.__proto__ === Object.prototype)
```

`__proto__` 只會指向 `prototype`， 答案是指向 `Object.prototype`

函數是引用數據類型，引用數據類型都會指向 `Object` 這個最大對象。

`Object.prototype` 也有 `Object.prototype__proto__`又會指向誰呢？

因爲 `Object` 已經是最大對象了，沒有可以指的 `prototype` ， 就會指向 `null`

![](/img/JS/JS-36-4.png)

基於原型對象的繼承，使得不同的構造函數的原型對象關聯在一起，形成鏈狀結構的關係，便被稱爲原型鏈。

這個原型鏈也和作用域鏈相似的查找規則，

1. 當訪問對象的屬性和方法，會先查找這個對象有沒有，比如一個數組 `arr` 找有沒有 `reduce` -> 沒有這個方法

2. 用 `ldh.prototype.__proto__` 指向上一層 `Array` 的 `Array.prototype` 找有沒有，有一個 `Array.prototype.reduce` 方法可以用。

3. 如果沒有的話 `Array` 的 `Array.prototype.__proto__` 就再往上一層找，找到最大的對象 `Object` 的 `Object.prototype` ，也沒有，直到 `null` 就是都沒有這個`reduce`方法爲止。

因此可以看到 `__proto__` 的意義就是為對象成員查找機制提供一個路綫。

這就解釋了爲什麽數組内置方法，比如 `reduce` 可以被我們聲明的變量 `arr` 使用。
<br>

此外，我們如果要知道構造函數的 `prototype` 是否出現在某個實例對象的 `prototype`， 可以用 `instanceof` 運算符。

```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

const ldh = new Star('劉德華', 18)

console.log(ldh instanceof Star)  // true
console.log(Star instanceof Object) // true
console.log(Star instanceof Array) // false
```







