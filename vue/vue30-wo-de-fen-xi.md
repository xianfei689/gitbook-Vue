# Vue3.0 我的分析

* 更**快**
* 更**小**
* 更**易于维护**
* 更好的**多端渲染支持**
* 新功能（新的API）

## 更**快**

* **数据监听系统的升级**_**\*\***_
  * 从V2.X的基于ES5的getter/setter\(也就是Object。defineProperty这个API\)，到V3.X的基于proxy（实现）这个新的数据系统
  * 全语言特性支持: 1. 对象属性增加/删除 2. 数组index/length 修改 3. Map,Set,WeakMap,WeakSet
  * 应用初始化侦听大规模的数据的时候，性能也会得到提升。基于proxy监听，就是所谓的**Lazy by default**（当只有数据被用到的时候我们才会监听它），如果你有一个庞大的数据，但是你只用了其中一小部分，那其实我们只会监听其中的一小部分
* **Virtual DOM 完全重写，编译时优化，以减少运行时的开销，初始渲染/更新提速 100% ；**
  * \(Component fast path）编译时判断元素的类型（是否是原生元素or组件），如果是原生元素的话直接生成元素元素对应的Vitrual DOM的代码，如果是组件那么生成组件代码
  * \(Monomorphic calls\)生成虚拟Node的时候，函数调用要竟可能的保持一致，同样个数的参数，利于JS引擎的优化
  * （Children type detection）运行时留下Hint，静态分析子元素的类型，以便跳过不必要的判断

    ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.0.jpg)
* **优化Slots生成**
  * 拆分父组件和子组件的耦合关系，引入生成函数，scope、slot统一为一个函数，由子组件决定是否调用函数，并把传入的内容分配给子组件来完成收集，避免不必要的组件渲染；
  * 真正依赖摸个数据的组件，才会可能重新渲染，就不存在去手动优化组件过度重绘的问题

   ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.1.jpg)
* **静态内容提取**
  * 检查到一部分模板不会变，直接提取，在之后更新中，直接复用之前的Vitrual DOM，甚至比对过程跳过整个树 

    ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.2.jpg)

  * 如果元素的所有属性的都是静态的，提取属性对象，对比元素时，发现data都一样，便可以跳过元素本身比对，只要比对它的children就可以了；

    ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.3.jpg)
* **内联事件函数提取** `<Com @event="count++"/>`
  * 重新渲染生成新的函数，会对内联事件缓存（Cache），方便重用，避免子组件无畏更新的效果

    ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.4.jpg)
* **性能压测**
  * 实测，**内存减半，速度加倍**

     ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.5.jpg)

## 更**小**

* Vue整个代码结构和Tree-shaking配合起来，把没有用过的代码在最后的编译阶段扔掉。Vue2.X的整个代码只有一个Vue对象，所有的东西都在Vue上，那么所有没有用到的东西就没有扔掉。但Vue3.X不是每个应用都需要的功能，就做成了按需引入（用ES module imports按需引入），按需的模块如下：
  * 内置组件（keep-alive，transition...\)
  * 指令的运行时helper（v-model，v-for...\)
  * 各种工具函数（asycComponent,mixins,memoize...\)
* 当只有用到Vue最核心的功能的时候（其他功能模块都被Tree-shaking），任何Vue都会用到的那一部分的代码，其他无关的东西都Tree-shaking掉之后，剩下~10KB 

## 更**易于维护**

* **Flow（facebook）-&gt;TypeScript（Microsoft） 重写**，内部的模块解耦，类型信息可以帮助更好的理解源码 ! ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.6.jpg)
* **编译器重写**
  * 插件化设计
  * 带位置信息的parser（source maps\)
  * 为更好的IDE工具链铺路，如 vetur作者作为vscode团队成员参与编译器的重构

## 更好的**多端渲染支持**

* **场景**
  * Vue native
  * wexx
  * mpvue小程序
* vue作为一个运行时去支持编译到尽可能多的多端，可以Learn once,run anywhere
  * 引入Custom Render API，在 import {createRenderer} from '@vue/runtime-core'，平台无关vue runtime

   ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.7.jpg)

## 新功能（新的API）

* **响应式数据监听API**
  * 轻松实现垮组件的状态共享

    ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.8.jpg)
* 排查组件更新的触发原因（提供新的 renderTriggered API），可以在浏览器debugger究竟哪一行触发组件更新 ![](http://zhouxianfei.gitee.io/imgstore/front/vue/6.9.jpg)
* 更好的typeScript 支持以及原生的Class API 和 TSX
* 更好的警告信息
  * 组件堆栈包含函数式组件
  * 可以在警告信息中查看组件的props
  * 在更新的警告中提供组件堆栈信息
* Experimental Hooks API，逻辑重用机制，可能取代mixins
* Time Slicing Support 切割JavaScript 计算，一帧一帧去处理，预留每16ms yield给浏览器让用户事件重新进来，允许中间用户添加进事件（背景：在浏览器的主线程里，进行大量的javascript操作，会使得整个主线程被block，那么浏览器会处在一个完全没有响应的状态，用户的所有的事件，点击，用户的键盘输入全都无法被响应，如果一个用户的一个输入导致了大量的JavaScript计算，同时用户还在继续输入，就会导致大量的计算被重复推进来，那么性能就直线下降，改进这个问题？）
* IE支持问题，有一部分的API方面的改进是不能用的
  * 会有一个专门的版本在IE11中自动降级为旧的getter/setter机制，并对IE中不支持的语法给出警告

