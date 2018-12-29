# Vue.js22

## 特点：

![](.gitbook/assets/image%20%2813%29.png)

## 介绍：

 一套用于构建用户界面的**渐进式框架**。 核心库**只关注视图层**。 **易于上手**。



### [声明式渲染](https://vuejs.bootcss.com/v2/guide/#%E5%A3%B0%E6%98%8E%E5%BC%8F%E6%B8%B2%E6%9F%93) <a id="&#x58F0;&#x660E;&#x5F0F;&#x6E32;&#x67D3;"></a>

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```javascript
<div id="app">
  {{ message }}
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

```text
Hello Vue!
```

### [组件化应用构建](https://vuejs.bootcss.com/v2/guide/#%E7%BB%84%E4%BB%B6%E5%8C%96%E5%BA%94%E7%94%A8%E6%9E%84%E5%BB%BA) <a id="&#x7EC4;&#x4EF6;&#x5316;&#x5E94;&#x7528;&#x6784;&#x5EFA;"></a>

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

![Component Tree](https://vuejs.bootcss.com/images/components.png)

在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。在 Vue 中注册组件很简单：

```javascript
// 定义名为 todo-item 的新组件
Vue.component('todo-item', {
  template: '<li>这是个待办项</li>'
})
```

 现在你可以用它构建另一个组件模板：

```javascript
<ol>
  <!-- 创建一个 todo-item 组件的实例 -->
  <todo-item></todo-item>
</ol>
```



