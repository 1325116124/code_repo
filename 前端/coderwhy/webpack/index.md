# Webpack

## 邂逅Webpack

前端开发目前我们面临哪些复杂的问题呢？

- 比如开发过程中我们需要通过模块化的方式来开发；

- 比如也会使用一些高级的特性来加快我们的开发效率或者安全性，比如通过ES6+、TypeScript开发脚本逻辑，通过sass、less等方式来编写css样式代码；

- 比如开发过程中，我们还希望试试的监听文件的变化来并且反映到浏览器上，提高开发的效率；

- 比如开发完成后我们还需要将代码进行压缩、合并以及其他相关的优化



**webpack是什么**

webpack是一个静态的模块化打包工具，为现代的JavaScript应用程序；

我们来对上面的解释进行拆解：

- 打包bundler：webpack可以将帮助我们进行打包，所以它是一个打包工具
- 静态的static：这样表述的原因是我们最终可以将代码打包成最终的静态资源（部署到静态服务器）；
- 模块化module：webpack默认支持各种模块化开发，ES Module、CommonJS、AMD等；
- 现代的modern：我们前端说过，正是因为现代前端开发面临各种各样的问题，才催生了webpack的出现和发展；

![image-20220408115907725](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220408115907725.png)



**webpack在日常工作的使用**

日常工作来说，比如在开发vue、react、angular等项目的过程中我们需要一些特殊的配置：比如给某些目录结构起别名，让我们的项目支持sass、less等预处理器，希望在项目中手动的添加TypeScript的支持，都需要对webpack进行一些特殊的配置工作。

当然，除了日常工作之外，如果我们希望将在原有的脚手架上来定制一些自己的特殊配置提供性能：比如安装性能分析工具、使用gzip压缩代码、引用cdn的资源，公共代码抽取等等操作，甚至包括需要编写属于自己的loader和plugin。

对于想要在前端领域进阶成为高级前端开发工程师，甚至是架构师的前端开发者来说，webpack等构建工具是必须学习的，包括其中的一些高级特性和原理，都是要熟练掌握的。企业在招聘高级前端工程师或者架构师时，必然会对webpack和其他的构建工具有比较高的要求。



**webpack和vite的思考**

vite的核心思想并不是首创

- 事实上，vite的很多思想和之前的snowpack是重合的，而且相对目前来说snowpack会更加成熟；

- 当然，后续发展来看vite可能会超越snowpack；

webpack的更新迭代

- webpack在发展的过程中，也会不断改进自己，借鉴其他工具的一些优势和思想；

- 在这么多年的发展中，无论是自身的优势还是生态都是非常强大的



**webpack官方文档**

webpack的官方文档是https://webpack.js.org/

webpack的中文官方文档是https://webpack.docschina.org/

DOCUMENTATION：文档详情，也是我们最关注的

点击DOCUMENTATION来到文档页：

API：API，提供相关的接口，可以自定义编译的过程（比如自定义loader和Plugin可以参考该位置的API）

BLOG：博客，等同于上一个tab的BLOG，里面有一些博客文章；

CONCEPTS：概念，主要是介绍一些webpack的核心概念，比如入口、出口、Loaders、Plugins等等，但是这里并没有一些对它们解析的详细API；

CONFIGURATION：配置，webpack详细的配置选项，都可以在这里查询到，更多的时候是作为查询手册；

GUIDES：指南，更像是webpack提供给我们的教程，我们可以按照这个教程一步步去学习webpack的使用过程；

LOADERS：loaders，webpack的核心之一，常见的loader都可以在这里查询到用法，比如css-loader、babel-loader、less-loader等等；

PLUGINS：plugins，webpack的核心之一，常见的plugin都可以在这里查询到用法，比如BannerPlugin、CleanWebpackPlugin、MiniCssExtractPlugin等等；

MIGRATE：迁移，可以通过这里的教程将webpack4迁移到webpack5等；



**webpack是依赖node环境的**

**webpack的安装**

webpack的安装目前分为两个：webpack、webpack-cli

那么它们是什么关系呢？

执行webpack命令，会执行node_modules下的.bin目录下的webpack；

webpack在执行时是依赖webpack-cli的，如果没有安装就会报错；

而webpack-cli中代码执行时，才是真正利用webpack进行编译和打包的过程；

所以在安装webpack时，我们需要同时安装webpack-cli（第三方的脚手架事实上是没有使用webpack-cli的，而是类似于自己的vue-service-cli的东西）

pm install webpack webpack-cli –g#全局安装

pm install webpack webpack-cli –D#局部安装



**传统开发的问题**

我们的代码存在什么问题呢？某些语法浏览器是不认识的（尤其在低版本浏览器上）

1.使用了ES6的语法，比如const、箭头函数等语法；

2.使用了ES6中的模块化语法；

3.使用CommonJS的模块化语法；

4.在通过script标签引入时，必须添加上type="module"属性；

显然，上面存在的问题，让我们在发布静态资源时，是不能直接发布的，因为运行在用户浏览器必然会存在各种各样的兼容性问题。

我们需要通过某个工具对其进行打包，让其转换成浏览器可以直接识别的语法



**webpack的默认打包**

本来单纯的通过script引入的js文件是没有模块化的，所以对于import和export等等是无法识别的，必须加上type=module这个属性，然后浏览器也默认不认识commonJS的模块化，所以要使用webpack打包之后导入才能使用

![image-20220408120556230](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220408120556230.png)

我们可以通过webpack进行打包，之后运行打包之后的代码p在目录下直接执行webpack 命令

生成一个dist文件夹，里面存放一个main.js的文件，就是我们打包之后的文件：

这个文件中的代码被压缩和丑化了；

我们暂时不关心他是如何做到的，后续我讲webpack实现模块化原理时会再次讲到；

另外我们发现代码中依然存在ES6的语法，比如箭头函数、const等，这是因为默认情况下webpack并不清楚我们打包后的文件是否需要转成ES5之前的语法，后续我们需要通过babel来进行转换和设置；

我们发现是可以正常进行打包的，但是有一个问题，webpack是如何确定我们的入口的呢？

事实上，当我们运行webpack时，webpack会查找当前目录下的src/index.js作为入口；

所以，如果当前项目中没有存在src/index.js文件，那么会报错



**注意点**

如果全局安装了webpack，那么敲webpack命令运行的是全局的webpack，如果想使用项目内安装的webpack，那么有两个方法，一个是通过npx webpack，默认会去项目中的node_modules下的bin找到webpack，另外一个方法是在package.json中配置script，在script中直接使用webpack命令默认优先去node_modules中去查找



## Webpack核心配置选项

npx webpack --entry ./src/mian.js --output-path ./build

当然，我们也可以通过配置来指定入口和出口 npx webpack --entry ./src/main.js --output-path ./build

### webpack配置文件

我们可以在根目录下创建一个webpack.config.js文件，来作为webpack的配置文件

![image-20220409200032423](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409200032423.png)

但是如果我们的配置文件并不是webpack.config.js的名字，而是其他的名字呢？

比如我们将webpack.config.js修改成了wk.config.js；

这个时候我们可以通过--config 来指定对应的配置文件；

但是每次这样执行命令来对源码进行编译，会非常繁琐，所以我们可以在package.json中增加一个新的脚本：

webpack --config wk.config.js

![image-20220409200606615](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409200606615.png)

### webpack依赖图

webpack到底是如何对我们的项目进行打包的呢？

事实上webpack在处理应用程序时，它会根据命令或者配置文件找到入口文件；

从入口开始，会生成一个依赖关系图，这个依赖关系图会包含应用程序中所需的所有模块（比如.js文件、css文件、图片、字体等）；

然后遍历图结构，打包一个个模块（根据文件的不同使用不同的loader来解析）

### 编写案例

我们创建一个component.js，通过JavaScript创建了一个元素，并且希望给它设置一些样式

![image-20220409202607144](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409202607144.png)

#### css-loader的使用

上面的错误信息告诉我们需要一个loader来加载这个css文件，但是loader是什么呢？

loader 可以用于对模块的源代码进行转换；

我们可以将css文件也看成是一个模块，我们是通过import来加载这个模块的；

在加载这个模块时，webpack其实并不知道如何对其进行加载，我们必须制定对应的loader来完成这个功能；

那么我们需要一个什么样的loader呢？

对于加载css文件来说，我们需要一个可以读取css文件的loader；

这个loader最常用的是css-loader；

css-loader的安装：npm install css-loader -D

#### css-loader的使用方案

如何使用这个loader来加载css文件呢？有三种方式：

- 内联方式：在引入的样式前加上使用的loader，并且使用!分割；

  ![image-20220409202954907](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409202954907.png)

- CLI方式（webpack5中不再使用）；

- 配置方式：

配置方式表示的意思是在我们的webpack.config.js文件中写明配置信息：

module.rules中允许我们配置多个loader（因为我们也会继续使用其他的loader，来完成其他文件的加载）；

这种方式可以更好的表示loader的配置，也方便后期的维护，同时也让你对各个Loader有一个全局的概览；

module.rules的配置如下：

rules属性对应的值是一个数组：[Rule]

数组中存放的是一个个的Rule，Rule是一个对象，对象中可以设置多个属性：

- test属性：用于对resource（资源）进行匹配的，通常会设置成正则表达式；

- use属性：对应的值时一个数组：[UseEntry]

  - UseEntry是一个对象，可以通过对象的属性来设置一些其他属性

    - loader：必须有一个loader属性，对应的值是一个字符串；

    - options：可选的属性，值是一个字符串或者对象，值会被传入到loader中；

    - query：目前已经使用options来替代；

传递字符串（如：use: [ 'style-loader' ]）是loader 属性的简写方式（如：use: [ { loader: 'style-loader'} ]）；

loader属性：Rule.use: [ { loader } ] 的简写。

![image-20220409203220306](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409203220306.png)

#### 认识style-loader

我们已经可以通过css-loader来加载css文件了

但是你会发现这个css在我们的代码中并没有生效（页面没有效果）。

这是为什么呢？

因为css-loader只是负责将.css文件进行解析，并不会将解析之后的css插入到页面中；

如果我们希望再完成插入style的操作，那么我们还需要另外一个loader，就是style-loader；

安装style-loader：npm install style-loader -D

#### 配置style-loader

那么我们应该如何使用style-loader：

- 在配置文件中，添加style-loader；

- 注意：因为loader的执行顺序是从右向左（或者说从下到上，或者说从后到前的），所以我们需要将style-loader写到css-loader的前面；

![image-20220409204931786](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409204931786.png)

重新执行编译npm run build，可以发现打包后的css已经生效了：

当前目前我们的css是通过页内样式的方式添加进来的；

后续我们也会讲如何将css抽取到单独的文件中，并且进行压缩等操作；

#### 如何处理less文件

![image-20220409205011346](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409205011346.png)

我们可以使用less工具来完成它的编译转换：

执行如下命令：npm install less -D

npx less ./src/css/title.less > title.css

但是在项目中我们会编写大量的css，它们如何可以自动转换呢？

这个时候我们就可以使用less-loader，来自动使用less工具转换less到css；npm install less-loader -D

配置webpack.config.js执行npm run build less就可以自动转换成css，并且页面也会生效了

#### 浏览器兼容性

我们来思考一个问题：开发中，浏览器的兼容性问题，我们应该如何去解决和处理？

当然这个问题很笼统，这里我说的兼容性问题不是指屏幕大小的变化适配；

我这里指的兼容性是针对不同的浏览器支持的特性：比如css特性、js语法，之间的兼容性；

我们知道市面上有大量的浏览器：

有Chrome、Safari、IE、Edge、Chrome for Android、UC Browser、QQBrowser等等；

它们的市场占率是多少？我们要不要兼容它们呢？

其实在很多的脚手架配置中，都能看到类似于这样的配置信息：

```js
\> 1%
last 2 versions
not dead
```

这里的百分之一，就是指市场占有率

#### 浏览器市场占有率

但是在哪里可以查询到浏览器的市场占有率呢？

这个最好用的网站，也是我们工具通常会查询的一个网站就是caniuse；

https://caniuse.com/usage-table

![image-20220409211654011](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409211654011.png)

#### 认识browserslist工具

但是有一个问题，我们如何可以在css兼容性和js兼容性下共享我们配置的兼容性条件呢？

就是当我们设置了一个条件：> 1%；

我们表达的意思是css要兼容市场占有率大于1%的浏览器，js也要兼容市场占有率大于1%的浏览器；

如果我们是通过工具来达到这种兼容性的，比如后面我们会讲到的postcss-prest-env、babel、autoprefixer等

如何可以让他们共享我们的配置呢？

这个问题的答案就是Browserslist；

Browserslist是什么？Browserslist是一个在不同的前端工具之间，共享目标浏览器和Node.js版本的配置：

- Autoprefixer

- Babel

- postcss-preset-env

- eslint-plugin-compat

- stylelint-no-unsupported-browser-features

- postcss-normalize

- obsolete-webpack-plugin

#### 浏览器查询过程

我们可以编写类似于这样的配置：

那么之后，这些工具会根据我们的配置来获取相关的浏览器信息，以方便决定是否需要进行兼容性的支持：

条件查询使用的是caniuse-lite的工具，这个工具的数据来自于caniuse的网站上；

![image-20220409211941422](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409211941422.png)

#### Browserslist编写规则

![image-20220409212048541](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409212048541.png)

![image-20220409212059327](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409212059327.png)

#### 使用browserslist

##### 命令行使用browserslist

我们可以直接通过命令来查询某些条件所匹配到的浏览器：

npx browserslist ">1%, last 2 version, not dead"

##### 配置browserslist

我们如何可以配置browserslist呢？两种方案：

方案一：在package.json中配置；

![image-20220409212246728](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409212246728.png)

方案二：单独的一个配置文件.browserslistrc文件

![image-20220409212254133](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409212254133.png)

#### 默认配置和条件关系

如果没有配置，那么也会有一个默认配置：

![image-20220409212423234](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409212423234.png)

我们编写了多个条件之后，多个条件之间是什么关系呢?

![image-20220409212438665](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409212438665.png)

#### 认识PostCSS工具

什么是PostCSS呢？

PostCSS是一个通过JavaScript来转换样式的工具；

这个工具可以帮助我们进行一些CSS的转换和适配，比如自动添加浏览器前缀、css样式的重置；

但是实现这些工具，我们需要借助于PostCSS对应的插件；

如何使用PostCSS呢？主要就是两个步骤：

第一步：查找PostCSS在构建工具中的扩展，比如webpack中的postcss-loader；

第二步：选择可以添加你需要的PostCSS相关的插件；

**命令行使用postcss**

npm install postcsspostcss-cli -D

我们编写一个需要添加前缀的css：

https://autoprefixer.github.io/

我们可以在上面的网站中查询一些添加css属性的样式

##### 插件autoprefixer

```shell
npm install autoprefixer -D
```

**直接使用postcss工具和使用定制的autoprefixer插件**

```shell
npx postcss --use autoprefixer -o end.css ./src/css/style.css
```

![image-20220409215548411](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220409215548411.png)

#### **postcss-loader**

真实开发中我们必然不会直接使用命令行工具来对css进行处理，而是可以借助于构建工具：

在webpack中使用postcss就是使用postcss-loader来处理的；

```shell
npm install postcss-loader -D
```

![image-20220410165243225](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410165243225.png)

##### 单独的postcss配置文件

当然，我们也可以将这些配置信息放到一个单独的文件中进行管理：

在根目录下创建postcss.config.js

![image-20220410165308779](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410165308779.png)

##### postcss-preset-env

postcss-preset-env也是一个postcss的插件；

它可以帮助我们将一些现代的CSS特性，转成大多数浏览器认识的CSS，并且会根据目标浏览器或者运行时环境添加所需的polyfill；

也包括会自动帮助我们添加autoprefixer（所以相当于已经内置了autoprefixer）；

```shell
npm install postcss-preset-env -D
```

之后，我们直接修改掉之前的autoprefixer即可:

![image-20220410165643107](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410165643107.png)

注意：我们在使用某些postcss插件时，也可以直接传入字符串：

![image-20220410165700792](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410165700792.png)

我们举一个例子：

我们这里在使用十六进制的颜色时设置了8位；

但是某些浏览器可能不认识这种语法，我们最好可以转成RGBA的形式；

但是autoprefixer是不会帮助我们转换的；

而postcss-preset-env就可以完成这样的功能；

##### importLoaders属性

存在的问题：当在css文件中引入其他的css文件的时候，这个被引入的css文件只会被css-loader处理，不会被前面的postcss-loader进行处理。

importLoaders的作用就是当遇到import这个属性的时候，需要前面的几个loaders开始加载

![image-20220410170433276](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410170433276.png)

## 加载和处理其他资源

### 案例准备

为了演示我们项目中可以加载图片，我们需要在项目中使用图片，比较常见的使用图片的方式是两种：

img元素，设置src属性；

其他元素（比如div），设置background-image的css属性

![image-20220410174050006](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410174050006.png)

### file-loader

要处理jpg、png等格式的图片，我们也需要有对应的loader：file-loader

file-loader的作用就是帮助我们处理import/require()方式引入的一个文件资源，并且会将它放到我们输出的文件夹中；

```shell
npm install file-loader -D
```

![image-20220410174241230](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410174241230.png)

#### 文件的名称规则

![image-20220410174313405](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410174313405.png)

![image-20220410174328047](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410174328047.png)

也可以根据outputPath：

![image-20220410174544682](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410174544682.png)

### url-loader

url-loader和file-loader的工作方式是相似的，但是可以将较小的文件，转成**base64的URI**

```shell
npm install url-loader -D
```

#### limit选项

但是开发中我们往往是小的图片需要转换，但是大的图片直接使用图片即可

这是因为小的图片转换base64之后可以和页面一起被请求，减少不必要的请求过程；

而大的图片也进行转换，反而会影响页面的请求速度；

那么，我们如何可以限制哪些大小的图片转换和不转换呢？

url-loader有一个options属性limit，可以用于设置转换的限制；

下面的代码38kb的图片会进行base64编码，而295kb的不会

**注意：limit是以字节为单位的**

![image-20220410174712621](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220410174712621.png)

### asset module type的介绍

我们当前使用的webpack版本是webpack5：

在webpack5之前，加载这些资源我们需要使用一些loader，比如raw-loader 、url-loader、file-loader；

在webpack5之后，我们可以直接使用资源模块类型（asset module type），来替代上面的这些loader；

资源模块类型(asset module type)，通过添加4 种新的模块类型，来替换所有这些loader：

- asset/resource 发送一个单独的文件并导出URL。之前通过使用file-loader 实现；
- asset/inline 导出一个资源的data URI。之前通过使用url-loader 实现；
- asset/source 导出资源的源代码。之前通过使用raw-loader 实现；
- asset在导出一个data URI 和发送一个单独的文件之间自动选择。之前通过使用url-loader，并且配置资源体积限制实现；

### Asset module type的使用

比如加载图片，我们可以使用下面的方式：

![image-20220411121749341](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411121749341.png)

但是，如何可以自定义文件的输出路径和文件名呢？p

方式一：修改output，添加assetModuleFilename属性；

![image-20220411121832767](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411121832767.png)

方式二：在Rule中，添加一个generator属性，并且设置filename；

![image-20220411121842789](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411121842789.png)

url-loader的limit效果：

我们需要两个步骤来实现：

步骤一：将type修改为asset；

步骤二：添加一个parser属性，并且制定dataUrl的条件，添加maxSize属性；

![image-20220411121948166](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411121948166.png)

### 加载字体文件

![image-20220411122030930](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411122030930.png)

这个时候打包会报错，因为无法正确的处理eot、ttf、woff等文件：

我们可以选择使用file-loader来处理，也可以选择直接使用webpack5的资源模块类型来处理；

![image-20220411122111845](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411122111845.png)

### 认识Plugin

Webpack的另一个核心是Plugin，官方有这样一段对Plugin的描述：

While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks like bundle optimization, asset management and injection of environment variables.

上面表达的含义翻译过来就是：

Loader是用于特定的模块类型进行转换；

Plugin可以用于执行更加广泛的任务，比如打包优化、资源管理、环境变量注入等；

![image-20220411122217396](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411122217396.png)

### CleanWebpackPlugin

前面我们演示的过程中，每次修改了一些配置，重新打包时，都需要手动删除dist文件夹：

我们可以借助于一个插件来帮助我们完成，这个插件就是CleanWebpackPlugin；

```shell
npm install clean-webpack-plugin -D
```

![image-20220411122551138](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411122551138.png)

### HtmlWebpackPlugin

另外还有一个不太规范的地方：

我们的HTML文件是编写在根目录下的，而最终打包的dist文件夹中是没有index.html文件的。

在进行项目部署的时，必然也是需要有对应的入口文件index.html；

所以我们也需要对index.html进行打包处理；

对HTML进行打包处理我们可以使用另外一个插件：HtmlWebpackPlugin；

```shell
npm install html-webpack-plugin -D
```

![image-20220411122640174](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411122640174.png)

我们会发现，现在自动在dist文件夹中，生成了一个index.html的文件：

该文件中也自动添加了我们打包的bundle.js文件；

![image-20220411122732875](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411122732875.png)

这个文件是如何生成的呢？

默认情况下是根据ejs的一个模板来生成的；

在html-webpack-plugin的源码中，有一个default_index.ejs模块；

#### 自定义HTML模板

如果我们想在自己的模块中加入一些比较特别的内容：

比如添加一个noscript标签，在用户的JavaScript被关闭时，给予响应的提示；

比如在开发vue或者react项目时，我们需要一个可以挂载后续组件的根标签<div id="app"></div>；

这个我们需要一个属于自己的index.html模块：

![image-20220411122927222](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411122927222.png)

#### 自定义模板数据填充

上面的代码中，会有一些类似这样的语法<%变量%>，这个是EJS模块填充数据的方式。

在配置HtmlWebpackPlugin时，我们可以添加如下配置：

template：指定我们要使用的模块所在的路径；

title：在进行htmlWebpackPlugin.options.title读取时，就会读到该信息；

![image-20220411123016387](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411123016387.png)

### DefinePlugin的介绍

但是，这个时候编译还是会报错，因为在我们的模块中还使用到一个BASE_URL的常量：

![image-20220411123044204](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411123044204.png)

这是因为在编译template模块时，有一个BASE_URL：

<link rel="icon"href="<%=BASE_URL %>favicon.ico">；

但是我们并没有设置过这个常量值，所以会出现没有定义的错误；

这个时候我们可以使用DefinePlugin插件；

### DefinePlugin的使用

DefinePlugin允许在编译时创建配置的全局常量，是一个webpack内置的插件（不需要单独安装）：

![image-20220411131625790](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411131625790.png)

### CopyWebpackPlugin

![image-20220411131819174](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220411131819174.png)

## 模块化原理和source-map

### Mode配置

Mode配置选项，可以告知webpack使用响应模式的内置优化：

默认值是production（什么都不设置的情况下）；

可选值有：'none' | 'development' | 'production'；

![image-20220412104849998](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412104849998.png)

### Mode配置代表更多	

![image-20220412104911574](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412104911574.png)

### Webpack的模块化

Webpack打包的代码，允许我们使用各种各样的模块化，但是最常用的是CommonJS、ESModule。

我们来研究一下它的原理，包括如下原理：

- CommonJS模块化实现原理；
- ESModule实现原理；
- CommonJS加载ESModule的原理；
- ESModule加载CommonJS的原理；

**commonJs**

```js
// 定义了一个对象
// 模块的路径(key): 函数(value)
var __webpack_modules__ = {
  "./src/js/format.js":
    (function (module) {
      const dateFormat = (date) => {
        return "2020-12-12";
      }
      const priceFormat = (price) => {
        return "100.00";
      }

      // 将我们要导出的变量, 放入到module对象中的exports对象
      module.exports = {
        dateFormat,
        priceFormat0
      }
    })
}

// 定义一个对象, 作为加载模块的缓存
var __webpack_module_cache__ = {};

// 是一个函数, 当我们加载一个模块时, 都会通过这个函数来加载
function __webpack_require__(moduleId) {
  // 1.判断缓存中是否已经加载过
  if (__webpack_module_cache__[moduleId]) {
    return __webpack_module_cache__[moduleId].exports;
  }

  // 2.给module变量和__webpack_module_cache__[moduleId]赋值了同一个对象
  var module = __webpack_module_cache__[moduleId] = { exports: {} };

  // 3.加载执行模块
  __webpack_modules__[moduleId](module, module.exports, __webpack_require__);

  // 4.导出module.exports {dateFormat: function, priceForamt: function}
  return module.exports;
}


// 具体开始执行代码逻辑
!function () {
  // 1.加载./src/js/format.js
  const { dateFormat, priceFormat } = __webpack_require__("./src/js/format.js");
  console.log(dateFormat("abc"));
  console.log(priceFormat("abc"));
}();
```

**es module**

```js
// 1.定义了一个对象, 对象里面放的是我们的模块映射
var __webpack_modules__ = {
  "./src/es_index.js":
    (function (__unused_webpack_module, __webpack_exports__, __webpack_require__) {
      // 调用r的目的是记录时一个__esModule -> true
      __webpack_require__.r(__webpack_exports__);

      // _js_math__WEBPACK_IMPORTED_MODULE_0__ == exports
      var _js_math__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__("./src/js/math.js");

      console.log(_js_math__WEBPACK_IMPORTED_MODULE_0__.mul(20, 30));
      console.log(_js_math__WEBPACK_IMPORTED_MODULE_0__.sum(20, 30));
    }),
  "./src/js/math.js":
    (function (__unused_webpack_module, __webpack_exports__, __webpack_require__) {
      __webpack_require__.r(__webpack_exports__);

      // 调用了d函数: 给exports设置了一个代理definition
      // exports对象中本身是没有对应的函数
      __webpack_require__.d(__webpack_exports__, {
        "sum": function () { return sum; },
        "mul": function () { return mul; }
      });

      const sum = (num1, num2) => {
        return num1 + num2;
      }
      const mul = (num1, num2) => {
        return num1 * num2;
      }
    })
};

// 2.模块的缓存
var __webpack_module_cache__ = {};

// 3.require函数的实现(加载模块)
function __webpack_require__(moduleId) {
  if (__webpack_module_cache__[moduleId]) {
    return __webpack_module_cache__[moduleId].exports;
  }
  var module = __webpack_module_cache__[moduleId] = {
    exports: {}
  };
  __webpack_modules__[moduleId](module, module.exports, __webpack_require__);
  return module.exports;
}

!function () {
  // __webpack_require__这个函数对象添加了一个属性: d -> 值function
  __webpack_require__.d = function (exports, definition) {
    for (var key in definition) {
      if (__webpack_require__.o(definition, key) && !__webpack_require__.o(exports, key)) {
        Object.defineProperty(exports, key, { enumerable: true, get: definition[key] });
      }
    }
  };
}();


!function () {
  // __webpack_require__这个函数对象添加了一个属性: o -> 值function
  __webpack_require__.o = function (obj, prop) { return Object.prototype.hasOwnProperty.call(obj, prop); }
}();

!function () {
  // __webpack_require__这个函数对象添加了一个属性: r -> 值function
  __webpack_require__.r = function (exports) {
    if (typeof Symbol !== 'undefined' && Symbol.toStringTag) {
      Object.defineProperty(exports, Symbol.toStringTag, { value: 'Module' });
    }
    Object.defineProperty(exports, '__esModule', { value: true });
  };
}();


__webpack_require__("./src/es_index.js");
```

**commonJs和es_module**

```js
var __webpack_modules__ = ({
  "./src/index.js":
    (function (__unused_webpack_module, __webpack_exports__, __webpack_require__) {
      "use strict";
      __webpack_require__.r(__webpack_exports__);
      var _js_format__WEBPACK_IMPORTED_MODULE_0__ = __webpack_require__("./src/js/format.js");
      var _js_format__WEBPACK_IMPORTED_MODULE_0___default = __webpack_require__.n(_js_format__WEBPACK_IMPORTED_MODULE_0__);
      
      // es module导出内容, CommonJS导入内容
      const math = __webpack_require__("./src/js/math.js");

      // CommonJS导出内容, es module导入内容
      console.log(math.sum(20, 30));
      console.log(math.mul(20, 30));
      console.log(_js_format__WEBPACK_IMPORTED_MODULE_0___default().dateFormat("aaa"));
      console.log(_js_format__WEBPACK_IMPORTED_MODULE_0___default().priceFormat("bbb"));
    }),
  "./src/js/format.js":
    (function (module) {
      const dateFormat = (date) => {
        return "2020-12-12";
      }
      const priceFormat = (price) => {
        return "100.00";
      }
      module.exports = {
        dateFormat,
        priceFormat
      }
    }),

  "./src/js/math.js":
    (function (__unused_webpack_module, __webpack_exports__, __webpack_require__) {
      
      __webpack_require__.r(__webpack_exports__);
      __webpack_require__.d(__webpack_exports__, {
        "sum": function () { return sum; },
        "mul": function () { return mul; }
      });
      const sum = (num1, num2) => {
        return num1 + num2;
      }

      const mul = (num1, num2) => {
        return num1 * num2;
      }
    })
});

var __webpack_module_cache__ = {};

// The require function
function __webpack_require__(moduleId) {
  // Check if module is in cache
  if (__webpack_module_cache__[moduleId]) {
    return __webpack_module_cache__[moduleId].exports;
  }
  // Create a new module (and put it into the cache)
  var module = __webpack_module_cache__[moduleId] = {
    // no module.id needed
    // no module.loaded needed
    exports: {}
  };

  // Execute the module function
  __webpack_modules__[moduleId](module, module.exports, __webpack_require__);

  // Return the exports of the module
  return module.exports;
}

!function () {
  // getDefaultExport function for compatibility with non-harmony modules
  __webpack_require__.n = function (module) {
    var getter = module && module.__esModule ?
      function () { return module['default']; } :
      function () { return module; };
    __webpack_require__.d(getter, { a: getter });
    return getter;
  };
}();

/* webpack/runtime/define property getters */
!function () {
  // define getter functions for harmony exports
  __webpack_require__.d = function (exports, definition) {
    for (var key in definition) {
      if (__webpack_require__.o(definition, key) && !__webpack_require__.o(exports, key)) {
        Object.defineProperty(exports, key, { enumerable: true, get: definition[key] });
      }
    }
  };
}();

/* webpack/runtime/hasOwnProperty shorthand */
!function () {
  __webpack_require__.o = function (obj, prop) { return Object.prototype.hasOwnProperty.call(obj, prop); }
}();

/* webpack/runtime/make namespace object */
!function () {
  // define __esModule on exports
  __webpack_require__.r = function (exports) {
    if (typeof Symbol !== 'undefined' && Symbol.toStringTag) {
      Object.defineProperty(exports, Symbol.toStringTag, { value: 'Module' });
    }
    Object.defineProperty(exports, '__esModule', { value: true });
  };
}();

__webpack_require__("./src/index.js");
```

### 认识source-map

我们的代码通常运行在浏览器上时，是通过打包压缩的：

也就是真实跑在浏览器上的代码，和我们编写的代码其实是有差异的；

比如ES6的代码可能被转换成ES5；

比如对应的代码行号、列号在经过编译后肯定会不一致；

比如代码进行丑化压缩时，会将编码名称等修改；

比如我们使用了TypeScript等方式编写的代码，最终转换成JavaScript；

但是，当代码报错需要调试时（debug），调试转换后的代码是很困难的

但是我们能保证代码不出错吗？不可能。

那么如何可以调试这种转换后不一致的代码呢？答案就是source-map

source-map是从已转换的代码，映射到原始的源文件；

使浏览器可以重构原始源并在调试器中显示重建的原始源；

### 如何使用source-map

如何可以使用source-map呢？两个步骤：

第一步：根据源文件，生成source-map文件，webpack在打包时，可以通过配置生成source-map；

第二步：在转换后的代码，最后添加一个注释，它指向sourcemap；

```js
//# sourceMappingURL=common.bundle.js.map
```

浏览器会根据我们的注释，查找响应的source-map，并且根据source-map还原我们的代码，方便进行调试。

在Chrome中，我们可以按照如下的方式打开source-map：

![image-20220412111503513](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412111503513.png)

### 分析source-map

最初source-map生成的文件带下是原始文件的10倍，第二版减少了约50%，第三版又减少了50%，所以目前一个133kb的文件，最终的source-map的大小大概在300kb。

![image-20220412111543032](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412111543032.png)

**source-map文件**

![image-20220412111900221](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412111900221.png)

### 生成source-map

如何在使用webpack打包的时候，生成对应的source-map呢？

webpack为我们提供了非常多的选项（目前是26个），来处理source-map；phttps://webpack.docschina.org/configuration/devtool/

选择不同的值，生成的source-map会稍微有差异，打包的过程也会有性能的差异，可以根据不同的情况进行选择；

下面几个值不会生成source-map

- false：不使用source-map，也就是没有任何和source-map相关的内容。
- none：production模式下的默认值，不生成source-map。
- eval：development模式下的默认值，不生成source-map
  - 但是它会在eval执行的代码中，添加//#sourceURL=；
  - 它会被浏览器在执行时解析，并且在调试面板中生成对应的一些文件目录，方便我们调试代码；

**eval的效果**

![image-20220412112137629](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412112137629.png)

#### source-map值

source-map：

- 生成一个独立的source-map文件，并且在bundle文件中有一个注释，指向source-map文件；

bundle文件中有如下的注释：

- 开发工具会根据这个注释找到source-map文件，并且解析；

```js
//# sourceMappingURL=bundle.js.map
```

![image-20220412112346986](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412112346986.png)

#### eval-source-map值

eval-source-map：会生成sourcemap，但是source-map是以DataUrl添加到eval函数的后面

![image-20220412112433626](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412112433626.png)

#### inline-source-map值

inline-source-map：会生成sourcemap，但是source-map是以DataUrl添加到bundle文件的后面

![image-20220412112552125](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412112552125.png)

#### cheap-source-map

cheap-source-map：

- 会生成sourcemap，但是会更加高效一些（cheap低开销），因为它没有生成列映射（Column Mapping）

- 因为在开发中，我们只需要行信息通常就可以定位到错误了

![image-20220412112846375](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412112846375.png)

#### cheap-module-source-map值

cheap-module-source-map：

- 会生成sourcemap，类似于cheap-source-map，但是对源自loader的sourcemap处理会更好。

这里有一个很模糊的概念：对源自loader的sourcemap处理会更好，官方也没有给出很好的解释

- 其实是如果loader对我们的源码进行了特殊的处理，比如babel；

如果我这里使用了babel-loader（注意：目前还没有讲babel）

- 可以先按照我的babel配置演练；

![image-20220412112932928](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412112932928.png)

#### cheap-source-map和cheap-module-source-map

cheap-source-map和cheap-module-source-map的区别：

![image-20220412113000107](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412113000107.png)

#### hidden-source-map值

hidden-source-map：

- 会生成sourcemap，但是不会对source-map文件进行引用；

- 相当于删除了打包文件中对sourcemap的引用注释；

```js
// 被删除掉的
//# sourceMappingURL=bundle.js.map
```

如果我们手动添加进来，那么sourcemap就会生效了

#### nosources-source-map值

nosources-source-map：

- 会生成sourcemap，但是生成的sourcemap只有错误信息的提示，不会生成源代码文件；

正确的错误提示：

![image-20220412113119350](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412113119350.png)

点击错误提示，无法查看源码：

![image-20220412113129081](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412113129081.png)

#### 多个值的组合

![image-20220412113149760](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220412113149760.png)

## Babel的深入解析

### 为什么需要babel

事实上，在开发中我们很少直接去接触babel，但是babel对于前端开发来说，目前是不可缺少的一部分：

开发中，我们想要使用ES6+的语法，想要使用TypeScript，开发React项目，它们都是离不开Babel的；

所以，学习Babel对于我们理解代码从编写到线上的转变过程直观重要；

了解真相，你才能获得真知的自由！

那么，Babel到底是什么呢？

- Babel是一个工具链，主要用于旧浏览器或者缓解中将ECMAScript 2015+代码转换为向后兼容版本的JavaScript；
- 包括：语法转换、源代码转换、Polyfill实现目标缓解缺少的功能等；

### Babel命令行使用

babel本身可以作为一个独立的工具（和postcss一样），不和webpack等构建工具配置来单独使用。

如果我们希望在命令行尝试使用babel，需要安装如下库：

@babel/core：babel的核心代码，必须安装；

@babel/cli：可以让我们在命令行使用babel；

```shell
npm install @babel/cli @babel/core
```

使用babel来处理我们的源代码：

- src：是源文件的目录；

- --out-dir：指定要输出的文件夹dist；

```shell
npx babel src --out-dirdist
```

### 插件的使用

- **比如我们需要转换箭头函数，那么我们就可以使用箭头函数转换相关的插件：**

```shell
npm install @babel/plugin-transform-arrow-functions -D
npxbabel src --out-dir dist --plugins=@babel/plugin-transform-arrow-functions
```

查看转换后的结果：我们会发现const 并没有转成var

这是因为plugin-transform-arrow-functions，并没有提供这样的功能；

我们需要使用plugin-transform-block-scoping 来完成这样的功能；

```shell
npm install @babel/plugin-transform-block-scoping -D 
npxbabel src --out-dir dist --plugins=@babel/plugin-transform-block-scoping,@babel/plugin-transform-arrow-functions
```

### Babel的预设preset

但是如果要转换的内容过多，一个个设置是比较麻烦的，我们可以使用预设（preset）：

后面我们再具体来讲预设代表的含义；

安装@babel/preset-env预设：

```shell
npm install @babel/preset-env -D
```

执行如下命令：

```shell
npx babel src --out-dir dist --presets=@babel/preset-env
```

### Babel的底层原理

babel是如何做到将我们的一段代码（ES6、TypeScript、React）转成另外一段代码（ES5）的呢？

从一种源代码（原生语言）转换成另一种源代码（目标语言），这是什么的工作呢？

就是编译器，事实上我们可以将babel看成就是一个编译器。

Babel编译器的作用就是将我们的源代码，转换成浏览器可以直接识别的另外一段源代码；

Babel也拥有编译器的工作流程：

- 解析阶段（Parsing）

- 转换阶段（Transformation）

- 生成阶段（Code Generation）

https://github.com/jamiebuilds/the-super-tiny-compiler

### babel编译器执行原理

Babel的执行阶段

![image-20220413004524980](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413004524980.png)

当然，这只是一个简化版的编译器工具流程，在每个阶段又会有自己具体的工作：

![image-20220413004536396](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413004536396.png)

### babel-loader

在实际开发中，我们通常会在构建工具中通过配置babel来对其进行使用的，比如在webpack中。

那么我们就需要去安装相关的依赖：

如果之前已经安装了@babel/core，那么这里不需要再次安装；

```shell
npm install babel-loader @babel/core
```

我们可以设置一个规则，在加载js文件时，使用我们的babel：

![image-20220413004745059](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413004745059.png)

### 指定使用的插件

我们必须指定使用的插件才会生效

![image-20220413004816198](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413004816198.png)

### babel-preset

如果我们一个个去安装使用插件，那么需要手动来管理大量的babel插件，我们可以直接给webpack提供一个preset，webpack会根据我们的预设来加载对应的插件列表，并且将其传递给babel

比如常见的预设有三个：

- env

- react

- TypeScript

安装preset-env：

```shell
npm install @babel/preset-env
```

![image-20220413111711902](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413111711902.png)

### 设置目标浏览器browserslist

我们最终打包的JavaScript代码，是需要跑在目标浏览器上的，那么如何告知babel我们的目标浏览器呢？

browserslist工具

target属性

之前我们项目中已经使用了browserslist工具，我们可以对比一下不同的配置，打包的区别：

![image-20220413111755470](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413111755470.png)

#### 设置目标浏览器targets

我们也可以通过targets来进行配置：

![image-20220413111820654](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413111820654.png)

那么，如果两个同时配置了，哪一个会生效呢？

配置的targets属性会覆盖browserslist；

但是在开发中，更推荐通过browserslist来配置，因为类似于postcss工具，也会使用browserslist，进行统一浏览器的适配；

### Stage-X的preset

要了解Stage-X，我们需要先了解一下TC39的组织：

TC39是指技术委员会（Technical Committee）第39 号；

它是ECMA 的一部分，ECMA 是“ECMAScript” 规范下的JavaScript 语言标准化的机构；

ECMAScript 规范定义了JavaScript 如何一步一步的进化、发展；

TC39 遵循的原则是：分阶段加入不同的语言特性，新流程涉及四个不同的Stage

**Stage 0**：strawman（稻草人），任何尚未提交作为正式提案的讨论、想法变更或者补充都被认为是第0 阶段的"稻草人"；

**Stage 1**：proposal（提议）,提案已经被正式化，并期望解决此问题，还需要观察与其他提案的相互影响；

**Stage 2**：draft（草稿），Stage2的提案应提供规范初稿、草稿。此时，语言的实现者开始观察runtime 的具体实现是否合理；

**Stage 3**：candidate（候补），Stage3提案是建议的候选提案。在这个高级阶段，规范的编辑人员和评审人员必须在最终规范上签字。Stage 3 的提案不会有太大的改变，在对外发布之前只是修正一些问题；

**Stage 4**：finished（完成），进入Stage4的提案将包含在ECMAScript 的下一个修订版中；

#### Babel的Stage-X设置

在babel7之前（比如babel6中），我们会经常看到这种设置方式：

它表达的含义是使用对应的babel-preset-stage-x预设；

但是从babel7开始，已经不建议使用了，建议使用preset-env来设置；

![image-20220413112350195](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413112350195.png)

### Babel的配置文件

像之前一样，我们可以将babel的配置信息放到一个独立的文件中，babel给我们提供了两种配置文件的编写：

babel.config.json（或者.js，.cjs，.mjs）文件；

.babelrc.json（或者.babelrc，.js，.cjs，.mjs）文件；

它们两个有什么区别呢？目前很多的项目都采用了多包管理的方式（babel本身、element-plus、umi等）；

.babelrc.json：早期使用较多的配置方式，但是对于配置Monorepos项目是比较麻烦的；

babel.config.json（babel7）：可以直接作用于Monorepos项目的子包，更加推荐；

![image-20220413112653173](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413112653173.png)

### 认识polyfill

Polyfill是什么呢？p

翻译：一种用于衣物、床具等的聚酯填充材料,使这些物品更加温暖舒适；

理解：更像是应该填充物（垫片），一个补丁，可以帮助我们更好的使用JavaScript；

为什么时候会用到polyfill呢？

比如我们使用了一些语法特性（例如：Promise,Generator,Symbol等以及实例方法例如Array.prototype.includes等）

但是某些浏览器压根不认识这些特性，必然会报错；

我们可以使用polyfill来填充或者说打一个补丁，那么就会包含该特性了；

### polyfill的使用

babel7.4.0之前，可以使用@babel/polyfill的包，但是该包现在已经不推荐使用了：

babel7.4.0之后，可以通过单独引入core-js和regenerator-runtime来完成polyfill的使用：

```shell
npm install core-js regenerator-runtime --save
```

为了防止对第三包的影响，加上exclude：

![image-20220413112850944](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413112850944.png)

### 配置babel.config.js

我们需要在babel.config.js文件中进行配置，给preset-env配置一些属性：

- useBuiltIns：设置以什么样的方式来使用polyfill；

- corejs：设置corejs的版本，目前使用较多的是3.x的版本，比如我使用的是3.8.x的版本；p另外corejs可以设置是否对提议阶段的特性进行支持；p设置proposals属性为true即可；

#### useBuiltIns属性设置

useBuiltIns属性有三个常见的值

第一个值：falsep

- 打包后的文件不使用polyfill来进行适配；

- 并且这个时候是不需要设置corejs属性的；

第二个值：usagep

- 会根据源代码中出现的语言特性，自动检测所需要的polyfill；

- 这样可以确保最终包里的polyfill数量的最小化，打包的包相对会小一些；

- 可以设置corejs属性来确定使用的corejs的版本；

![image-20220413113423207](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413113423207.png)

第三个值：entry

如果我们依赖的某一个库本身使用了某些polyfill的特性，但是因为我们使用的是usage，所以之后用户浏览器可能会报错；

所以，如果你担心出现这种情况，可以使用entry；

并且需要在入口文件中添加`import 'core-js/stable'; import 'regenerator-runtime/runtime';

这样做会根据browserslist目标导入所有的polyfill，但是对应的包也会变大；

![image-20220413113542831](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413113542831.png)

#### 认识Plugin-transform-runtime（了解）

在前面我们使用的polyfill，默认情况是添加的所有特性都是全局的p如果我们正在编写一个工具库，这个工具库需要使用polyfill；

别人在使用我们工具时，工具库通过polyfill添加的特性，可能会污染它们的代码；

所以，当编写工具时，babel更推荐我们使用一个插件：@babel/plugin-transform-runtime来完成polyfill的功能；

#### 使用Plugin-transform-runtime

安装@babel/plugin-transform-runtime：

```shell
npm install @babel/plugin-transform-runtime -D
```

使用plugins来配置babel.config.js：

![image-20220413120200978](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413120200978.png)

注意：因为我们使用了corejs3，所以我们需要安装对应的库：

![image-20220413120234660](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413120234660.png)

#### React的jsx支持

在我们编写react代码时，react使用的语法是jsx，jsx是可以直接使用babel来转换的。

对react jsx代码进行处理需要如下的插件：

- @babel/plugin-syntax-jsx

- @babel/plugin-transform-react-jsx

- @babel/plugin-transform-react-display-name

但是开发中，我们并不需要一个个去安装这些插件，我们依然可以使用preset来配置：npm install @babel/preset-react -D

![image-20220413120613262](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413120613262.png)

### TypeScript的编译

在项目开发中，我们会使用TypeScript来开发，那么TypeScript代码是需要转换成JavaScript代码。

可以通过TypeScript的compiler来转换成JavaScript：

```shell
npm install typescript -D
```

另外TypeScript的编译配置信息我们通常会编写一个tsconfig.json文件：

```shell
tsc --init
```

生成配置文件如下

![image-20220413120756623](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413120756623.png)

之后我们可以运行npxtsc来编译自己的ts代码：

```shell
npx tsc
```

#### 使用ts-loader

如果我们希望在webpack中使用TypeScript，那么我们可以使用ts-loader来处理ts文件：

```shell
npm install ts-loader -D
```

配置ts-loader：

![image-20220413153512735](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413153512735.png)

之后，我们通过npm run build打包即可

#### 使用babel-loader

除了可以使用TypeScript Compiler来编译TypeScript之外，我们也可以使用Babel：

- Babel是有对TypeScript进行支持；

- 我们可以使用插件：@babel/tranform-typescript；

- 但是更推荐直接使用preset：@babel/preset-typescript；

我们来安装@babel/preset-typescript：

```shell
npm install @babel/preset-typescript -D
```

![image-20220413153626830](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413153626830.png)

#### ts-loader和babel-loader选择

那么我们在开发中应该选择ts-loader还是babel-loader呢？

使用ts-loader（TypeScript Compiler）

- 来直接编译TypeScript，那么只能将ts转换成js；

- 如果我们还希望在这个过程中添加对应的polyfill，那么ts-loader是无能为力的；

- 我们需要借助于babel来完成polyfill的填充功能；

使用babel-loader（Babel）

- 来直接编译TypeScript，也可以将ts转换成js，并且可以实现polyfill的功能；

- 但是babel-loader在编译的过程中，不会对类型错误进行检测；

#### 编译TypeScript最佳实践

事实上TypeScript官方文档有对其进行说明：

![image-20220413154003291](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413154003291.png)

也就是说我们使用Babel来完成代码的转换，使用tsc来进行类型的检查。

但是，如何可以使用tsc来进行类型的检查呢？

在这里，我在scripts中添加了两个脚本，用于类型检查；

我们执行npm run type-check可以对ts代码的类型进行检测；

我们执行npm run type-check-watch可以实时的检测类型错误；

![image-20220413154242332](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413154242332.png)

**注意：--noEmit的意思是不编译产生js文件**

### 认识ESLint

什么是ESLint呢？

ESLint是一个静态代码分析工具（Static program analysis，在没有任何程序执行的情况下，对代码进行分析）；

ESLint可以帮助我们在项目中建立统一的团队代码规范，保持正确、统一的代码风格，提高代码的可读性、可维护性；

并且ESLint的规则是可配置的，我们可以自定义属于自己的规则；

早期还有一些其他的工具，比如JSLint、JSHint、JSCS等，目前使用最多的是ESLint。

### 使用ESLint

首先我们需要安装ESLint：

```shell
npm install eslint -D
```

选择想要使用的ESLint：

![image-20220413154404480](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413154404480.png)

执行检测命令：

```shell
npx eslint ./src/main.js
```

### ESLint的文件解析

默认创建的环境如下：

env：运行的环境，比如是浏览器，并且我们会使用es2021（对应的ecmaVersion是12）的语法；

extends：可以扩展当前的配置，让其继承自其他的配置信息，可以跟字符串或者数组（多个）；

parserOptions：这里可以指定ESMAScript的版本、sourceType的类型

- parser：默认情况下是espree（也是一个JS Parser，用于ESLint），但是因为我们需要编译TypeScript，所以需要指定对应的解释器；

plugins：指定我们用到的插件；

rules：自定义的一些规则；

#### VSCode的ESLint插件

但是如果每次校验时，都需要执行一次npm run eslint就有点麻烦了，所以我们可以使用一个VSCode的插件：ESLint

一方面我们可以修改代码来修复错误，另外我们可以通过配置规则：

格式是：配置的规则名称：对应的值值可以是数字、字符串、数组：

- 字符串对应有三个值：off、warn、error；
- 数字对应有三个值：0、1、2（分别对应上面的值）;
- 数组我们可以告知对应的提示以及希望获取到的值：比如['error','double']

#### VSCode的Prettier插件

ESLint会帮助我们提示错误（或者警告），但是不会帮助我们自动修复：

在开发中我们希望文件在保存时，可以自动修复这些问题；

我们可以选择使用另外一个工具：prettier；

![image-20220413154656087](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413154656087.png)

#### ESLint-Loader的使用

事实上，我们在编译代码的时候，也希望进行代码的eslint检测，这个时候我们就可以使用eslint-loader来完成了

```shell
npm install eslint-loader -D
```

![image-20220413154940599](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413154940599.png)

#### Webpack中配置vue加载

安装相关的依赖：	

```shell
npm install vue-loader -D
npm install vue-template-compiler -D
```

配置webpack

![image-20220413155237177](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220413155237177.png)

## 本地服务器和HMR

为什么要搭建本地服务器？

目前我们开发的代码，为了运行需要有两个操作：

- 操作一：npm run build，编译相关的代码；
- 操作二：通过live server或者直接通过浏览器，打开index.html代码，查看效果；

这个过程经常操作会影响我们的开发效率，我们希望可以做到，当文件发生变化时，可以自动的完成编译和展示

为了完成自动编译，webpack提供了几种可选的方式

- webpack watch mode；
- webpack-dev-server；
- webpack-dev-middleware

### Webpack watch

webpack给我们提供了watch模式：
在该模式下，webpack依赖图中的所有文件，只要有一个发生了更新，那么代码将被重新编译；
我们不需要手动去运行npm run build指令了

如何开启watch呢？两种方式：

- 方式一：在导出的配置中，添加watch: true；
- 方式二：在启动webpack的命令中，添加--watch的标识；

这里我们选择方式二，在package.json的scripts 中添加一个watch 的脚本：

![image-20220418202158630](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418202158630.png)

### webpack-dev-server

上面的方式可以监听到文件的变化，但是事实上它本身是自动刷新浏览器的功能

当然，目前我们可以在VSCode中使用live-server来完成这样的功能；

但是，我们希望在不适用live-server的情况下，可以具备live reloading（实时重新加载）的功能

安装webpack-dev-server

```shell
npm install --save-dev webpack-dev-server
```

添加一个新的scripts脚本

![image-20220418203230196](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418203230196.png)

webpack-dev-server 在编译之后不会写入到任何输出文件。而是将bundle 文件保留在内存中：

事实上webpack-dev-server使用了一个库叫memfs（memory-fs webpack自己写的）

### webpack-dev-middleware

默认情况下，webpack-dev-server已经帮助我们做好了一切：

比如通过express启动一个服务，比如HMR（热模块替换）；

如果我们想要有更好的自由度，可以使用webpack-dev-middleware；

什么是webpack-dev-middleware？

webpack-dev-middleware 是一个封装器(wrapper)，它可以把webpack 处理过的文件发送到一个server；

webpack-dev-server 在内部使用了它，然而它也可以作为一个单独的package 来使用，以便根据需求进行

更多自定义设置；

#### webpack-dev-middleware的使用

安装express和webpack-dev-middleware：

```shell
npm install --save-dev express webpack-dev-middleware
```

![image-20220418203743018](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418203743018.png)

### 认识模块热替换（HMR）

什么是HMR呢？
HMR的全称是Hot Module Replacement，翻译为模块热替换；
模块热替换是指在应用程序运行过程中，替换、添加、删除模块，而无需重新刷新整个页面；

HMR通过如下几种方式，来提高开发的速度：

- 不重新加载整个页面，这样可以保留某些应用程序的状态不丢失；
- 只更新需要变化的内容，节省开发的时间；
- 修改了css、js源代码，会立即在浏览器更新，相当于直接在浏览器的devtools中直接修改样式；

如何使用HMR呢？
默认情况下，webpack-dev-server已经支持HMR，我们只需要开启即可；
在不开启HMR的情况下，当我们修改了源代码之后，整个页面会自动刷新，使用的是live reloading；

#### 开启HMR

修改webpack的配置：

![image-20220418204616217](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418204616217.png)

浏览器可以看到如下效果：

![image-20220418204631063](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418204631063.png)

但是你会发现，当我们修改了某一个模块的代码时，依然是刷新的整个页面：

这是因为我们需要去指定哪些模块发生更新时，进行HMR

![image-20220418204651167](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418204651167.png)

#### 框架的HMR

有一个问题：在开发其他项目时，我们是否需要经常手动去写入module.hot.accpet相关的API呢？
比如开发Vue、React项目，我们修改了组件，希望进行热更新，这个时候应该如何去操作呢？
事实上社区已经针对这些有很成熟的解决方案了：
比如vue开发中，我们使用vue-loader，此loader支持vue组件的HMR，提供开箱即用的体验；
比如react开发中，有React Hot Loader，实时调整react组件（目前React官方已经弃用了，改成使用react-refresh）；
接下来我们分别对React、Vue实现一下HMR功能。

#### React的HMR

在之前，React是借助于React Hot Loader来实现的HMR，目前已经改成使用react-refresh来实现了

安装实现HMR相关的依赖：

注意：这里安装@pmmmwh/react-refresh-webpack-plugin，最新的npm安装有bug（建议使用lts版本对应的npm版本）；

```shell
npm install -D @pmmmwh/react-refresh-webpack-plugin react-refresh
```

修改webpack.config.js和babel.config.js文件：

![image-20220418210227068](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418210227068.png)

#### Vue的HMR

Vue的加载我们需要使用vue-loader，而vue-loader加载的组件默认会帮助我们进行HMR的处理

安装加载vue所需要的依赖：

```shell
npm install vue-loader vue-template-compiler -D
```

配置webpack.config.js：

![image-20220418210321327](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418210321327.png)

#### HMR的原理

那么HMR的原理是什么呢？如何可以做到只更新一个模块中的内容呢？
webpack-dev-server会创建两个服务：提供静态资源的服务（express）和Socket服务（net.Socket）；
express server负责直接提供静态资源的服务（打包后的资源直接被浏览器请求和解析）；

HMR Socket Server，是一个socket的长连接：
长连接有一个最好的好处是建立连接后双方可以通信（服务器可以直接发送文件到客户端）；
当服务器监听到对应的模块发生变化时，会生成两个文件.json（manifest文件）和.js文件（update chunk）；
通过长连接，可以直接将这两个文件主动发送给客户端（浏览器）；
浏览器拿到两个新的文件后，通过HMR runtime机制，加载这两个文件，并且针对修改的模块进行更新；

#### HMR的原理图

![image-20220418210601209](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418210601209.png)

### output的publicPath

output中的path的作用是告知webpack之后的输出目录：
比如静态资源的js、css等输出到哪里，常见的会设置为dist、build文件夹等；

output中还有一个publicPath属性，该属性是指定index.html文件打包引用的一个基本路径：
它的默认值是一个空字符串，所以我们打包后引入js文件时，路径是bundle.js；
在开发中，我们也将其设置为/ ，路径是/bundle.js，那么浏览器会根据所在的域名+路径去请求对应的资源；
如果我们希望在本地直接打开html文件来运行，会将其设置为./，路径时./bundle.js，可以根据相对路径去查找资源；

### devServer的publicPath

devServer中也有一个publicPath的属性，该属性是指定本地服务所在的文件夹：
它的默认值是/，也就是我们直接访问端口即可访问其中的资源http://localhost:8080；
如果我们将其设置为了/abc，那么我们需要通过http://localhost:8080/abc才能访问到对应的打包后的资源；
并且这个时候，我们其中的bundle.js通过http://localhost:8080/bundle.js也是无法访问的：

- 所以必须将output.publicPath也设置为/abc；
- 官方其实有提到，建议devServer.publicPath与output.publicPath相同；

### devServer的contentBase

devServer中contentBase对于我们直接访问打包后的资源其实并没有太大的作用，它的主要作用是如果我们打包后的资源，又依赖于其他的一些资源，那么就需要指定从哪里来查找这个内容：

比如在index.html中，我们需要依赖一个abc.js文件，这个文件我们存放在public文件中；

在index.html中，我们应该如何去引入这个文件呢？

比如代码是这样的：<script src="./public/abc.js"></script>；

但是这样打包后浏览器是无法通过相对路径去找到这个文件夹的；

所以代码是这样的：<script src="/abc.js"></script>;

但是我们如何让它去查找到这个文件的存在呢？设置contentBase即可；

当然在devServer中还有一个可以监听contentBase发生变化后重新编译的一个属性：watchContentBase

### hotOnly、host配置

hotOnly是当代码编译失败时，是否刷新整个页面：

- 默认情况下当代码编译失败修复后，我们会重新刷新整个页面；

- 如果不希望重新刷新整个页面，可以设置hotOnly为true；

host设置主机地址：

- 默认值是localhost；

- 如果希望其他地方也可以访问，可以设置为0.0.0.0；

localhost 和0.0.0.0 的区别：

- localhost：本质上是一个域名，通常情况下会被解析成127.0.0.1;

- 127.0.0.1：回环地址(Loop Back Address)，表达的意思其实是我们主机自己发出去的包，直接被自己接收;

  - 正常的数据库包经常应用层- 传输层- 网络层- 数据链路层- 物理层;

  - 而回环地址，是在网络层直接就被获取到了，是不会经常数据链路层和物理层的; 

  - 比如我们监听127.0.0.1时，在同一个网段下的主机中，通过ip地址是不能访问的;

- 0.0.0.0：监听IPV4上所有的地址，再根据端口找到不同的应用程序;

  - 比如我们监听0.0.0.0时，在同一个网段下的主机中，通过ip地址是可以访问的;

### port、open、compress

port设置监听的端口，默认情况下是8080

open是否打开浏览器：

- 默认值是false，设置为true会打开浏览器；
- 也可以设置为类似于Google Chrome等值

compress是否为静态文件开启gzip compression：

- 默认值是false，可以设置为true

![image-20220418220323951](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418220323951.png)

### Proxy代理

proxy是我们开发中非常常用的一个配置选项，它的目的设置代理来解决跨域访问的问题：

比如我们的一个api请求是http://localhost:8888，但是本地启动服务器的域名是http://localhost:8000，这个时候发送网络请求就会出现跨域的问题；

那么我们可以将请求先发送到一个代理服务器，代理服务器和API服务器没有跨域的问题，就可以解决我们的
跨域问题了；

我们可以进行如下的设置：

target：表示的是代理到的目标地址，比如/api-hy/moment会被代理到http://localhost:8888/api-hy/moment；

pathRewrite：默认情况下，我们的/api-hy也会被写入到URL中，如果希望删除，可以使用pathRewrite；

secure：默认情况下不接收转发到https的服务器上，如果希望支持，可以设置为false；

changeOrigin：它表示是否更新代理后请求的headers中host地址；

#### changeOrigin的解析

这个changeOrigin官方说的非常模糊，通过查看源码我发现其实是要修改代理请求中的headers中的host属性：

因为我们真实的请求，其实是需要通过http://localhost:8888来请求的；

但是因为使用了代码，默认情况下它的值时http://localhost:8000；

如果我们需要修改，那么可以将changeOrigin设置为true即可

![image-20220418220549818](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418220549818.png)

### historyApiFallback

historyApiFallback是开发中一个非常常见的属性，它主要的作用是解决SPA页面在路由跳转之后，进行页面刷新时，返回404的错误。

boolean值：默认是false

如果设置为true，那么在刷新时，返回404错误时，会自动返回index.html的内容；

object类型的值，可以配置rewrites属性：

可以配置from来匹配路径，决定要跳转到哪一个页面；

事实上devServer中实现historyApiFallback功能是通过connect-history-api-fallback库的:

可以查看connect-history-api-fallback文档

### resolve模块解析

resolve用于设置模块如何被解析：

- 在开发中我们会有各种各样的模块依赖，这些模块可能来自于自己编写的代码，也可能来自第三方库；

- resolve可以帮助webpack从每个require/import 语句中，找到需要引入到合适的模块代码；

- webpack 使用enhanced-resolve 来解析文件路径；

webpack能解析三种文件路径：

- 绝对路径: 由于已经获得文件的绝对路径，因此不需要再做进一步解析。
- 相对路径
  - 在这种情况下，使用 import 或require 的资源文件所处的目录，被认为是上下文目录；
  - 在import/require 中给定的相对路径，会拼接此上下文路径，来生成模块的绝对路径；

模块路径

- 在resolve.modules中指定的所有目录检索模块，默认值是['node_modules']，所以默认会从node_modules中查找文件；

### 确定文件还是文件夹

如果是一个文件：

如果文件具有扩展名，则直接打包文件；

否则，将使用resolve.extensions选项作为文件扩展名解析；

如果是一个文件夹：

会在文件夹中根据resolve.mainFiles配置选项中指定的文件顺序查找；

- resolve.mainFiles的默认值是['index']；

- 再根据resolve.extensions来解析扩展名；

### extensions和alias配置

extensions是解析到文件时自动添加扩展名：

- 默认值是['.wasm', '.mjs', '.js', '.json']；

- 所以如果我们代码中想要添加加载.vue 或者jsx 或者ts等文件时，我们必须自己写上扩展名

另一个非常好用的功能是配置别名alias：

- 特别是当我们项目的目录结构比较深的时候，或者一个文件的路径可能需要../../../这种路径片段；
- 我们可以给某些常见的路径起一个别名；

![image-20220418222318394](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220418222318394.png)

## 环境分离和代码分离

### 如何区分开发环境

目前我们所有的webpack配置信息都是放到一个配置文件中的：webpack.config.js

当配置越来越多时，这个文件会变得越来越不容易维护；

并且某些配置是在开发环境需要使用的，某些配置是在生成环境需要使用的，当然某些配置是在开发和生成环境都会使用的；

所以，我们最好对配置进行划分，方便我们维护和管理；

那么，在启动时如何可以区分不同的配置呢？

- 方案一：编写两个不同的配置文件，开发和生成时，分别加载不同的配置文件即可；

- 方式二：使用相同的一个入口配置文件，通过设置参数来区分它们；

![image-20220420010250105](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420010250105.png)

### 入口文件解析

我们之前编写入口文件的规则是这样的：./src/index.js，但是如果我们的配置文件所在的位置变成了config 目录，我们是否应该变成../src/index.js呢？

如果我们这样编写，会发现是报错的，依然要写成./src/index.js；

这是因为入口文件其实是和另一个属性时有关的context；

context的作用是用于解析入口（entry point）和加载器（loader）：

官方说法：默认是当前路径（但是经过我测试，默认应该是webpack的启动目录）

另外推荐在配置中传入一个值；

![image-20220420012332046](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420012332046.png)

### 配置文件的分离

这里我们创建三个文件：

webpack.comm.conf.js
webpack.dev.conf.js
webpack.prod.conf.js

**webpack.common.js**

```js
const resolveApp = require("./paths");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const VueLoaderPlugin = require("vue-loader/lib/plugin");
const TerserPlugin = require("terser-webpack-plugin");

const { merge } = require("webpack-merge");

const prodConfig = require("./webpack.prod");
const devConfig = require("./webpack.dev");

const commonConfig = {
  entry: {
    main: "./src/main.js",
    index: "./src/index.js"
    // main: { import: "./src/main.js", dependOn: "shared" },
    // index: { import: "./src/index.js", dependOn: "shared" },
    // lodash: "lodash",
    // dayjs: "dayjs"
    // shared: ["lodash", "dayjs"]
  },
  output: {
    path: resolveApp("./build"),
    filename: "[name].bundle.js",
    chunkFilename: "[name].[hash:6].chunk.js"
  },
  resolve: {
    extensions: [".wasm", ".mjs", ".js", ".json", ".jsx", ".ts", ".vue"],
    alias: {
      "@": resolveApp("./src"),
      pages: resolveApp("./src/pages"),
    },
  },
  optimization: {
    // 对代码进行压缩相关的操作
    minimizer: [
      new TerserPlugin({
        extractComments: false,
      }),
    ],
    // natural: 使用自然数(不推荐),
    // named: 使用包所在目录作为name(在开发环境推荐)
    // deterministic: 生成id, 针对相同文件生成的id是不变
    // chunkIds: "deterministic",
    splitChunks: {
      // async异步导入
      // initial同步导入
      // all 异步/同步导入
      chunks: "all",
      // 最小尺寸: 如果拆分出来一个, 那么拆分出来的这个包的大小最小为minSize
      minSize: 20000,
      // 将大于maxSize的包, 拆分成不小于minSize的包
      maxSize: 20000,
      // minChunks表示引入的包, 至少被导入了几次
      minChunks: 1,
      cacheGroups: {
        vendor: {
          test: /[\\/]node_modules[\\/]/,
          filename: "[id]_vendors.js",
          // name: "vendor-chunks.js",
          priority: -10
        },
        // bar: {
        //   test: /bar_/,
        //   filename: "[id]_bar.js"
        // }
        default: {
          minChunks: 2,
          filename: "common_[id].js",
          priority: -20
        }
      }
    },
    // true/multiple
    // single
    // object: name
    runtimeChunk: {
      name: function(entrypoint) {
        return `why-${entrypoint.name}`
      }
    }
  },
  module: {
    rules: [
      {
        test: /\.jsx?$/i,
        use: "babel-loader",
      },
      {
        test: /\.vue$/i,
        use: "vue-loader",
      },
      {
        test: /\.css/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./index.html",
    }),
    new VueLoaderPlugin(),
  ],
};

module.exports = function(env) {
  const isProduction = env.production;
  process.env.NODE_ENV = isProduction ? "production" : "development";

  const config = isProduction ? prodConfig : devConfig;
  const mergeConfig = merge(commonConfig, config);

  return mergeConfig;
};

```

**webpack.dev.js**

```js
const resolveApp = require('./paths');
const ReactRefreshWebpackPlugin = require('@pmmmwh/react-refresh-webpack-plugin');

const isProduction = false;

console.log("加载devConfig配置文件");

module.exports = {
  mode: "development",
  devServer: {
    hot: true,
    hotOnly: true,
    compress: true,
    contentBase: resolveApp("./why"),
    watchContentBase: true,
    proxy: {
      "/why": {
        target: "http://localhost:8888",
        pathRewrite: {
          "^/why": ""
        },
        secure: false,
        changeOrigin: true
      }
    },
    historyApiFallback: {
      rewrites: [
        {from: /abc/, to: "/index.html"}
      ]
    }
  },
  plugins: [
    // 开发环境
    new ReactRefreshWebpackPlugin(),
  ]
}
```

**webpack.prod.js**

```js
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const isProduction = true;

module.exports = {
  mode: "production",
  plugins: [
    // 生成环境
    new CleanWebpackPlugin({}),
  ]
}
```

### 认识代码分离

代码分离（Code Splitting）是webpack一个非常重要的特性：

- 它主要的目的是将代码分离到不同的bundle中，之后我们可以按需加载，或者并行加载这些文件

- 比如默认情况下，所有的JavaScript代码（业务代码、第三方依赖、暂时没有用到的模块）在首页全部都加载，就会影响首页的加载速度

- 代码分离可以分出出更小的bundle，以及控制资源加载优先级，提供代码的加载性能

Webpack中常用的代码分离有三种：

入口起点：使用entry配置手动分离代码

防止重复：使用EntryDependencies或者SplitChunksPlugin去重和分离代码

动态导入：通过模块的内联函数调用来分离代码

#### 多入口起点

入口起点的含义非常简单，就是配置多入口：

比如配置一个index.js和main.js的入口；

他们分别有自己的代码逻辑

![image-20220420012925106](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420012925106.png)

####   EntryDependencies(入口依赖)

假如我们的index.js和main.js都依赖两个库：lodash、dayjs

如果我们单纯的进行入口分离，那么打包后的两个bunlde都有会有一份lodash和dayjs

事实上我们可以对他们进行共享

![image-20220420013121416](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420013121416.png)

#### SplitChunks

另外一种分包的模式是splitChunk，它是使用SplitChunksPlugin来实现的:

因为该插件webpack已经默认安装和集成，所以我们并不需要单独安装和直接使用该插件

只需要提供SplitChunksPlugin相关的配置信息即可

Webpack提供了SplitChunksPlugin默认的配置，我们也可以手动来修改它的配置:

- 比如默认配置中，chunks仅仅针对于异步（async）请求，我们可以设置为initial或者all

![image-20220420013233645](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420013233645.png)

#### SplitChunks自定义配置

当然，我们可以自定义更多配置，我们来了解几个非常关键的属性：

![image-20220420013306998](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420013306998.png)

![image-20220420013413784](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420013413784.png)

#### SplitChunks自定义配置解析

![image-20220420013446426](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420013446426.png)

![image-20220420013623006](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420013623006.png)

#### 动态导入(dynamic import)

另外一个代码拆分的方式是动态导入时，webpack提供了两种实现动态导入的方式：

- 第一种，使用ECMAScript中的import() 语法来完成，也是目前推荐的方式；
- 第二种，使用webpack遗留的require.ensure，目前已经不推荐使用

比如我们有一个模块bar.js：

该模块我们希望在代码运行过程中来加载它（比如判断一个条件成立时加载）；

因为我们并不确定这个模块中的代码一定会用到，所以最好拆分成一个独立的js文件；

这样可以保证不用到该内容时，浏览器不需要加载和处理该文件的js代码；

这个时候我们就可以使用动态导入

注意：使用动态导入bar.js：

在webpack中，通过动态导入获取到一个对象；

真正导出的内容，在该对象的default属性中，所以我们需要做一个简单的解构；

##### 动态导入的文件命名

动态导入的文件命名：

因为动态导入通常是一定会打包成独立的文件的，所以并不会再cacheGroups中进行配置；

那么它的命名我们通常会在output中，通过chunkFilename 属性来命名

![image-20220420013839861](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420013839861.png)

但是，你会发现默认情况下我们获取到的[name] 是和id的名称保持一致的

如果我们希望修改name的值，可以通过magic comments（魔法注释）的方式

![image-20220420014159022](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220420014159022.png)
