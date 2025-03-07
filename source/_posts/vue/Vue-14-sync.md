---
title: Vue-14-簡化父子雙向綁定的魔法： .sync修飾符
excerpt: 本篇會介紹父子通信的雙向綁定如何用 `.sync` 簡化。
tags: [Vue， .sync]
categories: [Vue]
date: 2025-03-07 10:15:35
---

我們有提到過組件通信的原則之一就是單向數據流：子組件要把數據給父組件。

由父組件來收集數據和更改數據。

舉個例子：我們如果要關閉子組件的彈窗，點擊子組件的“關閉”，就回傳 `emit` 給父組件，父組件 `provide` 傳值給子組件關閉。

關閉彈窗是當那個組件 `:visible = isShow` ， `isShow` 的值是 `true` 時。
<br>

父組件
```vue
<BaseDialog :visible="isShow"></BaseDialog>
```
<br>

我們想要直接為 `visible` 這個 props進行 “雙向綁定”， 就是子組件傳 `false` 給父組件，父組件再傳 `false` 給子組件。
<br>

我們可以這樣寫：

父組件
```vue
<BaseDialog :visible="isShow" @update:visible></BaseDialog>
```

子組件
```vue
methods: {
  close(){
    this.$emit('update:visible', false)
  }
}
```
<br>

可是這樣寫還是有點太長，這時 `.sync` 就派上用場了：

父組件
```vue
<BaseDialog :visible.sync="isShow"></BaseDialog>
```


子組件還是一樣
```vue
methods: {
  close(){
    this.$emit('update:visible', false)
  }
}
```
<br>

我們就可以在父組件那裏的 `props` 修飾代碼，簡化代碼了。

有機會大家記得用起來！

<br>