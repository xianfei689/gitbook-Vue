# 生命周期

![](http://zhouxianfei.gitee.io/imgstore/front/vue/2.0.png)

## 1.What is?

**Vue实例从创建到销毁的过程，就是生命周期** 。从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、销毁等一系列过程，称之为Vue的生命周期。

## 2.Why is?

它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑

## 3.几个阶段？

创建前/后, 载入前/后,更新前/后,销毁前/销毁后。

## 4.第一次页面加载触发哪里几个钩子？

beforeCreate, created, beforeMount, mounted 。

## 5.DOM渲染在哪个生命周期中就已经完成？

DOM渲染在mounted中就已经完成了。

## 6.详解

1. beforeCreate（创建前）

> 实例初始化之后，数据观测（data observer） 和watcher/event 事件配置之前被调用。

1. created（创建后）

> 实例已经创建完成。实例已经完成以下配置：数据观测（data observer\),属性和方法运算，watcher/event 事件回调。【还没挂载，$el属性目前不可见】

3.beforeMount（载入前）

> 在挂载开始之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编译模板，把data里面的数据和模板生成html。注意此时还没有挂载html到页面上。

4.mounted（载入后）

> 在el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互。

5.beforeUpdate（更新前）

> 在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。 你可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。

6.updated（更新后）

> 在由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。 当这个钩子被调用的时候，组件已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。

`<keep-alive>` 包裹动态组件时，会缓存不活动的组件实例，而不是销毁他们

7.activated

> 组件激活时调用

8.deactivated

> 组件停用时调用

9.beforeDestroy（销毁前）

> 在实例销毁之前调用。实例仍然完全可用。

10.destroyed（销毁后）

> 在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

## 7.vue生命周期在真实场景下的业务应用

> created：进行ajax请求异步数据的获取、初始化数据
>
> mounted：挂载元素内dom节点的获取
>
> nextTick：针对单一事件更新数据后立即操作dom
>
> updated：任何数据的更新，如果要做统一的业务逻辑处理
>
> watch：监听具体数据变化，并做相应的处理

