# vue
## vue的基础指令

###  基本模板(不使用脚手架)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-Vue基本模板</title>
    <!--1.下载导入Vue.js-->
    <script src="js/vue.js"></script>
</head>
<body>
<div id="app">
    <p>{{ name }}</p>
</div>
<script>
    // 2.创建一个Vue的实例对象
    let vue = new Vue({
        // 3.告诉Vue的实例对象, 将来需要控制界面上的哪个区域
        el: '#app',
        // 4.告诉Vue的实例对象, 被控制区域的数据是什么
        data: {
            name: "李南江"
        }
    });
</script>
</body>
</html>

<!--
1.Vue框架使用方式
1.1传统下载导入使用
1.2vue-cli安装导入使用

2.Vue框架使用步骤
2.1下载Vue框架
2.2导入Vue框架
2.3创建Vue实例对象
2.4指定Vue实例对象控制的区域
2.5指定Vue实例对象控制区域的数据
-->
```

### vue数据的单项绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-Vue数据单向传递</title>
    <!--1.下载导入Vue.js-->
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.MVVM设计模式
在MVVM设计模式中由3个部分组成
M : Model      数据模型(保存数据, 处理数据业务逻辑)
V : View       视图(展示数据, 与用户交互)
VM: View Model 数据模型和视图的桥梁(M是中国人, V是美国人, VM就是翻译)

MVVM设计模式最大的特点就是支持数据的双向传递
数据可以从 M -> VM -> V
也可以从   V -> VM -> M

2.Vue中MVVM的划分
Vue其实是基于MVVM设计模式的
被控制的区域: View
Vue实例对象 : View Model
实例对象中的data: Model

3.Vue中数据的单向传递
我们把"数据"交给"Vue实例对象", "Vue实例对象"将数据交给"界面"
      Model  ->  View Model                       ->   View
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <p>{{ name }}</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            name: "李南江"
        }
    });
</script>
</body>
</html>
```

### v-model数据的双向传递

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-Vue数据双向传递</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.Vue调试工具安装
如果你能打开谷歌插件商店, 直接在线安装即可
https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN

由于国内无法打开谷歌国外插件商店, 所以可以离线安装
https://www.chromefor.com/vue-js-devtools_v5-3-0/

2.安装步骤:
2.1下载离线安装包
2.2打开谷歌插件界面
2.3直接将插件拖入
2.4报错 程序包无效: "CRX_HEADER_INVALID"
   可以将安装包修改为rar后缀, 解压之后再安装
2.5重启浏览器
-->
<!--
2.数据双向绑定
默认情况下Vue只支持数据单向传递 M -> VM -> V
但是由于Vue是基于MVVM设计模式的, 所以也提供了双向传递的能力
在<input>、<textarea> 及 <select> 元素上可以用 v-model 指令创建双向数据绑定

注意点: v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值
而总是将 Vue 实例的数据作为数据来源
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <p>{{ name }}</p>
    <input type="text" v-model="msg">
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            name: "李南江",
            msg: "知播渔"
        }
    });
</script>
</body>
</html>
```

### v-once只渲染一次

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-常用指令-v-once</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.什么是指令?
指令就是Vue内部提供的一些自定义属性,
这些属性中封装好了Vue内部实现的一些功能
只要使用这些指令就可以使用Vue中实现的这些功能

2.Vue数据绑定的特点
只要数据发生变化, 界面就会跟着变化

3.v-once指令:
让界面不要跟着数据变化, 只渲染一次
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <p v-once>原始数据: {{ name }}</p>
    <p>当前数据: {{ name }}</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            name: "李南江",
        }
    });
</script>
</body>
</html>
```

### v-cloak数据渲染完之后才显示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-常用指令-v-cloak</title>
    <style>
        [v-cloak] { display: none }
    </style>
</head>
<body>
<!--
1.Vue数据绑定过程
1.1会先将未绑定数据的界面展示给用户
1.2然后再根据模型中的数据和控制的区域生成绑定数据之后的HTML代码
1.3最后再将绑定数据之后的HTML渲染到界面上

正是在最终的HTML被生成渲染之前会先显示模板内容
所以如果用户网络比较慢或者网页性能比较差, 那么用户会看到模板内容

2.如何解决这个问题
利用v-cloak配合 [v-cloak]:{display: none}默认先隐藏未渲染的界面
等到生成HTML渲染之后再重新显示

3.v-cloak指令作用:
数据渲染之后自动显示元素
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <p v-cloak>{{ name }}</p>
</div>
<!--
<div id="app">
    <p> 李南江 </p>
</div>
-->
<script src="js/vue.js"></script>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            name: "李南江",
        }
    });
</script>
</body>
</html>
```

### v-text和v-html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>07-常用指令v-text和v-html</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.什么是v-text指令
v-text就相当于过去学习的innerText

2.什么是v-html指令
v-html就相当于过去学习的innerHTML
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <!--插值的方式: 可以将指定的数据插入到指定的位置-->
<!--    <p>++++{{ name }}++++</p>-->
    <!--插值的方式: 不会解析HTML-->
<!--    <p>++++{{ msg }}++++</p>-->
    <!--v-text的方式: 会覆盖原有的内容-->
<!--    <p v-text="name">++++++++</p>-->
    <!--v-text的方式: 也不会解析HTML-->
<!--    <p v-text="msg">++++++++</p>-->
    <!--v-html的方式: 会覆盖原有的内容-->
    <p v-html="name">++++++++</p>
    <!--v-html的方式:会解析HTML-->
    <p v-html="msg">++++++++</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            name: "李南江",
            msg: "<span>我是span</span>"
        }
    });
</script>
</body>
</html>
```

### v-if

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08-常用指令v-if</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.什么是v-if指令
条件渲染: 如果v-if取值是true就渲染元素, 如果不是就不渲染元素

2.v-if特点:
如果条件不满足根本就不会创建这个元素(重点)

3.v-if注意点
v-if可以从模型中获取数据
v-if也可以直接赋值一个表达式
-->
<!--
4.v-else指令
v-else指令可以和v-if指令配合使用, 当v-if不满足条件时就执行v-else就显示v-else中的内容

5.v-else注意点
v-else不能单独出现
v-if和v-else中间不能出现其它内容
-->
<!--
6.v-else-if指令
v-else-if可以和v-if指令配合使用, 当v-if不满足条件时就依次执行后续v-else-if, 哪个满足就显示哪个

7.v-else-if注意点
和v-else一样
-->

<!--这里就是MVVM中的View-->
<div id="app">
<!--    <p v-if="show">我是true</p>-->
<!--    <p v-if="hidden">我是false</p>-->
<!--    <p v-if="true">我是true</p>-->
<!--    <p v-if="false">我是false</p>-->
<!--    <p v-if="age >= 18">我是true</p>-->
<!--    <p v-if="age < 18">我是false</p>-->

<!--    <p v-if="age >= 18">成年人</p>-->
<!--    <p>中间的内容</p>-->
<!--    <p v-else>未成年人</p>-->

    <p v-if="score >= 80">优秀</p>
    <p v-else-if="score >= 60">良好</p>
    <p v-else>差</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            show: true,
            hidden: false,
            age: 17,
            score: 50
        }
    });
</script>
</body>
</html>
```

### v-show

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09-常用指令v-show</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.什么是v-show指令
v-show和v-if的能够一样都是条件渲染, 取值为true就显示, 取值为false就不显示

2.v-if和v-show区别
v-if: 只要取值为false就不会创建元素
v-show: 哪怕取值为false也会创建元素, 只是如果取值是false会设置元素的display为none

3.v-if和v-show应用场景
由于取值为false时v-if不会创建元素, 所以如果需要切换元素的显示和隐藏, 每次v-if都会创建和删除元素
由于取值为false时v-show会创建元素并设置display为none, 所有如果需要切换元素的显示和隐藏,
不会反复创建和删除, 只是修改display的值
所以: 如果企业开发中需要频繁切换元素显示隐藏, 那么推荐使用v-show, 否则使用v-if
-->

<!--这里就是MVVM中的View-->
<div id="app">
<!--    <p v-show="show">我是true</p>-->
<!--    <p v-show="hidden">我是false</p>-->
<!--    <p v-show="true">我是true</p>-->
<!--    <p v-show="false">我是false</p>-->
<!--    <p v-show="age >= 18">我是true</p>-->
<!--    <p v-show="age < 18">我是false</p>-->
    <p v-show="show">我是段落1</p>
    <p v-show="hidden">我是段落2</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            show: true,
            hidden: false,
            age: 18
        }
    });
</script>
</body>
</html>
```

### v-for

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>10-常用指令v-for</title>
    <script src="js/vue.js"></script>
  </head>
  <body>
    <!--
1.什么是v-for指令
相当于JS中的for in循环, 可以根据数据多次渲染元素

2.v-for特点
可以遍历 数组, 字符, 数字, 对象
-->

    <!--这里就是MVVM中的View-->
    <div id="app">
      <ul>
        <!-- <li v-for="(value, index) in list">{{index}}-&#45;&#45;{{value}}</li> -->
        <!-- <li v-for="(value, index) in 'abcdefg'">
          {{index}}-&#45;&#45;{{value}}
        </li> -->
        <!-- <li v-for="(value, index) in 6">{{index}}-&#45;&#45;{{value}}</li> -->
        <!-- <li v-for="(value, key) in obj">{{key}}---{{value}}</li> -->
      </ul>
    </div>
    <script>
      // 这里就是MVVM中的View Model
      let vue = new Vue({
        el: "#app",
        // 这里就是MVVM中的Model
        data: {
          list: ["张三", "李四", "王五", "赵六"],
          obj: {
            name: "lnj",
            age: 33,
            gender: "man",
            class: "知播渔",
          },
        },
      });
    </script>
  </body>
</html>
```

### v-bind(语法糖: :)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>10-常用指令v-bind</title>
    <script src="js/vue.js"></script>
  </head>
  <body>
    <!--
1.什么是v-bind指令
在企业开发中想要给"元素"绑定数据, 我们可以使用{{}}, v-text, v-html
但是如果想给"元素的属性"绑定数据, 就必须使用v-bind
所以v-bind的作用是专门用于给"元素的属性"绑定数据的

2.v-bind格式
v-bind:属性名称="绑定的数据"
:属性名称="绑定的数据"

3.v-bind特点
赋值的数据可以是任意一个合法的JS表达式
例如: :属性名称="age + 1"
-->

    <!--这里就是MVVM中的View-->
    <div id="app">
      <!--    <p>{{name}}</p>-->
      <!--    <p v-text="name"></p>-->
      <!--    <p v-html="name"></p>-->
      <!--注意点: 如果要给元素的属性绑定数据, 那么是不能够使用插值语法的-->
      <!-- <input type="text" value="{{name}}" /> -->
      <!--注意点: 虽然通过v-model可以将数据绑定到input标签的value属性上
                但是v-model是有局限性的, v-model只能用于input/textarea/select
                但是在企业开发中我们还可能需要给其它标签的属性绑定数据-->
      <!--    <input type="text" v-model="name">-->
      <!--    <input type="text" v-bind:value="name">-->
      <!--    <input type="text" :value="name">-->
      <input type="text" :value="age + 1" />
    </div>
    <script>
      // 这里就是MVVM中的View Model
      let vue = new Vue({
        el: "#app",
        // 这里就是MVVM中的Model
        data: {
          name: "知播渔666",
          age: 18,
        },
      });
    </script>
  </body>
</html>
```

### 绑定类名:class

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-常用指令-绑定类名</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .size{
            font-size: 100px;
        }
        .color{
            color: red;
        }
        .active{
            background: skyblue;
        }
    </style>
</head>
<body>
<!--
1.v-bind指令的作用
v-bind指令给"任意标签"的"任意属性"绑定数据
对于大部分的属性而言我们只需要直接赋值即可, 例如:value="name"
但是对于class和style属性而言, 它的格式比较特殊

2.通过v-bind绑定类名格式
:class="['需要绑定类名', ...]"

3.注意点:
3.1直接赋值一个类名(没有放到数组中)默认回去Model中查找
:class="需要绑定类名"
2.2数组中的类名没有用引号括起来也会去Model中查找
:class="[需要绑定类名]"
2.3数组的每一个元素都可以是一个三目运算符按需导入
:class="[flag?'active':'']"
2.4可以使用对象来替代数组中的三目运算符按需导入
:class="[{'active': true}]"
2.5绑定的类名太多可以将类名封装到Model中
obj: {
    'color': true,
    'size': true,
    'active': false,
}

4.绑定类名企业应用场景
从服务器动态获取样式后通过v-bind动态绑定类名
这样就可以让服务端来控制前端样式
常见场景: 618 双11等
-->

<!--这里就是MVVM中的View-->
<div id="app">
<!--    <p class="size color active">我是段落</p>-->
    <!--
    注意点:
    如果需要通过v-bind给class绑定类名, 那么不能直接赋值
    默认情况下v-bind会去Model中查找数据, 但是Model中没有对应的类名, 所以无效, 所以不能直接赋值
    -->
<!--    <p :class="size">我是段落</p>-->
    <!--
    注意点:
    如果想让v-bind去style中查找类名, 那么就必须把类名放到数组中
    但是放到数组中之后默认还是回去Model中查找
    -->
<!--    <p :class="[size]">我是段落</p>-->
    <!--
    注意点:
    将类名放到数组中之后, 还需要利用引号将类名括起来才会去style中查找
    -->
<!--    <p :class="['size', 'color', 'active']">我是段落</p>-->
    <!--
    注意点:
    如果是通过v-bind类绑定类名, 那么在绑定的时候可以编写一个三目运算符来实现按需绑定
    格式: 条件表达式 ? '需要绑定的类名' : ''
    -->
<!--    <p :class="['size', 'color', flag ? 'active' : '']">我是段落</p>-->
    <!--
    注意点:
    如果是通过v-bind类绑定类名, 那么在绑定的时候可以通过对象来决定是否需要绑定
    格式: {'需要绑定的类名' : 是否绑定}
    -->
<!--    <p :class="['size', 'color',{'active' : false}]">我是段落</p>-->
    <!--
    注意点:
    如果是通过v-bind类绑定类名, 那么还可以使用Model中的对象来替换数组
    -->
    <p :class="obj">我是段落</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            flag: false,
            obj:{
                'size': false,
                'color': false,
                'active': true,
            }
        }
    });
</script>
</body>
</html>
```

### 绑定样式:style

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13-常用指令-绑定样式</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.如何通过v-bind给style属性绑定数据
1.1将数据放到对象中
:style="{color:'red','font-size':'50px'}"
1.2将数据放到Model对象中
obj: {
    color: 'red',
    'font-size': '80px',
}

2.注意点
2.1如果属性名称包含-, 那么必须用引号括起来
2.2如果需要绑定Model中的多个对象, 可以放到一个数组中赋值
-->

<!--这里就是MVVM中的View-->
<div id="app">
<!--    <p style="color: red">我是段落</p>-->
    <!--
    注意点:
    和绑定类名一样, 默认情况下v-bind回去Model中查找, 找不到所以没有效果
    -->
<!--    <p :style="color: red">我是段落</p>-->
    <!--
    注意点:
    我们只需要将样式代码放到对象中赋值给style即可
    但是取值必须用引号括起来
    -->
<!--    <p :style="{color: 'red'}">我是段落</p>-->
    <!--
    注意点:
    如果样式的名称带-, 那么也必须用引号括起来才可以
    -->
<!--    <p :style="{color: 'red', 'font-size': '100px'}">我是段落</p>-->
<!--    <p :style="obj">我是段落</p>-->
    <!--
    注意点:
    如果Model中保存了多个样式的对象 ,想将多个对象都绑定给style, 那么可以将多个对象放到数组中赋值给style即可
    -->
    <p :style="[obj1, obj2]">我是段落</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            obj1:{
                "color": "blue",
                "font-size": "100px"
            },
            obj2: {
                "background-color": "red"
            }
        }
    });
</script>
</body>
</html>
```

### v-on(语法糖: @)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>14-常用指令-v-on</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.什么是v-on指令?
v-on指令专门用于给元素绑定监听事件

2.v-on指令格式
v-on:事件名称="回调函数名称"
@事件名称="回调函数名称"

3.v-on注意点:
v-on绑定的事件被触发之后, 会去Vue实例对象的methods中查找对应的回调函数
-->

<!--这里就是MVVM中的View-->
<div id="app">
<!--    <button onclick="alert('lnj')">我是按钮</button>-->
    <!--
    注意点:
    1.如果是通过v-on来绑定监听事件, 那么在指定事件名称的时候不需要写on
    2.如果是通过v-on来绑定监听事件, 那么在赋值的时候必须赋值一个回调函数的名称
    -->
<!--    <button v-on:click="alert('lnj')">我是按钮</button>-->
    <!--
    注意点:
    当绑定的事件被触发后, 会调用Vue实例的methods对象中对应的回调函数
    -->
<!--    <button v-on:click="myFn">我是按钮</button>-->
    <button @click="myFn">我是按钮</button>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
        },
        methods: {
            myFn(){
                alert('lnj')
            }
        }
    });
</script>
</body>
</html>
```

### v-on的修饰符

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>15-常用指令-v-on修饰符</title>
    <script src="js/vue.js"></script>
    <style>
      * {
        margin: 0;
        padding: 0;
      }
      .a {
        width: 300px;
        height: 300px;
        background: red;
      }
      .b {
        width: 200px;
        height: 200px;
        background: blue;
      }
      .c {
        width: 100px;
        height: 100px;
        background: green;
      }
    </style>
  </head>
  <body>
    <!--
1.v-on修饰符
在事件中有很多东西需要我们处理, 例如事件冒泡,事件捕获, 阻止默认行为等
那么在Vue中如何处理以上内容呢, 我们可以通过v-on修饰符来处理

2.常见修饰符
.once    - 只触发一次回调。
.prevent - 调用 event.preventDefault()。
.stop    - 调用 event.stopPropagation()。
.self    - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
.capture - 添加事件侦听器时使用 capture 模式。
-->

    <!--这里就是MVVM中的View-->
    <div id="app">
      <!--注意点: 默认情况下事件的回调函数可以反复的执行, 只要事件被触发就会执行-->
      <!--    <button v-on:click="myFn">我是按钮</button>-->
      <!--如果想让事件监听的回调函数只执行一次, 那么就可以使用.once修饰符-->
      <!--    <button v-on:click.once ="myFn">我是按钮</button>-->
      <!--如果想阻止元素的默认行为, 那么可以使用.prevent修饰符-->
      <!--    <a href="http://www.it666.com" v-on:click.prevent="myFn">我是A标签</a>-->
      <!--
    默认情况下载嵌套的元素中, 如果都监听了相同的事件, 那么会触发事件冒泡
    如果想阻止事件冒泡, 那么可以使用.stop修饰符
    -->
      <!--<div class="a" @click="myFn1">
        <div class="b" @click.stop="myFn2">
            <div class="c" @click="myFn3"></div>
        </div>
    </div>-->
      <!--
    如果想让回调只有当前元素触发事件的时候才执行, 那么就可以使用.self修饰符
    -->
      <!-- <div class="a" @click="myFn1">
        <div class="b" @click.self="myFn2">
          <div class="c" @click="myFn3"></div>
        </div>
      </div> -->
      <!--
    默认情况下是事件冒泡, 如果想变成事件捕获, 那么就需要使用.capture修饰符
    -->
      <!-- <div class="a" @click.capture="myFn1">
        <div class="b" @click.capture="myFn2">
            <div class="c" @click.capture="myFn3"></div>
        </div>
    </div> -->
    </div>
    <script>
      // 这里就是MVVM中的View Model
      let vue = new Vue({
        el: "#app",
        // 这里就是MVVM中的Model
        data: {},
        // 专门用于存储监听事件回调函数
        methods: {
          myFn() {
            alert("lnj");
          },
          myFn1() {
            console.log("爷爷");
          },
          myFn2() {
            console.log("爸爸");
          },
          myFn3() {
            console.log("儿子");
          },
        },
      });
    </script>
  </body>
</html>
```

### v-on的注意点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>16-常用指令-v-on注意点</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.v-on注意点
1.1绑定回调函数名称的时候, 后面可以写()也可以不写
v-on:click="myFn"
v-on:click="myFn()"

1.2可以给绑定的回调函数传递参数
v-on:click="myFn('lnj', 33)"

1.3如果在绑定的函数中需要用到data中的数据必须加上this
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <button @click="myFn('lnj', 33, $event)">我是按钮</button>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            gender: "man"
        },
        // 专门用于存储监听事件回调函数
        methods: {
            myFn(name, age, e){
                // alert('lnj');
                // console.log(name, age, e);
                console.log(this.gender);
            }
        }
    });
</script>
</body>
</html>
```

### v-on按键修饰符

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>17-常用指令-v-on按键修饰符</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.什么是按键修饰符
我们可以通过按键修饰符监听特定按键触发的事件
例如: 可以监听当前事件是否是回车触发的, 可以监听当前事件是否是ESC触发的等

2.按键修饰符分类
2.1系统预定义修饰符
2.2自定义修饰符
-->

<!--这里就是MVVM中的View-->
<div id="app">
<!--    <input type="text" @keyup.enter="myFn">-->
    <input type="text" @keyup.f2="myFn">
</div>
<script>
    Vue.config.keyCodes.f2 = 113;
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
        },
        // 专门用于存储监听事件回调函数
        methods: {
            myFn(){
                alert("lnj");
            }
        }
    });
</script>
</body>
</html>
```

### vue自定义全局指令

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>18-常用指令-自定义指令</title>
    <script src="js/vue.js"></script>
  </head>
  <body>
    <!--
1.自定义全局指令
在Vue中除了可以使用Vue内置的一些指令以外, 我们还可以自定义指令

2.自定义全局指令语法
vue.directive('自定义指令名称', {
    生命周期名称: function (el) {
        指令业务逻辑代码
    }
});

3.指令生命周期方法
自定义指令时一定要明确指令的业务逻辑代码更适合在哪个阶段执行
例如: 指令业务逻辑代码中没有用到元素事件, 那么可以在bind阶段执行
例如: 指令业务逻辑代码中用到了元素事件, 那么就需要在inserted阶段执行

4.自定义指令注意点
使用时需要加上v-, 而在自定义时不需要加上v-
-->

    <!--这里就是MVVM中的View-->
    <div id="app">
      <!--    <p v-color>我是段落</p>-->
      <input type="text" v-focus />
    </div>
    <script>
      /*
    directive方法接收两个参数
    第一个参数: 指令的名称
    第二个参数: 对象
    注意点: 在自定义指令的时候, 在指定指令名称的时候, 不需要写v-
    注意点: 指令可以在不同的生命周期阶段执行
    bind: 指令被绑定到元素上的时候执行
    inserted: 绑定指令的元素被添加到父元素上的时候执行
    * */
      Vue.directive("color", {
        // 这里的el就是被绑定指令的那个元素
        bind: function (el) {
          el.style.color = "red";
        },
      });
      Vue.directive("focus", {
        // 这里的el就是被绑定指令的那个元素
        inserted: function (el) {
          el.focus();
        },
      });
      // 这里就是MVVM中的View Model
      let vue = new Vue({
        el: "#app",
        // 这里就是MVVM中的Model
        data: {},
        // 专门用于存储监听事件回调函数
        methods: {},
      });
    </script>
  </body>
</html>
```

### 自定义指令传参

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>19-常用指令-自定义指令参数</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.自定义指令参数
在使用官方指令的时候我们可以给指令传参
例如: v-model="name"
在我们自定义的指令中我们也可以传递传递

2.获取自定义指令传递的参数
在执行自定义指令对应的方法的时候, 除了会传递el给我们, 还会传递一个对象给我们
这个对象中就保存了指令传递过来的参数
-->

<!--这里就是MVVM中的View-->
<div id="app">
<!--    <p v-color="'blue'">我是段落</p>-->
    <p v-color="curColor">我是段落</p>
</div>
<script>
    Vue.directive("color", {
        // 这里的el就是被绑定指令的那个元素
        bind: function (el, obj) {
            // el.style.color = "red";
            el.style.color = obj.value;
        }
    });
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            curColor: 'green'
        },
        // 专门用于存储监听事件回调函数
        methods: {
        }
    });
</script>
</body>
</html>
```

### directives自定义局部指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>21-常用指令-自定义局部指令</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.自定义全局指令的特点
在任何一个Vue实例控制的区域中都可以使用

2.自定义局部指令的特点
只能在自定义的那个Vue实例中使用

3.如何自定义一个局部指令
给创建Vue实例时传递的对象添加
directives: {
    // key: 指令名称
    // value: 对象
    'color': {
        bind: function (el, obj) {
            el.style.color = obj.value;
        }
    }
}
-->

<!--这里就是MVVM中的View-->
<div id="app1">
    <p v-color="'blue'">我是段落</p>
</div>
<div id="app2">
    <p v-color="'red'">我是段落</p>
</div>
<script>
    /*
    Vue.directive("color", {
        // 这里的el就是被绑定指令的那个元素
        bind: function (el, obj) {
            el.style.color = obj.value;
        }
    });
     */
    // 这里就是MVVM中的View Model
    let vue1 = new Vue({
        el: '#app1',
        // 这里就是MVVM中的Model
        data: {},
        // 专门用于存储监听事件回调函数
        methods: {}
    });
    // 这里就是MVVM中的View Model
    let vue2 = new Vue({
        el: '#app2',
        // 这里就是MVVM中的Model
        data: {},
        // 专门用于存储监听事件回调函数
        methods: {},
        // 专门用于定义局部指令的
        directives: {
            "color": {
                // 这里的el就是被绑定指令的那个元素
                bind: function (el, obj) {
                    el.style.color = obj.value;
                }
            }
        }
    });
</script>
</body>
</html>
```

### computed计算属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>22-Vue-计算属性</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.插值语法特点
可以在{{}}中编写合法的JavaScript表达式

2.在插值语法中编写JavaScript表达式缺点
2.1没有代码提示
2.2语句过于复杂不利于我们维护

3.如何解决?
对于任何复杂逻辑，你都应当使用计算属性
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <p>{{name}}</p>
    <p>{{age + 1}}</p>
    <p>{{msg.split("").reverse().join("")}}</p>
    <!--
    注意点:
    虽然在定义计算属性的时候是通过一个函数返回的数据
    但是在使用计算属性的时候不能在计算属性名称后面加上()
    因为它是一个属性不是一个函数(方法)
    -->
    <p>{{msg2}}</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            name: "lnj",
            age: 18,
            msg: "abcdef"
        },
        // 专门用于存储监听事件回调函数
        methods: {},
        // 专门用于定义计算属性的
        computed: {
            msg2: function () {
                let res = "abcdef".split("").reverse().join("");
                return res;
            }
        }
    });
</script>
</body>
</html>
```

### 计算属性和函数的区别

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>23-Vue-计算属性和函数</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.计算属性和函数
通过计算属性我们能拿到处理后的数据, 但是通过函数我们也能拿到处理后的数据
那么计算属性和函数有什么区别呢?
2.1函数"不会"将计算的结果缓存起来, 每一次访问都会重新求值
2.2计算属性"会"将计算的结果缓存起来, 只要数据没有发生变化, 就不会重新求值

2.计算属性应用场景
计算属性:比较适合用于计算不会频繁发生变化的的数据
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <!--<p>{{msg1()}}</p>
    <p>{{msg1()}}</p>
    <p>{{msg1()}}</p>-->
    <p>{{msg2}}</p>
    <p>{{msg2}}</p>
    <p>{{msg2}}</p>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
        },
        // 专门用于存储监听事件回调函数
        methods: {
            /*
            函数的特点: 每次调用都会执行
            * */
            msg1(){
                console.log("msg1函数被执行了");
                let res = "abcdef".split("").reverse().join("");
                return res;
            }
        },
        // 专门用于定义计算属性的
        computed: {
            /*
            计算属性的特点: 只要返回的结果没有发生变化, 那么计算属性就只会被执行一次
            计算属性的应用场景: 由于计算属性会将返回的结果缓存起来
                                所以如果返回的数据不经常发生变化,
                                那么使用计算属性的性能会比使用函数的性能高
            * */
            msg2: function () {
                console.log("msg2计算属性被执行了");
                let res = "abcdef".split("").reverse().join("");
                return res;
            }
        }
    });
</script>
</body>
</html>
```

### 自定义全局的过滤器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>24-Vue-自定义全局过滤器</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.什么是过滤器?
过滤器和函数和计算属性一样都是用来处理数据的
但是过滤器一般用于格式化插入的文本数据

2.如何自定义全局过滤器
Vue.filter("过滤器名称", 过滤器处理函数):

3.如何使用全局过滤器
{{msg | 过滤器名称}}
:value="msg | 过滤器名称"

4.过滤器注意点
4.1只能在插值语法和v-bind中使用
4.2过滤器可以连续使用
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <!--Vue会把name交给指定的过滤器处理之后, 再把处理之后的结果插入到指定的元素中-->
    <p>{{name | formartStr1 | formartStr2}}</p>
</div>
<script>
    /*
    如何自定义一个全局过滤器
    通过Vue.filter();
    filter方法接收两个参数
    第一个参数: 过滤器名称
    第二个参数: 处理数据的函数
    注意点: 默认情况下处理数据的函数接收一个参数, 就是当前要被处理的数据
    * */
    Vue.filter("formartStr1", function (value) {
        // console.log(value);
        value = value.replace(/学院/g, "大学");
        console.log(value);
        return value;
    });
    Vue.filter("formartStr2", function (value) {
        // console.log(value);
        value = value.replace(/大学/g, "幼儿园");
        console.log(value);
        return value;
    });
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            name: "知播渔学院, 指趣学院, 前端学院, 区块链学院"
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### filters自定义局部过滤器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>24-Vue-自定义全局过滤器</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.自定义全局过滤器的特点
在任何一个Vue实例控制的区域中都可以使用

2.自定义局部过滤器的特点
只能在自定义的那个Vue实例中使用

3.如何自定义一个局部指令
给创建Vue实例时传递的对象添加
filters: {
    // key: 过滤器名称
    // value: 过滤器处理函数
    'formartStr': function (value) {}
}
-->
<!--这里就是MVVM中的View-->
<div id="app1">
    <p>{{name | formartStr}}</p>
</div>
<div id="app2">
    <p>{{name | formartStr}}</p>
</div>
<script>
    /*
    Vue.filter("formartStr", function (value) {
        // console.log(value);
        value = value.replace(/学院/g, "大学");
        // console.log(value);
        return value;
    });
    */
    // 这里就是MVVM中的View Model
    let vue1 = new Vue({
        el: '#app1',
        // 这里就是MVVM中的Model
        data: {
            name: "知播渔学院, 指趣学院, 前端学院, 区块链学院"
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
    // 这里就是MVVM中的View Model
    let vue2 = new Vue({
        el: '#app2',
        // 这里就是MVVM中的Model
        data: {
            name: "知播渔学院, 指趣学院, 前端学院, 区块链学院"
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        },
        // 专门用于定义局部过滤器的
        filters: {
            "formartStr": function (value) {
                // console.log(value);
                value = value.replace(/学院/g, "大学");
                // console.log(value);
                return value;
            }
        }
    });
</script>
</body>
</html>
```

### 过滤器传参和String的原型方法padStart

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>24-Vue-自定义全局过滤器</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
需求: 利用过滤器对时间进行格式化
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <p>{{time | dateFormart("yyyy-MM-dd")}}</p>
</div>
<script>
    /*
    注意点: 在使用过滤器的时候, 可以在过滤器名称后面加上()
            如果给过滤器的名称后面加上了(), 那么就可以给过滤器的函数传递参数
    * */
    Vue.filter("dateFormart", function (value, fmStr) {
        // console.log(fmStr);
        let date = new Date(value);
        let year = date.getFullYear();
        let month = date.getMonth() + 1 + "";
        let day = date.getDate() + "";
        let hour = date.getHours() + "";
        let minute = date.getMinutes() + "";
        let second = date.getSeconds() + "";
        if(fmStr && fmStr === "yyyy-MM-dd"){
            return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
        }
        return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")} ${hour.padStart(2, "0")}:${minute.padStart(2, "0")}:${second.padStart(2, "0")}`;
    });
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            time: Date.now()
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

## vue过渡动画

### trainsition

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>28-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 200px;
            height: 200px;
            background: red;
        }
        .v-enter{
            opacity: 0;
        }
        .v-enter-to{
            opacity: 1;
        }
        .v-enter-active{
            transition: all 3s;
        }
        .v-leave{
            opacity: 1;
        }
        .v-leave-to{
            opacity: 0;
        }
        .v-leave-active{
            transition: all 3s;
        }
    </style>
</head>
<body>
<!--
1.如何给Vue控制的元素添加过渡动画
1.1将需要执行动画的元素放到transition组件中
1.2当transition组件中的元素显示时会自动查找.v-enter/.v-enter-active/.v-enter-to类名
   当transition组件中的元素隐藏时会自动查找.v-leave/ .v-leave-active/.v-leave-to类名
1.3我们只需要在.v-enter和.v-leave-to中指定动画动画开始的状态
             在.v-enter-active和.v-leave-active中指定动画执行的状态
             即可完成过渡动画
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <button @click="toggle">我是按钮</button>
    <transition>
        <div class="box" v-show="isShow"></div>
    </transition>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            isShow: false
        },
        // 专门用于存储监听事件回调函数
        methods: {
            toggle(){
                this.isShow = !this.isShow;
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### 给多个组件添加不同的动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>29-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 200px;
            height: 200px;
            background: red;
        }
        .one-enter{
            opacity: 0;
        }
        .one-enter-to{
            opacity: 1;
            margin-left: 500px;
        }
        .one-enter-active{
            transition: all 3s;
        }
        .two-enter{
            opacity: 0;
        }
        .two-enter-to{
            opacity: 1;
            margin-top: 500px;
        }
        .two-enter-active{
            transition: all 3s;
        }
    </style>
</head>
<body>
<!--
1.transition注意点:
transition中只能放一个元素, 多个元素无效
如果想给多个元素添加过渡动画, 那么就必须创建多个transition组件

2.初始动画设置
默认情况下第一次进入的时候没没有动画的
如果想一进来就有动画, 我们可以通过给transition添加appear属性的方式
告诉Vue第一次进入就需要显示动画

3.如何给多个不同的元素指定不同的动画
如果有多个不同的元素需要执行不同的过渡动画,那么我们可以通过给transition指定name的方式
来指定"进入之前/进入之后/进入过程中, 离开之前/离开之后/离开过程中"对应的类名
来实现不同的元素执行不同的过渡动画
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <button @click="toggle">我是按钮</button>
    <transition appear name="one">
        <div class="box" v-show="isShow"></div>
<!--        <div class="box" v-show="isShow"></div>-->
    </transition>
    <transition appear name="two">
        <div class="box" v-show="isShow"></div>
    </transition>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            isShow: true
        },
        // 专门用于存储监听事件回调函数
        methods: {
            toggle(){
                this.isShow = !this.isShow;
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### 通过js钩子执行动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>30-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 200px;
            height: 200px;
            background: red;
        }
        .v-enter{
            opacity: 0;
        }
        .v-enter-to{
            opacity: 1;
            margin-left: 500px;
        }
        .v-enter-active{
            transition: all 3s;
        }
    </style>
</head>
<body>
<!--
1.当前过渡存在的问题
通过transition+类名的方式确实能够实现过渡效果
但是实现的过渡效果并不能保存动画之后的状态
因为Vue内部的实现是在过程中动态绑定类名, 过程完成之后删除类名
正式因为删除了类名, 所以不能保存最终的效果

2.在Vue中如何保存过渡最终的效果
通过Vue提供的JS钩子来实现过渡动画
v-on:before-enter="beforeEnter"  进入动画之前
v-on:enter="enter"  进入动画执行过程中
v-on:after-enter="afterEnter"  进入动画完成之后
v-on:enter-cancelled="enterCancelled"  进入动画被取消

v-on:before-leave="beforeLeave" 离开动画之前
v-on:leave="leave"  离开动画执行过程中
v-on:after-leave="afterLeave" 离开动画完成之后
v-on:leave-cancelled="leaveCancelled" 离开动画被取消

3.JS钩子实现过渡注意点
3.1在动画过程中必须写上el.offsetWidth或者el.offsetHeight
3.2在enter和leave方法中必须调用done方法, 否则after-enter和after-leave不会执行
3.3需要需要添加初始动画, 那么需要把done方法包裹到setTimeout方法中调用
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <button @click="toggle">我是按钮</button>
    <!--
    注意点: 虽然我们是通过JS钩子函数来实现过渡动画
            但是默认Vue还是回去查找类名, 所以为了不让Vue去查找类名
            可以给transition添加v-bind:css="false"
    -->
    <transition appear
                v-bind:css="false"
                v-on:before-enter="beforeEnter"
                v-on:enter="enter"
                v-on:after-enter="afterEnter">
        <div class="box" v-show="isShow"></div>
    </transition>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            isShow: true
        },
        // 专门用于存储监听事件回调函数
        methods: {
            toggle(){
                this.isShow = !this.isShow;
            },
            beforeEnter(el){
                // 进入动画开始之前
                console.log("beforeEnter");
                el.style.opacity = "0";
            },
            enter(el, done){
                // 进入动画执行过程中
                console.log("enter");
                /*
                注意点: 如果是通过JS钩子来实现过渡动画
                        那么必须在动画执行过程中的回调函数中写上
                        el.offsetWidth / el.offsetHeight
                * */
                // el.offsetWidth;
                el.offsetHeight;
                el.style.transition = "all 3s";
                /*
                注意点: 动画执行完毕之后一定要调用done回调函数
                        否则后续的afterEnter钩子函数不会被执行
                * */
                // done();
                /*
                注意点: 如果想让元素一进来就有动画, 那么最好延迟以下再调用done方法
                * */
                setTimeout(function () {
                    done();
                }, 0);
            },
            afterEnter(el){
                // 进入动画执行完毕之后
                console.log("afterEnter");
                el.style.opacity = "1";
                el.style.marginLeft = "500px";
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### 自定义类名动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>32-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 200px;
            height: 200px;
            background: red;
        }
        .a{
            opacity: 0;
        }
        .b{
            opacity: 1;
            margin-left: 500px;
        }
        .c{
            transition: all 3s;
        }
    </style>
</head>
<body>
<!--
1.自定义类名动画
在Vue中除了可以使用 默认类名(v-xxx)来指定过渡动画
       除了可以使用 自定义类名前缀(yyy-xx)来指定过渡动画(transition name="yyy")
       除了可以使用 JS钩子函数来指定过渡动画以外
还可以使用自定义类名的方式来指定过渡动画

enter-class  // 进入动画开始之前
enter-active-class // 进入动画执行过程中
enter-to-class // 进入动画执行完毕之后
leave-class  // 离开动画开始之前
leave-active-class // 离开动画执行过程中
leave-to-class // 离开动画执行完毕之后
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <button @click="toggle">我是按钮</button>
    <transition appear
                enter-class="a"
                enter-active-class="c"
                enter-to-class="b">
        <div class="box" v-show="isShow"></div>
    </transition>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            isShow: true
        },
        // 专门用于存储监听事件回调函数
        methods: {
            toggle(){
                this.isShow = !this.isShow;
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### Animate动画库的使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>33-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 200px;
            height: 200px;
            background: red;
        }
    </style>
    <link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css">
</head>
<body>
<!--
1.配合Animate.css实现过渡动画
1.1导入Animate.css库
1.2在执行过程中的属性上绑定需要的类名
-->

<!--这里就是MVVM中的View-->
<div id="app">
    <button @click="toggle">我是按钮</button>
    <transition appear
                enter-class=""
                enter-active-class="animated bounceInRight"
                enter-to-class="">
        <div class="box" v-show="isShow"></div>
    </transition>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            isShow: true
        },
        // 专门用于存储监听事件回调函数
        methods: {
            toggle(){
                this.isShow = !this.isShow;
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### v-for中的key作用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>35-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .v-enter{
            opacity: 0;
        }
        .v-enter-to{
            opacity: 1;
        }
        .v-enter-active{
            transition: all 3s;
        }
        .v-leave{
            opacity: 1;
        }
        .v-leave-to{
            opacity: 0;
        }
        .v-leave-active{
            transition: all 3s;
        }
    </style>
</head>
<body>
<!--
1.如何同时给多个元素添加过渡动画
通过transition可以给单个元素添加过渡动画
如果想给多个元素添加过渡动画, 那么就必须通过transition-group来添加

transition-group和transition的用法一致, 只是一个是给单个元素添加动画, 一个是给多个元素添加动画而已
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <form>
        <input type="text" v-model="name">
        <input type="submit" value="添加" @click.prevent="add">
    </form>
    <ul>
        <transition-group appear>
        <li v-for="(person,index) in persons" :key="person.id" @click="del(index)">
            <input type="checkbox">
            <span>{{index}} --- {{person.name}}</span>
        </li>
        </transition-group>
    </ul>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            persons: [
                {name: "zs", id: 1},
                {name: "ls", id: 2},
                {name: "ww", id: 3}
                ],
            name: ""
        },
        // 专门用于存储监听事件回调函数
        methods: {
            add(){
                let lastPerson = this.persons[this.persons.length - 1];
                let newPerson = {name: this.name, id: lastPerson.id + 1};
                // this.persons.push(newPerson);
                this.persons.unshift(newPerson);
                this.name = "";
            },
            del(index){
                this.persons.splice(index, 1);
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### transition-group的使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>35-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .v-enter{
            opacity: 0;
        }
        .v-enter-to{
            opacity: 1;
        }
        .v-enter-active{
            transition: all 3s;
        }
        .v-leave{
            opacity: 1;
        }
        .v-leave-to{
            opacity: 0;
        }
        .v-leave-active{
            transition: all 3s;
        }
    </style>
</head>
<body>
<!--
1.如何同时给多个元素添加过渡动画
通过transition可以给单个元素添加过渡动画
如果想给多个元素添加过渡动画, 那么就必须通过transition-group来添加

transition-group和transition的用法一致, 只是一个是给单个元素添加动画, 一个是给多个元素添加动画而已
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <form>
        <input type="text" v-model="name">
        <input type="submit" value="添加" @click.prevent="add">
    </form>
    <ul>
        <transition-group appear>
        <li v-for="(person,index) in persons" :key="person.id" @click="del(index)">
            <input type="checkbox">
            <span>{{index}} --- {{person.name}}</span>
        </li>
        </transition-group>
    </ul>
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            persons: [
                {name: "zs", id: 1},
                {name: "ls", id: 2},
                {name: "ww", id: 3}
                ],
            name: ""
        },
        // 专门用于存储监听事件回调函数
        methods: {
            add(){
                let lastPerson = this.persons[this.persons.length - 1];
                let newPerson = {name: this.name, id: lastPerson.id + 1};
                // this.persons.push(newPerson);
                this.persons.unshift(newPerson);
                this.name = "";
            },
            del(index){
                this.persons.splice(index, 1);
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### transition-group的注意点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>36-Vue-过渡动画</title>
    <script src="js/vue.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .v-enter{
            opacity: 0;
        }
        .v-enter-to{
            opacity: 1;
        }
        .v-enter-active{
            transition: all 3s;
        }
        .v-leave{
            opacity: 1;
        }
        .v-leave-to{
            opacity: 0;
        }
        .v-leave-active{
            transition: all 3s;
        }
    </style>
</head>
<body>
<!--
1.transition-group注意点:
默认情况下transition-group会将动画的元素放到span标签中
我们可以通过tag属性来指定将动画元素放到什么标签中

2.transition-group动画混乱问题
一般情况下组动画出现动画混乱都是因为v-for就地复用导致的
我们只需要保证所有数据key永远是唯一的即可
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <form>
        <input type="text" v-model="name">
        <input type="submit" value="添加" @click.prevent="add">
    </form>
<!--    <ul>-->
        <transition-group appear tag="ul">
        <li v-for="(person,index) in persons" :key="person.id" @click="del(index)">
            <input type="checkbox">
            <span>{{index}} --- {{person.name}}</span>
        </li>
        </transition-group>
<!--    </ul>-->
</div>
<script>
    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
            persons: [
                {name: "zs", id: 1},
                {name: "ls", id: 2},
                {name: "ww", id: 3}
                ],
            name: "",
            id: 3
        },
        // 专门用于存储监听事件回调函数
        methods: {
            add(){
                this.id++;
                // let lastPerson = this.persons[this.persons.length - 1];
                let newPerson = {name: this.name, id: this.id};
                // this.persons.push(newPerson);
                this.persons.unshift(newPerson);
                this.name = "";
            },
            del(index){
                this.persons.splice(index, 1);
            }
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```
## 组件化开发

### 自定义全局组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>37-Vue组件-自定义全局组件</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
Vue两大核心: 1.数据驱动界面改变 2.组件化
1.什么是组件? 什么是组件化?
1.1在前端开发中组件就是把一个很大的界面拆分为多个小的界面, 每一个小的界面就是一个组件
1.2将大界面拆分成小界面就是组件化

2.组件化的好处
2.1可以简化Vue实例的代码
2.2可以提高复用性

3.Vue中如何创建组件?
3.1创建组件构造器
3.2注册已经创建好的组件
3.3使用注册好的组件
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <!--// 3.3使用注册好的组件-->
    <abc></abc>
</div>
<script>
    // 3.1创建组件构造器
    let Profile = Vue.extend({
        // 注意点: 在创建组件指定组件的模板的时候, 模板只能有一个根元素
        template: `
           <div>
                <img src="images/fm.jpg"/>
                <p>我是描述信息</p>
            </div>
        `
    });
    // 3.2注册已经创建好的组件
    // 第一个参数: 指定注册的组件的名称
    // 第二个参数: 传入已经创建好的组件构造器
    Vue.component("abc", Profile );

    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### 自定义全局组件的注意点

- **首先vue.component(组件名，组件对象)可以代替vue.extend**
- **script标签中可以定义组件，加上id属性**
- **template标签加上id属性才是vue官方的方式**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>38-Vue组件-自定义全局组件</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.创建组件的其它方式
1.1在注册组件的时候, 除了传入一个组件构造器以外, 还可以直接传入一个对象
1.2在编写组件模板的时候, 除了可以在字符串模板中编写以外, 还可以像过去的art-template一样在script中编写
1.3在编写组件模板的时候, 除了可以在script中编写以外, vue还专门提供了一个编写模板的标签template
-->
<!--这里就是MVVM中的View-->
<div id="app">
    <!--// 3.3使用注册好的组件-->
    <abc></abc>
</div>
<!--
<script id="info" type="text/html">
    <div>
        <img src="images/fm.jpg"/>
        <p>我是描述信息</p>
    </div>
</script>
-->
<template id="info">
    <div>
        <img src="images/fm.jpg"/>
        <p>我是描述信息</p>
    </div>
</template>
<script>
    // 3.1创建组件构造器
    /*
    let Profile = Vue.extend({
        // 注意点: 在创建组件指定组件的模板的时候, 模板只能有一个根元素
        template: `
           <div>
                <img src="images/fm.jpg"/>
                <p>我是描述信息</p>
            </div>
        `
    });
     */
    /*
    let obj = {
        // 注意点: 在创建组件指定组件的模板的时候, 模板只能有一个根元素
        template: `
           <div>
                <img src="images/fm.jpg"/>
                <p>我是描述信息</p>
            </div>
        `
    };
     */
    // 3.2注册已经创建好的组件
    // 第一个参数: 指定注册的组件的名称
    // 第二个参数: 传入已经创建好的组件构造器
    // Vue.component("abc", Profile );
    // Vue.component("abc", obj );
    /*
    Vue.component("abc", {
        // 注意点: 在创建组件指定组件的模板的时候, 模板只能有一个根元素
        template: `
           <div>
                <img src="images/fm.jpg"/>
                <p>我是描述信息</p>
            </div>
        `
    });
     */
    Vue.component("abc", {
        // 注意点: 在创建组件指定组件的模板的时候, 模板只能有一个根元素
        template: "#info"
    });

    // 这里就是MVVM中的View Model
    let vue = new Vue({
        el: '#app',
        // 这里就是MVVM中的Model
        data: {
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
</script>
</body>
</html>
```

### 自定义局部组件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>39-Vue组件-自定义局部组件</title>
    <script src="js/vue.js"></script>
</head>
<body>
<!--
1.自定义全局组件特点
在任何一个Vue实例控制的区域中都可以使用

2.自定义局部组件特点
只能在自定义的那个Vue实例控制的区域中可以使用

3.如何自定义一个局部组件
在vue实例中新增components: {}
在{}中通过key/vue形式注册组件
components:{
   abc: {}
}
-->
<!--这里就是MVVM中的View-->
<div id="app1">
    <abc></abc>
</div>
<div id="app2">
    <abc></abc>
</div>
<template id="info">
    <div>
        <img src="images/fm.jpg"/>
        <p>我是描述信息</p>
    </div>
</template>
<script>
    /*
    // 自定义全局组件
    Vue.component("abc", {
        // 注意点: 在创建组件指定组件的模板的时候, 模板只能有一个根元素
        template: "#info"
    });
     */
    // 这里就是MVVM中的View Model
    let vue1 = new Vue({
        el: '#app1',
        // 这里就是MVVM中的Model
        data: {
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        }
    });
    // 这里就是MVVM中的View Model
    let vue2 = new Vue({
        el: '#app2',
        // 这里就是MVVM中的Model
        data: {
        },
        // 专门用于存储监听事件回调函数
        methods: {
        },
        // 专门用于定义计算属性的
        computed: {
        },
        // 专门用于定义局部组件的
        components: {
            "abc": {
                // 注意点: 在创建组件指定组件的模板的时候, 模板只能有一个根元素
                template: "#info"
            }
        }
    });
</script>
</body>
</html>
```

