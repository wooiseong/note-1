---
title: Vue-9-進入Vue工程化開發的世界！
excerpt: 本篇介紹如何使用Vue CLI建立一個Vue工程，並介紹一些基本的配置。
tags: [Vue， Vue CLI]
categories: [Vue]
date: 2025-03-02 21:35:59
---

在初學HTML, CSS和JS的時候，面對需要打包的情況，我們都需要手動去設置 `webpack` 的配置，

麻煩不説，缺乏統一標準，還特別容易出錯，尤其是引入路徑的時候😂😂😂

在業界，我們是基於構建工具的環境中開發Vue。什麽意思呢？就是我們不需要手動去設置 `webpack` 的配置，只要輸入指令，就會直接幫我們打包好了。

我們要學習的構建工具，是 `Vue CLI`。

事不宜遲，我們就來開始吧！

## 1. 安裝Vue CLI
1. 全局安裝（一次）： `npm i @vue/cli -g`
2. 查看Vue版本： `vue --version`
3. 創建項目架子： `vue create project-xxx`
<br><br>

我們這個階段會先用Vue 2，所以選擇 `Vue 2`

![](/img/Vue/Vue-9-1.png) 
<br>

根據指示輸入 `cd project-new` 和 `npm run serve`

![](/img/Vue/Vue-9-2.png) 
<br>

4. 啓動項目： `npm run serve`

當我們看到這個頁面，就是成功啦！

![](/img/Vue/Vue-9-4.png) 

## 2. 項目結構介紹
我們點進去文件夾，會生成很多文件夾，
![](/img/Vue/Vue-9-5.png) 


最重要需要瞭解的有幾個：
- `main.js`
這是文件的核心，我們需要導入的 `js` 都會在這裏，然後導入 `App.vue` ， 基於 `App.vue` 創建結構渲染 `index.html`

- `App.vue`
這裏就相當於我們之前寫的項目運行看到的内容，也就是 `template` , `script` 和 `style` 等等。

- `public` > `index.html`
這是模板文件。 `main.js` 實例化 `Vue` ，后，根據我們寫的 `App.vue` 渲染在這裏，也就是我們網頁看到 “門面” 。


### 2.1 App.vue
我們可以仔細去觀察 `App.vue` ：
![](/img/Vue/Vue-9-6.png) 

其實這個結構跟我們學習 `JS，CSS，HTML` 的時候是差不多的，

- `template` 是結構，也就是 HTML寫的標簽。 
<font color="	#46A3FF" >**`<template>` 標簽裏只可以有一個根元素 大 `div` **</font>

- `script` 是我們書寫邏輯/行爲的地方

- `style` 是我們書寫樣式的地方。 我們可以指定要引入的預處理器是哪一類，不過必須要安裝相應的插件，比如 `sass` 就需要安裝 `sass-loader` <br><br>擧個例子： `<style lang="sass">`
