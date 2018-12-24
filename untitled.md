# 语法

## 模板语法

### [插值](https://vuejs.bootcss.com/v2/guide/syntax.html#%E6%8F%92%E5%80%BC) <a id="&#x63D2;&#x503C;"></a>

#### [文本](https://vuejs.bootcss.com/v2/guide/syntax.html#%E6%96%87%E6%9C%AC) <a id="&#x6587;&#x672C;"></a>

数据绑定最常见的形式就是使用“Mustache”语法 \(双大括号\) 的文本插值：

```javascript
<span>Message: {{ msg }}</span>
```

Mustache 标签将会被替代为对应数据对象上 `msg` 属性的值。无论何时，绑定的数据对象上 `msg` 属性发生了改变，插值处的内容都会更新。

通过使用 [v-once 指令](https://vuejs.bootcss.com/v2/api/#v-once)，你也能执行一次性地插值，当数据改变时，插值处的内容不会更新。但请留心这会影响到该节点上的其它数据绑定：

```javascript
<span v-once>这个将不会改变: {{ msg }}</span>
```

#### [原始 HTML](https://vuejs.bootcss.com/v2/guide/syntax.html#%E5%8E%9F%E5%A7%8B-HTML) <a id="&#x539F;&#x59CB;-HTML"></a>

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html` 指令：

```javascript
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

![](.gitbook/assets/image%20%281%29.png)

 这个 `span` 的内容将会被替换成为属性值 `rawHtml`，直接作为 HTML——会忽略解析属性值中的数据绑定。注意，你不能使用 `v-html` 来复合局部模板，因为 Vue 不是基于字符串的模板引擎。反之，对于用户界面 \(UI\)，组件更适合作为可重用和可组合的基本单位。

![](.gitbook/assets/image%20%282%29.png)

#### [特性](https://vuejs.bootcss.com/v2/guide/syntax.html#%E7%89%B9%E6%80%A7) <a id="&#x7279;&#x6027;"></a>

Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 [v-bind 指令](https://vuejs.bootcss.com/v2/api/#v-bind)：

```javascript
<div v-bind:id="dynamicId"></div>
```

#### [使用 JavaScript 表达式](https://vuejs.bootcss.com/v2/guide/syntax.html#%E4%BD%BF%E7%94%A8-JavaScript-%E8%A1%A8%E8%BE%BE%E5%BC%8F) <a id="&#x4F7F;&#x7528;-JavaScript-&#x8868;&#x8FBE;&#x5F0F;"></a>

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

### [缩写](https://vuejs.bootcss.com/v2/guide/syntax.html#%E7%BC%A9%E5%86%99) <a id="&#x7F29;&#x5199;"></a>

#### [`v-bind` 缩写](https://vuejs.bootcss.com/v2/guide/syntax.html#v-bind-%E7%BC%A9%E5%86%99) <a id="v-bind-&#x7F29;&#x5199;"></a>

```markup
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```

#### [`v-on` 缩写](https://vuejs.bootcss.com/v2/guide/syntax.html#v-on-%E7%BC%A9%E5%86%99) <a id="v-on-&#x7F29;&#x5199;"></a>

```markup
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```

