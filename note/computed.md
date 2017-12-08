## 计算属性和观察者

### 计算属性

> 模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```
当你想要在模板中多次引用此处的翻转字符串时，就会更加难以处理。

> *所以，对于任何复杂逻辑，你都应当使用计算属性。*

### [基础例子](https://cn.vuejs.org/v2/guide/computed.html#基础例子)

> computed: {}

```
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})

// console.log(vm.reversedMessage)
<p>Reversed message: "{{ reversedMessage() }}"</p>
```

### 计算属性缓存 vs 方法

computed: {} // 缓存
 VS
methods: {}  // 无缓存

```
// 在组件中
methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```

> 我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。

> 计算属性只有在它的相关依赖发生改变时才会重新求值。这就意味着只要 `message` 还没有发生改变，多次访问 `reversedMessage` 计算属性会立即返回之前的计算结果，而不必再次执行函数。

这也同样意味着下面的计算属性将不再更新，因为 `Date.now()` 不是响应式依赖：

```
computed: {
  now: function () {
    return Date.now()
  }
}
```

> **相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。**

### [计算属性 vs 侦听属性](https://cn.vuejs.org/v2/guide/computed.html#计算属性-vs-侦听属性)

AngularJS **watch: {}**

### 计算属性的 setter

> 计算属性默认只有 `getter` ，不过在需要时你也可以提供一个 `setter` ：

```
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

### [侦听器](https://cn.vuejs.org/v2/guide/computed.html#侦听器)

> 虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器。

这就是为什么 Vue 通过 watch 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。






























