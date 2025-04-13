---
title: Vue3-1-Vue 2的未來 --- Vue3的登場 (一)
excerpt: 本篇主要介紹Vue 2之後更好的版本 --- Vue 3的一些特性，讓我們更好掌握兩者的不同。
tags: [Vue， Vue3]
categories: [Vue]
date: 2025-04-12 16:33:34
---

我們算是把Vue 2大部分内容都學習完了，在Vue2 之後，研發團隊開始推出更好的，更新，更好用的版本 Vue 3，

## 1. Vue3 簡介

我們來看看Vue 3有哪些新的特性：

![](/img/Vue/Vue3/Vue3-1-1.png) 

除了這些好處外，最特別的是代碼的寫法，和原來的不一樣了，

之前我們學的是選項式寫法(Optional API)； Vue 3的是組合式寫法(Compostion API)

之前我們根據 `data` , `methods` 等等把一個功能的數據和方法進行拆分，

但問題就是維護的時候要上找下找，其實很不方便，

之後的方式把一個功能的數據和方法都放在一個地方，這樣的話，維護起來就會很方便了，

![](/img/Vue/Vue3/Vue3-1-3.png)

我們可以比較一下兩者的差異：

![](/img/Vue/Vue3/Vue3-1-2.png)


## 2. Vue 3安裝
安裝和Vue 2差不多，但之前用的脚手架工具是Vue CLI，

Vue3用的是更快的 Vite

1. 查看node版本，需要16以上
`node -v`

2. 安裝Vue 3
`npm init vue@next`

3. 按照指示安裝和打開vue 3的項目

## 3. Vue 3項目介紹
之前在Vue 2的項目，我們大概就討論了項目的架構，

這裏就不贅述，只是列出一些跟Vue CLI創建的項目不同的地方：

![](/img/Vue/Vue3/Vue3-1-4.png)


這裏特別提到原來在寫代碼的時候，頁面是 `template` > `script` > `style`

在 Vue 變成 `script` > `template` > `style`

![](/img/Vue/Vue3/Vue3-1-5.png)

而且我們在寫代碼的時候，會注意到我們引用組件，

連 `component` 這樣的聲明都不用做了，直接在 `template` 使用即可。

之前Vue 2我們的一個組件只可以一個根元素， 在 Vue 3有多個根元素也沒關係，

比如在上面的示範頁面有 `header` 和 `main` 兩個根標簽也沒問題。

## 4.Vue 3的 `setup`
大家應該發現原來的 `script` 怎麽會加上 `setup` 呢？

其實它是比 `beforeCreate` 更早執行的函數，所以 `this` 也拿不到了。

這個 `setup` 函數，可以這麽寫：

```js
<script>
setup(){
  const message = 'this is a message'

  return {
    message
  }
}
</script>
```

不過必須注意，`setup` 的變量和方法都必須 `return` 不然不能在 `template` 使用。

爲了省卻 `return` 的麻煩， `<script setup>` 這個語法糖可以幫助我們省卻 `return` 的步驟。

## 5. Vue 3的 `ref` 和 `reactive`
如果我們使用 `<script setup>`， 爲了讓數據響應式，

我們需要導入 `ref` 或者 `reactive` ， 

`ref` 是用來處理基本和複雜數據類型， `reactive` 是用來處理複雜數據類型。

我們需要先導入 `reactive` ， 再用一個變量接收 `reactive` 包裹的内容。


```vue
<script setup>
import { reactive } from 'vue'

const state = reactive({
  count: 1
})
</script>

<template>
  <div>{{ state.count }}</div>
</template>
```

`ref` 的用法也是類似：

```vue
<script setup>
import { ref } from 'vue'

const state = ref({
  count: 1
})

</script>
```

原理可以簡單理解爲用 `ref` 把數據包了一層對象。這個對象藉助 `reactive` 實現響應式。

不過要注意的是，在 `script` 訪問 `ref` 包裹的數據是訪問 `value` ， 不是接收 `ref` 包裹的變量。

![](/img/Vue/Vue3/Vue3-1-6.png)

那我們應該用哪一個呢？建議用 `ref` 因爲簡單和複雜數據都可以用，

用 `ref` 編碼風格比較統一。


## 6. Vue 3的 `computed`
在Vue 3的 `computed`， 是直接包裹匿名函數，然後用一個變量接受返回的結果即可：

```vue
<script setup>
import { ref, computed } from 'vue'

const state = ref({
  list: [1,2,3,4,5,6,7]
})

const result = computed(() => {
 return state.value.list.filter(item => item > 5)
})

</script>

<template>
  <div>{{ state.list }}</div>
  <div>{{ result }}</div>
</template>
```

如果是要完整的 `computed` 寫法，可以這麽寫：

```vue
const result = computed({
  get: () => state.value.list.push(11),
  set: (val) => {
    return val
  }
})
```
<br>

不過一般情況都不會直接修改 `computed` 的值，所以這樣寫通常是特殊情況。

我們可以用 `watch` 來監聽和更新修改的值。

這個部分會留到下一篇解説，敬請期待。


