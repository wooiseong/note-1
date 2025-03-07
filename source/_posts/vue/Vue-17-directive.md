---
title: Vue-17-自定義Vue指令
excerpt: 本篇會詳細介紹如何封裝自己定義的vue指令。
tags: [Vue， directive]
categories: [Vue]
date: 2025-03-07 13:15:02
---

我們到現在學習了很多vue指令，像是 `v-model` 和 `v-if` 等等。

其實我們也可以自己封裝屬於自己的指令，要用的時候直接在標簽上寫上指令名稱就可以了。

封裝指令有兩種寫法：全局注冊和局部注冊。

## 1. 封裝自定義指令

我們舉個例子：想要封裝一個輸入框可以聚焦的自定義指令

```vue
<template>
  <div>
      <input ref="textInput" type="text">
  </div>
</template>

<script>

export default {
  mounted(){
    this.$refs.textInput.focus()
  }
}
</script>
```


### 1.1 全局注冊

在 `main.js`文件：

```js
Vue.directive('focus', {
  //指令綁定的元素
  inserted(el){
    el.focus()
  }
})
```

### 1.2 局部注冊

在需要自定義指令的組件：
```js
<script>
export default {
  directives: {
    focus: {
      inserted(el){
        el.focus()
      }
    }
  }
}
</script>
```
<br>

在標簽上，我們就可以直接使用 `v-focus` 指令了：
```vue
<template>
  <div>
      <input v-focus type="text">
  </div>
</template>
```
<br><br>

## 2. 封裝自定義指令(傳值)
可是問題來了：如果我們想要根據傳入的值來控制DOM，

然後在值更新後自動更新我們自定義指令的操作呢？
<br>

我們的 `directive` 其實可以傳第二個參數，也就是綁定DOM元素上面的值。

也可以用 `update`函數，當該值被修改就馬上觸發，提供DOM更新的邏輯
<br>

比如我們要對兩個字段指定 `color` 指令，染成不同顏色：

```vue
<template>
  <div>
     <h2 v-color="color1">指令1 紅色</h2>
     <h2 v-color="color2">指令2 綠色</h2>
  </div>
</template>

<script>
export default {
  data(){
    return {
      color1:'red',
      color2:'green'
    }
  },
  directives: {
    color: {
      inserted(el,binding){
        el.style.color = binding.value
      },
      update(el,binding){
        console.log('更新了')
        el.style.color = binding.value
      }
    }
  }
}
</script>
```

![](/img/Vue/Vue-17-1.png) 

<br>
這樣就可以客制化執行自定義指令了！