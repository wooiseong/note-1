---
title: Javascript-9-變量傳説（四）let和const的選擇
excerpt: 本篇會仔細闡述Javascript使用let和const的時機。
tags: [Javascript, 變量] 
categories: [Javascript]
date: 2025-01-04 22:00:36
---

## 1.1. 前言
在上一篇 [Javascript-8-變量傳説（三）變量提升](https://wooiseong.vercel.app/2025/01/04/JS-8-Hoisting/), 我們討論了變量提升，解釋了爲什麽需要先聲明變量再訪問。

什麽時機應該用 `let` 和 `const`卻也非常重要。

這解釋特定的運算，比如以下for loop使用的計數變量 `i` 應該是 `let` 而不是 `const` 的原因。

```javascript
for (let i = 0; i < 3; i++) {
  console.log('wawawa')
}
```
<br>

在解答這個問題前，我們需要先知道數據是如何被保存的。


## 1.2. 數據是如何存儲的
我們聲明變量，儲存數據時，數據會被存儲在內存中：棧（stack）和堆（heap）。

棧是操作系統自動分配存放函數的參考值，局部變量的值等。簡單數據類型存放在棧裏面。

堆是存儲複雜數據類型，一般又程序員分配釋放。如果不釋放，由垃圾回收機制處理。

![](/img/JS/JS-9-1.png)  

`let num = 10`
當我們聲明一個簡單數據 `num` ，把 `10` 賦值給 `num`。在棧就會保存 `10`,由 `num` 指向 `10`。
<br>

`let obj = {age: 18}`
當我們聲明一個對象 `obj`， 把 `{age: 18}` ，存進去。在棧會保存一個地址（上面的 “0 x 112233”）， 

`{age: 18}` 則會存在堆中。<font color="#46A3FF">**`obj` 指向的是棧中的地址，由地址再指向堆中保存的值**</font>。

因此，複雜類型類型才被稱爲“引用數據類型”。
<br>

我們不妨再看幾個例子。


### 1.2.1. 簡單數據類型
<font size="4"><u>簡單數據類型</u></font>
![](/img/JS/JS-9-2.png) 

如果是簡單數據類型， `num1` 的值是 `10` ， `num2`的值取自 `num1` 也是 `10`，它們的值存在棧中的兩個空間。

如果 `num2` 的值被改成 `20`， `num1` 的值不會變，因爲它們的值是獨立的。

![](/img/JS/JS-9-3.png) 
<br>

### 1.2.2. 複雜數據類型
![](/img/JS/JS-9-4.png) 

`obj2` 複製 `obj1` 的屬性 `{age:18}`。

當 `obj2.age` 的值改成 `20` ， `obj1.age` 結果還會是 `18` 嗎？

答案是 `20`。

這是因爲 `obj2` 複製的是 `obj1` 在棧的地址(“0x112233”)，所以兩個對象指向堆中的同一個數據 `{age: 18}`。

![](/img/JS/JS-9-5.png) 

當 `obj2`的 `age` 被修改成 `20` ， `obj1` 的 `age` 當然也會變成 `20`， 因爲兩個對象在棧的地址是一樣的，指向堆中的同一個數據。

![](/img/JS/JS-9-6.png) 

我們明白了簡單和複雜數據如何儲存，就可以解決我們對於 `let` 和 `const` 的問題了。

## 1.3. let和const的選擇
`let`用於聲明會常常改變存儲數值的變量; `const` 用於聲明不太會再更改的變量。

在開發中會更推薦使用 `const`， 因爲 `const` 會讓我們避免一些錯誤，比如不小心修改了變量的值，

語義化的角度來説會更好。
<br>

如果還是在糾結到底聲明一個變量要用哪一個的話，
{% note info %}
有變量先用 `const`， 發現它的值後面要修改，再改爲 `let` 。 
{% endnote %}
<br>

### 1.3.1. 簡單數據類型
```javascript
let num = 10
let num1 = 11
console.log(num + num1)
```
請問以上的變量聲明 `let` 可以改成 `const` 嗎？

答案是可以的，因爲值沒有變過。
<br>
<br>

```javascript
for (let i = 0; i < 3; i++) {
  console.log('wawawa') 
}
```
![](/img/JS/JS-9-7.png) 
請問以上的 `i` 可不可以用 `const` 聲明？答案是<font color="#46A3FF">**不能**</font>。

爲什麽呢？

我們來試試看 `const` 聲明 `i` 的結果：
```javascript
for (let i = 0; i < 3; i++) {
  console.log('wawawa') 
}
```
![](/img/JS/JS-9-8.png) 

每一次for循環， `i` 的值都會變，所以會不斷加 `1`， 如果使用`const` 聲明，

第一次循環 `i` 的值是 `0`， 會打印 `'wawawa'`，可是因爲 `i` 是用 `const` 聲明的，所以不能加 `1` ， `i` 的值只能永遠是 `0`，for loop無法循環就會報錯。

只要是可能改變保存簡單數據的變量的值，都是用 `let` 去聲明。
<br>

還有一個例子是累積的操作：
```javascript
let num = 1
num = num + 1
console.log(num) // 2
```
上面的 `num` 值累加 `1` ，所以需要用 `let` 聲明。
<br>

### 1.3.2. 複雜數據類型
```javascript
let arr = ['red', 'pink']
arr.push('blue')
console.log(arr) // ['red', 'pink', 'blue']
```

如果把 `let` 變成 `const` 呢？
```javascript
const arr = ['red', 'pink']
arr.push('blue')
console.log(arr) // ['red', 'pink', 'blue']
```
居然不報錯！

🤣？？？ 到底發生什麽事？

因爲這是<font color="#46A3FF">**複雜數據**</font>。`const` 保存的是棧的地址（“0x112233”） ，不是值。

`push` 新增數組的元素，但那個堆中的數組還是原來的那個數組，棧保存的地址還是那個地址。

![](/img/JS/JS-9-9.png) 

所以不會報錯。

如果是以下例子呢？

```javascript
const arr = ['red', 'pink']
arr = [1,2,3]
console.log(arr)
```
![](/img/JS/JS-9-10.png) 

這是因爲 `[1,2,3]` 賦值給 `arr` ，就等於聲明新數組給 `arr` ， 在棧的地址就是新的地址 （“0x112244”）,地址指向新保存的數組，覆蓋原來的那個。

![](/img/JS/JS-9-11.png)

所以，能不能使用 `const` 聲明變量，保存複雜數據的關鍵在於

<font size="5" color="#46A3FF">**棧的地址是否會被改變**</font>。
<br>
<br>

本文參考至Bili Bili的黑馬程序員
- [簡單和引用數據類型](https://www.bilibili.com/video/BV1Y84y1L7Nn/?spm_id_from=333.788.videopod.episodes&vd_source=84db9197eb55b6426c6715a8061d95ef&p=77)

- [聲明變量const優先](https://www.bilibili.com/video/BV1Y84y1L7Nn/?spm_id_from=333.788.videopod.episodes&vd_source=84db9197eb55b6426c6715a8061d95ef&p=79)

有興趣可以查閲。