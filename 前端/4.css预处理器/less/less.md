# Less

## Less初体验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-less初体验</title>

    <!--<style type="text/css">
        *{
            margin: 0;
            padding: 0;
        }
        .a{
            width: 300px;
            height: 300px;
            background: red;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
        .a>.b{
            width: 200px;
            height: 200px;
            background: blue;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
    </style>-->
    <<!--style type="text/less">
        *{
            margin: 0;
            padding: 0;
        }
        .center{
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
        .a{
            width: 300px;
            height: 300px;
            background: red;
            .center;
            .b{
                width: 200px;
                height: 200px;
                background: blue;
                .center;
            }
        }
    </style>-->
    <!--<link rel="stylesheet/less" href="css/index.less">
    <script src="js/less.js"></script>-->
    <link rel="stylesheet" href="css/index.css">
</head>
<body>
<div class="a">
    <div class="b"></div>
</div>
<script>
    /*
    1.什么是CSS预处理器?
    CSS 预处理器就是用某一种语言用来为 CSS 增加一些动态语言的的特性(变量、函数、继承等)，
    CSS预处理器可以让你的 CSS 更见简洁，适应性更强，代码更直观等诸多好处
    简而言之: CSS预处理器就是升级版CSS

    2.常见的CSS预处理器
    Less、 Sass 、Stylus
    */
    /*
   1.为什么需要less?
   1.1CSS的语法虽然简单, 但它同时也带来一些问题
   1.2CSS需要书写大量看似没有逻辑的代码, 不方便维护及扩展, 也不利于复用,
   1.3造成这些原因的本质源于CSS是一门非程序式的语言, 没有变量/函数/作用域等概念

   2.什么是less(Leaner Style Sheets)?
   2.1Less 是一门 CSS 预处理语言，为CSS赋予了动态语言的特征。
   2.2它扩展了 CSS 语言，增加了变量、Mixin(混合)、嵌套、函数和运算等特性，使 CSS 更易维护和扩展
   2.3一句话：用类似JS的语法去写CSS
   */
    /*
   4.less基本使用:
   4.1在浏览器中直接运行
   编写less文件-->引入less文件-->引入less.js-->运行
   注意点:
   一定要先引入less.css再引入less.js
   如果less代码是写到单独的文件中, 一定要在服务端环境运行才能生效

   4.2提前预编译
   编写less文件-->利用工具转换为css文件-->引入css文件
   考拉客户端: http://koala-app.com/index-zh.html
   开源中国  : https://tool.oschina.net/less
   构建工具配置loader自动编译: 后续课程内容
   注意点:
   无需引入less.js, 无需在服务端运行
    */
</script>
</body>
</html>
```

## Less中的注释

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-less中的注释</title>
    <link rel="stylesheet" href="css/02-less中的注释.css">
</head>
<body>
<div></div>
<script>
    /*
    less中的注释和JS中的注释一样, 也有单行注释和多行注释
    less中单行注释和多行注释最大的区别在于: 是否会被编译
    单行注释不会被编译(不会出现在编译后的文件中)
    多行注释会被编译  (会出现在编译后的文件中)
    */
</script>
</body>
</html>
```

```less
// 这里是单行注释
/*
这里是多行注释
*/
div{
  width: 200px;
  height: 200px;
  background: red;
}
```

## Less中的变量

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-less中的变量</title>
    <link rel="stylesheet" href="css/03-less中的变量.css">
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
</body>
</html>
```

```less
/*
1.什么是变量?
和js中的概念基本一样
*/
/*
2.less中定义变量的格式
@变量名称: 值;
3.less中使用变量的格式
@变量名称;
*/
@w: 400px;
//@h: 200px;
/*
4.和js一样可以将一个变量赋值给另外一个变量
@变量名称 : @变量名称;
*/
@h: @w;
/*
5.和js一样less中的变量也有全局变量和局部变量
定义在{}外面的就是全局的变量, 什么地方都可以使用
定义在{}里面的就是局部变量, 只能在{}中使用

注意定: less中的变量是延迟加载的, 写到后面也能在前面使用
*/
//@c: blue;
/*
6.和js一样不同作用域的变量不会相互影响, 只有相同作用域的变量才会相互影响
  和js一样在访问变量时会采用就近原则
*/
@c: red;
.box1{
  @c: blue;
  width: @w;
  height: @h;
  background: @c;
  margin-bottom: 20px;
  @c: pink;
}
.box2{
  width: @w;
  height: @h;
  background: @c;
}
@c: red;
```

## less变量插值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-less变量插值</title>
    <link rel="stylesheet" href="css/04-less变量插值.css">
</head>
<body>
<div></div>
</body>
</html>
```

```less
@size: 200px;
@w: width;
@s: div;
/*
1.什么是变量插值?
在less中如果属性的取值可以直接使用变量, 但是如果是属性名称或者选择器名称并不能直接使用变量
如果属性名称或者选择器名称想使用变量中保存的值, 那么必须使用变量插值的格式

2.变量插值的格式
格式: @{变量名称}
*/
@{s}{
  @{w}: @size;
  height: @size;
  background: red;
}
```

## less中的运算

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-less中的运算</title>

    <!--
    <style>
        div{
            width: 200px;
            height: 200px;
            background: blue;
            position: absolute;
            left: 50%;
            /*以下的方式不利于编码, 因为需要我们自己口算元素宽度的一半是多少*/
            /*margin-left: -100px;*/
            /*以下的方式不利于兼容, 因为transform属性是CSS3新增的, 只有支持C3属性的浏览器才可以使用*/
            /*transform: translateX(-50%);*/
            /*在CSS3中新增了一个calc函数, 可以实现简单的+ - * / 运算*/
            /*margin-left: calc(-200px / 2);*/
            margin-left: calc(-200px * 0.5);
        }
    </style>
    -->
    <link rel="stylesheet" href="css/05-less中的运算.css">
</head>
<body>
<div></div>
</body>
</html>
```

```less
div{
  width: 200px;
  height: 200px;
  background: blue;
  position: absolute;
  left: 50%;
  /*less中的运算和CSS3中新增的calc函数一样, 都支持+ - * / 运算*/
  //margin-left: (-200px * 0.5);
  margin-left: (-200px / 2);
}
```

## less中的混合

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-less中的混合</title>
    <link rel="stylesheet" href="css/06-less中的混合.css">
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
</body>
</html>
```

```less
/*
2.less中混合的注意点:
如果混合名称的后面没有(), 那么在预处理的时候, 会保留混合的代码
如果混合名称的后面加上(), 那么在预处理的时候, 不会保留混合的代码
 */
/*
.center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
*/
.center(){
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
/*
1.什么是less中的混合(Mix in)?
将需要重复使用的代码封装到一个类中, 在需要使用的地方调用封装好的类即可
在预处理的时候less会自动将用到的封装好的类中的代码拷贝过来
本质就是ctrl+c  --> ctrl + v
 */
.father{
  width: 300px;
  height: 300px;
  background: red;
  .center();
  .son{
    width: 200px;
    height: 200px;
    background: blue;
    .center();
  }
}
```

## less中带参数混合

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>07-less中带参数混合</title>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
</body>
</html>
```

```less
// 这里就是带参数的混合
/*
.whc(@w, @h, @c){
  width: @w;
  height: @h;
  background: @c;
}
 */
// 这里就是带参数的混合, 并且带默认值
.whc(@w:100px, @h:100px, @c:pink){
  width: @w;
  height: @h;
  background: @c;
}
.box1{
  //width: 200px;
  //height: 200px;
  //background: red;
  //.whc(200px, 200px, red);

  // 这里是给混合的指定形参传递数据
  .whc(@c:red);
}
.box2{
  //width: 300px;
  //height: 300px;
  //background: blue;
  .whc(300px, 300px, blue);
}
```

## less中的可变参数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08-less中的可变参数</title>
</head>
<body>
<div></div>
</body>
</html>
```

```less
/*
.animate(@name, @time, @mode, @delay){
  //transition: @name @time @mode @delay;
  transition: @arguments;
}
 */
/*
.animate(...){
  //transition: @name @time @mode @delay;
  transition: @arguments;
}
*/
/*
.animate(@name, ...){
  //transition: @name @time @mode @delay;
  transition: @arguments;
}
 */
/*
less中的@arguments和js中的arguments一样, 可以拿到传递进来的所有形参
*/
/*
less中的...表示可以接收0个或多个参数
如果形参列表中使用了..., 那么...必须写在形参列表最后
*/
.animate(@name, @time, ...){
  //transition: @name @time @mode @delay;
  transition: @arguments;
}
div{
  width: 200px;
  height: 200px;
  background: red;
  //transition: all 4s linear 0s;
  //.animate(all, 4s, linear, 0s);
  .animate(all, 4s, linear, 0s);
}
div:hover{
  width: 400px;
  height: 400px;
  background: blue;
}
```

## less中的匹配模式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09-less中的匹配模式</title>

    <!--
    <style>
        div{
            width: 0;
            height: 0;
            border-width: 40px 40px 40px 40px;
            border-style: solid solid solid solid;
            /*border-color: #000 #f00 #0f0 #00f;*/
            border-color: #000 transparent transparent transparent;
        }
    </style>
    -->
    <link rel="stylesheet" href="css/09-less中的匹配模式.css">
</head>
<body>
<div></div>
</body>
</html>
```

```less
/*
@_: 表示通用的匹配模式

什么是通用的匹配模式?
无论同名的哪一个混合被匹配了, 都会先执行通用匹配模式中的代码
*/
.triangle(@_, @width, @color){
  width: 0;
  height: 0;
  border-style: solid solid solid solid;
}
.triangle(Down, @width, @color){
  //width: 0;
  //height: 0;
  border-width: @width;
  //border-style: solid solid solid solid;
  border-color: @color transparent transparent transparent;
}
.triangle(Top, @width, @color){
  //width: 0;
  //height: 0;
  border-width: @width;
  //border-style: solid solid solid solid;
  border-color: transparent transparent @color transparent;
}
.triangle(Left, @width, @color){
  //width: 0;
  //height: 0;
  border-width: @width;
  //border-style: solid solid solid solid;
  border-color: transparent @color transparent transparent;
}
.triangle(Right, @width, @color){
  //width: 0;
  //height: 0;
  border-width: @width;
  //border-style: solid solid solid solid;
  border-color: transparent transparent transparent @color;
}
div{
  /*
  混合的匹配模式
  就是通过混合的第一个字符串形参,来确定具体要执行哪一个同名混合
  */
  //.triangle(Down, 80px, green);
  //.triangle(Top, 80px, green);
  //.triangle(Left, 80px, green);
  .triangle(Right, 80px, green);
}
```

## less导入其他的less文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-less导入其它less文件</title>
    <link rel="stylesheet" href="css/10-less导入其它less文件.css">

</head>
<body>
<div></div>
</body>
</html>
```

```less
//@import "triangle.less";
@import "triangle";

div{
  .triangle(Down, 40px, red);
}
```

## less中其他的内置函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>11-less中的内置函数</title>
    <link rel="stylesheet" href="css/11-less中的内置函数.css">
</head>
<body>
<div></div>
<script>
    /*
    由于less的底层就是用JavaScript实现的,
    所以JavaScript中常用的一些函数在less中都支持
    */
    // 混杂方法
    /*
    image-size("file.jpg"); // => 100px 50px
    image-width("file.jpg"); // => 100px
    image-height("file.jpg"); // => 50px

    // 单位转换
    convert(9s, "ms"); // => 9000ms
    convert(14cm, mm); // => 140mm
    convert(8, mm); // => 8

    // 列表
    @list: "A", "B", C, "D";
    length(@list); // => 4
    extract(@list, 3); // => C
    */
    // 数学
    /*
    ceil(2.1); // => 3 向上取整
    floor(2.1); // => 2 向下取整
    percentage(.3); // => 30% 转百分比
    round(1.67, 1); // => 1.7 四舍五入，保留一位小数点
    sqrt(25cm); // => 5cm 取平方根
    abs(-5cm); // => 5cm 取绝对值
    pi(); // => 3.141592653589793 圆周率π
    max(3px, 42px, 1px, 16px); // => 42px
    min(3px, 42px, 1px, 16px); // => 1px
    */
    // 字符串
    /*
    replace("Hi Tom?", "Tom", "Jack"); // => "Hi Jack"
    */
    // 判断类型
    /*
    isnumber(56px); // => true 是否含数字
    isstring("string"); // => true
    iscolor(#ff0); // => true
    iscolor(blue); // => true
    iskeyword(keyword); // => true
    isurl(url(...)); // => true
    ispixel(56px); // => true
    isem(7.8em); // => true
    ispercentage(7.8%); // => true
    isunit(4rem, rem); // => true 是否为指定单位
    isruleset(@rules); // => true 是否为变量
    */
    // 颜色操作
    /*
    增加饱和度
    saturate(color, 20%)
    减少饱和度
    desaturate(color, 20%)
    增加亮度
    lighten(color, 20%)
    减少亮度
    darken(color, 20%)
    降低透明度
    fadein(color, 10%)
    增加透明度
    fadeout(color, 10%)
    设置绝对不透明度(覆盖原透明度)
    fade(color, 20%)
    旋转色调角度
    spin(color, 10)
    将两种颜色混合，不透明度包括在计算中。
    mix(#f00, #00f, 50%)
    与白色混合
    tint(#007fff, 50%)
    与黑色混合
    shade(#007fff, 50%)
    灰度，移除饱和度
    greyscale(color)
    返回对比度最大的颜色
    contrast(color1, color2)
    */
    // 颜色混合
    /*
    每个RGB 通道相乘，然后除以255
    multiply(color1, color2);
    与 multiply 相反
    screen(color1, color2);
    使之更浅或更暗
    overlay(color1, color2)
    避免太亮或太暗
    softlight(color1, color2)
    与overlay相同，但颜色互换
    hardlight(color1, color2)
    计算每个通道(RGB)基础上的两种颜色的平均值
    average(color1, color2)
    */

</script>
</body>
</html>
```

```less
@str: "./../images/1.jpg";
@str2: replace(@str, "1", "2");
div{
  width: 200px;
  height: 200px;
  //background: url(@str2);
  background: desaturate(yellow, 50%);
}
div:hover{
  background: saturate(yellow, 50%);
}
```

## less中的层级结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-less中的层级结构</title>
    <link rel="stylesheet" href="css/12-less中的层级结构.css">
</head>
<body>
<div class="father">
<!--    <div class="son"></div>-->
</div>
</body>
</html>
```

```less
.father{
  width: 300px;
  height: 300px;
  background: red;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  /*如果在某一个选择器的{}中直接写上了其它的选择器, 会自动转换成后代选择器
    例如以下代码: .father .son
  */
  /*
  .son{
    // 这里的&的作用, 是告诉less在转换的时候不用用后代来转换, 直接拼接在当前选择器的后面即可
    &:hover{
      background: skyblue;
    }
    width: 200px;
    height: 200px;
    background: blue;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
   */

  &::before{
    content: "子元素";
    display: block;
    background: yellowgreen;
    width: 100px;
    height: 100px;
  }
}
/*
.son:hover{
  background: skyblue;
}
 */
```

## less中的继承

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13-less中的继承</title>
    <link rel="stylesheet" href="css/13-less中的继承.css">
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
</body>
</html>
```

```less
.center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
/*
1.less中的继承和less中混合的区别
1.1使用时的语法格式不同
1.2转换之后的结果不同(混合是直接拷贝, 继承是并集选择器)
*/
.father:extend(.center){
  width: 300px;
  height: 300px;
  background: red;
  //.center;
  .son:extend(.center){
    width: 200px;
    height: 200px;
    background: blue;
    //.center;
  }
}
```

## less中的条件判断

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>14-less中的条件判断</title>
    <link rel="stylesheet" href="css/14-less中的条件判断.css">
</head>
<body>
<div></div>
</body>
</html>
```

```less
/*
less中可以通过when给混合添加执行限定条件, 只有条件满足(为真)才会执行混合中的代码
when表达式中可以使用比较运算符(> < >= <= =)、逻辑运算符、或检查函数来进行条件判断
*/
/*
.size(@width,@height) when (@width = 100px){
  width: @width;
  height: @height;
}
 */
// (),()相当于JS中的||
/*
.size(@width,@height) when (@width = 100px),(@height = 100px){
  width: @width;
  height: @height;
}
 */
// ()and()相当于JS中的&&
/*
.size(@width,@height) when (@width = 100px)and(@height = 100px){
  width: @width;
  height: @height;
}
 */
.size(@width,@height) when (ispixel(@width)){
  width: @width;
  height: @height;
}
div{
  .size(100px,100px);
  background: red;
}
```

