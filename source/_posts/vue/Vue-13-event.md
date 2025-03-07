---
title: Vue-12-組件之間的通信（二）
excerpt: 本篇會詳細介紹 Vue.js 的非組件傳遞數據的方式，即 event bus和 provide inject。
tags: [Vue， eventBus，provide and inject]
categories: [Vue]
date: 2025-03-06 16:32:38
---

我們在上一篇學習了父子組件的數據傳遞。這一篇會學習非父子組件的數據傳遞。

## 2.1 非父子關係
### 2.1.1 event bus 事件總綫

這個方式是在發送方和接收方創建一個js文件（event bus），當成中間人的角色，用來傳遞數據。

![](/img/Vue/Vue-13-1.png) 
<br>

我們就開始吧！

假如我們有兩個不相關的組件，一個是`A`組件，一個是`B`組件。我們希望`A`組件可以傳遞數據給`B`組件。

![](/img/Vue/Vue-13-2.png) 
<br>

`App.vue`
1. 注冊和使用組件 `BaseA` 和 `BaseB`

```vue
<template>
  <div>
    <BaseA></BaseA>
    <br>
    <BaseB></BaseB>
  </div>
</template>

<script>
import BaseA from '@/components/BaseA.vue'
import BaseB from '@/components/BaseB.vue'
export default {
  components: {
    BaseA,
    BaseB
  } 
}
</script>
```
<br>
<br>

`EventBus.js`
1. EventBus.js是一個js文件，就開一個 `utils` 的工具箱放入這個文件。

```js
import Vue from 'vue'

const Bus = new Vue()

export default Bus
```
<br>
<br>

`BaseA.vue` 發佈方
1. import `EventBus.js`
2. 在 `methods` 寫一個方法：`Bus.$emit('事件名'，數據)`

```vue
<template>
  <div>
    我是組件A 我
    <button @click="clickEvent">點擊</button>
    發送：
  </div>
</template>

<script>
import Bus from '../utils/EventBus'
export default {
  methods: {
    clickEvent(){
      Bus.$emit('sendingMsg', '我發送一則信息給你')
    }
  }
}
</script>
```
<br>
<br>

`BaseB.vue` 接收方
1. import `EventBus.js`
2. 在 `created(){}` 函數接收信息：`Bus.$on('事件名'，(data)=>{ this.data = data})`
3. 在 `data(){}` 函數可以寫一個變量 `data` 來接收數據， `return` 出去。

```vue
<template>
  <div>
    我是組件B 我收到：{{ msg }}
  </div>
</template>

<script>
import Bus from '../utils/EventBus.js'
export default {
  components: {
  
  },
  created(){
    Bus.$on('sendingMsg', (msg) => {
      this.msg = msg
    })
  },
  data (){
    return {
      msg : ''
    }
  }
}
</script>
```
<br>

![](/img/Vue/Vue-13-3.png) 
<br>

是不是覺得和父子傳遞非常像？其實寫法差不多，只是父子用 `props` 和 `emits` ； 非父子用 `$on` 和 `$emit` 。

這個方式多數應用在需要給多個組件傳遞數據的情況。一個發佈，其他只要有寫 `Bus.$on` 就可以收到數據，

就是類似我們訂閲和收到通知的電子郵件服務一樣。

### 2.1.2. provide和inject
上面的方法有一個不方便的地方，就是需要設立一個 `EventBus.js` 的中間人。

如果我們只是想要跨層級共享數據呢？爺組件傳給孫組件就好，那就可以用 `provide` 和 `inject` 。

![](/img/Vue/Vue-13-4.png) 
<br>

我們來試試看：

假如 `App.vue` 有一個子組件，子組件裏面再包一個組件（孫組件），

在 `App.vue` 把數據傳給孫組件:


![](/img/Vue/Vue-13-5.png) 
<br>

`App.vue` 爺組件
1. 用 `provide` 把想要傳遞的數據直接 `return`

```vue
<template>
  <div>
    我是爺組件
    <br><br><br>
    <OwnSon></OwnSon>
  </div>
</template>

<script>
import OwnSon from './components/OwnSon.vue'

export default {
  components: {
    OwnSon
  }, 
  provide(){
    return {
      color: this.color,
      userInfo: this.userInfo
    }
  },
  data(){
    return {
      color: 'red',
      userInfo: {
        name: '陳小華',
        age: 18
      }
    }
  }
}
</script>
```
<br>

`GrandSon.vue` 孫組件
1. 用 `inject` 接收數據，數據即可使用

```vue
<template>
  <div>
    我是孫組件，收到： {{ color }}， {{ userInfo.name }}
  </div>
</template>

<script>

export default {
  inject: ['color', 'userInfo'],
}
</script>
```
<br>

![](/img/Vue/Vue-13-6.png) 
<br>

是不是比較方便？就只是用 `provide` 和 `inject` 直接傳遞就好，
<br>

{% note danger %}
`provide` 和 `inject`有一個缺點：

<font size="5" color="#46A3FF">**傳遞複雜數據是響應式的，簡單數據沒有**</font>

什麽意思呢？就是 `provide` 那方數據更動，只有傳遞過去的複雜數據在 `inject` 那方也會更動。

因此，鼓勵用這種方式傳遞複雜數據就好。
{% endnote %}
<br>
<br>
以上介紹完了非父子和父子通信，大家第一次學習會較混亂,<br><br>

畢竟組件化的概念還沒有很清楚，多練習就會比較熟悉了。
<br><br>