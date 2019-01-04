# Mutation

## Mutation 

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 \(type\)** 和 一个 **回调函数 \(handler\)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```text
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 **store.commit** 方法：

```text
store.commit('increment')
```

#### 提交载荷

你可以向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**：

```text
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

```text
store.commit('increment', 10)
```

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：

```text
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

```text
store.commit('increment', {
  amount: 10
})
```

#### [\#](https://vuex.vuejs.org/zh/guide/mutations.html#%E5%AF%B9%E8%B1%A1%E9%A3%8E%E6%A0%BC%E7%9A%84%E6%8F%90%E4%BA%A4%E6%96%B9%E5%BC%8F)对象风格的提交方式 <a id="&#x5BF9;&#x8C61;&#x98CE;&#x683C;&#x7684;&#x63D0;&#x4EA4;&#x65B9;&#x5F0F;"></a>

提交 mutation 的另一种方式是直接使用包含 `type` 属性的对象：

```text
store.commit({
  type: 'increment',
  amount: 10
})
```

当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数，因此 handler 保持不变：

```text
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

#### [\#](https://vuex.vuejs.org/zh/guide/mutations.html#mutation-%E9%9C%80%E9%81%B5%E5%AE%88-vue-%E7%9A%84%E5%93%8D%E5%BA%94%E8%A7%84%E5%88%99)Mutation 需遵守 Vue 的响应规则 <a id="mutation-&#x9700;&#x9075;&#x5B88;-vue-&#x7684;&#x54CD;&#x5E94;&#x89C4;&#x5219;"></a>

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

* 使用 `Vue.set(obj, 'newProp', 123)`, 或者
* 以新对象替换老对象。例如，利用 stage-3 的[对象展开运算符](https://github.com/sebmarkbage/ecmascript-rest-spread)我们可以这样写：

  ```text
  state.obj = { ...state.obj, newProp: 123 }
  ```

#### [\#](https://vuex.vuejs.org/zh/guide/mutations.html#%E4%BD%BF%E7%94%A8%E5%B8%B8%E9%87%8F%E6%9B%BF%E4%BB%A3-mutation-%E4%BA%8B%E4%BB%B6%E7%B1%BB%E5%9E%8B)使用常量替代 Mutation 事件类型 <a id="&#x4F7F;&#x7528;&#x5E38;&#x91CF;&#x66FF;&#x4EE3;-mutation-&#x4E8B;&#x4EF6;&#x7C7B;&#x578B;"></a>

使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然：

```text
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```

```text
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

用不用常量取决于你——在需要多人协作的大型项目中，这会很有帮助。但如果你不喜欢，你完全可以不这样做。

#### [\#](https://vuex.vuejs.org/zh/guide/mutations.html#mutation-%E5%BF%85%E9%A1%BB%E6%98%AF%E5%90%8C%E6%AD%A5%E5%87%BD%E6%95%B0)Mutation 必须是同步函数 <a id="mutation-&#x5FC5;&#x987B;&#x662F;&#x540C;&#x6B65;&#x51FD;&#x6570;"></a>

一条重要的原则就是要记住 **mutation 必须是同步函数**。为什么？请参考下面的例子：

```text
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的。

#### [\#](https://vuex.vuejs.org/zh/guide/mutations.html#%E5%9C%A8%E7%BB%84%E4%BB%B6%E4%B8%AD%E6%8F%90%E4%BA%A4-mutation)在组件中提交 Mutation <a id="&#x5728;&#x7EC4;&#x4EF6;&#x4E2D;&#x63D0;&#x4EA4;-mutation"></a>

你可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation，或者使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用（需要在根节点注入 `store`）。

```text
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```

#### [\#](https://vuex.vuejs.org/zh/guide/mutations.html#%E4%B8%8B%E4%B8%80%E6%AD%A5%EF%BC%9Aaction)下一步：Action <a id="&#x4E0B;&#x4E00;&#x6B65;&#xFF1A;action"></a>

在 mutation 中混合异步调用会导致你的程序很难调试。例如，当你调用了两个包含异步回调的 mutation 来改变状态，你怎么知道什么时候回调和哪个先回调呢？这就是为什么我们要区分这两个概念。在 Vuex 中，**mutation 都是同步事务**：

```text
store.commit('increment')
// 任何由 "increment" 导致的状态变更都应该在此刻完成。
```

为了处理异步操作，让我们来看一看 [Action](https://vuex.vuejs.org/zh/guide/actions.html)。

