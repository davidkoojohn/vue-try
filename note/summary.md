## 模板语法总结：

```
// 文本插值: {{ data }}
<span>Message: {{ msg }}</span>

// 插值处不会更新: v-once
<span v-once>这个将不会改变: {{ msg }}</span>

// 插入 HTML: v-html="rawHtml"
<p>Using v-html directive: <span v-html="rawHtml"></span></p>

// 特性绑定: v-bind:id="dynamicId"
<div v-bind:id="dynamicId"></div>

// 使用 JavaScript 表达式
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>

// 指令: v-
<p v-if="seen">现在你看到我了</p>

// 参数: v-bind || 监听 DOM 事件： v-on
<a v-bind:href="url">...</a>
<a v-on:click="doSomething">...</a>

// 修饰符: event.preventDefault()
<form v-on:submit.prevent="onSubmit">...</form>

// 缩写
/**
 * v-bind 缩写
 *
 * <!-- 完整语法 -->
 * <a v-bind:href="url">...</a>
 * <!-- 缩写 -->
 * <a :href="url">...</a>
 */

/**
 * v-on 缩写
 *
 * <!-- 完整语法 -->
 * <a v-on:click="doSomething">...</a>
 * <!-- 缩写 -->
 * <a @click="doSomething">...</a>
 */
 
```
