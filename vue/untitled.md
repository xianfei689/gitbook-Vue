# 语法

## 模板语法

### [插值](https://vuejs.bootcss.com/v2/guide/syntax.html#插值)  <a id="&#x63D2;&#x503C;"></a>

#### [文本](https://vuejs.bootcss.com/v2/guide/syntax.html#文本)  <a id="&#x6587;&#x672C;"></a>

数据绑定最常见的形式就是使用“Mustache”语法 \(双大括号\) 的文本插值：

```javascript
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 `msg` 属性的值。无论何时，绑定的数据对象上 `msg` 属性发生了改变，插值处的内容都会更新。

通过使用 [v-once 指令](https://vuejs.bootcss.com/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```javascript
<span v-once>这个将不会改变: {{ msg }}</span>
```

#### [原始 HTML](https://vuejs.bootcss.com/v2/guide/syntax.html#原始-HTML)  <a id="&#x539F;&#x59CB;-HTML"></a>

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html` 指令：

```javascript
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

![](http://zhouxianfei.gitee.io/imgstore/front/vue/3.0.png)

这个 `span` 的内容将会被替换成为属性值 `rawHtml`，直接作为 HTML——会忽略解析属性值中的数据绑定。注意，你不能使用 `v-html` 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。反之，对于用户界面 \(UI\)，组件更适合作为可重用和可组合的基本单位。

![](http://zhouxianfei.gitee.io/imgstore/front/vue/3.1.png)

#### [特性](https://vuejs.bootcss.com/v2/guide/syntax.html#特性)  <a id="&#x7279;&#x6027;"></a>

Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 [v-bind 指令](https://vuejs.bootcss.com/v2/api/#v-bind)：

```javascript
<div v-bind:id="dynamicId"></div>
```

#### [使用 JavaScript 表达式](https://vuejs.bootcss.com/v2/guide/syntax.html#使用-JavaScript-表达式)  <a id="&#x4F7F;&#x7528;-JavaScript-&#x8868;&#x8FBE;&#x5F0F;"></a>

迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。

```javascript
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。有个限制就是，每个绑定都只能包含**单个表达式**，所以下面的例子都**不会**生效。

```javascript
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

### [缩写](https://vuejs.bootcss.com/v2/guide/syntax.html#缩写)  <a id="&#x7F29;&#x5199;"></a>

#### [`v-bind` 缩写](https://vuejs.bootcss.com/v2/guide/syntax.html#v-bind-缩写)  <a id="v-bind-&#x7F29;&#x5199;"></a>

```markup
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```

#### [`v-on` 缩写](https://vuejs.bootcss.com/v2/guide/syntax.html#v-on-缩写)  <a id="v-on-&#x7F29;&#x5199;"></a>

```markup
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```

## 事件语法

### [事件修饰符](https://vuejs.bootcss.com/v2/guide/events.html#事件修饰符)  <a id="&#x4E8B;&#x4EF6;&#x4FEE;&#x9970;&#x7B26;"></a>

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。

* `.stop`
* `.prevent`
* `.capture`
* `.self`
* `.once`
* `.passive`

```markup
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

![](http://zhouxianfei.gitee.io/imgstore/front/vue/3.2.png)

2.1.4 新增

```markup
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

2.3.0 新增

Vue 还对应 [`addEventListener` 中的 `passive` 选项](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters)提供了 `.passive` 修饰符。

```markup
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

这个 `.passive` 修饰符尤其能够提升移动端的性能。

![](http://zhouxianfei.gitee.io/imgstore/front/vue/3.3.png)

### [按键修饰符](https://vuejs.bootcss.com/v2/guide/events.html#按键修饰符)  <a id="&#x6309;&#x952E;&#x4FEE;&#x9970;&#x7B26;"></a>

在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

```markup
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
```

记住所有的 `keyCode` 比较困难，所以 Vue 为最常用的按键提供了别名：

```markup
<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名：

* `.enter`
* `.tab`
* `.delete` \(捕获“删除”和“退格”键\)
* `.esc`
* `.space`
* `.up`
* `.down`
* `.left`
* `.right`

可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](https://vuejs.bootcss.com/v2/api/#keyCodes)：

```markup
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

## 表单输入绑定

### [基础用法](https://vuejs.bootcss.com/v2/guide/forms.html#基础用法)  <a id="&#x57FA;&#x7840;&#x7528;&#x6CD5;"></a>

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

![](http://zhouxianfei.gitee.io/imgstore/front/vue/3.4.png)

![](http://zhouxianfei.gitee.io/imgstore/front/vue/3.5.png)

### [修饰符](https://vuejs.bootcss.com/v2/guide/forms.html#修饰符)  <a id="&#x4FEE;&#x9970;&#x7B26;"></a>

#### [`.lazy`](https://vuejs.bootcss.com/v2/guide/forms.html#lazy)  <a id="lazy"></a>

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 \(除了[上述](https://vuejs.bootcss.com/v2/guide/forms.html#vmodel-ime-tip)输入法组合文字时\)。你可以添加 `lazy` 修饰符，从而转变为使用 `change`事件进行同步：

```markup
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

#### [`.number`](https://vuejs.bootcss.com/v2/guide/forms.html#number)  <a id="number"></a>

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```markup
<input v-model.number="age" type="number">
```

这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。

#### [`.trim`](https://vuejs.bootcss.com/v2/guide/forms.html#trim)  <a id="trim"></a>

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```markup
<input v-model.trim="msg">
```

