# Node.js
## Node.js基础

### Node.js介绍

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>node环境安装</title>
</head>
<body>
<!--
    1.什么是Node.js?
    Node.js 是一个基于"Chrome V8 引擎" 的JavaScript "运行环境"

    2.什么是V8引擎?
    V8引擎是一款专门解释和执行JS代码的虚拟机, 任何程序只要集成了V8引擎都可以执行JS代码
    例如:
    -将V8引擎嵌入到浏览器中，那么我们写的JavaScript代码就会被浏览器所执行；
    -将V8引擎嵌入到NodeJS中，那么我们写的JavaScript代码就会被NodeJS所执行。

    3.什么是运行环境?
    运行环境 就是 生存环境
    地球是人类的生存环境
    浏览器是网页的生存环境
    windows是.exe应用程序的生存环境
    Android是.apk应用程序的生存环境
    也就是说运行环境就是特定事物的生存环境
    ...
    NodeJS也是一个生存的环境, 由于NodeJS中集成了V8引擎
    所以NodeJS是JavaScript应用程序的一个生存环境

    4.总结:
    NodeJS不是一门编程语言, NodeJS是一个运行环境,
    由于这个运行环境集成了V8引擎, 所以在这个运行环境下可以运行我们编写的JS代码,
    这个运行环境最大的特点就是提供了操作"操作系统底层的API",
    通过这些底层API我们可以编写出网页中无法实现的功能(诸如: 打包工具, 网站服务器等)
-->
</body>
</html>
```

### 环境配置

```js
<!--
搭建方式一:
1.官网下载.msi安装包: https://nodejs.org/zh-cn/
2.全程下一步
3.在命令行工具中输入 node -v

搭建方式二:
1.官网下载.zip安装包: https://nodejs.org/zh-cn/
2.解压下载好的安装包
3.在"高级系统设置"中手动配置环境变量
4.在命令行工具中输入 node -v

搭建方式三:
1.下载NVM: https://github.com/coreybutler/nvm-windows
2.在D盘创建dev目录
3.在Dev目中中创建两个子目录nvm和nodejs, 并且把nvm包解压进去nvm目录中
4.在install.cmd文件上面右键选择【以管理员身份运行】
  在终端中直接按下回车
  将弹出的文件另存为到NVM目录
  打开settings.txt文件. 修改
  root: D:\Developer\Dev\NVM
  path: D:\Developer\Dev\Node
6.配置环境变量
  NVM_HOME: D:\Developer\Dev\NVM
  NVM_SYMLINK: D:\Developer\Dev\Node
  在Path中添加 %NVM_HOME% %NVM_SYMLINK%
7.在命令行工具中输入 nvm version

NVM常用命令
- nvm list 查看当前安装的Node.js所有版本
- nvm install 版本号 安装指定版本的Node.js
- nvm uninstall 版本号 卸载指定版本的Node.js
- nvm use 版本号 选择指定版本的Node.js
-->
```

### 在webstorm中执行js代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-Node程序执行</title>
    <script>
        let a = 10;
        let b = 20;
        let res = a + b;
        console.log(res);
    </script>
</head>
<body>
<!--
Node程序执行方式:
1.由于浏览器中集成了V8引擎, 所以浏览器可以解释执行JS代码
  1.1可以直接在浏览器控制台中执行JS代码
  1.2可以在浏览器中执行JS文件中的JS代码

2.由于NodeJS中也集成了V8引擎, 所以浏览器可以解释执行JS代码
  2.1可以直接在命令行工具中编写执行JS代码(REPL -- Read Eval Print Loop:交互式解释器)
  2.2可以在命令行工具中执行JS文件中的JS代码
-->
</body>
</html>
```

### Node环境和浏览器环境的区别

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-Node环境和浏览器环境区别</title>
    <script>
        // console.log(window);
        console.log(this);
    </script>
</head>
<body>
<!--
1.Node环境和浏览器环境区别
NodeJS环境和浏览器环境一样都是一个JS的运行环境, 都可以执行JS代码.
但是由于宿主不同所以特点也有所不同

1.1内置对象不同
- 浏览器环境中提供了window全局对象
- NodeJS环境中的全局对象不叫window, 叫global

1.2this默认指向不同
- 浏览器环境中全局this默认指向window
- NodeJS环境中全局this默认指向空对象{}

1.3API不同
- 浏览器环境中提供了操作节点的DOM相关API和操作浏览器的BOM相关API
- NodeJS环境中没有HTML节点也没有浏览器, 所以NodeJS环境中没有DOM/BOM
-->
</body>
</html>
```

### node.js在全局对象上的属性

**__dirname:当前文件夹的路径**

**__filename:当前文件的路径**

```js
/*
1.和浏览器一样Node环境中的全局对象也提供了很多方法属性供我们使用
中文文档地址: http://nodejs.cn/api/

__dirname: 当前文件所在文件夹的绝对路径
__filename: 当前文件的绝对路径
setInterval / clearInterval : 和浏览器中window对象上的定时器一样
setTimeout /  clearTimeout : 和浏览器中window对象上的定时器一样
console :  和浏览器中window对象上的打印函数一样
*/

<----------------------------------------------------------------->
// console.log(__dirname); // C:\Users\Jonathan_Lee\Desktop\Git-Webpack\Node
console.log(__filensame); // C:\Users\Jonathan_Lee\Desktop\Git-Webpack\Node\05.js

console.log("abc");
/*
setTimeout(function () {
    console.log("123");
}, 2000);
 */
setInterval(function () {
    console.log("123");
}, 1000);
```

### Node模块：采用CommonJs模块规范

**export暴露，require导入**

```js
/*
1.什么是模块?
1.1浏览器开发中的模块
   在浏览器开发中为了避免命名冲突, 方便维护等等
   我们采用类或者立即执行函数的方式来封装JS代码, 来避免命名冲突和提升代码的维护性
   其实这里的一个类或者一个立即执行函数就是浏览器开发中一个模块
   let obj = {
       模块中的业务逻辑代码
   };
   ;(function(){
      模块中的业务逻辑代码
      window.xxx = xxx;
   })();
存在的问题:
没有标准没有规范

1.2NodeJS开发中的模块
   NodeJS采用CommonJS规范实现了模块系统

1.3CommonJS规范
CommonJS规范规定了如何定义一个模块, 如何暴露(导出)模块中的变量函数, 以及如何使用定义好的模块

- 在CommonJS规范中一个文件就是一个模块
- 在CommonJS规范中每个文件中的变量函数都是私有的，对其他文件不可见的
- 在CommonJS规范中每个文件中的变量函数必须通过exports暴露(导出)之后其它文件才可以使用
- 在CommonJS规范中想要使用其它文件暴露的变量函数必须通过require()导入模块才可以使用
*/

//  a.js
let name = "it666.com";
function sum(a, b) {
    return a + b;
}
exports.str = name;
exports.fn = sum;

//  b.js
let aModule = require("./06-a");
console.log(aModule);
console.log(aModule.str);
let res = aModule.fn(10, 20);
console.log(res);
```

### Node模块导出数据的几种方式

**exports.属性，通过require导入赋值给一个对象**

**module.exports.属性，使用方法和exports一样**

**global.属性不符合CommonJS的规范，不推荐使用**

```js
/*
1.在NodeJS中想要导出模块中的变量函数有三种方式
1.1通过exports.xxx = xxx导出
1.2通过module.exports.xxx = xxx导出
1.3通过global.xxx = xxx导出
注意点:
无论通过哪种方式导出, 使用时都需要先导入(require)才能使用
通过global.xxx方式导出不符合CommonJS规范, 不推荐使用
*/

//  a.js
let name = "it666.com";
function sum(a, b) {
    return a + b;
}
// exports.str = name;
// exports.fn = sum;
module.exports.str = name;
module.exports.fn = sum;
// global.str = name;
// global.fn = sum;

//  b.js
let aModule = require("./07-a");
// console.log(aModule);
// console.log(aModule.str);
// let res = aModule.fn(10, 20);
// console.log(res);
console.log(str);
let res = fn(10, 20);
console.log(res);
```

### exports和module.exports的区别

**区别就是exports是不能直接赋值，Module.exports是可以直接赋值的**

```js
/*
1.exports和module.exports区别
exports只能通过 exports.xxx方式导出数据, 不能直接赋值
module.exports既可以通过module.exports.xxx方式导出数据, 也可以直接赋值

注意点:
在企业开发中无论哪种方式都不要直接赋值, 这个问题只会在面试中出现
*/

//  a.js  
let name = "lnj";
// exports.str = name;
// module.exports.str = name;

// exports = name;

module.exports = name;

//b.js
let aModule = require("./08-a.js");
/*
exports = name;
{}
* */
/*
module.exports = name;
lnj
* */
console.log(aModule);
```

### require注意点

```html
<!--
1.require注意点
1.1require导入模块时可以不添加导入模块的类型
如果没有指定导入模块的类型, 那么会依次查找.js .json .node文件
无论是三种类型中的哪一种, 导入之后都会转换成JS对象返回给我们

1.2导入自定义模块时必须指定路径
require可以导入"自定义模块(文件模块)"、"系统模块(核心模块)"、"第三方模块"
导入"自定义模块"模块时前面必须加上路径
导入"系统模块"和"第三方模块"是不用添加路径

1.3导入"系统模块"和"第三方模块"是不用添加路径的原因
如果是"系统模块"直接到环境变量配置的路径中查找
如果是"第三方模块"会按照module.paths数组中的路径依次查找
-->
```

### Node包和包管理

```html
<!--
1.什么是包?
前面说过在编写代码的时候尽量遵守单一原则,
也就是一个函数尽量只做一件事情
例如: 读取数据函数/写入数据函数/生成随机数函数等等,
不要一个函数既读取数据又写入数据又生成随机数,
这样代码非常容易出错, 也非常难以维护

在模块化开发中也一样, 在一个模块(一个文件中)尽量只完成一个特定的功能
但是有些比较复杂的功能可能需要由多个模块组成,
例如: jQuery选择器相关的代码在A模块, CSS相关的代码在B模块, ...
      我们需要把这些模块组合在一起才是完成的jQuery
那么这个时候我们就需要一个东西来维护多个模块之间的关系
这个维护多个模块之间关系的东西就是"包"

简而言之: 一个模块是一个单独的文件, 一个包中可以有一个或多个模块

2.NodeJS包的管理
在NodeJS中为了方便开发人员发布、安装和管理包, NodeJS推出了一个包管理工具
NPM(Node Package Manager)
NPM不需要我们单独安装, 只要搭建好NodeJS环境就已经自动安装好了
NPM就相当于电脑上的"QQ管家软件助手", 通过NPM我们可以快速找到我们需要的包,
可以快速安装我们需要的包, 可以快速删除我们不想要的包等等
-->
```

### npm的使用

```html
<!--
    1.NPM包安装方式
    - 全局安装  (一般用于安装全局使用的工具, 存储在全局node_modules中)
    npm install -g 包名   (默认安装最新版本)
    npm uninstall -g 包名
    npm update -g 包名   (更新失败可以直接使用install)

    - 本地安装 (一般用于安装当前项目使用的包, 存储在当前项目node_modules中)
    npm install 包名
    npm uninstall 包名
    npm update 包名

    2.初始化本地包
    npm init   ->  初始化package.json文件
    npm init -y -> 初始化package.json文件

    npm install 包名 --save
    npm install 包名 --save-dev

    包描述文件 package.json, 定义了当前项目所需要的各种模块，以及项目的配置信息（比如名称、版本、许可证等元数据）。
    npm install 命令根据这个配置文件，自动下载所需的模块，也就是配置项目所需的运行和开发环境
    注意点:package.json文件中, 不能加入任何注释

    - dependencies：生产环境包的依赖，一个关联数组，由包的名称和版本号组成
    - devDependencies：开发环境包的依赖，一个关联数组，由包的名称和版本号组成

    1.将项目拷贝给其它人, 或者发布的时候, 我们不会将node_modules也给别人, 因为太大
    2.因为有的包可能只在开发阶段需要, 但是在上线阶段不需要, 所以需要分开指定

    npm i               所有的包都会被安装
    npm i --production  只会安装dependencies中的包
    npm i --development  只会安装devDependencies中的包
-->
```

### NRM和CNPM的使用

```html
<!--
1.什么是nrm?
由于npm默认回去国外下载资源, 所以对于国内开发者来说下载会比较慢
所以就有人写了一个nrm工具, 允许你将资源下载地址从国外切换到国内

npm install -g nrm   安装NRM
nrm --version 查看是否安装成功
npm ls    查看允许切换的资源地址
npm use taobao  将下载地址切换到淘宝

PS:淘宝资源地址和国外的地址内容完全同步,。淘宝镜像与官方同步频率目前为 10分钟 一次以保证尽量与官方服务同步

2.什么是cnpm?
由于npm默认回去国外下载资源, 所以对于国内开发者来说下载会比较慢
cnpm 就是将下载源从国外切换到国内下载, 只不过是将所有的指令从npm变为cnpm而已

npm install cnpm -g –registry=https://registry.npm.taobao.org  安装CNPM
cnpm -v  查看是否安装成功
使用方式同npm, 例如: npm install jquery 变成cnpm install jquery 即可
-->
```

### yarn的使用

```html
<!--
1.什么是YARN?
Yarn是由Facebook、Google、Exponent 和 Tilde 联合推出了一个新的 JS 包管理工具
Yarn 是为了弥补 npm5.0之前 的一些缺陷而出现的

注意点:
在npm5.0之前，yarn的优势特别明显.但是现在NPM已经更新到6.9.x甚至7.x了,
随着NPM的升级NPM优化甚至超越Yarn,所以个人还是建议使用NPM

2.NPM的缺陷:
2.1npm install的时候巨慢
   npm 是按照队列执行每个 package，也就是说必须要等到当前 package 安装完成之后，才能继续后面的安装
2.2同一个项目，npm install的时候无法保持一致性
  “5.0.3”表示安装指定的5.0.3版本，
  “~5.0.3”表示安装5.0.X中最新的版本，
  “^5.0.3”表示安装5.X.X中最新的版本
2.3... ...

3.YARN优点:
3.1速度快:
    - 并行安装: 而 Yarn 是同步执行所有任务，提高了性能
    - 离线模式：如果之前已经安装过一个软件包，用Yarn再次安装时之间从缓存中获取，就不用像npm那样再从网络下载了
3.2安装版本统一：
    - 为了防止拉取到不同的版本，Yarn 有一个锁定文件 (lock file) 记录了被确切安装上的模块的版本号
3.3... ...

4.YARN的安装
npm install -g yarn
yarn --version

5.YARN使用
5.1初始化包
npm init -y
yarn init -y

5.2安装包
npm install xxx --save
yarn add xxx
npm install xxx --save-dev
yarn add xxx --dev

5.3移除包
npm uninstall xxx
yarn remove xxx

5.4更新包
npm update xxx
yarn upgrade xxx --latest

5.5全局安装
npm install -g xxx
npm uninstall -g xxx
npm update -g xxx

yarn global add xxx
yarn global upgrade xxx
yarn global remove xxx
-->
```

## Node.js核心API

### Buffer

**创建一个指定大小的Buffer：Buffer.alloc(size[, fill[, encoding]])**

**根据数组/字符串创建一个Buffer对象：Buffer.from(string[, encoding])**

**Buffer的本质是一个数组，因此可以通过下标来操作Buffer**

```html
<!--
0.准备知识
0.1计算机只能识别0和1(因为计算机只认识通电和断电两种状态),
0.2所有存储在计算机上的数据都是0和1组成的(数据越大0和1就越多)
0.3计算机中的度量单位
1 B(Byte字节) = 8 bit(位)
// 00000000  就是一个字节
// 111111111 也是一个字节
// 10101010  也是一个字节
// 任意8个 0或1的组合都是一个字节
1 KB = 1024 B
1 MB = 1024KB
1 GB = 1024MB

1.什么是Buffer?
Buffer是NodeJS全局对象上的一个类, 是一个专门用于存储字节数据的类
NodeJS提供了操作计算机底层API, 而计算机底层只能识别0和1,
所以就提供了一个专门用于存储字节数据的类

2.如何创建一个Buffer对象
2.1创建一个指定大小的Buffer
Buffer.alloc(size[, fill[, encoding]])

2.2根据数组/字符串创建一个Buffer对象
Buffer.from(string[, encoding])

3.Buffer对象本质
本质就是一个数组
-->
<script>
    // let buf = Buffer.alloc(5);
    // console.log(buf); // <Buffer 00 00 00 00 00>
    // 注意点: 通过console.log();输出Buffer. 会自动将存储的内容转换成16进制再输出

    // let buf = Buffer.alloc(5, 17);
    // console.log(buf);

    // let buf = Buffer.from("abc");
    // console.log(buf); // <Buffer 61 62 63>

    // let buf = Buffer.from([1, 3, 5]);
    // console.log(buf);
    // console.dir(buf);
    // buf[0] = 6;
    // console.log(buf);
</script>
```

### Buffer实例方法

**将二进制数据转换成字符串，返回: <string> 转换后的字符串数据：buf.toString();**

**往Buffer中写入数据：buf.write(string[, offset[, length]][, encoding])**
**第一个参数为字符串，第二个参数为偏移量（从Buffer中的第几位开始存储数据），第三个参数为存储的长度，第四个为编码方式**

**从指定位置截取新Buffer：buf.slice([start[, end]）**

```html
<!--
1.将二进制数据转换成字符串
返回: <string> 转换后的字符串数据。
buf.toString();

2.往Buffer中写入数据
string <string> 要写入 buf 的字符串。
offset <integer> 开始写入 string 之前要跳过的字节数。默认值: 0。
length <integer> 要写入的字节数。默认值: buf.length - offset。
encoding <string> string 的字符编码。默认值: 'utf8'。
返回: <integer> 已写入的字节数。
buf.write(string[, offset[, length]][, encoding])

3.从指定位置截取新Buffer
start <integer> 新 Buffer 开始的位置。默认值: 0。
end <integer> 新 Buffer 结束的位置（不包含）
buf.slice([start[, end]])
-->
<script>
// let buf = Buffer.from([97, 98, 99]);
// console.log(buf);
// console.log(buf.toString());

// let buf = Buffer.alloc(5); // <Buffer 00 00 00 00 00>
// console.log(buf);
// // buf.write("abcdefg");
// // buf.write("abcdefg", 2);
// buf.write("abcdefg", 2, 2);
// console.log(buf);
// console.log(buf.toString());

// let buf1 = Buffer.from("abcdefg");
// let buf2 = buf1.slice();
// let buf2 = buf1.slice(2);
// let buf2 = buf1.slice(2,4);
// console.log(buf2);
// console.log(buf2.toString());
</script>
```

### Buffer的静态方法

**检查是否支持某种编码格式：Buffer.isEncoding(encoding)**

**检查是否是Buffer类型对象：Buffer.isBuffer(obj)**

**获取Buffer实际字节长度：Buffer.byteLength(string[, encoding])**

**合并Buffer中的数据：Buffer.concat(list[, totalLength])**

```html
<!--
1.检查是否支持某种编码格式
Buffer.isEncoding(encoding)

2.检查是否是Buffer类型对象
Buffer.isBuffer(obj)

3.获取Buffer实际字节长度
Buffer.byteLength(string[, encoding])
注意点: 一个汉字占用三个字节

4.合并Buffer中的数据
Buffer.concat(list[, totalLength])
-->
<script>
// let res = Buffer.isEncoding("gbk");
// console.log(res);

// let obj = {};
// let obj = Buffer.alloc(5);
// let res = Buffer.isBuffer(obj);
// console.log(res);

// let buf = Buffer.from("123");
// let buf = Buffer.from("知播渔");
// let res = Buffer.byteLength(buf);
// console.log(res);
// console.log(buf.length);

let buf1 = Buffer.from("123");
let buf2 = Buffer.from("abc");
let buf3 = Buffer.from("xxx");
let res = Buffer.concat([buf1, buf2, buf3]);
console.log(res);
console.log(res.toString());
</script>
```

### path模块

**path.basename(path, ext)：获取路径的最后一部分，ext可以指定文件的扩展名，然后返回的字符串中会自动把指定的后缀给截取掉**

**path.dirname(path)：dirname用于获取路径中的目录, 也就是除了最后一个部分以外的内容**

**path.extname(path)：extname用于获取路径中最后一个组成部分的扩展名**

**path.isAbsolute(path)：isAbsolute用于判断路径是否是一个绝对路径**

**path.delimiter：path.delimiter用于获取当前操作系统环境变量的分隔符的**

**path.sep：path.sep用于获取当前操作系统中路径的分隔符的**

**path.parse(path): 用于将路径转换成对象**

**path.format(pathObject): 用于将对象转换成路径**

**path.join([...paths]): 用于拼接路径**

**path.normalize(path): 用于规范化路径**

**path.relative(from, to): 用于计算相对路径**

**path.resolve([...paths]): 用于解析路径**

```html
<!--
1.路径模块(path)
封装了各种路径相关的操作
和Buffer一样,NodeJS中的路径也是一个特殊的模块
不同的是Buffer模块已经添加到Global上了, 所以不需要手动导入
而Path模块没有添加到Global上, 所以使用时需要手动导入

2.获取路径的最后一部分
path.basename(path[, ext])

3.获取路径
path.dirname(path)

4.获取扩展名称
path.extname(path)

5.判断是否是绝对路径
path.isAbsolute(path)

6.获取当前操作系统路径分隔符
path.delimiter  （windows是\ Linux是/）

7.获取当前路径环境变量分隔符
path.sep  (windows中使用; linux中使用:)
-->
<!--
1.路径的格式化处理
// path.parse()  string->obj
// path.format() obj->string

2.拼接路径
path.join([...paths])

3.规范化路径
path.normalize(path)

4.计算相对路径
path.relative(from, to)

5.解析路径
path.resolve([...paths])
-->

<script>
    let path = require("path");

/*
// basename用于获取路径的最后一个组成部分
// let res = path.basename('/a/b/c/d/index.html');
// let res = path.basename('/a/b/c/d');
// let res = path.basename('/a/b/c/d/index.html', ".html");
// console.log(res);

// dirname用于获取路径中的目录, 也就是除了最后一个部分以外的内容
// let res = path.dirname('/a/b/c/d/index.html');
// let res = path.dirname('/a/b/c/d');
// console.log(res);

// extname用于获取路径中最后一个组成部分的扩展名
// let res = path.extname('/a/b/c/d/index.html');
// let res = path.extname('/a/b/c/d');
// console.log(res);
*/
/*
isAbsolute用于判断路径是否是一个绝对路径
注意点:
区分操作系统
在Linux操作系统中/开头就是绝对路径
在Windows操作系统中盘符开头就是绝对路径

在Linux操作系统中路径的分隔符是左斜杠 /
在Windows操作系统中路径的分隔符是右斜杠 \
* */
/*
// let res = path.isAbsolute('/a/b/c/d/index.html'); // true
// let res = path.isAbsolute('./a/b/c/d/index.html'); // false
// let res = path.isAbsolute('c:\\a\\b\\c\\d\\index.html'); // true
// let res = path.isAbsolute('a\\b\\c\\d\\index.html'); // false
// console.log(res);

// path.sep用于获取当前操作系统中路径的分隔符的
// 如果是在Linux操作系统中运行那么获取到的是 左斜杠 /
// 如果是在Windows操作系统中运行那么获取到的是 右斜杠 \
// console.log(path.sep);

// path.delimiter用于获取当前操作系统环境变量的分隔符的
// 如果是在Linux操作系统中运行那么获取到的是 :
// 如果是在Windows操作系统中运行那么获取到的是 ;
// console.log(path.delimiter);
*/

/*
path.parse(path): 用于将路径转换成对象
{
  root: '/',
  dir: '/a/b/c/d',
  base: 'index.html',
  ext: '.html',
  name: 'index'
}
path.format(pathObject): 用于将对象转换成路径
* */
/*
// let obj = path.parse("/a/b/c/d/index.html");
// console.log(obj);

let obj = {
    root: '/',
    dir: '/a/b/c/d',
    base: 'index.html',
    ext: '.html',
    name: 'index'
};
let str = path.format(obj);
console.log(str);
 */

/*
path.join([...paths]): 用于拼接路径
注意点:
如果参数中没有添加/, 那么该方法会自动添加
如果参数中有.., 那么会自动根据前面的参数生成的路径, 去到上一级路径
* */
/*
// let str = path.join("/a/b", "c"); // /a/b/c
// let str = path.join("/a/b", "/c"); // /a/b/c
// let str = path.join("/a/b", "/c", "../"); // /a/b/c --> /a/b
// let str = path.join("/a/b", "/c", "../../"); // /a/b/c --> /a
// console.log(str);
 */

/*
path.normalize(path): 用于规范化路径
* */
/*
let res = path.normalize("/a//b///c////d/////index.html");
console.log(res);
*/

/*
path.relative(from, to): 用于计算相对路径
第一个参数: /data/orandea/test/aaa
第二个参数: /data/orandea/impl/bbb

/data/orandea/test/aaa --> ../  --> /data/orandea/test
/data/orandea/test --> ../ --> /data/orandea
..\..\impl\bbb
* */
/*
let res = path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
console.log(res);
 */

/*
path.resolve([...paths]): 用于解析路径
注意点: 如果后面的参数是绝对路径, 那么前面的参数就会被忽略
* */
// let res = path.resolve('/foo/bar', './baz'); // /foo/bar/baz
// let res = path.resolve('/foo/bar', '../baz'); // /foo/baz
let res = path.resolve('/foo/bar', '/baz'); // /baz
console.log(res);
</script>
```

### fs文件模块

#### fs文件操作

**fs.stat(path, callback) 异步的方法：数据是通过回调函数获取
fs.statSync(path) 同步的方法：数据是通过返回值的方式获取**

```html
<!--
1.文件模块(fs)
封装了各种文件相关的操作

2.查看文件状态
fs.stat(path, callback) 异步的方法
fs.statSync(path) 同步的方法
-->

<script>
let fs = require("fs");
/*
console.log("1");
fs.stat(__dirname, function (err, stats) {
    // console.log("3");
    // console.log(err);
    // birthtime: 文件的创建时间
    // mtime: 文件中内容发生变化, 文件的修改时间
    // console.log(stats);

    if(stats.isFile()){
        console.log("当前路径对应的是一个文件");
    }else if(stats.isDirectory()){
        console.log("当前路径对应的是一个文件夹");
    }
});
console.log("2");
 */

let stats = fs.statSync(__filename);
console.log(stats);
</script>
```

#### fs文件读取

**fs.readFile(path,options, callback)：如果options给了”utf8“则返回的数据会变成字符串**

**fs.readFileSync(path, options)：同步的方法，即通过接收返回的数据来操作**

```html
 <!--
1.文件读取
fs.readFile(path[, options], callback)
fs.readFileSync(path[, options])

注意点:
没有指定第二个参数, 默认会将读取到的数据放到Buffer中
第二个参数指定为utf8, 返回的数据就是字符串
-->

<script>
let fs = require("fs");
let path = require("path");

// 1.拿到需要读取的文件路径
let str = path.join(__dirname, "data.txt");
console.log(str);
// 2.读取文件
/*
fs.readFile(str,"utf8", function (err, data) {
    if(err){
        throw new Error("读取文件失败");
    }
    console.log(data);
    // console.log(data.toString());
});
 */
let data = fs.readFileSync(str, "utf8");
console.log(data);
</script>
```

#### fs文件写入

**fs.writeFile(file, data, options, callback)：options为可选的编码方式，一般为”utf8“，
fs.writeFileSync(file, data[, options])**

**可以写入字符串或者buffer**

```html
<!--
1.文件写入
fs.writeFile(file, data[, options], callback)
fs.writeFileSync(file, data[, options])
-->
<script>
let fs = require("fs");
let path = require("path");

// 1.拼接写入的路径
let str = path.join(__dirname, "lnj.txt");

// 2.写入数据
/*
// fs.writeFile(str, "知播渔 www.it666.com", "utf-8", function (err) {
 */
let buf = Buffer.from("www.itzb.com");
fs.writeFile(str, buf, "utf-8", function (err) {
    if(err){
        throw new Error("写入数据失败");
    }else{
        console.log("写入数据成功");
    }
});

// let res = fs.writeFileSync(str, "知播渔 www.it666.com", "utf-8");
// console.log(res);
</script>
```

#### fs文件追加写入

**fs.appendFile(path, data, options, callback)
fs.appendFileSync(path, data,options)**

```html
<!--
1.追加写入
fs.appendFile(path, data[, options], callback)
fs.appendFileSync(path, data[, options])
-->
<script>
let fs = require("fs");
let path = require("path");

// 1.拼接写入的路径
let str = path.join(__dirname, "lnj.txt");

// 2.开始追加数据
fs.appendFile(str, "知播渔", "utf8", function (err) {
    if(err){
        throw new Error("追加数据失败");
    }else{
        console.log("追加数据成功");
    }
});
</script>
```

#### 大文件操作

##### 文件分批读取

**fs.createReadStream(path, options)：options是一个对象，最重要的是encoding和highWaterMark**

**然后拿到返回的读取流，对读取流进行事件的监听，在data事件中我们可以操作data，然后进行相关的操作**

```html
<!--
1.大文件操作
前面讲解的关于文件写入和读取操作都是一次性将数据读入内存或者一次性写入到文件中
但是如果数据比较大, 直接将所有数据都读到内存中会导致计算机内存爆炸,卡顿,死机等
所以对于比较大的文件我们需要分批读取和写入

fs.createReadStream(path[, options])
fs.createWriteStream(path[, options])
-->
<!-- 读取流 -->
<script>
/*
// 1.拼接读取的路径
let str = path.join(__dirname, "lnj.txt");
// 2.创建一个读取流
let readStream = fs.createReadStream(str, {encoding : "utf8", highWaterMark : 1});
// 3.添加事件监听
readStream.on("open", function () {
    console.log("表示数据流和文件建立关系成功");
});
readStream.on("error", function () {
    console.log("表示数据流和文件建立关系失败");
});
readStream.on("data", function (data) {
    console.log("表示通过读取流从文件中读取到了数据", data);
});
readStream.on("close", function () {
    console.log("表示数据流断开了和文件的关系, 并且数据已经读取完毕了");
});
 */
</script>
```

##### 大文件分批写入

**创建一个写入流，可以监听文件和数据流关系建立成功，失败和关闭，但是写入数据时通过writeStream.write()写入，但每次都只能写入一个字符，写入完后，我们必须手动调用writeStream.end()关闭连接**

```html
<script>
// 1.拼接写入的路径
let str = path.join(__dirname, "it666.txt");
// 2.创建一个写入流
let writeStream = fs.createWriteStream(str, {encoding : "utf8"});
// 3.监听写入流的事件
writeStream.on("open", function () {
    console.log("表示数据流和文件建立关系成功");
});
writeStream.on("error", function () {
    console.log("表示数据流和文件建立关系失败");
});
writeStream.on("close", function () {
    console.log("表示数据流断开了和文件的关系");
});
let data = "www.it666.com";
let index = 0;
let timerId = setInterval(function () {
    let ch = data[index];
    index++;
    writeStream.write(ch);
    console.log("本次写入了", ch);
    if(index === data.length){
        clearInterval(timerId);
        writeStream.end();
    }
}, 1000);
</script>
```

#### 文件拷贝案例

```js
// 1.生成读取和写入的路径
let readPath = path.join(__dirname, "test.mp4");
let writePath = path.join(__dirname, "abc.mp4");
// 2.创建一个读取流
let readStream = fs.createReadStream(readPath);
// 3.创建一个写入流
let writeStream = fs.createWriteStream(writePath);
// 4.监听读取流事件
readStream.on("open", function () {
    console.log("表示数据流和文件建立关系成功");
});
readStream.on("error", function () {
    console.log("表示数据流和文件建立关系失败");
});
readStream.on("data", function (data) {
    // console.log("表示通过读取流从文件中读取到了数据", data);
    writeStream.write(data);
});
readStream.on("close", function () {
    console.log("表示数据流断开了和文件的关系, 并且数据已经读取完毕了");
    writeStream.end();
});
// 5.监听写入流的事件
writeStream.on("open", function () {
    console.log("表示数据流和文件建立关系成功");
});
writeStream.on("error", function () {
    console.log("表示数据流和文件建立关系失败");
});
writeStream.on("close", function () {
    console.log("表示数据流断开了和文件的关系");
});
```

#### 读取流的管道方法

**利用读取流中的readStream.pipe(writeStream)中拷贝文件**

```js
// 1.生成读取和写入的路径
let readPath = path.join(__dirname, "test.mp4");
let writePath = path.join(__dirname, "abc.mp4");
// 2.创建一个读取流
let readStream = fs.createReadStream(readPath);
// 3.创建一个写入流
let writeStream = fs.createWriteStream(writePath);
// 利用读取流的管道方法来快速的实现文件拷贝
readStream.pipe(writeStream);
```

#### 文件目录操作

**fs.rmdir(path, callback(err,files))：files是所有的文件名，可以通过forEach来遍历获取到所有的文件信息**

```html
<!--
1、创建目录
fs.mkdir(path[, mode], callback)
fs.mkdirSync(path[, mode])

2、读取目录
fs.readdir(path[, options], callback)
fs.readdirSync(path[, options])

3、删除目录
fs.rmdir(path, callback)
fs.rmdirSync(path)
-->

<script>
let fs = require("fs");
let path = require("path");
let str = path.join(__dirname, "abc");
/*
fs.mkdir(str, function (err) {
    if(err){
        throw new Error("创建目录失败");
    }else{
        console.log("创建目录成功");
    }
});
 */
/*
fs.rmdir(str, function (err) {
    if(err){
        throw new Error("删除目录失败");
    }else{
        console.log("删除目录成功");
    }
});
 */
fs.readdir(__dirname, function (err, files) {
    if(err){
        throw new Error("读取目录失败");
    }else{
        // console.log(files);
        files.forEach(function (obj) {
            // console.log(obj);
            let filePath = path.join(__dirname, obj);
            // console.log(filePath);
            let stats = fs.statSync(filePath);
            if(stats.isFile()){
                console.log("是一个文件", obj);
            }else if(stats.isDirectory()){
                console.log("是一个目录", obj);
            }
        });
    }
})
</script>
```

#### 文件操作联系

```html
<!--
利用NodeJS生成项目模板
projectName
   |---images
   |---css
   |---js
   |---index.html
-->
<script>
let fs = require("fs");
let path = require("path");

class CreateProject {
    constructor(rootPath, projectName){
        this.rootPath = rootPath;
        this.projectName = projectName;
        this.subFiles = ["images", "css", "js", "index.html"];
    }
    initProject(){
        // 1.创建站点文件夹
        let projectPath = path.join(this.rootPath, this.projectName);
        fs.mkdirSync(projectPath);
        // 2.创建子文件和子目录
        this.subFiles.forEach(function (fileName) {
            if(path.extname(fileName) === ""){
                let dirPath = path.join(projectPath, fileName);
                fs.mkdirSync(dirPath);
            }else{
                let filePath = path.join(projectPath, fileName);
                fs.writeFileSync(filePath, "");
            }
        })
    }
}

let cp = new CreateProject(__dirname, "taobao");
cp.initProject();
</script>
```

### Http模块

#### http模块-服务器

**1.创建一个服务器实例对象**

**2.注册请求监听**

**3.指定监听的端口**

```html
<!--
1.什么是HTTP模块
通过Nodejs提供的http模块，我们可以快速的构建一个web服务器,
也就是快速实现过去PHP服务器的功能(接收浏览器请求、响应浏览器请求等)

2.通过HTTP模块实现服务器功能步骤
2.1导入HTTP模块
2.2创建服务器实例对象
2.3绑定请求事件
2.4监听指定端口请求
-->
<script>
let http = require("http");
// 1.创建一个服务器实例对象
let server = http.createServer();
// 2.注册请求监听
server.on("request", function (req, res) {
    // end方法的作用: 结束本次请求并且返回数据
    // res.end("www.it666.com");
    // writeHead方法的作用: 告诉浏览器返回的数据是什么类型的, 返回的数据需要用什么字符集来解析
    res.writeHead(200, {
        "Content-Type": "text/plain; charset=utf-8"
    });
    res.end("知播渔");
});
// 3.指定监听的端口
server.listen(3000);
</script>
```

#### 服务器的简写

```js
http.createServer(function (req, res) {
    res.writeHead(200, {
        "Content-Type": "text/plain; charset=utf-8"
    });
    res.end("知播渔666");
}).listen(3000);
```

#### 路径的分发

**通过req.url来获取url地址**

**res的相关方法**

**res.write返回数据**

```html
<!--
1.什么是路径分发?
路径分发也称之为路由, 就是根据不同的请求路径返回不同的数据

2.如何根据不同的请求路径返回不同的数据?
通过请求监听方法中的request对象, 我们可以获取到当前请求的路径
通过判断请求路径的地址就可以实现不同的请求路径返回不同的数据
-->
<script>
let http = require("http");

// 1.创建一个服务器实例对象
let server = http.createServer();
// 2.注册请求监听
/*
request对象其实是http.IncomingMessage 类的实例
response对象其实是http.ServerResponse 类的实例
* */
server.on("request", function (req, res) {
    res.writeHead(200, {
        "Content-Type": "text/plain; charset=utf-8"
    });
    // console.log(req.url);
    if(req.url.startsWith("/index")){
        // 注意点: 如果通过end方法来返回数据, 那么只会返回一次
        // res.end("首页1");
        // res.end("首页2");
        // 注意点: 如果通过write方法来返回数据, 那么可以返回多次
        //         write方法不具备结束本次请求的功能, 所以还需要手动的调用end方法来结束本次请求
        res.write("首页1");
        res.write("首页2");
        res.end();
    }else if(req.url.startsWith("/login")){
        res.end("登录");
    }else{
        res.end("没有数据");
    }
});
// 3.指定监听的端口
server.listen(3000);
</script>
```

#### 相应完整的网页

```html
<!--
1.响应完整页面
拿到用户请求路径之后, 只需要利用fs模块将对应的网页返回即可
-->
<script>
let http = require("http");
let path = require("path");
let fs = require("fs");

// 1.创建一个服务器实例对象
let server = http.createServer();
// 2.注册请求监听
server.on("request", function (req, res) {
    /*
    if(req.url.startsWith("/index")){
        // let filePath = path.join(__dirname, "www", "index.html");
        // let filePath = path.join(__dirname, "www", req.url);
        // fs.readFile(filePath, "utf8", function (err, content) {
        //     if(err){
        //         res.end("Server Error");
        //     }
        //     res.end(content);
        // });
        readFile(req, res);
    }else if(req.url.startsWith("/login")){
        // let filePath = path.join(__dirname, "www", req.url);
        // fs.readFile(filePath, "utf8", function (err, content) {
        //     if(err){
        //         res.end("Server Error");
        //     }
        //     res.end(content);
        // });
        readFile(req, res);
    }else{
        res.writeHead(200, {
            "Content-Type": "text/plain; charset=utf-8"
        });
        res.end("没有数据");
    }
    */
    readFile(req, res);
});
// 3.指定监听的端口
server.listen(3000);

function readFile(req, res) {
    let filePath = path.join(__dirname, "www", req.url);
    fs.readFile(filePath, "utf8", function (err, content) {
        if(err){
            res.end("Server Error");
        }
        res.end(content);
    });
}
</script>
```

#### 返回静态资源

**返回的数据要和响应头进行匹配**

```html
<!--
1.响应静态资源
在给浏览器返回数据的时候,
如果没有指定响应头的信息,
如果没有设置返回数据的类型,
那么浏览器不一定能正确的解析
所以无论返回什么类型的静态资源都需要添加对应的响应头信息
-->
<script>
function readFile(req, res) {
    let filePath = path.join(__dirname, "www", req.url);
    // console.log(filePath);
    // 注意点:
    // 1.加载其它的资源不能写utf8
    // 2.如果服务器在响应数据的时候没有指定响应头, 那么在有的浏览器上, 响应的数据有可能无法显示
    let extName = path.extname(filePath);
    let type = mime[extName];
    if(type.startsWith("text")){
        type += "; charset=utf-8;";
    }
    res.writeHead(200, {
        "Content-Type": type
    });
    fs.readFile(filePath, function (err, content) {
        if(err){
            res.end("Server Error");
        }
        res.end(content);
    });
}
</script>
```

#### 封装静态资源获取函数

```js
// let path = require("path");
let fs = require("fs");
let mime = require("./mime.json");

function readFile(req, res, rootPath) {
    let filePath = path.join(rootPath, req.url);
    // console.log(filePath);
    /*
    注意点:
    1.加载其它的资源不能写utf8
    2.如果服务器在响应数据的时候没有指定响应头, 那么在有的浏览器上, 响应的数据有可能无法显示
    * */
    let extName = path.extname(filePath);
    let type = mime[extName];
    if(type.startsWith("text")){
        type += "; charset=utf-8;";
    }
    res.writeHead(200, {
        "Content-Type": type
    });
    fs.readFile(filePath, function (err, content) {
        if(err){
            res.end("Server Error");
        }
        res.end(content);
    });
}

exports.StaticServer = readFile;
```

#### 获取get请求的参数

**url.parse(path,true)将对象中的query转换为一个对象**

```html
<!--
1.如何拿到Get请求传递过来的参数
使用URL模块
使用url.parse(path)将路径转换为对象
url.parse(path,true)将对象中的query转换为一个对象
url.format(urlObject)将对象转换为路径
-->
<script>
let url = require("url");
let http = require("http");

// let str = "http://root:123456@www.it666.com:80/index.html?name=lnj&age=68#banner";
// let obj = url.parse(str, true);
// console.log(obj);
// console.log(obj.query.name);
// console.log(obj.query.age);

// 1.创建一个服务器实例对象
let server = http.createServer();
server.on("request", function (req, res) {
    // console.log(req.url);
    let obj = url.parse(req.url, true);
    res.end(obj.query.name + "----" + obj.query.age);
});
// 3.指定监听的端口
server.listen(3000);
</script>
```

#### 获取post请求的参数

**利用querystring包，调用querystring中的parse方法**

**post每次只能拿到一部分数据，必须通过监听事件的方式获取数据**

```html
<!--
1.如何拿到POST请求传递过来的参数
使用querystring模块
querystring.parse(str[, sep[, eq[, options]]])  将参数转换为对象
querystring.stringify(obj[, sep[, eq[, options]]]) 将对象转换为参数
-->
<script>
let http = require("http");
let queryString = require("querystring");

// 1.创建一个服务器实例对象
let server = http.createServer();
server.on("request", function (req, res) {
    // 1.定义变量保存传递过来的参数
    let params = "";
    // 注意点: 在NODEJS中 ,POST请求的参数我们不能一次性拿到, 必须分批获取
    req.on("data", function (chunk) {
        // 每次只能拿到一部分数据
        params += chunk;
    });
    req.on("end", function () {
        // 这里才能拿到完整的数据
        // console.log(params);
        let obj = queryString.parse(params);
        // console.log(obj.userName);
        // console.log(obj.password);
        res.end(obj.userName + "----" + obj.password);
    });
});
// 3.指定监听的端口
server.listen(3000);
</script>
```

#### 获取请求类型

**通过req.method获取请求的类型**

```html
<!--
1.在服务端如何区分用户发送的是GET请求和POST请求?
通过HTTP模块http.IncomingMessage 类的.method属性
-->
<script>
let http = require("http");9

// 1.创建一个服务器实例对象
let server = http.createServer();
server.on("request", function (req, res) {
    // console.log(req.method);
    res.writeHead(200, {
        "Content-Type": "text/plain; charset=utf-8"
    });
    if(req.method.toLowerCase() === "get"){
        res.end("利用GET请求的方式处理参数");
    }else if(req.method.toLowerCase() === "post"){
        res.end("利用POST请求的方式处理参数");
    }
});
// 3.指定监听的端口
server.listen(3000);
</script>
```

#### 返回动态网页

```js
let http = require("http");
let path = require("path");
let fs = require("fs");
let url = require("url");
let queryString = require("querystring");
let template = require("art-template");

let persons = {
    "lisi": {
        name: "lisi",
        gender: "male",
        age: "33"
    },
    "zhangsan": {
        name: "zhangsan",
        gender: "female",
        age: "18"
    }
};

// 1.创建一个服务器实例对象
let server = http.createServer();
// 2.注册请求监听
server.on("request", function (req, res) {
   if(req.url.startsWith("/index") && req.method.toLowerCase() === "get"){
       let obj = url.parse(req.url);
       let filePath = path.join(__dirname, obj.pathname);
       fs.readFile(filePath, "utf8", function (err, content) {
           if(err){
               res.writeHead(404, {
                   "Content-Type": "text/plain; charset=utf-8"
               });
               res.end("Page Not Found");
           }
           res.writeHead(200, {
               "Content-Type": "text/html; charset=utf-8"
           });
           res.end(content);
       });
   }
   else if(req.url.startsWith("/info") && req.method.toLowerCase() === "post"){
       let params = "";
       req.on("data", function (chunk) {
           params += chunk;
       });
       req.on("end", function () {
           let obj = queryString.parse(params);
           let per = persons[obj.userName];
           // console.log(per);
           let filePath = path.join(__dirname, req.url);
           /*
           fs.readFile(filePath, "utf8", function (err, content) {
               if(err){
                   res.writeHead(404, {
                       "Content-Type": "text/plain; charset=utf-8"
                   });
                   res.end("Page Not Found");
               }
               content = content.replace("!!!name!!!", per.name);
               content = content.replace("!!!gender!!!", per.gender);
               content = content.replace("!!!age!!!", per.age);
               res.end(content);
           });
            */
           let html = template(filePath, per);
           res.writeHead(200, {
               "Content-Type": "text/html; charset=utf-8"
           });
           res.end(html);
       });
   }
});
// 3.指定监听的端口
server.listen(3000);
```

