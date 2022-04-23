# node



## 给node传递参数

**在C/C++程序中的main函数中，实际上可以获取到两个参数：**

**argc：argument counter的缩写，传递参数的个数；**

**argv：argument vector的缩写，传入的具体参数。**

```js
console.log(process.argv[2]);
console.log(process.argv[3]);

process.argv.forEach(item => {
  console.log(item);
})

```



## node的输出

### console.log

最常用的输入内容的方式：console.log

### console.clear

清空控制台：console.clear

### console.trace

打印函数的调用栈：console.trace



## 常见的全局对象

![image-20220110193048464](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220110193048464.png)

### 特殊的全局对象

**这些全局对象可以在模块中任意使用，但是在命令行交互中是不可以使用的**

**包括：dirname、filename、exports、module、require()**

**__dirname：获取当前文件所在的路径：**

​	**注意：不包括后面的文件名**

**__filename：获取当前文件所在的路径和文件名称：**

​	**注意：包括后面的文件名称**

### 常见的全局对象

**process对象：process提供了Node进程中相关的信息：**

- **比如Node的运行环境、参数信息等；**

- **后面在项目中，我也会讲解，如何将一些环境变量读取到process的env中；**

**console对象：提供了简单的调试控制台**

**定时器函数：在Node中使用定时器有好几种方式：**

- **setTimeout(callback, delay[, ...args])：callback在delay毫秒后执行一次；**

- **setInterval(callback, delay[, ...args])：callback每delay毫秒重复执行一次；**

- **setImmediate(callback[, ...args])：callbackI/ O事件后的回调的“立即”执行；**

- **process.nextTick(callback[, ...args])：添加到下一次tick队列中；**

**global对象:**

**global是一个全局对象，事实上前端我们提到的process、console、setTimeout等都有被放到global中**

```js
console.log(global);

var name = "coderwhy";
console.log(name);
console.log(global.name);

console.log(global.process);
```



## js模块化

**CommonJS和Node**

**我们需要知道CommonJS是一个规范，最初提出来是在浏览器以外的地方使用，并且当时被命名为ServerJS，后来为了体现它的广泛性，修改为CommonJS，平时我们也会简称为CJS。**

**所以，Node中对CommonJS进行了支持和实现，让我们在开发node的过程中可以方便的进行模块化开发：**

**在Node中每一个js文件都是一个单独的模块**

**这个模块中包括CommonJS规范的核心变量：exports、module.exports、require**

**我们可以使用这些变量来方便的进行模块化开发**

### 没有模块化

- index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
  <script src="./bar.js"></script>
  <script src="./foo.js"></script>
  <script src="./baz.js"></script>
</body>
</html>
```

- bar.js

```js
// 1000行

var moduleBar = (function () {
  var name = "小明";
  var age = 18;

  console.log(name);

  return {
    name,
    age
  }
})()

```

- baz.js

```js
// 1000行
console.log(moduleBar.name);
console.log(moduleBar.age);
```

- foo.js

```js
(function() {
  var name = "小丽";

  console.log(name);
})();
```

### commonjs

#### exports导出

**exports是一个对象，我们可以在这个对象中添加很多个属性，添加的属性会导出**

```js
exports.name = name;
exports.age = age
exports.sayHello = sayHello
```

**另外一个文件中可以导入**

```js
const bar = require('./bar.js')
```

**上面这行完成了什么操作呢？理解下面这句话，Node中的模块化一目了然**

**意味着main中的bar变量等于exports对象；**

**也就是require通过各种查找方式，最终找到了exports这个对象；**

**并且将这个exports对象赋值给了bar变量；**

**bar变量就是exports对象了；**

![image-20220110195959293](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220110195959293.png)

#### module.exports

**但是Node中我们经常导出东西的时候，又是通过module.exports导出的：**

**module.exports和exports有什么关系或者区别呢**

**我们追根溯源，通过维基百科中对CommonJS规范的解析：**

- **CommonJS中是没有module.exports的概念的**

- **但是为了实现模块的导出，Node中使用的是Module的类，每一个模块都是Module的一个实例，也就是module**

- **所以在Node中真正用于导出的其实根本不是exports，而是module.exports**

- **因为module才是导出的真正实现者**

**但是，为什么exports也可以导出呢**

- **这是因为module对象的exports属性是exports对象的一个引用**

- **也就是说module.exports = exports = main中的bar**

![image-20220110200425212](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220110200425212.png)

#### require的细节

**这里我总结比较常见的查找规则：导入格式如下：require(X)**

- **情况一：X是一个核心模块，比如path、http**

- **情况二：X是以./ 或../ 或/（根目录）开头的**

  - **第一步：将X当做一个文件在对应的目录下查找；**
  - **如果没有后缀名，会按照如下顺序：**
    - **直接查找文件X**
    - **查找X.js文件**
    - **查找X.json文件**
    - **查找X.node文件**
  - **第二步：没有找到对应的文件，将X作为一个目录**
  - **查找目录下面的index文件**
    - **查找X/index.js文件**
    - **查找X/index.json文件**
    - **查找X/index.node文件**

- **情况三：直接是一个X（没有路径），并且X不是一个核心模块**

  **Users/coderwhy/Desktop/Node/TestCode/04_learn_node/05_javascript-module/02_commonjs/main.js中编写require('why’)**

![image-20220110202317076](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220110202317076.png)

### 模块的加载过程

**结论一：模块在被第一次引入时，模块中的js代码会被运行一次**

**结论二：模块被多次引入时，会缓存，最终只加载（运行）一次**

- **这是因为每个模块对象module都有一个属性：loaded。**
- **为false表示还没有加载，为true表示已经加载**

**结论三：如果有循环引入，那么加载顺序是什么**

![image-20220110202818190](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220110202818190.png)

**Node采用的是深度优先算法：main -> aaa-> ccc -> ddd-> eee->bbb**

### commonjs的缺点

**CommonJS加载模块是同步的：**

- **同步的意味着只有等到对应的模块加载完毕，当前模块中的内容才能被运行；**

- **这个在服务器不会有什么问题，因为服务器加载的js文件都是本地文件，加载速度非常快**

**如果将它应用于浏览器呢？**

- **浏览器加载js文件需要先从服务器将文件下载下来，之后在加载运行；**
- **那么采用同步的就意味着后续的js代码都无法正常运行，即使是一些简单的DOM操作；**

**所以在浏览器中，我们通常不使用CommonJS规范：**

- **当然在webpack中使用CommonJS是另外一回事；**
- **因为它会将我们的代码转成浏览器可以直接执行的代码**

**在早期为了可以在浏览器中使用模块化，通常会采用AMD或CMD：**

- **但是目前一方面现代的浏览器已经支持ES Modules，另一方面借助于webpack等工具可以实现对CommonJS或者ES Module代码的转换；**
- **AMD和CMD已经使用非常少了，所以这里我们进行简单的演练**

### AMD

- index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
  <script src="./lib/require.js" data-main="./index.js"></script>
</body>
</html>
```

- bar.js

```js
define(function() {
  const name = "coderwhy";
  const age = 18;
  const sayHello = function(name) {
    console.log("你好" + name);
  }

  return {
    name,
    age,
    sayHello
  }
});
```

- foo.js

```js
define(['bar'], function(bar) {
  console.log(bar.name);
  console.log(bar.age);
  bar.sayHello("kobe");
});

```

- index.js

```js
(function() {
  require.config({
    baseUrl: '',
    paths: {
      "bar": "./modules/bar",
      "foo": "./modules/foo"
    }
  })

  require(['foo'], function(foo) {
    
  });
})();

```

### CMD

**CMD规范也是应用于浏览器的一种模块化规范：**

- **CMD是Common Module Definition（通用模块定义）的缩写；**
- **它也采用了异步加载模块，但是它将CommonJS的优点吸收了过来；**
- **但是目前CMD使用也非常少了；**

**CMD也有自己比较优秀的实现方案：**

- **SeaJS**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
  <script src="./lib/sea.js"></script>
  <script>
    seajs.use('./index.js');
  </script>
</body>
</html>
```

- foo.js

```js
define(function(require, exports, module) {
  const name = "李银河";
  const age = 20;
  const sayHello = function(name) {
    console.log("你好" + name);
  }

  module.exports = {
    name: name,
    age: age,
    sayHello: sayHello
  }
});
```

- index.js

```js
define(function(require, exports, module) {
  const foo = require('./modules/foo');

  console.log(foo.name);
  console.log(foo.age);
  foo.sayHello("王小波");
})
```

### ESModule

**JavaScript没有模块化一直是它的痛点，所以才会产生我们前面学习的社区规范：CommonJS、AMD、CMD等，所以在ES推出自己的模块化系统时，大家也是兴奋异常。**

**ES Module和CommonJS的模块化有一些不同之处：**

- **一方面它使用了import和export关键字**

- **另一方面它采用编译期的静态分析，并且也加入了动态引用的方式**

**ES Module模块采用export和import关键字来实现模块化**

- **export负责将模块内的内容导出**
- **import负责从其他模块导入内容**

**了解：采用ES Module将自动采用严格模式：use strict**

#### 导出

**export关键字**

- **方式一：在语句声明的前面直接加上export关键字**
- **方式二：将所有需要导出的标识符，放到export后面的{}中**
  - **注意：这里的{}里面不是ES6的对象字面量的增强写法，{}也不是表示一个对象的；**
  - **所以：export {name: name}，是错误的写法；**
- **方式三：导出时给标识符起一个别名**

```js
const name = "why";
const age = 18;
const sayHello = function(name) {
  console.log("你好" + name);
}

// 导出方式三种
// 1.方式一:
// export const name = "why";
// export const age = 18;
// export const sayHello = function(name) {
//   console.log("你好" + name);
// }

// 2.方式二: {}中统一导出
// {}大括号, 但是不是一个对象
// {放置要导出的变量的引用列表}
export {
  name,
  age,
  sayHello
}

// 3.方式三: {} 导出时, 可以给变量起名别
// export {
//   name as fName,
//   age as fAge,
//   sayHello as fSayHello
// }

export default function() {
  console.log("对某一个东西进行格式化");
}
```

#### 导入

**import关键字**

**导入内容的方式也有多种:**

- **方式一：import {标识符列表}from '模块'**
- **方式二：导入时给标识符起别名**
- **方式三：通过*将模块功能放到一个模块功能对象（a module object）上**

```js
// 常见的导入方式也是有三种
// 方式一: import {} from '路径';
// import { name, age, sayHello } from './modules/foo.js';

// 方式二: 导出变量之后可以起别名
// import { name as wName, age as wAge, sayHello as wSayHello } from './modules/foo.js';
// import { fName as wName, fAge as wAge, fSayHello as wSayHello } from './modules/foo.js';

// 方式三: * as foo
// import * as foo from './modules/foo.js';

// console.log(foo.name);
// console.log(foo.age);
// foo.sayHello("王小波");


// 演练: export和import结合使用
import { name, age, sayHello } from './modules/bar.js';

console.log(name);
console.log(age);
console.log(sayHello);


// 演练: default export如何导入
import format from './modules/foo.js';
format();

// 演练: import()函数
let flag = true;
if (flag) {
  // require的本质是一个函数
  // require('')

  // 执行函数 
  // 如果是webpack的环境下: 模块化打包工具: es CommonJS require()
  // 纯ES Module环境下面: import()
  // 脚手架 -> webpack: import()
  import('./modules/foo.js').then(res => {
    console.log("在then中的打印");
    console.log(res.name);
    console.log(res.age);
  }).catch(err => {

  })
}

```

#### 封装第三方库的导入导出:

**Export和import结合使用**

**为什么要这样做呢:**

- **在开发和封装一个功能库时，通常我们希望将暴露的所有接口放到一个文件中**
- **这样方便指定统一的接口规范，也方便阅读**
- **这个时候，我们就可以使用export和import结合使用**

```js
// import {name, age, sayHello} from './foo.js';

// export {
//   name,
//   age, 
//   sayHello9555
// }
export {name, age, sayHello} from './foo.js';
```

#### default用法

**还有一种导出叫做默认导出（default export）**

- **默认导出export时可以不需要指定名字**

- **在导入时不需要使用{}，并且可以自己来指定名字**

- **它也方便我们和现有的CommonJS等规范相互操作**

**注意：在一个模块中，只能有一个默认导出（default export）**



#### import函数

**通过import加载一个模块，是不可以在其放到逻辑代码中的:**

![image-20220110221859485](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220110221859485.png)

**为什么会出现这个情况呢?**

- **这是因为ES Module在被JS引擎解析时，就必须知道它的依赖关系**

- **由于这个时候js代码没有任何的运行，所以无法在进行类似于if判断中根据代码的执行情况**

- **甚至下面的这种写法也是错误的：因为我们必须到运行时能确定path的值**

**但是某些情况下，我们确确实实希望动态的来加载某一个模块:**

- **如果根据不懂的条件，动态来选择加载模块的路径**
- **这个时候我们需要使用import() 函数来动态加载**

![image-20220111004513562](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220111004513562.png)



#### CommonJS的加载过程

**CommonJS模块加载js文件的过程是运行时加载的，并且是同步的：**

- **运行时加载意味着是js引擎在执行js代码的过程中加载模块**

- **同步的就意味着一个文件没有加载结束之前，后面的代码都不会执行**

![image-20220111004533818](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220111004533818.png)

**CommonJS通过module.exports导出的是一个对象：**

- **导出的是一个对象意味着可以将这个对象的引用在其他模块中赋值给其他变量**

- **但是最终他们指向的都是同一个对象，那么一个变量修改了对象的属性，所有的地方都会被修改**



#### ESModule加载过程

**ES Module加载js文件的过程是编译（解析）时加载的，并且是异步的：**

- **编译时（解析）时加载，意味着import不能和运行时相关的内容放在一起使用**
- **比如from后面的路径需要动态获取**
- **比如不能将import放到if等语句的代码块中**
- **所以我们有时候也称ES Module是静态解析的，而不是动态或者运行时解析的**

**异步的意味着：JS引擎在遇到import时会去获取这个js文件，但是这个获取的过程是异步的，并不会阻塞主线程继续执行**

- **SS也就是说设置了type=module 的代码，相当于在script标签上也加上了async 属性**
- **如果我们后面有普通的script标签以及对应的代码，那么ES Module对应的js文件和代码不会阻塞它们的执行**

![image-20220111004800663](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220111004800663.png)

**ES Module通过export导出的是变量本身的引用：**

- **export在导出一个变量时，js引擎会解析这个语法，并且创建模块环境记录（module environment record）**
- **模块环境记录会和变量进行绑定（binding），并且这个绑定是实时的**
- **而在导入的地方，我们是可以实时的获取到绑定的最新值的**

**所以，如果在导出的模块中修改了变化，那么导入的地方可以实时获取最新的变量**

**注意：在导入的地方不可以修改变量，因为它只是被绑定到了这个变量上（其实是一个常量）**

**思考：如果bar.js中导出的是一个对象，那么main.js中是否可以修改对象中的属性呢？**

- **答案是可以的，因为他们指向同一块内存空间；（自己编写代码验证，这里不再给出）**

![image-20220111004930965](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220111004930965.png)

### ![image-20220111012442930](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220111012442930.png)



### Node对ESModule的支持

**在最新的Current版本（v14.13.1）中，支持es module我们需要进行如下操作：**

- **方式一：在package.json中配置type: module（后续学习，我们现在还没有讲到package.json文件的作用）**
- **方式二：文件以.mjs结尾，表示使用的是ES Module；**
- **这里我们暂时选择以.mjs结尾的方式来演练：**



### CommonJS和ESModule交互

**结论一：通常情况下，CommonJS不能加载ES Modulep**

- **因为CommonJS是同步加载的，但是ES Module必须经过静态分析等，无法在这个时候执行JavaScript代码；**
- **但是这个并非绝对的，某些平台在实现的时候可以对代码进行针对性的解析，也可能会支持；**
- **Node当中是不支持的；**

**结论二：多数情况下，ES Module可以加载CommonJSpES** 

- **Module在加载CommonJS时，会将其module.exports导出的内容作为default导出方式来使用；**
- **这个依然需要看具体的实现，比如webpack中是支持的、Node最新的Current版本也是支持的；**
- **但是在最新的LTS版本中就不支持；**



## node常用内置模块

