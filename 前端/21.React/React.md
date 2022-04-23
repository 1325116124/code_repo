

# React

## 开篇

```html
<!--
1.什么是React?
- React 起源于 Facebook 的内部项目，
  因为Facebook对市场上所有 JavaScript MVC 框架，都不满意，
  就决定自己写一个框架，用来架设 Instagram 的网站
来源: https://reactjs.org/blog/2013/06/05/why-react.html

2.什么是框架?
- 框架是一个'半成品',已经对基础的代码进行了封装并提供相应的API，
  开发者在使用框架时,可以复用框架中封装好的代码，从而提高工作效率
- 框架就是毛坯房, 已经帮助我们搭建好了基本的架子, 我们只需要拿过来根据我们自己的需求装修即可

3.为什么要学习框架?
- 提升开发效率：
    + 对于企业来说，时间就是金钱
    + 对于个人来说, 时间≈女朋友

4.为什么要学习React?
- 真香定律(国内技术发展潜规则)
    + 国外流行 -> 国内大厂尝试 -> 大厂觉得很香 -> 其它公司觉得也很香
- 国外流行
    + 2020 Stackoverflow 全球开发者调研报告中, 最受欢迎框架排第二
        + https://insights.stackoverflow.com/survey/2020
        + https://www.sohu.com/a/398379279_827544
    + HackerRank 2020 全球开发者调研报告, 程序员最想学习框架排第一
        + https://research.hackerrank.com/developer-skills/2020
        + https://segmentfault.com/a/1190000021729876


5.为什么要学习React?
- 安全可靠
    + React是由Facebook来更新和维护, 所以一般不会出现跑路情况
- 思想升华
    + React是一个开源项目, 融合了全世界诸多优秀成员的编程思想
- 值得借鉴
    +  Vue.js设计之初，有很多的灵感来自Angular和React
       Vue3的很多新特性, 在React中你也能看到它们的身影 ()
       诸如: Composition API / Fragment / Teleport(Protal)/ Suspense

6.为什么要学习React?
- 面向未来, 迎接5G
    + 2013年，React发布之初主要是开发Web页面
    + 2015年，Facebook推出了ReactNative，用于开发移动端跨平台(Android/iOS APP)
    （虽然目前Flutter非常火爆，但是还是有很多公司在使用ReactNative）
    + 2017年，Facebook推出ReactVR，用于开发虚拟现实Web应用程序
    （随着5G的普及，VR也会是一个火爆的应用场景）
-->
<!--
1.React核心思想?
- 数据驱动界面更新(声明式渲染):
   + 只要数据发生了改变, 界面就会自动改变
- 组件化开发(乐高帝国):
   + 将网页拆分成一个个独立的组件来编写,然后再将编写好的组件拼接成一个完整的网页
   + https://cn.vuejs.org/images/components.png
- https://reactjs.org/
-->
```

## 虚拟dom和真实dom

```html
<!--
0.什么是虚拟DOM?
- 虚拟 DOM 是相对于浏览器所渲染出来的真实 DOM 的
- 虚拟 DOM 就是使用JS对象来表示页面上真实的 DOM
- 例如:
<div id="name" title= "name">  // 真实的DOM
let obj = {                    // 虚拟DOM
  tagName: 'div',
  attrs:{
    id: "name" ，
    title: "name"
	}
}
-->
```

## React创建元素和渲染元素

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./react17/react.development.v17.js"></script>
    <script src="./react17/react-dom.development.v17.js"></script>
</head>
<body>
<!--
1.使用React的几种方式?
- 自行配置
    + https://zh-hans.reactjs.org/docs/add-react-to-a-website.html
- 通过脚手架自动配置
    + https://zh-hans.reactjs.org/docs/create-a-new-react-app.html
-->
<!--
2.react.js和react-dom.js
- react.js包含了React和React-Native所共同拥有的核心代码, 主要用于'生成虚拟DOM'
- react-dom.js包含了针对不同平台渲染不同内容的核心代码, 主要用于'将虚拟DOM转换为真实DOM'
- 简而言之:
    + 利用react.js编写界面(创建虚拟DOM)
    + 利用react-dom.js渲染界面(创建真实DOM)
-->
<!--
3.React如何创建DOM元素?
- 在React中, 我们不能通过 HTML标签 直接创建DOM元素
- 在React中, 我们必须先通过react.js创建虚拟DOM, 在通过react-dom.js渲染元素(创建真实DOM)

4.如何通过react.js创建虚拟DOM?
- 通过React.createElement()方法
- 该方法接收3个参数
    + 第一个参数: 需要创建的元素类型或组件
    + 第二个参数: 被创建出来的元素拥有的属性
    + 第三个参数: 被创建出来的元素拥有的内容(可以是多个)

https://zh-hans.reactjs.org/docs/react-api.html#createelement
-->
<!--
5.如何通过react-dom.js渲染虚拟DOM?
- 通过 ReactDOM.render()方法
- 该方法接收3个参数
    + 第一个参数: 被渲染的虚拟DOM
    + 第二个参数: 要渲染到哪个元素中
    + 第三个参数: 渲染或更新完成后的回调函数

https://zh-hans.reactjs.org/docs/react-dom.html#render
-->
<!--
6.如何给元素添加监听?
- 给元素添加监听的本质就是给元素添加属性
  所以可以在createElement()的第二个参数中添加
- <button onclick="btnClick">按钮</button>
- React.createElement('button', {onClick: btnClick}, '按钮');

注意事项:
如果想给元素绑定事件, 那么事件名称必须是驼峰命名
-->
<div id="app"></div>
<script>
    // 1.创建虚拟DOM
    let message = '知播渔';
    // <div>知播渔</div>
    let oDiv = React.createElement('div', null, message);

    // 2.将虚拟DOM转换成真实DOM
    ReactDOM.render(oDiv, document.getElementById('app'), ()=>{
        console.log('已经将虚拟DOM转换成了真实DOM, 已经渲染到界面上了');
    });
</script>
</body>
</html>

```

## React渲染多个元素，并且实现监听原理

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="../react17/react.development.v17.js"></script>
    <script src="../react17/react-dom.development.v17.js"></script>
</head>
<body>
<!--
1.render方法注意点
- 多次渲染, 后渲染会覆盖先渲染
- render方法一次只能渲染一个元素/组件

2.createElement方法注意点
- 可以添加3个以上参数, 后续参数都会作为当前创建元素内容处理

3.如何给元素添加监听?
- 给元素添加监听的本质就是给元素添加属性
  所以可以在createElement()的第二个参数中添加
- <button onclick="btnClick">按钮</button>
- React.createElement('button', {onClick: btnClick}, '按钮');

注意事项:
如果想给元素绑定事件, 那么事件名称必须是驼峰命名
-->
<div id="app"></div>
<script>
    // 1.创建虚拟DOM
    let message = '知播渔';
    // <div>知播渔</div>
    let oDiv = React.createElement('div', null, message);
    // <button onclick='myfn'>按钮</button>
    let oBtn = React.createElement('button', null, '按钮');
    // 注意点: 默认createElement方法只能接收3个参数, 但是我们也可以传递3个以上的参数
    //        如果我们传递了3个以上参数, 那么从第3个开始都会当做是内容来处理
    /*
    React.createElement(
      type,
      [props],
      [...children]
    )
    * */
    // 注意点: 如果想通过React绑定事件, 那么事件名称必须是驼峰命名
    let oRoot = React.createElement('div', {onClick:myFn}, oDiv, oBtn);

    // 2.将虚拟DOM转换成真实DOM
    ReactDOM.render(oRoot, document.getElementById('app'), ()=>{
        console.log('已经将虚拟DOM转换成了真实DOM, 已经渲染到界面上了');
    });
    function myFn() {
        message = 'www.it666.com';
        // 注意点: 默认情况下载React中, 修改完数据之后, 是不会自动更新界面的
        let oDiv = React.createElement('div', null, message);
        let oBtn = React.createElement('button', null, '按钮');
        let oRoot = React.createElement('div', {onClick:myFn}, oDiv, oBtn);
        ReactDOM.render(oRoot, document.getElementById('app'), ()=>{
            console.log('已经将虚拟DOM转换成了真实DOM, 已经渲染到界面上了');
        });
    }
    // 注意点: render方法最多只能接收3个参数, 所以不能同时渲染多个元素
    // ReactDOM.render(oDiv, oBtn, document.getElementById('app'), ()=>{
    //     console.log('已经将虚拟DOM转换成了真实DOM, 已经渲染到界面上了');
    // });
    // 注意点: 如果多次调用render方法, 那么后渲染的会覆盖先渲染的
    // ReactDOM.render(oBtn, document.getElementById('app'), ()=>{
    //     console.log('已经将虚拟DOM转换成了真实DOM, 已经渲染到界面上了');
    // });
</script>
</body>
</html>

```

## Jsx的语法格式

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./react17/react.development.v17.js"></script>
    <script src="./react17/react-dom.development.v17.js"></script>
    <script src="./react17/babel.min.js"></script>
</head>
<body>
<!--
1.通过createElement创建元素存在的问题?
- 如果结构比较简单还好, 但是如果结构比较复杂, 就比较难以下手
  所以大牛们就发明了JSX, 专门用来编写React中的页面结构体

2.什么是JSX?
- JSX === JavaScript + X === (XML) === (eXtension)
- JSX 是一个看起来很像 XML 的 JavaScript 语法扩展

3.为什么要使用JSX?
- 使用JSX使得我们在React中编写页面结构更为简单、灵活
- JSX 是类型安全的，在编译过程中就能发现错误
- JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化
- 防止XSS注入攻击

https://zh-hans.reactjs.org/docs/introducing-jsx.html
-->
<!--
4.JSX本质?
- 浏览器只认识JS不认识JSX, 所以我们编写的JSX代码是无法在浏览器中执行的
- 为了解决这个问题, 我们需要借助babel将JSX转换成JS, 也就是转换成React.createElement();

https://zh-hans.reactjs.org/docs/react-without-jsx.html
https://babeljs.io/repl/

5.如何在项目中将JSX转换成JS?
- 导入babel.js
- 在script标签上添加type="text/babel"
-->
<div id="app"></div>
<script type="text/babel">
    // 1.创建虚拟DOM
    let message = '知播渔123';

    function myRender() {
        // let oDiv = React.createElement('div', null, message);
        // let oBtn = React.createElement('button', null, '按钮');
        // let oRoot = React.createElement('div', {onClick:myFn}, oDiv, oBtn);

        let oRoot = (
            <div>
                <div>{message}</div>
                <button onClick={myFn}>按钮</button>
            </div>
        )

        ReactDOM.render(oRoot, document.getElementById('app'), ()=>{
            console.log('已经将虚拟DOM转换成了真实DOM, 已经渲染到界面上了');
        });
    }
    myRender();
    function myFn() {
        message = 'www.it666.com';
        myRender();
    }
</script>
</body>
</html>

```

##  React中定义组件的方式

### Es5的组件定义方式（无状态）

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.在React中如何定义组件?
- 在React中创建组件有两种方式
    + 第一种: 通过ES6之前的构造函数来定义(无状态组件)
    + 第二种: 通过ES6开始的class来定义(有状态组件)
-->
<!--
2.如何通过ES5的构造函数来定义组件
- 在构造函数中返回组件的结构即可
function Home() {
    return (
      <div>
          <div>{message}</div>
          <button onClick={btnClick}>按钮</button>
      </div>
    );
}
https://zh-hans.reactjs.org/docs/components-and-props.html
-->
<div id="app"></div>
<script type="text/babel">
    let message = '知播渔';
    function Home() {
        return (
            <div>
                <div>{message}</div>
                <button onClick={myFn}>按钮</button>
            </div>
        )
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));

    function myFn() {
        message = 'www.it666.com';
        ReactDOM.render(<Home/>, document.getElementById('app'));
    }
</script>
</body>
</html>

```

### ES6定义组件的方式（通过class）

**通过class关键字，并且继承自React.Component类，实现其中的render方法即可**

```html
<!doctype html>
<html lang="en">
<body>
<!--
3.如何通过ES6的class来定义组件
- 定义一个类, 在这个类中实现render方法, 在render方法中返回组件的结构即可
class Home extends React.Component{
    render(){
        return (
            <div>
                <div>{message}</div>
                <button onClick={btnClick}>按钮</button>
            </div>
        )
    }
}
https://zh-hans.reactjs.org/docs/components-and-props.html
-->
<div id="app"></div>
<script type="text/babel">
    let message = '知播渔';
    /*
    function Home() {
        return (
            <div>
                <div>{message}</div>
                <button onClick={myFn}>按钮</button>
            </div>
        )
    }
     */
    class Home extends React.Component{
        render(){
            return (
                <div>
                    <div>{message}</div>
                    <button onClick={myFn}>按钮</button>
                </div>
            )
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));

    function myFn() {
        message = 'www.it666.com';
        ReactDOM.render(<Home/>, document.getElementById('app'));
    }
</script>
</body>
</html>

```

### 有状态组件和无状态组件

```html
```

## React中this的指向问题

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.this指向问题
- 在ES6之前, 方法中的this谁调用就是谁,
  并且还可以通过call/apply/bind方法修改this
- 从ES6开始, 新增了箭头函数, 箭头函数没有自己的this,
  箭头函数中的this是函数外最近的那个this
  并且由于箭头函数没有自己的this, 所以不能通过call/apply/bind方法修改this
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions


2.监听事件中的this
- React内部在调用监听方法的时候, 默认会通过apply方法将监听方法的this修改为了undefined
  所以在监听方法中无法通过this拿到当前组件的state. (undefined.state)
- 如果想在监听方法中拿到当前组件的state, 那么就必须保证监听方法中的this就是当前实例
  所以我们可以借助箭头函数的特性, 让React无法修改监听方法中的this, 让监听方法中的this就是当前实例
-->
<div id="app"></div>
<script type="text/babel">
    // let message = '知播渔';
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                message:'知播渔123'
            }
        }
        render(){
            return (
                <div>
                    <div>{this.state.message}</div>
                    <button onClick={this.myFn}>按钮</button>
                </div>
            )
        }
        myFn = () => {
            // 注意点: React在调用监听方法的时候, 会通过apply修改监听方法的this
            //        所以在普通的方法中, 我们拿到的this是undefined,
            //        所以我们无法在普通的方法中通过this拿到当前组件的state
            // func.apply(context, funcArgs);
            // console.log(this, '----------');
            this.state.message = 'www.it666.com';
            console.log(this.state.message);
            ReactDOM.render(<Home/>, document.getElementById('app'));
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));

    // function myFn() {
    //     message = 'www.it666.com';
    //     ReactDOM.render(<Home/>, document.getElementById('app'));
    // }
</script>
</body>
</html>

```

## setState修改组件中state属性

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.state属性注意点
- 永远不要直接修改state
- 直接修改state并不会触发界面更新
- 只有使用setState方法修改state才会触发界面更新
https://zh-hans.reactjs.org/docs/state-and-lifecycle.html
-->
<div id="app"></div>
<script type="text/babel">
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                message:'知播渔123'
            }
        }
        render(){
            return (
                <div>
                    <div>{this.state.message}</div>
                    <button onClick={this.myFn}>按钮</button>
                </div>
            )
        }
        myFn = () => {
            // 注意点:
            // 如果要修改state中的数据, 那么永远不要直接修改
            // 如果要修改state中的数据, 那么需要通过setState方法来修改
            // this.state.message = 'www.it666.com';
            // console.log(this.state.message);
            this.setState({
                message: 'www.it666.com'
            })
            //ReactDOM.render(<Home/>, document.getElementById('app'));
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

## 数据驱动界面更新原理

```html
<!doctype html>
<html lang="en">
<body>
<div id="app"></div>
<script type="text/babel">
    class NJComponent extends React.Component{
        constructor(){
            super();
            this.state = null;
        }
        setState(val){
            console.log('需要更新界面');
            this.state = val;
            ReactDOM.render(this.render(), document.getElementById('app'));
        }
    }
    class Home extends NJComponent{
        constructor(){
            super();
            this.state = {
                message:'知播渔123'
            }
        }
        render(){
            return (
                <div>
                    <div>{this.state.message}</div>
                    <button onClick={this.myFn}>按钮</button>
                </div>
            )
        }
        myFn = () => {
            this.state.message = 'www.it666.com';

            this.setState({
                message: 'www.it666.com'
            })
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

## 计数器案例

```html
<!doctype html>
<html lang="en">
<body>
<div id="app"></div>
<script type="text/babel">
    /*购物车*/
    class Counter extends React.Component{
        constructor(){
            super();
            this.state = {
                count : 0
            }
        }
        render(){
            return (
                <div>
                    <button onClick={this.sub}>减少</button>
                    <span>{this.state.count}</span>
                    <button onClick={this.add}>增加</button>
                </div>
            )
        }
        sub = ()=>{
            this.setState({
                count: this.state.count - 1
            })
        }
        add = ()=>{
            this.setState({
                count: this.state.count + 1
            })
        }
    }
    ReactDOM.render(<Counter/>, document.getElementById('app'));
</script>
</body>
</html>

```

## JSX语法详解

### Jsx注释

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.JSX注释
- 在JSX中遇到<会当做XML元素解析, 遇到{会当做JS解析
  所以在JSX中不能使用HTML的注释

- JSX代码用于定义网页的结构, 而网页的内容中必然会包含内容
  所以直接在JSX中使用单行注释//或多行注释/**/会被当做元素的内容处理
- 正确打开姿势:
https://zh-hans.reactjs.org/docs/faq-build.html
-->
<div id="app"></div>
<script type="text/babel">
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                message:'知播渔'
            }
        }
        render(){
            /*
            1.在JSX中是不能使用HTML的注释的, 因为JSX会把它当做是一个元素来处理
            <!--注释内容-->
            2.在JSX中是不能使用JS的单行注释的, 因为元素中是有内容的, 所以JSX会把单行注释当做是元素的内容来处理
            // 单行注释
            3.在JSX中是不能使用JS的多行注释的, 因为元素中是有内容的, 所以JSX会把多行注释当做是元素的内容来处理
            4.在JSX中不能使用单行注释和多行注释的原因是, 默认情况下JSX会把注释的内容当做是元素的内容来处理
              所以我们只需要告诉JSX, 注释的内容不是元素的内容即可
              所以我们只需要将注释的内容写到{}中, JSX就会把注释的内容当做是JS来处理
            * */
            return (
                <div>
                    {
                        // 单行注释
                        /*
                        多行注释
                        * */
                    }
                    <p>{this.state.message}</p>
                </div>
            )
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

![img](file:///C:\Users\86157\AppData\Roaming\Tencent\Users\1325116124\QQ\WinTemp\RichOle\1$NLR7O`F0]68[`A4]3]L46.png)

### Jsx绑定属性

**绑定普通属性的时候，只需要在字符串外加上{}即可**

**绑定类名无法使用class属性，因为在js中class是关键字。必须使用className**

**绑定样式的时候，必须将所有的样式放进一个对象，同时对于短横线连接的属性必须使用驼峰命名法**

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.JSX绑定内容
- 在JSX中只要看到{}就会当做JS解析(执行里面的JS代码)
  所以无论是绑定属性,还是绑定类名,还是绑定样式, 只需要将字符串改为{}
  然后再通过JS动态获取, 动态绑定即可

- 绑定普通属性
    + <p title="我是标题">我是段落</p>
    + <p title={message}>我是段落</p>
- 绑定类名(class)
    +  由于JSX本质是转换成JS代码, 而在JS中class有特殊含义, 所以不能使用
            同理可证, 但凡是属性名称是JS关键字的都不能直接使用
		因此绑定类名用className
- 绑定样式(style)
    + 由于样式是键值对形式的, 所以在JSX中如果想要动态绑定样式
      必须将样式放到一个对象中, 并且所有以-连接的样式名称都要转换成驼峰命名
    +  <p style={{color:'red', fontSize:'50px'}}>绑定样式</p>
-->
<div id="app"></div>
<!--<p id="box" class="active" style="color: red; font-size: 100px">我是段落</p>-->
<script type="text/babel">
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                message:'知播渔'
            }
        }
        render(){
            return (
                <div>
                    {/*1.对于普通属性而言, 过去怎么绑定, 现在就怎么绑定*/}
                    <p id="box">{this.state.message}</p>
                    <p title={this.state.message}>{this.state.message}</p>
                    {/*2.如果想通过JSX绑定类名, 那么不能直接通过class来绑定
                         JSX本质是转换成JS代码, 而在JS代码中class是一个关键字
                         所以不能直接使用class来绑定类名*/}
                    {/*3.所以以后但凡名称是JS关键字的属性, 都不能直接绑定*/}
                    <p className="active">{this.state.message}</p>
                    {/*4.如果想通过JSX绑定样式, 那么不能像过去一样编写
                         必须通过对象的形式来绑定*/}
                    {/*5.在绑定样式的时候, 如果过去在原生中是通过-连接的, 那么就必须转换成驼峰命名*/}
                    <p style={{color:'red', fontSize:'100px'}}>{this.state.message}</p>
                </div>
            )
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

### Jsx嵌入内容

**对于特俗的一些数据值，比如null、undefined、false之类有特定含义的内容无法在{}中进行显示，必须先转换成字符串然后才能显示**

```html
<!doctype html>
<html lang="en">
<body>
<!--
2.JSX嵌入数据注意点
- 以下代码等价
<div></div>
<div>{[]}</div>
<div>{false}</div>
<div>{true}</div>
<div>{null}</div>
<div>{undefined}</div>
- 要想显示以上内容必须先转换成字符串
- 以下代码报错
<div>{{}}</div>
- 其它数据正常显示
-->
<!--
1.JSX嵌入表达式
- 只要是合法的表达式, 都可以嵌入到JSX中
-->
<div id="app"></div>
<script type="text/babel">
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                flag:true,
                message: '知播渔'
            }
        }
        render(){
            return (
                <div>
                    {/*1.任何合法的JS表达式都可以嵌入到{}中*/}
                    <p>{this.state.flag ? '为真' : '为假'}</p>
                    {/*2.以下嵌入的内容不会被显示出来 [] true false null undefined*/}
                    <p>{[]}</p>
                    <p>{true}</p>
                    <p>{false}</p>
                    <p>{null}</p>
                    <p>{undefined}</p>
                    {/*3.如果想显示上面的这些内容, 那么就必须先转换成字符串
                         但是对于空数组来说, 哪怕转换成了字符串, 也不能显示*/}
                    <p>{[].toString()}</p>
                    <p>{true + ''}</p>
                    <p>{false + ''}</p>
                    <p>{null + ''}</p>
                    <p>{undefined + ''}</p>
                    {/*4.除了上述内容以外, 其它的内容都可以正常显示*/}
                    <p>我是段落</p>
                    <p>{this.state.message}</p>
                </div>
            )
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

### Jsx的灵活性

#### Jsx实现vue中的v-if v-show

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.JSX灵活性
- JSX使我们在JS中拥有了直接编写XML代码的能力
- 所以在JS中能干的事, 在JSX中都能干
-->
<div id="app"></div>
<script type="text/babel">
    // 需求: 通过按钮控制界面上p标签的显示和隐藏
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                flag: true
            }
        }
        render(){
            return (
                <div>
                    {/*和Vue中的v-show指令很像*/}
                    {/*
                    <p style={{display: this.state.flag?'block':'none'}}>我是段落</p>
                    */}
                    {/*和Vue中的v-if指令很像*/}
                    {this.state.flag && <p>我是段落</p>}
                    <button onClick={this.myFn}>按钮</button>
                </div>
            )
        }
        myFn = () =>{
            this.setState({
                flag: !this.state.flag
            })
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

#### Jsx中使用js处理数据

**使用es6中的map处理数据**

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.JSX灵活性
- JSX使我们在JS中拥有了直接编写XML代码的能力
- 所以在JS中能干的事, 在JSX中都能干
-->
<div id="app"></div>
<script type="text/babel">
    // 需求: 通过按钮控制界面上p标签的显示和隐藏
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                names: ['鲁班', '虞姬', '黄忠']
            }
        }
        render(){
            const {names} = this.state;
            // const nameList = [];
            // nameList.push(<li>{this.state.names[0]}</li>);
            // nameList.push(<li>{this.state.names[1]}</li>);
            // nameList.push(<li>{this.state.names[2]}</li>);
            // nameList.push(<li>{this.state.names[3]}</li>);

            // const nameList = [];
            // for(let i = 0, j = names.length; i < j; i++){
            //     const li = <li>{names[i]}</li>
            //     nameList.push(li);
            // }

            // const nameList = names.map(name=>{
            //     return<li>{name}</li>
            // });
            return (
                <div>
                    {/*
                    <ul>
                        <li>{this.state.names[0]}</li>
                        <li>{this.state.names[1]}</li>
                        <li>{this.state.names[2]}</li>
                    </ul>
                    */}
                    {/*
                    <ul>{nameList}</ul>
                    */}
                    <ul>{
                        names.map(name=>{
                            return<li>{name}</li>
                        })
                    }</ul>
                </div>
            )
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

### Jsx书写规范

**1.在编写JSX代码的时候, 建议使用()将JSX代码包裹起来
2.在JSX中只能有一个根元素
3.在JSX中可以编写单标签, 也可以编写双标签
但是如果编写的是单便签, 那么就必须加上闭合符号
4.在使用组件的时候, 可以使用单便签, 也可以使用双标签
但是如果是使用的单标签, 那么就必须加上闭合符号
5.在使用组件的时候, 如果组件中没有内容, 那么建议使用单标签**

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.JSX书写规范
- JSX的顶层只能有一个根元素
- JSX中的标签可以是单标签也可以是双标签, 但如果是单标签必须闭合(/>)
- 如果JSX中包含多个元素, 建议使用()包裹
-->
<div id="app"></div>
<script type="text/babel">
    class About extends React.Component{
        render(){
            return (
                <div>about</div>
            )
        }
    }
    class Home extends React.Component{
        constructor(){
            super();
        }
        render(){
            // 1.在编写JSX代码的时候, 建议使用()将JSX代码包裹起来
            // 2.在JSX中只能有一个根元素
            // 3.在JSX中可以编写单标签, 也可以编写双标签
            //   但是如果编写的是单便签, 那么就必须加上闭合符号
            // 4.在使用组件的时候, 可以使用单便签, 也可以使用双标签
            //   但是如果是使用的单标签, 那么就必须加上闭合符号
            // 5.在使用组件的时候, 如果组件中没有内容, 那么建议使用单标签
            return (
                <div>
                    <div>456</div>
                    <div>456</div>
                    <img/>
                    {/*
                    <About></About>
                    */}
                    <About/>
                </div>
            )
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>
```

### Jsx中绑定事件

**企业开发推荐：手动绑定一个箭头函数, 然后再箭头函数的函数体中手动调用监听方法**

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.JSX绑定事件
- JSX中绑定事件必须使用驼峰命名
    + <button onClick={this.btnClick}>按钮</button>

2.监听方法this处理
- 箭头函数
- 创建时通过bind修改
- 注册时通过bind修改
- 普通函数和箭头函数结合

https://zh-hans.reactjs.org/docs/handling-events.html

3.企业开发推荐方案
- 普通函数和箭头函数结合
-->
<div id="app"></div>
<script type="text/babel">
    class Home extends React.Component{
        constructor(){
            super();
            this.state = {
                message: '知播渔'
            }
            this.myClick = this.btnClick.bind(this);
        }
        render() {
            return (
                <div>
                    <div>{this.state.message}</div>
                    {/*
                    <button onClick={this.btnClick.bind(this)}>按钮</button>
                    */}
                    {/*
                    <button onClick={this.myClick}>按钮</button>
                    */}
                    <button onClick={()=>{this.btnClick()}}>按钮</button>
                </div>
            )
        }
        /*
        2.如何解决监听方法中this默认是undefined的问题
        2.1通过箭头函数
        2.2通过添加监听方法的时候, 手动通过bind修改监听方法中的this
        2.3通过在构造函数中,  手动通过bind修改监听方法中的this
        2.4手动绑定一个箭头函数, 然后再箭头函数的函数体中手动调用监听方法
           因为箭头函数中的this, 就是当前的实例对象
           因为监听方法并不是React调用的, 而是我们在箭头函数中手动调用的
           因为普通的方法, 默认情况下谁调用就是谁
        注意点: 在企业开发中, 最为推荐的一种方式就是第四种
        * */
        btnClick(){
            // alert(123);
            console.log(this, '----');
        }
        /*
        btnClick = ()=>{
            console.log(this);
        }
        /*
        1.事件监听方法中的this
        默认情况下React在调用事件监听方法的时候, 是通过apply来调用的
        并且在调用的时候将监听方法中的this修改为了undefined
        所以默认情况下我们是无法在监听方法中使用this的
        * */
        /*
        btnClick(){
            // alert(123);
            console.log(this);
        }
         */
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

### Jsx事件传参

**直接在箭头函数中的括号传递参数即可**

```html
<!doctype html>
<html lang="en">
<body>
<div id="app"></div>
<script type="text/babel">
    class Home extends React.Component {
        constructor() {
            super();
            this.state = {
                message: '知播渔'
            }
            this.myClick = this.btnClick.bind(this);
        }
        render() {
            return (
                <div>
                    <div>{this.state.message}</div>
                    <button onClick={this.btnClick}>按钮2</button>
                    <button onClick={this.btnClick.bind(this)}>按钮3</button>
                    <button onClick={this.myClick}>按钮4</button>
                    <button onClick={()=>{this.btnClick(1, 'abc')}}>按钮5</button>
                </div>
            )
        }
        /*
        btnClick = () => {
            console.log(this);
        }
         */
        btnClick(a, b) {
            console.log(a, b);
            console.log(this);
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

### JSX事件对象

**在箭头函数中直接传递参数e即为事件对象，调用e.nativeEvent即可拿到js原生的事件对象**

```html
<!doctype html>
<html lang="en">
<body>
<!--
1.JSX事件参数
- 和原生JS一样, React在执行监听方法会传递一个事件对象给我们
  但是React传递给我们的并不是原生的事件对象, 而是一个React自己合成的事件对象

2.什么是合成事件?
- 合成事件是 React 在浏览器事件基础上做的一层包装，
  基本上有着和浏览器的原生事件有相同的接口，
  也能够进行 stopPropagation() 和 preventDefault()，
  并且合成事件在所有浏览器中的工作方式相同

- 如果由于某种原因需要浏览器的原生事件，
  则能够简单的通过 nativeEvent 属性就能够获取

- 注意点:
    + 从 ReactV0.14 起，从事件处理程序返回 false 将不再停止事件的传递。
      应当手动调用 e.stopPropagation() 或 e.preventDefault() 去阻止传递。
    + 合成事件 是合并而来。这意味着 合成事件 对象可能会被重用，
      而且在事件回调函数被调用后，所有的属性都会无效。出于性能考虑，你不能通过异步访问事件。

3.React事件处理性能优化
- React并不会把事件处理函数直接绑定到真实的节点上，
  而是使用一个统一的事件监听器 ReactEventListener，
  把所有事件绑定到结构的最外层 document 节点上，依赖冒泡机制完成事件委派

- ReactEventListener：React事件监听器维持了一个映射来保存所有组件内部的事件监听和处理函数，
  负责事件注册和事件分发。当组件在挂载或卸载时，只是在这个统一的事件监听器上插入或删除一些对象；
  当事件发生时，首先被这个统一的事件监听器处理，然后在映射里找到真正的事件处理函数并调用。
  这样简化了事件处理和回收机制，提升了效率


https://zh-hans.reactjs.org/docs/handling-events.html
https://zh-hans.reactjs.org/docs/events.html
-->
<div id="app"></div>
<script type="text/babel">
    class Home extends React.Component {
        constructor() {
            super();
            this.state = {
                message: '知播渔'
            }
        }
        render() {
            return (
                <div>
                    <div>{this.state.message}</div>
                    {/*
                    在React中当监听方法被触发的时候, React也会传递一个事件对象给我们
                    但是React传递给我们的这个事件对象并不是原生的事件对象,
                    而是React根据原生的事件对象自己合成的一个事件对象
                    注意点: 虽然传递给我们的是React自己合成的事件对象, 但是提供的API和元素的几乎一致
                           如果你用到了一个没有提供的API, 那么你也可以根据合成的事件对象拿到原生的事件对象
                    */}
                    <button onClick={(e)=>{console.log(e)}}>按钮</button>
                    <button onClick={(e)=>{console.log(e.nativeEvent)}}>按钮</button>
                </div>
            )
        }
    }
    ReactDOM.render(<Home/>, document.getElementById('app'));
</script>
</body>
</html>

```

## React组件

### create-react-app脚手架创建工具

```html
<!--
1.什么是脚手架?
脚手架是一种能快速帮助我们生成项目结构和依赖的工具
- 每个项目完成的效果不同，但是它们的基本工程化结构是相似的
- 既然相似，就没有必要每次都从零开始搭建，完全可以使用一些工具，帮助我们生成基本的项目模板
- 那么这个帮助我们生成项目模板的工具我们就称之为'脚手架'

2.什么是create-react-app?
create-react-app就是React的脚手架工具,
它可以快速的帮我们生成一套利用webpack管理React的项目模板

3.如何安装和使用create-react-app?
npm install -g create-react-app
create-react-app 项目名称
cd 项目名称
npm start
注意点: 如果我们是通过create-react-app来创建React项目
       那么在指定项目名称的时候, 项目的名称只能是英文, 并且只能是小写字母
       如果出现了多个单词, 那么我们需要通过_-来连接. myName->my_name->my-name
注意点: 第一次运行项目的时候大概率会出现一个错误, 会出现本地webpack的版本和项目依赖的webpack版本不同的错误
       如果遇到了这个错误, 我们就需要先通过 npm uninstall webapck 卸载掉本地的webpack
       再通过 npm install -g webpack@xx.xx.xx安装和项目相同版本的webpack即可

4.暴露webapck配置
npm run eject
-->
```

### 脚手架创建工具的项目目录结构

```html
<!--
1.通过create-react-app生成的项目结构解读
项目名称
├── README.md           // 说明文档
├── node_modules        // 依赖包
├── package.json        // 对应用程序的描述
├── .gitignore          // 不通过git管理的文件描述
├── public
│   ├── favicon.ico    // 应用程序收藏图标
│   ├── index.html     // 应用程序入口
│   ├── logo192.png    // manifest中PWA使用的图片
│   ├── logo512.png    // manifest中PWA使用的图片
│   ├── manifest.json  // PWA相关配置
│   └── robots.txt     // 搜索引擎爬虫权限描述文件
└── src
    ├── App.css         // 组件相关样式
    ├── App.js          // 组件相关代码
    ├── App.test.js     // 组件测试文件
    ├── index.css       // 全局样式
    ├── index.js        // 全局入口文件
    ├── logo.svg        // React图标
    └── serviceWorker.js// 注册PWA相关代码

2.什么是PWA?
小程序老祖宗
https://baijiahao.baidu.com/s?id=1612919514973793701&wfr=spider&for=pc
-->
```

### 组件开篇

**组件化开发思想**

**D:\webVideo\代码文件\21、react\任务026：03-组件-代码资料\03-组件**

```html
import React from "react"
import "./header.css"
function Header(){
  return (
      <div className={"header"}>我是头2</div>
  )
}

export default Header

<!-- header.css -->
.header{
    background-color: red;
}

<!-- App.js -->
import React from "react";
// import "./App.css"
import Header from "./components/header"
import Footer from "./components/Footer"
import Main from "./components/main"
class App extends React.Component{
  render(){
    return(
        <div>
          {/*<div className={'header'}>头部</div>*/}
          {/*<div className={'main'}>中部</div>*/}
          {/*<div className={'footer'}>底部</div>*/}
          <Header/>
          <Main/>
          <Footer/>
        </div>
    )
  }
}
export default App
```

![image-20210720105918310](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210720105918310.png)

### 父子组件通信

#### 函数式组件

##### 父组件向子组件传递

**直接在子组件标签中加入属性，在子组件中的函数构造中用参数props进行数据的接收**

```html
<!-- 父组件 -->
import React from "react";
// import "./App.css"
import Header from "./components/header"
import Footer from "./components/Footer"
import Main from "./components/main"
class App extends React.Component{
  render(){
    return(
        <div>
          <Header name={"lnj"} age={"18"}/>
          <Main/>
          <Footer/>
        </div>
    )
  }
}
export default App

<!-- 子组件Header -->
import React from "react"
import "./header.css"
function Header(props){
  console.log(props);
  return (
      <div className={"header"}>我是头2</div>
  )
}

export default Header

```

##### 对父向子组件传递数据的补充

**Header.defaultProps：如果父组件没有传递对应的值，则使用子组件设置的默认值**

**Header.propTypes：安装prop-types，使用该对象可以对父组件传递的数据进行数据类型的校验**

```html
import React from "react"
import ReactTypes from "prop-types"
import "./header.css"
function Header(props){
  console.log(props);
  return (
      <div className={"header"}>我是头2</div>
  )
}
//当父组件没有传递任何的参数的时候使用默认的参数
Header.defaultProps = {
  name:'lnj',
  age:18
}
Header.propTypes = {
  name:ReactTypes.string,
  age:ReactTypes.number
}
export default Header
```

#### 类组件

##### 父向子组件传递数据

**和函数式组件相似，但是子组件参数的接收是在类的构造函数中**

**其他补充的内容在类中是用静态对象进行定义**

```html
<!-- 子组件 -->
import React from "react"
import "./main.css"
import ReactTypes from "prop-types"
class Main extends React.Component{
  constructor(props) {
    super();
    this.props = props
  }
  render() {
    console.log(this.props);
    return (
        <div className={"main"}>我是中欧</div>
    )
  }
  static defaultProps = {
    name:'lnj',
    age:18
  }
  static propTypes = {
    name:ReactTypes.string,
    age:ReactTypes.number
  }
}

export default Main

<!-- 父组件 -->
import React from "react";
// import "./App.css"
import Header from "./components/header"
import Footer from "./components/Footer"
import Main from "./components/main"
class App extends React.Component{
  render(){
    return(
        <div>
          <Header name={"lnj"} age={18}/>
          <Main name={'yh'} age={45}/>
          <Footer/>
        </div>
    )
  }
}
export default App
```

##### 利用super对props进行赋值

**React.Component里面含有对props赋值的函数，因此利用原型链，super给props进行赋值**

```javascript
import React from "react"
import "./main.css"
import ReactTypes from "prop-types"
class Main extends React.Component{
  constructor(props) {
    super(props);
    // this.props = props
    console.log(123)
  }
  render() {
    console.log(this.props);
    return (
        <div className={"main"}>我是中欧</div>
    )
  }
  static defaultProps = {
    name:'lnj',
    age:18
  }
  static propTypes = {
    name:ReactTypes.string,
    age:ReactTypes.number
  }
}

export default Main

```

### 子组件向父组件通信

**使用父组件的函数进行子向父组件传递数据**

```javascript
<!-- 父组件 -->
import React from "react";
// import "./App.css"
import Header from "./components/header"
import Footer from "./components/Footer"
import Main from "./components/main"
class App extends React.Component{
  render(){
    return(
        <div>
          <Header name={"lnj"} age={18}/>
          <Main name={'yh'} age={45}/>
           {/*传递函数给子组件*/}
          <Footer fatherFn={this.myFn.bind(this)}/>
        </div>
    )
  }
  myFn(a,b){
    console.log(a,b)
  }
}
export default App

<!-- 子组件:自己定义一个函数，在自定义函数中调用父函数进行数据传参 -->
import React from "react"
import "./footer.css"
class Footer extends React.Component{
  constructor(props) {
    super(props);
    console.log(123)
  }
  render(){
    return (
        <div>
          <div className={"footer"}>我是底2</div>
          <button onClick={() => {this.btnClick()}}></button>
        </div>
    )
  }
  btnClick(){
    this.props.fatherFn(123,456)
  }
}
export default Footer

```

### 跨组件通信

#### 方式一：爷爷孙子传递

**子向父组件通过不停地回调函数进行，父向子组件通过不断的props传递**

```javascript
class Son extends React.Component{
  constructor(props){
    super(props);
    console.log(123)
  }
  render(){
    return (
        <div>
          <p>我是儿子</p>
          <p>{this.props.name}</p>
          <button onClick={()=>{this.btnClick()}}>儿子按钮</button>
        </div>
    )
  }
  btnClick(){
    this.props.appFn(18);
  }
}
class Father extends React.Component{
  constructor(props){
    super(props);
    console.log(123)

  }
  render(){
    return (
        <div>
          <p>我是爸爸</p>
          <Son name={this.props.name} appFn={this.props.appFn}/>
        </div>
    )
  }
}
class App extends React.Component{
  render(){
    return (
        <div>
          <Father name={'lnj'} appFn={this.myFn.bind(this)}/>
        </div>
    )
  }
  myFn(age){
    console.log(age);
  }
}
```

#### 方式二：兄弟传递

**A传递给父，父传递给B**

```js
import React from 'react';

class A extends React.Component{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <div>
                <button onClick={()=>{this.btnClick()}}>A按钮</button>
            </div>
        )
    }
    btnClick(){
        this.props.appFn('lnj');
    }
}
class B extends React.Component{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <div>
                <p>{this.props.name}</p>
            </div>
        )
    }
}
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            name:''
        }
    }
    render(){
        return (
            <div>
                <A appFn={this.myFn.bind(this)}/>
                <B name={this.state.name}/>
            </div>
        )
    }
    myFn(name){
        // console.log(name);
        this.setState({
            name: name
        })
    }
}

export default App;

```

#### 方式三：context上下文传递

**Provider: 生产者容器组件, 专门用于负责生产数据
Consumer: 消费者容器组件, 专门用于消费生产者容器组件生产的数据的**

```js
import React from 'react';

/*
1.如果传递数据层次太深, 一层一层的传递比较麻烦, 所以React也提供了其它的解决方案
1.1 通过context上下文传递
1.2 通过Redux传递  (相当于Vuex)
1.3 通过Hooks传递  (相当牛X)
* */
/*
2.如何通过context上下文来传递数据
2.1调用创建上下文的方法, 只要我们调用了创建上下文的方法, 这个方法就会给我们返回两个容器组件
   生产者容器组件(Provider) / 消费者容器组件(Consumer)
2.2只要拿到了这两个容器组件, 那么我们就可以通过这两个容器组件从祖先传递数据给所有的后代了
2.3首先在祖先组件中利用 '生产者容器组件' 包裹后代组件
2.4然后在后代组件中利用 '消费者容器组件' 获取祖先组件传递过来的数据即可
* */

// 1.创建一个上下文对象
const AppContext = React.createContext({});
// 2.从上下文对象中获取容器组件
// Provider: 生产者容器组件, 专门用于负责生产数据
// Consumer: 消费者容器组件, 专门用于消费生产者容器组件生产的数据的
// 容器组件  : 专门用于包裹其它组件的组件, 我们就称之为容器组件
const {Provider, Consumer} = AppContext;
class Son extends React.Component{
    render(){
        return (
            <Consumer>
                {
                    (value)=>{
                        return (
                            <div>
                                <p>{value.name}</p>
                                <p>{value.age}</p>
                            </div>
                        )
                    }
                }
            </Consumer>
        )
    }
}
class Father extends React.Component{
    render(){
        return (
            <div>
                <Son></Son>
            </div>
        )
    }
}
class App extends React.Component{
    render(){
        return (
            /*我们可以在生产者容器组件中通过value来生产数据*/
            <Provider value={{name:'lnj', age:18}}>
                <Father></Father>
            </Provider>
        )
    }
}

export default App;

```

##### Context初始化的默认值

**如果在初始化Context的时候设置了默认值，那么该默认值可以不用生产者消费者标签，直接通过this.context来使用定义的对象的属性，但是必须用组件绑定对应的context**

```js

// 1.创建一个上下文对象
const AppContext = React.createContext({
    name:'知播渔',
    age: 666
});
class Son extends React.Component{
    render(){
        return (
            <div>
                {/*3.从当前组件的上下文中消费数据*/}
                <p>{this.context.name}</p>
                <p>{this.context.age}</p>
            </div>
        )
    }
}
// 2.指定当前组件的上下文
Son.contextType = AppContext;
```

##### 多个context对应多个生产者消费者

**感觉实际上没有什么使用场景，就是当一个context中有重复的属性名的时候可能会需要**

```js
import React from 'react';

const AppContext1 = React.createContext({});
const AppContext2 = React.createContext({});

class Son extends React.Component{
    render(){
        return (
            <AppContext1.Consumer>
                {
                    (value)=>{
                        return (
                            <AppContext2.Consumer>
                                {
                                    (value2)=>{
                                        return (
                                            <div>
                                                <p>{value.name}</p>
                                                <p>{value.age}</p>
                                                <p>{value2.gender}</p>
                                            </div>
                                        )
                                    }
                                }
                            </AppContext2.Consumer>
                        )
                    }
                }
            </AppContext1.Consumer>
        )
    }
}
// 注意: 如果有多个生产者, 那么不能通过这种方式来消费
// Son.contextType = AppContext1;
// Son.contextType = AppContext2;

class Father extends React.Component{
    render(){
        return (
            <div>
                <Son></Son>
            </div>
        )
    }
}
class App extends React.Component{
    render(){
        return (
            <AppContext1.Provider value={{name:'lnj', age:18}}>
                <AppContext2.Provider value={{gender:'man'}}>
                    <Father></Father>
                </AppContext2.Provider>
            </AppContext1.Provider>
        )
    }
}

export default App;
```

#### 方式四：event库

##### event方法的创建

**通过event库实现从下至上传递数据，更加方便**

```js
import React from 'react';
import {EventEmitter} from 'events';
/*
1.跨组件事件通讯
- 通过context我们已经能够实现跨组件通讯
  但是通过context我们只能实现从上往下传递
  不能实现从下往上传递或者同级之间传递
- 经过我们前面的学习我们知道, 子父组件之间通讯, 是通过回调函数的方式
  兄弟组件之间通讯, 也是通过父组件, 通过回调函数的方式
- 但是如果通过回调函数, 传统的方式我们需要一层一层的传递, 比较复杂
  所以我们可以借助一个第三方库(events)来实现跨组件事件通讯
*/
/*
2.如何使用events库实现跨组件通讯
2.1 安装events库, npm install events
2.2创建EventEmitter对象：eventBus对象；
2.3监听事件：eventBus.addListener("事件名称", 监听函数)；
2.4移除事件：eventBus.removeListener("事件名称", 监听函数)；
2.5发出事件：eventBus.emit("事件名称", 参数列表);
* */

// 1.在全局创建一个全局的事件管理器对象
const eventBus = new EventEmitter();

class A extends React.Component{
    // componentDidMount是React组件的一个生命周期方法
    // 这个方法不用我们手动调用, React会自动帮我们调用
    // 当前组件被渲染到界面上的时候, React就会自动调用
    componentDidMount() {
        eventBus.addListener('say', this.aFn.bind(this))
    }
    aFn(name, age){
        console.log(name, age);
    }
    render(){
        return (
            <div>A</div>
        )
    }
}
class B extends React.Component{
    render(){
        return (
            <div>
                <p>B</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    btnClick(){
        eventBus.emit('say', 'lnj', 18)
    }
}
class App extends React.Component{
    render(){
        return (
            <div>
                <A/>
                <B/>
            </div>
        )
    }
}

export default App;

```

##### event库方法的销毁

**在组件销毁的时候，调用销毁生命周期函数，必须在函数体内手动销毁定义的event事件，否则会影响性能**

```js
import React from 'react';
import {EventEmitter} from 'events';

// 1.在全局创建一个全局的事件管理器对象
const eventBus = new EventEmitter();

class Son extends React.Component{
    render(){
        return (
            <div>
                <p>son</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    btnClick(){
        eventBus.emit('say', 'lnj', 18);
    }
}
class Father extends React.Component{
    render(){
        return (
            <div>
                <Son/>
            </div>
        )
    }
}
class App extends React.Component{
    componentDidMount() {
        eventBus.addListener('say', this.appFn.bind(this));
    }
    // 注意点: 如果通过events来实现跨组件的通讯
    //        那么为了性能考虑, 应该在组件卸载的时候移除掉对应的事件]

    // componentWillUnmount也是React组件的一个生命周期方法
    // 这个生命周期方法我们不用手动调用, React会自动调用
    // 当前组件被卸载的时候, React就会自动调用
    componentWillUnmount() {
        eventBus.removeListener('say', this.appFn.bind(this));
    }

    appFn(name, age){
        console.log(name, age);
    }
    render(){
        return (
            <div>
                <Father/>
            </div>
        )
    }
}

export default App;

```

### props和state的区别

```html
1.props和state区别:
- props和state都是用来存储数据的
    - props存储的是父组件传递归来的数据
    - state存储的是自己的数据
    - props只读的
    - state可读可写
```

### setState的面试题

**1.setState是同步的还是异步的?
        默认情况下setState是异步的
2.为什么setState是异步的?
        主要是为了优化性能
3.如何拿到更新之后的数据?
        setState方法其实可以接收两个参数
        通过setState方法的第二个参数, 通过回调函数拿到
4.setState一定是异步的吗?
        不一定: 在定时器中, 在原生事件中**

```js
<!-- 通过回调函数拿到改变后的参数 -->
this.setState({
    age: 111
}, ()=>{
    console.log('回到函数中', this.state.age);
});
```

```js
import React from 'react';

/*
1.setState是同步的还是异步的?
- 在组件生命周期或React合成事件中，setState是异步；
- 在setTimeout或者原生dom事件中，setState是同步；
https://zh-hans.reactjs.org/docs/state-and-lifecycle.html
* */
class Home extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            age: 18, // 张三通过setState修改了age
            name: 'lnj', // 李四通过setState修改name
            gender: 'man' // 王五通过setState修改gender
        }
    }
    render(){
        console.log('渲染界面');
        return (
            <div>
                <p>{this.state.age}</p>
                {/*
                <button onClick={()=>{this.btnClick()}}>按钮</button>
                */}
                <button id={'btn'}>按钮</button>
            </div>
        )
    }
    componentDidMount() {
        const oBtn = document.getElementById('btn');
        oBtn.onclick = () => {
            this.setState({
                age: 666
            });
            console.log(this.state.age); // 666
        }
    }

    btnClick(){
        /*
        this.setState({
            age: 111
        }, ()=>{
            console.log('回到函数中', this.state.age);
        });
        // this.setState({
        //     age: 222
        // });
        // this.setState({
        //     age: 333
        // });
        console.log(this.state.age); // 18
         */
        setTimeout(()=>{
            this.setState({
                age: 666
            });
            console.log(this.state.age); // 666
        }, 0);
    }
}
class App extends React.Component{
    render(){
        return (
            <div>
                <Home/>
            </div>
        )
    }
}

export default App;

```

#### setState如何给state赋值？

**内部调用Object.assign方法将两个对象进行合并，然后相同属性名的数据直接用新值进行覆盖**

```js
import React from 'react';

/*
1.setState是如何给state赋值的?
- 通过Object.assign()
* */
class Home extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            name: 'lnj',
            age: 18
        }
        let oldObj = {name:'lnj', age:18};
        let newObj = {age: 666};
        /*
        {name:'lnj', age:666}
        * */
        let obj = Object.assign({}, oldObj, newObj);
        console.log(obj);
    }
    render(){
        return (
            <div>
                <p>{this.state.name}</p>
                <p>{this.state.age}</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    btnClick(){
        this.setState({
            age: 666
        });
    }
}
class App extends React.Component{
    render(){
        return (
            <div>
                <Home/>
            </div>
        )
    }
}

export default App;

```

#### state的合并现象

```js
import React from 'react';

/*
1.State合并现象?
- 因为setState会收集一段时间内所有的修改再更新界面
  所以就出现了State合并现象
* */
class Home extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            age: 0
        };
    }
    render(){
        return (
            <div>
                <p>{this.state.age}</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    btnClick(){
        /*
        1.为什么最终的一个值是1, 不是3
        因为setState默认是一个异步的方法, 默认会收集一段时间内所有的更新, 然后再统一更新
        所以就导致了最终的一个值是1, 不是3
        * */
        this.setState({
            age: this.state.age + 1
        });
        // console.log(this.state.age); // 0
        this.setState({
            age: this.state.age + 1
        });
        this.setState({
            age: this.state.age + 1
        });
    }
}
class App extends React.Component{
    render(){
        return (
            <div>
                <Home/>
            </div>
        )
    }
}

export default App;

```

**合并现象的内部实现**

```js
let oldObj = {age: 0};
let stateList = [
    // {age: oldObj.age + 1},
    // {age: oldObj.age + 1},
    // {age: oldObj.age + 1},
    // {age: 0 + 1},
    // {age: 0 + 1},
    // {age: 0 + 1},
    {age: 1},
    {age: 1},
    {age: 1}
];
stateList.forEach((newObj)=>{
    // Object.assign({}, {age: 0}, {age: 1}); // {age: 1}
    // Object.assign({}, {age: 1}, {age: 1}); // {age: 1}
    // Object.assign({}, {age: 1}, {age: 1}); // {age: 1}
    oldObj = Object.assign({}, oldObj, newObj);
});
console.log(oldObj);
```

#### 解决合并现象(解决异步更新)

#####  方式一：回调地狱

```js
 this.setState({
     age: this.state.age + 1
 }, ()=>{
     this.setState({
         age: this.state.age + 1
     }, ()=>{
         this.setState({
             age: this.state.age + 1
         });
     });
 });
```

##### 方式二：通过setState传入函数参数

```js
//preState是上一次更新后的值
this.setState((preState, props)=>{
    return {age: preState.age + 1}
})
this.setState((preState, props)=>{
    return {age: preState.age + 1}
})
this.setState((preState, props)=>{
    return {age: preState.age + 1}
})
```

**其中的原理**

```js
/*
let oldObj = {age: 0};
let stateList = [
    // {age: oldObj.age + 1},
    // {age: oldObj.age + 1},
    // {age: oldObj.age + 1},
    // {age: 0 + 1},
    // {age: 0 + 1},
    // {age: 0 + 1},
    {age: 1},
    {age: 1},
    {age: 1}
];
stateList.forEach((newObj)=>{
    // Object.assign({}, {age: 0}, {age: 1}); // {age: 1}
    // Object.assign({}, {age: 1}, {age: 1}); // {age: 1}
    // Object.assign({}, {age: 1}, {age: 1}); // {age: 1}
    oldObj = Object.assign({}, oldObj, newObj);
});
console.log(oldObj);
 */
let oldObj = {age: 0};
let stateList = [
    (preState)=>{return {age: preState.age + 1}},
    (preState)=>{return {age: preState.age + 1}},
    (preState)=>{return {age: preState.age + 1}},
];
stateList.forEach((fn)=>{
    // {age: 1}
    // {agg: 2}
    // {agg: 3}
    let newObj =  fn(oldObj);
    // {age: 0} {age: 1} / {age: 1}
    // {age: 1} {age: 2} / {age: 2}
    // {age: 2} {age: 3} / {age: 3}
    oldObj = Object.assign({}, oldObj, newObj);
});
console.log(oldObj);
```

### 组件的生命周期

- **constructor函数: 组件被创建的时候, 就会调用**
- **render函数: 渲染组件的时候, 就会调用**
- **componentDidMount函数：组件已经挂载到DOM上时，就会回调**
- **componentDidUpdate函数：组件已经发生了更新时，就会回调**
- **componentWillUnmount函数：组件即将被移除时，就会回调**

![16开始生命周期](D:\webVideo\代码文件\21、react\任务026：03-组件-代码资料\03-组件\21-组件生命周期\16开始生命周期.png)

![16之前生命周期](D:\webVideo\代码文件\21、react\任务026：03-组件-代码资料\03-组件\21-组件生命周期\16之前生命周期.jpg)

```js
import React from 'react';

/*
1.什么是生命周期?
事物从生到死的过程, 我们称之为生命周期

2.什么是生命周期方法?
事物在从生到死过程中, 在特定时间节点调用的方法, 我们称之为生命周期方法

1.React组件生命方法?
组件从生到死的过程, 在在特定的时间节点调用的方法, 我们称之为组件的生命周期方法
https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

- constructor函数: 组件被创建的时候, 就会调用
- render函数: 渲染组件的时候, 就会调用
- componentDidMount函数：组件已经挂载到DOM上时，就会回调
- componentDidUpdate函数：组件已经发生了更新时，就会回调
- componentWillUnmount函数：组件即将被移除时，就会回调
* */
class Home extends React.Component{
    constructor(props){
        super(props);
        console.log('挂载时- 创建组件constructor');
        this.state = {
            count: 0
        }
    }
    render(){
        if(this.state.count===0){
            console.log('挂载时- 渲染组件render');
        }else{
            console.log('更新时- 渲染组件render');
        }

        return (
            <div>
                <p>Home</p>
                <p>{this.state.count}</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    btnClick(){
        this.setState({
            count: 1
        });
    }
    componentDidMount() {
        console.log('挂载时- 渲染完成componentDidMount');
    }
    componentDidUpdate(prevProps, prevState, snapshot) {
        console.log('更新时- 更新完成componentDidUpdate');
    }
    componentWillUnmount() {
        console.log('卸载时- 即将被卸载componentWillUnmount');
    }
}
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            flag : true
        }
    }
    render(){
        return (
            <div>
                {this.state.flag && <Home/>}
                <button onClick={()=>{this.btnClick()}}>父按钮</button>
            </div>
        )
    }
    btnClick(){
        this.setState({
            flag:false
        })
    }
}

export default App;

```

### 生命周期的作用

```js
import React from 'react';

class Home extends React.Component{
    constructor(props){
        super(props);
        console.log('挂载时- 创建组件constructor');
        /*
        1.constructor生命周期方法中做什么?
        - 通过 props接收父组件传递过来的数据
        - 通过 this.state 初始化内部的数据
        - 通过bind为事件绑定实例（this)
        this.myClick = this.btnClick.bind(this);
        * */
        this.state = {
            count: 0
        }
    }
    render(){
        if(this.state.count===0){
            console.log('挂载时- 渲染组件render');
        }else{
            console.log('更新时- 渲染组件render');
        }
        /*
        2.render生命周期方法中做什么?
        - 返回组件的网页结构
        * */
        return (
            <div>
                <p>Home</p>
                <p>{this.state.count}</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    btnClick(){
        this.setState({
            count: 1
        });
    }
    componentDidMount() {
        console.log('挂载时- 渲染完成componentDidMount');
        /*
         3.componentDidMount生命周期方法中做什么?
         - 依赖于DOM的操作可以在这里进行
         - 在此处发送网络请求就最好的地方（官方建议）
         - 可以在此处添加一些订阅（会在componentWillUnmount取消订阅）
        * */
    }
    componentDidUpdate(prevProps, prevState, snapshot) {
        console.log('更新时- 更新完成componentDidUpdate');
        /*
        4.componentDidUpdate生命周期方法中做什么?
        - 可以在此对更新之后的组件进行操作
        * */
    }
    componentWillUnmount() {
        console.log('卸载时- 即将被卸载componentWillUnmount');
        /*
        5.componentWillUnmount生命周期方法中做什么?
        - 在此方法中执行必要的清理操作
        - 例如，清除 timer，取消网络请求或清除
        - 在 componentDidMount() 中创建的订阅等
        * */
    }
}
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            flag : true
        }
    }
    render(){
        return (
            <div>
                {this.state.flag && <Home/>}
                <button onClick={()=>{this.btnClick()}}>父按钮</button>
            </div>
        )
    }
    btnClick(){
        this.setState({
            flag:false
        })
    }
}

export default App;

```

### 生命周期的其他方法

**getDerivedStateFromProps(props, state)：拿到props和state，将props映射到state中**

**shouldComponentUpdate(nextProps, nextState, nextContext):觉得是否需要更新**

**getSnapshotBeforeUpdate(prevProps, prevState)：拿到更新之前的数据**

```js
import React from 'react';

class Home extends React.Component{
    constructor(props){
        super(props);
        console.log('挂载时- 创建组件constructor');
        this.state = {
            count: 0,
            // gender: 'man'
        }
    }
    // 注意点: getDerivedStateFromProps只需要了解即可(用得非常非常的少)
    static getDerivedStateFromProps(props, state){
        console.log('挂载或更新时-映射数据getDerivedStateFromProps');
        console.log(props, state);
        // return {gender: 'man'};
        return props;
    }
    shouldComponentUpdate(nextProps, nextState, nextContext) {
        console.log('更新时-决定是否要更新UIshouldComponentUpdate');
        return true;
        // return false;
    }

    render(){
        if(this.state.count===0){
            console.log('挂载时- 渲染组件render');
        }else{
            console.log('更新时- 渲染组件render');
        }
        return (
            <div>
                <p>Home</p>
                <p>{this.state.count}</p>
                <p>{this.state.gender}</p>
                <p>{this.state.name}</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    getSnapshotBeforeUpdate(prevProps, prevState) {
        console.log('更新时-最后能获取到更新之前数据的地方getSnapshotBeforeUpdate');
        console.log(prevProps, prevState);
        return null;
    }
    btnClick(){
        this.setState({
            count: 1
        });
    }
}


export default App;

```

### 组件的渲染、更新：diff算法

```js
/*
1.React渲染流程
- 执行render方法
<div>
    <div><p>我是段落</p></div>
    <div><span>我是span</span></div>
</div>
- 将JSX转换成createElement
React.createElement("div", null,
    React.createElement("div", null,
        React.createElement("p", null, "我是段落")),
    React.createElement("div", null,
        React.createElement("span", null, "我是span"))
);
- 执行createElement创建虚拟DOM, 得到虚拟DOM树
{
 targetName: 'div',
 children:[
    {
     targetName: 'div',
     children:[
        {
         targetName: 'p'
        }
     ]
    },
    {
     targetName: 'div',
     children:[
        {
         targetName: 'span'
        }
     ]
    }
 ]
}
- 根据虚拟DOM树在界面上生成真实DOM
*/

/*
2.React更新流程
- props/state发生改变
- render方法重新执行
- 将JSX转换成createElement
- 利用createElement重新生成新的虚拟DOM树
- 新旧虚拟DOM通过'diff算法'进行比较
- 每发现一个不同就生成一个mutation
- 根据mutation更新真实DOM

3.React-Diff算法
- 只会比较同层元素
- 只会比较同位置元素(默认)
- 在比较过程中:
    + 同类型元素做修改
    + 不同类型元素重新创建
https://zh-hans.reactjs.org/docs/reconciliation.html#the-diffing-algorithm
* */
```

### 列表的渲染优化

**类似于vue中的key，react也是通过key来标识**

```js
/*
1.列表渲染优化
由于diff算法在比较的时候默认情况下只会进行同层同位置的比较
所以在渲染列表时可能会存在性能问题

2.如何让diff算法递归比较同层所有元素?
给列表元素添加key,告诉React除了和同层同位置比, 还需要和同层其它位置比
https://zh-hans.reactjs.org/docs/reconciliation.html#the-diffing-algorithm
* */

/*
1.key注意点:
如果列表中出现相同的key, 所以我们必须保证列表中key的取值唯一
* */
class Home extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            heroList : ['鲁班', '虞姬', '黄忠']
        }
    }

    render(){
        return (
            <div>
                <ul>{
                    this.state.heroList.map(name=>{
                        return <li key={name}>{name}</li>
                    })
                }</ul>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        )
    }
    btnClick(){
        this.setState({
            // heroList: [...this.state.heroList, '阿珂']
            heroList: ['阿珂', ...this.state.heroList]
        })
    }
}
```

### 组件的更新优化

#### 方式一（不推荐）:

**利用shouldComponentUpdate(nextProps, nextState, nextContext)判断当前组件是否发生了更新，如果没有就不重新渲染**

```js
import React from 'react';
/*
1.嵌套组件的render调用
默认情况下, 只要父组件render被调用, 那么所有后代组件的render也会被调用

2.当前存在的问题
如果我们只修改了父组件的数据, 并没有修改子组件的数据, 并且子组件中也没有用到父组件中的数据
那么子组件还是会重新渲染, 子组件的render方法还是会重新执行, 这样就带来了性能问题

https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate
* */
class Home extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            age : 18
        }
    }
    shouldComponentUpdate(nextProps, nextState, nextContext) {
        // return true;
        // return false;
        if(this.state.age !== nextState.age){
            return true;
        }
        return false;
    }

    render(){
        console.log('Home-render被调用');
        return (
            <div>
                <p>{this.state.age}</p>
            </div>
        )
    }
}
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            name : 'lnj'
        }
    }
    render(){
        console.log('App-render被调用');
        return (
            <div>
                <p>{this.state.name}</p>
                <button onClick={()=>{this.btnClick()}}>APP按钮</button>
                <Home/>
            </div>
        )
    }
    btnClick(){
        this.setState({
            name:'知播渔'
        })
    }
}
```

#### 方式二：

**继承PureComponent，内部已经实现了判断是否更新的函数，同时推荐以后所有的组件全部都继承于PureComponent**

```js
/*
1.shouldComponentUpdate存在的问题
所有需要优化子组件都需要实现这个方法, 但这个方法并没有技术含量

2.解决方法
让组件继承于PureComponent, 让React自动帮我们实现
* */
// class Home extends React.Component{
class Home extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            age : 18
        }
    }
    /*
    shouldComponentUpdate(nextProps, nextState, nextContext) {
        if(this.state.age !== nextState.age){
            return true;
        }
        return false;
    }
    */
    render(){
        console.log('Home-render被调用');
        return (
            <div>
                <p>{this.state.age}</p>
            </div>
        )
    }
}
```

#### 函数式组件的优化

**通过React.memo()函数**

```js
/*
对于函数式组件来说:
- 没有继承关系
- 没有生命周期方法
* */
/*
1.函数组件的性能优化
对于类组件, 我们可以通过实现shouldComponentUpdate方法, 或者继承于PureComponent
来解决性能优化问题, 但是对于函数式组件, 是没有生命周期的, 是没有继承关系的
那么在函数式组件中如何解决性能优化问题呢?

2.通过React.memo()高阶函数
React.memo()会返回一个优化后的组件给我们
* */
const PurHome = React.memo(function() {
    console.log('Home-render被调用');
    return (
        <div>
            <p>Home</p>
        </div>
    )
});
```

#### 注意点

**反正记住修改数据必须使用setState即可**

```js
import React from 'react';
/*
1.state注意点
永远不要直接修改state中的数据
如果要修改state中的数据, 必须通过setState传递一个新的值
 */
class App extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            age : 0
        }
    }
    render(){
        console.log('App-render被调用');
        return (
            <div>
                <p>{this.state.age}</p>
                <button onClick={()=>{this.btnClick()}}>APP按钮</button>
            </div>
        )
    }
    /*
    shouldComponentUpdate(nextProps, nextState, nextContext) {
        console.log(this.state.age, nextState.age); // 0 1 //1 1
        if(this.state.age !== nextState.age){
            return true;
        }
        return false;
    }
    */
    btnClick(){
        // 以下两种写法区别:
        // 上面一种方式是设置了一个新的对象
        // 下面这种方式是设置了以前的对象

        // this.setState({
        //     age:this.state.age + 1
        // })
        this.state.age = this.state.age + 1;
        this.setState(this.state);
    }
}

export default App;

```

### 获取dom元素的四种方式

**1.js原生方法**

**2.ref = {‘字符串’}**

**3.ref = 自定义对象**

**4.ref = () => {}回调函数中赋值**

```js
import React from 'react';
/*
1.React中获取元素的方式
- 字符串
- 对象
- 回调函数
https://zh-hans.reactjs.org/docs/refs-and-the-dom.html#gatsby-focus-wrapper
* */
class App extends React.PureComponent{
    constructor(props){
        super(props);
        // this.oPRef = React.createRef();
        this.oPRef = null;
    }
    render(){
        console.log('App-render被调用');
        return (
            <div>
                {/*
                <p id={'box'}>我是段落</p>
                */}
                {/*
                <p ref={'box'}>我是段落</p>
                */}
                {/*
                <p ref={this.oPRef}>我是段落</p>
                */}
                <p ref={(arg)=>{this.oPRef = arg}}>我是段落</p>
                <button onClick={()=>{this.btnClick()}}>APP按钮</button>
            </div>
        )
    }
    btnClick(){
        // 第一种获取方式: 传统方式(在React中及其不推荐)
        // let oP = document.getElementById('box');
        // 第二种获取方式: 通过ref='字符串' / this.refs.字符串 (通过字符串的方式即将被废弃, 也不推荐)
        // let oP = this.refs.box;
        // 第三种获取方式: 通过createRef()创建一个对象, 然后将这个对象传递给ref (推荐)
        // let oP = this.oPRef.current;
        // 第四种获取方式: 通过传递一个回调函数, 然后保存回调函数参数的方式(推荐)
        let oP = this.oPRef;
        oP.innerHTML = 'www.it666.com';
        console.log(oP);
    }
}

export default App;

```

#### 通过ref获取元素注意点

**如果获取的是原生的元素, 那么拿到的就是元素本身
如果获取的是类组件元素, 那么拿到的就是类组件的实例对象
如果获取的是函数组件元素, 那么什么都拿不到**

```js
// 如果获取的是原生的元素, 那么拿到的就是元素本身
// 如果获取的是类组件元素, 那么拿到的就是类组件的实例对象
// 如果获取的是函数组件元素, 那么什么都拿不到
```

#### 获取函数式组件内部的元素

**在创建组件的时候用React.forwardRef()创建**

```js
import React from 'react';
/*
1.如何获取函数式组件中的元素
如果要获取的不是函数式组件本身,
而是想获取函数式组件中的某个元素
那么我们可以使用Ref转发来获取
* */
const About = React.forwardRef(function(props, myRef) { // myRef === this.myRef
    return (
        <div>
            <p>我是段落</p>
            <span ref={myRef}>我是span</span>
        </div>
    )
});
class App extends React.PureComponent{
    constructor(props){
        super(props);
        this.myRef = React.createRef();
    }
    render(){
        return (
            <div>
                <About ref={this.myRef}/>
                <button onClick={()=>{this.btnClick()}}>APP按钮</button>
            </div>
        )
    }
    btnClick(){
        console.log(this.myRef.current);
    }
}

export default App;

```

### 受控组件

**一些dom的内容是用户控制的，并实时更新的**

**指的是state的数据来源是state，去向也是state**

```js
import React from 'react';
/*
1.受控组件:
值受到react控制的表单元素
https://zh-hans.reactjs.org/docs/forms.html#controlled-components
* */
class App extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            name: 'lnj'
        }
    }
    render(){
        return (
            <form>
                <input type="text"
                       value={this.state.name}
                        onChange={(e)=>{this.change(e)}}/>
            </form>
        )
    }
    change(e){
        console.log(e.target.value);
        this.setState({
            name: e.target.value
        })
    }
}

export default App;

```

**当有过个input（受控组件时），使用name属性**

```js
import React from 'react';
/*
1.受控组件:
值受到react控制的表单元素
https://zh-hans.reactjs.org/docs/forms.html#controlled-components
* */
class App extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            name: 'lnj',
            email: '97606813@qq.com',
            phone: '13554499311'
        }
    }
    render(){
        return (
            <form>
                <input type="text"
                       value={this.state.name}
                       name={'name'}
                       onChange={(e)=>{this.change(e)}}/>
                <input type="text"
                       name={'email'}
                       value={this.state.email}
                       onChange={(e)=>{this.change(e)}}/>
                <input type="text"
                       name={'phone'}
                       value={this.state.phone}
                       onChange={(e)=>{this.change(e)}}/>
            </form>
        )
    }
    change(e){
        console.log(e.target.name);
        this.setState({
            [e.target.name]: e.target.value
        })
    }
    /*
    nameChange(e){
        console.log(e.target.name);
        this.setState({
            name: e.target.value
        })
    }
    emailChange(e){
        console.log(e.target.name);
        this.setState({
            email: e.target.value
        })
    }
    phoneChange(e){
        console.log(e.target.name);
        this.setState({
            phone: e.target.value
        })
    }
     */
}

export default App;

```

### 非受控组件

**通过ref进行元素的获取，并获取内部的值**

```js
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.handleSubmit = this.handleSubmit.bind(this);
    this.input = React.createRef();
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.input.current.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" ref={this.input} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

### 高阶组件

**参数为组件，返回值为新组件的函数**

```js
import React from 'react';
/*
1.高阶组件(Higher-Order Components，简称为 HOC):
- 参数为组件，返回值为新组件的函数
https://zh-hans.reactjs.org/docs/higher-order-components.html#gatsby-focus-wrapper
* */
class Home extends React.PureComponent{
    render(){
        return (
            <div>Home</div>
        )
    }
}
function enhanceComponent(WrappedComponent) {
    class AdvComponent extends React.PureComponent{
        render() {
            return (
                <div>
                    <WrappedComponent/>
                </div>
            )
        }
    }
    return AdvComponent;
}
const AdvComponent = enhanceComponent(Home);

class App extends React.PureComponent{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <div>
                <AdvComponent/>
            </div>
        )
    }
}

export default App;

```

#### 高阶组件的应用场景

##### 场景一：对于父子对应的组件，可以集成为一个函数，去除冗余代码

```js
import React from 'react';
/*
1.高阶组件应用场景:
- 增强Props
https://zh-hans.reactjs.org/docs/higher-order-components.html#gatsby-focus-wrapper
* */
const UserContext = React.createContext({});
const {Provider, Consumer} = UserContext;
/*
class Father1 extends React.PureComponent{
    render() {
        return (
            <div>
                <Son1/>
            </div>
        )
    }
}
class Father2 extends React.PureComponent{
    render() {
        return (
            <div>
                <Son2/>
            </div>
        )
    }
}
class Son1 extends React.PureComponent{
    render() {
        return (
            <Consumer>{
                value =>{
                    return (
                        <div>
                            <p>{value.name}</p>
                            <p>{value.age}</p>
                        </div>
                    )
                }
            }</Consumer>
        )
    }
}
class Son2 extends React.PureComponent{
    render() {
        return (
            <Consumer>{
                value =>{
                    return (
                        <ul>
                            <li>{value.name}</li>
                            <li>{value.age}</li>
                        </ul>
                    )
                }
            }</Consumer>
        )
    }
}
 */
class Son1 extends React.PureComponent{
    render() {
        return (
            <div>
                <p>{this.props.name}</p>
                <p>{this.props.age}</p>
            </div>
        )
    }
}
class Son2 extends React.PureComponent{
    render() {
        return (
            <ul>
                <li>{this.props.name}</li>
                <li>{this.props.age}</li>
            </ul>
        )
    }
}
function EnhancedComponent (WrappedComponent) {
    class Father extends React.PureComponent{
        render() {
            return (
                <Consumer>{
                    value =>{
                        return (
                            <WrappedComponent name={value.name} age={value.age}/>
                        )
                    }
                }</Consumer>
            )
        }
    }
    return Father;
}
const Father1 = EnhancedComponent(Son1);
const Father2 = EnhancedComponent(Son2);

class App extends React.PureComponent{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <Provider value={{name:'zs', age:18}}>
                <Father1/>
                <Father2/>
            </Provider>
        )
    }
}

export default App;

```

##### 场景二：增强props，可以在高阶组件使用不同的数据

**利用解构赋值简化代码<WrappedComponent {...value} {...this.props}/>**

**传不同的值，可以使用this.props进行接收，因此可以使用不同的值**

```js
import React from 'react';
/*
1.高阶组件应用场景:
- 增强Props
https://zh-hans.reactjs.org/docs/higher-order-components.html#gatsby-focus-wrapper
* */
const UserContext = React.createContext({});
const {Provider, Consumer} = UserContext;
class Son1 extends React.PureComponent{
    render() {
        return (
            <div>
                <p>{this.props.name}</p>
                <p>{this.props.age}</p>
                <p>{this.props.country}</p>
            </div>
        )
    }
}
class Son2 extends React.PureComponent{
    render() {
        return (
            <ul>
                <li>{this.props.name}</li>
                <li>{this.props.age}</li>
                <li>{this.props.country}</li>
            </ul>
        )
    }
}
function EnhancedComponent (WrappedComponent) {
    class Father extends React.PureComponent{
        render() {
            return (
                <Consumer>{
                    value =>{
                        return (
                            /*
                            <WrappedComponent name={value.name} age={value.age}/>
                            */
                            <WrappedComponent {...value} {...this.props}/>
                        )
                    }
                }</Consumer>
            )
        }
    }
    return Father;
}
const Father1 = EnhancedComponent(Son1);
const Father2 = EnhancedComponent(Son2);

class App extends React.PureComponent{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <Provider value={{name:'zs', age:18}}>
                <Father1 country={'中国'}/>
                <Father2 country={'俄罗斯'}/>
            </Provider>
        )
    }
}

export default App;

```

##### 场景三：抽离state，拦截生命周期函数

**当父组件传递给子组件的时候，多个子组件发生改变的时候，可以把子组件的state换为子组件的props**

```js
import React from 'react';
import {EventEmitter} from 'events';
/*
1.高阶组件应用场景:
- 代码复用/增强Props/抽离State/生命周期拦截
https://zh-hans.reactjs.org/docs/higher-order-components.html#gatsby-focus-wrapper
* */
const UserContext = React.createContext({});
const {Provider, Consumer} = UserContext;
const eventBus = new EventEmitter();
class Son1 extends React.PureComponent{
    /*
    constructor(props){
        super(props);
        this.state = {
            list: []
        }
    }
    componentDidMount() {
        eventBus.addListener('update', this.update.bind(this));
    }
    componentWillUnmount() {
        eventBus.removeListener('update', this.update.bind(this));
    }
    update(list){
        this.setState({
            list: list
        })
    }
     */

    render() {
        return (
            <div>
                <p>{this.props.name}</p>
                <p>{this.props.age}</p>
                <p>{this.props.country}</p>
                {
                    this.props.list.map(name=>{
                        return <p key={name}>{name}</p>
                    })
                }
            </div>
        )
    }
}
class Son2 extends React.PureComponent{
    /*
    constructor(props){
        super(props);
        this.state = {
            list: []
        }
    }
    componentDidMount() {
        eventBus.addListener('update', this.update.bind(this));
    }
    componentWillUnmount() {
        eventBus.removeListener('update', this.update.bind(this));
    }
    update(list){
        this.setState({
            list: list
        })
    }
     */
    render() {
        return (
            <ul>
                <li>{this.props.name}</li>
                <li>{this.props.age}</li>
                <li>{this.props.country}</li>
                {
                    this.props.list.map(name=>{
                        return <li key={name}>{name}</li>
                    })
                }
            </ul>
        )
    }
}
function EnhancedComponent (WrappedComponent) {
    class Father extends React.PureComponent{
        constructor(props){
            super(props);
            this.state = {
                list: []
            }
        }
        componentDidMount() {
            eventBus.addListener('update', this.update.bind(this));
        }
        componentWillUnmount() {
            eventBus.removeListener('update', this.update.bind(this));
        }
        update(list){
            this.setState({
                list: list
            })
        }
        render() {
            return (
                <Consumer>{
                    value =>{
                        return (
                            <WrappedComponent {...value} {...this.props} {...this.state}/>
                        )
                    }
                }</Consumer>
            )
        }
    }
    return Father;
}
const Father1 = EnhancedComponent(Son1);
const Father2 = EnhancedComponent(Son2);

class App extends React.PureComponent{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <Provider value={{name:'zs', age:18}}>
                <Father1 country={'中国'}/>
                <Father2 country={'俄罗斯'}/>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </Provider>
        )
    }
    btnClick(){
        eventBus.emit('update', ['鲁班', '虞姬', '黄忠']);
    }
}

export default App;

```

##### 场景四：权限控制

```js
import React from 'react';
/*
1.高阶组件应用场景:
- 代码复用/增强Props/抽离State/生命周期拦截
- 权限控制
https://zh-hans.reactjs.org/docs/higher-order-components.html#gatsby-focus-wrapper
* */
class Info extends React.PureComponent{
    render() {
        return (
            <div>用户信息</div>
        )
    }
}
class Login extends React.PureComponent{
    render() {
        return (
            <div>用户登录</div>
        )
    }
}
function EnhancedComponent (WrappedComponent) {
    class Authority extends React.PureComponent{
        render() {
            if(this.props.isLogin){
                return <Info/>
            }else{
                return <Login/>
            }
        }
    }
    return Authority;
}
const AuthorityInfo = EnhancedComponent(Info);

class App extends React.PureComponent{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <AuthorityInfo isLogin={true}/>
        )
    }
}

```

### Protals

**默认情况下, 所以的组件都是渲染到root元素中的Portal提供了一种将组件渲染到其它元素中的能力**

**比如一些公共的样式组件，可以渲染到其他的地方**

```js
import React from 'react';
import ReactDOM from 'react-dom';
/*
1.Portals:
- 默认情况下, 所以的组件都是渲染到root元素中的
  Portal提供了一种将组件渲染到其它元素中的能力
https://zh-hans.reactjs.org/docs/portals.html
* */
class Modal extends React.PureComponent{
    render() {
        /*
        this.props.children: 可以获取到当前组件所有的子元素或者子组件
        createPortal: 接收两个参数
        第一个参数: 需要渲染的内容
        第二个参数: 渲染到什么地方
        * */
        return ReactDOM.createPortal(this.props.children, document.getElementById('other'));
    }
}
class App extends React.PureComponent{
    constructor(props){
        super(props);
    }
    render(){
        return (
            <div id={'app'}>
                <Modal>
                    <div id={'modal'}>Modal</div>
                </Modal>
            </div>
        )
    }
}

export default App;
```

### Fragment

**产生不可见的dom元素，可以减少不必要的嵌套**

<**React.Fragment**>
</**React.Fragment**>

**或者**

<></>

```js
import React from 'react';
/*
1.Fragment:
- 由于React规定, 组件中只能有一个根元素
  所以每次编写组件的时候, 我们都需要在最外层包裹一个冗余的标签
- 如果不想渲染这个冗余的标签, 那么就可以使用Fragment来代替
https://zh-hans.reactjs.org/docs/fragments.html
* */
class Home extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            heroList: ['鲁班', '虞姬', '黄忠']
        }
    }
    render() {
        // 如果组件的结构比较复杂, 那么只能有一个根元素
        return (
            /*
            <React.Fragment>
                <p>{this.state.heroList[0]}</p>
                <p>{this.state.heroList[1]}</p>
                <p>{this.state.heroList[2]}</p>
            </React.Fragment>
            */
            // 如果需要给Fragment添加key, 那么就不能使用语法糖
            <>
                {
                    this.state.heroList.map(name=>{
                        return (
                            <React.Fragment key={name}>
                              <p >{name}</p>
                            </React.Fragment>
                        )
                    })
                }
            </>
        )
    }
}
class App extends React.PureComponent{
    constructor(props){
        super(props);
    }
    render(){
        return (
            /*
            <React.Fragment id={'app'}>
                <Home/>
            </React.Fragment>
             */
            // 下面的这种写法就是上面写法的语法糖
            <>
                <Home/>
            </>
        )
    }
}

export default App;

```

### strictMode(了解)

**严格模式：用于检查当前的程序是否使用了一些过时的方法**

```js
import ReactDOM from 'react-dom';
import React from 'react';
import App from './App';
/*
1.什么是StrictMode?
作用: 开启严格模式, 检查后代组件中是否存在潜在问题
注意点:
- 和Fragment一样, 不会渲染出任何UI元素
- 仅在'开发模式'下有效

2.StrictMode检查什么?
- 检查过时或废弃的属性/方法/...
- 检查意外的副作用
 + 这个组件的constructor会被调用两次
 + 检查这里写的一些逻辑代码被调用多次时，是否会产生一些副作用

https://zh-hans.reactjs.org/docs/strict-mode.html#gatsby-focus-wrapper
* */

ReactDOM.render(
    <React.StrictMode>
        <App/>
    </React.StrictMode>
    , document.getElementById('root'));

```

## 样式

### 内联样式

```js
import React from 'react';
import './App.css'
/*
1.React中的样式
React并没有像Vue那样提供特定的区域给我们编写CSS代码
所以你会发现在React代码中, CSS样式的写法千奇百怪

2.内联样式
- 内联样式的优点:
    + 内联样式, 样式之间不会有冲突
    + 可以动态获取当前state中的状态
- 内联样式的缺点：
    + 写法上都需要使用驼峰标识
    + 某些样式没有提示
    + 大量的样式, 代码混乱
    + 某些样式无法编写(比如伪类/伪元素)
* */
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            color: 'red'
        }
    }
    render(){
        return (
            <div>
                <p style={{fontSize:'50px', color: this.state.color}}>我是段落1</p>
                <p style={{fontSize:'50px', color:'green'}}>我是段落2</p>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        );
    }
    btnClick(){
        this.setState({
            color: 'blue'
        })
    }
}
export default App;
```

### 外链样式

**外联的css文件是一个全局的文件**

```js
import React from 'react';
import './App.css'
import Home from './Component/Home'
import About from './Component/About'
/*
1.外链样式
将CSS代码写到一个单独的CSS文件中, 在使用的时候导入进来
- 外链样式的优点:
    + 编写简单, 有代码提示, 支持所有CSS语法
- 外链样式的缺点：
    + 不可以动态获取当前state中的状态
    + 属于全局的css，样式之间会相互影响
* */
class App extends React.Component{
    render(){
        return (
            <div>
                <Home></Home>
                <About></About>
            </div>
        );
    }
}
export default App;
```

![img](file:///C:\Users\86157\AppData\Roaming\Tencent\Users\1325116124\QQ\WinTemp\RichOle\SQL2Q`3NUT35WMM}}[2[GJG.png)

**上述虽然每个组件对应一个样式，但是链接之后仍然是全局的，因此会有冲突**

### css模块化

```js
import React from 'react';
import './App.css'
import Home from './Component/Home'
import About from './Component/About'
/*
1.Css Module
- React的脚手架已经内置了css modules的配置：
    + .css/.less/.scss 等样式文件都修改成 .module.css/.module.less/.module.scss 等；
- Css Modules优点
    + 编写简单, 有代码提示, 支持所有CSS语法
    + 解决了全局样式相互污染问题
- Css Modules缺点
    + 不可以动态获取当前state中的状态
* */
class App extends React.Component{
    render(){
        return (
            <div>
                <Home></Home>
                <About></About>
            </div>
        );
    }
}
export default App;

<!-- import一个css模块，通过.属性名的方法赋值给类名 -->
import React from 'react';
import AboutStyle from './About.module.css';

class Home extends React.Component{
    render() {
        return (
            <div>
                <p className={AboutStyle.title}>我是home段落</p>
                <a href={'www.it666.com'} className={AboutStyle.link}>我是home超链接</a>
            </div>
        )
    }
}
export default Home;

```

### 字符串模板的补充知识

```js
const name = 'lnj';
const age = 18;
/*
const str = `my name is ${name}, my age is ${age}`;
console.log(str); // my name is lnj, my age is 18
 */

function test(...args) {
    console.log(args);
}
// 在JS中除了可以通过()来调用函数以外,
// 其实我们还可以通过模板字符串来调用函数
// test(1, 3, 5); // [ 1, 3, 5 ]
// test`1, 3, 5`; // [ [ '1, 3, 5' ] ]
// 通过模板字符串调用函数规律
// 参数列表中的第一个参数是一个数组, 这个数组中保存了所有不是插入的值
// 参数列表的第二个参数开始, 保存的就是所有插入的值
// 总结结论
// 我们可以拿到模板字符串中所有的内容
// 我们可以拿到模板字符串中所有非插入的内容
// 我们可以拿到模板字符串中所有插入的内容
// 所以我们就可以对模板字符串中所有的内容进行单独的处理
test`1, 3, 5, ${name}, ${age}`; // [ [ '1, 3, 5, ', ', ', '' ], 'lnj', 18 ]
```

### CSS in JS(推荐)

```js
import React from 'react';
import './App.css'
import Home from './Component/Home'
import About from './Component/About'
/*
1.CSS-in-JS
- 在React中, React认为结构和逻辑是密不可分的,所以在React中结构代码也是通过JS来编写的
  正是受到React这种思想的影响, 所以就有很多人开发了用JS来编写CSS的库
    + styled-components / emotion
- 利用JS来编写CSS, 可以让CSS具备样式嵌套、函数定义、逻辑复用、动态修改状态等特性
    + 也就是说, 从某种层面上, 提供了比过去Less/Scss更为强大的功能
    + 所以Css-in-JS, 在React中也是一种比较推荐的方式

2.styled-components使用
- 安装styled-components
  npm install styled-components --save
- 导入styled-components
  import styled from 'styled-components';
- 利用styled-components创建组件并设置样式
    styled.div`
      xxx:xxx
    `
* */
class App extends React.Component{
    render(){
        return (
            <div>
                <Home></Home>
                <About></About>
            </div>
        );
    }
}
export default App;

<!-- 具体的用法 -->
import React from 'react';
import styled from 'styled-components';

// 注意点:
// 默认情况下在webstorm中编写styled-components的代码是没有任何的代码提示的
// 如果想有代码提示, 那么必须给webstorm安装一个插件
// 用字符串模板来进行定义
const StyleDiv = styled.div`
    p{
        font-size: 50px;
        color: red;
    }
    a{
       font-size: 25px;
       color: green;
    }
`;

class Home extends React.Component{
    render() {
        return (
            <StyleDiv>
                <p>我是home段落</p>
                <a href={'www.it666.com'}>我是home超链接</a>
            </StyleDiv>
        )
    }
}
export default Home;

```

### CSS in JS的特性

#### 动态修改状态：props和attrs的使用

**props可以应在css中，attrs可以在创建styled-component时设置dom的一些属性**

```js
import React from 'react';
import styled from 'styled-components';
/*
1.styled-components特性
- props
- attrs
* */
const StyleDiv = styled.div`
    p{
        font-size: 50px;
        color: ${props => props.color};
    }
    a{
       font-size: 25px;
       color: green;
    }
`;
// 注意点:
// 调用完attrs方法之后, 这个方法返回的还是一个函数
// 所以我们还可以继续通过字符串模板来调用
const StyleInput = styled.input.attrs({
    type:'password'
})``

class Home extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            color: 'red'
        }
    }
    render() {
        return (
            <StyleDiv color={this.state.color}>
                <p>我是home段落</p>
                <a href={'www.it666.com'}>我是home超链接</a>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
                {/*
                <StyleInput type={'password'}></StyleInput>
                */}
                <StyleInput></StyleInput>
            </StyleDiv>
        )
    }
    btnClick(){
        this.setState({
            color: 'green'
        })
    }
}
export default Home;

```

#### 设置主题样式：公共样式

```js
import React from 'react';
import './App.css'
import Home from './Component/Home'
import About from './Component/About'
import {ThemeProvider} from 'styled-components'

/*
1.设置主题
通过styled-components中的ThemeProvider组件
设置要传递的数据即可，用法和Provider相似
* */
class App extends React.Component{
    render(){
        return (
            <ThemeProvider theme={{size:'50px', color:'blue'}}>
                <Home></Home>
                <About></About>
            </ThemeProvider>
        );
    }
}
export default App;

<!-- 子组件中用props接收 -->
import React from 'react';
import styled from 'styled-components';

const StyleDiv = styled.div`
    p{
        font-size: ${props=>props.theme.size};
        color: ${props=>props.theme.color};
    }
`;
class About extends React.Component{
    render() {
        return (
            <StyleDiv>
                <p>我是about段落</p>
            </StyleDiv>
        )
    }
}
export default About;

```

#### 继承：减少冗余代码

```js
import React from 'react';
import './App.css'
import styled from 'styled-components'

/*
1.继承
https://styled-components.com/docs
* */
/*
const StyleDiv1 = styled.div`
  font-size: 100px;
  color: red;
  background: blue;
`;
const StyleDiv2 = styled.div`
  font-size: 100px;
  color: green;
  background: blue;
`;
 */
const BaseDiv = styled.div`
  font-size: 100px;
  background: blue;
`;
const StyleDiv1 = styled(BaseDiv)`
  color: red;
`;
const StyleDiv2 = styled(BaseDiv)`
  color: green;
`;
class App extends React.Component{
    render(){
        return (
            <div>
                <StyleDiv1>我是div1</StyleDiv1>
                <StyleDiv2>我是div2</StyleDiv2>
            </div>
        );
    }
}
export default App;

```

## 动画

### 原生动画

```js
import React from 'react';
import './App.css'
import styled from 'styled-components';

/*
1.React过渡动画
- 在React中我们可以通过原生的CSS来实现过渡动画，
  但是React社区为我们提供了react-transition-group帮助我们快速过渡动画
*/
/*
2.动画组件：
- Transition
    + 该组件是一个和平台无关的组件（不一定要结合CSS）；
    + 在前端开发中，我们一般是结合CSS来完成样式，所以比较常用的是CSSTransition；
- CSSTransition
    + 在前端开发中，通常使用CSSTransition来完成过渡动画效果
- SwitchTransition
    + 两个组件显示和隐藏切换时，使用该组件
- TransitionGroup
    + 将多个动画组件包裹在其中，一般用于列表中元素的动画；
* */

const StyleDiv  = styled.div`
  width: ${props => props.width};
  height: ${props => props.height};
  background: skyblue;
  opacity: ${props => props.opacity};
  transition: all 3s;
`
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            width: 0,
            height: 0,
            opacity:0
        }
    }
    render(){
        return (
            <div>
                <StyleDiv {...this.state}></StyleDiv>
                <button onClick={()=>{this.btnClick()}}>按钮</button>
            </div>
        );
    }
    btnClick(){
        this.setState({
            width: '100px',
            height: '100px',
            opacity:1
        })
    }
}
export default App;

```

### 动画组件

```js
import React from 'react';
import './App.css'
import {CSSTransition} from 'react-transition-group';
/*
1.React过渡动画
- 在React中我们可以通过原生的CSS来实现过渡动画，
  但是React社区为我们提供了react-transition-group帮助我们快速过渡动画
*/
/*
2.动画组件：
- Transition容器组件
    + 该组件是一个和平台无关的组件（不一定要结合CSS）；
    + 在前端开发中，我们一般是结合CSS来完成过渡，所以比较常用的是CSSTransition；
- CSSTransition容器组件
    + 在前端开发中，通常使用CSSTransition来完成过渡动画效果
- SwitchTransition容器组件
    + 两个组件显示和隐藏切换时，使用该组件
- TransitionGroup容器组件
    + 将多个动画组件包裹在其中，一般用于列表中元素的动画；
* */
/*
3.CSSTransition状态
- CSSTransition有三个状态：
    + appear: 初始
    + enter : 进入
    + exit；: 退出
- 当组件第一次加载时候会自动查找
    -appear / -appear-active / -appear-done
- 当组件显示时会自动查找
    -enter / -enter-active / -enter-done
- 当组件退出时会自动查找
    -exit / -exit-active / -exit-done
* */

/*
1.如何通过CSSTransition来实现过渡效果?
1.1安装react-transition-group
npm install react-transition-group --save
1.2从安装好的库中导入CSSTransition
import {CSSTransition} from 'react-transition-group';
1.3利用CSSTransition将需要执行过渡效果的组件或元素包裹起来
1.4编写对应的CSS动画
实现: .-enter / .-enter-active / .-enter-done
1.5给CSSTransition添加一些属性
in属性        :
取值是一个布尔值, 如果取值为false表示触发退出动画, 如果取值是true表示触发进入动画
classNames属性:
指定动画类名的前缀
timeout属性   :
设置动画超时时间
* */
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            isShow: false
        }
    }
    render(){
        return (
            <div>
                <CSSTransition in={this.state.isShow}
                                classNames={'box'}
                                timeout={3000}>
                    <div></div>
                </CSSTransition>
                <button onClick={()=>{this.btnClick()}}>显示</button>
            </div>
        );
    }
    btnClick(){
        this.setState({
            isShow: true
        })
    }
}
export default App;

<!-- App.css -->
.box-enter{
    /*进入动画执行之前绑定的类名*/
    width: 0;
    height: 0;
    opacity: 0;
    background: skyblue;
}
.box-enter-active{
    /*进入动画执行过程中绑定的类名*/
    width: 100px;
    height: 100px;
    opacity: 1;
    transition: all 3s;
}
.box-enter-done{
    /*进入动画执行完毕之后绑定的类名*/
    width: 100px;
    height: 100px;
    opacity: 1;
    background: red;
}

```

**隐藏动画**

**添加三个样式，加上一个unmountOnExit属性即可**

```js
import React from 'react';
import './App.css'
import {CSSTransition} from 'react-transition-group';
/*
3.CSSTransition状态
- CSSTransition有三个状态：
    + appear: 初始
    + enter : 进入
    + exit；: 退出
- 当组件第一次加载时候会自动查找
    -appear / -appear-active / -appear-done
- 当组件显示时会自动查找
    -enter / -enter-active / -enter-done
- 当组件退出时会自动查找
    -exit / -exit-active / -exit-done
* */
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            isShow: false
        }
    }
    render(){
        return (
            /*unmountOnExit: 如果取值为true, 那么表示退出动画执行完毕之后删除对应的元素*/
            <div>
                <CSSTransition in={this.state.isShow}
                                classNames={'box'}
                                timeout={3000}
                                unmountOnExit={true}>
                    <div></div>
                </CSSTransition>
                <button onClick={()=>{this.btnClick1()}}>显示</button>
                <button onClick={()=>{this.btnClick2()}}>隐藏</button>
            </div>
        );
    }
    btnClick1(){
        this.setState({
            isShow: true
        })
    }
    btnClick2(){
        this.setState({
            isShow: false
        })
    }
}
export default App;

<!-- 组件隐藏的动画 -->
.box-exit{
    /*退出动画执行之前绑定的类名*/
    width: 100px;
    height: 100px;
    opacity: 1;
    background: red;
}
.box-exit-active{
    /*退出动画执行过程中绑定的类名*/
    width: 0;
    height: 0;
    opacity: 0;
    transition: all 3s;
}
.box-exit-done{
    /*退出动画执行完毕之后绑定的类名*/
    width: 0;
    height: 0;
    opacity: 0;
    background: skyblue;
}
```

**初始化动画**

**添加三个css，然后添加appear属性即可**

```js
import React from 'react';
import './App.css'
import {CSSTransition} from 'react-transition-group';
/*
3.CSSTransition状态
- CSSTransition有三个状态：
    + appear: 初始
    + enter : 进入
    + exit；: 退出
- 当组件第一次加载时候会自动查找
    -appear / -appear-active / -appear-done
- 当组件显示时会自动查找
    -enter / -enter-active / -enter-done
- 当组件退出时会自动查找
    -exit / -exit-active / -exit-done
* */
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            isShow: true
        }
    }
    render(){
        return (
            <div>
                <CSSTransition in={this.state.isShow}
                                classNames={'box'}
                                timeout={3000}
                                unmountOnExit={true}
                                appear>
                    <div className={'box'}></div>
                </CSSTransition>
                <button onClick={()=>{this.btnClick1()}}>显示</button>
                <button onClick={()=>{this.btnClick2()}}>隐藏</button>
            </div>
        );
    }
    btnClick1(){
        this.setState({
            isShow: true
        })
    }
    btnClick2(){
        this.setState({
            isShow: false
        })
    }
}
export default App;
<!-- App.css -->
.box{
    width: 100px;
    height: 100px;
    background: skyblue;
}

.box-appear{
    /*初始化动画执行之前绑定的类名*/
    width: 0;
    height: 0;
    opacity: 0;
    background: skyblue;
}
.box-appear-active{
    /*初始化动画执行过程中绑定的类名*/
    width: 100px;
    height: 100px;
    opacity: 1;
    transition: all 3s;
}
.box-appear-done{
    /*初始化动画执行完毕之后绑定的类名*/
    width: 100px;
    height: 100px;
    opacity: 1;
    background: red;
}
```

### 动画的回调函数

**onEnter执行前、onEntering执行中、onEntered执行完，exit同理**

```js
import React from 'react';
import './App.css'
import {CSSTransition} from 'react-transition-group';
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            isShow: true
        }
    }
    render(){
        return (
            <div>
                {/*
                生命周期方法
                onEnter/onEntering/onEntered
                onExit/onExiting/onExited
                */}
                <CSSTransition in={this.state.isShow}
                                classNames={'box'}
                                timeout={3000}
                                unmountOnExit={true}
                                appear
                                onEnter = {this.myFn}
                               onEntering={()=>{console.log('进入动画执行过程中');}}
                               onEntered={()=>{console.log('进入动画执行完毕');}}
                >
                    <div className={'box'}></div>
                </CSSTransition>
                <button onClick={()=>{this.btnClick1()}}>显示</button>
                <button onClick={()=>{this.btnClick2()}}>隐藏</button>
            </div>
        );
    }
    myFn(currentEl, isAppear){
        // console.log(currentEl, isAppear);
        console.log('进入动画开始之前');
    }
    btnClick1(){
        this.setState({
            isShow: true
        })
    }
    btnClick2(){
        this.setState({
            isShow: false
        })
    }
}
export default App;

```

### 切换动画

```js
import React from 'react';
import './App.css'
import {CSSTransition, SwitchTransition} from 'react-transition-group';
/*
1.SwitchTransition
- SwitchTransition可以完成组件切换的动画
- SwitchTransition组件里面要有CSSTransition或者Transition组件，不能直接包裹你想要切换的组件
- SwitchTransition里面的CSSTransition或Transition组件不再像以前那样接受in属性来判断元素是何种状态，
  取而代之的是key属性
* */
class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            isOn: true
        }
    }
    render(){
        return (
            <div>
                <SwitchTransition mode={'in-out'}>
                    <CSSTransition key={this.state.isOn}
                                   timeout={3000}
                                    classNames={'btn'}>
                        <button onClick={()=>{this.setState({isOn: !this.state.isOn})}}>{this.state.isOn ? 'on' : 'off'}</button>
                    </CSSTransition>
                </SwitchTransition>
            </div>
        );
    }
}
export default App;

<!-- App.css -->
.btn-enter{
    /*进入动画执行之前绑定的类名*/
    opacity: 0;
    transform: translateX(-100%);
}
.btn-enter-active{
    /*进入动画执行过程中绑定的类名*/
    opacity: 1;
    transform: translateX(0);
    transition: all 3s;
}
.btn-enter-done{
    /*进入动画执行完毕之后绑定的类名*/
}

.btn-exit{
    /*退出动画执行之前绑定的类名*/
    opacity: 1;
    transform: translateX(0);
}
.btn-exit-active{
    /*退出动画执行过程中绑定的类名*/
    opacity: 0;
    transform: translateX(100%);
    transition: all 3s;
}
.btn-exit-done{
    /*退出动画执行完毕之后绑定的类名*/
}

button{
    padding: 10px 20px;
    margin-left: 50%;
}

```

### 动画组组件

**TransitionGroup包住<li>元素**

```js
import React from "react";
import "./App.css"
// import {CSSTransition} from "react-transition-group";
import {CSSTransition,TransitionGroup} from "react-transition-group";
/*
1.TransitionGroup
-当我们有一组组件需要执行动画时，就需要将这些CSSTransition放入到一个TransitionGroup中来完成动画
-注意点和SwitchTransition一样
 */
class App extends React.Component{
  constructor(props) {
    super(props);
    this.state = {
      heroList:['鲁班','雨季','皇后']
    }
  }
  render(){
    return (
        <div>
          <ul>
            {/*
            注意点：无论是SwitchTransition还是TransitionGroup，在使用的时候CSSTransition都必须是他直接的子元素才可以
            */}
            <TransitionGroup>
              {
                this.state.heroList.map((name,index) => {
                  return (
                      <CSSTransition key={name}
                                     timeout={3000}
                                     classNames={'item'}>
                        <li onClick={() => {this.removeHero(index)}}>{name}</li>
                      </CSSTransition>
                  )
                })
              }
            </TransitionGroup>
          </ul>
          <button onClick={() => {this.setState({heroList:[...this.state.heroList,'金科']})}}>新增</button>
        </div>
    );
  }
  removeHero(index){
    const list = this.state.heroList.filter((name,idx) => {
      return index !== idx
    })
    this.setState({
      heroList:list
    })
  }
}
export default App;

<!-- App.css -->
.item-enter{
    /*进入动画执行之前绑定的类名*/
    opacity: 0;
    transform: translateX(100%);
}
.item-enter-active{
    /*进入动画执行过程中绑定的类名*/
    opacity: 1;
    transform: translateX(0);
    transition: all 3s;
}
.item-enter-done{
    /*进入动画执行完毕之后绑定的类名*/
}

.item-exit{
    /*退出动画执行之前绑定的类名*/
    opacity: 1;
    transform: translateX(0);
}
.item-exit-active{
    /*退出动画执行过程中绑定的类名*/
    opacity: 0;
    transform: translateX(100%);
    transition: all 3s;
}
.item-exit-done{
    /*退出动画执行完毕之后绑定的类名*/
}

button{
    padding: 10px 20px;
    margin-left: 50%;
}

```

### 注意点

**保证key是唯一的才可以**

```js
import React from 'react';
import './App.css'
import {CSSTransition, TransitionGroup} from 'react-transition-group';

class App extends React.Component{
    constructor(props){
        super(props);
        this.state = {
            // heroList : ['鲁班', '鲁班', '鲁班']
            heroList:[
                {id:1, name:'鲁班'},
                {id:2, name:'虞姬'},
                {id:3, name:'黄忠'},
            ]
        }
    }
    render(){
        return (
            <div>
                <ul>
                    <TransitionGroup>
                    {
                        /*
                        注意点:
                        在企业开发中一定要保证CSSTransition key的唯一性
                        * */
                        this.state.heroList.map((obj, index) =>{
                            return (
                                <CSSTransition key={obj.id}
                                               timeout={3000}
                                               classNames={'item'}>
                                    <li onClick={()=>{this.removeHero(index)}}>{obj.name}</li>
                                </CSSTransition>
                            )
                        })
                    }
                    </TransitionGroup>
                </ul>
                <button onClick={()=>{this.btnClick()}}>新增</button>
            </div>
        );
    }
    btnClick(){
        this.setState({
            // heroList: [...this.state.heroList, '阿珂']
            heroList: [...this.state.heroList, {id: this.state.heroList.length + 1, name:'阿珂'}]
        })
    }
    removeHero(index){
        // console.log(index);
        const list = this.state.heroList.filter((name, idx) =>{
            return idx !== index;
        })
        // console.log(list);
        this.setState({
            heroList: list
        })
    }
}
export default App;

```

## 路由

### 基本使用

**BrowserRouter：指定监听模式为history**

**HashRouter：指定监听模式为hash**

**Link：配置url地址**

**Route：通过Route匹配路由地址**

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import {BrowserRouter, HashRouter, Link, Route} from 'react-router-dom';

/*
1.什么是路由?
- 路由维护了URL地址和组件的映射关系, 通过这个映射关系,
  我们就可以根据不同的URL地址，去渲染不同的组件
*/
/*
2.如何在React中使用路由
- 安装react-router
npm install react-router-dom
- 通过指定监听模式
    + BrowserRouter history模式 http://www.it666.com/home
    + HashRouter hash模式 http://www.it666.com/#/home
- 通过Link修改路由URL地址
- 通过Route匹配路由地址

官网文档地址: https://reactrouter.com/web/guides/quick-start
* */
class App extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            flag: true
        }
    }
    render(){
        // 需求: 界面上有两个按钮, 点击不同按钮显示不同组件
        return (
            <div>
                {/*
                1.BrowserRouter和HashRouter作用:
                指定路由的监听模式 history模式 / hash模式
                http://www.it666.com
                http://www.it666.com/home     history模式
                http://www.it666.com/#/home   hash模式
                */}
                <HashRouter>
                    {/*
                    2.Link作用:
                    用于修改URL的资源地址
                    */}
                    <Link to={'/home'}>Home</Link>
                    <Link to={'/about'}>About</Link>
                    {/*
                    3.Route作用:
                    用于维护URL和组件的关系
                    Route是一个占用组件, 将来它会根据匹配到的资源地址渲染对应的组件
                    */}
                    <Route path={'/home'} component={Home}/>
                    <Route path={'/about'} component={About}/>
                </HashRouter>
                {/*
                <div>
                    <button onClick={()=>{this.btnClick(true)}}>Home</button>
                    <button onClick={()=>{this.btnClick(false)}}>About</button>
                </div>
                {this.state.flag ? <Home/> : <About/>}
                */}
            </div>
        )
    }
    btnClick(flag){
        this.setState({
            flag:flag
        })
    }
}

export default App;

```

### 注意点

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import {BrowserRouter, HashRouter, Link, Route} from 'react-router-dom';

/*
1.React路由注意点
- react-router4之前, 所有路由代码都是统一放到react-router中管理的
- react-router4开始, 拆分为了两个包react-router-dom和react-router-native
    + react-router-dom 在浏览器中使用路由
    + react-router-native 在原生应用中使用路由

- BrowserRouter history模式使用的是H5的特性, 所以兼容性会比HashRouter hash模式差一些
- 在企业开发中如果不需要兼容低级版本浏览器, 建议使用BrowserRouter
            如果需要兼容低级版本浏览器, 那么只能使用HashRouter

- 无论是Link还是Route都只能放到BrowserRouter和HashRouter中才有效
*/
class App extends React.PureComponent{
    render(){
        return (
            <div>
                {/*设置监听模式*/}
                <BrowserRouter>
                    {/*修改路由地址*/}
                    <Link to={'/home'}>Home</Link>
                    <Link to={'/about'}>About</Link>
                    {/*维护URL和组件关系*/}
                    <Route path={'/home'} component={Home}/>
                    <Route path={'/about'} component={About}/>
                </BrowserRouter>
            </div>
        )
    }
}

export default App;
```
**路由地址的匹配默认从上至下，匹配完所有的地址**

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import {BrowserRouter, Link, Route, NavLink} from 'react-router-dom';

/*
2.Route注意点
- 默认情况下Route在匹配资源地址时, 是模糊匹配
  如果必须和资源地址一模一样才匹配, 那么需要添加exact属性, 开启精准匹配
* */
/*
3.Link注意点
- 默认情况下Link会渲染成一个a标签, 如果想渲染成其他的元素, 可以通过手动路由跳转来实现
* */
/*
4.NavLink注意点
- 默认情况下NavLink在匹配资源地址时, 是模糊匹配
  如果必须和资源地址一模一样才匹配, 那么需要添加exact属性, 开启精准匹配
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                <BrowserRouter>
                    {/*
                    Link注意点:
                    默认情况下Link会渲染成一个a标签,
                    如果想渲染成其他的元素, 可以通过手动路由跳转来实现
                    */}
                    <Link to={'/home'}>Home</Link>
                    <Link to={'/home/about'}>About</Link>
                    {/*
                    NavLink注意点:
                    NavLink在匹配路由的时候, 是利用当前资源地址从左至右的和path中的地址进行匹配的
                    只要当前资源地址从左至右完整的包含了path中的地址那么就认为匹配
                    当前资源地址 : /home/about
                    to中的地址: /home
                    to中的地址: /home/about
                    */}
                    {/*
                    <NavLink exact to={'/home'} activeStyle={{color:'red'}}>Home</NavLink>
                    <NavLink exact to={'/home/about'} activeStyle={{color:'red'}}>About</NavLink>
                    */}
                    {/*
                    Route注意点:
                    Route在匹配路由的时候, 是利用当前资源地址从左至右的和path中的地址进行匹配的
                    只要当前资源地址从左至右完整的包含了path中的地址那么就认为匹配
                    当前资源地址 : /home/about
                    path中的地址: /home
                    path中的地址: /home/about
                    */}
                    <Route exact path={'/home'} component={Home}/>
                    <Route exact path={'/home/about'} component={About}/>
                </BrowserRouter>
            </div>
        )
    }
}

export default App;
```

**当设置Switch标签时，一旦有一个地址匹配了，就不会再继续往下匹配了**

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import {BrowserRouter, Link, Route, Switch} from 'react-router-dom';

/*
1.Switch:
默认情况下路由会从上至下匹配所有的Route, 只要匹配都会显示
但是在企业开发中大部分情况下, 我们希望的是一旦有一个匹配到了后续就不要再匹配了
此时我们就可以通过Switch来实现
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                <BrowserRouter>
                    <Link to={'/home'}>Home</Link>
                    <Link to={'/about'}>About</Link>

                    <Switch>
                        <Route exact path={'/home'} component={Home}/>
                        <Route exact path={'/about'} component={About}/>
                        {/*如果Route没有指定path, 那么表示这个Route和所有的资源地址都匹配*/}
                        <Route component={Other}/>
                    </Switch>
                </BrowserRouter>
            </div>
        )
    }
}

export default App;

```

### 路由重定向

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import User from './components/User'
import Login from './components/Login'
import {BrowserRouter, Link, Route, Switch} from 'react-router-dom';

/*
1.Redirect:
资源重定向, 也就是可以在访问某个资源地址的时候重定向到另外一个资源地址
例如: 访问/user 重定向到 /login
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                <BrowserRouter>
                    <Link to={'/home'}>Home</Link>
                    <Link to={'/about'}>About</Link>
                    <Link to={'/user'}>User</Link>

                    <Switch>
                        <Route exact path={'/home'} component={Home}/>
                        <Route exact path={'/about'} component={About}/>
                        <Route exact path={'/user'} component={User}/>
                        <Route exact path={'/login'} component={Login}/>
                        <Route component={Other}/>
                    </Switch>
                </BrowserRouter>
            </div>
        )
    }
}

export default App;

<!-- User.js -->
import React from 'react';
import {Redirect} from 'react-router-dom';

class User extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            isLogin: false
        }
    }
    render(){
        let user = (
            <div>
                <h1>用户界面</h1>
                <p>用户名: jonathan_lee</p>
                <p>密码: www.it666.com</p>
            </div>
        );
        let login = <Redirect to={'/login'}/>
        return this.state.isLogin ? user : login;
    }
}

export default User;

```

### 嵌套路由

**嵌套路由的一级组件里面继续定义嵌套的路由，但前缀必须是第一个路由的url，同时外层路由不能够使用精准匹配**

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import User from './components/User'
import Login from './components/Login'
import Discover from './components/Discover'
import {BrowserRouter, NavLink, Route, Switch} from 'react-router-dom';

/*
1.嵌套路由(子路由):
路由里面又有路由, 我们就称之为嵌套路由
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                <BrowserRouter>
                    <NavLink to={'/home'} activeStyle={{color:'red'}}>Home</NavLink>
                    <NavLink to={'/about'} activeStyle={{color:'red'}}>About</NavLink>
                    <NavLink to={'/user'} activeStyle={{color:'red'}}>User</NavLink>
                    <NavLink to={'/discover'} activeStyle={{color:'red'}}>广场</NavLink>

                    <Switch>
                        <Route exact path={'/home'} component={Home}/>
                        <Route exact path={'/about'} component={About}/>
                        <Route exact path={'/user'} component={User}/>
                        <Route exact path={'/login'} component={Login}/>
                        {/*
                        注意点:如果要使用嵌套路由, 那么外层路由不能添加精准匹配exact
                        /discover/toplist
                        /discover
                        */}
                        <Route path={'/discover'} component={Discover}/>
                        <Route component={Other}/>
                    </Switch>
                </BrowserRouter>
            </div>
        )
    }
}

export default App;

<!-- Discover -->
    import React from 'react';
import {NavLink, Switch, Route} from "react-router-dom";

function Hot() {
    return (
        <div>推荐</div>
    )
}
function TopList() {
    return (
        <div>排行榜</div>
    )
}
function PlayList() {
    return (
        <div>歌单</div>
    )
}
class Discover extends React.PureComponent{
    render(){
        return (
            /*
            注意点: 由于当前组件是在BrowserRouter or HashRouter中显示的
                   所以在当前组件中不用使用BrowserRouter or HashRouter来包裹NavLink/Switch/Route
            * */
            <div>
                <NavLink exact to={'/discover'} activeStyle={{color:'red'}}>推荐</NavLink>
                <NavLink exact to={'/discover/toplist'} activeStyle={{color:'red'}}>排行榜</NavLink>
                <NavLink exact to={'/discover/playlist'} activeStyle={{color:'red'}}>歌单</NavLink>

                <Switch>
                    <Route exact path={'/discover'} component={Hot}/>
                    <Route exact path={'/discover/toplist'} component={TopList}/>
                    <Route exact path={'/discover/playlist'} component={PlayList}/>
                </Switch>
            </div>
        )
    }
}

export default Discover;

```

### 手动跳转路由地址

**hash模式：window.location.hash = '/discover/playlist'; **

**history模式：this.props.history.push('/discover/playlist');**

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import User from './components/User'
import Login from './components/Login'
import Discover from './components/Discover'
import {BrowserRouter, HashRouter, NavLink, Route, Switch} from 'react-router-dom';

/*
1.手动路由跳转:
不通过Link/NavLink来设置资源地址, 而是通过JS来设置资源地址
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                <BrowserRouter>
                    <NavLink to={'/home'} activeStyle={{color:'red'}}>Home</NavLink>
                    <NavLink to={'/about'} activeStyle={{color:'red'}}>About</NavLink>
                    <NavLink to={'/user'} activeStyle={{color:'red'}}>User</NavLink>
                    <NavLink to={'/discover'} activeStyle={{color:'red'}}>广场</NavLink>

                    <Switch>
                        <Route exact path={'/home'} component={Home}/>
                        <Route exact path={'/about'} component={About}/>
                        <Route exact path={'/user'} component={User}/>
                        <Route exact path={'/login'} component={Login}/>
                        <Route path={'/discover'} component={Discover}/>
                        <Route component={Other}/>
                    </Switch>
                </BrowserRouter>
            </div>
        )
    }
}

export default App;

<!-- 手动跳转的代码实现 -->
class Discover extends React.PureComponent{
    render(){
        return (
            <div>
                <NavLink exact to={'/discover'} activeStyle={{color:'red'}}>推荐</NavLink>
                <NavLink exact to={'/discover/toplist'} activeStyle={{color:'red'}}>排行榜</NavLink>
                <NavLink exact to={'/discover/playlist'} activeStyle={{color:'red'}}>歌单</NavLink>
                <button onClick={()=>{this.btnClick()}}>歌单</button>

                <Switch>
                    <Route exact path={'/discover'} component={Hot}/>
                    <Route exact path={'/discover/toplist'} component={TopList}/>
                    <Route exact path={'/discover/playlist'} component={PlayList}/>
                </Switch>
            </div>
        )
    }
    btnClick(){
        // 如果是Hash模式, 那么只需要通过JS设置Hash值即可
        // window.location.hash = '/discover/playlist';
        // 如果一个组件是通过路由创建出来的, 那么系统就会自动传递一个history给我们
        // 我们只需要拿到这个history对象, 调用这个对象的push方法, 通过push方法修改资源地址即可
        // console.log(this.props.history);
        this.props.history.push('/discover/playlist');
    }
}

export default Discover;
```

### 手动路由跳转的注意点

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import User from './components/User'
import Login from './components/Login'
import Discover from './components/Discover'
import {BrowserRouter, HashRouter, NavLink, Route, Switch, withRouter} from 'react-router-dom';

/*
1.手动路由跳转注意点:
- 只有通过路由创建出来的组件才有history对象, 所以不能在根组件中使用手动路由跳转
- 如果想在根组件中使用手动路由跳转, 那么需要借助一个withRouter高阶组件
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                    <NavLink to={'/home'} activeStyle={{color:'red'}}>Home</NavLink>
                    <NavLink to={'/about'} activeStyle={{color:'red'}}>About</NavLink>
                    <NavLink to={'/user'} activeStyle={{color:'red'}}>User</NavLink>
                    <NavLink to={'/discover'} activeStyle={{color:'red'}}>广场</NavLink>
                    <button onClick={()=>{this.btnClick()}}>广场</button>

                    <Switch>
                        <Route exact path={'/home'} component={Home}/>
                        <Route exact path={'/about'} component={About}/>
                        <Route exact path={'/user'} component={User}/>
                        <Route exact path={'/login'} component={Login}/>
                        <Route path={'/discover'} component={Discover}/>
                        <Route component={Other}/>
                    </Switch>
            </div>
        )
    }
    btnClick(){
        // window.location.hash = '/discover';
        /*
        Cannot read property 'push' of undefined
        如果一个组件是通过路由创建的, 那么系统就会自动给这个组件传递一个history对象
        但是如果一个组件不是通过路由创建的, 那么系统就不会给这个组件传递一个history对象

        如果现在非路由创建出来的组件中使用history对象, 那么可以借助withRouter高阶组件
        只要把一个组件传递给withRouter方法, 那么这个方法就会通过路由将传入的组件创建出来

        注意点: 如果一个组件要使用路由创建, 那么这个组件必须包裹在BrowserRouter, HashRouter中
        * */
        this.props.history.push('/discover');
    }
}

export default withRouter(App);

<!-- index.js -->
import ReactDOM from 'react-dom';
import React from 'react';
import App from './App';
import {BrowserRouter} from 'react-router-dom';

ReactDOM.render(
    <BrowserRouter>
        <React.StrictMode>
            <App/>
        </React.StrictMode>
    </BrowserRouter>
    , document.getElementById('root'));
```

### 路由传参

#### 方式一：

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import User from './components/User'
import Login from './components/Login'
import Discover from './components/Discover'
import {NavLink, Route, Switch, withRouter} from 'react-router-dom';

/*
1.路由参数传递
- URL参数
?key=value&key=value
- 路由参数(动态路由)
/path/:key
- 对象
https://reactrouter.com/web/api/Link
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                <NavLink to={'/home?name=lnj&age=18'} activeStyle={{color:'red'}}>Home</NavLink>
                <NavLink to={'/about'} activeStyle={{color:'red'}}>About</NavLink>
                <NavLink to={'/user'} activeStyle={{color:'red'}}>User</NavLink>
                <NavLink to={'/discover'} activeStyle={{color:'red'}}>广场</NavLink>
                <button onClick={()=>{this.btnClick()}}>广场</button>

                <Switch>
                    <Route exact path={'/home'} component={Home}/>
                    <Route exact path={'/about'} component={About}/>
                    <Route exact path={'/user'} component={User}/>
                    <Route exact path={'/login'} component={Login}/>
                    <Route path={'/discover'} component={Discover}/>
                    <Route component={Other}/>
                </Switch>
            </div>
        )
    }
    btnClick(){
        this.props.history.push('/discover');
    }
}

export default withRouter(App);

<!-- Home组件 -->
import React from 'react';
class Home extends React.PureComponent{
    constructor(props){
        super(props);
        // console.log(this.props.location);
        // console.log(this.props.location.search);
        let query = this.props.location.search.substring(1);
        query = query.split('&');
        let obj = {};
        query.forEach((item)=>{
            // name=lnj  [name, lnj]
            let temp = item.split('=');
            obj[temp[0]] = temp[1];
        });
        console.log(obj);
    }
    render(){
        return (
            <div>Home</div>
        )
    }
}

export default Home;

```

#### 方式二：动态路由传参

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import User from './components/User'
import Login from './components/Login'
import Discover from './components/Discover'
import {NavLink, Route, Switch, withRouter} from 'react-router-dom';

/*
1.路由参数传递
- URL参数
?key=value&key=value
- 路由参数(动态路由)
/path/:key
- 对象
https://reactrouter.com/web/api/Link
* */
class App extends React.PureComponent{
    render(){
        return (
            <div>
                <NavLink to={'/home?name=lnj&age=18'} activeStyle={{color:'red'}}>Home</NavLink>
                <NavLink to={'/about/lnj/18'} activeStyle={{color:'red'}}>About</NavLink>
                <NavLink to={'/user'} activeStyle={{color:'red'}}>User</NavLink>
                <NavLink to={'/discover'} activeStyle={{color:'red'}}>广场</NavLink>
                <button onClick={()=>{this.btnClick()}}>广场</button>

                <Switch>
                    <Route exact path={'/home'} component={Home}/>
                    <Route exact path={'/about/:name/:age'} component={About}/>
                    <Route exact path={'/user'} component={User}/>
                    <Route exact path={'/login'} component={Login}/>
                    <Route path={'/discover'} component={Discover}/>
                    <Route component={Other}/>
                </Switch>
            </div>
        )
    }
    btnClick(){
        this.props.history.push('/discover');
    }
}

export default withRouter(App);

<!-- About.js -->
import React from 'react';
class About extends React.PureComponent{
    constructor(props){
        super(props);
        // console.log(this.props.match);
        console.log(this.props.match.params);
    }
    render(){
        return (
            <div>About</div>
        )
    }
}

export default About;
```

#### 方式三：to里面传对象

**通过this.props.location.state获取传递的对象**

```js
import React from 'react';
import Home from './components/Home'
import About from './components/About'
import Other from './components/Other'
import User from './components/User'
import Login from './components/Login'
import Discover from './components/Discover'
import {NavLink, Route, Switch, withRouter} from 'react-router-dom';

/*
1.路由参数传递
- URL参数
?key=value&key=value
- 路由参数(动态路由)
/path/:key
- 对象
https://reactrouter.com/web/api/Link
* */
class App extends React.PureComponent{
    render(){
        let obj = {
            name:'lnj',
            age:18,
            gender:'man'
        };
        return (
            <div>
                <NavLink to={'/home?name=lnj&age=18'} activeStyle={{color:'red'}}>Home</NavLink>
                <NavLink to={'/about/lnj/18'} activeStyle={{color:'red'}}>About</NavLink>
                <NavLink to={{
                    pathname: "/user",
                    search: "",
                    hash: "",
                    state: obj
                }} activeStyle={{color:'red'}}>User</NavLink>
                <NavLink to={'/discover'} activeStyle={{color:'red'}}>广场</NavLink>
                <button onClick={()=>{this.btnClick()}}>广场</button>

                <Switch>
                    <Route exact path={'/home'} component={Home}/>
                    <Route exact path={'/about/:name/:age'} component={About}/>
                    <Route exact path={'/user'} component={User}/>
                    <Route exact path={'/login'} component={Login}/>
                    <Route path={'/discover'} component={Discover}/>
                    <Route component={Other}/>
                </Switch>
            </div>
        )
    }
    btnClick(){
        this.props.history.push('/discover');
    }
}

export default withRouter(App);

<!-- User.js -->
import React from 'react';
import {Redirect} from 'react-router-dom';

class User extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            isLogin: true
        }
        console.log(this.props.location.state);
    }
    render(){
        let user = (
            <div>
                <h1>用户界面</h1>
                <p>用户名: jonathan_lee</p>
                <p>密码: www.it666.com</p>
            </div>
        );
        let login = <Redirect to={'/login'}/>
        return this.state.isLogin ? user : login;
    }
}

export default User;

```

### 路由集中式管理

**安装react-router-config**

**renderRoutes(routers)来替换原来的路由代码**

```js
import React from 'react';
import {NavLink, withRouter} from 'react-router-dom';
import {renderRoutes} from 'react-router-config';
import routers from './router/index';

/*
1.路由统一管理(路由集中管理)
现在虽然我们能通过路由实现组件切换, 但是现在我们的路由都比较分散, 不利于我们管理和维护
所以React也考虑到了这个问题, 也给我们提供了统一管理路由的方案
https://www.npmjs.com/package/react-router-config
* */
class App extends React.PureComponent{
    render(){
        let obj = {
            name:'lnj',
            age:18,
            gender:'man'
        };
        return (
            <div>
                <NavLink to={'/home?name=lnj&age=18'} activeStyle={{color:'red'}}>Home</NavLink>
                <NavLink to={'/about/lnj/18'} activeStyle={{color:'red'}}>About</NavLink>
                <NavLink to={{
                    pathname: "/user",
                    search: "",
                    hash: "",
                    state: obj
                }} activeStyle={{color:'red'}}>User</NavLink>
                <NavLink to={'/discover'} activeStyle={{color:'red'}}>广场</NavLink>
                <button onClick={()=>{this.btnClick()}}>广场</button>

                {/*
                <Switch>
                    <Route exact path={'/home'} component={Home}/>
                    <Route exact path={'/about/:name/:age'} component={About}/>
                    <Route exact path={'/user'} component={User}/>
                    <Route exact path={'/login'} component={Login}/>
                    <Route path={'/discover'} component={Discover}/>
                    <Route component={Other}/>
                </Switch>
                */}
                {renderRoutes(routers)}
            </div>
        )
    }
    btnClick(){
        this.props.history.push('/discover');
    }
}

export default withRouter(App);

<!-- route里面的index.js -->
import Home from '../components/Home'
import About from '../components/About'
import Other from '../components/Other'
import User from '../components/User'
import Login from '../components/Login'
import Discover from '../components/Discover'

const routers = [
    {
        path:'/home',
        exact:true,
        component: Home
    },
    {
        path:'/about/:name/:age',
        exact:true,
        component: About
    },
    {
        path:'/user',
        exact:true,
        component: User
    },
    {
        path:'/login',
        exact:true,
        component: Login
    },
    {
        path:'/discover',
        exact:true,
        component: Discover
    },
    {
        component: Other
    },
];
export default routers;

```

### 嵌套路由的集中式管理

```js
<!-- index.js -->
import Home from '../components/Home'
import About from '../components/About'
import Other from '../components/Other'
import User from '../components/User'
import Login from '../components/Login'
import Discover from '../components/Discover'
import {Hot, TopList, PlayList} from '../components/Discover';

const routers = [
    {
        path:'/home',
        exact:true,
        component: Home
    },
    {
        path:'/about/:name/:age',
        exact:true,
        component: About
    },
    {
        path:'/user',
        exact:true,
        component: User
    },
    {
        path:'/login',
        exact:true,
        component: Login
    },
    {
        path:'/discover',
        component: Discover,
        routes:[
            {
                path:'/discover',
                exact:true,
                component: Hot,
            },
            {
                path:'/discover/toplist',
                exact:true,
                component: TopList,
            },
            {
                path:'/discover/playlist',
                exact:true,
                component: PlayList,
            },
        ]
    },
    {
        component: Other
    },
];
export default routers;

<!-- Discover.js -->
    import React from 'react';
import {NavLink, Switch, Route} from "react-router-dom";
import {renderRoutes} from 'react-router-config';
import routers from '../router/index';

export function Hot() {
    return (
        <div>推荐</div>
    )
}
export function TopList() {
    return (
        <div>排行榜</div>
    )
}
export function PlayList() {
    return (
        <div>歌单</div>
    )
}

class Discover extends React.PureComponent{
    render(){
        return (
            <div id={'discover'}>
                <NavLink exact to={'/discover'} activeStyle={{color:'red'}}>推荐</NavLink>
                <NavLink exact to={'/discover/toplist'} activeStyle={{color:'red'}}>排行榜</NavLink>
                <NavLink exact to={'/discover/playlist'} activeStyle={{color:'red'}}>歌单</NavLink>
                <button onClick={()=>{this.btnClick()}}>歌单</button>

                {/*
                <Switch>
                    <Route exact path={'/discover'} component={Hot}/>
                    <Route exact path={'/discover/toplist'} component={TopList}/>
                    <Route exact path={'/discover/playlist'} component={PlayList}/>
                </Switch>
                */}
                {/*以下是2B铅笔写法, 企业开发中千万不要这么写*/}
                {/*{renderRoutes(routers[4].routes)}*/}
                {/*
                注意点: 如果当前组件是通过renderRoutes创建的, 那么系统过就会自动给这个组件传递一个route对象
                */}
                {renderRoutes(this.props.route.routes)}
            </div>
        )
    }
    btnClick(){
        // this.props.history.push('/discover/playlist');
        console.log(this.props.route);
    }
}

export default Discover;

```

## Redux

### 核心概念

```js
import React from 'react';

/*
1.什么是Redux？
Redux是一个管理状态（数据）的容器，提供了可预测的状态管理

2.什么是可预测的状态管理？
数据 在什么时候， 因为什么， 发生了什么改变，都是可以控制和追踪的， 我们就称之为预测的状态管理

3.为什么要使用Redux？
- React是通过数据驱动界面更新的，React负责更新界面， 而我们负责管理数据
- 默认情况下我们可以在每个组件中管理自己的状态， 但是现在前端应用程序已经变得越来越复杂
  状态之间可能存在依赖关系（父子、共享等），一个状态的变化会引起另一个状态的变化
- 所以当应用程序复杂的时候， 状态在什么时候改变，因为什么改变，发生了什么改变，就会变得非常难以控制和追踪
- 所以当应用程序复杂的时候，我们想很好的管理、维护、追踪、控制状态时， 我们就需要使用Redux

4.Redux核心理念
- 通过store来保存数据
- 通过action来修改数据
- 通过reducer将store和action串联起来

                    -------------
        --------->  | Component |  ---------
       |            -------------           |
       |                                    ↓
-------------       -------------       -------------
|   Store   | <---- |  Reducer  | <---- |  Action   |
-------------       -------------       -------------


const initialState = {
   heroes:[
     {name:'鲁班'， age:18},
     {name:'虞姬'， age:22},
   ]
}

const action = {type:'CHANGE_NAME', playload:{index:0, newName:'黄忠'}}
const action = {type:'CHANGE_AGE', playload:{index:1, newAge:66}}

function reducer(state = initialState, action){
    switch(action.type){
        case: 'CHANGE_NAME':
            // 修改姓名
            return newState;
        case: 'CHANGE_AGE':
             // 修改年龄
            return newState;
    }
}

官方文档： https://www.redux.org.cn/docs/introduction/CoreConcepts.html
* */
class App extends React.PureComponent{
    render(){
        return (
            <div></div>
        )
    }
}

export default App;

```

### 三大原则

```js
import React from 'react';

/*
1.Redux三大原则
- 单一数据源
    + 整个应用程序的state只存储在一个 store 中
    + Redux并没有强制让我们不能创建多个Store，但是那样做并不利于数据的维护
    + 单一的数据源可以让整个应用程序的state变得方便维护、追踪、修改

- State是只读的
    + 唯一修改State的方法一定是触发action，不要试图在其他地方通过任何的方式来修改State
    + 这样就确保了View或网络请求都不能直接修改state，它们只能通过action来描述自己想要如何修改stat；
    + 这样可以保证所有的修改都被集中化处理，并且按照严格的顺序来执行，所以不需要担心race condition（竟态）的问题；

- 使用纯函数来执行修改
    + 通过reducer将 旧state和 action联系在一起，并且返回一个新的State：
    + 随着应用程序的复杂度增加，我们可以将reducer拆分成多个小的reducers，分别操作不同state tree的一部分
    + 但是所有的reducer都应该是纯函数，不能产生任何的副作用

2.什么是纯函数？
- 返回结果只依赖于它的参数，并且在执行过程里面没有副作用

// 纯函数
function sum(num1, num2){
    return num1 + num2;
}

// 非纯函数
let num1 = 10;
function sum(num2){
    return num1 + num2;
}

// 纯函数
const num1 = 10;
function sum(num2){
    return num1 + num2;
}

官方文档： https://www.redux.org.cn/docs/introduction/ThreePrinciples.html
* */
class App extends React.PureComponent{
    render(){
        return (
            <div></div>
        )
    }
}

export default App;

```

### 基本使用

```js
const redux = require('redux');

// 定义一个状态
let initialState = {
    count: 0
};

// 利用store来保存状态（state）
const store = redux.createStore(reducer);

// 利用action来修改状态
const addAction = {type:'ADD_COUNT', num: 1};
const subAction = {type:'SUB_COUNT', num: -1};

// 利用reducer将store和action串联起来
function reducer(state = initialState, action) {
    switch (action.type) {
        case 'ADD_COUNT':
            return {count: state.count + action.num};
        case 'SUB_COUNT':
            return {count: state.count - action.num};
        default:
            return state;
    }
}

// 在组件中如何监听状态的改变？
store.subscribe((a)=>{
    console.log(store.getState());
});

// 在组件中如何从Store中获取存储的状态？
console.log(store.getState());

// 在组件中如何修改Store中存储的状态？
store.dispatch(addAction);

// console.log(store.getState());

```

### 粗略优化redux

```js
const redux = require('redux');
/*
当前代码存在的问题：
1.store、action、reducer代码都写在一个文件中， 不利于维护
2.action和reducer中都是使用字符串来指定和判断操作类型， 写错不报错
3.action中的操作写死了， 不够灵活
  ... ...
* */
const ADD_COUNT = 'ADD_COUNT';
const SUB_COUNT = 'SUB_COUNT';
// 定义一个状态
let initialState = {
    count: 0
};

// 利用store来保存状态（state）
const store = redux.createStore(reducer);

// 利用action来修改状态
// const addAction = {type:ADD_COUNT, num: 1};
// const subAction = {type:SUB_COUNT, num: -1};
const addAction = (num)=>{
    return {type:ADD_COUNT, num: num};
};
const subAction = (num)=>{
    return {type:SUB_COUNT, num: num};
};

// 利用reducer将store和action串联起来
function reducer(state = initialState, action) {
    switch (action.type) {
        case ADD_COUNT:
            return {count: state.count + action.num};
        case SUB_COUNT:
            return {count: state.count - action.num};
        default:
            return state;
    }
}

// 在组件中如何监听状态的改变？
store.subscribe((a)=>{
    console.log(store.getState());
});

// 在组件中如何从Store中获取存储的状态？
console.log(store.getState());

// 在组件中如何修改Store中存储的状态？
// store.dispatch(addAction(5));
store.dispatch(subAction(5));

// console.log(store.getState());
```

### 再react中使用redux

```js
import React from 'react';
import store from './store/store';
import {addAction, subAction} from './store/action';

class App extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            count: store.getState().count
        }
    }
    componentDidMount() {
        store.subscribe(()=>{
            this.setState({
                count: store.getState().count
            })
        })
    }
    componentWillUnmount() {
        store.unsubscribe();
    }

    render(){
        return (
            <div>
                <p>{this.state.count}</p>
                <button onClick={()=>{this.btnClick()}}>增加</button>
            </div>
        )
    }
    btnClick(){
        store.dispatch(addAction(5));
    }
}

export default App;

```

**代码的目录结构如下**

![img](file:///C:\Users\86157\AppData\Roaming\Tencent\Users\1325116124\QQ\WinTemp\RichOle\M{]EEY76$59H}0O0FAH`3AK.png)

### 子组件中使用redux

**和上述的App.js差不多，使用子组件也和平常的子组件一样，只不过现在我们修改的是共享的数据**

```js
import React from 'react';
import store from '../store/store';
import {addAction, subAction} from '../store/action';

class About extends React.PureComponent{
    constructor(props){
        super(props);
        this.state = {
            count: store.getState().count
        }
    }
    componentDidMount() {
        store.subscribe(()=>{
            this.setState({
                count: store.getState().count
            })
        })
    }
    componentWillUnmount() {
        store.unsubscribe();
    }

    render(){
        return (
            <div>
                <p>{this.state.count}</p>
                <button onClick={()=>{this.btnClick()}}>递减</button>
            </div>
        )
    }
    btnClick(){
        store.dispatch(subAction(1));
    }
}

export default About;

```

### React-Redux

**使用步骤：首先用Provider将根组件包住，然后传递store，在需要使用store的子组件文件中定义映射的方法，暴露的时候使用connect将组件和映射方法相关联**

```js
/*
1.当前使用Redux存在的问题
- 冗余代码太多, 每次使用都需要在构造函数中获取
              每次使用都需要监听和取消监听
- 操作store的代码过于分散

2.如何解决冗余代码太多问题?
使用React-Redux

3.什么是React-Redux
React-Redux是Redux官方的绑定库,能够让我们在组件中更好的读取和操作Redux保存的状态
https://react-redux.js.org/introduction/quick-start
* */

<!-- index.js -->
import ReactDOM from 'react-dom';
import React from 'react';
import App from './App';
import { Provider } from 'react-redux'
import store from './store/store';

ReactDOM.render(
    /*只要利用Provider将祖先组件包裹起来,
      并且通过Provider的store属性将Redux的store传递给Provider
      那么就可以在所有后代中直接使用Redux了*/
    <Provider store={store}>
        <React.StrictMode>
            <App/>
        </React.StrictMode>
    </Provider>
    , document.getElementById('root'));

<!-- Home.js -->
import React from 'react';
import { connect } from 'react-redux'
import {addAction, subAction} from '../store/action';

class Home extends React.PureComponent{
    render(){
        return (
            <div>
                {/*通过props来使用redux中保存的数据*/}
                <p>{this.props.count}</p>
                <button onClick={()=>{this.props.increment()}}>递增</button>
            </div>
        )
    }
}
// 在mapStateToProps方法中告诉React-Redux, 需要将store中保存的哪些数据映射到当前组件的props上
const mapStateToProps = (state)=>{
    return {
        count: state.count
    }
};
// 在mapDispatchToProps方法中告诉React-Redux, 需要将哪些派发的任务映射到当前组件的props上
const mapDispatchToProps = (dispatch) =>{
    return {
        increment(){
            dispatch(addAction(1));
        }
    }
};
//使用connect函数将组件和上述定义的映射方法相关联
export default connect(mapStateToProps, mapDispatchToProps)(Home);

```

### connect的实现原理

```js
import ReactDOM from 'react-dom';
import React from 'react';
import store from '../store/store';

function connect(mapStateToProps, mapDispatchToProps) {
    return function enhanceComponent(WrappedComponent) {
        class AdvComponent extends React.PureComponent{
            constructor(props){
                super(props);
                this.state = {
                    storeState : {...mapStateToProps(store.getState())}
                }
            }
            componentDidMount() {
                store.subscribe(()=>{
                    this.setState({
                        storeState: {...mapStateToProps(store.getState())}
                    })
                })
            }
            componentWillUnmount() {
                store.unsubscribe();
            }
            render() {
                return (<WrappedComponent {...this.props}
                                          {...mapStateToProps(store.getState())}
                                          {...mapDispatchToProps(store.dispatch)}/>)
            }
        }
        return AdvComponent;
    }
}
export default connect;

```

### redux太难了，剩下的没有总结了

## Hooks

### Hooks概念

```js
import React from 'react';
/*
1.什么是Hook?
- Hook 是 React 16.8 的新增特性，
  它可以让函数式组件拥有类组件特性

2.为什么需要Hook?
- 在Hook出现之前, 如果我们想在组件中保存自己的状态,
  如果我们想在组件的某个生命周期中做一些事情, 那么我们必须使用类组件
    + 但是类组件的学习成本是比较高的, 你必须懂得ES6的class, 你必须懂得箭头函数
    + 但是在类组件的同一个生命周期方法中, 我们可能会编写很多不同的业务逻辑代码
      这样就导致了大量不同的业务逻辑代码混杂到一个方法中, 导致代码变得很难以维护
      (诸如: 在组件被挂载的生命周期中, 可能主要注册监听, 可能需要发送网络请求等)
    + 但是在类组件中共享数据是非常繁琐的, 需要借助Context或者Redux等
- 所以当应用程序变得复杂时, 类组件就会变得非常复杂, 非常难以维护
- 所以Hook就是为了解决以上问题而生的

3.如何使用Hook?
- Hook的使用我们无需额外安装任何第三方库, 因为它就是React的一部分
- Hook只能在函数组件中使用, 不能在类组件，或者函数组件之外的地方使用
- Hook只能在函数最外层调用, 不要在循环、条件判断或者子函数中调用

官方文档地址: https://react.docschina.org/docs/hooks-intro.html
* */
function Home() {
    // 只能在函数体的最外层使用
    // 只能在这个地方使用Hook
    if(){
        // 不能使用Hook
    }
    while (){
        // 不能使用Hook
    }
    for(){
        // 不能使用Hook
    }
    switch () {
        // 不能使用Hook
    }
    function demo() {
        // 不能使用Hook
    }
}
class App extends React.PureComponent{
    render(){
        return (
            <div>APP</div>
        )
    }
}
export default App;

```

### Hooks的基本使用

```js
import React, {useState} from 'react';
/*
1.什么是Hook?
Hook就是一个特殊的函数

2.什么是useState Hook?
可以让函数式组件保存自己状态的函数

3.useState Hook如何使用?
- Hook只能在函数式组件中使用, 并且只能在函数体的最外层使用


* */
function App() {
    /*
    useState:
    参数: 保证状态的初始值
    返回值: 是一个数组, 这个数组中有两个元素
           第一个元素: 保存的状态
           第二个元素: 修改保存状态的方法
    * */
    const arr = useState(666);
    const [state, setState] = arr;
    return (
        <div>
            <p>{state}</p>
            <button onClick={()=>{setState(state + 1)}}>增加</button>
            <button onClick={()=>{setState(state - 1)}}>减少</button>
        </div>
    )
}
export default App;
```

### useState保存多个状态、复杂状态

```js
import React, {useState} from 'react';
function App() {
    /*
    useState:
    参数: 保证状态的初始值
    返回值: 是一个数组, 这个数组中有两个元素
           第一个元素: 保存的状态
           第二个元素: 修改保存状态的方法
    * */
    // const arr = useState(666);
    // const [state, setState] = arr;

    // 注意点: 在同一个函数式组件中, 是可以多次使用同名的Hook的
    const [ageState, setAgeState] = useState(18);
    const [nameState, setNameState] = useState('lnj');
    const [studentState, setStudentState] = useState({name:'zs', age:23});
    const [heroState, setHeroState] = useState([
        {id: 1, name:'鲁班'},
        {id: 1, name:'虞姬'},
        {id: 1, name:'黄忠'},
    ]);

    return (
        <div>
            <p>{ageState}</p>
            <button onClick={()=>{setAgeState(ageState + 1)}}>增加</button>
            <button onClick={()=>{setAgeState(ageState - 1)}}>减少</button>
            <hr/>
            <p>{nameState}</p>
            <button onClick={()=>{setNameState('it666')}}>修改</button>
            <hr/>
            <p>{studentState.name}</p>
            <p>{studentState.age}</p>
            <hr/>
            <ul>{
                heroState.map((hero)=>{
                    return <li>{hero.name}</li>
                })
            }</ul>
        </div>
    )
}
export default App;
```

### useState的注意点：异步和数据更新

```js
import React, {useState} from 'react';
function App() {
    /*
    useState注意点:
    和类组件中的setState一样
    * */
    const [ageState, setAgeState] = useState(18);
    const [nameState, setNameState] = useState('lnj');
    const [studentState, setStudentState] = useState({name:'zs', age:23});
    const [heroState, setHeroState] = useState([
        {id: 1, name:'鲁班'},
        {id: 1, name:'虞姬'},
        {id: 1, name:'黄忠'},
    ]);
    function incrementAge() {
        // setAgeState(ageState + 10);
        // setAgeState(ageState + 10);
        // setAgeState(ageState + 10);

        setAgeState((preAgeState)=>preAgeState + 10);
        setAgeState((preAgeState)=>preAgeState + 10);
        setAgeState((preAgeState)=>preAgeState + 10);
    }
    function changeName() {
        // studentState.name = 'it666';
        setStudentState({...studentState, name:'it666'});
    }
    return (
        <div>
            <p>{ageState}</p>
            <button onClick={()=>{incrementAge()}}>增加</button>
            <button onClick={()=>{setAgeState(ageState - 1)}}>减少</button>
            <hr/>
            <p>{nameState}</p>
            <button onClick={()=>{setNameState('it666')}}>修改</button>
            <hr/>
            <p>{studentState.name}</p>
            <p>{studentState.age}</p>
            <button onClick={()=>{changeName()}}>修改</button>
            <hr/>
            <ul>{
                heroState.map((hero)=>{
                    return <li>{hero.name}</li>
                })
            }</ul>
        </div>
    )
}
export default App;

```

### useEffect

```js
import React, {useState, useEffect} from 'react';
/*
1.什么是useEffect Hook?
可以把 useEffect Hook 看做
componentDidMount，componentDidUpdate 和 componentWillUnmount
这三个生命周期函数的组合

2.useEffect Hook特点?
可以设置依赖, 只有依赖发生变化的时候才执行
第一个参数为更新或者挂载的回调函数，在第一个函数中可以返回另一个函数，返回的函数作为卸载的生命周期函数
第二个参数为一个数组，数组中的变量表示只有当这些数据变化时才会触发useEffect
* */
function Home() {
    const [nameState, setNameState] = useState('lnj');
    const [ageState, setAgeState] = useState(0);
    useEffect(()=>{
        // componentDidMount
        // componentDidUpdate
        console.log('组件被挂载或者组件更新完成');
        return ()=>{
            // componentWillUnmount
            console.log('组件即将被卸载');
        }
    });
    return (
        <div>
            <p>{nameState}</p>
            <button onClick={()=>{setNameState('it666')}}>修改</button>
            <p>{ageState}</p>
            <button onClick={()=>{setAgeState(ageState + 1)}}>增加</button>
            <button onClick={()=>{setAgeState(ageState - 1)}}>减少</button>
            <hr/>
        </div>
    )
}
function App() {
    const [isShowState, setIsShowState] = useState(true);
    return (
        <div>
            {isShowState && <Home/>}
            <button onClick={()=>{setIsShowState(!isShowState)}}>切换</button>
        </div>
        )
}
export default App;

```

### useEffect的优点

```js
import React, {useState, useEffect} from 'react';
/*
1.useEffect Hook对比类组件生命周期方法优势
易于拆分
可以调用多次useEffect
* */
function Home() {
    const [nameState, setNameState] = useState('lnj');
    const [ageState, setAgeState] = useState(0);
    useEffect(()=>{
        // 组件被挂载
        console.log('修改DOM');
    });
    useEffect(()=>{
        // 组件被挂载
        console.log('注册监听');
        return ()=>{
            console.log('移出监听');
        }
    });
    useEffect(()=>{
        console.log('发送网络请求');
    });
    return (
        <div>
            <p>{nameState}</p>
            <button onClick={()=>{setNameState('it666')}}>修改</button>
            <p>{ageState}</p>
            <button onClick={()=>{setAgeState(ageState + 1)}}>增加</button>
            <button onClick={()=>{setAgeState(ageState - 1)}}>减少</button>
            <hr/>
        </div>
    )
}
function App() {
    const [isShowState, setIsShowState] = useState(true);
    return (
        <div>
            {isShowState && <Home/>}
            <button onClick={()=>{setIsShowState(!isShowState)}}>切换</button>
        </div>
        )
}
export default App;

```

### useContext

**用来解决函数式组件对应多个生产者的情况**

```js
import React, {createContext, useContext} from 'react';
/*
1.什么是useContext Hook?
useContext相当于 类组件中的 static contextType = Context
* */
const UserContext = createContext({});
const ColorContext = createContext({});

// const {Provider, Consumer} = UserContext;
function Home() {
    const user = useContext(UserContext);
    const color = useContext。(ColorContext);
    return (
        /*
        <UserContext.Consumer>
            {
                value1 =>{
                    return (
                        <ColorContext.Consumer>
                            {
                                value2 =>{
                                    return (
                                        <div>
                                            <p>{value1.name}</p>
                                            <p>{value1.age}</p>
                                            <p>{value2.color}</p>
                                        </div>
                                    )
                                }
                            }
                        </ColorContext.Consumer>
                    )
                }
            }
        </UserContext.Consumer>
         */
        /*
        <div>
            <p>{this.context.name}</p>
            <p>{this.context.age}</p>
        </div>
         */
        <div>
            <p>{user.name}</p>
            <p>{user.age}</p>
            <p>{color.color}</p>
        </div>
    )
}
// Home.contextType = UserContext;
function App() {
    return (
        <UserContext.Provider value={{name:'lnj', age:18}}>
            <ColorContext.Provider value={{color:'red'}}>
                <Home/>
            </ColorContext.Provider>
        </UserContext.Provider>
        )
}
export default App;

```

### useReducer

**更好的复用对数据的操作逻辑代码**

```js
import React, {useState, useReducer} from 'react';
/*
1.什么是useReducer Hook?
从名称来看, 很多人会误以为useReducer是用来替代Redux的, 但是其实不是
useReducer是useState的一种替代方案, 可以让我们很好的复用操作数据的逻辑代码
* */
function reducer(state, action) {
    switch (action.type) {
        case 'ADD':
            return {...state, num: state.num + 1};
        case 'SUB':
            return {...state, num: state.num - 1};
        default:
            return state;
    }
}
function Home() {
    /*
    const [numState, setNumState] = useState(0);
    function increment() {
        setNumState(numState + 1);
    }
    function decrement() {
        setNumState(numState - 1);
    }
     */
    /*
    useReducer接收的参数:
    第一个参数: 处理数据的函数
    第二个参数: 保存的默认值
    useReducer返回值:
    会返回一个数组, 这个数组中有两个元素
    第一个元素: 保存的数据
    第二个元素: dispatch函数
    * */
    const [state, dispatch] = useReducer(reducer, {num: 0});
    return (
        <div>
            <p>{state.num}</p>
            <button onClick={()=>{dispatch({type:'ADD'})}}>增加</button>
            <button onClick={()=>{dispatch({type:'SUB'})}}>减少</button>
        </div>
    )
}
function About() {
    // 注意点: 不同组件中的useState保存的状态是相互独立的, 是相互不影响的
    /*
    const [numState, setNumState] = useState(5);
    function increment() {
        setNumState(numState + 1);
    }
    function decrement() {
        setNumState(numState - 1);
    }
     */
    const [state, dispatch] = useReducer(reducer, {num: 5});
    return (
        <div>
            <p>{state.num}</p>
            <button onClick={()=>{dispatch({type:'ADD'})}}>增加</button>
            <button onClick={()=>{dispatch({type:'SUB'})}}>减少</button>
        </div>
    )
}
function App() {
    return (
        <div>
            <Home/>
            <About/>
        </div>
        )
}
export default App;

```

### 	useCallback

```js
import React, {useState, memo, useCallback} from 'react';

/*
1.什么是useCallback Hook?
useCallback用于优化代码, 可以让对应的函数只有在依赖发生变化时才重新定义
* */
/*
当前Home和About重新渲染的原因是因为
父组件中的数据发生了变化, 会重新渲染父组件
重新渲染父组件, 就会重新执行父组件函数
重新执行父组件函数, 就会重新定义increment/decrement
既然increment/decrement是重新定义的, 所以就和上一次的不是同一个函数了
既然不是同一个函数, 所以Home和About接收到的内容也和上一次的不一样了
既然接收到的内容和上一次不一样了, 所以就会重新渲染
* */
function Home(props) {
    console.log('Home被渲染了');
    return (
        <div>
            <p>Home</p>
            <button onClick={()=>{props.handler()}}>增加</button>
        </div>
    )
}
function About(props) {
    console.log('About被渲染了');
    return (
        <div>
            <p>About</p>
            <button onClick={()=>{props.handler()}}>减少</button>
        </div>
    )
}

const MemoHome = memo(Home);
const MemoAbout = memo(About);

function App() {
    console.log('App被渲染了');
    const [numState, setNumState] = useState(0);
    const [countState, setCountState] = useState(0);
    function increment() {
        setNumState(numState + 1);
    }
    // function decrement() {
    //     setCountState(countState - 1);
    // }
    // 以下代码的作用: 只要countState没有发生变化, 那么useCallback返回的永远都是同一个函数
    const decrement = useCallback(()=>{
        setCountState(countState - 1);
    }, [countState]);
    return (
        <div>
            <p>numState = {numState}</p>
            <p>countState = {countState}</p>
            {/*<button onClick={()=>{increment()}}>增加</button>*/}
            {/*<button onClick={()=>{decrement()}}>减少</button>*/}
            <MemoHome handler={increment}/>
            <MemoAbout handler={decrement}/>
        </div>
    )
}
export default App;
```

### useMemo：useCallback的底层实现

**useMemo的语法和useCallback差不多**

```js
import React, {useState, memo, useCallback, useMemo} from 'react';

/*
1.什么是useMemo Hook?
useMemo用于优化代码, 可以让对应的函数只有在依赖发生变化时才返回新的值
* */
function Home(props) {
    console.log('Home被渲染了');
    return (
        <div>
            <p>Home</p>
            <button onClick={()=>{props.handler()}}>增加</button>
        </div>
    )
}
function About(props) {
    console.log('About被渲染了');
    return (
        <div>
            <p>About</p>
            <button onClick={()=>{props.handler()}}>减少</button>
        </div>
    )
}

const MemoHome = memo(Home);
const MemoAbout = memo(About);

function App() {
    console.log('App被渲染了');
    const [numState, setNumState] = useState(0);
    const [countState, setCountState] = useState(0);
    function increment() {
        setNumState(numState + 1);
    }
    // 以下代码的作用: 只要countState没有发生变化, 那么useCallback返回的永远都是同一个函数
    /*
    function useCallback(fn, arr){
        return useMemo(()=>{
            return fn;
        }, arr);
    }
    * */
    /*
    const decrement = useCallback(()=>{
        setCountState(countState - 1);
    }, [countState]);
     */
    // 以下代码的作用: 只要countState没有发生变化, 那么useMemo返回的永远都是同一个值
    const decrement = useMemo(()=>{
        return ()=>{
            setCountState(countState - 1);
        };
    }, [countState]);
    return (
        <div>
            <p>numState = {numState}</p>
            <p>countState = {countState}</p>
            <MemoHome handler={increment}/>
            <MemoAbout handler={decrement}/>
        </div>
    )
}
export default App;
```

### useMemo的另一种用法

**useMemo返回一个值**

```js
import React, {useState, memo, useCallback, useMemo} from 'react';

/*
1.什么是useMemo Hook?
useMemo用于优化代码, 可以让对应的函数只有在依赖发生变化时才返回新的值
* */
function Home(props) {
    console.log('Home被渲染了');
    return (
        <div>
            <p>Home</p>
            <button onClick={()=>{props.handler()}}>增加</button>
        </div>
    )
}
function About(props) {
    console.log('About被渲染了');
    return (
        <div>
            <p>About</p>
            <p>{props.user.name}</p>
            <p>{props.user.age}</p>
        </div>
    )
}

const MemoHome = memo(Home);
const MemoAbout = memo(About);

function App() {
    console.log('App被渲染了');
    const [numState, setNumState] = useState(0);
    function increment() {
        setNumState(numState + 1);
    }
	//让数据不依赖任何一个值，则永远不会发生变化，则不需要重新渲染
    const user = useMemo(()=>{
        return {name: 'lnj', age:18};
    }, []);
    return (
        <div>
            <p>numState = {numState}</p>
            <MemoHome handler={increment}/>
            <MemoAbout user={user}/>
        </div>
    )
}
export default App;

```

### useMemo的另一个例子

```js
import React, {useState, useMemo} from 'react';

/*
1.什么是useMemo Hook?
useMemo用于优化代码, 可以让对应的函数只有在依赖发生变化时才返回新的值
* */
// 定义一个函数, 模拟耗时耗性能操作
function calculate() {
    console.log('calculate被执行了');
    let total = 0;
    for(let i = 0; i < 999; i++){
        total += i;
    }
    return total;
}
function App() {
    console.log('App被渲染了');
    const [numState, setNumState] = useState(0);
    // const total = calculate();
    const total = useMemo(()=>{
        return calculate();
    }, []);
    return (
        <div>
            <p>{total}</p>
            <p>{numState}</p>
            <button onClick={()=>{setNumState(numState + 1)}}>增加</button>
        </div>
    )
}
export default App;

```

### useRef

**将类组件中的createRef替换为useRef即可**

```js
import React, {createRef, useRef} from 'react';

/*
1.什么是useRef Hook?
useRef就是createRef的Hook版本, 只不过比createRef更强大一点
* */
class Home extends React.PureComponent{
    render() {
        return (
            <div>Home</div>
        )
    }
}
function About() {
    return (
        <div>About</div>
    )
}
function App() {
    // const pRef = createRef();
    // const homeRef = createRef();
    const pRef = useRef();
    const homeRef = useRef();
    function btnClick() {
        console.log(pRef.current);
        console.log(homeRef.current);
    }
    return (
        <div>
            <p ref={pRef}>我是段落</p>
            <Home ref={homeRef}/>
            <About/>
            <button onClick={()=>{btnClick()}}>获取</button>
        </div>
    )
}
export default App;

```

### useRef和useState的区别

```js
import React, {useState, createRef, useRef, useEffect} from 'react';

/*
1.什么是useRef Hook?
useRef就是createRef的Hook版本, 只不过比createRef更强大一点
* */
class Home extends React.PureComponent{
    render() {
        return (
            <div>Home</div>
        )
    }
}
function About() {
    return (
        <div>About</div>
    )
}

function App() {
    /*
    const pRef = createRef();
    // createRef和useRef区别:
    // useRef除了可以用来获取元素以外, 还可以用来保存数据
    const homeRef = useRef();
    function btnClick() {
        console.log(pRef); // {current: p}
        console.log(homeRef); // {current: Home}
    }
     */
    /*
    useState和useRef区别:
    useRef中保存的数据, 除非手动修改, 否则永远都不会发生变化
    * */
    const [numState, setNumState] = useState(0);
    // const age = useRef(18); // {current: 18}
    const age = useRef(numState); // {current: 0}
    //这样age.current会保存numState的旧值
    useEffect(()=>{
        age.current = numState;
    }, [numState]);
    return (
        <div>
            {/*
            <p ref={pRef}>我是段落</p>
            <Home ref={homeRef}/>
            <About/>
            <button onClick={()=>{btnClick()}}>获取</button>
            */}
            <p>上一次的值: {age.current}</p>
            <p>当前的值  :{numState}</p>
            <button onClick={()=>{setNumState(numState + 1)}}>增加</button>
        </div>
    )
}
export default App;

```

### useImperativeHandle

**在函数式子组件中定义一些方法，通过useImperativeHandle暴露给父组件**

```js
import React, {useRef, forwardRef, useImperativeHandle} from 'react';

/*
1.什么是useImperativeHandle Hook?
useImperativeHandle可以让你在使用 ref 时自定义暴露给父组件的实例值
* */
function Home(props, appRef) {
    const inputRef = useRef();
    useImperativeHandle(appRef, ()=>{
        return {
            myFocus: ()=>{
                inputRef.current.focus();
            }
        }
    });
    return (
        <div>
            <p>Home</p>
            <input ref={inputRef} type="text" placeholder={'请输入内容'}/>
        </div>
    )
}
const ForwardHome = forwardRef(Home);
function App() {
    const appRef = useRef();
    function btnClick() {
        appRef.current.myFocus();
    }
    return (
        <div>
            <ForwardHome ref={appRef}/>
            <button onClick={()=>{btnClick()}}>获取</button>
        </div>
    )
}
export default App;
```

### useLayoutEffect

**大部分使用情况和useEffect相同**

**区别：useEffect在更新和挂载的时候比useLayoutEffect慢，所以如果要在挂载之前操作dom，则在useLayoutEffect中操作dom**

```js
import React, {useRef, useState, useEffect, useLayoutEffect} from 'react';
import './app.css'
/*
1.什么是useLayoutEffect Hook?
- 大部分情况下useLayoutEffect和useEffect没太大区别(用法格式都相同)
- 但是如果需要修改DOM的布局样式, 那么推荐使用useLayoutEffect

2.为什么推荐在useLayoutEffect中修改DOM的布局样式?
- useEffect 函数会在组件渲染到屏幕之后才执行, 所以会可能会出现闪屏情况
- useLayoutEffect 函数是在组件渲染到屏幕之前执行, 所以不会出现闪屏情况
* */
function Home() {
    /*
    useEffect和useLayoutEffect区别:
    执行时机不同:
    如果是挂载或者更新组件, 那么useLayoutEffect会比useEffect先执行
    如果是卸载组件, 那么useEffect会比useLayoutEffect先执行
    useEffect: 同步
    useLayoutEffect: 异步

    什么时候使用useEffect?
    在绝大多数的情况下能用useEffect, 就用useEffect

    什么时候用useLayoutEffect?
    只有在需要组件挂载之后更新DOM的布局和样式的时候才使用useLayoutEffect

    为什么要使用useLayoutEffect来更新DOM布局和样式?
    useEffect是组件已经渲染到屏幕上了才执行
    useLayoutEffect是组件还没有渲染到屏幕上就会执行
    所以如果在组件已经渲染到屏幕上了, 才去更新DOM的布局和样式, 那么用户体验不好, 会看到闪屏的情况
    而如果是在组件还没有渲染到屏幕上, 就去更新DOM的布局和样式, 那么用户体验更好, 看不到闪屏情况
    * */
    const pRef = useRef();
    // useEffect(()=>{
    //     // console.log('组件被挂载或更新完成 - useEffect');
    //     pRef.current.style.left = '0px';
    //     pRef.current.style.left = '500px';
    //
    //     return ()=>{
    //         console.log('组件即将被卸载 - useEffect');
    //     }
    // });
    useLayoutEffect(()=>{
        // console.log('组件被挂载或更新完成 - useLayoutEffect');
        pRef.current.style.left = '0px';
        pRef.current.style.left = '500px';

        return ()=>{
            console.log('组件即将被卸载 - useLayoutEffect');
        }
    });
    return (
        <div>
            <p ref={pRef}></p>
        </div>
    )
}
function App() {
    const [show, setShow] = useState(true);
    return (
        <div>
            {show && <Home/>}
            <button onClick={()=>{setShow(!show)}}>切换</button>
        </div>
    )
}
export default App;
```

### 自定义Hooks

**React中只有两个地方可以使用Hook，一个时函数式组件，一个时自定义Hook中**

**自定义Hook：在函数的名称前加上use即可**

```js
import React, {useEffect, useState} from 'react';
import './app.css'
/*
1.什么是自定义 Hook?
通过自定义 Hook，可以对其它Hook的代码进行复用

官方文档地址: https://react.docschina.org/docs/hooks-custom.html
* */

/*
注意点: 在React中只有两个地方可以使用Hook
       - 函数式组件中
       - 自定义Hook中
如何自定义一个Hooks
只要在函数名称前面加上use, 那么就表示这个函数是一个自定义Hook, 就表示可以在这个函数中使用其它的Hook
* */
function useAddListenr(name) {
    useEffect(()=>{
        console.log(name, ' - 组件被挂载或者更新完成 -- 添加监听');
        return ()=>{
            console.log(name, ' - 组件即将被卸载 -- 移出监听');
        }
    });
}
function Home() {
    /*
    useEffect(()=>{
        console.log('Home - 组件被挂载或者更新完成 -- 添加监听');
        return ()=>{
            console.log('Home - 组件即将被卸载 -- 移出监听');
        }
    });
     */
    useAddListenr('Home');
    return (
        <div>Home</div>
    )
}
function About() {
    /*
    useEffect(()=>{
        console.log('About - 组件被挂载或者更新完成 -- 添加监听');
        return ()=>{
            console.log('About - 组件即将被卸载 -- 移出监听');
        }
    });
     */
    useAddListenr('About');
    return (
        <div>About</div>
    )
}
function App() {
    const [show, setShow] = useState(true);
    return (
        <div>
            {show && <Home/>}
            {show && <About/>}
            <button onClick={()=>{setShow(!show)}}>切换</button>
        </div>
    )
}
export default App;

```

### 自定义Hook的使用案例

```js
import React, {createContext, useContext} from 'react';
/*
1.什么是自定义 Hook?
通过自定义 Hook，可以对其它Hook的代码进行复用

官方文档地址: https://react.docschina.org/docs/hooks-custom.html
* */
const UserContext = createContext({});
const InfoContext = createContext({});
function useGetContext() {
    // 在企业开发中, 但凡需要抽取代码, 但凡被抽取的代码中用到了其它的Hook,
    // 那么就必须把这些代码抽取到自定义Hook中
    const user = useContext(UserContext);
    const info = useContext(InfoContext);
    return [user, info]
}
function Home() {
    // const user = useContext(UserContext);
    // const info = useContext(InfoContext);
    const [user, info] = useGetContext();
    return (
        <div>
            <p>{user.name}</p>
            <p>{user.age}</p>
            <p>{info.gender}</p>
            <hr/>
        </div>
    )
}
function About() {
    // const user = useContext(UserContext);
    // const info = useContext(InfoContext);
    const [user, info] = useGetContext();
    return (
        <div>
            <p>{user.name}</p>
            <p>{user.age}</p>
            <p>{info.gender}</p>
            <hr/>
        </div>
    )
}
function App() {
    return (
        <UserContext.Provider value={{name:'lnj', age:18}}>
            <InfoContext.Provider value={{gender:'man'}}>
                <Home/>
                <About/>
            </InfoContext.Provider>
        </UserContext.Provider>
    )
}
export default App;

```

