---
title: Javascript-35- Javascript的原型（二） 原型 
excerpt: 本篇討論Javascript的原型的概念以及用處。
tags: [Javascript, Contructor, prototype] 
categories: [Javascript]
date: 2025-01-19 20:48:28
---

這篇涉及構造函數，需要上一篇 [Javascript-34- Javascript的原型（一） 編程思想 ](https://wooiseong.vercel.app/2025/01/19/JS-34-prototype1/)的基礎。

## 4. 原型
上篇提到構造函數創建實例對象，構造函數的方法就會開闢内存，造成性能浪費。

但構造函數可以通過原型分配函數給所有對象共享。

那什麽是原型？

Javascript的構造函數有一個 `prototype` 原型的屬性，我們可以把不變的方法定義在原型，

由構造函數創建的實例對象便可以共享這些方法。

我們可以在構造函數直接看到 `prototype` 這個屬性：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
  this.sing = function() {
      console.log('我會唱歌')
  }
}

console.dir(Star.prototype)
```

![](/img/JS/JS-35-1.png)
<br>

我們可以看到 `prototype` 是一個對象，所以被稱爲原型對象。原型對象的 `this` 指向實例對象。

我們從新寫 `sing` 的方法：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

Star.prototype.sing = function() {
  console.log('我會唱歌')
}

const ldh = new Star('劉德華', 18)
const zxy = new Star('張學友', 20)

console.log(ldh.sing === zxy.sing) // true
```
<br>

你看！通過原型對象挂載 `sing` 方法，實例對象 `ldh` 和 `zxy` 的 `sing` 都是同一個方法。

![](/img/JS/JS-35-2.png)

用圖來畫解釋,就是構造函數有個屬性 `prototype` ，實例對象直接找到 `prototype` 上的 `sing` 方法。

這個 `sing` 方法不是在構造函數身上。

我們要如何證明原型對象的 `this` 是指向實例對象？

可以這麽做：

```javascript
let that

function Star(uname, age) {
  this.uname = uname
  this.age = age
}

Star.prototype.sing = function() {
  that = this
}

const ldh = new Person('劉德華', 18)

console.log(that === ldh) // true
```

## 5. constructor屬性
我們知道構造函數有原型對象，原型對象有一個屬性叫constructor屬性。

這個constructor屬性指向構造函數本身。

我們從新寫 `sing` 的方法：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

const ldh = new Star('劉德華', 18)

console.log(Star.prototype.constructor === Star) // true
```
<br>

那這個constructor屬性是怎麽用呢？

我們可以直接給原型對象添加方法：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

Star.prototype.sing = function() {
  console.log('我會唱歌')
}

const ldh = new Star('劉德華', 18)

console.dir(Star.prototype)
```

![](/img/JS/JS-35-3.png)
<br>

如果我們要在原型對象上添加好幾個屬性和方法，會用對象的形式：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

Star.prototype= {
  sing: function(){ console.log('我會唱歌') },
  dance: function(){ console.log('我會跳舞') }
}

const ldh = new Star('劉德華', 18)

console.dir(Star.prototype)
```

![](/img/JS/JS-35-4.png)

<font size="5">我的 `constructor: f Star(uname,age)`呢🤷‍♂️???</font>
<br>

我們挂在原型對象上的方法，會把原型對象的内容修改，導致原型對象的constructor屬性不再指向當前構造函數。

所以，我們需要在原型對象上加上 `constructor: 構造函數` ，

```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

Star.prototype= {
  constructor: Star,
  sing: function(){ console.log('我會唱歌') },
  dance: function(){ console.log('我會跳舞') }
}

const ldh = new Star('劉德華', 18)

console.dir(Star.prototype)
```

![](/img/JS/JS-35-5.png)
<br>

我們證明了原型對象可以挂載公共屬性和方法，可是爲什麽實例對象可以訪問構造函數的原型對象的屬性和方法呢？

答案是因爲實例對象有一個<font color="#46A3FF">對象原型叫 `__proto__`</font>。


## 6. 對象原型
實例對象有一個屬性 `__proto__`， 這個屬性是實例對象的原型，所以被稱爲對象原型，它會指向構造函數的原型對象 `prototype` (紅色箭頭)。

![](/img/JS/JS-35-6.png)
<br>

我們可以打印實例對象看看：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

const ldh = new Star('劉德華', 18)
console.log(ldh)
```

![](/img/JS/JS-35-7.png)

我們可以看到實例對象也有原型 `prototype`（紅色箭頭），

這個就是對象原型 `__proto__` 。
<br>

欸可是明明看到的不是 `[[prototype]]` 嗎？

這是因爲 `__proto__` 不是Javascript標準寫法。
<br>

在上圖，我們可以看到這個對象原型有個 `constructor` 屬性，指向構造函數 `Star` 。

我們試試打印實例對象的 `__proto__` 看看：
```javascript
function Star(uname, age) {
  this.uname = uname
  this.age = age
}

const ldh = new Star('劉德華', 18)
console.log(ldh.__proto__)
console.log(ldh.__proto__.constructor)
console.log(ldh.__proto__ === Star.prototype)
```
![](/img/JS/JS-35-8.png)

我們可以看到對象原型的 `constructor` 也是指向構造函數 `Star` 呢！
<br>

把我們這個篇章所學畫一個關係圖的話：
![](/img/JS/JS-35-9.png)
<br>
相信第一次接觸的朋友應該和我剛學習的時候一樣懵圈，沒關係，可以慢慢消化一下。

我們下一篇再談原型繼承和原型鏈的概念，敬請期待。
<br>
<br>

