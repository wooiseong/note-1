---
title: Vue-12-組件之間的通信（一）
excerpt: 本篇會詳細介紹 Vue.js 的 props，並展示如何使用 props 來傳遞組件間的數據。
tags: [Vue， props， emit]
categories: [Vue]
date: 2025-03-05 16:06:32
---

我們已經學會了組建的編寫和注冊，那麽我們會要開始學習組件的邏輯了。

## 1. 組建的數據傳遞
我們在寫組件的數據時，會遇到組件A有自己的數據，組件B也有自己的，

甚至還有父組件(父組件裏面包有一個子組件)，那麽這時候我們就需要一定的方式進行數據傳遞了。

在數據傳遞有兩個核心概念：

<font color="	#46A3FF">
1. 組件數據是獨立的，無法直接訪問其他組件的數據<br><br>
2. 單向數據流：數據應該從子組件傳給父組件，由父組件修改，而不是子組件自己修改
</font>

<br>
<br>
根據關係可以分類： 父子關係和非父子關係。

## 1.1. 父子關係
### 1.1.1. props和$emit

`props` 是父組件傳遞數據給子組件； `$emit` 是子組件傳遞給父組件。
<br>

![](/img/Vue/Vue-12-1.png) 
<br>


我們就來開始吧！


父傳子：
1. 父組件 `App.vue` 的 `<template>` , 子組件 `<mySon>`綁定一個變量 `message` ，那個變量要傳的值 `sendMessage` 寫在 `App.vue` 的 `data`

2. `mySon.vue` 的邏輯寫 `[props]` 接收 `message` 的值
<br>
<br>
子收到 `Hi` 了~

![](/img/Vue/Vue-12-2.png) 

父
```vue
<template>
  <div>
    <h3>父組件</h3>
    <div>我收到子組件的信息是: {{ sonMessage }}</div>

    <mySon :message="sendMessage"></mySon>
    <!-- @sendBackMessage="receivedMessage" -->
  </div>
</template>

<script>
import mySon from '@/components/mySon.vue'
export default {
  components: {
    mySon
  } ,
  data (){
    return {
      sendMessage: 'Hi',
      sonMessage: '無消息'
    }
  },
}
</script>
```

子
```vue
<template>
  <div>
    <h3>子組件</h3>
    <div>我是子，收到父組件的值: {{ message }}

    </div><br>

    <div>傳送值給父組件
      <button >傳送</button>
    </div>
  </div>
</template>

<script>

export default {
  props: ['message'],

}
</script>
```

<br>
<br>

子傳父：
1. `mySon` 在 `methods` 定義一個函數 `returnMessage`， 用 `this.$emit(傳事件名， 要傳的值)`

2. 父的 `<mySon>` 綁定傳事件名 
  `@sendBackMessage="receivedMessage"`，`receivedMessage(要傳的值)` 是父組件的函數，接收子傳的值
<br>
<br>

點擊子組件的傳送，父收到 `Hi to my Father` ~
![](/img/Vue/Vue-12-3.png) 

子
```vue
<template>
  <div>
    <h3>子組件</h3>
    <div>我是子，收到父組件的值: {{ message }}

    </div><br>

    <div>傳送值給父組件
      <button @click="returnMessage">傳送</button>

    </div>
  </div>
</template>

<script>
export default {
  props: ['message'],
  methods: {
    returnMessage(){
      this.$emit('sendBackMessage', 'Hi to my Father')
    }
  }
}
</script>
```

父
```vue
<template>
  <div>
    <h3>父組件</h3>
    <div>我收到子組件的信息是: {{ sonMessage }}</div>

    <mySon :message="sendMessage" @sendBackMessage="receivedMessage"></mySon>

  </div>
</template>

<script>
import mySon from '@/components/mySon.vue'
export default {
  components: {
    mySon
  } ,
  data (){
    return {
      sendMessage: 'Hi',
      sonMessage: '無消息'
    }
  },
  methods: {
    receivedMessage(t){
      this.sonMessage = t
    }
  }
}
</script>

```
<br><br>

`props` 有其他可以設定的功能：

- `type` 是設定數據類型
- `default` 是設定預設值
- `required` 是設定是否為必填值
- `validator` 是設定驗證函數，用來驗證數據是否合法

<br>

```js
export default {
  props: {
    message: {
      type: String,
      default: 'Hellooo',
      required: true,
      validator: function (value) {
        if (value.length > 5){
          return true
        } else {
          return false
        }
      }
    },
  }
}
```
<br>

關於我們提到的非父子組件通信就留到下一篇囖，敬請期待！