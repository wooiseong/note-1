---
title: Javascript-25-事件循環
excerpt: 本篇討論Javascript執行人物的機制。
tags: [Javascript, Event Loop] 
categories: [Javascript]
date: 2025-01-14 15:49:51
---

在還沒開始這篇前，我們來一道經典面試題：

```javascript
  console.log(111)

  setTimeout(() => {
    console.log(222)
  },0)

  console.log(333)
```
答案是:
![](/img/JS/JS-25-1.png) 


我們知道Javscript執行函數，應該是從上到下。

爲什麽中間的`setTimeout`會被延遲呢？

## 1.1. Javascript執行機制
Javascript的一大特點是單綫程，<font color="#46A3FF">**白話說就是同一時間只能做一件事**</font>。

Javascript原來目的是處理用戶的交互DOM，比如我們對DOM增刪改查。

我們必須要先添加才可以刪除，不能兩個動作同時進行。

因此，單綫程意味著添加和刪除任務必須排隊進行。

添加任務做完才可以接著做刪除任務。
<br>

可是這樣隨之而來的問題是：萬一前面的任務需要長時間執行，後面不是會一直等待嗎？頁面渲染不連貫。

爲了解決這個問題，需要引入同步和異步處理的概念。
<br>

## 2.1. 同步任務和異步任務
Javascript脚本可以創建多綫程，分成同步和異步。
<br>

同步是執行一個任務后接著下一個任務。程序執行順序與任務的排列是一致的。

比如我們吃飯，吃完再洗碗這樣的順序。
<br>

異步是執行一個耗時的任務，在執行這個任務的同時，可以去執行其他的任務。

比如我們在煮水的時候不會呆等等水煮開，同時進行切換，備料等工作。
<br>
因此，兩者的差別在於：執行流程的順序不同
<br>

什麽是異步任務呢？可以分爲三種類型：
1. 普通事件： `click`, `mouseenter` 等等
2. 資源加載，比如 `load` 事件，還有 ajax請求 （後面會詳談）
3. 定時器：`setTimeout` 和 `setInterval 等等`
<br><br>

我們接下來分析同步和異步任務的執行過程：
![](/img/JS/JS-25-2.png)


1. 同步任務在主綫程執行，形成執行棧。
2. 異步任務會先加到任務隊列中（也被稱爲消息隊列）等待不執行
3. 當同步任務執行完畢，系統會按次讀取任務隊列的異步任務
4. 異步任務進入執行棧，開始執行

以簡單概念理解的話，執行棧是主車道；任務隊列是應急車道
<br>

> 主綫程不斷重複獲得任務，執行，再獲取任務，再執行，這樣的機制被稱爲 “事件循環 (Event Loop)”


<br>可是，如果我們必須要先執行異步任務，再執行同步任務呢？

比如，我們要請求資源，加載完畢才可以跳轉頁面。
<br>

其實有方法可以讓同步任務先等異步任務執行完，才執行自身，以後的AJAX課程會詳談。

本篇的重點圍繞在Javascript執行任務的機制。
<br>
<br>