---
title: Javascript-32- 談對象（一） 創建對象 
excerpt: 本篇討論創建對象的各種方法。
tags: [Javascript, Constructor] 
categories: [Javascript]
date: 2025-01-17 15:58:30
---

## 1. 前言
我們對對象都有基本認識。在Javascript中，對象是很重要的數據類型。

往後學習串接API時，我們就會接觸更多對象的内容。

這個篇章系列主要會討論對象的創建方法，而其中一個創造方式 ———— 構造函數的知識，對於編程思想有很重要的作用。

我們先從對象創建方法開始了解吧！

## 2. 創建對象
### 2.1. 用對象字面量創建
這種方式是我們最開始學習的方式，用 `{}` 賦值于 `=` 左邊的字面量：
```javascript
const obj = { name: 'A' }
```

### 2.2. 用 new Object創建
我們可以用 `new Object` 的方式創建一個對象，

`new Object` 裏面就寫想要創建的對象屬性和它的值

```javascript
const obj = new Object({ name: 'A' })
```

### 2.3. 用構造函數(Constructor)創建
這是本篇的重點，也是最重要創建對象的方法。

構造函數也是函數，但它可以用來初始對象，而且是創建多個類似的對象。

什麽意思呢？

假設我們有一個學生 `A` ，有幾種屬性，比如年齡和性別
```javascript
const A = {
  name: 'A',
  age: 18,
  gender: 'male'
}
```
<br>

如果我有幾個學生，都要創建對象，包括他們的屬性，

他們的屬性都是一樣的，只是值不一樣，構造函數就可以大顯身手了：

```javascript
function Student(name,age,gender){
  this.name = name
  this.age = age
  this.gender = gender
}

//學生 B
const B = new Student('B', 20, 'male')
console.log(B) // { name: 'B', age: 20, gender: 'male' }

//學生 C
const C = new Student('C', 22, 'female')
console.log(C) // { name: 'C', age: 22, gender: 'female' }
```

這樣，我們只需要輸入對象屬性的值，就會幫我們創建多個類似對象。
<br>

{% note warning %}
構造函數有三個需要注意的點：
1. 命名以大寫字母開頭
2. 只能用 `new` 操作符來執行構造函數
3. 不需要寫 `return` ，寫了也不會有返回值
{% endnote %}
<br>

看到這裏，相信很多人還是不知道到底實例化發生了什麽事，我們就來詳細瞭解。

我們舉一個例子：
```javascript
function Goods(name,price, count){
  this.name = name
  this.price = price
  this.count = count
}

const goodsNew = new Goods('小米',1000,1)
```

![](/img/JS/JS-32-1.png)


1. `new` 會創建新的對象，這個對象内容是空的
2. 構造函數的 `this` 會指向新對象
3. 執行構造函數代碼，修改 `this`， 添加新的屬性（如 `this name = '小米'`）
4. 最後返回新對象


## 3. 對象成員類別（實例成員和靜態成員）
### 3.1. 實例成員
通過構造函數創建的新對象是實例對象，實例對象中的屬性和方法被稱爲實例成員。

```javascript
function Goods(name,price, count){
  this.name = name
  this.price = price
  this.count = count
}

const goodsNew = new Goods('小米',1000,1)
```

`name`, `age` 和 `gender`就是實例對象的實例成員。
<br>

有兩個核心要點需要留意的：
1. 為構造函數傳入參數，創建的實例對象的結構相同（都有 `name`, `age` 和 `gender`），但都是不同的對象。

```javascript
function Goods(name,price, count){
  this.name = name
  this.price = price
  this.count = count
}

const goodsNew = new Goods('小米',1000,1)
const goodsNew2 = new Goods('小米',1000,1)

console.log(goodsNew === goodsNew2) // false
```
<br>

2. 構造函數創建的實例對象彼此互不影響，修改一個實例對象的實例成員，不會影響其他實例對象，儘管都是同個構造函數創建的。

```javascript
function Goods(name,price, count){
  this.name = name
  this.price = price
  this.count = count
}

const goodsNew = new Goods('小米',1000,1)

const goodsNew2 = new Goods('小米',1000,1)
goodsNews2.callMe = () => {
  console.log('call me')
}

console.log(goodsNew)
console.log(goodsNews2)
```

![](/img/JS/JS-32-2.png)

雖然我新增了 `goodsNews2.callMe` 方法，但 `goodsNew` 並不會被添加這個方法，因為他們是兩個不同的對象。
<br>

### 3.2. 靜態成員
構造函數原有的屬性和方法被稱爲靜態成員。

靜態成員和實例成員最大的不同就是<font color="#46A3FF">**靜態成員的是在構造函數身上的，實例成員是在被創建的對象上**</font>。

因此，我們要訪問靜態成員，需要通過構造函數。
```javascript
function Goods(name,price, count){
  this.name = name
  this.price = price
  this.count = count
}

Goods.loveMe = function(){
  console.log('love me')
  console.log(this)
}

Goods.loveMe()
```

![](/img/JS/JS-32-3.png)


我們在 `Goods` 這個構造函數添加靜態方法 `loveMe` ，就需要用 `Goods.loveMe` 的方式。

因爲調用這個方法的是構造函數，方法的 `this` 就指向 `Goods`，

不像實例方法會指向實例對象。
<br>

我們其實已經使用很多靜態方法了，就比如 `Date.now()` 和 `Math.random()` ，

`Date` 和 `Math` 是構造函數，而 `now()` 和 `random()` 分別是它們原有的靜態方法。
<br>

依照分類，Javascript有内置的構造函數，`Object` , `Array` , `String` 和 `Number` ，以及它們身上的靜態屬性和方法。

在下一篇會再詳細解説，敬請期待。
<br>
