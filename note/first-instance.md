# 创建一个 Vue 的实例

### 每个 Vue 应用都是通过 Vue 函数创建一个新的 Vue 实例开始的：

```
var vm = new Vue({
  // 选项
})
```

> 没有完全遵循 MVVM 模型，`vm` (ViewModel 的简称) 这个变量名表示 Vue 实例。

> 一个 Vue 应用由一个通过 new Vue 创建的根 Vue 实例，以及可选的嵌套的、可复用的组件树组成。

## 数据与方法

当一个 Vue 实例被创建时，它向 Vue 的响应式系统中加入了其 `data` 对象中能找到的所有的属性。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。

```
// 我们的数据对象
var data = { a: 1 }

// 该对象被加入到一个 Vue 实例中
var vm = new Vue({
  data: data
})

// 他们引用相同的对象！
vm.a === data.a // => true

// 设置属性也会影响到原始数据
vm.a = 2
data.a // => 2

// ... 反之亦然
data.a = 3
vm.a // => 3
```

```
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

除了 `data` 属性，Vue 实例暴露了一些有用的实例属性与方法。它们都有前缀 `$`，以便与用户定义的属性区分开来。例如：

```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

## 实例生命周期

> 每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如需要设置数据监听、编译模板、挂载实例到 DOM、在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，给予用户机会在一些特定的场景下添加他们自己的代码。

比如 `created` 钩子可以用来在一个实例被创建之后执行代码：

```
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

也有一些其它的钩子，在实例生命周期的不同场景下调用，如 `mounted`、`updated`、`destroyed`。钩子的 this 指向调用它的 Vue 实例。
*不要在选项属性或回调上使用箭头函数*

## [生命周期图示](https://cn.vuejs.org/v2/guide/instance.html#生命周期图示)

* beforeCreate
* create
* beforeMount
* mounted
* beforeDestroy
* destroyed



Next:[模板语法](./syntax.md)
