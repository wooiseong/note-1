---
title: Vue-1-Vue簡介
excerpt: 本篇會介紹Vue這個框架的基本概念。
tags: [Vue]
categories: [Vue]
date: 2025-02-06 13:04:08
---

## 1. 前言

Vue是一款輕量型前端框架，基本上我們説的三大前端框架 Vue, React和Angular， Vue就是其中一種，

從前端工程師職缺要求來看，在北部要求React的比較多。在博弈，游戲公司和部分網頁設計公司是使用Vue的。

我會第一個選擇Vue的原因之一也是Vue比較好上手，畢竟是大陸人開發的，中文文檔比較豐富。

官方定義的Vue是<font size="5" color="	#46A3FF">**構建用戶界面**</font>的<font size="5" color="	#46A3FF">**漸進式**</font>前端<font size="5" color="	#46A3FF">**框架**</font>。

我們在上面看到三個關鍵字：
- 構建用戶界面
使用Vue的語法，把數據渲染成用戶看到的頁面，比如對象數組的購物車產品内容展示成一個表格給用戶看。

- 漸進式
Vue的核心是聲明式渲染，我們學習了這個内容就可以完成一些需求。基於更複雜的需求而開發的其他内容，比如組件系統和路由，可以在以後學習，有一個循序漸進的步驟。當我們學習了基礎語法，可以再深入掌握其他更深入的功能。

![](/img/Vue/Vue-1-1.png) 

- 框架
框架就是一套完整的項目開發，我們搭建新項目的配置已經做好了，即開即用，這樣可以很好提升開發項目的效率，這也是爲什麽現在的前端工程師一定要掌握至少一個框架的原因。

<br>
現在已經推出了Vue 3，Vue 2的語法已經慢慢被Vue 3取代了。

但現在還處在過渡階段，所以Vue 2學習還是需要的。接下來的課程會先以Vue 2爲先。



事不宜遲，我們開始我們的第一個Vue項目吧。

## 2. 創建Vue項目
```html
<body>  
  <!-- 步驟1：準備容器 -->
  <div id="app">
    {{ msg }}
  </div>

  <!-- 步驟2：引入需要的包 -->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16
  /dist/vue.js"></script>

  <script>
  // 步驟3：創建Vue實例
  const app = new Vue({
    // 選擇Vue要管理的盒子
    el: '#app',
    // 提供要渲染的數據
    data: {
      msg: 'First Vue Project'
    }
  })
  </script>
</body>
```

![](/img/Vue/Vue-1-2.png) 

我們這樣就創建第一個Vue項目了。