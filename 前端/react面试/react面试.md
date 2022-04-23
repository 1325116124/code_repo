# React详解

## JSX如何变成dom

JSX: 是javascript的一种语法扩展，它和模板语言很接近，但是它充分具备JavaScript的能力

jsx代码 -> babel进行转换 -> React.createElement调用 -> ReactElement调用 -> 产生虚拟dom -> 传入ReactDOM.render() -> 渲染处理产生真实dom

```js
export function createElement(type,config,children)
type: 用于标识节点的类型。
config: 以对象形式传入，组件所有的属性都会以键值对的形式存储在 config 对象中.
children: 以对象形式传入，它记录的是组件标签之间嵌套的内容，也就是所谓的“子节点”“子元素”。
```

![image-20220226174223246](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220226174223246.png)



## 生命周期详解

组件在初始化时，会通过调用生命周期中的 render 方法，**生成虚拟** **DOM**，然后再通过调用ReactDOM.render 方法，实现虚拟 DOM 到真实 DOM 的转换。

当组件更新时，会再次通过调用 render 方法**生成新的虚拟** **DOM**，然后借助 diff定位出两次虚拟**DOM** **的差异**，从而针对发生变化的真实DOM 作定向更新。

**React15的生命周期：**

![image-20220226175757462](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220226175757462.png)

**componentReceiveProps** **并不是由** **props** **的变化触发的，而是由父组件的更新触发的**，其实是因为父组件的数据发生了更新所以重新渲染，因此子组件就会被重新渲染

React 组件会根据 shouldComponentUpdate 的返回值，来决定是否执行该方法之后的生命周期，进而决定是否对组件进行 **re-render**（重渲染）。shouldComponentUpdate 的默认值为 true，也就是说 “无条件 re-render”。在实际的开发中，我们往往通过手动往 shouldComponentUpdate 中填充判定逻辑，或者直接在项目中引入 PureComponent 等最佳实践，来实现 “有条件的 re-render”。                                                                                               



**React16生命周期：**

![image-20220226180526883](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220226180526883.png)

废弃了componentWillMount，新增了getDerivedStateFromProps

**getDerivedStateFromProps** **是一个静态方法**。静态方法不依赖组件实例而存在，因此你在这个方法内部是**访问不到** **this** 的

该方法可以接收两个参数：props 和 state，它们分别代表当前组件接收到的来自父组件的 props 和当前组件自身的 state。

getDerivedStateFromProps 需要一个对象格式的返回值。

**getDerivedStateFromProps** **方法对** **state的更新动作并非覆盖式的更新，而是针对某个属性的定向更新**。



在 React 16.4 中，**任何因素触发的组件更新流程**（包括由 this.setState 和 forceUpdate 触发的更新流程）都会触发 getDerivedStateFromProps；

而在 v 16.3 版本时，**只有父组件的更新**会触发该生命周期。

**为什么要用** **getDerivedStateFromProps代替componentWillReceiveProps?**

1. 为了限制在这个生命周期的其他多余的操作，方便跟踪和管理数据（因为拿不到this)，也就是仅仅用来完成props到state的映射



如图所示，**同步渲染的递归调用栈是非常深的**，只有最底层的调用返回了，整个渲染过程才会开始逐层返回。**这个漫长且不可打断的更新过程，将会带来用户体验层面的巨大风险：**同步渲染一旦开始，便会牢牢抓住主线程不放，直到递归彻底完成。在这个过程中，浏览器没有办法处理任何渲染之外的事情，会进入一种**无法处理用户交互**的状态。因此若渲染时间稍微长一点，页面就会面临卡顿甚至卡死的风险。

而 React 16 引入的 Fiber 架构，恰好能够解决掉这个风险：**Fiber** **会将一个大的更新任务拆解为许多个小任务**。每当执行完一个小任务时，**渲染线程都会把主线程交回去**，看看有没有优先级更高的工作要处理，确保不会出现其他任务被 “饿死” 的情况，进而避免同步渲染带来的卡顿。在这个过程中，**渲染线程不再一去不回头，而是可以被打断的**，这就是所谓的 “异步渲染”，

Fiber 架构的重要特征就是**可以被打断的**异步渲染模式。但这个 “打断” 是有原则的，根据“**能否被打断**”这一标准，React 16 的生命周期被划分为了 render 和 commit 两个阶段，而 commit 阶段又被细分为了 pre-commit 和 commit。

![image-20220226193227448](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220226193227448.png)

**总的来说，renders **阶段在执行过程中允许被打断，而 **commit** **阶段则总是同步执行的。**

在 Fiber 机制下，**render** **阶段是允许暂停、终止和重启的**。当一个任务执行到一半被打断后，下一次渲染线程抢回主动权时，这个任务被重启的形式是 “重复执行一遍整个任务” 而非“接着上次执行到的那行代码往下走”。**这就导致** **render** **阶段的生命周期都是有可能被重复执行的**。

为什么要废弃componentWill***:

**1.**完全可以转移到其他生命周期（尤其是**componentDidxxx**）里去做。

**2.**在**Fiber** **带来的异步渲染机制下，可能会导致非常严重的** **Bug**。

总的来说，**React 16** **改造生命周期的主要动机是为了配合** **Fiber** **架构带来的异步渲染机制**。在这个改造的过程中，React 团队精益求精，**针对生命周期中长期被滥用的部分推行了具有强制性的最佳实践**。这一系列的工作做下来，首先是**确保了** **Fiber** **机制下数据和视图的安全性**，同时也**确保了生命周期方法的行为更加纯粹、可控、可预测**。



## 组件通信

**基于** **props** **的单向数据流**

所谓单向数据流，指的就是当前组件的 state 以 props 的形式流动时，**只能流向组件树中比自己层级更低的组件。**

父子通信，子父通信，兄弟通信，发布订阅模式

context API:

旧的context API存在的问题：

1. 首先映入眼帘的第一个问题是**代码不够优雅**：一眼望去，你很难迅速辨别出谁是 Provider、谁是Consumer。

2. 如果组件提供的一个 Context 发生了变化，而中间父组件的 shouldComponentUpdate 返回false，**那么使用到该值的后代组件不会进行更新**。使用了 Context 的组件则完全失控，所以基本上没有办法能够可靠的更新 Context。

新的 Context API 改进了这一点：**即便组件的** **shouldComponentUpdate** **返回** **false**，它仍然可以穿透 **组件继续向后代组件进行传播**，**进而确保了数据生产者和数据消费者之间数据的一致性**。再加上更加 “好看” 的语义化的声明式写法，新版 Context API 终于顺利地摘掉了 “试验性 API” 的帽子，成了一种确实可行的 React 组件间通信解决方案。

redux:

Redux 主要由三部分组成：store、reducer 和 action。我们先来看看它们各自代表什么：

store 就好比组件群里的 “群文件”，它是一个**单一的数据源**，而且是只读的；

action 人如其名，是 “动作” 的意思，它是**对变化的描述**。

reducer 是一个函数，它负责**对变化进行分发和处理，** 最终将新的数据返回给 store。

**从编码的角度理解** **Redux** **工作流**

**1.** **使用** **createStore** **来完成** **store** **对象的创建**

**2. reducer** **的作用是将新的** **state** **返回给** **store**

**3. action** **的作用是通知reducer “让改变发生”**

**4.派发** **action**，靠的是 **dispatch**



## React Hooks

**函数组件会捕获** **render** **内部的状态，这是两类组件最大的不同。**

不夸张地说，**React** **组件本身的定位就是函数，一个吃进数据、吐出** **UI** **的函数**。作为开发者，我们编写的是声明式的代码，而 React 框架的主要工作，就是及时地**把声明式的代码转换为命令式的** **DOM** **操作，把数据层面的描述映射到用户可见的** **UI** **变化中去**。这就意味着从原则上来讲，**React** **的数据应该总是紧紧地和渲染绑定在一起的**，**而类组件做不到这一点**。

props 会在 ProfifilePage 函数执行的一瞬间就被捕获，而 props 本身又是一个不可变值，因此**我们可以充分确保从现在开始，在任何时机下读取到的props，都是最初捕获到的那个props**。



**Hooks** **是如何帮助我们升级工作模式的:**

告别难以理解的 Class： **this** **和生命周期这两个痛点**。

解决业务逻辑难以拆分的问题：而在 Hooks 的帮助下，我们完全可以把这些繁杂的操作**按照逻辑上的关联拆分进不同的函数组件里：**我们可以有专门管理订阅的函数组件、专门处理 DOM 的函数组件、专门获取数据的函数组件等。Hooks 能够帮助我们**实现业务逻辑的聚合，避免复杂的组件和冗余的代码**。

使状态逻辑复用变得简单可行：Hooks 可以视作是 React 为解决状态逻辑复用这个问题所提供的一个原生途径。现在我们可以通过自定义Hook，达到既不破坏组件结构、又能够实现逻辑复用的效果。

函数组件从设计思想上来看，更加契合 React 的理念。



**从源码调用流程看原理：Hooks的正常运作，在底层依赖于顺序链表**

首先要说明的是，React-Hooks 的调用链路在首次渲染和更新阶段是不同的，这里我将两个阶段的链路各总结进了两张大图里，我们依次来看。首先是首次渲染的过程，请看下图

![image-20220226211723820](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220226211723820.png)

![image-20220226211742046](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220226211742046.png)

**hook** **相关的所有信息收敛在一个** **hook** **对象里，而** **hook** **对象之间以单向链表的形式相互串联**。

**而 updateState 之后的操作链路，虽然涉及的代码有很多，但其实做的事情很容易理解：按顺序去遍历之前构建好的链表，取出对应的数据信息进行渲染**。

**hooks** **的渲染是通过依次遍历来定位每个hooks内容的。如果前后两次读到的链表在顺序上出现差异，那么渲染的结果自然是不可控的**。



## 真正理解虚拟Dom

什么是虚拟dom：虚拟 DOM（Virtual DOM）本质上是 **JS** **和** **DOM** **之间的一个映射缓存**，它在形态上表现为一个能够描述DOM结构及其属性信息的 **JS** **对象**。

**React选用虚拟DOM，真的是为了更好的性能吗？**

​	**1.虚拟** **DOM** **的优越之处在于，它能够在提供更爽、更高效的研发模式（也就是函数式的UI编程方式）的同时，仍然保持一个还不错的性能**。

(因为之前重新渲染都是删除整个真实dom，这是很消耗性能的，而虚拟dom可以通过diff算法进行差量修改，再操作真实dom)

![image-20220226214347903](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220226214347903.png)

2. **跨平台的问题：虚拟 DOM 是对真实渲染内容的一层抽象。若没有这一层抽象，那么视图层将和渲染平台紧密耦合在一起，为了描述同样的视图内容，你可能要分别在 Web 端和 Native 端写完全不同的两套甚至多套代码。**

**除了差量更新以外，“批量更新” 也是虚拟 DOM 在性能方面所做的一个重要努力：批量更新在通用虚拟 DOM 库里是由batch函数来处理的。在差量更新速度非常快的情况下（比如极短的时间里多次操作同一个 DOM），用户实际上只能看到最后一次更新的效果。这种场景下，前面几次的更新动作虽然意义不大，但都会触发重渲染流程，带来大量不必要的高耗能操作。这时就需要请 batch 来帮忙了，batch的作用是缓冲每次生成的补丁集，它会把收集到的多个补丁集暂存到队列中，再将最终的结果交给渲染函数，最终实现集中化的 DOM 批量更新。**



## React中的栈调和

**调和（Reconciliation）过程与 Diff 算法**

**通过如 ReactDOM 等类库使虚拟 DOM 与 “真实的” DOM 同步，这一过程叫作协调（调和）。**

**根据 Diff 实现形式的不同，调和过程被划分为了以 React 15 为代表的 “栈调和” 以及 React 16 以来的 “Fiber 调和”。**

**diff算法的逻辑：**

1. **Diff 算法性能突破的关键点在于 “分层对比”；** 

2. **类型一致的节点才有继续 Diff 的必要性；**

3. **key 属性的设置，可以帮我们尽可能重用同一层级内的节点。**

**对于 React 15 下的 Diff 过程，我个人的建议是你了解到逻辑这一层，把握住 “树递归” 这个特征，这就够了。**



## setState到底是同步的还是异步的？

**这正是 setState 异步的一个重要的动机——避免频繁的 re-render。**

**每来一个 setState，就把它塞进一个队列里 “攒起来”。等时机成熟，再把“攒起来” 的state 结果做合并，最后只针对最新的 state 值走一次更新流程。这个过程，叫作“批量更新”**

**并不是 setTimeout 改变了 setState，而是 setTimeout 帮助 setState “逃 脱” 了 React 对它的管控。只要是在 React 管控下的 setState，一定是异步的。**

batchingStrategy 对象并不复杂，你可以理解为它是一个 “锁管理器”。这里的 “锁”，是指 React 全局唯一的 isBatchingUpdates 变量，isBatchingUpdates 的初始值是false，意味着“当前并未进行任何批量更新操作”。每当 React 调用 batchedUpdate 去执行更新动作时，会先把这个锁给“锁上”（置为 true），表明“现在正处于批量更新过程中”。当锁被“锁上” 的时候，任何需要更新的组件都只能暂时进入 dirtyComponents 里排队等候下一次的批量更新，而不能随意 “插 队”。此处体现的“任务锁” 的思想，是 React 面对大量状态仍然能够实现有序分批处理的基石。

Transaction 在 React 源码中表现为一个核心类，React 官方曾经这样描述它：Transaction 是创建一个黑盒，该黑盒能够封装任何的方法。因此，那些需要在函数运行前、后运行的方法可以通过此方法封装（即使函数运行中有异常抛出，这些固定的方法仍可运行），实例化 Transaction 时只需提供相关的方法即可。

说白了，Transaction 就像是一个 “壳子”，它首先会将目标函数用 wrapper（一组 initialize 及 close 方法称为一个 wrapper） 封装起来，同时需要使用 Transaction 类暴露的 perform 方法去执行它。如上面的注释所示，在 anyMethod 执行之前，perform 会先执行所有 wrapper 的 initialize 方法，执行完后，再执行所有 wrapper 的 close 方法。这就是 React 中的事务机制。

下面结合对事务机制的理解，我们继续来看在 ReactDefaultBatchingStrategy 这个对象。ReactDefaultBatchingStrategy 其实就是一个批量更新策略事务，它的 wrapper 有两个：FLUSH_BATCHED_UPDATES 和 RESET_BATCHED_UPDATES。

![image-20220227000241568](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220227000241568.png)

**话说到这里，一切都变得明朗了起来：isBatchingUpdates 这个变量，在 React 的生命周期函数以及合成事件执行前，已经被 React 悄悄修改为了 true，这时我们所做的 setState 操作自然不会立即生效。当函数执行完毕后，事务的 close 方法会再把 isBatchingUpdates 改为 false。**

**咱们开头锁上的那个 isBatchingUpdates，对 setTimeout 内部的执行逻辑完全没有约束力。因为 isBatchingUpdates 是在同步代码中变化的，而 setTimeout 的逻辑是异步执行的。当this.setState 调用真正发生的时候，isBatchingUpdates 早已经被重置为了 false，这就使得当前场景下的 setState 具备了立刻发起同步更新的能力。所以咱们前面说的没错——setState 并不是具备同步这种特性，只是在特定的情境下，它会从 React 的异步管控中 “逃脱” 掉。**

**setState 并不是单纯同步 / 异步的，它的表现会因调用场景的不同而不同：在 React 钩子函数及合成事件中，它表现为异步；而在 setTimeout、setInterval 等函数中，包括在 DOM 原生事件中，它都表现为同步。这种差异，本质上是由 React 事务机制和批量更新机制的工作方式来决定的。**



## Fiber架构

Stack Reconciler 所带来的一个无解的问题，正是 JavaScript 对主线程的超时占用问题。

Stack Reconciler 是一个同步的递归过程。

这个过程的致命性在于它是同步的，不可以被打断。当处理结构相对复杂、体量相对庞大的虚拟 DOM树时，Stack Reconciler 需要的调和时间会很长，这就意味着 JavaScript 线程将长时间地霸占主线程，进而导致我们上文中所描述的渲染卡顿 / 卡死、交互长时间无响应等问题。

Fiber 架构的应用目的，按照 React 官方的说法，是实现 “增量渲染”。所谓 “增量渲染”，通俗来说就是把一个渲染任务分解为多个渲染任务，而后将其分散到多个帧里面。不过严格来说，增量渲染其实也只是一种手段，实现增量渲染的目的，是为了实现任务的可中断、可恢复，并给不同的任务赋予不同的优先级，最终达成更加顺滑的用户体验。

**Fiber 架构核心：“可中断”“可恢复” 与 “优先级”**

**在这套架构模式下，更新的处理工作流变成了这样：首先，每个更新任务都会被赋予一个优先级。当更新任务抵达调度器时，高优先级的更新任务（记为 A）会更快地被调度进 Reconciler 层；此时若有新的更新任务（记为 B）抵达调度器，调度器会检查它的优先级，若发现 B 的优先级高于当前任务 A，那么当前处于 Reconciler 层的 A 任务就会被中断，调度器会将 B 任务推入 Reconciler 层。当 B 任务完成渲染后，新一轮的调度开始，之前被中断的 A 任务将会被重新推入 Reconciler 层，继续它的渲染之旅，这便是所谓 “可恢复”。**

![image-20220227003220303](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220227003220303.png)

![image-20220227003231404](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220227003231404.png)

**在 render 阶段，一个庞大的更新任务被分解为了一个个的工作单元，这些工作单元有着不同的优先级，React 可以根据优先级的高低去实现工作单元的打断和恢复。**





## Redux

事实上，在许多服务端的 MVC 应用中，数据流确实能够保持单向。但是在前端场景下，实际的 MVC应用要复杂不少，前端应用 / 框架往往出于交互的需要，允许 View 和 Model 直接通信。这就允许了双向数据流的存在。当业务复杂度较高时，数据流会变得非常混乱

Flux 最核心的地方在于严格的单向数据流，在单向数据流下，状态的变化是可预测的。如果 store 中的数据发生了变化，那么有且仅有一个原因，那就是由 Dispatcher 派发 Action 来触发的。

Store：它是一个单一的数据源，而且是只读的。
Action 人如其名，是 “动作” 的意思，它是对变化的描述。
Reducer 是一个函数，它负责对变化进行分发和处理，最终将新的数据返回给 Store。

![image-20220227012805035](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220227012805035.png)



**dispatch**

![image-20220227013630968](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220227013630968.png)

**subscribe**

![image-20220227013822020](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220227013822020.png)

**注册监听也是操作 nextListeners，触发订阅也是读取 nextListeners（实际上，细心的同学会注意到，取消监听操作的也是 nextListeners 数组）。既然如此，要 currentListeners 有何用？**

**currentListeners 在此处的作用，就是为了记录下当前正在工作中的 listeners 数组的引用，将它与可能发生改变的 nextListeners 区分开来，以确保监听函数在执行过程中的稳定性。**





递的render阶段：beginwork：目的是创建当前fiber节点的第一个子fiber节点，判断当前fiber的类型，进入不同的update方法，进入update之后会判断当前的fiber是否存在current Fiber来决定是否标记effectTag，接着进入reconcileChild，然后判断它的child是什么类型，如果是一个单一的节点，就进入reconcileSingleElement，最终创建一个子fiber节点fiberNode