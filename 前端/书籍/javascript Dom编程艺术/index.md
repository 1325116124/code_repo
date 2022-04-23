# javascript Dom编程艺术

## 第三章

**DOM**

D: document

O: Object

M: Model

**节点**

元素节点、文本节点、属性节点

## 第四章 javascript图片库

总结：

- childNodes
- nodeType
- nodeValue
- firstChild
- lastChild

## 第五章 最佳实践

### 平稳退化

确保网页在没有javascript的情况下也能正常工作

注意避免：

- 内嵌的事件处理函数
- JavaScript伪协议

### 分离javascript

把网页的结构和内容与javascript脚本的动作行为分开

### 向后兼容性

确保老版本的浏览器不会因为你的javascript脚本而死掉

方法：

- 对象检测（通过if判断api是否支持）
- 浏览器嗅探技术：通过浏览器商家提供的信息

### 性能考虑

确定脚本执行的性能最优

方法：

- 尽量少访问DOM和尽量减少标记
- 合并和放置脚本（减少请求数量通常都是在性能优化时首先要考虑的）
- 压缩脚本

## 第六章 图片案例的改进

**1.首先想的是它支持平稳退化吗**

也就是当javascript功能被禁用了会怎么样

**2.html和javascript代码是否是分离的**

书中共享onload事件的案例

```JS
// 方法一
window.onload = function () {
    firstFunction()
    secondFunction()
}
// 方法二
function addLoadEvent(func) {
    var oldonload = window.onload
    if(typeof window.onload !== 'function') {
        window.onload = func
    } else {
        window.onload = function() {
            oldonload()
            func()
        }
    }
}
```

**3.判断对象是否存在或者说兼容浏览器的api**

**DOM Core和HTML-DOM**

DOM通过很多方法来获取属性，而这些属性很多是内置在HTML-DOM上的，比如src或者href等等
