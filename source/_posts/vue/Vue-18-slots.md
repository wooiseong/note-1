---
title: Vue-18-客制化組件的神器————插槽
excerpt: 本篇會詳細介紹插槽如何幫助我們客制化組件。
tags: [Vue， slots]
categories: [Vue]
date: 2025-03-07 13:49:50
---

假如我們有一個對話框，想要復用，但對話框內容可能不同。

你可能第一個想法是在父組件一個一個修改内容，但這樣會很麻煩，而且如果有很多對話框，還要一個一個修改，這樣會很難維護。

有個更好的方法是利用插槽 (slots) 

插槽就是讓組件内部結構有自定義的内容。根據種類可以分爲默認插槽，具名插槽和作用域插槽。

## 1. 插槽
舉例來説，我父組件有兩個對話框，要内容不一樣

父組件
```vue
<template>
  <div>
    <br>
    <popupDialog></popupDialog>
    <br>
    <popupDialog></popupDialog>
  </div>
</template>

<script>
import popupDialog from '@/components/popupDialog'
export default {
  components:{
    popupDialog
  }
}
</script>
```

子組件
```vue
<template>
  <div class="dialog">
    我是大雄
    ，請問你要幹嘛？
  </div>
</template>

<script>
export default {
}
</script>

<style scoped>
.dialog {
  padding: 10px;
  border: 1px solid #000;
}
</style>
```

![](/img/Vue/Vue-18-2.png) 

### 1.1. 默認插槽
我們可以在子組件用 `<slot>` 占想要換的位子，然後在父組件用 `<template>` 來填入内容：

父組件
```vue
<template>
  <div>
    <br>
    <popupDialog>
      <template>我是大雄</template>
    </popupDialog>
    <br>
    <popupDialog>
      <template>我是小夫</template>
    </popupDialog>
  </div>
</template>
```

子組件
```vue
<template>
  <div class="dialog">
    <slot></slot>
    ，請問你要幹嘛？
  </div>
</template>
```

![](/img/Vue/Vue-18-1.png) 
<br>

如果是我們插槽還沒有内容呢？

我們在子組件的 `<slot>` 可以加上後備内容。當沒有替代内容就以這個内容替代：

子組件
```vue
<template>
  <div class="dialog">
    <slot>我是無名氏</slot>
    ，請問你要幹嘛？
  </div>
</template>
```
<br>

### 1.2. 具名插槽
如果内容只有一個還好，如果有多處結構，上面的方式顯然會非常凌亂。

我們要做的呢？就是給這些 `<slot>` 按上名字，變成具名插槽：

父組件
```vue
<template>
  <div>
    <br>
    <popupDialog>
      <template v-slot:name>我是大雄</template>
      <template v-slot:action>我要借書</template>
    </popupDialog>
    <br>
    <popupDialog>
      <template v-slot:name>我是小夫</template>
      <template v-slot:action>還書給你</template>
    </popupDialog>
  </div>
</template>
```

子組件
```vue
<template>
  <div class="dialog">
    <slot name="name">我是無名氏</slot>
    ，
    <slot name="action">請問你要幹嘛？</slot>
  </div>
</template>
```
![](/img/Vue/Vue-18-3.png) 
<br>

### 1.3. 作用域插槽
我們在上面的插槽只是傳文字，其實連數據都可以直接從父組件傳到子組件。

比如，我在父組件加個 `data` 綁定書的名單。子組件的 `props` 直接接收就可以了。


父組件
```vue
<template>
  <div>
    <br>
    <popupDialog :data="list1">
      <template v-slot:name>我是大雄</template>
      <template v-slot:action>我要借書</template>
    </popupDialog>
    <br>
    <popupDialog :data="list2">
      <template v-slot:name>我是小夫</template>
      <template v-slot:action>還書給你</template>
    </popupDialog>
  </div>
</template>

<script>
import popupDialog from '@/components/popupDialog'
export default {
  data(){
    return {
      list1: [
        {id: 1, bookname: '小叮噹'},
        {id: 2, bookname: '金銀島'},
      ],
      list2: [
        {id: 3, bookname: '上尉的女兒'},
        {id: 4, bookname: '福爾摩斯'},
      ],
    }
  },
  components:{
    popupDialog
  }
}
</script>
```
<br>

子組件
```vue
<template>
  <div class="dialog">
    <slot name="name">我是無名氏</slot>
    ，
    <slot name="action">請問你要幹嘛？
    </slot>
    <div v-for="item in data" :key="item.id">
        {{ item.bookname }}
      </div>
  </div>
</template>

<script>
export default {
  props: {
    data: Array
  }
}
</script>
```

![](/img/Vue/Vue-18-4.png) 