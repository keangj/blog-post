---
title: Vue组件通信
date: 2019-03-01 21:50:12
tags:	Vue
cover:  https://miro.medium.com/max/1200/1*qHsE-PM5TLmqzmYjmlr6-A.png
---

# Vue 组件通信

## prop & $emit

使用 `:number="number"` 为组件传递一个数据  
使用 `@add="addNumber"` 监听自定义事件

``` html
  <div id="app">
    {{ msg }}
    <Child :number="number" @add="addNumber"></Child>
  </div>
```

使用 `props` 接收传递的数据  
使用 `$emit` 触发自定义事件

``` js
// 子组件
Vue.component('Child', {
  // 在 button 中监听 click 事件
  template: `
    <div>
      <p>{{ number }}</p>
      <button @click="onClick">
        add
      </button>
    </div>
  `,
    props: {
      // 接收数据 number
      number: {
        type: Number,
        default: 0
      }
    },
    methods: {
      onClick () {
        // 触发自定义事件 add
        // 第一个参数为事件名，第二个参数接受传递的参数
        this.$emit('add', '我是自定义事件')
      }
  }
})

// 父组件
new Vue({
  el: '#app',
  components: {
    Child
  },
  data: {
    number: 0,
    msg: '我是父组件'
  },
  methods: {
    // 自定义事件触发时调用
    addNumber (msg) {
      this.number++
      this.msg = msg
      console.log(msg) // 我是自定义事件
    }
  }
})
```

## [依赖注入](https://cn.vuejs.org/v2/guide/components-edge-cases.html#%E4%BE%9D%E8%B5%96%E6%B3%A8%E5%85%A5)

``` html
<div id="app">
    <Child></Child>
</div>
```

在父组件中通过 `provide` 将数据传递出去  
在子组件通过 `inject` 接收 `provide` 传出来的数据

``` js
// 子组件
Vue.component('Child', {
  template: `
    <p>我是{{ name }}， 我{{ age }}岁了</p>
  `,
  // 接收数据
  inject: ['name', 'age']
})

// 父组件
new Vue({
  el: '#app',
  components: {
    Child
  },
  data: {
    name: 'jay'
  },
  // 传递数据
  provide () {
    return {
      name: this.name,
      age: 18
    }
  }
})
```

## eventBus

``` html
<div id="app">
  <button @click="onClick">toggle</button>
  <Child></Child>
</div>
```

创建一个 `Vue` 新实例 `eventBus`， 在 `eventBus` 上通过自定义事件和监听事件传递数据。

``` js
// 创建 eventBus
let eventBus = new Vue()

// B 组件
Vue.component('Child', {
  template: `
    <p v-show="isShow">我是 {{ msg }}</p>
  `,
  data () {
    return {
      isShow: true,
      msg: 'message'
    }
  },
  mounted () {
    // 监听自定义事件 toggle
    eventBus.$on('toggle', (msg) => {
      console.log(msg) // event bus
      this.isShow = !this.isShow
      this.msg = msg
    })
  }
})

// A 组件
new Vue({
  el: '#app',
  components: {
    Child
  },
  methods: {
    onClick () {
      // 触发自定义事件 toggle 传递数据
      eventBus.$emit('toggle', 'event bus')
    }
  }
})
```

## .sync

在组件上使用 `v-bind:selected="selected"` 为子组件传递数据  
`v-on:update:selected="selected = $event"` 监听事件并更新数据

``` html
<div id="app">
  <h3>parent {{ selected }}</h3>
  <Child
    v-bind:selected="selected"
    v-on:update:selected="selected = $event"
  ></Child>
</div>
```

`.sync` 是上边代码的缩写

``` html
<div id="app">
  <h3>parent {{ selected }}</h3>
  <!-- .sync 缩写 -->
  <!-- .sync 只能绑定属性名，不能使用表达式 -->
  <Child :selected.sync="selected"></Child>
</div>
```

子组件通过 `props` 接收父组件的数据
子组件使用 `this.$emit('update:selected', newValue)` 触发父组件 `update:selected` 事件，更改父组件的数据

``` js
// 子组件
Vue.component('Child', {
  template: `
    <div>
      <button @click="onClickA">A</button>
      <button @click="onClickB">B</button>
      <h3>child {{ selected }}</h3>
    </div>
  `,
  // 接收数据
  props: {
    selected: {
      type: String,
      default: 'A'
    }
  },
  methods: {
    onClickA () {
      // 触发事件，传递参数
      this.$emit('update:selected', 'A')
    },
    onClickB () {
      // 触发事件，传递参数
      this.$emit('update:selected', 'B')
    }
  }
})

// 父组件
new Vue({
  el: '#app',
  components: {
    Child
  },
  data () {
    return {
      selected: 'B'
    }
  }
})
```