# Class 与 Style 绑定

## 绑定 HTML Class

### 对象语法

可以传给 `v-bind:class` 一个对象，以动态地切换 class：

```
<div v-bind:class="{ active: isActive }"></div>
```

```
// html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>

// js
data: {
  isActive: true,
  hasError: false
}

// view
<div class="static active"></div>
```

绑定的数据对象不必内联定义在模板里：
```
<div v-bind:class="classObject"></div>

// js
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}

// 渲染的结果和上面一样。我们也可以在这里绑定一个返回对象的计算属性。这是一个常用且强大的模式：
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
### 数组语法

我们可以把一个数组传给 `v-bind:class`，以应用一个 class 列表：

```
<div v-bind:class="[activeClass, errorClass]"></div>
// js
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
// view
<div class="active text-danger"></div>
```

```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>

<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

### 用在组件上

当在一个自定义组件上使用 `class` 属性时，这些类将被添加到该组件的根元素上面。这个元素上已经存在的类不会被覆盖。

例如，如果你声明了这个组件：

```
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

然后在使用它的时候添加一些 class：

```
<my-component class="baz boo"></my-component>
```

HTML 将被渲染为:

```
<p class="foo bar baz boo">Hi</p>
```

对于带数据绑定 class 也同样适用：

```
<my-component v-bind:class="{ active: isActive }"></my-component>

// 当 isActive 为 truthy[1] 时，HTML 将被渲染成为：
<p class="foo bar active">Hi</p>
```

## 绑定内联样式

### 对象语法

> `v-bind:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS 属性名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用单引号括起来) 来命名：

```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

// js
data: {
  activeColor: 'red',
  fontSize: 30
}

// OR

<div v-bind:style="styleObject"></div>

// js
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

### 数组语法

> `v-bind:style` 的数组语法可以将多个样式对象应用到同一个元素上：

### 自动添加前缀

> 当 v-bind:style 使用需要添加浏览器引擎前缀的 CSS 属性时，如 transform，Vue.js 会自动侦测并添加相应的前缀。

### 多重值

> 从 2.3.0 起你可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

```
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```











