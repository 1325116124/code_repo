# bootstrap详解

## bootstrap官网

[Bootstrap中文网 (bootcss.com)](https://www.bootcss.com/)

```javascript
/*
    1.什么是Bootstrap?
    Bootstrap 是twitter公司推出的，专门用于开发响应式布局、移动设备优先的 WEB 框架。
    Bootstrap当前最新的版本的Bootstrap4, 但当下企业使用最多的是Bootstrap3

    2.Bootstrap3和4的区别
    2.1CSS预处理器不同, Bootstrap3采用Less, Bootstrap4采用SASS
    2.2格栅种类不同, Bootstrap3提供4种格栅, Bootstrap4提供5种格栅
    2.3使用单位不同, Bootstrap3使用px作为单位, Bootstrap4使用rem作为单位
    2.4内部布局方式不同, Bootstrap3使用float布局, Bootstrap4使用flexbox布局
    2.5 ... ...

    3.Bootstrap兼容性
    Bootstrap 的目标是在最新的桌面和移动浏览器上有最佳的表现，也就是说，在较老旧的浏览器上可能会导致某些组件表现出的样式有些不同，但是功能是完整的。
    IE8以上都能完美使用, IE8以下需要通过一些额外的配置来保证其完整性
*/
```

## bootstrap3模板

```html
<!--bootstrapd3的使用配置-->
<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
<!--  可以让部分国产浏览器默认采用高速渲染模式-->
  <meta name="renderer" content="webkit">
<!--  为了让IE浏览器运行最新的渲染模式下-->
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--  meta:vp是为了保证在移动端能够正常显示-->
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
  <title>Bootstrap 101 Template</title>
  <!-- Bootstrap -->
  <link rel="stylesheet" href="css/bootstrap.css">
  <!-- HTML5 shim 和 Respond.js 是为了让 IE8 支持 HTML5 元素和媒体查询（media queries）功能 -->
  <!-- 警告：通过 file:// 协议（就是直接将 html 页面拖拽到浏览器中）访问页面时 Respond.js 不起作用 -->
  <!--  导入respond.js文件的目的：是为了能够在IE8以及IE8以下的浏览器中使用媒体查询-->
  <!--导入html5shiv.js文件的目的, 是为了能够在IE8以及IE8以下的浏览器中使用H5新增的标签-->
  <!--
    [if xxx] ![endif]这个是IE中的条件注释, 只有在IE浏览器下才会执行
    以下代码的含义: 如果当前是IE9以下的浏览器, 那么就导入以下的两个JS文件
  -->
  <!--[if lt IE 9]>
  <script src="js/html5shiv.js"></script>
  <script src="js/respond.js"></script>
  <![endif]-->
</head>
<body>
<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
  <script src="js/jquery-1.12.4.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
  <script src="js/bootstrap.js"></script>
</body>
</html>
```

## bootstrap4模板

```html
<!doctype html>
<html lang="zh-CN">
<head>
  <!-- 必须的 meta 标签 -->
  <meta charset="utf-8">
  <!--  meta:vp是为了保证在移动端能够正常显示-->
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

  <!-- Bootstrap 的 CSS 文件 -->
  <link rel="stylesheet" href="css/bootstrap.css">
  <title>Hello, world!</title>

</head>
<body>
<h1>Hello, world!</h1>

<!-- JavaScript 文件是可选的。从以下两种建议中选择一个即可！ -->
<!-- jQuery (Bootstrap 的所有 JavaScript 插件都依赖 jQuery，所以必须放在前边) -->
<script src="js/jquery-3.1.1.js"></script>
<!--在Bootstrap4中很多的提示/弹窗都是通过popper.min.js实现的, 所以需要导入-->
<script src="js/popper.js"></script>
<!-- 加载 Bootstrap 的所有 JavaScript 插件。你也可以根据需要只加载单个插件。 -->
<script src="js/bootstrap.js"></script>

</body>
</html>
```

## bootstrap容器

bootstrap容器分为两种：container和container-fluid

```html
<!--
    1.Bootstrap容器
    1.1在Bootstrap中容器是响应式布局的基础, Bootstrap推荐将所有内容都定义在容器之中
    1.2并且容器是启用Bootstrap栅格系统必不可少的前置条件

    2. Bootstrap中提供了两个容器: container/container-fluid
    2.1container容器(固定容器):
    只要给标签添加了container类名, 这个标签就变成了Bootstrap的container容器
    只要这个标签变成了Bootstrap的container容器, 在不同视口大小下就会有不同的固定宽度

    2.2container-fluid(自适应宽度容器)
    只要给标签添加了container-fluid类名, 这个标签就变成了Bootstrap的container-fluid容器
    只要这个标签变成了Bootstrap的container-fluid容器, 那么容器的宽度永远都是100%自适应

    3.Bootstrap对视口划分
    Bootstrap4将视口划分为了5钟
    超小屏幕(xs): <576px
    小屏幕(sm): >=576px
    中等屏幕(md):>=768
    大屏幕(lg): >=992px
    超大屏幕(xl): >= 1200px
-->
```

**下面是container-fluid的实现**

```css
.container-fluid {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}
#设置宽度为100%即可
```

**下面是container的实现**

```css
.container {
  width: 100%;
  padding-right: 15px;
  padding-left: 15px;
  margin-right: auto;
  margin-left: auto;
}

@media (min-width: 576px) {
  .container {
    max-width: 540px;
  }
}

@media (min-width: 768px) {
  .container {
    max-width: 720px;
  }
}

@media (min-width: 992px) {
  .container {
    max-width: 960px;
  }
}

@media (min-width: 1200px) {
  .container {
    max-width: 1140px;
  }
}
#通过媒体查询进行响应式布局
```

## bootstrap的栅格系统

```html
<!--
    1.Bootstrap栅格系统
    Bootstrap的栅格系统使用一系列的"行"和"列"来实现复杂的响应式布局
    默认情况下栅格系统会将一行的内容等分为12份,
    然后我们可以通过绑定类名的方式指定这一行中的每一列占用多少分

    2.Bootstrap栅格系统格式
    <容器>
        <行>
            <列>我们的内容</列>
            <列>我们的内容</列>
            ... ...
        </行>
    </容器>

   3.Bootstrap栅格系统特点
   3.1默认情况下行的宽度和所在容器一样
   3.2默认情况下所有列的宽度是等分所在行的宽度
   3.3可以通过col-num方式指定当前列占用12分之几
   3.4如果一行中内容的宽度超过了12分, 那么将自动换行
-->
<div class="container">
  <div class="row">
    <div class="col-6">我是第一列</div>
    <div class="col-4">我是第二列</div>
    <div class="col-8">我是第三列</div>
  </div>
</div>
```

## boostrap栅格系统的实现原理

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
      .container {
          width: 100%;
          padding-right: 15px;
          padding-left: 15px;
          margin-right: auto;
          margin-left: auto;
      }

      @media (min-width: 576px) {
          .container {
              max-width: 540px;
          }
      }

      @media (min-width: 768px) {
          .container {
              max-width: 720px;
          }
      }

      @media (min-width: 992px) {
          .container {
              max-width: 960px;
          }
      }

      @media (min-width: 1200px) {
          .container {
              max-width: 1140px;
          }
      }
      div{
          height: 100px;
          background-color: red;
      }
      .row{
          display: flex;
          flex-wrap: wrap;
      }
      .container>.row{
          height: 100%;
          background-color: blue;
          margin-left: -15px;
          margin-right: -15px;
      }
      .row>div{
          background-color: skyblue;
      }
      .row>div:nth-child(2){
          background-color: pink;
      }
      .row>div:nth-child(3){
          background-color: green;
      }
      .col{
          flex: 1;
      }
      .col-6{
          flex-basis: 50%;
      }
  </style>
</head>
<body>
<div class="container">
  <div class="row">
    <div class="col-6">我是第一列</div>
    <div class="col">我是第二列</div>
    <div class="col">我是第三列</div>
  </div>
</div>
</body>
</html>
```

##  栅格系统列的尺寸设置

```html
<!--
    1.栅格系统列的尺寸设置
    1.1Bootstrap的固定宽度容器提供了5种响应的尺寸,
       同样Bootstrap也给列提供了5种响应的尺寸
    col-*:   设置超小屏幕
    col-sm-*:设置小屏幕
    col-md-*:设置中等屏幕
    col-lg-*:设置大屏幕
    col-xl-*:设置超大屏幕

    2.栅格系统列的尺寸特点
    1.如果只设置了小屏幕的大小, 那么大屏幕也会采用小屏幕设置的大小
    2.如果值设置了大屏幕的大小, 那么小屏幕默认100%
    3.如果大小屏幕都设置了大小, 那么在什么屏幕就显示什么尺寸
-->
<div class="container">
  <div class="row">
    <div class="col-lg-2 col-xl-6">我是第一列</div>
    <div class="col-lg-4 col-xl-4">我是第二列</div>
    <div class="col-lg-6 col-xl-2">我是第三列</div>
  </div>
</div>
```

## 栅格系统沟槽和对齐方式

```html
<!--
    1.栅格系统-沟槽
    BootStrap默认的栅格和列间有间隙沟槽，一般是左右-15px的margin或padding处理，您可以使用.no-gutters类来消除它，这将影响到.row行、列平行间隙及所有子列

    2.栅格系统列-对齐方式
    Bootstrap4的格栅系统是使用伸缩布局实现的, 所以也可以通过类名快速的设置伸缩项的在主轴和侧轴对齐方式
-->
<!-- 去除行的沟槽，no-gutters，去除容器的margin，px-0 -->
<!--<div class="container px-0">-->
<!--  <div class="row no-gutters">-->
<!--    <div class="col-6">我是第一列</div>-->
<!--    <div class="col-2">我是第二列</div>-->
<!--    <div class="col-2">我是第三列</div>-->
<!--  </div>-->
<!--</div>-->
<div class="container">
  <div class="row align-items-center">
    <div class="col-6">我是第一列</div>
    <div class="col-2">我是第二列</div>
    <div class="col-2">我是第三列</div>
  </div>
</div>
```

## 栅格系统的偏移和排序

```html
<!--
    1.栅格系统-列偏移
    offset-*: 但前列偏移多少份位置
    注意点: 写在那一列就是那一列偏移

    2.栅格系统列-列排序
    order-*: 从小到大顺序排序, 小的在前面, 大的在后面
    注意点: 没有排序的列位置不会发生变化, 只有排序的列才参与位置变化
-->
<div class="container">
  <div class="row justify-content-center">
    <div class="col-6">我是第一列</div>
    <div class="col-2">我是第二列</div>
    <div class="col-2">我是第三列</div>
  </div>
</div>
<div class="container" style="margin-top: 20px">
  <div class="row">
    <div class="col-6 offset-1">我是第一列</div>
    <div class="col-2">我是第二列</div>
    <div class="col-2">我是第三列</div>
  </div>
</div>
<div class="container">
  <div class="row">
    <div class="col-6 order-3">我是第一列</div>
    <div class="col-2 order-2">我是第二列</div>
    <div class="col-2 order-1">我是第三列</div>
  </div>
</div>
```

## 公共样式

```html
<!--
	查文档即可
    1.文字颜色
    柔和灰（text-muted）
    主要蓝（text-primary）
    次要灰（text-secondary）
    成功绿（text-success）
    危险红（text-danger）
    警告黄（text-warning）
    危息绿（text-info）
    黑白对比（text-dark）
    注意点: .text-white 、 .text-muted这两个样式不支持链接上使用
-->
<p class="text-success">知播渔</p>
<p class="text-danger">知播渔</p>
<p class="text-warning">知播渔</p>
<!--
    2.背景颜色
    主要蓝（bg-primary）
    次要灰（bg-secondary）
    成功绿（bg-success）
    危险红（bg-danger）
    警告黄（bg-warning）
    危息绿（bg-info）
    黑白对比（bg-dark）
-->
<p class="bg-success">知播渔</p>
<p class="bg-danger">知播渔</p>
<p class="bg-warning">知播渔</p>

<!--
    3.边框相关
    3.1快速添加边框
    border / border-*
    3.2快速删除边框
    border-0 / border-*-0
    3.3快速指定边框颜色
    border-color
    3.4快速添加边框圆角
    rounded / rounded-*
-->
<!--<div class="border-top border-left border-warning"></div>-->
<div class="border border-warning rounded-circle"></div>



<!--
    1.显示模式
    d-*
    d-屏幕尺寸-*
-->
<div class="bg-success d-inline d-sm-inline-block d-md-block">我是div</div>

<!--
    2.浮动和清除浮动
    float-*
    float-屏幕尺寸-*
    3.清除浮动
    clearfix
-->
<div class="bg-success clearfix">
    <div class="float-left">左边</div>
    <div class="float-right">右边</div>
</div>

<!--
    3.定位
    position-*
-->
<div class="position-relative">我是div</div>

<!--
    1.在Bootstrap中可以通过m-* / p-* 快速添加间隙
    2.在Bootstrap中可以通过 list-unstyled 快速去除项目符号
    3.在Bootstrap中可以通过 img-fluid快速设置等比拉伸图片
    4.在Bootstrap中可以通过 img-thumbnail快速设置缩略图样式
-->
<div class="bg-success m-auto" style="width: 200px; height: 200px"></div>
<div class="bg-success pt-5" style="width: 200px; height: 200px">我是div</div>

<ol class="list-unstyled">
    <li>我是第1个li</li>
    <li>我是第2个li</li>
    <li>我是第3个li</li>
</ol>

<img src="images/img1.jpg" class="img-fluid">
```

## 组件

### 提示框

```html
<!--
    1.什么是role?
    role属性用于增强语义性, 当现有的HTML标签不能充分表达语义性的时候，就可以借助role来说明
    比如用div做button，那么设置div 的 role=“button”，辅助工具就可以认出这实际上是个button

    2.什么是aria-xxx?
    aria全称: Accessible Rich Internet Applications
    这一套协定是w3和apple为了"残疾人士"无障碍使用网站的

    3.什么是aria-hidden?
    为了避免屏幕识读设备抓取非故意的和可能产生混淆的输出内容（尤其是当图标纯粹作为装饰用途时），我们为这些图标设置了 aria-hidden=“true” 属性
-->
<!-- 如果要改变内置的css样式，可以先找到对应的样式名，再覆写即可，或者用多类选择器，可以不影响其他的样式 -->
 /*.alert.test{
       padding: 0;
    }*/
<div class="alert alert-success" role="alert">
    A simple primary alert—check it out!
</div>

<div class="alert alert-warning alert-dismissible fade show" role="alert">
    <img src="images/ad.jpg" alt="">
    <!--
    data-dismiss="alert" 告诉button需要关闭谁
    -->
    <button type="button" class="close" data-dismiss="alert">
        <span>&times;</span>
    </button>
</div>
```

### 按钮和按钮组

```html
<button type="button" class="btn btn-primary">Primary</button>
<input type="button" value="我是按钮" class="btn btn-success">
<a href="#" class="btn btn-danger">我是按钮</a>

<div class="btn-group">
  <button type="button" class="btn btn-secondary">1</button>
  <button type="button" class="btn btn-secondary">2</button>
  <button type="button" class="btn btn-secondary">3</button>
</div>
```

### 卡片组件

```html
<div class="card" style="width: 18rem;">
  <img src="images/img1.jpg" class="card-img-top">
  <div class="card-body">
    <h5 class="card-title">Card title</h5>
    <p class="card-text">Some quick example text to build on the card title and make up the bulk of the card's content.</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </div>
</div>
```

### 轮播图

```html
<!--
class="carousel slide" 创建一个轮播组件
data-ride="carousel" 自动轮播
-->
<div id="carouselExampleCaptions" class="carousel slide" data-ride="carousel">
  <!--
  1.class="carousel-indicators" 创建指示器
  2.data-target="#carouselExampleCaptions" 告诉bootstrap当前的索引属于哪一个轮播图
  3.data-slide-to="0" 指定当前指示器的索引
  4.class="active" 设置默认选中的指示器索引
  -->
  <ol class="carousel-indicators">
    <li data-target="#carouselExampleCaptions" data-slide-to="0" class="active"></li>
    <li data-target="#carouselExampleCaptions" data-slide-to="1"></li>
    <li data-target="#carouselExampleCaptions" data-slide-to="2"></li>
  </ol>
  <!--
  1.class="carousel-inner" 创建轮播图的内容部分
  2.class="carousel-item" 创建轮播图中的每一页
  -->
  <div class="carousel-inner">
    <div class="carousel-item active">
      <img src="images/img1.jpg" class="d-block w-100" alt="...">
      <div class="carousel-caption d-none d-md-block">
        <h5>First slide label</h5>
        <p>Some representative placeholder content for the first slide.</p>
      </div>
    </div>
    <div class="carousel-item">
      <img src="images/img2.jpg" class="d-block w-100" alt="...">
      <div class="carousel-caption d-none d-md-block">
        <h5>Second slide label</h5>
        <p>Some representative placeholder content for the second slide.</p>
      </div>
    </div>
    <div class="carousel-item">
      <img src="images/img3.jpg" class="d-block w-100" alt="...">
      <div class="carousel-caption d-none d-md-block">
        <h5>Third slide label</h5>
        <p>Some representative placeholder content for the third slide.</p>
      </div>
    </div>
  </div>

  <!--
  1.class="carousel-control-prev" 创建上一页的控制按钮
  2.data-target="#carouselExampleCaptions" 指定控制按钮可用来控制哪一个轮播图
  -->
  <button class="carousel-control-prev" type="button" data-target="#carouselExampleCaptions"  data-slide="prev">
    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
    <span class="sr-only">Previous</span>
  </button>
  <button class="carousel-control-next" type="button" data-target="#carouselExampleCaptions"  data-slide="next">
    <span class="carousel-control-next-icon" aria-hidden="true"></span>
    <span class="sr-only">Next</span>
  </button>
</div>
```

### 折叠面板

```html
<!--
1.data-toggle="collapse" 告诉bootstrap需要切换的是折叠面板
2.href="#collapseExample" 指定控制的对象
-->
<a class="btn btn-primary" data-toggle="collapse" href="#collapseExample" role="button" aria-expanded="false" aria-controls="collapseExample">
  Link with href
</a>
<!--
1.class="collapse" 创建一个折叠面板
注意点：折叠面板默认情况下是不会显示的
-->
<div class="collapse" id="collapseExample">
  <div class="card card-body">
    Some placeholder content for the collapse component. This panel is hidden by default but revealed when the user activates the relevant trigger.
  </div>
</div>
```

### 下拉菜单

```html
<!--
1.class="dropdown" 创建一个下拉菜单的容器
-->
<div class="dropdown">
  <!--
  1.dropdown-toggle：指定菜单按钮的小箭头
  2.data-toggle="dropdown"：告诉bootstrap需要切换的是下拉菜单
  -->
  <button class="btn btn-secondary" type="button" data-toggle="dropdown">
    Dropdown button
  </button>
  <!--
  1.class="dropdown-menu" 创建一个弹出的下拉菜单
  2.class="dropdown-item" 指定菜单项
  -->
  <div class="dropdown-menu" aria-labelledby="dropdownMenuButton">
    <a class="dropdown-item" href="#">Action</a>
    <a class="dropdown-item" href="#">Another action</a>
    <a class="dropdown-item" href="#">Something else here</a>
  </div>
</div>
```

### 模态弹窗组件

```html
<!--
data-toggle="modal": 告诉bootstrap需要切换的是模态弹窗
data-target="#exampleModal":指定控制的模态弹窗
-->
<button class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
  我是按钮
</button>

<!--
class="modal fade" 创建模态弹窗的容器
-->
<div class="modal fade" id="exampleModal" tabindex="-1">
  <!--
  class="modal-dialog" 创建真正弹出的内容
  -->
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Modal title</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
          <span aria-hidden="true">&times;</span>
        </button>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div>
  </div>
</div>
```

### 提示气泡

**(1)**

```html
<!--
1.data-toggle="tooltip":告诉bootstrap需要切换的是提示气泡
2.data-placement="top":告诉bootstrap提示气泡出现的位置
3.title="Tooltip on top":告诉bootstrap提示气泡中提示的内容是什么
-->
<button class="btn btn-secondary" data-toggle="tooltip" data-placement="top" title="Tooltip on top">
  我是按钮
</button>
<script>
  $(function () {
    $('[data-toggle="tooltip"]').tooltip()
  })
</script>
```

**(2)**

```html
<!--
    1.data-container="body": 当你在一个父元素上有一些样式与提示框产生样式干扰，你可以指定一个自定义的container容器，这样提示框的HTML将出现在这个元素内部了。
    2.data-toggle="popover": 告诉bootstrap需要切换的是提示气泡
    3.data-placement="top": 告诉bootstrap提示气泡出现的位置
    4.data-content="": 告诉bootstrap提示气泡中提示的内容是什么
    -->
<button class="btn btn-secondary" data-container="body" data-toggle="popover" data-placement="right" data-content="我是内容我是内容">
  我是按钮
</button>
<script>
    $('button').popover()
</script>
```

### 导航栏

```html
<!--
1.class="navbar": 告诉Bootstrap需要创建一个响应式的导航栏
2.class="navbar-expand-lg": 告诉Bootstrap在哪一种屏幕尺寸上需要展示完整的导航栏
3.class="navbar-light bg-light": 设置导航栏的主题样式
-->
<nav class="navbar navbar-expand-xl navbar-light bg-light">
    <!--
    1.class="navbar-brand": 指定导航栏中公司的品牌信息(公司名称/LOGO)
    -->
    <a class="navbar-brand" href="#">知播渔</a>
    <!--
    1.class="navbar-toggler": 创建一个移动端的菜单按钮
    2.data-toggle="collapse": 告诉bootstrap需要切换的是折叠面板
    3.data-target="#navbarSupportedContent": 告诉bootstrap需要切换的是哪一个折叠面板
    4.class="navbar-toggler-icon": 设置按钮的字体图标
    -->
    <button class="navbar-toggler"  data-toggle="collapse" data-target="#navbarSupportedContent">
        <span class="navbar-toggler-icon"></span>
    </button>
    <!--
    1.class="collapse": 创建一个折叠面板
    注意点: 折叠面板默认情况下是不会显示的
    2.class="navbar-collapse":用于通过父断点进行分组和隐藏导航列内容
    -->
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <!--
        1. class="navbar-nav": 创建导航栏的菜单
        2. class="nav-item": 创建导航来的菜单项
        -->
        <ul class="navbar-nav mr-auto">
            <li class="nav-item">
                <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
            </li>
            <li class="nav-item active">
                <a class="nav-link" href="#">Link</a>
            </li>
            <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                    Dropdown
                </a>
                <div class="dropdown-menu" aria-labelledby="navbarDropdown">
                    <a class="dropdown-item" href="#">Action</a>
                    <a class="dropdown-item" href="#">Another action</a>
                    <div class="dropdown-divider"></div>
                    <a class="dropdown-item" href="#">Something else here</a>
                </div>
            </li>
            <li class="nav-item">
                <a class="nav-link disabled" href="#">Disabled</a>
            </li>
        </ul>
    </div>
</nav>
```

## bootstrap配合media使用

```html
<!-- 语法：@media screen and (min-width/max-width:~){} -->
<style>
    div{
        @media screen and (min-width: 1450px){
        width: 153px;
        height: 34px;
        background-position: -29px -26px;
      }
      @media screen and (max-width: 1199px){
        width: 153px;
        height: 34px;
        background-position: -29px -26px;
      }
    }
</style>
```

