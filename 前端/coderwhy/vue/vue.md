# Vue

## 邂逅

![image-20220224110545078](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220224110545078.png)

diff算法：如果列表中有key，就会调用patchKeyChildren方法，否则调用patchUnKeyedChildren方法

没有key：

1.获取旧节点的长度

2.获取新节点的长度

3.获取最小的那个长度

4.从位置0开始依次进行patch比较

5.如果旧的节点大于新的节点，则删除

6.如果新的节点大于旧的节点则添加

有key：

1.从头部开始遍历，遇到相同的节点就继续，遇到不同的就跳出循环

2.从尾部开始遍历，遇到相同的节点就继续，遇到不同的就跳出循环

3.如果最后新节点更多，那么就添加新节点

4.如果旧节点更多，那么就移出旧节点

5.如果中间存在不知道如何排列的位置序列，那么就使用key建立索引图最大限度的使用旧节点



## OptionAPI

### 计算属性

这是因为计算属性会基于它们的依赖关系进行缓存；

在数据不发生变化时，计算属性是不需要重新计算的；

但是如果依赖的数据发生变化，在使用时，计算属性依然会重新进行计算；



计算属性在大多数情况下，只需要一个getter方法即可，所以我们会将计算属性直接写成一个函数。

但是，如果我们确实想设置计算属性的值呢？

这个时候我们也可以给计算属性设置一个setter的方法	



### watch

开发中我们在data返回的对象中定义了数据，这个数据通过插值语法等方式绑定到template中；

当数据变化时，template会自动进行更新来显示最新的数据；

但是在某些情况下，我们希望在代码逻辑中监听某个数据的变化，这个时候就需要用侦听器watch来完成了

这是因为默认情况下，watch只是在侦听info的引用变化，对于内部属性的变化是不会做出响应的：

这个时候我们可以使用一个选项deep进行更深层的侦听；

注意前面我们说过watch里面侦听的属性对应的也可以是一个Object；

还有另外一个属性，是希望一开始的就会立即执行一次：

这个时候我们使用immediate选项；

这个时候无论后面数据是否有变化，侦听的函数都会有限执行一次

**侦听器watch的其他方式**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  
  <div id="app"></div>

  <template id="my-app">
    <h2>{{info.name}}</h2>
    <button @click="changeInfo">改变info</button>
    <button @click="changeInfoName">改变info.name</button>
    <button @click="changeInfoNbaName">改变info.nba.name</button>
    <button @click="changeFriendName">改变friends[0].name</button>
  </template>

  <script src="../js/vue.js"></script>
  <script>
    const App = {
      template: '#my-app',
      data() {
        return {
          info: { name: "why", age: 18, nba: {name: 'kobe'} },
          friends: [
            {name: "why"}, 
            {name: "kobe"}
          ]
        }
      },
      watch: {
        info(newValue, oldValue) {
          console.log(newValue, oldValue);
        },
        "info.name": function(newName, oldName) {
          console.log(newName, oldName);
        },
        "friends[0].name": function(newName, oldName) {
          console.log(newName, oldName);
        },
        // friends: {
        //   handler(newFriends, oldFriend) {
        //   },
        //   deep: true
        // }
      },
      methods: {
        changeInfo() {
          this.info = {name: "kobe"};
        },
        changeInfoName() {
          this.info.name = "kobe";
        },
        changeInfoNbaName() {
          this.info.nba.name = "james";
        },
        changeFriendName() {
          this.friends[0].name = "curry";
        }
      },
      created() {
        const unwatch = this.$watch("info", function(newInfo, oldInfo) {
          console.log(newInfo, oldInfo);
        }, {
          deep: true,
          immediate: true
        })
        // unwatch()
      }
    }

    Vue.createApp(App).mount('#app');
  </script>
</body>
</html>
```

### v-model

v-model的原理:

v-bind绑定value属性的值；

v-on绑定input事件监听到函数中，函数会获取最新的值赋值到绑定的属性中;

#### v-model修饰符-lazy

默认情况下，v-model在进行双向绑定时，绑定的是input事件，那么会在每次内容输入后就将最新的值和绑定的属性进行同步；

如果我们在v-model后跟上lazy修饰符，那么会将绑定的事件切换为change 事件，只有在提交时（比如回车）才会触发

#### v-model修饰符-number

我们先来看一下v-model绑定后的值是什么类型的：

message总是string类型，即使在我们设置type为number也是string类型；

如果我们希望转换为数字类型，那么可以使用.number 修饰符：

另外，在我们进行逻辑判断时，如果是一个string类型，在可以转化的情况下会进行隐式转换的：

下面的score在进行判断的过程中会进行隐式转化的；

#### v-model修饰符-trim

如果要自动过滤用户输入的守卫空白字符，可以给v-model添加trim 修饰符：

## 组件化开发

如果我们将一个页面中所有的处理逻辑全部放在一起，处理起来就会变得非常复杂，而且不利于后续的管理以及扩展；p但如果，我们讲一个页面拆分成一个个小的功能块，每个功能块完成属于自己这部分独立的功能，那么之后整个页面的管理和维护就变得非常容易了；p如果我们将一个个功能块拆分后，就可以像搭建积木一下来搭建我们的项目

我们将一个完整的页面分成很多个组件；

每个组件都用于实现页面的一个功能块；

而每一个组件又可以进行细分；

而组件本身又可以在多个地方进行复用；

## 如何支持SFC

方式一：使用Vue CLI来创建项目，项目会默认帮助我们配置好所有的配置选项，可以在其中直接使用.vue文件；

方式二：自己使用webpack或rollup或vite这类打包工具，对其进行打包处理

## webpack

webpack是一个静态的模块化打包工具，为现代的JavaScript应用程序；

JavaScript的打包：将ES6转换成ES5的语法；TypeScript的处理，将其转换成JavaScript；

Css的处理：CSS文件模块的加载、提取；Less、Sass等预处理器的处理；

资源文件img、font：图片img文件的加载；字体font文件的加载；HTML资源的处理：打包HTML资源文件；

处理vue项目的SFC文件.vue文件

### css-loader的使用

```js
const path = require('path');

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "bundle.js"
  },
  module: {
    rules: [
      {
        test: /\.css$/, //正则表达式
        // 1.loader的写法(语法糖)
        // loader: "css-loader"

        // 2.完整的写法
        use: [
          // {loader: "css-loader"}
          "style-loader",
          "css-loader",
          "postcss-loader"
          // {
          //   loader: "postcss-loader",
          //   options: {
          //     postcssOptions: {
          //       plugins: [
          //         require("autoprefixer")
          //       ]
          //     }
          //   }
          // }
        ]
      },
      {
        test: /\.less$/,
        use: [
          "style-loader",
          "css-loader",
          "less-loader"
        ]
      },
      // {
      //   test: /\.(less|css)$/,
      //   use: [
      //     "style-loader",
      //     "css-loader",
      //     "less-loader"
      //   ]
      // }
    ]
  }
}
```

### file-loader

有时候我们处理后的文件名称按照一定的规则进行显示：

比如保留原来的文件名、扩展名，同时为了防止重复，包含一个hash值等；

这个时候我们可以使用PlaceHolders来完成，webpack给我们提供了大量的PlaceHolders来显示不同的内容：https://webpack.js.org/loaders/file-loader/#placeholdersp我们可以在文档中查阅自己需要的placeholder；

我们这里介绍几个最常用的placeholder：

[ext]：处理文件的扩展名；p[name]：处理文件的名称；

[hash]：文件的内容，使用MD4的散列函数处理，生成的一个128位的hash值（32个十六进制）；

[contentHash]：在file-loader中和[hash]结果是一致的（在webpack的一些其他地方不一样，后面会讲到）；

[hash:<length>]：截图hash的长度，默认32个字符太长了；

[path]：文件相对于webpack配置文件的路径

```js
const path = require('path');

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "bundle.js",
    // assetModuleFilename: "img/[name]_[hash:6][ext]"
  },
  module: {
    rules: [
      {
        test: /\.css$/, 
        use: [
          "style-loader",
          "css-loader",
          "postcss-loader"
        ]
      },
      {
        test: /\.less$/,
        use: [
          "style-loader",
          "css-loader",
          "less-loader"
        ]
      },
      // {
      //   test: /\.(jpe?g|png|gif|svg)$/,
      //   use: {
      //     loader: "url-loader",
      //     options: {
      //       // outputPath: "img",
      //       name: "img/[name]_[hash:6].[ext]",
      //       limit: 100 * 1024
      //     }
      //   }
      // },
      {
        test: /\.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]"
        },
        parser: {
          dataUrlCondition: {
            maxSize: 100 * 1024
          }
        }
      },
      // {
      //   test: /\.(eot|ttf|woff2?)$/,
      //   use: {
      //     loader: "file-loader",
      //     options: {
      //       // outputPath: "font",
      //       name: "font/[name]_[hash:6].[ext]"
      //     }
      //   }
      // },
      {
        test: /\.(eot|ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name]_[hash:6][ext]"
        }
      }
    ]
  }
}
```

### 认识Plugin

Loader是用于特定的模块类型进行转换；

Plugin可以用于执行更加广泛的任务，比如打包优化、资源管理、环境变量注入等

#### CleanWebpackPlugin

前面我们演示的过程中，每次修改了一些配置，重新打包时，都需要手动删除dist文件夹：

我们可以借助于一个插件来帮助我们完成，这个插件就是CleanWebpackPlugin；

#### HtmlWebpackPlugin

我们的HTML文件是编写在根目录下的，而最终打包的dist文件夹中是没有index.html文件的。

在进行项目部署的时，必然也是需要有对应的入口文件index.html；

所以我们也需要对index.html进行打包处理；

对HTML进行打包处理我们可以使用另外一个插件：HtmlWebpackPlugin；

#### DefinePlugin的介绍

但是，这个时候编译还是会报错，因为在我们的模块中还使用到一个BASE_URL的常量：

这是因为在编译template模块时，有一个BASE_URL：p<link rel="icon"href="<%=BASE_URL %>favicon.ico">；

但是我们并没有设置过这个常量值，所以会出现没有定义的错误；

这个时候我们可以使用DefinePlugin插件；

#### CopyWebpackPlugin

在vue的打包过程中，如果我们将一些文件放到public的目录下，那么这个目录会被复制到dist文件夹中。

这个复制的功能，我们可以使用CopyWebpackPlugin来完成；

安装CopyWebpackPlugin插件：n接下来配置CopyWebpackPlugin即可：

复制的规则在patterns中设置；

from：设置从哪一个源中开始复制；

to：复制到的位置，可以省略，会默认复制到打包的目录下；

globOptions：设置一些额外的选项，其中可以编写需要忽略的文件：index.html：也不需要复制，因为我们已经通过HtmlWebpackPlugin完成了index.html的生成；

#### Mode配置

前面我们一直没有讲mode。

Mode配置选项，可以告知webpack使用响应模式的内置优化：

默认值是production（什么都不设置的情况下）；

可选值有：'none'|'development'|'production'；

```js
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require('copy-webpack-plugin');

module.exports = {
  // 设置模式
  // development 开发阶段, 会设置development
  // production 准备打包上线的时候, 设置production
  mode: "development",
  // 设置source-map, 建立js映射文件, 方便调试代码和错误
  devtool: "source-map",
  
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "js/bundle.js",
    // assetModuleFilename: "img/[name]_[hash:6][ext]"
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      // {
      //   test: /\.(jpe?g|png|gif|svg)$/,
      //   use: {
      //     loader: "url-loader",
      //     options: {
      //       // outputPath: "img",
      //       name: "img/[name]_[hash:6].[ext]",
      //       limit: 100 * 1024
      //     }
      //   }
      // },
      {
        test: /\.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]",
        },
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024,
          },
        },
      },
      // {
      //   test: /\.(eot|ttf|woff2?)$/,
      //   use: {
      //     loader: "file-loader",
      //     options: {
      //       // outputPath: "font",
      //       name: "font/[name]_[hash:6].[ext]"
      //     }
      //   }
      // },
      {
        test: /\.(eot|ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name]_[hash:6][ext]",
        },
      },
    ],
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: "./public/index.html",
      title: "哈哈哈哈"
    }),
    new DefinePlugin({
      BASE_URL: "'./'"
    }),
    new CopyWebpackPlugin({
      patterns: [
        {
          from: "public",
          to: "./",
          globOptions: {
            ignore: [
              "**/index.html"
            ]
          }
        }
      ]
    })
  ],
};
```

### 为什么需要babel

事实上，在开发中我们很少直接去接触babel，但是babel对于前端开发来说，目前是不可缺少的一部分：

开发中，我们想要使用ES6+的语法，想要使用TypeScript，开发React项目，它们都是离不开Babel的；

所以，学习Babel对于我们理解代码从编写到线上的转变过程至关重要；

那么，Babel到底是什么呢？

Babel是一个工具链，主要用于旧浏览器或者环境中将ECMAScript 2015+代码转换为向后兼容版本的JavaScript；

包括：语法转换、源代码转换等；

### Babel的底层原理

babel是如何做到将我们的一段代码（ES6、TypeScript、React）转成另外一段代码（ES5）的呢？

从一种源代码（原生语言）转换成另一种源代码（目标语言），这是什么的工作呢？

就是编译器，事实上我们可以将babel看成就是一个编译器。

Babel编译器的作用就是将我们的源代码，转换成浏览器可以直接识别的另外一段源代码；

Babel也拥有编译器的工作流程：

解析阶段（Parsing）

转换阶段（Transformation）

生成阶段（Code Generation）

### Babel编译器执行原理

![image-20220301002011851](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301002011851.png)

### babel-loader

在实际开发中，我们通常会在构建工具中通过配置babel来对其进行使用的，比如在webpack中。

那么我们就需要去安装相关的依赖：

如果之前已经安装了@babel/core，那么这里不需要再次安装；

我们可以设置一个规则，在加载js文件时，使用我们的babel：

### 认识模块热替换（HMR）

什么是HMR呢？

HMR的全称是Hot Module Replacement，翻译为模块热替换；

模块热替换是指在应用程序运行过程中，替换、添加、删除模块，而无需重新刷新整个页面；

HMR通过如下几种方式，来提高开发的速度：

不重新加载整个页面，这样可以保留某些应用程序的状态不丢失；

只更新需要变化的内容，节省开发的时间；p修改了css、js源代码，会立即在浏览器更新，相当于直接在浏览器的devtools中直接修改样式；

如何使用HMR呢？

默认情况下，webpack-dev-server已经支持HMR，我们只需要开启即可；

在不开启HMR的情况下，当我们修改了源代码之后，整个页面会自动刷新，使用的是live reloading

### HMR的原理

那么HMR的原理是什么呢？如何可以做到只更新一个模块中的内容呢？

webpack-dev-server会创建两个服务：提供静态资源的服务（express）和Socket服务（net.Socket）；

express server负责直接提供静态资源的服务（打包后的资源直接被浏览器请求和解析）；

HMR Socket Server，是一个socket的长连接：

长连接有一个最好的好处是建立连接后双方可以通信（服务器可以直接发送文件到客户端）；

当服务器监听到对应的模块发生变化时，会生成两个文件.json（manifest文件）和.js文件（update chunk）；

通过长连接，可以直接将这两个文件主动发送给客户端（浏览器）；

浏览器拿到两个新的文件后，通过HMR runtime机制，加载这两个文件，并且针对修改的模块进行更新；HMR的原理

### Proxy

proxy是我们开发中非常常用的一个配置选项，它的目的设置代理来解决跨域访问的问题：

比如我们的一个api请求是http://localhost:8888，但是本地启动服务器的域名是http://localhost:8000，这个时候发送网络请求就会出现跨域的问题；

那么我们可以将请求先发送到一个代理服务器，代理服务器和API服务器没有跨域的问题，就可以解决我们的跨域问题了；

我们可以进行如下的设置：

target：表示的是代理到的目标地址，比如/api-hy/moment会被代理到http://localhost:8888/api-hy/moment；

pathRewrite：默认情况下，我们的/api-hy也会被写入到URL中，如果希望删除，可以使用pathRewrite；

secure：默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false；

changeOrigin：它表示是否更新代理后请求的headers中host地址

### resolve模块解析

resolve用于设置模块如何被解析：

在开发中我们会有各种各样的模块依赖，这些模块可能来自于自己编写的代码，也可能来自第三方库；

resolve可以帮助webpack从每个require/import 语句中，找到需要引入到合适的模块代码；

webpack 使用enhanced-resolve来解析文件路径；nwebpack能解析三种文件路径：

绝对路径p由于已经获得文件的绝对路径，因此不需要再做进一步解析。

相对路径p在这种情况下，使用import 或require 的资源文件所处的目录，被认为是上下文目录；

在import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径；

模块路径p在resolve.modules中指定的所有目录检索模块；默认值是['node_modules']，所以默认会从node_modules中查找文件；

我们可以通过设置别名的方式来替换初识模块路径，具体后面讲解alias的配置

```js
const path = require("path");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { DefinePlugin } = require("webpack");
const CopyWebpackPlugin = require('copy-webpack-plugin');
const { VueLoaderPlugin } = require('vue-loader/dist/index');

module.exports = {
  target: "web",
  mode: "development",
  devtool: "source-map",
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "./build"),
    filename: "js/bundle.js",
  },
  devServer: {
    contentBase: "./public",
    hot: true,
    host: "0.0.0.0",
    port: 7777,
    open: true,
    // compress: true,
    proxy: {
      "/api": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/api": ""
        },
        secure: false,
        changeOrigin: true
      }
    }
  },
  resolve: {
    extensions: [".js", ".json", ".mjs", ".vue", ".ts", ".jsx", ".tsx"],
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "js": path.resolve(__dirname, "./src/js")
    }
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader", "postcss-loader"],
      },
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
      // },
      {
        test: /\.(jpe?g|png|gif|svg)$/,
        type: "asset",
        generator: {
          filename: "img/[name]_[hash:6][ext]",
        },
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024,
          },
        },
      },
      {
        test: /\.(eot|ttf|woff2?)$/,
        type: "asset/resource",
        generator: {
          filename: "font/[name]_[hash:6][ext]",
        },
      },
      {
        test: /\.js$/,
        loader: "babel-loader"
      },
      {
        test: /\.vue$/,
        loader: "vue-loader"
      }
    ],
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: "./public/index.html",
      title: "哈哈哈哈"
    }),
    new DefinePlugin({
      BASE_URL: "'./'",
      __VUE_OPTIONS_API__: true,
      __VUE_PROD_DEVTOOLS__: false
    }),
    // new CopyWebpackPlugin({
    //   patterns: [
    //     {
    //       from: "public",
    //       to: "./",
    //       globOptions: {
    //         ignore: [
    //           "**/index.html"
    //         ]
    //       }
    //     }
    //   ]
    // }),
    new VueLoaderPlugin()
  ],
};
```

### 区分开发和生成环境配置

这里我们创建三个文件

webpack.comm.conf.js

webpack.dev.conf.js

webpack.prod.conf.js

## 认识Vite

Webpack是目前整个前端使用最多的构建工具，但是除了webpack之后也有其他的一些构建工具：

比如rollup、parcel、gulp、vite等等n什么是vite呢？官方的定位：下一代前端开发与构建工具；

如何定义下一代开发和构建工具呢？

我们知道在实际开发中，我们编写的代码往往是不能被浏览器直接识别的，比如ES6、TypeScript、Vue文件等等；

所以我们必须通过构建工具来对代码进行转换、编译，类似的工具有webpack、rollup、parcel；

但是随着项目越来越大，需要处理的JavaScript呈指数级增长，模块越来越多；

构建工具需要很长的时间才能开启服务器，HMR也需要几秒钟才能在浏览器反应出来；

所以也有这样的说法：天下苦webpack久矣；

Vite(法语意为"快速的"，发音/vit/)是一种新型前端构建工具，能够显著提升前端开发体验

### vite的构造

它主要由两部分组成：

一个开发服务器，它基于原生ES模块提供了丰富的内建功能，HMR的速度非常快速；

一套构建指令，它使用rollup打开我们的代码，并且它是预配置的，可以输出生成环境的优化过的静态资源

在vite2中，已经不再使用Koa了，而是使用Connect来搭建的服务器

## vue3组件化开发

### 组件通信

#### 父子组件的通信

父组件传递给子组件：通过props属性；

子组件传递给父组件：通过$emit触发事件

**什么是Props呢？**

Props是你可以在组件上注册一些自定义的attribute；

父组件给这些attribute赋值，子组件通过attribute的名称获取到对应的值

**非Prop的Attribute**

当我们传递给一个组件某个属性，但是该属性并没有定义对应的props或者emits时，就称之为非Prop的Attribute；

常见的包括class、style、id属性等

**Attribute继承**

当组件有单个根节点时，非Prop的Attribute将自动添加到根节点的Attribute中

**禁用Attribute继承和多根节点**

如果我们不希望组件的根元素继承attribute，可以在组件中设置inheritAttrs: false：

禁用attribute继承的常见情况是需要将attribute应用于根元素之外的其他元素；

我们可以通过$attrs来访问所有的非props的attribute

**多个根节点的attribute**

多个根节点的attribute如果没有显示的绑定，那么会报警告，我们必须手动的指定要绑定到哪一个属性上

```vue
<template>
  <div>
    <show-message id="abc" class="why" title="哈哈哈" content="我是哈哈哈哈" message-info=""></show-message>
    <show-message title="呵呵呵" content="我是呵呵呵呵"></show-message>
    <show-message :title="title" :content="content"></show-message>

    <show-message :title="message.title" :content="message.content"></show-message>
    <show-message v-bind="message"></show-message>

    <multi-root-element id="aaaa"></multi-root-element>
  </div>
</template>

<script>
  import ShowMessage from './ShowMessage.vue';
  import MultiRootElement from './MultiRootElement.vue';

  export default {
    components: {
      ShowMessage,
      MultiRootElement
    },
    data() {
      return {
        title: "嘻嘻嘻",
        content: "我是嘻嘻嘻嘻",
        message: {
          title: "嘿嘿嘿",
          content: "我是嘿嘿嘿"
        }
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
    <h2 v-bind="$attrs">{{title}}</h2>
    <p>{{content}}</p>
  </div>
</template>

<script>
  export default {
    // props: ['title', 'content']
    inheritAttrs: false,
    props: {
      title: String,
      content: {
        type: String,
        required: true,
        default: "123"
      },
      counter: {
        type: Number
      },
      info: {
        type: Object,
        default() {
          return {name: "why"}
        }
      },
      messageInfo: {
        type: String
      }
    }
  }
</script>

<style scoped>

</style>
```

#### 子组件传递给父组件

首先，我们需要在子组件中定义好在某些情况下触发的事件名称；

其次，在父组件中以v-on的方式传入要监听的事件名称，并且绑定到对应的方法中；

最后，在子组件中发生某个事件的时候，根据事件名称触发对应的事件；

**流程：先在子组件定义emits数组或者对象，然后定义click事件，通过this.$emit发送事件，然后在父组件自定义emit发送的事件并且接受参数**

#### 非父子组件的通信

这里我们主要讲两种方式：

Provide/Inject；

Mitt全局事件总线

##### Provide和Inject

对于这种情况下，我们可以使用Provide 和Inject ：

无论层级结构有多深，父组件都可以作为其所有子组件的依赖提供者；

父组件有一个provide 选项来提供数据；p子组件有一个inject 选项来开始使用这些数据；

实际上，你可以将依赖注入看作是“long range props”，除了：

父组件不需要知道哪些子组件使用它provide 的propertyp子组件不需要知道inject 的property 来自哪里

**处理响应式数据**

我们先来验证一个结果：如果我们修改了this.names的内容，那么使用length的子组件会不会是响应式的？

我们会发现对应的子组件中是没有反应的：

这是因为当我们修改了names之后，之前在provide中引入的this.names.length本身并不是响应式的；

那么怎么样可以让我们的数据变成响应式的呢？

非常的简单，我们可以使用响应式的一些API来完成这些功能，比如说computed函数；

当然，这个computed是vue3的新特性，在后面我会专门讲解，这里大家可以先直接使用一下；

注意：我们在使用length的时候需要获取其中的valuep这是因为computed返回的是一个ref对象，需要取出其中的value来使用；

```vue
<template></template>
  <div>
    <home></home>
    <button @click="addName">+name</button>
  </div>
</template>

<script>
  import Home from './Home.vue';
  import { computed } from 'vue';

  export default {
    components: {
      Home
    },
    provide() {
      return {
        name: "why",
        age: 18,
        length: computed(() => this.names.length) // ref对象 .value
      }
    },
    data() {
      return {
        names: ["abc", "cba", "nba"]
      }
    },
    methods: {
      addName() {
        this.names.push("why");
        console.log(this.names);
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
    HomeContent: {{name}} - {{age}} - {{length.value}}
  </div>
</template>

<script>
  export default {
    inject: ["name", "age", "length"],
  }
</script>

<style scoped>

</style>
```

#### 全局事件总线mitt库

```js
import mitt from 'mitt';

const emitter = mitt();
// export const emitter1 = mitt();
// export const emitter2 = mitt();
// export const emitter3 = mitt();

export default emitter;

```

```vue
<template>
  <div>
    <button @click="btnClick">按钮点击</button>
  </div>
</template>

<script>
  import emitter from './utils/eventbus';

  export default {
    methods: {
      btnClick() {
        console.log("about按钮的点击");
        emitter.emit("why", {name: "why", age: 18});
        emitter.emit("kobe", {name: "kobe", age: 30});
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
  </div>
</template>

<script>
  import emitter from './utils/eventbus';

  export default {
    created() {
      // emitter.on("why", (info) => {
      //   console.log("why:", info);
      // });
      //
      // emitter.on("kobe", (info) => {
      //   console.log("kobe:", info);
      // });

      emitter.on("*", (type, info) => {
        console.log("* listener:", type, info);
      })
    }
  }
</script>

<style scoped>

</style>
```

###  认识插槽Slot

Vue中将<slot> 元素作为承载分发内容的出口；

在封装组件中，使用特殊的元素<slot>就可以为封装组件开启一个插槽；

该插槽插入什么内容取决于父组件如何使用；

```vue
<template>
  <div>
    <my-slot-cpn>
      <button>我是按钮</button>
    </my-slot-cpn>

    <my-slot-cpn>
      我是普通的文本
    </my-slot-cpn>

    <my-slot-cpn>
      <my-button/>
    </my-slot-cpn>

    <my-slot-cpn></my-slot-cpn>

    <!-- 插入了很多的内容 -->
    <my-slot-cpn>
      <h2>哈哈哈</h2>
      <button>我是按钮</button>
      <strong>我是strong</strong>
    </my-slot-cpn>
  </div>
</template>

<script>
  import MySlotCpn from './MySlotCpn.vue';
  import MyButton from './MyButton.vue';

  export default {
    components: {
      MySlotCpn,
      MyButton
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
    <h2>组件开始</h2>
    <slot>
      <i>我是默认的i元素</i>
    </slot>
    <slot>
      <i>我是默认的i元素</i>
    </slot>
    <slot>
      <i>我是默认的i元素</i>
    </slot>
    <h2>组件结束</h2>
  </div>
</template>

<script>
  export default {
    
  }
</script>

<style scoped>

</style>
```

### 具名插槽的使用

事实上，我们希望达到的效果是插槽对应的显示，这个时候我们就可以使用具名插槽：

具名插槽顾名思义就是给插槽起一个名字，<slot> 元素有一个特殊的attribute：name；

一个不带name 的slot，会带有隐含的名字default

**动态插槽名**

目前我们使用的插槽名称都是固定的；

比如v-slot:left、v-slot:center等等；

我们可以通过v-slot:[dynamicSlotName]方式动态绑定一个名称；

**具名插槽使用的时候缩写**

跟v-on 和v-bind 一样，v-slot 也有缩写；

即把参数之前的所有内容(v-slot:)替换为字符#

```vue
<template>
  <div>
    <nav-bar :name="name">
      <template #left>
        <button>左边的按钮</button>
      </template>
      <template #center>
        <h2>我是标题</h2>
      </template>
      <template #right>
        <i>右边的i元素</i>
      </template>
      <template #[name]>
        <i>why内容</i>
      </template>
    </nav-bar>
  </div>
</template>

<script>
  import NavBar from './NavBar.vue';

  export default {
    components: {
      NavBar
    },
    data() {
      return {
        name: "why"
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div class="nav-bar">
    <!-- <slot name="default"></slot> -->
    <div class="left">
      <slot name="left"></slot>
    </div>
    <div class="center">
      <slot name="center"></slot>
    </div>
    <div class="right">
      <slot name="right"></slot>
    </div>
    <div class="addition">
      <slot :name="name"></slot>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      name: String
    }
    // data() {
    //   return {
    //     name: "why"
    //   }
    // }
  }
</script>

<style scoped>
  .nav-bar {
    display: flex;
  }

  .left, .right, .center {
    height: 44px;
  }

  .left, .right, .addition {
    width: 80px;
    background-color: red;
  }

  .center {
    flex: 1;
    background-color: blue;
  }
</style>
```

### 认识作用域插槽

1.在App.vue中定义好数据

2.传递给ShowNames组件中

3.ShowNames组件中遍历names数据

4.定义插槽的prop

5.通过v-slot:default的方式获取到slot的props

6.使用slotProps中的item和index认识作用域插槽

**独占默认插槽的缩写**

如果我们的插槽是默认插槽default，那么在使用的时候v-slot:default="slotProps"可以简写为v-slot="slotProps"

![image-20220301111125200](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301111125200.png)

```vue
<template>
  <div>
    <!-- 编译作用域 -->
    <!-- <child-cpn>
      <button>{{title}}</button>
    </child-cpn> -->

    <show-names :names="names">
      <template v-slot="coderwhy">
        <button>{{coderwhy.item}}-{{coderwhy.index}}</button>
      </template>
    </show-names>

    <show-names :names="names" v-slot="coderwhy">
      <button>{{coderwhy.item}}-{{coderwhy.index}}</button>
    </show-names>

    <!-- 注意: 如果还有其他的具名插槽, 那么默认插槽也必须使用template来编写 -->
    <show-names :names="names">
      <template v-slot="coderwhy">
        <button>{{coderwhy.item}}-{{coderwhy.index}}</button>
      </template>

      <template v-slot:why>
        <h2>我是why的插入内容</h2>
      </template>
    </show-names>

    <show-names :names="names">
      <template v-slot="slotProps">
        <strong>{{slotProps.item}}-{{slotProps.index}}</strong>
      </template>
    </show-names>
  </div>
</template>

<script>
  import ChildCpn from './ChildCpn.vue';
  import ShowNames from './ShowNames.vue';

  export default {
    components: {
      ChildCpn,
      ShowNames
    },
    data() {
      return {
        names: ["why", "kobe", "james", "curry"]
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
    <template v-for="(item, index) in names" :key="item">
      <slot :item="item" :index="index"></slot>

      <slot name="why"></slot>
    </template>
  </div>
</template>

<script>
  export default {
    props: {
      names: {
        type: Array,
        default: () => []
      }
    }
  }
</script>

<style scoped>

</style>
```

### 动态组件

动态组件是使用component 组件，通过一个特殊的attribute is 来实现：

这个currentTab的值需要是什么内容呢？

可以是通过component函数注册的组件；

在一个组件对象的components对象中注册的组件；

**keep-alive**

比如我们将counter点到10，那么在切换到home再切换回来about时，状态是否可以保持呢？

答案是否定的；

这是因为默认情况下，我们在切换组件后，about组件会被销毁掉，再次回来时会重新创建组件；

但是，在开发中某些情况我们希望继续保持组件的状态，而不是销毁掉，这个时候我们就可以使用一个内置组件：keep-alive。

```vue
<template>
  <div>
    <button v-for="item in tabs" :key="item"
            @click="itemClick(item)"
            :class="{active: currentTab === item}">
      {{item}}
    </button>

    <!-- 2.动态组件 -->
    <keep-alive include="home,about">
      <component :is="currentTab"
                 name="coderwhy"
                 :age="18"
                 @pageClick="pageClick">
      </component>
    </keep-alive>
    

    <!-- 1.v-if的判断实现 -->
    <!-- <template v-if="currentTab === 'home'">
      <home></home>
    </template>
    <template v-else-if="currentTab === 'about'">
      <about></about>
    </template>
    <template v-else>
      <category></category>
    </template> -->
  </div>
</template>

<script>
  import Home from './pages/Home.vue';
  import About from './pages/About.vue';
  import Category from './pages/Category.vue';

  export default {
    components: {
      Home,
      About,
      Category
    },
    data() {
      return {
        tabs: ["home", "about", "category"],
        currentTab: "home"
      }
    },
    methods: {
      itemClick(item) {
        this.currentTab = item;
      },
      pageClick() {
        console.log("page内部发生了点击");
      }
    }
  }
</script>

<style scoped>
  .active {
    color: red;
  }
</style>
```

### 异步组件

如果我们的项目过大了，对于某些组件我们希望通过异步的方式来进行加载（目的是可以对其进行分包处理），那么Vue中给我们提供了一个函数：defineAsyncComponent。

```vue
<template>
  <div>
    App组件
    <home></home>

    <suspense>
      <template #default>
        <async-category></async-category>
      </template>
      <template #fallback>
        <loading></loading>
      </template>
    </suspense>

  </div>
</template>

<script>
  import { defineAsyncComponent } from 'vue';

  import Home from './Home.vue';
  import Loading from './Loading.vue';

  // import AsyncCategory from './AsyncCategory.vue';
  const AsyncCategory = defineAsyncComponent(() => import("./AsyncCategory.vue"))

  // const AsyncCategory = defineAsyncComponent({
  //   loader: () => import("./AsyncCategory.vue"),
  //   loadingComponent: Loading,
  //   // errorComponent,
  //   // 在显示loadingComponent组件之前, 等待多长时间
  //   delay: 2000,
  //   /**
  //    * err: 错误信息,
  //    * retry: 函数, 调用retry尝试重新加载
  //    * attempts: 记录尝试的次数
  //    */
  //   onError: function(err, retry, attempts) {

  //   }
  // })

  export default {
    components: {
      Home,
      AsyncCategory,
      Loading
    }
  }
</script>

<style scoped>

</style>
```

### ref

$refs的使用

某些情况下，我们在组件中想要直接获取到元素对象或者子组件实例：

在Vue开发中我们是不推荐进行DOM操作的；

这个时候，我们可以给元素或者组件绑定一个ref的attribute属性；

组件实例有一个$refs属性：

它一个对象Object，持有注册过ref attribute 的所有DOM元素和组件实例

```vue
<template>
  <div>
    <!-- 绑定到一个元素上 -->
    <h2 ref="title">哈哈哈</h2>

    <!-- 绑定到一个组件实例上 -->
    <nav-bar ref="navBar"></nav-bar>

    <button @click="btnClick">获取元素</button>
  </div>
</template>

<script>
  import NavBar from './NavBar.vue';

  export default {
    components: {
      NavBar
    },
    data() {
      return {
        names: ["abc", "cba"]
      }
    },
    methods: {
      btnClick() {
        console.log(this.$refs.title);

        console.log(this.$refs.navBar.message);
        this.$refs.navBar.sayHello();

        // $el
        console.log(this.$refs.navBar.$el);
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
    <h2>NavBar</h2>
    <button @click="getParentAndRoot">获取父组件和根组件</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: "我是NavBar中的message"
      }
    },
    methods: {
      sayHello() {
        console.log("Hello NavBar");
      },
      getParentAndRoot() {
        console.log(this.$parent);
        console.log(this.$root);
      }
    }
  }
</script>

<style scoped>

</style>
```

### 生命周期函数

生命周期函数是一些钩子函数，在某个时间会被Vue源码内部进行回调；p通过对生命周期函数的回调，我们可以知道目前组件正在经历什么阶段；

```vue
<template>
  <div>
    <h2 ref="title">{{message}}</h2>
    <button @click="changeMessage">修改message</button>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: "Hello Home"
      }
    },
    methods: {
      changeMessage() {
        this.message = "你好啊, 李银河"
      }
    },
    beforeCreate() {
      console.log("home beforeCreate");
    },
    created() {
      console.log("home created");
    },
    beforeMount() {
      console.log("home beforeMount");
    },
    mounted() {
      console.log("home mounted");
    },
    beforeUnmount() {
      console.log("home beforeUnmount");
    },
    unmounted() {
      console.log("home unmounted");
    },
    beforeUpdate() {
      console.log(this.$refs.title.innerHTML);
      console.log("home beforeUpdate");
    },
    updated() {
      console.log(this.$refs.title.innerHTML);
      console.log("home updated");
    }
  }
</script>

<style scoped>

</style>
```

### 组件的v-model

前面我们在input中可以使用v-model来完成双向绑定：

这个时候往往会非常方便，因为v-model默认帮助我们完成了两件事；

v-bind:value的数据绑定和@input的事件监听；

如果我们现在封装了一个组件，其他地方在使用这个组件时，是否也可以使用v-model来同时完成这两个功能呢？

也是可以的，vue也支持在组件上使用v-model；

当我们在组件上使用的时候，等价于如下的操作：

我们会发现和input元素不同的只是属性的名称和事件触发的名称而已；

### 组件v-model的实现

那么，为了我们的MyInput组件可以正常的工作，这个组件内的<input> 必须：

将其value attribute 绑定到一个名叫modelValue的prop 上；

在其input 事件被触发时，将新的值通过自定义的update:modelValue事件抛出

### computed实现

我们依然希望在组件内部按照双向绑定的做法去完成，应该如何操作呢？

**但是又不能通过修改props的值，因为不推荐修改props，第二是因为修改当前组件的props外界的值也不会随之改变**

我们可以使用计算属性的setter和getter来完成。

### 绑定多个属性

我们现在通过v-model是直接绑定了一个属性，如果我们希望绑定多个属性呢？

也就是我们希望在一个组件上使用多个v-model是否可以实现呢？

我们知道，默认情况下的v-model其实是绑定了modelValue属性和@update:modelValue的事件；

如果我们希望绑定更多，可以给v-model传入一个参数，那么这个参数的名称就是我们绑定属性的名称；

注意：这里我是绑定了两个属性的

v-model:title相当于做了两件事：

​	绑定了title属性；

​	监听了@update:title的事件；

```vue
<template>
  <div>
    <!-- <input v-model="message">
    <input :value="message" @input="message = $event.target.value"> -->

    <!-- 组件上使用v-model -->
    <!-- <hy-input v-model="message"></hy-input> -->
    <!-- <hy-input :modelValue="message" @update:model-value="message = $event"></hy-input> -->

    <!-- 绑定两个v-model -->
    <hy-input v-model="message" v-model:title="title"></hy-input>

    <h2>{{message}}</h2>
    <h2>{{title}}</h2>
  </div>
</template>

<script>
  import HyInput from './HyInput.vue';

  export default {
    components: {
      HyInput
    },
    data() {
      return {
        message: "Hello World",
        title: "哈哈哈"
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
    <input v-model="value">
    <input v-model="why">
  </div>
</template>

<script>
  export default {
    props: {
      modelValue: String,
      title: String 
    },
    emits: ["update:modelValue", "update:title"],
    computed: {
      value: {
        set(value) {
          this.$emit("update:modelValue", value);
        },
        get() {
          return this.modelValue;
        }
      },
      why: {
        set(why) {
          this.$emit("update:title", why);
        },
        get() {
          return this.title;
        }
      }
    }
  }
</script>

<style scoped>

</style>
```

```vue
<template>
  <div>
    <!-- 1.默认绑定和事件处理 -->
    <!-- <button @click="btnClick">hyinput按钮</button>
    <h2>HyInput的message: {{modelValue}}</h2> -->

    <!-- 2.通过input -->
    <!-- <input :value="modelValue" @input="btnClick"> -->

    <!-- 3.绑定到props中是不对的 -->
    <!-- <input v-model="modelValue"> -->

    <!-- 4. -->
    <input v-model="value">

  </div>
</template>

<script>
  export default {
    props: {
      modelValue: String
    },
    emits: ["update:modelValue"],
    computed: {
      value: {
        set(value) {
          this.$emit("update:modelValue", value);
        },
        get() {
          return this.modelValue;
        }
      }
    },
    methods: {
      btnClick(event) {
        this.$emit("update:modelValue", event.target.value);
      }
    }
  }
</script>

<style scoped>

</style>
```

## Composition API

### 认识Mixin

目前我们是使用组件化的方式在开发整个Vue的应用程序，但是组件和组件之间有时候会存在相同的代码逻辑，我们希望对相同的代码逻辑进行抽取。

在Vue2和Vue3中都支持的一种方式就是使用Mixin来完成：

Mixin提供了一种非常灵活的方式，来分发Vue组件中的可复用功能；

一个Mixin对象可以包含任何组件选项；

当组件使用Mixin对象时，所有Mixin对象的选项将被混合进入该组件本身的选项中

### Mixin的合并规则

如果Mixin对象中的选项和组件对象中的选项发生了冲突，那么Vue会如何操作呢？

这里分成不同的情况来进行处理；

情况一：如果是data函数的返回值对象

返回值对象默认情况下会进行合并；

如果data返回值对象的属性发生了冲突，那么会保留组件自身的数据；

情况二：如何生命周期钩子函数

生命周期的钩子函数会被合并到数组中，都会被调用；

情况三：值为对象的选项，例如methods、components 和directives，将被合并为同一个对象。

比如都有methods选项，并且都定义了方法，那么它们都会生效；

但是如果对象的key相同，那么会取组件对象的键值对；

### OptionsAPI的弊端

在Vue2中，我们编写组件的方式是Options API：

Options API的一大特点就是在对应的属性中编写对应的功能模块；

比如data定义数据、methods中定义方法、computed中定义计算属性、watch中监听属性改变，也包括生命周期钩子；

但是这种代码有一个很大的弊端：

当我们实现某一个功能时，这个功能对应的代码逻辑会被拆分到各个属性中；

当我们组件变得更大、更复杂时，逻辑关注点的列表就会增长，那么同一个功能的逻辑就会被拆分的很分散；

尤其对于那些一开始没有编写这些组件的人来说，这个组件的代码是难以阅读和理解的（阅读组件的其他人）；

下面我们来看一个非常大的组件，其中的逻辑功能按照颜色进行了划分：

这种碎片化的代码使用理解和维护这个复杂的组件变得异常困难，并且隐藏了潜在的逻辑问题；

并且当我们处理单个逻辑关注点时，需要不断的跳到相应的代码块中

### 认识CompositionAPI

那么既然知道Composition API想要帮助我们做什么事情，接下来看一下到底是怎么做呢？

为了开始使用Composition API，我们需要有一个可以实际使用它（编写代码）的地方；

在Vue组件中，这个位置就是setup 函数；

setup其实就是组件的另外一个选项：

只不过这个选项强大到我们可以用它来替代之前所编写的大部分其他选项；

比如methods、computed、watch、data、生命周期等等；

接下来我们一起学习这个函数的使用：

函数的参数

函数的返回值

### setup函数的参数

我们先来研究一个setup函数的参数，它主要有两个参数：

第一个参数：propsp第二个参数：contextnprops非常好理解，它其实就是父组件传递过来的属性会被放到props对象中，我们在setup中如果需要使用，那么就可以直接通过props参数获取：

对于定义props的类型，我们还是和之前的规则是一样的，在props选项中定义；

并且在template中依然是可以正常去使用props中的属性，比如message；

如果我们在setup函数中想要使用props，那么不可以通过this 去获取（后面我会讲到为什么）；

因为props有直接作为参数传递到setup函数中，所以我们可以直接通过参数来使用即可；

另外一个参数是context，我们也称之为是一个SetupContext，它里面包含三个属性：

attrs：所有的非prop的attribute；

slots：父组件传递过来的插槽（这个在以渲染函数返回时会有作用，后面会讲到）；

emit：当我们组件内部需要发出事件时会用到emit（因为我们不能访问this，所以不可以通过this.$emit发出事件）；

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>

    <h2>{{title}}</h2>
    <h2>当前计数: {{counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    data() {
      return {
        counter: 100
      }
    },
    /**
     * 参数一: props, 父组件传递过来属性
     */
    // setup函数有哪些参数?
    // setup函数有什么样的返回值
    // setup(props, context) {
    setup(props, {attrs, slots, emit}) {
      console.log(props.message);
      console.log(attrs.id, attrs.class);
      console.log(slots);
      console.log(emit);

      return {
        title: "Hello Home",
        counter: 100
      }
    },
    methods: {
      btnClick() {
        this.$emit("")
      }
    }
  }
</script>

<style scoped>

</style>
```

### setup函数的返回值

setup既然是一个函数，那么它也可以有返回值，它的返回值用来做什么呢？

setup的返回值可以在模板template中被使用；

也就是说我们可以通过setup的返回值来替代data选项；

甚至是我们可以返回一个执行函数来代替在methods中定义的方法：

但是，如果我们将counter在increment 或者decrement进行操作时，是否可以实现界面的响应式呢？

答案是不可以；

这是因为对于一个定义的变量来说，默认情况下，Vue并不会跟踪它的变化，来引起界面的响应式操作

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>

    <h2>{{title}}</h2>
    <h2>当前计数: {{counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    setup() {
      let counter = 100;

      // 局部函数
      const increment = () => {
        counter++;
        console.log(counter);
      }

      return {
        title: "Hello Home",
        counter,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```

### setup不可以使用this

表达的含义是this并没有指向当前组件实例；

并且在setup被调用之前，data、computed、methods等都没有被解析；

所以无法在setup中获取this

### ReactiveAPI

这是因为当我们使用reactive函数处理我们的数据之后，数据再次被使用时就会进行依赖收集；

当数据发生改变时，所有收集到的依赖都是进行对应的响应式操作（比如更新界面）；

事实上，我们编写的data选项，也是在内部交给了reactive函数将其编程响应式对象的

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>
    <h2>当前计数: {{state.counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
  import { reactive } from 'vue';

  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    setup() {
      const state = reactive({
        counter: 100
      })

      // 局部函数
      const increment = () => {
        state.counter++;
        console.log(state.counter);
      }

      return {
        state,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```

### RefAPI

reactive API对传入的类型是有限制的，它要求我们必须传入的是一个对象或者数组类型：

如果我们传入一个基本数据类型（String、Number、Boolean）会报一个警告；

这个时候Vue3给我们提供了另外一个API：ref APIpref会返回一个可变的响应式对象，该对象作为一个响应式的引用维护着它内部的值，这就是ref名称的来源；

它内部的值是在ref的value 属性中被维护的；

这里有两个注意事项：

在模板中引入ref的值时，Vue会自动帮助我们进行解包操作，所以我们并不需要在模板中通过ref.value的方式来使用；

但是在setup 函数内部，它依然是一个ref引用，所以对其进行操作时，我们依然需要使用ref.value的方式；

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>
    <!-- 当我们在template模板中使用ref对象, 它会自动进行解包 -->
    <h2>当前计数: {{counter}}</h2>
    <button @click="increment">+1</button>

    <show-message :message="counter"></show-message>
  </div>
</template>

<script>
  import { ref } from 'vue';

  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    setup() {
      // counter编程一个ref的可响应式的引用
      // counter = 100;
      let counter = ref(100);

      // 局部函数
      const increment = () => {
        counter.value++;
        console.log(counter.value);
      }

      return {
        counter,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```

**Ref自动解包**

模板中的解包是浅层的解包，如果我们的代码是下面的方式：

如果我们将ref放到一个reactive的属性当中，那么在模板中使用时，它会自动解包

```vue
<template>
  <div>
    Home Page
    <h2>{{message}}</h2>
    <!-- 当我们在template模板中使用ref对象, 它会自动进行解包 -->
    <h2>当前计数: {{counter}}</h2>
    <!-- ref的解包只能是一个浅层解包(info是一个普通的JavaScript对象) -->
    <h2>当前计数: {{info.counter.value}}</h2>
    <!-- 当如果最外层包裹的是一个reactive可响应式对象, 那么内容的ref可以解包 -->
    <h2>当前计数: {{reactiveInfo.counter}}</h2>
    <button @click="increment">+1</button>
  </div>
</template>

<script>
  import { ref, reactive } from 'vue';

  export default {
    props: {
      message: {
        type: String,
        required: true
      }
    },
    setup() {
      let counter = ref(100);

      const info = {
        counter
      }

      const reactiveInfo = reactive({
        counter
      })

      // 局部函数
      const increment = () => {
        counter.value++;
        console.log(counter.value);
      }

      return {
        counter,
        info,
        reactiveInfo,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```

### 认识readonly

我们通过reactive或者ref可以获取到一个响应式的对象，但是某些情况下，我们传入给其他地方（组件）的这个响应式对象希望在另外一个地方（组件）被使用，但是不能被修改，这个时候如何防止这种情况的出现呢？

Vue3为我们提供了readonly的方法；

readonly会返回原生对象的只读代理（也就是它依然是一个Proxy，这是一个proxy的set方法被劫持，并且不能对其进行修改）；

在开发中常见的readonly方法会传入三个类型的参数：

类型一：普通对象；

类型二：reactive返回的对象；

类型三：ref的对象

### readonly的使用

在readonly的使用过程中，有如下规则：preadonly返回的对象都是不允许修改的；

但是经过readonly处理的原来的对象是允许被修改的：比如const info = readonly(obj)，info对象是不允许被修改的；

当obj被修改时，readonly返回的info对象也会被修改；

但是我们不能去修改readonly返回的对象info；

其实本质上就是readonly返回的对象的setter方法被劫持了而已；

```vue
<template>
  <div>
    <button @click="updateState">修改状态</button>
  </div>
</template>

<script>
  import { reactive, ref, readonly } from 'vue';

  export default {
    setup() {
      // 1.普通对象
      const info1 = {name: "why"};
      const readonlyInfo1 = readonly(info1);

      // 2.响应式的对象reactive
      const info2 = reactive({
        name: "why"
      })
      const readonlyInfo2 = readonly(info2);

      // 3.响应式的对象ref
      const info3 = ref("why");
      const readonlyInfo3 = readonly(info3);

      const updateState = () => {
        // readonlyInfo3.value = "coderwhy"
        info3.value = "coderwhy";
      }

      return {
        updateState,
      }
    }
  }
</script>

<style scoped>

</style>
```

### Reactive判断的API

isProxy

检查对象是否是由reactive 或readonly创建的proxy。

isReactive

检查对象是否是由reactive创建的响应式代理：

如果该代理是readonly建的，但包裹了由reactive 创建的另一个代理，它也会返回true；

isReadonly

检查对象是否是由readonly创建的只读代理。

toRaw

返回reactive 或readonly代理的原始对象（不建议保留对原始对象的持久引用。请谨慎使用）。

shallowReactive

创建一个响应式代理，它跟踪其自身property 的响应性，但不执行嵌套对象的深层响应式转换(深层还是原生对象)。n

shallowReadonly

创建一个proxy，使其自身的property 为只读，但不执行嵌套对象的深度只读转换（深层还是可读、可写的）

### toRefs

如果我们使用ES6的解构语法，对reactive返回的对象进行解构获取值，那么之后无论是修改结构后的变量，还是修改reactive返回的state对象，数据都不再是响应式的：

那么有没有办法让我们解构出来的属性是响应式的呢？

Vue为我们提供了一个toRefs的函数，可以将reactive返回的对象中的属性都转成ref；

那么我们再次进行结构出来的name 和age 本身都是ref的；

这种做法相当于已经在state.name和ref.value之间建立了链接，任何一个修改都会引起另外一个变化

### toRef

如果我们只希望转换一个reactive对象中的属性为ref, 那么可以使用toRef的方法：

```vue
<template>
  <div>
    <h2>{{name}}-{{age}}</h2>
    <button @click="changeAge">修改age</button>
  </div>
</template>

<script>
  import { reactive, toRefs, toRef } from 'vue';

  export default {
    setup() {
      const info = reactive({name: "why", age: 18});
      // 1.toRefs: 将reactive对象中的所有属性都转成ref, 建立链接
      // let { name, age } = toRefs(info);
      // 2.toRef: 对其中一个属性进行转换ref, 建立链接
      let { name } = info;
      let age = toRef(info, "age");

      const changeAge = () => {
        age.value++;
      }

      return {
        name,
        age,
        changeAge
      }
    }
  }
</script>

<style scoped>

</style>
```

### ref其他的API

unref

如果我们想要获取一个ref引用中的value，那么也可以通过unref方法：

如果参数是一个ref，则返回内部值，否则返回参数本身；

这是val= isRef(val) ? val.value: val的语法糖函数；

isRef

判断值是否是一个ref对象。

shallowRef

创建一个浅层的ref对象；

triggerRef

手动触发和shallowRef相关联的副作用：

```vue
<template>
  <div>
    <h2>{{info}}</h2>
    <button @click="changeInfo">修改Info</button>
  </div>
</template>

<script>
  import { ref, shallowRef, triggerRef } from 'vue';

  export default {
    setup() {
      const info = shallowRef({name: "why"})

      const changeInfo = () => {
        info.value.name = "james";
        triggerRef(info);
      }

      return {
        info,
        changeInfo
      }
    }
  }
</script>

<style scoped>

</style>
```

### customRef

创建一个自定义的ref，并对其依赖项跟踪和更新触发进行显示控制：

它需要一个工厂函数，该函数接受track 和trigger 函数作为参数；

并且应该返回一个带有get 和set 的对象；

这里我们使用一个的案例：

对双向绑定的属性进行debounce(节流)的操作；

```js
import { customRef } from 'vue';

// 自定义ref
export default function(value, delay = 300) {
  let timer = null;
  return customRef((track, trigger) => {
    return {
      get() {
        track();
        return value;
      },
      set(newValue) {
        clearTimeout(timer);
        timer = setTimeout(() => {
          value = newValue;
          trigger();
        }, delay);
      }
    }
  })
}

```

```vue
<template>
  <div>
    <input v-model="message"/>
    <h2>{{message}}</h2>
  </div>
</template>

<script>
  import debounceRef from './hook/useDebounceRef';

  export default {
    setup() {
      const message = debounceRef("Hello World");

      return {
        message
      }
    }
  }
</script>

<style scoped>

</style>
```

### computed

如何使用computed呢？

方式一：接收一个getter函数，并为getter 函数返回的值，返回一个不变的ref 对象；

方式二：接收一个具有get 和set 的对象，返回一个可变的（可读写）ref 对象；

```vue
<template>
  <div>
    <h2>{{fullName}}</h2>
    <button @click="changeName">修改firstName</button>
  </div>
</template>

<script>
  import { ref, computed } from 'vue';

  export default {
    setup() {
      const firstName = ref("Kobe");
      const lastName = ref("Bryant");

      // 1.用法一: 传入一个getter函数
      // computed的返回值是一个ref对象
      const fullName = computed(() => firstName.value + " " + lastName.value);

      // 2.用法二: 传入一个对象, 对象包含getter/setter
      const fullName = computed({
        get: () => firstName.value + " " + lastName.value,
        set(newValue) {
          const names = newValue.split(" ");
          firstName.value = names[0];
          lastName.value = names[1];
        }
      });

      const changeName = () => {
        // firstName.value = "James"
        fullName.value = "coder why";
      }

      return {
        fullName,
        changeName
      }
    }
  }
</script>

<style scoped>

</style>
```

### 侦听数据的变化

在前面的Options API中，我们可以通过watch选项来侦听data或者props的数据变化，当数据变化时执行某一些操作。

在Composition API中，我们可以使用watchEffect和watch来完成响应式数据的侦听；

watchEffect用于自动收集响应式数据的依赖；pwatch需要手动指定侦听的数据源；

#### watchEffect

当侦听到某些响应式数据变化时，我们希望执行某些操作，这个时候可以使用watchEffect。

我们来看一个案例：

首先，watchEffect传入的函数会被立即执行一次，并且在执行的过程中会收集依赖；

其次，只有收集的依赖发生变化时，watchEffect传入的函数才会再次执行；

```vue
<template>
  <div>
    <h2>{{name}}-{{age}}</h2>
    <button @click="changeName">修改name</button>
    <button @click="changeAge">修改age</button>
  </div>
</template>

<script>
  import { ref, watchEffect } from 'vue';

  export default {
    setup() {
      // watchEffect: 自动收集响应式的依赖
      const name = ref("why");
      const age = ref(18);

      const changeName = () => name.value = "kobe"
      const changeAge = () => age.value++

      watchEffect(() => {
        console.log("name:", name.value, "age:", age.value);
      });

      return {
        name,
        age,
        changeName,
        changeAge
      }
    }
  }
</script>

<style scoped>

</style>
```



#### watchEffect的停止侦听

如果在发生某些情况下，我们希望停止侦听，这个时候我们可以获取watchEffect的返回值函数，调用该函数即可。

比如在上面的案例中，我们age达到20的时候就停止侦听：

```vue
<template>
  <div>
    <h2>{{name}}-{{age}}</h2>
    <button @click="changeName">修改name</button>
    <button @click="changeAge">修改age</button>
  </div>
</template>

<script>
  import { ref, watchEffect } from 'vue';

  export default {
    setup() {
      // watchEffect: 自动收集响应式的依赖
      const name = ref("why");
      const age = ref(18);

      const stop = watchEffect(() => {
        console.log("name:", name.value, "age:", age.value);
      });

      const changeName = () => name.value = "kobe"
      const changeAge = () => {
        age.value++;
        if (age.value > 25) {
          stop();
        }
      }

      return {
        name,
        age,
        changeName,
        changeAge
      }
    }
  }
</script>

<style scoped>

</style>
```

#### watchEffect清除副作用

什么是清除副作用呢？

比如在开发中我们需要在侦听函数中执行网络请求，但是在网络请求还没有达到的时候，我们停止了侦听器，或者侦听器侦听函数被再次执行了。

那么上一次的网络请求应该被取消掉，这个时候我们就可以清除上一次的副作用；

在我们给watchEffect传入的函数被回调时，其实可以获取到一个参数：onInvalidate

当副作用即将重新执行或者侦听器被停止时会执行该函数传入的回调函数；

我们可以在传入的回调函数中，执行一些清楚工作

```vue
<template>
  <div>
    <h2>{{name}}-{{age}}</h2>
    <button @click="changeName">修改name</button>
    <button @click="changeAge">修改age</button>
  </div>
</template>

<script>
  import { ref, watchEffect } from 'vue';

  export default {
    setup() {
      // watchEffect: 自动收集响应式的依赖
      const name = ref("why");
      const age = ref(18);

      const stop = watchEffect((onInvalidate) => {
        const timer = setTimeout(() => {
          console.log("网络请求成功~");
        }, 2000)

        // 根据name和age两个变量发送网络请求
        onInvalidate(() => {
          // 在这个函数中清除额外的副作用
          // request.cancel()
          clearTimeout(timer);
          console.log("onInvalidate");
        })
        console.log("name:", name.value, "age:", age.value);
      });

      const changeName = () => name.value = "kobe"
      const changeAge = () => {
        age.value++;
        if (age.value > 25) {
          stop();
        }
      }

      return {
        name,
        age,
        changeName,
        changeAge
      }
    }
  }
</script>

<style scoped>

</style>
```

#### setup中使用ref

在讲解watchEffect执行时机之前，我们先补充一个知识：在setup中如何使用ref或者元素或者组件？

其实非常简单，我们只需要定义一个ref对象，绑定到元素或者组件的ref属性上即可

![image-20220301145422968](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301145422968.png)

#### watchEffect的执行时机

默认情况下，组件的更新会在副作用函数执行之前：

如果我们希望在副作用函数中获取到元素，是否可行呢？

我们会发现打印结果打印了两次：

这是因为setup函数在执行时就会立即执行传入的副作用函数，这个时候DOM并没有挂载，所以打印为null；

而当DOM挂载时，会给title的ref对象赋值新的值，副作用函数会再次执行，打印出来对应的元素；

#### 调整watchEffect的执行时机

如果我们希望在第一次的时候就打印出来对应的元素呢？

这个时候我们需要改变副作用函数的执行时机；

它的默认值是pre，它会在元素挂载或者更新之前执行；

所以我们会先打印出来一个空的，当依赖的title发生改变时，就会再次执行一次，打印出元素；

我们可以设置副作用函数的执行时机：

flush 选项还接受sync，这将强制效果始终同步触发。然而，这是低效的，应该很少需要

```vue
<template>
  <div>
    <h2 ref="title">哈哈哈</h2>
  </div>
</template>

<script>
  import { ref, watchEffect } from 'vue';

  export default {
    setup() {
      const title = ref(null);

      watchEffect(() => {
        console.log(title.value);
      }, {
        flush: "post"
      })

      return {
        title
      }
    }
  }
</script>

<style scoped>

</style>
```

#### Watch的使用

watch的API完全等同于组件watch选项的Property：

watch需要侦听特定的数据源，并在回调函数中执行副作用；

默认情况下它是惰性的，只有当被侦听的源发生变化时才会执行回调；

与watchEffect的比较，watch允许我们：

懒执行副作用（第一次不会直接执行）；

更具体的说明当哪些状态发生变化时，触发侦听器的执行；

访问侦听状态变化前后的值；Watch的使用

#### 侦听单个数据源

watch侦听函数的数据源有两种类型：

一个getter函数：但是该getter函数必须引用可响应式的对象（比如reactive或者ref）；

直接写入一个可响应式的对象，reactive或者ref（比较常用的是ref）

```vue
<template>
  <div>
    <h2 ref="title">{{info.name}}</h2>
    <button @click="changeData">修改数据</button>
  </div>
</template>

<script>
  import { ref, reactive, watch } from 'vue';

  export default {
    setup() {
      const info = reactive({name: "why", age: 18});

      // 1.侦听watch时,传入一个getter函数
      watch(() => info.name, (newValue, oldValue) => {
        console.log("newValue:", newValue, "oldValue:", oldValue);
      })

      // 2.传入一个可响应式对象: reactive对象/ref对象
      // 情况一: reactive对象获取到的newValue和oldValue本身都是reactive对象
      // watch(info, (newValue, oldValue) => {
      //   console.log("newValue:", newValue, "oldValue:", oldValue);
      // })
      // 如果希望newValue和oldValue是一个普通的对象
      watch(() => {
        return {...info}
      }, (newValue, oldValue) => {
        console.log("newValue:", newValue, "oldValue:", oldValue);
      })
      // 情况二: ref对象获取newValue和oldValue是value值的本身
      // const name = ref("why");
      // watch(name, (newValue, oldValue) => {
      //   console.log("newValue:", newValue, "oldValue:", oldValue);
      // })

      const changeData = () => {
        info.name = "kobe";
      }

      return {
        changeData,
        info
      }
    }
  }
</script>

<style scoped>

</style>
```

#### 侦听多个数据源

侦听器还可以使用数组同时侦听多个源：

```vue
<template>
  <div>
    <h2 ref="title">{{info.name}}</h2>
    <button @click="changeData">修改数据</button>
  </div>
</template>

<script>
  import { ref, reactive, watch } from 'vue';

  export default {
    setup() {
      // 1.定义可响应式的对象
      const info = reactive({name: "why", age: 18});
      const name = ref("why");

      // 2.侦听器watch
      watch([() => ({...info}), name], ([newInfo, newName], [oldInfo, oldName]) => {
        console.log(newInfo, newName, oldInfo, oldName);
      })

      const changeData = () => {
        info.name = "kobe";
      }

      return {
        changeData,
        info
      }
    }
  }
</script>

<style scoped>

</style>
```

#### 侦听响应式对象

如果我们希望侦听一个数组或者对象，那么可以使用一个getter函数，并且对可响应对象进行解构：

#### watch的选项

如果我们希望侦听一个深层的侦听，那么依然需要设置deep 为true：

也可以传入immediate 立即执行；

```vue
<template>
  <div>
    <h2 ref="title">{{info.name}}</h2>
    <button @click="changeData">修改数据</button>
  </div>
</template>

<script>
  import { ref, reactive, watch } from 'vue';

  export default {
    setup() {
      // 1.定义可响应式的对象
      const info = reactive({
        name: "why", 
        age: 18,
        friend: {
          name: "kobe"
        }
      });

      // 2.侦听器watch
      watch(() => ({...info}), (newInfo, oldInfo) => {
        console.log(newInfo, oldInfo);
      }, {
        deep: true,
        immediate: true
      })

      const changeData = () => {
        info.friend.name = "james";
      }

      return {
        changeData,
        info
      }
    }
  }
</script>

<style scoped>

</style>
```

### 生命周期钩子

那么setup中如何使用生命周期函数呢？

可以使用直接导入的onX函数注册生命周期钩子

```vue
<template>
  <div>
    <button @click="increment">{{counter}}</button>
  </div>
</template>

<script>
  import { onMounted, onUpdated, onUnmounted, ref } from 'vue';

  export default {
    setup() {
      const counter = ref(0);
      const increment = () => counter.value++

      onMounted(() => {
        console.log("App Mounted1");
      })
      onMounted(() => {
        console.log("App Mounted2");
      })
      onUpdated(() => {
        console.log("App onUpdated");
      })
      onUnmounted(() => {
        console.log("App onUnmounted");
      })

      return {
        counter,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```

### Provide函数

事实上我们之前还学习过Provide和Inject，Composition API也可以替代之前的Provide 和Inject 的选项。

我们可以通过provide来提供数据：

可以通过provide 方法来定义每个Property；

provide可以传入两个参数：

name：提供的属性名称；

value：提供的属性值；

```vue
<template>
  <div>
    <home/>
    <h2>App Counter: {{counter}}</h2>
    <button @click="increment">App中的+1</button>
  </div>
</template>

<script>
  import { provide, ref, readonly } from 'vue';

  import Home from './Home.vue';

  export default {
    components: {
      Home
    },
    setup() {
      const name = ref("coderwhy");
      let counter = ref(100);

      provide("name", readonly(name));
      provide("counter", readonly(counter));

      const increment = () => counter.value++;

      return {
        increment,
        counter
      }
    }
  }
</script>

<style scoped>

</style>
```

### Inject函数

在后代组件中可以通过inject 来注入需要的属性和对应的值：

可以通过inject 来注入需要的内容；

inject可以传入两个参数：

要inject 的property 的name；

默认值

```vue
<template>
  <div>
    <h2>{{name}}</h2>
    <h2>{{counter}}</h2>

    <button @click="homeIncrement">home+1</button>
  </div>
</template>

<script>
  import { inject } from 'vue';

  export default {
    setup() {
      const name = inject("name");
      const counter = inject("counter");

      const homeIncrement = () => counter.value++

      return {
        name,
        counter,
        homeIncrement
      }
    }
  }
</script>

<style scoped>

</style>
```

### 数据的响应式

为了增加provide 值和inject 值之间的响应性，我们可以在provide 值时使用ref 和reactive。

### 修改响应式Property

如果我们需要修改可响应的数据，那么最好是在数据提供的位置来修改：

我们可以将修改方法进行共享，在后代组件中进行调用

![image-20220301150856069](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301150856069.png)

### hooks的抽取

![image-20220301151144897](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301151144897.png)

**useCounter**

```js
import { ref, computed } from 'vue';

export default function() {
  const counter = ref(0);
  const doubleCounter = computed(() => counter.value * 2);

  const increment = () => counter.value++;
  const decrement = () => counter.value--;

  return {
    counter, 
    doubleCounter, 
    increment, 
    decrement
  }
}
```

**useTitle**

```js
import { ref, watch } from 'vue';

export default function(title = "默认的title") {
  const titleRef = ref(title);

  watch(titleRef, (newValue) => {
    document.title = newValue
  }, {
    immediate: true
  })

  return titleRef
}
```

**useLocalStorage**

```js
import { ref, watch } from 'vue';

export default function(key, value) {
  const data = ref(value);

  if (value) {
    window.localStorage.setItem(key, JSON.stringify(value));
  } else {
    data.value = JSON.parse(window.localStorage.getItem(key));
  }

  watch(data, (newValue) => {
    window.localStorage.setItem(key, JSON.stringify(newValue));
  })

  return data;
}

// 一个参数: 取值
// const data = useLocalStorage("name");

// // 二个参数: 保存值
// const data = useLocalStorage("name", "coderwhy");

// data.value = "kobe";

```

**useMousePosition**

```js
import { ref } from 'vue';

export default function() {
  const mouseX = ref(0);
  const mouseY = ref(0);

  window.addEventListener("mousemove", (event) => {
    mouseX.value = event.pageX;
    mouseY.value = event.pageY;
  });

  return {
    mouseX,
    mouseY
  }
}

```

**useScrollPosition**

```js
import { ref } from 'vue';

export default function() {
  const scrollX = ref(0);
  const scrollY = ref(0);

  document.addEventListener("scroll", () => {
    scrollX.value = window.scrollX;
    scrollY.value = window.scrollY;
  });

  return {
    scrollX,
    scrollY
  }
}
```

**index.js**

```js
import useCounter from './useCounter';
import useTitle from './useTitle';
import useScrollPosition from './useScrollPosition';
import useMousePosition from './useMousePosition';
import useLocalStorage from './useLocalStorage';

export {
  useCounter,
  useTitle,
  useScrollPosition,
  useMousePosition,
  useLocalStorage
}
```

```vue
<template>
  <div>
    <h2>当前计数: {{counter}}</h2>
    <h2>计数*2: {{doubleCounter}}</h2>
    <button @click="increment">+1</button>
    <button @click="decrement">-1</button>

    <h2>{{data}}</h2>
    <button @click="changeData">修改data</button>

    <p class="content"></p>

    <div class="scroll">
      <div class="scroll-x">scrollX: {{scrollX}}</div>
      <div class="scroll-y">scrollY: {{scrollY}}</div>
    </div>
    <div class="mouse">
      <div class="mouse-x">mouseX: {{mouseX}}</div>
      <div class="mouse-y">mouseY: {{mouseY}}</div>
    </div>
  </div>
</template>

<script>
  import { ref, computed } from 'vue';

  import {
    useCounter,
    useLocalStorage,
    useMousePosition,
    useScrollPosition,
    useTitle
  } from './hooks';

  export default {
    setup() {
      // counter
      const { counter, doubleCounter, increment, decrement } = useCounter();

      // title
      const titleRef = useTitle("coderwhy");
      setTimeout(() => {
        titleRef.value = "kobe"
      }, 3000);

      // 滚动位置
      const { scrollX, scrollY } = useScrollPosition();

      // 鼠标位置
      const { mouseX, mouseY } = useMousePosition();

      // localStorage
      const data = useLocalStorage("info");
      const changeData = () => data.value = "哈哈哈哈"

      return {
        counter,
        doubleCounter,
        increment,
        decrement,

        scrollX,
        scrollY,

        mouseX,
        mouseY,

        data,
        changeData
      }
    }
  }
</script>

<style scoped>
  .content {
    width: 3000px;
    height: 5000px;
  }

  .scroll {
    position: fixed;
    right: 30px;
    bottom: 30px;
  }
  .mouse {
    position: fixed;
    right: 30px;
    bottom: 80px;
  }
</style>
```

### setup顶层

```vue
<template>
  <div>
    <h2>Hello World</h2>
    <h2>{{message}}</h2>
    <button @click="emitEvent">发射事件</button>
  </div>
</template>

<script setup>
  import { defineProps, defineEmit } from 'vue';

  const props = defineProps({
    message: {
      type: String,
      default: "哈哈哈"
    }
  })

  const emit = defineEmit(["increment", "decrement"]);

  const emitEvent = () => {
    emit('increment', "100000")
  }
  
</script>

<style scoped>

</style>
```

### 认识h函数

Vue推荐在绝大数情况下使用模板来创建你的HTML，然后一些特殊的场景，你真的需要JavaScript的完全编程的能力，这个时候你可以使用渲染函数，它比模板更接近编译器；

前面我们讲解过VNode和VDOM的改变：

Vue在生成真实的DOM之前，会将我们的节点转换成VNode，而VNode组合在一起形成一颗树结构，就是虚拟DOM（VDOM）；

事实上，我们之前编写的template 中的HTML 最终也是使用渲染函数生成对应的VNode；

那么，如果你想充分的利用JavaScript的编程能力，我们可以自己来编写createVNode函数，生成对应的VNode；

那么我们应该怎么来做呢？使用h()函数：

h() 函数是一个用于创建vnode的一个函数；

其实更准备的命名是createVNode() 函数，但是为了简便在Vue将之简化为h() 函数；

![image-20220301151948834](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301151948834.png)

![image-20220301152148236](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301152148236.png)

### jsx的使用

![image-20220301152325384](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301152325384.png)

### 认识自定义指令

在Vue的模板语法中我们学习过各种各样的指令：v-show、v-for、v-model等等，除了使用这些指令之外，Vue也允许我们来自定义自己的指令。

注意：在Vue中，代码的复用和抽象主要还是通过组件；

通常在某些情况下，你需要对DOM元素进行底层操作，这个时候就会用到自定义指令；

自定义指令分为两种：

自定义局部指令：组件中通过directives 选项，只能在当前组件中使用；

自定义全局指令：app的directive 方法，可以在任意组件中被使用；

比如我们来做一个非常简单的案例：当某个元素挂载完成后可以自定获取焦点

实现方式一：如果我们使用默认的实现方式；

实现方式二：自定义一个v-focus 的局部指令；

这个自定义指令实现非常简单，我们只需要在组件选项中使用directives即可；

它是一个对象，在对象中编写我们自定义指令的名称（注意：这里不需要加v-）；

自定义指令有一个生命周期，是在组件挂载后调用的mounted，我们可以在其中完成操作；

```vue
<template>
  <div>
    <input type="text" v-focus>
  </div>
</template>

<script>
  export default {
    // 局部指令
    directives: {
      focus: {
        mounted(el, bindings, vnode, preVnode) {
          console.log("focus mounted");
          el.focus();
        }
      }
    }
  }
</script>

<style scoped>

</style>
```

实现方式三：自定义一个v-focus 的全局指令；

![image-20220301152620170](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301152620170.png)

### 指令的生命周期

一个指令定义的对象，Vue提供了如下的几个钩子函数：

created：在绑定元素的attribute 或事件监听器被应用之前调用；

beforeMount：当指令第一次绑定到元素并且在挂载父组件之前调用；

mounted：在绑定元素的父组件被挂载后调用；

beforeUpdate：在更新包含组件的VNode之前调用；

updated：在包含组件的VNode及其子组件的VNode更新后调用；

beforeUnmount：在卸载绑定元素的父组件之前调用；

unmounted：当指令与元素解除绑定且父组件已卸载时，只调用一次

### 指令的参数和修饰符

如果我们指令需要接受一些参数或者修饰符应该如何操作呢？

info是参数的名称；

aaa-bbb是修饰符的名称；

后面是传入的具体的值；

在我们的生命周期中，我们可以通过bindings 获取到对应的内容

```VUE
<template>
  <div>
    <button v-if="counter < 2" v-why.aaaa.bbbb="'coderwhy'" @click="increment">当前计数: {{counter}}</button>
  </div>
</template>

<script>
  import { ref } from "vue";

  export default {
    // 局部指令
    directives: {
      why: {
        created(el, bindings, vnode, preVnode) {
          console.log("why created", el, bindings, vnode, preVnode);
          console.log(bindings.value);
          console.log(bindings.modifiers);
        },
        beforeMount() {
          console.log("why beforeMount");
        },
        mounted() {
          console.log("why mounted");
        },
        beforeUpdate() {
          console.log("why beforeUpdate");
        },
        updated() {
          console.log("why updated");
        },
        beforeUnmount() {
          console.log("why beforeUnmount");
        },
        unmounted() {
          console.log("why unmounted");
        }
      }
    },
    setup() {
      const counter = ref(0);
      const increment = () => counter.value++;

      return {
        counter,
        increment
      }
    }
  }
</script>

<style scoped>

</style>
```

### 自定义时间格式化指令

```js
import dayjs from 'dayjs';

export default function(app) {
  app.directive("format-time", {
    created(el, bindings) {
      bindings.formatString = "YYYY-MM-DD HH:mm:ss";
      if (bindings.value) {
        bindings.formatString = bindings.value;
      }
    },
    mounted(el, bindings) {
      const textContent = el.textContent;
      let timestamp = parseInt(textContent);
      if (textContent.length === 10) {
        timestamp = timestamp * 1000
      }
      el.textContent = dayjs(timestamp).format(bindings.formatString);
    }
  })
}
```

### 认识Teleport

在组件化开发中，我们封装一个组件A，在另外一个组件B中使用：

那么组件A中template的元素，会被挂载到组件B中template的某个位置；

最终我们的应用程序会形成一颗DOM树结构；

但是某些情况下，我们希望组件不是挂载在这个组件树上的，可能是移动到Vue app之外的其他位置：

比如移动到body元素上，或者我们有其他的div#app之外的元素上；

这个时候我们就可以通过teleport来完成；

Teleport是什么呢？

它是一个Vue提供的内置组件，类似于react的Portals；

teleport翻译过来是心灵传输、远距离运输的意思；

它有两个属性：

to：指定将其中的内容移动到的目标元素，可以使用选择器；

disabled：是否禁用teleport 的功能；

```vue
<template>
  <div class="app">
    <teleport to="#why">
      <h2>当前计数</h2>
      <button>+1</button>
      <hello-world></hello-world>
    </teleport>

    <teleport to="#why">
      <span>呵呵呵呵</span>
    </teleport>
  </div>
</template>

<script>
  import { getCurrentInstance } from "vue";

  import HelloWorld from './HelloWorld.vue';

  export default {
    components: {
      HelloWorld
    },
    setup() {
      const instance = getCurrentInstance();
      console.log(instance.appContext.config.globalProperties.$name);
    },
    mounted() {
      console.log(this.$name);
    },
    methods: {
      foo() {
        console.log(this.$name);
      }
    }
  }
</script>

<style scoped>

</style>
```

### 认识Vue插件

通常我们向Vue全局添加一些功能时，会采用插件的模式，它有两种编写方式：

对象类型：一个对象，但是必须包含一个install 的函数，该函数会在安装插件时执行；

函数类型：一个function，这个函数会在安装插件时自动执行；

插件可以完成的功能没有限制，比如下面的几种都是可以的：

添加全局方法或者property，通过把它们添加到config.globalProperties上实现；

添加全局资源：指令/过滤器/过渡等；

通过全局mixin来添加一些组件选项；

一个库，提供自己的API，同时提供上面提到的一个或多个功能

![image-20220301154135458](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301154135458.png)

## vue的源码学习

### 虚拟DOM的优势

目前框架都会引入虚拟DOM来对真实的DOM进行抽象，这样做有很多的好处：

首先是可以对真实的元素节点进行抽象，抽象成VNode（虚拟节点），这样方便后续对其进行各种操作：

因为对于直接操作DOM来说是有很多的限制的，比如diff、clone等等，但是使用JavaScript编程语言来操作这些，就变得非常的简单；

我们可以使用JavaScript来表达非常多的逻辑，而对于DOM本身来说是非常不方便的；

其次是方便实现跨平台，包括你可以将VNode节点渲染成任意你想要的节点p如渲染在canvas、WebGL、SSR、Native（iOS、Android）上；

并且Vue允许你开发属于自己的渲染器（renderer），在其他的平台上渲染；

### 三大核心系统

事实上Vue的源码包含三大核心：

Compiler模块：编译模板系统；

Runtime模块：也可以称之为Renderer模块，真正渲染的模块；

Reactivity模块：响应式系统



### 一道面试题

组件的vnode和组件的instance有什么区别？

组件的vnode是虚拟dom，组件的instance保存各种状态，比如data，methods，setup，computed等等



### block Tree

1.对于不会改变的静态节点，进行作用域的提升（hoisted)

2.将可能发生改变的节点放到dynamicChildren数组上，为了之后进行diff算法



### 数据的查找顺序

![image-20220315111212619](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220315111212619.png)

setup -> data -> props -> ctx(methods/computed)



## vue-router

认识前端路由

路由的概念在软件工程中出现，最早是在后端路由中实现的，原因是web的发展主要经历了这样一些阶段：

后端路由阶段；

前后端分离阶段；

单页面富应用（SPA）;

### 后端路由阶段

![image-20220301155020395](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301155020395.png)

### 前后端分离阶段

![image-20220301155101335](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301155101335.png)

### URL的hash

前端路由是如何做到URL和内容进行映射呢？监听URL的改变。

URL的hashpURL的hash也就是锚点(#), 本质上是改变window.location的href属性；

我们可以通过直接赋值location.hash来改变href, 但是页面不发生刷新；

hash的优势就是兼容性更好，在老版IE中都可以运行，但是缺陷是有一个#，显得不像一个真实的路径。

### HTML5的History

history接口是HTML5新增的, 它有六种模式改变URL而不刷新页面：

replaceState：替换原来的路径；

pushState：使用新的路径；

popState：路径的回退；

go：向前或向后改变路径；

forward：向前改变路径；

back：向后改变路径；

### 路由的使用步骤

使用vue-router的步骤:

第一步：创建路由组件的组件；

第二步：配置路由映射:组件和路径映射关系的routes数组；

第三步：通过createRouter创建路由对象，并且传入routes和history模式；

第四步：使用路由:通过<router-link>和<router-view>；

![image-20220301155811239](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301155811239.png)

![image-20220301155858492](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301155858492.png)

### 路由懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载：

如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就会更加高效；

也可以提高首屏的渲染效率；

其实这里还是我们前面讲到过的webpack的分包知识，而Vue Router默认就支持动态来导入组件：

这是因为component可以传入一个组件，也可以接收一个函数，该函数需要放回一个Promise；

而import函数就是返回一个Promise；

### 动态路由

![image-20220301160057987](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301160057987.png)

![image-20220301160129246](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301160129246.png)

### NotFound

![image-20220301161846368](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301161846368.png)

### 路由的嵌套

![image-20220301162036515](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301162036515.png)

### 代码的页面跳转

![image-20220301162137358](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301162137358.png)

### query方式的参数

![image-20220301162207606](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301162207606.png)

![image-20220301162224210](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301162224210.png)

![image-20220301162326824](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301162326824.png)

### router-link的v-slot

![image-20220301163424748](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301163424748.png)

### router-view的v-slot

![image-20220301163444634](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301163444634.png)

### 动态添加路由

![image-20220301163554188](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301163554188.png)

### 动态删除路由

![image-20220301163733246](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301163733246.png)

### 路由导航守卫

![image-20220301163923459](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301163923459.png)

![image-20220301163955735](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301163955735.png)

**index.js**

```js
import { createRouter, createWebHistory, createWebHashHistory } from 'vue-router'

// import Home from "../pages/Home.vue";
// import About from "../pages/About.vue";

// 配置映射关系
const routes = [
  { 
    path: "/", 
    redirect: "/home" 
  },
  // /home/shops
  { 
    path: "/home", 
    name: "home",
    component: () => import(/* webpackChunkName: "home-chunk" */"../pages/Home.vue"),
    meta: {
      name: "why",
      age: 18,
      height: 1.88
    },
    children: [
      {
        path: "",
        redirect: "/home/message"
      },
      {
        path: "message",
        component: () => import("../pages/HomeMessage.vue")
      },
      {
        path: "shops",
        component: () => import("../pages/HomeShops.vue")
      }
    ]
  },
  { 
    path: "/about",
    name: "about",
    component: () => import("../pages/About.vue") 
  },
  { 
    path: "/user/:username/id/:id",
    component: () => import("../pages/User.vue") 
  },
  {
    path: "/login",
    component: () => import("../pages/Login.vue")
  },
  {
    path: "/:pathMatch(.*)",
    component: () => import("../pages/NotFound.vue")
  }
];

// 创建一个路由对象router
const router = createRouter({
  routes,
  history: createWebHistory()
})

// 动态添加路由
const categoryRoute = {
  path: "/category",
  component: () => import("../pages/Category.vue")
}

// 添加顶级路由对象
router.addRoute(categoryRoute);

// 添加二级路由对象
router.addRoute("home", {
  path: "moment",
  component: () => import("../pages/HomeMoment.vue")
})


// 导航守卫beforeEach
let counter = 0;
// to: Route对象, 即将跳转到的Route对象
// from: Route对象, 
/**
 * 返回值问题:
 *    1.false: 不进行导航
 *    2.undefined或者不写返回值: 进行默认导航
 *    3.字符串: 路径, 跳转到对应的路径中
 *    4.对象: 类似于 router.push({path: "/login", query: ....})
 */
router.beforeEach((to, from) => {
  console.log(`进行了${++counter}路由跳转`)
  // if (to.path.indexOf("/home") !== -1) {
  //   return "/login"
  // }
  if (to.path !== "/login") {
    const token = window.localStorage.getItem("token");
    if (!token) {
      return "/login"
    }
  }
})


export default router
```

```vue
<template>
  <div id="app">
    <!-- props: href 跳转的链接 -->
    <!-- props: route对象 -->
    <!-- props: navigate导航函数 -->
    <!-- props: isActive 是否当前处于活跃的状态 -->
    <!-- props: isExactActive 是否当前处于精确的活跃状态 -->
    <router-link to="/home" v-slot="props" custom>
      <button @click="props.navigate">{{props.href}}</button>
      <button @click="props.navigate">哈哈哈</button>
      <span :class="{'active': props.isActive}">{{props.isActive}}</span>
      <span :class="{'active': props.isActive}">{{props.isExactActive}}</span>
      <!-- <p>{{props.route}}</p> -->
    </router-link>
    <router-link to="/about">关于</router-link>
    <router-link to="/user/kobe/id/111">用户</router-link>
    <router-link to="/category">分类</router-link>

    <button @click="jumpToAbout">关于</button>
    <button @click="forwardOneStep">前进一步</button>

    <router-view v-slot="props">
      <!-- <transition name="why"> -->
        <keep-alive>
          <component :is="props.Component"></component>
        </keep-alive>
      <!-- </transition> -->
    </router-view>
  </div>
</template>

<script>

import { useRouter } from 'vue-router'
import NavBar from './components/NavBar.vue'

export default {
  name: 'App',
  components: {
    NavBar
  },
  methods: {
    // jumpToAbout() {
    //   // router
    //   this.$router.push("/about")
    // }
  },
  setup() {
    const router = useRouter();

    const jumpToAbout = () => {
      // router.push("/about")
      // router.push({
      //   path: "/about",
      //   query: {
      //     name: "why",
      //     age: 18
      //   }
      // })
      // router.replace("/about")
    }

    const forwardOneStep = () => {
      router.go(1)
      // router.go(-1)
      // router.forward()
      // router.back()
    }

    return {
      jumpToAbout,
      forwardOneStep
    }
  }
}
</script>

<style>
  .why-active {
    color: red;
  }

  .why-enter-from,
  .why-leave-to {
    opacity: 0;
  }

  .why-enter-active,
  .why-leave-active {
    transition: opacity 1s ease;
  }
</style>
```

## vuex

### 什么是状态管理

在开发中，我们的应用程序需要处理各种各样的数据，这些数据需要保存在我们应用程序中的某一个位置，对于这些数据的管理我们就称之为是状态管理。

在前面我们是如何管理自己的状态呢？

在Vue开发中，我们使用组件化的开发方式；

而在组件中我们定义data或者在setup中返回使用的数据，这些数据我们称之为state；

在模块template中我们可以使用这些数据，模块最终会被渲染成DOM，我们称之为View；

在模块中我们会产生一些行为事件，处理这些行为事件时，有可能会修改state，这些行为事件我们称之为actions

### 复杂的状态管理

JavaScript开发的应用程序，已经变得越来越复杂了：

JavaScript需要管理的状态越来越多，越来越复杂；

这些状态包括服务器返回的数据、缓存数据、用户操作产生的数据等等；

也包括一些UI的状态，比如某些元素是否被选中，是否显示加载动效，当前分页；

当我们的应用遇到多个组件共享状态时，单向数据流的简洁性很容易被破坏：

多个视图依赖于同一状态；

来自不同视图的行为需要变更同一状态；

我们是否可以通过组件数据的传递来完成呢？

对于一些简单的状态，确实可以通过props的传递或者Provide的方式来共享状态；

但是对于复杂的状态管理来说，显然单纯通过传递和共享的方式是不足以解决问题的，比如兄弟组件如何共享数据呢？

### 创建Store

![image-20220301164751119](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301164751119.png)

```js
import { createStore } from "vuex"
import home from './modules/home'
import user from './modules/user'

const store = createStore({
  state() {
    return {
      rootCounter: 100
    }
  },
  getters: {
    doubleRootCounter(state) {
      return state.rootCounter * 2
    }
  },
  mutations: {
    increment(state) {
      state.rootCounter++
    }
  },
  modules: {
    home,
    user
  }
});

export default store;

```

### 单一状态树

Vuex使用单一状态树：

用一个对象就包含了全部的应用层级状；

采用的是SSOT，SingleSourceof Truth，也可以翻译成单一数据源；

这也意味着，每个应用将仅仅包含一个store实例；

单状态树和模块化并不冲突，后面我们会讲到module的概念；

单一状态树的优势：

如果你的状态信息是保存到多个Store对象中的，那么之后的管理和维护等等都会变得特别困难；

所以Vuex也使用了单一状态树来管理应用层级的全部状态；

单一状态树能够让我们最直接的方式找到某个状态的片段，而且在之后的维护和调试过程中，也可以非常方便的管理和维护；

### 组件获取状态

![image-20220301165709340](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301165709340.png)

```vue
<template>
  <div>
    <h2>Home:{{ $store.state.counter }}</h2>
    <h2>Home:{{ sCounter }}</h2>
    <h2>Home:{{ sName }}</h2>
    <!-- <h2>Home:{{ age }}</h2>
    <h2>Home:{{ height }}</h2> -->
  </div>
</template>

<script>
  import { mapState } from 'vuex'

  export default {
    computed: {
      fullName() {
        return "Kobe Bryant"
      },
      // 其他的计算属性, 从state获取
      // ...mapState(["counter", "name", "age", "height"])
      ...mapState({
        sCounter: state => state.counter,
        sName: state => state.name
      })
    }
  }
</script>

<style scoped>

</style>
```

### 在setup中使用mapState

在setup中如果我们单个获取装是非常简单的：

通过useStore拿到store后去获取某个状态即可；

但是如果我们需要使用mapState的功能呢？

默认情况下，Vuex并没有提供非常方便的使用mapState的方式，这里我们进行了一个函数的封装

```vue
<template>
  <div>
    <h2>Home:{{ $store.state.counter }}</h2>
    <hr>
    <h2>{{sCounter}}</h2>
    <h2>{{counter}}</h2>
    <h2>{{name}}</h2>
    <h2>{{age}}</h2>
    <h2>{{height}}</h2>
    <hr>
  </div>
</template>

<script>
  import { mapState, useStore } from 'vuex'
  import { computed } from 'vue'

  export default {
    computed: {
      fullName: function() {
        return "1fdasfdasfad"
      },
      ...mapState(["name", "age"])
    },

    setup() {
      const store = useStore()
      const sCounter = computed(() => store.state.counter)
      // const sName = computed(() => store.state.name)
      // const sAge = computed(() => store.state.age)

      const storeStateFns = mapState(["counter", "name", "age", "height"])

      // {name: function, age: function, height: function}
      // {name: ref, age: ref, height: ref}
      const storeState = {}
      Object.keys(storeStateFns).forEach(fnKey => {
        const fn = storeStateFns[fnKey].bind({$store: store})
        storeState[fnKey] = computed(fn)
      })

      return {
        sCounter,
        ...storeState
      }
    }
  }
</script>

<style scoped>

</style>
```

### useState的封装

```js
import { computed } from 'vue'
import { mapState, useStore } from 'vuex'

export function useState(mapper) {
  // 拿到store独享
  const store = useStore()

  // 获取到对应的对象的functions: {name: function, age: function}
  const storeStateFns = mapState(mapper)

  // 对数据进行转换
  const storeState = {}
  Object.keys(storeStateFns).forEach(fnKey => {
    const fn = storeStateFns[fnKey].bind({$store: store})
    storeState[fnKey] = computed(fn)
  })

  return storeState
}
```

```vue
<template>
  <div>
    <h2>Home:{{ $store.state.counter }}</h2>
    <hr>
    <h2>{{counter}}</h2>
    <h2>{{name}}</h2>
    <h2>{{age}}</h2>
    <h2>{{height}}</h2>
    <h2>{{sCounter}}</h2>
    <h2>{{sName}}</h2>
    <hr>
  </div>
</template>

<script>
  import { useState } from '../hooks/useState'

  export default {
    setup() {
      const storeState = useState(["counter", "name", "age", "height"])
      const storeState2 = useState({
        sCounter: state => state.counter,
        sName: state => state.name
      })

      return {
        ...storeState,
        ...storeState2
      }
    }
  }
</script>

<style scoped>

</style>
```

### getters的基本使用

![image-20220301170300106](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301170300106.png)

### getters第二个参数

![image-20220301170422049](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301170422049.png)

### getters的返回函数

![image-20220301170644309](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301170644309.png)

### mapGetters的辅助函数

```vue
<template>
  <div>
    <h2>总价值: {{ $store.getters.totalPrice }}</h2>
    <h2>总价值: {{ $store.getters.totalPriceCountGreaterN(1) }}</h2>
    <hr>
    <h2>{{ sNameInfo }}</h2>
    <h2>{{ sAgeInfo }}</h2>
    <!-- <h2>{{ ageInfo }}</h2>
    <h2>{{ heightInfo }}</h2> -->
    <hr>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'

  export default {
    computed: {
      ...mapGetters(["nameInfo", "ageInfo", "heightInfo"]),
      ...mapGetters({
        sNameInfo: "nameInfo",
        sAgeInfo: "ageInfo"
      })
    },
    setup() {
      
    }
  }
</script>

<style scoped>

</style>
```

### useGetters的封装

```js
import { computed } from 'vue'
import { mapGetters, useStore } from 'vuex'

export function useGetters(mapper) {
  // 拿到store独享
  const store = useStore()

  // 获取到对应的对象的functions: {name: function, age: function}
  const storeStateFns = mapGetters(mapper)

  // 对数据进行转换
  const storeState = {}
  Object.keys(storeStateFns).forEach(fnKey => {
    const fn = storeStateFns[fnKey].bind({$store: store})
    storeState[fnKey] = computed(fn)
  })

  return storeState
}
```

```vue
<template>
  <div>
    <h2>总价值: {{ $store.getters.totalPrice }}</h2>
    <h2>总价值: {{ $store.getters.totalPriceCountGreaterN(1) }}</h2>
    <hr>
    <h2>{{ nameInfo }}</h2>
    <h2>{{ ageInfo }}</h2>
    <h2>{{ heightInfo }}</h2>
    <hr>
  </div>
</template>

<script>
  import { useGetters } from '../hooks/useGetters'

  export default {
    computed: {

    },
    setup() {
      const storeGetters = useGetters(["nameInfo", "ageInfo", "heightInfo"])
      return {
        ...storeGetters
      }
    }
  }
</script>

<style scoped>

</style>
```

### Mutation基本使用

![image-20220301180620740](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301180620740.png)

### Mutation携带数据

![image-20220301180746679](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301180746679.png)

### Mutation常量类型

![image-20220301180911103](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301180911103.png)

### mapMutations辅助函数

![image-20220301181210702](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301181210702.png)

```vue
<template>
  <div>
    <h2>当前计数: {{ $store.state.counter }}</h2>
    <hr>
      <button @click="increment">+1</button>
      <button @click="add">+1</button>
      <button @click="decrement">-1</button>
      <button @click="increment_n({n: 10})">+10</button>
    <hr>
  </div>
</template>

<script>
  import { mapMutations, mapState } from 'vuex'

  import { INCREMENT_N } from '../store/mutation-types'

  export default {
    methods: {
      ...mapMutations(["increment", "decrement", INCREMENT_N]),
      ...mapMutations({
        add: "increment"
      })
    },
    setup() {
      const storeMutations = mapMutations(["increment", "decrement", INCREMENT_N])

      return {
        ...storeMutations
      }
    }
  }
</script>

<style scoped>

</style>
```

### mutation重要原则

一条重要的原则就是要记住mutation 必须是同步函数

这是因为devtool工具会记录mutation的日记；

每一条mutation被记录，devtools都需要捕捉到前一状态和后一状态的快照；

但是在mutation中执行异步操作，就无法追踪到数据的变化；

所以Vuex的重要原则中要求mutation必须是同步函数；

### actions的基本使用

Action类似于mutation，不同在于：

Action提交的是mutation，而不是直接变更状态；

Action可以包含任意异步操作；

这里有一个非常重要的参数context：

context是一个和store实例均有相同方法和属性的context对象；

所以我们可以从其中获取到commit方法来提交一个mutation，或者通过context.state和context.getters来获取state和getters；

但是为什么它不是store对象呢？这个等到我们讲Modules时再具体来说；

![image-20220301181349763](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301181349763.png)

### actions的分发操作

![image-20220301182345529](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301182345529.png)

```VUE
<template>
  <div>
    <h2>当前计数: {{ $store.state.counter }}</h2>
    <hr>
      <button @click="increment">+1</button>
      <button @click="decrement">-1</button>
    <hr>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    methods: {
      increment() {
        this.$store.dispatch("incrementAction", {count: 100})
      },
      decrement() {
        // 3.派发风格(对象类型)
        this.$store.dispatch({
          type: "decrementAction"
        })
      }
    },
    mounted() {
      this.$store.dispatch("getHomeMultidata")
    },
    setup() {
    }
  }
</script>

<style scoped>

</style>
```

### actions的辅助函数

```vue
<template>
  <div>
    <h2>当前计数: {{ $store.state.counter }}</h2>
    <hr>
      <button @click="incrementAction">+1</button>
      <button @click="decrementAction">-1</button>
      <button @click="add">+1</button>
      <button @click="sub">-1</button>
    <hr>
  </div>
</template>

<script>
  import { mapActions } from 'vuex'

  export default {
    methods: {
      // ...mapActions(["incrementAction", "decrementAction"]),
      // ...mapActions({
      //   add: "incrementAction",
      //   sub: "decrementAction"
      // })
    },
    setup() {
      const actions = mapActions(["incrementAction", "decrementAction"])
      const actions2 = mapActions({
        add: "incrementAction",
        sub: "decrementAction"
      })

      return {
        ...actions,
        ...actions2
      }
    }
  }
</script>

<style scoped>

</style>
```

### actions的异步操作

Action通常是异步的，那么如何知道action什么时候结束呢？

我们可以通过让action返回Promise，在Promise的then中来处理完成后的操作；

![image-20220301182742708](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301182742708.png)

```vue
<template>
  <div>
    <h2>当前计数: {{ $store.state.counter }}</h2>
    <hr>
      <button @click="incrementAction">+1</button>
      <button @click="decrementAction">-1</button>
      <button @click="add">+1</button>
      <button @click="sub">-1</button>
    <hr>
  </div>
</template>

<script>
  import { onMounted } from "vue";
  import { useStore } from 'vuex'

  export default {
    setup() {
      const store = useStore()

      onMounted(() => {
        const promise = store.dispatch("getHomeMultidata")
        promise.then(res => {
          console.log(res)
        }).catch(err => {
          console.log(err)
        })
      })
    }
  }
</script>

<style scoped>

</style>
```

### module的基本使用

什么是Module？

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象，当应用变得非常复杂时，store对象就有可能变得相当臃肿；

为了解决以上问题，Vuex允许我们将store分割成模块（module）；

每个模块拥有自己的state、mutation、action、getter、甚至是嵌套子模块；

![image-20220301182947039](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301182947039.png)

![image-20220301183810767](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301183810767.png)

```js
const homeModule = {
  namespaced: true,
  state() {
    return {
      homeCounter: 100
    }
  },
  getters: {
    doubleHomeCounter(state, getters, rootState, rootGetters) {
      return state.homeCounter * 2
    },
    otherGetter(state) {
      return 100
    }
  },
  mutations: {
    increment(state) {
      state.homeCounter++
    }
  },
  actions: {
    incrementAction({commit, dispatch, state, rootState, getters, rootGetters}) {
      commit("increment")
      commit("increment", null, {root: true})

      // dispatch
      // dispatch("incrementAction", null, {root: true})
    }
  }
}

export default homeModule

```

```js
const userModule = {
  namespaced: true,
  state() {
    return {
      userCounter: 10
    }
  },
  getters: {

  },
  mutations: {

  },
  actions: {

  }
}

export default userModule

```

```js
import { createStore } from "vuex"
import home from './modules/home'
import user from './modules/user'

const store = createStore({
  state() {
    return {
      rootCounter: 100
    }
  },
  getters: {
    doubleRootCounter(state) {
      return state.rootCounter * 2
    }
  },
  mutations: {
    increment(state) {
      state.rootCounter++
    }
  },
  modules: {
    home,
    user
  }
});

export default store;

```

### module的局部状态

![image-20220301183051410](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301183051410.png)

### module的命名空间

默认情况下，模块内部的action和mutation仍然是注册在全局的命名空间中的：

这样使得多个模块能够对同一个action或mutation作出响应；

Getter同样也默认注册在全局命名空间；

如果我们希望模块具有更高的封装度和复用性，可以添加namespaced: true的方式使其成为带命名空间的模块：

当模块被注册后，它的所有getter、action及mutation都会自动根据模块注册的路径调整命名;

![image-20220301183202942](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301183202942.png)

### module修改或派发根组件

![image-20220301183537618](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301183537618.png)

### module的辅助函数

![image-20220301183642232](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301183642232.png)

![image-20220301183655626](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301183655626.png)

### 对useState和useGetters修改

**useMappers**

```js
import { computed } from 'vue'
import { useStore } from 'vuex'

export function useMapper(mapper, mapFn) {
  // 拿到store独享
  const store = useStore()

  // 获取到对应的对象的functions: {name: function, age: function}
  const storeStateFns = mapFn(mapper)

  // 对数据进行转换
  const storeState = {}
  Object.keys(storeStateFns).forEach(fnKey => {
    const fn = storeStateFns[fnKey].bind({$store: store})
    storeState[fnKey] = computed(fn)
  })

  return storeState
}
```

**useGetters**

```js
import { mapGetters, createNamespacedHelpers } from 'vuex'
import { useMapper } from './useMapper'

export function useGetters(moduleName, mapper) {
  let mapperFn = mapGetters
  if (typeof moduleName === 'string' && moduleName.length > 0) {
    mapperFn = createNamespacedHelpers(moduleName).mapGetters
  } else {
    mapper = moduleName
  }

  return useMapper(mapper, mapperFn)
}
```

**useStates**

```js
import { mapState, createNamespacedHelpers } from 'vuex'
import { useMapper } from './useMapper'

export function useState(moduleName, mapper) {
  let mapperFn = mapState
  if (typeof moduleName === 'string' && moduleName.length > 0) {
    mapperFn = createNamespacedHelpers(moduleName).mapState
  } else {
    mapper = moduleName
  }

  return useMapper(mapper, mapperFn)
}
```

**index.js**

```js
import { useGetters } from './useGetters';
import { useState } from './useState';

export {
  useGetters,
  useState
}
```

### nexttick

官方解释：将回调推迟到下一个DOM更新周期之后执行。在更改了一些数据以等待DOM更新后立即使用它。

比如我们有下面的需求：

点击一个按钮，我们会修改在h2中显示的message；

message被修改后，获取h2的高度；

实现上面的案例我们有三种方式：

方式一：在点击按钮后立即获取到h2的高度（错误的做法）

方式二：在updated生命周期函数中获取h2的高度（但是其他数据更新，也会执行该操作）

方式三：使用nexttick函数；nnexttick是如何做到的呢？

```vue
<template>
  <div>
    <h2>{{counter}}</h2>
    <button @click="increment">+1</button>
    <h2 class="title" ref="titleRef">{{message}}</h2>
    <button @click="addMessageContent">添加内容</button>
  </div>
</template>

<script>
  import { ref, onUpdated, nextTick } from "vue";

  export default {
    setup() {
      const message = ref("")
      const titleRef = ref(null)

      const counter = ref(0)

      const addMessageContent = () => {
        message.value += "哈哈哈哈哈哈哈哈哈哈"

        // 更新DOM
        nextTick(() => {
          console.log(titleRef.value.offsetHeight)
        })
      }

      const increment = () => {
        for (let i = 0; i < 100; i++) {
          counter.value++
        }
      }

      onUpdated(() => {
      })

      return {
        message,
        counter,
        increment,
        titleRef,
        addMessageContent
      }
    }
  }
</script>

<style scoped>
  .title {
    width: 120px;
  }
</style>
```

