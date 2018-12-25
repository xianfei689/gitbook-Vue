---
description: VUE 3.0 展望~
---

# VUE 3.0 展望~

## 总结：

 **1. 更快**   **2. 更小**   **3. 更易于维护**   **4. 更多的原生支持**   **5. 更易于开发使用**

\*\*\*\*

**1. 更快**

* Virtual DOM 完全重写，mounting & patching 提速 100% ；
* 更多编译时（compile-time）提醒以减少 runtime 开销；
* 基于 Proxy 观察者机制以满足全语言覆盖及更好的性能；
* 放弃 Object.defineProperty ，使用更快的原生 Proxy ；
* 组件实例初始化速度提高 100％ ；
* 提速一倍/内存使用降低一半。

**2. 更小**

* Tree-shaking 更友好；
* 新的 core runtime: ~10kb gzipped

**3. 更易于维护**

* Flow -&gt; TypeScript
* Decoupled Packages（解耦包）
* 编译器重写

**4. 更多的原生支持**

* 自定义 Renderer API

**5. 更易于开发使用**

* Exposed reactivity API
* 轻松识别组件重新渲染的原因
* 改进 TypeScript 以支持 w/TSX
* 改进警告追踪
* Experimental Hooks API
* Experimental Time Slicing Support
* 支持 IE11



#### 基于 Proxy 的观察者机制

> 目前，Vue 的反应系统是使用 Object.defineProperty 的 getter 和 setter。 但是，Vue 3 将使用 ES2015 Proxy 作为其观察者机制。 这消除了以前存在的警告，使速度加倍，并节省了一半的内存开销。

#### 静态树提升\(Static Tree Hoisting\)

> 使用静态树提升，这意味着 Vue 3 的编译器将能够检测到什么是静态组件，然后将其提升，从而降低了渲染成本。它将能够跳过未整个树结构打补丁的过程。

![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://images.gitee.com/uploads/images/2018/1207/144346_da2a0744_1422543.png) ![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://images.gitee.com/uploads/images/2018/1207/144405_c66b4577_1422543.png)

#### 静态属性提升（Static Props Hoisting）

> 静态属性提升，其中 Vue 3 将跳过不会改变节点的打补丁过程。

![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://images.gitee.com/uploads/images/2018/1207/144646_3825c690_1422543.png) ![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://images.gitee.com/uploads/images/2018/1207/144700_02d8d053_1422543.png)

#### 重写虚拟DOM \(Virtual DOM Rewrite\)

> 编译时（compile-time）提示来减少 运行时（runtime）开销。重写将包括更有效的代码来创建虚拟节点。

![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://images.gitee.com/uploads/images/2018/1207/144914_171edf13_1422543.png) ![&#x8F93;&#x5165;&#x56FE;&#x7247;&#x8BF4;&#x660E;](https://images.gitee.com/uploads/images/2018/1207/145411_1cc12ba0_1422543.png)

