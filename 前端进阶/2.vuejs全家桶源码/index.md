# Vuejs全家桶和源码实现

## 第一节全家桶实现原理和细节

### 预习

vue-router的实现细节：通过哈希变化监听哈希变化函数，根据路由配置表，将对应的component组件取出来进行渲染render

vuex的实现原理：实现store的state，dispatch和commit方法，然后通过vue的响应式来重新渲染

需要的前置知识：

1. vue plugin

2. render(h)

3. vue.util.defineReactive
4. es6 class

### 正课

1. 为什么要Vue.use(router)  --- router是一个插件

2. 为什么要new Vue({router}).$mount("#app")
3. router-view的作用
4. 为什么router-view和router-link是全局的

**需求分析**

spa ⻚⾯不能刷新

- hash #/about

- History api /about

根据url显示对应的内容

- router-view

- 数据响应式：current变量持有url地址，⼀旦变化，动态重新执⾏render

**任务**

实现⼀个插件

- 实现VueRouter类

​	- 处理路由选项

​	- 监控url变化，hashchange

​	- 响应这个变化

- 实现install⽅法

​	- $router注册

​	- 两个全局组件

## 第二节

## 第三节vue2源码分析

入口文件：entry-runtime-with-compiler.js

Vue实例中的优先级问题：render > template > el，因为template或者el最后还是会编译为render函数的

该文件的作用：扩展了$mount

- 首先提供了ssr的配置选项
- 然后判断options中的render，template，el，最终根据优先级转换为render函数，挂载到options.render中，最后进行mount挂载



依赖的Vue实例文件：src/platforms/web/runtime/index.js

- 安装了一个平台特有的patch函数，作用是将vdom转换为dom
  - init，初始化创建
  - update，diff算法，oldVnode和vnode的diff

- 实现了挂载方法



又依赖了：src/core/index.js

- 初始化所有全局API：Vue.component/filter/directive/use/set/delete/...



真正Vue的构造函数：src/core/instance/index.js

- 声明Vue构造函数

![image-20220406113346929](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220406113346929.png)



initMixin：vue\src\core\instance\init.js

- 选项合并：合并系统选项和用户选项
- new Vue()发生了什么

![image-20220406142015388](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220406142015388.png)



挂载的过程：src/core/instance/lifecycle.js

- updateComponent
- new Watcher 一个组件一个watcher
- render -> vdom
- patch ->真实dom

## 第四节vue2源码分析

queueWatcher(this)

nextTick(flushSchedulerQueue): 传入的flushSchedulerQueue会放入callbacks数组中

timerFunc()

p.then(flushCallbacks)

-----------------------------

flushCallbacks() 遍历执行callbacks中所有函数

flushSchedulerQueue() 遍历执行queue，执行里面所有watcher里面的更新函数

watcher.run()

watcher.get()

watcher.getter()

updateComponent()

vm._render()得到了vDom

vm.\_update(vm.\_render(), hydrating) -> vm._update(vDom)

patch(oldvDom,vDom)

你如何理解虚拟dom和diff算法：

首先在vue1的时候是没有虚拟dom的，因为vue1是每一个更改都对应一个watcher，所以根本不需要虚拟dom，但是这样带来的问题是，每一个小更改都对应一个watcher实例，那么在大型项目的时候就会特别的卡。所以vue2引入了虚拟dom，每一个组件对应一个watcher实例，那么需要更新的地方就需要通过新旧的虚拟dom进行一个diff算法来进行一个比对更新

虚拟DOM轻量、快速：当它们发生变化时通过新旧虚拟DOM比对可以得到最小DOM操作量，配合异步更新策略减少刷新频率，从而提升性能

**下面是初始化的patch算法的细节:**

```js
// 返回的就是更新函数patch
return function patch (oldVnode, vnode, hydrating, removeOnly) {
    // patch(oldVnode, null)
    // 删除指定vdom树
    if (isUndef(vnode)) {
        if (isDef(oldVnode)) invokeDestroyHook(oldVnode)
        return
    }

    let isInitialPatch = false
    const insertedVnodeQueue = []

    // patch(null, vnode)
    // 批量的创建行为
    if (isUndef(oldVnode)) {
        // empty mount (likely as component), create new root element
        isInitialPatch = true
        createElm(vnode, insertedVnodeQueue)
    } else {
        const isRealElement = isDef(oldVnode.nodeType)
        if (!isRealElement && sameVnode(oldVnode, vnode)) {
            // patch existing root node
            patchVnode(oldVnode, vnode, insertedVnodeQueue, null, null, removeOnly)
        } else {
            if (isRealElement) {
                // 初始化逻辑：将传入的vnode转换为dom，追加到宿主元素oldVnode
                // mounting to a real element
                // check if this is server-rendered content and if we can perform
                // a successful hydration.
                if (oldVnode.nodeType === 1 && oldVnode.hasAttribute(SSR_ATTR)) {
                    oldVnode.removeAttribute(SSR_ATTR)
                    hydrating = true
                }
                if (isTrue(hydrating)) {
                    if (hydrate(oldVnode, vnode, insertedVnodeQueue)) {
                        invokeInsertHook(vnode, insertedVnodeQueue, true)
                        return oldVnode
                    } else if (process.env.NODE_ENV !== 'production') {
                        warn(
                            'The client-side rendered virtual DOM tree is not matching ' +
                            'server-rendered content. This is likely caused by incorrect ' +
                            'HTML markup, for example nesting block-level elements inside ' +
                            '<p>, or missing <tbody>. Bailing hydration and performing ' +
                            'full client-side render.'
                        )
                    }
                }
                // either not server-rendered, or hydration failed.
                // create an empty node and replace it
                // 标准化：将真实dom转换为虚拟dom
                oldVnode = emptyNodeAt(oldVnode)
            }

            // replacing existing element
            const oldElm = oldVnode.elm // 宿主元素，比如div#app
            const parentElm = nodeOps.parentNode(oldElm) // 宿主元素的父元素

            // create new node
            // vnode -> dom -> append
            createElm(
                vnode,
                insertedVnodeQueue,
                // extremely rare edge case: do not insert if old element is in a
                // leaving transition. Only happens when combining transition +
                // keep-alive + HOCs. (#4590)
                oldElm._leaveCb ? null : parentElm, // 宿主元素的父亲
                nodeOps.nextSibling(oldElm) // 宿主元素的兄弟元素
            )

            // update parent placeholder node element, recursively
            if (isDef(vnode.parent)) {
                let ancestor = vnode.parent
                const patchable = isPatchable(vnode)
                while (ancestor) {
                    for (let i = 0; i < cbs.destroy.length; ++i) {
                        cbs.destroy[i](ancestor)
                    }
                    ancestor.elm = vnode.elm
                    if (patchable) {
                        for (let i = 0; i < cbs.create.length; ++i) {
                            cbs.create[i](emptyNode, ancestor)
                        }
                        // #6513
                        // invoke insert hooks that may have been merged by create hooks.
                        // e.g. for directives that uses the "inserted" hook.
                        const insert = ancestor.data.hook.insert
                        if (insert.merged) {
                            // start at index 1 to avoid re-invoking component mounted hook
                            for (let i = 1; i < insert.fns.length; i++) {
                                insert.fns[i]()
                            }
                        }
                    } else {
                        registerRef(ancestor)
                    }
                    ancestor = ancestor.parent
                }
            }

            // destroy old node
            if (isDef(parentElm)) {
                // 移除模板元素
                removeVnodes([oldVnode], 0, 0)
            } else if (isDef(oldVnode.tag)) {
                invokeDestroyHook(oldVnode)
            }
        }
    }

    invokeInsertHook(vnode, insertedVnodeQueue, isInitialPatch)
    return vnode.elm
}
```

**patch的实现**

首先进行树级别比较，可能有三种情况：增删改。

new VNode不存在就删；

old VNode不存在就增；

都存在就执行diffff执行更新



作业：

- 1. patch函数是怎么获取的(web/runtime/index.js)

![image-20220407210935444](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220407210935444.png)

- 2.节点属性是如何更新的

首先是初始化cbs的映射关系

![image-20220407211052728](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220407211052728.png)

然后在patchVnode函数中使用，直接将对应的所有函数都执行一遍

![image-20220407211157549](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220407211157549.png)

- 3.口述diff

## 第五节vue2源码分析

组件注册：

- Vue.component():
  - 将组件定义转换为构造函数
  - 给全局选项中components里面加入组件定义

![image-20220408141447459](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220408141447459.png)

组件实例化

组件渲染更新

parentCom.patch

Comp

parentComp.render()

_c('comp') => vnode

comp = new Ctor

comp.$mount()

new Watcher()

updateComponent()

comp.render()

patch

dom

 
