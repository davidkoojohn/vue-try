# 组件

## 什么是组件？

> 组件 (Component) 是 Vue.js 最强大的功能之一。组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素，Vue.js 的编译器为它添加特殊功能。在有些情况下，组件也可以表现为用 `is` 特性进行了扩展的原生 HTML 元素。

> 所有的 Vue 组件同时也都是 Vue 的实例，所以可接受相同的选项对象 (除了一些根级特有的选项) 并提供相同的生命周期钩子。

## 使用组件

### 全局注册

可以通过以下方式创建一个 Vue 实例：

```
new Vue({
  el: '#some-element',
  // 选项
})
```

要注册一个全局组件，可以使用 Vue.component(tagName, options)。例如：

```
Vue.component('my-component', {
  // 选项
})
```

组件在注册之后，便可以作为自定义元素 <my-component></my-component> 在一个实例的模板中使用。注意确保在初始化根实例之前注册组件：

```
<div id="example">
  <my-component></my-component>
</div>
```

```
// 注册
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})

// 创建根实例
new Vue({
  el: '#example'
})


// 渲染为：
<div id="example">
  <div>A custom component!</div>
</div>
```

### 局部注册

你不必把每个组件都注册到全局。你可以通过某个 Vue 实例/组件的实例选项 components 注册仅在其作用域中可用的组件：

```
var Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 将只在父组件模板中可用
    'my-component': Child
  }
})
```

这种封装也适用于其它可注册的 Vue 功能，比如指令。

### DOM 模板解析注意事项

在自定义组件中使用这些受限制的元素时会导致一些问题，例如：

```
<table>
  <my-row>...</my-row>
</table>
```

自定义组件 `<my-row>` 会被当作无效的内容，因此会导致错误的渲染结果。变通的方案是使用特殊的 `is` 特性：

```
<table>
  <tr is="my-row"></tr>
</table>
```

应当注意，如果使用来自以下来源之一的字符串模板，则没有这些限制：

* `<script type="text/x-template">`
* JavaScript 内联模板字符串
* `.vue` 组件

因此，请尽可能使用字符串模板。

### data 必须是函数

```
Vue.component('simple-counter', {
  template: '<button v-on:click="counter += 1">{{ counter }}</button>',
  // 技术上 data 的确是一个函数了，因此 Vue 不会警告，
  // 但是我们却给每个组件实例返回了同一个对象的引用
  data: function () {
    return {
      counter: 0
    }
  }
})

new Vue({
  el: '#example-2'
})
```

### 组件组合

在 Vue 中，父子组件的关系可以总结为 prop 向下传递，事件向上传递。父组件通过 prop 给子组件下发数据，子组件通过事件给父组件发送消息。

## Prop

### 使用 Prop 传递数据

组件实例的作用域是孤立的。这意味着不能 (也不应该) 在子组件的模板内直接引用父组件的数据。父组件的数据需要通过 prop 才能下发到子组件中。

子组件要显式地用 `props` 选项声明它预期的数据：

```
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 也可以在模板中使用
  // 同样也可以在 vm 实例中通过 this.message 来使用
  template: '<span>{{ message }}</span>'
})

// 
<child message="hello!"></child>
```

### camelCase vs. kebab-case

HTML 特性是不区分大小写的。所以，当使用的不是字符串模板时，camelCase (驼峰式命名) 的 prop 需要转换为相对应的 kebab-case (短横线分隔式命名)：

```
Vue.component('child', {
  // 在 JavaScript 中使用 camelCase
  props: ['myMessage'],
  template: '<span>{{ myMessage }}</span>'
})

<!-- 在 HTML 中使用 kebab-case -->
<child my-message="hello!"></child>
```

*如果你使用字符串模板，则没有这些限制。*

### 动态 Prop

与绑定到任何普通的 HTML 特性相类似，我们可以用 `v-bind` 来动态地将 prop 绑定到父组件的数据。每当父组件的数据变化时，该变化也会传导给子组件：

```
<div>
  <input v-model="parentMsg">
  <br>
  <child v-bind:my-message="parentMsg"></child>
</div>

// 你也可以使用 v-bind 的缩写语法：
<child :my-message="parentMsg"></child>
```

如果你想把一个对象的所有属性作为 prop 进行传递，可以使用不带任何参数的 `v-bind` (即用 `v-bind` 而不是 `v-bind:prop-name)`。例如，已知一个 todo 对象：

```
todo: {
  text: 'Learn Vue',
  isComplete: false
}

// ...
<todo-item v-bind="todo"></todo-item>
```

### 字面量语法 vs 动态语法

```
<!-- 传递了一个字符串 "1" -->
<comp some-prop="1"></comp>
```

如果想传递一个真正的 JavaScript 数值，则需要使用 `v-bind`，从而让它的值被当作 JavaScript 表达式计算：

```
<!-- 传递真正的数值 -->
<comp v-bind:some-prop="1"></comp>
```

### 单向数据流

Prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是反过来不会。这是为了防止子组件无意间修改了父组件的状态，来避免应用的数据流变得难以理解。

另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop。如果你这么做了，Vue 会在控制台给出警告。

在两种情况下，我们很容易忍不住想去修改 prop 中数据：

1. Prop 作为初始值传入后，子组件想把它当作局部数据来用；
2. Prop 作为原始数据传入，由子组件处理成其它数据输出。

对这两种情况，正确的应对方式是：

```
// 定义一个局部变量，并用 prop 的值初始化它：
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}

// 定义一个计算属性，处理 prop 的值并返回：
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

> 注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。

### Prop 验证

我们可以为组件的 prop 指定验证规则。如果传入的数据不符合要求，Vue 会发出警告。这对于开发给他人使用的组件非常有用。

要指定验证规则，需要用对象的形式来定义 prop，而不能用字符串数组：

```
Vue.component('example', {
  props: {
    // 基础类型检测 (`null` 指允许任何类型)
    propA: Number,
    // 可能是多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数值且有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组/对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

`type` 可以是下面原生构造器：

* String
* Number
* Boolean
* Function
* Object
* Array
* Symbol

`type` 也可以是一个自定义构造器函数，使用 `instanceof` 检测。

## [非 Prop 特性](https://cn.vuejs.org/v2/guide/components.html#非-Prop-特性)

所谓非 prop 特性，就是指它可以直接传入组件，而不需要定义相应的 prop。

































