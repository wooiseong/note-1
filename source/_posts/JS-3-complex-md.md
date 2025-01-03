---
title: Javascript-3-數據類型(三) 複雜數據(函數)
excerpt: 上一篇討論了複雜數據類型的數組和對象，本文討論複雜數據第三種類型：函數
tags: [Javascript, 數據類型, 函數] 
categories: [Javascript]
date: 2025-01-01 19:33:51
---

上一篇討論了Javascript簡單數據類型，想瞭解的朋友可以點擊[Javascript-2-數據類型(二) 複雜數據(數組和對象)](https://wooiseong.vercel.app/2024/12/31/JS-2-type-complex/)。

### 1.2.3. 函數 (function)

#### 1.2.3.1. 函數的定義和重要性
函數是被設計爲執行特定任務的代碼塊。

在需要執行相似任務的代碼，比如簡單運算，可以封裝成函數，以函數名()的形式調用( `add()` )：
```javascript
//第一個相加任務
let a = 10
let b = 20
let c = a + b
console.log(c) //30

//第二個相加任務
let d = 30
let e = 40
let f = d + e
console.log(f)  //70


//可以把相加任務封裝成函數
function add(x,y) {
   console.log(x + y)
}

// 調用函數
add(10,20) //30
add(30,40) //70
```
這樣就不用寫重複的代碼，簡化代碼，方便復用。

而且維護或者優化代碼時，只需要修改函數內的代碼即可，提升開發效率，這就是<font color="#46A3FF">**封裝代碼成函數的重要性**</font>。

#### 1.2.3.1. Input > 形參 (parameter) 和實參 (argument)
函數定義時，傳入的數據的變量叫實參。函數被調用時，函數内的變量叫形參。參數超過一個時，使用 `,` 分隔。

用上面的函數作例子：
```javascript
function add(x,y) {
    console.log(x + y)
}

// 調用函數
add(10,20) //30
```
`add` 的 `x` 和 `y` 是形參， `10` 和 `20` 是實參。當 `10` 和 `20` 傳入 `add` 函數時，内部的 `x` 和 `y` 分別會接收 `10` 和 `20` 的值。

如果現在只有獲得 `x` 值，沒有 `y` 值，結果會是：
```javascript
function add(x,y) {
    console.log(x) // 10
    console.log(y) // undefined
    console.log(x + y) // NaN
}

// 調用函數
add(10)
```

這是因爲 `y` 沒有接收任何實參。所以`10 + undefined` 結果會是 `NaN`。

那要怎麽解決這個問題呢？

我們可以先給函數設置<font color="#46A3FF">**參數默認值**</font>。

如果沒有傳入實參，形參的值會以參數默認值作為初始值。

```javascript
function add(x=5 , y=20) {
    console.log(x) // 5
    console.log(y) // 20
    console.log(x + y) // 25
}
add()

//當實參傳入時，形參的值會換成實參的數值：
function add(x=5 , y=20) {
    console.log(x) // 10
    console.log(y) // 40
    console.log(x + y) // 50
}
add(10,40)

```

#### 1.2.3.1. Output > 返回值 (return) 
函數執行后可以有返回值，并用變量接收該返回值

```javascript
function add(x=5 , y=20) {
    return x + y
}
let a = add(10,40) 
console.log(a) // 50
```

這樣做的好處是可以取得函數的返回值，做後續的處理。

比如返回值成爲下一個函數的實參，還是根據數據判斷後續的操作。

{% note danger %}
函數`return`下一行不再執行，所以`return`後面的代碼不要<font color="#EA0000">**換行寫**</font>
{% endnote %}