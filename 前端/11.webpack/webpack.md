# webpack

## webpack基础核心

### 开篇

**注意新版的webpack在require要使用相对路径，打包的文件也需要用相对路径表示**

```html
<!--
1.什么是webpack?
webpack是一套基于NodeJS的"模块打包工具",
在webpack刚推出的时候就是一个单纯的JS模块打包工具,可以将多个模块的JS文件合并打包到一个文件中
但是随着时间的推移、众多开发者的追捧和众多开发者的贡献
现在webpack不仅仅能够打包JS模块, 还可以打包CSS/LESS/SCSS/图片等其它文件

2.为什么要分模块?
如果将所有的JS代码都写到一个文件中, 十分不利于代码的维护和复用
所以我们可以将不同的功能写到不同的模块中, 这样就提升了代码的维护性和复用性
但是当将代码写到不同模块时新的问题又出现了,
例如: 导入资源变多了, 请求次数变多了, 网页性能也就差了
例如: 不同功能都放到了不同模块中了, 那么如何维护模块之间的关系也变成一个难题了
    <script src="./header.js"></script>
    <script src="./content.js"></script>
    <script src="./index.js"></script>
    <script src="./footer.js"></script> // 如果index.js中用到了footer,就会报错
例如: ... ...

3.如何解决上述问题
3.1项目上线时将用到的所有模块都合并到一个文件中
3.2在index.html中只导入主文件, 再主文件中再导入依赖模块

4.如何通过webpack来打包JS模块
4.1安装webpack
npm init -y
npm install --save-dev webpack
npm install --save-dev webpack-cli
4.2在终端中输入打包的指令
npx webpack ./index.js
注意点:
index.js就是需要打包的文件
打包之后的文件会放到dist目录中, 名称叫做main.js
-->
```

### 配置文件

**如果通过webpack.config.js进行打包，则只需要npx webpack，不用指定文件**

```html
<!--
1.什么是webpack配置文件?
我们在打包JS文件的时候需要输入:  npx webpack index.js
这句指令的含义是: 利用webpack将index.js和它依赖的模块打包到一个文件中
其实在webpack指令中除了可以通过命令行的方式告诉webpack需要打包哪个文件以外,
还可以通过配置文件的方式告诉webpack需要打包哪个文件

2.webpack常见配置
entry: 需要打包的文件
output: 打包之后输出路径和文件名称
mode: 打包模式  development/production
development: 不会压缩打包后的JS代码
production:  会自动压缩打包后的JS代码
-->
<script>
    //webpack.config.js
    
const path = require("path");
module.exports = {
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    }
};
</script>
```

### 配置文件注意点

```html
<!--
1.webpack配置注意事项
配置文件的名称必须叫做: webpack.config.js, 否则直接输入 npx webpack打包会出错
如果要使用其它名称, 那么在输入打包命令时候必须通过 --config 指定配置文件名称
npx webpack --config xxx

2.打包命令简化
每次输入npx webpack --config xxx来打包文件会有一点蛋疼, 所以我们可以通过npm script来简化这个操作
通过设置package.json中的script来简化指令，通过npm run xxx来执行
-->
```

### sourcemao配置

**存储打包之前和打包之后的错误额映射关系**

**通过webpack.config.js中的devtool属性进行配置**

```html
<!--
1.什么是sourcemap?
webpack打包后的文件会自动添加很多代码, 在开发过程中非常不利于我们去调试
因为如果运行webpack打包后的代码,错误提示的内容也是打包后文件的内容
所以为了降低调试的难度, 提高错误代码的阅读性, 我们就需要知道打包后代码和打包之前代码的映射关系
只要有了这个映射关系我们就能很好的显示错误提示的内容, 存储这个映射关系的文件我们就称之为sourcemap

2.如何开启sourcemap
https://www.webpackjs.com/configuration/devtool/
2.1在webpack.config.js中添加
devtool: "xxx",
2.2各配置项说明:
eval:
不会单独生成sourcemap文件, 会将映射关系存储到打包的文件中, 并且通过eval存储
优势: 性能最好
缺点: 业务逻辑比较复杂时候提示信息可能不全面不正确

source-map:
会单独生成sourcemap文件, 通过单独文件来存储映射关系
优势: 提示信息全面,可以直接定位到错误代码的行和列
缺点: 打包速度慢

inline:
不会单独生成sourcemap文件, 会将映射关系存储到打包的文件中, 并且通过base64字符串形式存储

cheap:
生成的映射信息只能定位到错误行不能定位到错误列

module:
不仅希望存储我们代码的映射关系, 还希望存储第三方模块映射关系, 以便于第三方模块出错时也能更好的排错

2.3企业开发配置:
development: cheap-module-eval-source-map
只需要行错误信息, 并且包含第三方模块错误信息, 并且不会生成单独sourcemap文件

production: cheap-module-source-map
只需要行错误信息, 并且包含第三方模块错误信息, 并且会生成单独sourcemap文件
-->

<script>
const path = require("path");
module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output:  
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    }
};
</script>
```

### file-loader打包图片

**file-loader处理之后我们导入图片拿到的是图片打包之后的地址**

```html
<!--
1.什么是loader?
webapck的本质是一个模块打包工具, 所以webpack默认只能处理JS文件,不能处理其他文件,
因为其他文件中没有模块的概念, 但是在企业
开发中我们除了需要对JS进行打包以外,
还有可能需要对图片/CSS等进行打包, 所以为了能够让webpack能够对其它的文件类型进行打包,
在打包之前就必须将其它类型文件转换为webpack能够识别处理的模块,
用于将其它类型文件转换为webpack能够识别处理模块的工具我们就称之为loader

2.如何使用loader
webpack中的loader都是用NodeJS编写的, 但是在企业开发中我们完全没有必要自己编写,
因为已经有众多大神帮我们编写好了企业中常用的loader, 我们只需要安装、配置、使用即可

2.1通过npm安装对应的loader
2.2按照loader作者的要求在webpack进行相关配置
2.3使用配置好的loader

3.fileloader使用
https://www.webpackjs.com/loaders/file-loader/
3.1安装file-loader
npm install --save-dev file-loader
3.2在webpack.config.js中配置file-loader
module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'file-loader',
            options: {}
          }
        ]
      }
    ]
  }
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {}
                    }
                ]
            }
        ]
    }
};
</script>

<!-- bundle.js -->
<script>
const icon = require("./lnj.jpg");
// file-loader处理之后我们导入图片拿到的是图片打包之后的地址
console.log(icon);

let oImg = document.createElement("img");
oImg.src = icon;
document.body.appendChild(oImg);
</script>
```

### file-loader其他配置

**其他的配置是放在options数组里面的**

```html
<!--
注意点:
默认情况下fileloader生成的图片名就是文件内容的 MD5 哈希值
如何想打包后不修改图片的名称, 那么可以新增配置  name: "[name].[ext]"
其它命名规则详见: placeholders

注意点:
默认情况下fileloader会将生成的图片放到dist根目录下面
如果想打包之后放到指定目录下面, 那么可以新增配置 outputPath: "images/"

注意点:
如果需要将图片托管到其它服务器, 那么只需在打包之前配置 publicPath: "托管服务器地址"即可
-->
<!-- webpack.config.js -->
<script>
const path = require("path");

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                            // 指定托管服务器地址(统一替换图片地址)
                            publicPath: 'http://www.it666.com/images/'
                        }
                    }
                ]
            }
        ]
    }
};
</script>
```

### url-loader打包图片

**和file-loader相似，但是可以将小图片转换为base64的字符串**

```html
<!--
1.urlloader
url-loader 功能类似于 file-loader，
但是在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL

2.urlloader使用
https://www.webpackjs.com/loaders/url-loader/
2.1安装urlloader
npm install --save-dev url-loader
2.2配置urlloader
{
    test: /\.(png|jpg|gif)$/,
    use: [
        {
            loader: 'url-loader',
            options: {
                name: "[name].[ext]",
                outputPath: "/images",
                limit: 1024
            }
        }
    ]
}

优势:
图片比较小的时候直接转换成base64字符串图片, 减少请求次数
图片比较大的时候由于生成的base64字符串图片也比较大, 就保持原有的图片
-->
<!-- webpack.config.js -->
<script>
const path = require("path");

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            /*
                            limit: 指定图片限制的大小
                            如果被打包的图片超过了限制的大小, 就会将图片保存为一个文件
                            如果被打包的图片没有超过限制的大小, 就会将图片转换成base64的字符串
                            注意点:
                            对于比较小的图片, 我们将图片转换成base64的字符串之后, 可以提升网页的性能(因为减少了请求的次数)
                            对于比较大的图片, 哪怕我们将图片转换成了base64的字符串之后, 也不会提升网页的性能, 还有可能降低网页的性能
                            (因为图片如果比较大, 那么转换之后的字符串也会比较多, 那么网页的体积就会表达, 那么访问的速度就会变慢)
                            * */
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            }
        ]
    }
};
</script>
```

### css-loader打包css文件

**打包好的文件中直接在js中require css文件即可**

```html
<!--
1.css-loader
和图片一样webpack默认能不能处理CSS文件, 所以也需要借助loader将CSS文件转换为webpack能够处理的类型

2.css-loader使用:
2.1安装cssloader
npm install --save-dev css-loader
2.2安装styleloader
npm install style-loader --save-dev
2.3配置css-loader
{
    test: /\.css$/,
    use: [ 'style-loader', 'css-loader' ]
}

3.css-loader和style-loader作用
css-loader:   解析css文件中的@import依赖关系
style-loader: 将webpack处理之后的内容插入到HTML的HEAD代码中
-->
<!-- webapck.config.js -->
<script>
const path = require("path");

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            /*
                            limit: 指定图片限制的大小
                            如果被打包的图片超过了限制的大小, 就会将图片保存为一个文件
                            如果被打包的图片没有超过限制的大小, 就会将图片转换成base64的字符串
                            注意点:
                            对于比较小的图片, 我们将图片转换成base64的字符串之后, 可以提升网页的性能(因为减少了请求的次数)
                            对于比较大的图片, 哪怕我们将图片转换成了base64的字符串之后, 也不会提升网页的性能, 还有可能降低网页的性能
                            (因为图片如果比较大, 那么转换之后的字符串也会比较多, 那么网页的体积就会表达, 那么访问的速度就会变慢)
                            * */
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                /*
                css-loader:   解析css文件中的@import依赖关系
                style-loader: 将webpack处理之后的内容插入到HTML的HEAD代码中
                * */
                use: [ 'style-loader', 'css-loader' ]
            }
        ]
    }
};
</script>
```

### loader的相关注意点

```html
<!--
4.loader特点:
4.1单一原则, 一个loader只做一件事情
4.2多个loader会按照从右至左, 从下至上的顺序执行
例如: 从右至左
      [ 'style-loader', 'css-loader' ]
      先执行css-loader解析css文件关系拿到所有内容,
      再执行style-loader将内容插入到HTML的HEAD代码中
例如: 从下至上
    [{
        loader: "style-loader"
    },{
        loader: "css-loader"
    }]
    先执行css-loader解析css文件关系拿到所有内容,
    再执行style-loader将内容插入到HTML的HEAD代码中
-->
```

### es6模块化

**通过export {}导出**

**通过import {}导入：使用的是解构赋值**

**通过export default XXX导出**

**通过import XXX导入**

```html
<!--
1.ES6模块化
在ES6出现之前，JS不像其他语言拥有“模块化”这一概念，于是为了支持JS模块化
我们使用类、立即执行函数或者第三方插件(RequireJS、seaJS)来实现模块化
但是在ES6出现之后, 上述解决方案都已经被废弃, 因为ES6中正式引入了模块化的概念

ES6模块化模块和NodeJS中一样, 一个文件就是一个模块, 模块中的数据都是私有的
ES6模块化模块和NodeJS中一样, 可以通过对应的关键字暴露模块中的数据,
                             可以通过对应的关键字导入模块, 使用模块中暴露的数据

2.ES6模块化使用
2.1常规导出
2.1.1分开导入导出
export xxx;
import {xxx} from "path";

2.1.2一次性导入导出
export {xxx, yyy, zzz};
import {xxx, yyy, zzz} from "path";

注意点:
接收导入变量名必须和导出变量名一致
如果想修改接收变量名可以通过 xxx as newName方式
变量名被修改后原有变量名自动失效

2.2默认导入导出
export default xxx;
import xxx from "path";

注意点:
一个模块只能使用一次默认导出, 多次无效
默认导出时, 导入的名称可以和导出的名称不一致
-->
<script>
/*
ES6模块化的第一种方式
导出数据: export {xxx};
导入数据: import {xxx} from "path";
* */
/*
注意点:
1.如果是通过export {xxx};方式导出数据, 那么在导入接收的时候接收的变量名称必须和导出的名称一致
  究其原因是因为在导入的时候本质上是ES6的解构赋值
2.如果是通过export {xxx};方式导出数据, 又想在导入数据的时候修改接收的变量名称, 那么可以使用as来修改
  但是如果通过as修改了接收的变量名称, 那么原有的变量名称就会失效
* */
/*
// import {name} from "./a.js";
import {str} from "./a.js";

console.log(str);
 */
/*
let obj = {
    name: "lnj",
    age: 34
};
// let {name, age} = obj;
// console.log(name);
let {str, age} = obj;
console.log(str);
console.log(age);
 */
/*
import {name as str} from "./a.js";

console.log(name);
console.log(str);
 */


/*
ES6模块化的第二种方式
导出数据: export default xxx;
导入数据: import xxx from "path";
* */
/*
注意点:
1.如果是通过export default xxx;导出数据, 那么在接收导出数据的时候变量名称可以和导出的明白不一致
2.如果是通过export default xxx;导出数据, 那么在模块中只能使用一次export default
* */
/*
// import name from "./b.js";
import str from "./b.js";

console.log(str);
 */
/*
import name from "./b.js";
import age from "./b.js";
console.log(name);
console.log(age);
 */
/*
import {name, age} from "./a.js";

console.log(name);
console.log(age);
 */

/*
// 两种方式混合使用
import Person,{name, age, say} from "./c.js";
let p = new Person();
console.log(p);

console.log(name);
console.log(age);
say();
 */

// const icon = require("./lnj.jpg");
// const _ = require("./index.css");

//通过es6的模块化对代码进行修改
import icon from "./lnj.jpg";
import "./index.css";

let oImg = document.createElement("img");
oImg.src = icon;
oImg.setAttribute("class", "size");
document.body.appendChild(oImg);
</script>
```

### less-loader

```html
<!--
1.less-loader
自动将less转换为CSS

2.less-loader使用:
2.0安装less
npm install --save-dev less
2.1安装less-loader
npm install --save-dev less-loader
2.2配置less-loader
{
    test: /\.less$/,
    use: [{
        loader: "style-loader"
    }, {
        loader: "css-loader"
    }, {
        loader: "less-loader"
    }]
}

注意点:
因为loader是从右至左从下至上,所以必须先由less-loader处理往后才能交给其他loader处理
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                /*
                css-loader:   解析css文件中的@import依赖关系
                style-loader: 将webpack处理之后的内容插入到HTML的HEAD代码中
                * */
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader",
                    },
                    {
                        loader: "css-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader" // creates style nodes from JS strings
                }, {
                    loader: "css-loader" // translates CSS into CommonJS
                }, {
                    loader: "less-loader" // compiles Less to CSS
                }]
            }
        ]
    }
};
</script>
```

### sass-loader

```html
<!--
1.scss-loader
自动将scss转换为CSS

2.scss-loader使用:
2.0安装scss
npm install --save-dev node-sass
2.1安装less-loader
npm install --save-dev sass-loader
2.2配置less-loader
{
  test: /\.scss$/,
  use: [{
      loader: "style-loader" // 将 JS 字符串生成为 style 节点
  }, {
      loader: "css-loader" // 将 CSS 转化成 CommonJS 模块
  }, {
      loader: "sass-loader" // 将 Sass 编译成 CSS
  }]
}

注意点:
因为loader是从右至左从下至上,所以必须先由sass-loader处理往后才能交给其他loader处理
-->

<!-- webpack.config.js -->
<script>
const path = require("path");

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                /*
                css-loader:   解析css文件中的@import依赖关系
                style-loader: 将webpack处理之后的内容插入到HTML的HEAD代码中
                * */
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader",
                    },
                    {
                        loader: "css-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader" // creates style nodes from JS strings
                }, {
                    loader: "css-loader" // translates CSS into CommonJS
                }, {
                    loader: "less-loader" // compiles Less to CSS
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader" // 将 JS 字符串生成为 style 节点
                }, {
                    loader: "css-loader" // 将 CSS 转化成 CommonJS 模块
                }, {
                    loader: "sass-loader" // 将 Sass 编译成 CSS
                }]
            }
        ]
    }
};
</script>
```

### postcss-loader

**自动添加浏览器私有前缀**

```html
<!--
1.什么是PostCSS?
https://www.postcss.com.cn/
PostCSS和sass/less不同, 它不是CSS预处理器
PostCSS是一款使用插件去转换CSS的工具，
PostCSS有许多非常好用的插件
例如
autoprefixer(自动补全浏览器前缀)
postcss-pxtorem(自动把px代为转换成rem)
... ...
-->

<!--
2.使用PostCSS自动补全浏览器前缀
2.1安装postcss-loader
npm i -D postcss-loader
2.2安装需要的插件
npm i -D autoprefixer
2.3配置postcss-loader
在css-loader or less-loader or sass-loader之前添加postcss-loader
2.4创建postcss-loader配置文件
postcss.config.js
https://github.com/browserslist/browserslist#queries
也可以通过can i use这个网站查询各主流浏览器的兼容版本
2.5在配置文件中配置autoprefixer
module.exports = {
    plugins: {
        "autoprefixer": {
            "overrideBrowserslist": [
                // "ie >= 8", // 兼容IE7以上浏览器
                // "Firefox >= 3.5", // 兼容火狐版本号大于3.5浏览器
                // "chrome  >= 35", // 兼容谷歌版本号大于35浏览器,
                // "opera >= 11.5" // 兼容欧朋版本号大于11.5浏览器,
                "chrome  >= 36", // 如果需要适配的浏览器完全兼容则不会添加前缀
            ]
        }
    }
};
-->

<!-- webpack.config.js -->
<script>
const path = require("path");

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                /*
                css-loader:   解析css文件中的@import依赖关系
                style-loader: 将webpack处理之后的内容插入到HTML的HEAD代码中
                * */
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader",
                    },
                    {
                        loader: "css-loader"
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                }]
            }
        ]
    }
};
</script>

<!-- postcss.config.js -->
<script>
module.exports = {
    plugins: {
        "autoprefixer": {
            "overrideBrowserslist": [
                // "ie >= 8", // 兼容IE7以上浏览器
                // "Firefox >= 3.5", // 兼容火狐版本号大于3.5浏览器
                // "chrome  >= 35", // 兼容谷歌版本号大于35浏览器,
                // "opera >= 11.5" // 兼容欧朋版本号大于11.5浏览器,
                "chrome  >= 36",
            ]
        }
    }
};
</script>
```

**自动将px转换为rem适配移动端**

```html
<!--
3.使用PostCSS自动将px转换成rem
https://www.npmjs.com/package/postcss-pxtorem

3.1安装postcss-pxtorem
npm install postcss-pxtorem -D
3.2在配置文件中配置postcss-pxtorem
"postcss-pxtorem": {
            rootValue: 100, // 根元素字体大小
            // propList: ['*'] // 可以从px更改到rem的属性
            propList: ["height"]
        }

rootValue (Number) root 元素的字体大小。
unitPrecision (Number) 允许REM单位增长到的十进制数。
propList ( array ) 可以从px更改到rem的属性。
    值需要精确匹配。
    使用通配符 * 启用所有属性。 示例：['*']
    在单词的开头或者结尾使用 *。 ( ['*position*'] 将匹配 background-position-y )
    使用 与属性不匹配。! 示例：['*','letter-spacing']!
    将"非"前缀与其他前缀合并。 示例：['*','font*']!
selectorBlackList ( array ) 要忽略和离开的选择器。
    如果值为字符串，它将检查选择器是否包含字符串。
    ['body'] 将匹配 .body-class
    如果值为 regexp，它将检查选择器是否匹配正则表达式。
    [/^body$/] 将匹配 body，但不匹配 .body
replace (Boolean) 替代包含rems的规则，而不是添加回退。
mediaQuery (Boolean) 允许在媒体查询中转换 px。
minPixelValue (Number) 设置要替换的最小像素值。

突然感觉淡淡不忧伤了, 过去麻烦的事webpack都帮我们做完了
-->
<!-- webpack.config.js和上述一样 -->
<!-- postcss.config.js -->
<script>
module.exports = {
    plugins: {
        "autoprefixer": {
            "overrideBrowserslist": [
                "ie >= 8", // 兼容IE7以上浏览器
                "Firefox >= 3.5", // 兼容火狐版本号大于3.5浏览器
                "chrome  >= 35", // 兼容谷歌版本号大于35浏览器,
                "opera >= 11.5" // 兼容欧朋版本号大于11.5浏览器,
            ]
        },
        "postcss-pxtorem": {
            rootValue: 100, // 根元素字体大小
            // propList: ['*'] // 可以从px更改到rem的属性
            propList: ["height"]
        }
    }
};
</script>
```

### css-loader模块化

```html
<!--
1.默认情况下通过import "./xxx.css"导入的样式是全局样式
也就是只要被导入, 在其它文件中也可以使用
如果想要导入的CSS文件只在导入的文件中有效, 那么就需要开启CSS模块化
{
    loader: "css-loader",
    options: {
        modules: true // 开启CSS模块化
    }
}
然后在导入的地方通过 import xxx from "./xxx.css"导入
然后在使用的地方通过 xxx.className方式使用即可
-->

<!-- index.js -->
<script>
import icon from "./lnj.jpg";
// import "./index.css"
import cssModule from "./index.css"
import {addImage} from "./custom.js";

console.log(cssModule);
let oImg = document.createElement("img");
oImg.src = icon;
// oImg.setAttribute("class", "size");
oImg.setAttribute("class", cssModule.size);
document.body.appendChild(oImg);

addImage();
</script>

<!-- webpack.config.js -->
<script>
const path = require("path");

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader"
                    },
                    {
                        loader: "css-loader",
                        options: {
                            modules: true
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    }
};
</script>
```

### webpack打包字体图标

**用file-loader来处理**

```html
<!--
1.如何打包字体图标
字体图标中也用到了url用到了文件, 所以我们需要通过file-loader来处理字体图标文件
{
    test: /\.(eot|svg|ttf|woff|woff2)$/,
    use:[{
        loader: "file-loader",
        options: {
            name: "[name].[ext]",
            outputPath: "font/",
        }
    }]
}
-->

<!-- webpack.config.js -->
<script>
const path = require("path");

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader"
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    }
};
</script>
```

### html-plugin

**自动将指定的html（或者默认的）中引入打包好的js文件**

```html
<!--
1.什么是插件(plugin)?
plugin 用于扩展webpack的功能
当然loader也是变相的扩展了webpack ，但是它只专注于转化文件这一个领域。
而plugin的功能更加的丰富，而不仅局限于资源的加载。
-->
<!--
2.什么是HtmlWebpackPlugin?
HtmlWebpackPlugin会在打包结束之后自动创建一个index.html, 并将打包好的JS自动引入到这个文件中

3.HtmlWebpackPlugin使用
https://www.webpackjs.com/plugins/html-webpack-plugin/
3.1安装HtmlWebpackPlugin
npm install --save-dev html-webpack-plugin
3.2配置HtmlWebpackPlugin
const HtmlWebpackPlugin = require('html-webpack-plugin');
plugins: [new HtmlWebpackPlugin()]

4.HtmlWebpackPlugin高级使用
https://github.com/jantimon/html-webpack-plugin#configuration
默认情况下HtmlWebpackPlugin生成的html文件是一个空的文件,
如果想指定生成文件中的内容可以通过配置模板的方式来实现
 plugins: [new HtmlWebpackPlugin({
        template: "index.html"
    })]

默认情况下生成html文件并没有压缩,
如果想让html文件压缩可以设置
new HtmlWebpackPlugin({
    template: "index.html",
    minify: {
        collapseWhitespace: true
    }
})]
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024 * 100,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader"
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            collapseWhitespace: true,
        }
    })]
};
</script>
```

### clean-plugin

```html
<!--
1.什么是clean-webpack-plugin?
webpack-clean-plugin会在打包之前将我们指定的文件夹清空
应用场景每次打包前将dist目录清空, 然后再存放新打包的内容, 避免新老混淆问题

3.clean-webpack-plugin使用
https://github.com/johnagan/clean-webpack-plugin
3.1安装clean-webpack-plugin
npm install --save-dev clean-webpack-plugin
3.2配置clean-webpack-plugin
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
plugins: [new CleanWebpackPlugin()]
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader"
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin()]
};
</script>
```

### copy-plugin

```html
<!--
1.什么是copy-webpack-plugin?
在打包项目的时候除了JS/CSS/图片/字体图标等需要打包以外, 可能还有一些相关的文档也需要打包
文档内容是固定不变的, 我们只需要将对应的文件拷贝到打包目录中即可
那么这个时候我们就可以使用copy-plugin来实现文件的拷贝

3.copy-webpack-plugin使用
https://www.webpackjs.com/plugins/copy-webpack-plugin/
3.1安装copy-webpack-plugin
npm install --save-dev copy-webpack-plugin
3.2配置copy-webpack-plugin
const CopyWebpackPlugin = require('copy-webpack-plugin');
plugins: [new CopyWebpackPlugin([
            {from:"doc", to:"./doc"}
        ])];
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader"
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }])
    ]
};
</script>
```

### 项目目录结构调整

```html
<!--
|-----项目文件
|-------|src
|----------|css
|----------|js
|----------|images
|----------|index.html
然后修改一下webpack.config.js的入口和出口配置即可
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                // use: [ 'style-loader', 'css-loader' ]
                use:[
                    {
                        loader: "style-loader"
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }])
    ]
};
</script>
```

### css-plugin

```html
<!--
1.什么是mini-css-extract-plugin?
mini-css-extract-plugin是一个专门用于将打包的CSS内容提取到单独文件的插件
> 前面我们通过style-loader打包的CSS都是直接插入到head中的

2.mini-css-extract-plugin使用
https://webpack.js.org/plugins/mini-css-extract-plugin/
2.1mini-css-extract-plugin安装
npm install --save-dev mini-css-extract-plugin
2.2配置mini-css-extract-plugin
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
new MiniCssExtractPlugin({
    filename: './css/[name].css',
})
2.3替换style-loader
loader: MiniCssExtractPlugin.loader,

注意点: 如果相关文件资源无法显示, 需要根据打包后的结构手动设置公开路径
options: {
    publicPath: "xxx"
}
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
    ]
};
</script>
```

### 压缩CSS代码

```html
<!--
3.mini-css-extract-plugin压缩css
https://www.npmjs.com/package/mini-css-extract-plugin
3.1安装JS代码压缩插件
npm install --save-dev terser-webpack-plugin
3.2安装CSS代码压缩插件
npm install --save-dev optimize-css-assets-webpack-plugin
3.3导入插件
const TerserJSPlugin = require('terser-webpack-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
3.4配置webpack优化项
optimization: {
    minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
}
注意: 由于配置了webpack的optimization.minimizer项目会覆盖默认的JS压缩选项,
所以JS代码也需要通过插件自己压缩
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');

module.exports = {
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "production", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
    ]
};
</script>
```

### watch(一般不使用，使用下面的方法)

```html
<!--
1.什么是watch?
webpack 可以监听打包文件变化，当它们修改后会重新编译打包
那么如何监听打包文件变化呢?  使用 watch

2.watch相关配置watchOptions
poll: 1000 // 每隔多少时间检查一次变动
aggregateTimeout:  // 防抖, 和函数防抖一样, 改变过程中不重新打包, 只有改变完成指定时间后才打包
ignored: 排除一些巨大的文件夹, 不需要监控的文件夹, 例如 node_modules
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');

module.exports = {
    watch: true,
    watchOptions: {
        aggregateTimeout: 300, // 防抖, 和函数防抖一样, 改变过程中不重新打包, 只有改变完成指定时间后才打包
        poll: 1000, // 每隔多少时间检查一次变动
        ignored: /node_modules/ // 排除一些巨大的文件夹, 不需要监控的文件夹
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "production", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
    ]
};
</script>
```

### webpack-dev-server

```html
<!--
1.什么是webpack-dev-server?
webpack-dev-server和watch一样可以监听文件变化
webpack-dev-server可以将我们打包好的程序运行在一个服务器环境下
webpack-dev-server可以解决企业开发中"开发阶段"的跨域问题

2.webpack-dev-server使用
2.1安装webpack-dev-server
https://www.npmjs.com/package/webpack-dev-server
npm install webpack-dev-server --save-dev
2.2配置webpack-dev-server
devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090
    }
"start": "npx webpack-dev-server --config webpack.config.js"
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');

module.exports = {
    /*
    watch: true,
    watchOptions: {
        aggregateTimeout: 300, // 防抖, 和函数防抖一样, 改变过程中不重新打包, 只有改变完成指定时间后才打包
        poll: 1000, // 每隔多少时间检查一次变动
        ignored: /node_modules/ // 排除一些巨大的文件夹, 不需要监控的文件夹
    },
     */
    devServer: {
        contentBase: "./bundle", // 打包后的目录
        open: true, // 是否自动在浏览器中打开
        port: 9090 // 服务器端口号
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "production", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
    ]
};
</script>
```

### webpack-CORS:解决跨域问题

```html
<!--
1.前端跨域问题?
同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能
所谓同源是指: 协议，域名，端口都相同,就是同源, 否则就是跨域
http://127.0.0.1:8080
http://127.0.0.1:8080  // 同源

http://127.0.0.1:8080
http://127.0.0.1:9090  // 跨域

2.利用webpack-dev-server代理解决跨域问题
devServer: {
        contentBase: "./dist",
        open: true,
        port: 9090,
        proxy: {
            // 所有API开头的请求都会被代理到target
            // 例如: 我们发送请求地址: http://127.0.0.1:9090/api
            //       实际发送请求地址: http://127.0.0.1:3000/api
            "/api": {
                target: "http://127.0.0.1:3000", // 代理地址
                changeOrigin: true,     // 域名跨域
                secure: false,          // https跨域
            }
        }
    }

devServer: {
    contentBase: "./dist",
    open: true,
    port: 9090,
    proxy: [{
            context:["/api", "/login"],
            target: "http://127.0.0.1:3000", // 代理地址
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
    }]
}

3.利用webpack-dev-server重写请求路径
proxy: [{
        context:["/api", "/login"],
        target: "http://127.0.0.1:3000", // 代理地址
        changeOrigin: true,     // 域名跨域
        secure: false,          // https跨域
        pathRewrite:{"^/api": ""} // 路径重写, 将路径中的api替换为空
}]
-->
<!--
常用配置附录
target：要使用url模块解析的url字符串
forward：要使用url模块解析的url字符串
agent：要传递给http（s）.request的对象（请参阅Node的https代理和http代理对象）
ssl：要传递给https.createServer（）的对象
ws：true / false，是否代理websockets
xfwd：true / false，添加x-forward标头
secure：true / false，是否验证SSL Certs
toProxy：true / false，传递绝对URL作为路径（对代理代理很有用）
prependPath：true / false，默认值：true - 指定是否要将目标的路径添加到代理路径
ignorePath：true / false，默认值：false - 指定是否要忽略传入请求的代理路径（注意：如果需要，您必须附加/手动）。
localAddress：要为传出连接绑定的本地接口字符串
changeOrigin：true / false，默认值：false - 将主机标头的原点更改为目标URL
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');

module.exports = {
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        /*
        proxy: {
            // 以下配置的含义:
            // 当我们在代码中发送请求到/user的时候, devServer就会自动将我们请求的地址替换为
            // http://127.0.0.1:3000/user
            "/user": {
                target: "http://127.0.0.1:3000",
                changeOrigin: true,     // 域名跨域
                secure: false,          // https跨域
            },
            "/login": {
                target: "http://127.0.0.1:3000",
                changeOrigin: true,     // 域名跨域
                secure: false,          // https跨域
            },
        }
         */
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }]
        /*
        注意点:
        devServer只能解决开发阶段的跨域问题, 并不能解决项目上线之后的跨域问题
        原因非常简单, 因为项目上线之后是将打包好的文件上传到服务器, 而打包好的文件中并没有devServer
        所以项目上线之后要想解决跨域问题还是需要依赖后端开发人员
        * */
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "production", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
    ]
};
</script>
```

### webpack-HMR-热更新

```html
<!--
1.什么是HMR?
1.1通过webpack-dev-server自动打包并没有真正的放到指定的目录中
因为读写磁盘是非常耗时和消耗性能的,
所以为了提升性能webpack-dev-server将转换好的内容直接放到了内存中
1.2通过webpack-dev-server可以实现实时监听打包内容的变化,
每次打包之后都会自动刷新网页, 但是正是因为每当内容被修改时都会自动刷新网页
所以给我们带来了很多不便, 这时就需要通过HMR插件来优化调试开发
1.3HMR(HotModuleReplacementPlugin)热更新插件,
会在内容发生改变的时候时时的更新修改的内容但是不会重新刷新网站

2.HMR使用:
HotModuleReplacementPlugin是一个内置插件, 所以不需要任何安装直接引入webpack模块即可使用
2.1在devServer中开启热更新
hot: true, // 开启热更新
hotOnly: true // 即使热更新不生效，浏览器也不自动刷新
2.2在webpack.config.js中创建热更新插件
new Webpack.HotModuleReplacementPlugin()

3.注意点:
如果是通过style-loader来处理CSS, 那么经过前面两步就已经实现了热更新
如果是通过MiniCssExtractPlugin.loader来处理CSS, 那么还需要额外配置MiniCssExtractPlugin.loader
options:{
    hmr: true
}
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require("webpack");

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options:{
                            hmr: true
                        }
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>
```

### webpack-HMR-热更新处理js

```html
<!--
1.JS模块使用HMR注意点?
import "./index.less"
对于css模块而言, 在css-loader中已经帮我们实现了热更新, 只要css代码被修改就会立即更新
import copy from "./test.js"
但是对于js模块而言, 系统默认并没有给我们实现热更新, 所以修改了js模块代码并不会立即更新

2.JS模块如何实现热更新?
2.1手动监听模块变化
if(module.hot){ // 判断是否开启热更新
    module.hot.accept("./test.js", function () { // 监听指定JS模块变化

    });
}
2.2手动编写模块变化后的业务逻辑
if(module.hot){
    module.hot.accept("./test.js", function () {
        let div = document.querySelector("div");
        console.log(div);
        document.body.removeChild(div);
        copy();
    });
}
-->
<!-- index.js -->
<script>
//在导入js文件的主文件中加入下面的代码，手动实现js的热更新
// 判断当前有没有开启热更新
if(module.hot){
    // 告诉热更新需要监听哪一个JS模块的变化
    module.hot.accept("./test.js", function () {
        let oSpan = document.querySelector("span");
        document.body.removeChild(oSpan);
        addSpan();
    });
}
</script>
```

### webpack-babel-转换ES678语法

```html
<!--
1.webpack-ES6语法处理
在企业开发中为了兼容一些低级版本的浏览器, 我们需要将ES678高级语法转换为ES5低级语法
否则在低级版本浏览器中我们的程序无法正确执行
默认情况下webpack是不会将我们的代码转换成ES5低级语法的, 如果需要转换我们需要使用babel来转换

2.如何使用babel?
https://babeljs.io/
2.1安装转换到ES5的相关包
npm install --save-dev babel-loader @babel/core  @babel/preset-env
2.2配置babel
{
    test: /\.js$/,
    exclude: /node_modules/,  // 不做处理的目录
    loader: "babel-loader",
    options: {
        presets: ["@babel/preset-env"],
    },
}
-->
<!--
3.presets优化:
在实际企业开发中默认情况下babel会将所有高于ES5版本的代码都转换为ES5代码
但是有时候可能我们需要兼容的浏览器已经实现了更高版本的代码, 那么这个时候我们就不需要转换
因为如果浏览器本身已经实现了, 我们再去转换就会增加代码的体积,就会影响到网页的性能
所以我们通过配置presets的方式来告诉webpack我们需要兼容哪些浏览器
然后babel就会根据我们的配置自动调整转换方案, 如果需要兼容的浏览器已经实现了, 就不转换了
https://babeljs.io/docs/en/babel-preset-env
presets: [["@babel/preset-env",{
            targets: {
                "chrome": "58",
                "ie": "10"
            },
        }]],
-->

<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require("webpack");

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包JS规则
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: { // 告诉babel你需要兼容哪些浏览器
                            "chrome": "58",
                        },
                    }]]
                }
            },
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options:{
                            hmr: true
                        }
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>
```

### 转换es678语法的补充

```html
<!--
4.利用babel实现低版本语法
对于有对应关系的语法而言, 经过我们上节课的配置就已经能够实现自动转换了
但是对于没有对应关系的语法而言, 经过我们上节课的配置还不能实现自动转换
什么叫有对应关系, 什么叫做没有对应关系?
有对应关系就是指ES5中有对应的概念,  例如: 箭头函数对应普通函数, let对应var, 这个就叫做有对应关系
没有对应关系就是指E5中根本就没有对应的语法,  例如Promise, includes等方法是ES678新增的
ES5中根本就没有对应的实现, 这个时候就需要再增加一些额外配置, 让babel自己帮我们实现对应的语法

4.1安装不存在代码的实现包
npm install --save @babel/polyfill
4.2在用到不存在代码的文件中导入包
import "@babel/polyfill";
注意点:
如果导入了polyfill,那么无论我们有没有用到不存在的语法都会打包到文件中
但是这样会增加打包后文件的大小, 我们希望的是只将用到的不存在语法打包到文件中
那么就需要在webpack.config.js中再配置一下
presets: [["@babel/preset-env",{
            targets: {
                "chrome": "58",
                "ie": "10"
            },
            useBuiltIns: "usage"
        }]]
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require("webpack");

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包JS规则
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: {
                            // "chrome": "58",
                        },
                        useBuiltIns: "usage"
                    }]],
                }
            },
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options:{
                            hmr: true
                        }
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>
```

### polyfill注意点

```html
<!--
1.直接在文件中导入polyfill模块的弊端
直接导入polyfill的方式只适用于一般项目开发, 但是如果是在编写一些第三方模块的时候这种方式会出现一些问题
因为这种方式是通过全局变量的方式来注入代码, 会污染全局环境. 所以我们再来看一下polyfill的第二种配置方式

2.第二种配置方式
2.1安装相关模块
npm install --save @babel/polyfill
npm install --save-dev @babel/plugin-transform-runtime
npm install --save @babel/runtime

2.2配置相关信息
plugins: [
    ["@babel/plugin-transform-runtime",
        {
            "absoluteRuntime": false,
            "corejs": 2,
            "helpers": true,
            "regenerator": true,
            "useESModules": false
        }
    ]
]

注意点:
"corejs": false, 还是全局注入,还是会污染全局环境
"corejs": 2, 则不会污染全局环境
npm install --save @babel/runtime-corejs2
-->

<!--
// 转换前
function sleep(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve()
    }, ms)
  })
}
// "corejs": false转换后
require("core-js/modules/es6.promise");  // 全局引入

function sleep(ms) {
  return new Promise(function (resolve, reject) {
    setTimeout(function () {
      resolve();
    }, ms);
  });
}

// "corejs": 2转换后
var _interopRequireDefault = require("@babel/runtime-corejs2/helpers/interopRequireDefault");
var _promise = _interopRequireDefault(require("@babel/runtime-corejs2/core-js/promise")); // 独立变量, 局部引入
function sleep(ms) {
  return new _promise.default(function (resolve, reject) {
    setTimeout(function () {
      resolve();
    }, ms);
  });
}
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require("webpack");

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包JS规则
            /*
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: {
                            // "chrome": "58",
                        },
                        useBuiltIns: "usage"
                    }]],
                }
            },
             */
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: {
                            // "chrome": "58",
                        },
                        // useBuiltIns: "usage"
                    }]],
                    "plugins": [
                        [
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime": false,
                                "corejs": 2,
                                "helpers": true,
                                "regenerator": true,
                                "useESModules": false
                            }
                        ]
                    ]
                }
            },
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options:{
                            hmr: true
                        }
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>
```

### babel的使用技巧

```html
<!--
1.babel-使用技巧
1.1查看错误提示
1.2根据错误信息查询文档:https://babeljs.io/docs/en/babel-preset-env
1.3根据文档缺什么安装配置什么
-->
```

### html-img-loader

```html
<!--
1.什么是html-withimg-loader?
我们通过file-loader或者url-loader已经可以将JS或者CSS中用到的图片打包到指定目录中了
但是file-loader或者url-loader并不能将HTML中用到的图片打包到指定目录中
所以此时我们就需要再借助一个名称叫做"html-withimg-loader"的加载器来实现HTML中图片的打包

2.html-withimg-loader使用
https://www.npmjs.com/package/html-withimg-loader
2.1安装html-withimg-loader
npm install html-withimg-loader --save
2.2配置html-withimg-loader
{
    test: /\.(htm|html)$/i,
    loader: 'html-withimg-loader'
}
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require("webpack");

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包JS规则
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: {
                            // "chrome": "58",
                        },
                        // useBuiltIns: "usage"
                    }]],
                    "plugins": [
                        ["@babel/plugin-proposal-class-properties", { "loose": true }],
                        [
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime": false,
                                "corejs": 2,
                                "helpers": true,
                                "regenerator": true,
                                "useESModules": false
                            }
                        ]
                    ]
                }
            },
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    }
                ]
            },
            {
                test: /\.(htm|html)$/i,
                loader: 'html-withimg-loader'
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options:{
                            hmr: true
                        }
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>
```

### 压缩图片

```html
<!--
1.图片压缩和合并
在企业开发中为了提升网页的访问速度, 我们除了会压缩HTML/CSS/JS以外
还会对网页上的图片进行压缩和合并, 压缩可以减少网页体积, 合并可以减少请求次数
-->
<!--
1.压缩打包之后的图片
每次在打包图片之前,我们可以通过配置webpack对打包的图片进行压缩, 以较少打包之后的体积

2.如何压缩图片?
利用image-webpack-loader/img-loader压缩图片
https://www.npmjs.com/package/image-webpack-loader
https://www.npmjs.com/package/img-loader
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require("webpack");

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包JS规则
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: {
                            // "chrome": "58",
                        },
                        // useBuiltIns: "usage"
                    }]],
                    "plugins": [
                        ["@babel/plugin-proposal-class-properties", { "loose": true }],
                        [
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime": false,
                                "corejs": 2,
                                "helpers": true,
                                "regenerator": true,
                                "useESModules": false
                            }
                        ]
                    ]
                }
            },
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                        }
                    },
                    {
                        loader: 'image-webpack-loader',
                        options: {
                            mozjpeg: {
                                progressive: true,
                                quality: 65
                            },
                            // optipng.enabled: false will disable optipng
                            optipng: {
                                enabled: false,
                            },
                            pngquant: {
                                quality: [0.65, 0.90],
                                speed: 4
                            },
                            gifsicle: {
                                interlaced: false,
                            },
                            // the webp option will enable WEBP
                            webp: {
                                quality: 75
                            }
                        }
                    },
                ]
            },
            {
                test: /\.(htm|html)$/i,
                loader: 'html-withimg-loader'
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options:{
                            hmr: true
                        }
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>
```

### 图片合并(用的少)

```html
<!--
1.将打包之后的图片合成精灵图
过去为了减少网页请求的次数, 我们需要"UI设计师"给我们提供精灵图,
并且在使用时还需要手动的去设置每张图片的位置
但是有了webpack之后我们只需要让"UI设计师"给我们提供切割好的图片
我们可以自己合成精灵图, 并且还不用手动去设置图片的位置

2.如何合并图片
利用postcss-sprites/webpack-spritesmith合并图片
https://www.npmjs.com/package/postcss-sprites
https://www.npmjs.com/package/webpack-spritesmith
-->
```

### 图片路径问题

```html
<!--
1.webpack打包图片路径问题
webpack打包之后给我们的都是相对路径
但是正是因为是相对路径, 所以会导致在html中使用的图片能够正常运行, 在css中的图片不能正常运行
例如: 打包之后的路径是 images/lnj.jpg
      那么在html中, 会去html文件所在路径下找images,正好能找到所以不报错
      但是在css中,  会去css文件所在路径下找images, 找不到所以报错

|---bundle
       |---css
            |---index.css
       |---js
            |---index.js
       |---images
            |---lnj.jpg
       |---index.html

2.解决方案
在开发阶段将publicPath设置为dev-server服务器地址
在上线阶段将publicPath设置为线上服务器地址
-->
<!-- webpack.config.js -->
<script>
const path = require("path");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require("webpack");

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: "./bundle",
        open: true,
        port: 9090,
        proxy: [{
            context: ["/user", "/login"],
            target: "http://127.0.0.1:3000",
            changeOrigin: true,     // 域名跨域
            secure: false,          // https跨域
            pathRewrite:{"": "/api"} // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})],
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: "cheap-module-eval-source-map",
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: "development", // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: "./src/js/index.js",
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
        /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: "js/bundle.js",
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, "bundle")
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 打包JS规则
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: "babel-loader",
                options: {
                    "presets": [["@babel/preset-env", {
                        targets: {
                            // "chrome": "58",
                        },
                        // useBuiltIns: "usage"
                    }]],
                    "plugins": [
                        ["@babel/plugin-proposal-class-properties", { "loose": true }],
                        [
                            "@babel/plugin-transform-runtime",
                            {
                                "absoluteRuntime": false,
                                "corejs": 2,
                                "helpers": true,
                                "regenerator": true,
                                "useESModules": false
                            }
                        ]
                    ]
                }
            },
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/',
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                            publicPath: "http://127.0.0.1:9090/images"
                        }
                    },
                    {
                        loader: 'image-webpack-loader',
                        options: {
                            mozjpeg: {
                                progressive: true,
                                quality: 65
                            },
                            // optipng.enabled: false will disable optipng
                            optipng: {
                                enabled: false,
                            },
                            pngquant: {
                                quality: [0.65, 0.90],
                                speed: 4
                            },
                            gifsicle: {
                                interlaced: false,
                            },
                            // the webp option will enable WEBP
                            webp: {
                                quality: 75
                            }
                        }
                    },
                ]
            },
            {
                test: /\.(htm|html)$/i,
                loader: 'html-withimg-loader'
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use:[
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options:{
                            hmr: true
                        }
                    },
                    {
                        loader: "css-loader",
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: "postcss-loader"
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "less-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: "style-loader"
                }, {
                    loader: "css-loader"
                }, {
                    loader: "sass-loader"
                },{
                    loader: "postcss-loader"
                }]
            },
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
        // 指定打包的模板, 如果不指定会自动生成一个空的
        template: "./src/index.html",
        minify: {
            // 告诉htmlplugin打包之后的html文件需要压缩
            // collapseWhitespace: true,
        }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: "./doc",
            to: "doc"
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css',
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>
```

### eslint

```html
<!--
1.什么是eslint?
ESLint 是一个插件化的 javascript 代码检测工具，
它可以用于检查常见的 JavaScript 代码错误，也可以进行"代码规范"检查，
在企业开发中项目负责人会定制一套 ESLint 规则，然后应用到所编写的项目上，
从而实现辅助编码规范的执行，有效控制项目代码的质量。
在编译打包时如果语法有错或者有不符合规范的语法就会报错, 并且会提示相关错误信息

2.如何使用eslint
2.1对应环境和loader
npm install eslint --save-dev
npm install eslint-loader --save-dev
2.2生成eslint配置文件
https://www.webpackjs.com/loaders/
http://eslint.cn/
{
    // 由于loader是从右至左从下至上执行
    // 而如果先执行了babel-loader就会对我们的代码进行转换
    // 而我们想检查代码规范的代码并不是转换之后的代码
    // 所以通过enforce: 'pre'告诉webpack在所有loader执行之前执行
    enforce: 'pre',
    test: /\.js$/,
    exclude: /node_modules/, // 排除不检查的文件夹
    include: path.resolve(__dirname, 'src'), // 指定检查的文件夹
    loader: 'eslint-loader',
    options: {
        // eslint options (if necessary)
        // fix: true
    }
},

3.如何提升开发效率
通过阅读打包错误信息来修复不符合规范语法非常低效
所以我们可以通过webstrom插件来帮我们完成提示
webstrom--setting--eslint--autoxxx
-->

<!-- webpack.config.js -->
<script>
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const CopyWebpackPlugin = require('copy-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCSSAssetsPlugin = require('optimize-css-assets-webpack-plugin');
const TerserJSPlugin = require('terser-webpack-plugin');
const Webpack = require('webpack');

module.exports = {
    /*
    devServer: 自动检测文件变化配置
    * */
    devServer: {
        contentBase: './bundle',
        open: true,
        port: 9090,
        proxy: [{
            context: ['/user', '/login'],
            target: 'http://127.0.0.1:3000',
            changeOrigin: true, // 域名跨域
            secure: false, // https跨域
            pathRewrite: { '': '/api' } // 路径重写, 将路径中的api替换为空
        }],
        hot: true, // 开启热更新, 只要开启了热更新就不会自动刷新网页了
        hotOnly: true // 哪怕不支持热更新也不要刷新网页
    },
    /*
    optimization: 配置webpack的优化项
    * */
    optimization: {
        minimizer: [new TerserJSPlugin({}), new OptimizeCSSAssetsPlugin({})]
    },
    /*
    配置sourcemap
    development: cheap-module-eval-source-map
    production: cheap-module-source-map
    * */
    devtool: 'cheap-module-eval-source-map',
    /*
    mode: 指定打包的模式, 模式有两种
    一种是开发模式(development): 不会对打包的JS代码进行压缩
    还有一种就是上线(生产)模式(production): 会对打包的JS代码进行压缩
    * */
    mode: 'development', // "production" | "development"
    /*
    entry: 指定需要打包的文件
    * */
    entry: './src/js/index.js',
    /*
    output: 指定打包之后的文件输出的路径和输出的文件名称
    * */
    output: {
    /*
        filename: 指定打包之后的JS文件的名称
        * */
        filename: 'js/bundle.js',
        /*
        path: 指定打包之后的文件存储到什么地方
        * */
        path: path.resolve(__dirname, 'bundle')
    },
    /*
    module: 告诉webpack如何处理webpack不能够识别的文件
    * */
    module: {
        rules: [
            // 检查编码规范的规则
            {
                // enforce: "pre"作用: 让当前的loader再其它loader之前执行
                enforce: "pre",
                test: /\.js$/,
                exclude: /node_modules/,
                include: path.resolve(__dirname, "src"),
                loader: 'eslint-loader',
                options: {
                    // eslint options (if necessary)
                    fix: true
                },
            },
            // 打包JS规则
            {
                test: /\.js$/,
                exclude: /node_modules/, // 告诉webpack不处理哪一个文件夹
                loader: 'babel-loader',
                options: {
                    presets: [['@babel/preset-env', {
                        targets: {
                            // "chrome": "58",
                        }
                        // useBuiltIns: "usage"
                    }]],
                    plugins: [
                        ['@babel/plugin-proposal-class-properties', { loose: true }],
                        [
                            '@babel/plugin-transform-runtime',
                            {
                                absoluteRuntime: false,
                                corejs: 2,
                                helpers: true,
                                regenerator: true,
                                useESModules: false
                            }
                        ]
                    ]
                }
            },
            // 打包字体图标规则
            {
                test: /\.(eot|json|svg|ttf|woff|woff2)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'font/'
                        }
                    }
                ]
            },
            // 打包图片规则
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'url-loader',
                        options: {
                            // 指定图片限制的大小
                            limit: 1024,
                            // 指定打包后文件名称
                            name: '[name].[ext]',
                            // 指定打包后文件存放目录
                            outputPath: 'images/',
                            publicPath: 'http://127.0.0.1:9090/images'
                        }
                    },
                    {
                        loader: 'image-webpack-loader',
                        options: {
                            mozjpeg: {
                                progressive: true,
                                quality: 65
                            },
                            // optipng.enabled: false will disable optipng
                            optipng: {
                                enabled: false
                            },
                            pngquant: {
                                quality: [0.65, 0.90],
                                speed: 4
                            },
                            gifsicle: {
                                interlaced: false
                            },
                            // the webp option will enable WEBP
                            webp: {
                                quality: 75
                            }
                        }
                    }
                ]
            },
            {
                test: /\.(htm|html)$/i,
                loader: 'html-withimg-loader'
            },
            // 打包CSS规则
            {
                test: /\.css$/,
                use: [
                    {
                        // loader: "style-loader"
                        loader: MiniCssExtractPlugin.loader,
                        options: {
                            hmr: true
                        }
                    },
                    {
                        loader: 'css-loader',
                        options: {
                            // modules: true // 开启CSS模块化
                        }
                    },
                    {
                        loader: 'postcss-loader'
                    }
                ]
            },
            // 打包LESS规则
            {
                test: /\.less$/,
                use: [{
                    loader: 'style-loader'
                }, {
                    loader: 'css-loader'
                }, {
                    loader: 'less-loader'
                }, {
                    loader: 'postcss-loader'
                }]
            },
            // 打包SCSS规则
            {
                test: /\.scss$/,
                use: [{
                    loader: 'style-loader'
                }, {
                    loader: 'css-loader'
                }, {
                    loader: 'sass-loader'
                }, {
                    loader: 'postcss-loader'
                }]
            }
        ]
    },
    /*
    plugins: 告诉webpack需要新增一些什么样的功能
    * */
    plugins: [
        new HtmlWebpackPlugin({
            // 指定打包的模板, 如果不指定会自动生成一个空的
            template: './src/index.html',
            minify: {
                // 告诉htmlplugin打包之后的html文件需要压缩
                // collapseWhitespace: true,
            }
        }),
        new CleanWebpackPlugin(),
        new CopyWebpackPlugin([{
            from: './doc',
            to: 'doc'
        }]),
        new MiniCssExtractPlugin({
            filename: 'css/[name].css'
        }),
        new Webpack.HotModuleReplacementPlugin()
    ]
};
</script>

<!-- eslintrc.js -->
<script>
// .eslintrc.js
// https://eslint.org/docs/user-guide/configuring
module.exports = {
  /*
  不重要,永远写true
  * */
  root: true,
  parserOptions: {
    // parser: 'babel-eslint',
    /*
    默认设置为 3，5（默认）， 你可以使用 6、7、8、9 或 10 来指定你想要使用的 ECMAScript 版本
    * */
    "ecmaVersion": 10,
    /*
    设置为 "script" (默认) 或 "module"（如果你的代码是 ECMAScript 模块)。
    * */
    "sourceType": "module",
    /*
    ecmaFeatures - 这是个对象，表示你想使用的额外的语言特性:
    globalReturn - 允许在全局作用域下使用 return 语句
    impliedStrict - 启用全局 strict mode (如果 ecmaVersion 是 5 或更高)
    jsx - 启用 JSX
    * */
    "ecmaFeatures": {}
  },
  // 指定代码运行的宿主环境
  env: {
    browser: true, // 浏览器
    node: true, // node
    /*
    支持 ES6 语法并不意味着同时支持新的 ES6 全局变量或类型（比如 Set 等新类型）。
    对于 ES6 语法，使用 { "parserOptions": { "ecmaVersion": 6 } }
    * */
    es6: true,
  },
  extends: [
      /*
      引入standard代码规范
      * */
    // https://github.com/standard/standard/blob/master/docs/RULES-en.md
    'standard'
  ],
  /*
  扩展或覆盖规则
  * */
  rules: {
    // 强制语句结束添加，分号
    semi: ["error", "always"],
    // 强制缩进2个空格
    indent: ["error", 4],
    // 方法名和刮号之间不加空格
    'space-before-function-paren': ['error', 'never'],
    "no-unexpected-multiline": "off"
  }
};
</script>
```

### 配置文件的优化

**手动将webpack.config.js中的公共部分抽离，再将开发阶段和上线阶段的不同的配置存放到两个文件中即可**

```html
<!--
1.区分开发环境和线上环境
1.1在开发阶段我们为了提升运行效率以及调试效率, 一般会通过dev-server来打包
   在开发阶段我们为了提升打包效率,不会对打包的内容进行压缩
   ... ...
1.2在上线阶段我们需要拿到真实的打包文件, 所以不会通过dev-server来打包
   在上线阶段我们为了提升访问的效率, 所以在打包时需要对打包的内容进行压缩
   ... ...
1.3但是当前我们将"开发环境和线上环境"的配置都写到了一个文件中, 这样非常不利于我们去维护配置文件
   所以我们需要针对不同的环境将不同的配置写到不同的文件中

2.区分开发环境和线上环境优化
区分完不同环境配置文件之后发现两个文件之间存在大量重复配置
这我们可以利用webpack-merge模块来实现冗余代码的抽离和合并进一步优化配置文件
2.1将冗余代码抽取到 webpack.config.common.js中
2.2在dev.js和prod.js中导入common.js, 利用merge合并即可
-->
```

![image-20210724182612079](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210724182612079.png)

