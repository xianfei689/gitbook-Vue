# Vue3.0展望 +1



#### vue3.0 `先占坑`

> [https://www.bilibili.com/video/av36787459/](https://www.bilibili.com/video/av36787459/) vue.js 3.0 最新进展，来自 Vue Conf 杭州

**模板编译、Vitrual Dom runtime性能**

* Virtual DOM 实现 `完全重构，100%，减少运行时开销（潜在），编译时来提供性能`
  * 重构原生HTML标签的判断逻辑，编译时，判断标签，在运行时生成响应的标签或组件
  * 生成虚拟Node，保持“形状一致”，同样个数的参数，利于JavaScript引擎优化
  * 运行时留下Hint，静态分析元素包含的子元素类型，以便跳过不必要分支判断
* 优化Slots 生成
  * 拆分父组件和子组件的耦合关系，引入生成函数，scope、slot统一称为一个函数，由子组件决定是否调用函数，并把传入的内容分配给子组件来完成收集，避免不要的组件渲染
  * 真正依赖某个数据的组件，才会可能重新渲染，不存在手动优化组件过度重绘问题
* 静态内容提取
  * 检测到一部分模板不会变，直接提取，在之后更新中，直接复用 vitrual dom，甚至可以在比对过程跳过整个树
  * 包含深度动态内容时，如果元素所有的属性都是静态的，提取属性对象，比对元素时，发现data都一样，便可跳过，去比对children即可
* 内联事件函数提取 `<Com @event="count++">`
  * 重新渲染时会生成新函数，Cache后，以便重用，避免子组件无谓更新的效果

**数据监听系统**

* 把Object.defineProperty 改为 proxy
* 全语言特性支持
  * 新属性增加
  * 属性的删除
  * 数组index
  * 数组length 的修改
  * Map
  * Set
  * WeakMap
  * WeakSet
  * 大规模数据帧听性能提升
* 利用proxy 内部判断，减少组件实例初始化开销
* 实测，内存减半，性能提升100%

**减少runtime 体积，目测测试10kb左右**

* tree-shaking代码结构，按需引入 import
  * 内置组件
    * keep-alive
    * transition
  * 指令运行时
    * v-model
    * v-for
  * 工具函数
    * async component
    * mixins
    * memoize`new 新的工具函数`

**更易维护**

* flow 迁移 typeScript
  * 降低源码阅读能力，引入了类型信息
  * 内部模块解耦，如独立observer
  * 让更多的开发者参与进来
* 编译器重构
  * 插件化设计
  * 带位置信息的parser（source maps）
  * 铺路 IDE工具链，如 vetur 作者作为vscode团队成员参与vue 3.0的开发

**多端渲染支持**

* 场景
  * vue native
  * wexx
  * mpvue 小程序
* vue作为runtime编译到多端
  * 独立出真正的 custome render API，在 import {createRenderer} from '@vue/runtime-core'，平台无关vue runtime
  * vue组件和 vitrual Dom 直接渲染到元素的对象上去

**响应式数据监听API**

* 实现跨组件的状态共享
* 排查组件更新的触发原因
  * 提供新的 renderTriggered API

**更好的typeScript 支持以及原生的Class API 和 TSX**

* 甚至不需要babel

**更好的警告信息**

* 组件堆栈包含函数式组件
* 可以在警告信息中查看组建的props
* 在更新的警告中提供组件堆栈信息

**Experimental Hooks API，逻辑重用机制，可能取代mixins**

* Hooks 替代mixins
* Time Slicing Support 切割JavaScript 计算，一帧一帧去处理，预留每16ms yield给浏览器让用户事件重新进来，允许中间有机会让用户添加进事件

