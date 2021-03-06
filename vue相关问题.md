## 1.MVVM框架  
Model代表数据模型，可以在model中定义数据修改和操作的业务逻辑  
view代表UI组件，讲数据模型转化成ui展现出来  
viewmodel可以监听模型数据的改变和控制视图行为、处理用户交互  
在mvvm框架下，view层和model层并没有直接的联系，而是通过viewmodel作为桥梁进行交互。view和model之间的交互是双向的，也就是说view的变化会同步到model中，model的变化也会同步到view中。  
viewmodel通过双向数据绑定把view和model连接起来，他们之间的同步工作是自动的，不需要人为干涉。 
vue通过双向数据绑定将view和model连接起来。  

## 2.vue实现双向数据绑定  
#### 实现双向数据绑定的要点：  
1. 实现一个指令解析器compile，对每个元素节点的指令进行解析和扫描，根据指令模板替换数据，以及绑定相应的更新函数。  
2. 实现一个监听器observer，能够对数据对象的所有属性进行监听，如有变动可拿到最新值并通知订阅者。  
3. 实现一个watcher，作为observer和compile的桥梁，能够订阅并受到每个属性变动的通知，执行指令绑定的相应回调函数（发布），从而更新视图。  
#### vue中的双向数据绑定  
当把一个对象传给vue实例作为它的data选项时，vue将遍历它的属性，用object.defineProperty创建一个observer将他们转为getter、setter。  
vue中每个组件实例都会对应一个watcher实例，它会在组件渲染的过程中把使用过的数据属性通过getter收集为依赖。之后当依赖项的setter触发时，会通知watcher，从而使它关联的组件重新渲染。  
#### Object.defineProperty&Proxy
* proxy的优缺点：  
  1. 优点：可以直接监听对象而非属性并返回一个新对象。可以直接监听数值的变化。可以劫持整个对象并返回一个新对象。  
  2. 缺点：兼容性不好  
* Object.defineProperty的优缺点：  
  1. 优点： 兼容性好  
  2. 缺点：无法监控到数组下标的变化，导致直接通过数组下标给数组设置值不能实时响应。只能劫持对象的属性，需要对每个对象的每个属性进行遍历。  

## 3.组件通信方式  
* [vue中8种组件通信方式, 值得收藏!](https://juejin.cn/post/6844903887162310669#heading-10)

## 4.vue的生命周期  
**beforeCreated** 在数据观测和初始化事件还未开始  
**created** 完成数据观测，属性和方法的运算，初始化事件，$el属性（指向当前组件的dom元素）还没有显示出来   
**beforeMount** 在挂载开始之前被调用，相关的render函数首次被调用。编译模板，把data里的数据和模板生成html，此时还没有挂载html到页面上。  
**mounted** 在挂载实例之后调用。用上面编译好的html内容替换el属性指向dom对象。*（在此进行ajax交互，dom渲染在此中已经完成。）*  
**beforeUpdate** 在数据更新之前调用，发生在虚拟dom重新渲染和打补丁之前。可以在该钩子中进一步的更改状态，不会触发附加的重渲染过程。  
**updated** 在由于数据更改导致的虚拟dom重新渲染和打补丁之后调用。调用时组件dom已经更新。应该避免在此期间更改状态可能会导致更新无限循环。在服务器端渲染期间不被调用。  
**beforeDestroy** 实例销毁之前调用，实例仍然可用。  
**destroyed** 实例销毁之后调用。  

## 5.组件中的data为什么是函数  
因为组件是可复用的，每复用一次组件，data就应该复用一次。每一处复用组件的data改变应该对其他复用组件的数据不影响。对象是引用类型，一处修改会影响其他组件数据。以函数形式可以让每个组件维护独立的数据拷贝，互不影响。  

## 6.computed和watch的区别  
* 计算属性是自动监听依赖值得变化从而动态返回内容，监听是一个过程，在监听值得变化时，可以触发一个回调并做一些操作。  
* 只需要动态值就用计算属性，需要知道值得改变后执行业务逻辑用watch。  
* computed和methods：methods可以接受参数但不会缓存，每次都会执行相应的方法。computed会缓存，只有依赖值变化才会重新求值。

## 7.computed的实现原理   
1. 当组件初始化的时候，computed和data会分别建立各自的响应系统，observer遍历data中每个属性设置get/set数据拦截 
2. 初始化computed会调用initComputed函数  
   1. 注册一个watcher实例，并在内实例化一个dep消息订阅器用作后续收集依赖（比如渲染函数的watcher或者其他观察该       计算属性变化的watcher）   
   2. 调用计算属性时会触发其Object.defineProperty的get访问器函数  
   3. 调用watcher.depend()方法向自身的消息订阅器dep的subs中添加其他属性的watcher  
   4. 调用watcher的evaluate方法（进而调用watcher的get方法）让自身成为其他watcher的消息订阅器的订阅者，首先       将watcher赋给dep.target，然后执行getter求值函数，当访问求值函数里面的属性（来自data，props或者其他         computed）时，会同样触发他们的隔天访问器函数从而将该计算属性的watcher添加到求值函数中属性的watcher的消       息订阅器dep中，当这些操作完成，最后关闭dep.target赋为null并返回求值函数结果。  
3. 当某个属性发生变化，触发set拦截函数，然后调用自身消息订阅器dep的notify方法，遍历当前dep中保存着所有订阅者watcher的subs数组，并逐个调用watcher的update方法，完成响应更新。  

## 8.vue响应式  
* 核心：observer，watcher，dep
  1. observer遍历data中的属性进行数据劫持  
  2. dep：每个属性拥有自己的消息订阅器dep，用于存放所有订阅了该属性的观察者对象  
  3. watcher：观察者，通过dep实现对响应属性的监听，监听到结果后主动触发自己的回调进行响应  
  
## 9.vue模板编译原理  
1. 将模板字符串转换成elements ASTs（解析器）  
   * 截取字符串以及对截取之后的字符串做解析  
   * 每截取一段标签的开头就push到stack中，解析到标签结束就pop
2. 对AST进行静态节点标记，主要用来做虚拟dom的渲染优化（优化器）  
   * 递归AST，将静态节点和静态根节点（子节点全是静态节点）找到并标记  
3. 使用element ASTs生成render函数代码字符串（代码生成器）  

## 10.vue的render时机
* [vue.js 实践总结（二）Render 函数](https://juejin.cn/post/6844903640423989256)  

## 11.bue-router中hash和history的区别  
* [vue-router的两种模式的区别](https://juejin.cn/post/6844903552519766029)   

## 12.vue中的diff算法  
* [【Vue原理】Diff - 白话版](https://zhuanlan.zhihu.com/p/81752104)  

## 13.vue的data什么时候不是响应式的  
* vue会在初始化实例的时候，对属性执行getter/setter转换，如果在实例创建之后添加新的属性到实例上，不会触发视图更新。  
* 可以使用vue.set()/this.$set()
  * 区别：vue.set是将set函数绑定在vue的构造函数上(vue.set=set)，this.$set是将set函数绑定在vue的原型上（vue.prototype.$set=set）  
  * [从vue源码看Vue.set()和this.$set()](https://www.cnblogs.com/heavenYJJ/p/9559439.html)

## 14.vue中的key的作用  
key 的特殊属性主要用在 Vue的虚拟DOM算法，在新旧nodes对比时辨识VNodes。如果不使用key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试修复/再利用相同类型元素的算法。使用key，它会基于key的变化重新排列元素顺序，并且会移除key不存在的元素。  
* [vue 中 key 值有什么作用？](https://cloud.tencent.com/developer/article/1676293)   


  

> 参考文章  
* [通俗易懂了解Vue双向绑定原理及实现](https://my.oschina.net/u/4386652/blog/4281447)  
* [2020年大厂面试指南 - Vue篇](https://juejin.cn/post/6844904069182521351#heading-17)  
* [面试题（2020）Vue面试题汇总](https://segmentfault.com/a/1190000037636720)  
* [浅谈Vue中计算属性computed的实现原理](https://segmentfault.com/a/1190000016368913)  
* [Vue 模板编译原理](https://github.com/berwin/Blog/issues/18)  
