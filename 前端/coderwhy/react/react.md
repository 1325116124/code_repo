# React

## React开发依赖

开发React必须依赖三个库：

react：包含react所必须的核心代码

react-dom：react渲染在不同平台所需要的核心代码

babel：将jsx转换成React代码的工具

React和Babel的关系: 那么我们就可以直接编写jsx（JavaScript XML）的语法，并且让babel帮助我们转换成React.createElement。



## 认识JSX

JSX是一种JavaScript的语法扩展（eXtension），也在很多地方称之为JavaScript XML，因为看起就是一段XML语法；

它用于描述我们的UI界面，并且其完成可以和JavaScript融合在一起使用；

它不同于Vue中的模块语法，你不需要专门学习模块语法中的一些指令（比如v-for、v-if、v-else、v-bind）；



## 事件监听方法中的this

1.事件监听方法中的this

​    默认情况下React在调用事件监听方法的时候, 是通过apply来调用的

​    并且在调用的时候将监听方法中的this修改为了undefined

​    所以默认情况下我们是无法在监听方法中使用this的



## 虚拟DOM的创建过程

React利用ReactElement对象组成了一个JavaScript的对象树；

JavaScript的对象树就是大名鼎鼎的虚拟DOM（Virtual DOM）；

## 为什么使用虚拟DOM

很难跟踪状态发生的改变：原有的开发模式，我们很难跟踪到状态发生的改变，不方便针对我们应用程序进行调试

操作真实DOM性能较低：传统的开发模式会进行频繁的DOM操作，而这一的做法性能非常的低

DOM操作会引起浏览器的回流和重绘，所以在开发中应该避免频繁的DOM操作



## 脚手架

编程中提到的脚手架（Scaffold），其实是一种工具，帮我们可以快速生成项目的工程化结构；

每个项目作出完成的效果不同，但是它们的基本工程化结构是相似的；

既然相似，就没有必要每次都从零开始搭建，完全可以使用一些工具，帮助我们生产基本的工程化模板；

不同的项目，在这个模板的基础之上进行项目开发或者进行一些配置的简单修改即可；

这样也可以间接保证项目的基本机构一致性，方便后期的维护

![image-20220225200626691](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220225200626691.png)



## WebPack

webpack是一个现代JavaScript 应用程序的静态模块打包器(module bundler)；

当webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个bundle；

yarn eject：弹出webpack的配置文件



## 组件化思想

如果我们将一个页面中所有的处理逻辑全部放在一起，处理起来就会变得非常复杂，而且不利于后续的管理以及扩展。

但如果，我们讲一个页面拆分成一个个小的功能块，每个功能块完成属于自己这部分独立的功能，那么之后整个页面的管理和维护就变得非常容易了。

我们将一个完整的页面分成很多个组件；

每个组件都用于实现页面的一个功能块；

而每一个组件又可以进行细分；

而组件本身又可以在多个地方进行复用





## setState

setState设计为异步，可以显著的提升性能；

如果每次调用setState都进行一次更新，那么意味着render函数会被频繁调用，界面重新渲染，这样效率是很低的；

最好的办法应该是获取到多个更新，之后进行批量更新

![image-20220225202930868](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220225202930868.png)

## React更新机制

state/props的数据发生改变时，会调用React的render方法，产生新的Dom树，新旧Dom树进行diff算法，计算出差异进行更新，更新到真实的Dom上

React对这个算法进行了优化，将其优化成了O(n)，如何优化的呢？

同层节点之间相互比较，不会跨节点比较

不同类型的节点，产生不同的树结构

开发中，可以通过key来指定哪些节点在不同的渲染下保持稳定



## Diff算法

### 情况一：对比不同类型的元素

当节点为不同的元素，React会拆卸原有的树，并且建立起新的树：

当一个元素从<a>变成<img>，从<Article>变成<Comment>，或从<Button>变成<div>都会触发一个完整的重建流程；

当卸载一棵树时，对应的DOM节点也会被销毁，组件实例将执行componentWillUnmount()方法；

当建立一棵新的树时，对应的DOM节点会被创建以及插入到DOM中，组件实例将执行componentWillMount()方法，紧接着componentDidMount()方法；

###  情况二：对比同一类型的元素

当比对两个相同类型的React 元素时，React 会保留DOM节点，仅比对及更新有改变的属性。

如果是同类型的组件元素：

组件会保持不变，React会更新该组件的props，并且调用componentWillReceiveProps()和componentWillUpdate()方法；

下一步，调用render()方法，diff算法将在之前的结果以及新的结果中进行递归；

### 情况三：对子节点进行递归

在默认条件下，当递归DOM节点的子元素时，React 会同时遍历两个子元素的列表；当产生差异时，生成一个mutation。

可以进行keys的优化



## Redux

![image-20220225213240227](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220225213240227.png)

![image-20220225213840414](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220225213840414.png)