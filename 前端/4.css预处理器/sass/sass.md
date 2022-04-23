# Scss

## scss初体验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-SASS初体验</title>
    <!--
    <style>
        .father{
            width: 300px;
            height: 300px;
            background: red;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
        .son{
            width: 200px;
            height: 200px;
            background: blue;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
    </style>
    -->
    <link rel="stylesheet" href="Sass基础/css/01.css">
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    /*
    1.什么是SASS(Syntactically Awesome Stylesheets Sass)?
    SASS是一套利用Ruby实现的, 最早最成熟的CSS预处理器, 诞生于2007年.
    它扩展了 CSS 语言，增加了变量、Mixin(混合)、嵌套、函数和运算等特性，使 CSS 更易维护和扩展

    2.如何学习SASS?
    LESS是一套利用JavaScript实现的CSS预处理器, 诞生于2009年.
    由于LESS的诞生比SASS要晚, 并且LESS受到了Sass的影响, 所以在LESS中能看到大量SASS中的特性.
    所以只要学会了LESS就等同于学会了大部分的SASS

    3.LESS和SASS文件后缀名区别
    LESS以.less结尾
    SASS以.sass或者.scss结尾
    两种不同结尾方式区别: .sass结尾以缩进替代{}表示层级结构, 语句后面不用编写分号
                          .scss以{}表示层级结构, 语句后面需要写分号
                          企业开发中推荐使用.scss结尾
    注意点: 如果需要使用考拉编译sass文件, 项目目录结构中(包含文件名)不能出现中文和特殊字符
    */
</script>
</body>
</html>
```

```scss
@mixin center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.father{
  width: 300px;
  height: 300px;
  background: red;
  @include center;
  .son{
    width: 200px;
    height: 200px;
    background: blue;
    @include center;
  }
}
```

## scss中的注释

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-SASS中的注释</title>
</head>
<body>
<script>
    /*
    1.SASS中的注释
    SASS中的注释和LESS一样
    单行注释不会被编译(不会出现在编译后的文件中)
    多行注释会被编译  (会出现在编译后的文件中)
    */
</script>
</body>
</html>
```

```scss
    // 单行注释
    /*
    多行注释
    */
```

## scss中的变量

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-SASS中的变量</title>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
<script>
    /*
    1.SASS中的变量
    SASS中的变量和LESS中一样, 只是定义格式不同
    LESS中定义变量   @变量名称: 值;
    SASS中定义办理   $变量名称: 值;
    */
    /*
    2.SASS中变量特点
    SASS中变量特点和LESS中几乎一样
    2.1后定义覆盖先定义
    2.2可以把变量赋值给其它变量
    2.3区分全局变量和局部变量(访问采用就近原则)

    注意点: LESS中变量是延迟加载, 可以先使用后定义
            SASS中变量不是延迟加载, 不可以先使用后定义
    */
</script>
</body>
</html>
```

```scss
$w: 200px;
// 后定义覆盖先定义
// $w: 600px;
$h: 300px;
// 可以把变量赋值给其它变量
//$h: $w;

.box1{
  $w: 666px;
  width: $w;
  height: $h;
  background: red;
  margin-bottom: 20px;
}
.box2{
  width: $w;
  height: $h;
  background: blue;
}
// 和LESS中变量不同的是
// SASS中变量不是延迟加载, 不可以先使用后定义
//$w: 200px;
```

## scss中的变量插值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-SASS变量插值</title>
</head>
<body>
<div></div>
<script>
    /*
    1.什么是变量插值?
    如果是属性的取值可以直接使用变量,
    但是如果是属性名称或者选择器名称并不能直接使用变量, 必须使用变量插值的格式

    2.SASS中的变量插值
    SASS中的变量插值和LESS中也一样, 只不过格式不一样
    LESS变量插值格式: @{变量名称}
    SASS变量插值格式: #{$变量名称}
    */
</script>
</body>
</html>
```

```scss
$size: 200px;
$w: width;
$s: div;
#{$s}{
  #{$w}: $size;
  height: $size;
  background: red;
}
```

## scss中的运算

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-SASS中运算</title>
</head>
<body>
<div></div>
<script>
    /*
    1.SASS中的运算?
    SASS中的运算和LESS也一样, 都支持+ - * / 运算
    注意点: 无论是LESS中的运算还是SASS中的运算都需要加上()
    */
</script>
</body>
</html>
```

```scss
div{
  width: 200px;
  height: 200px;
  background: red;
  position: absolute;
  left: 50%;
  //margin-left: (-200px / 2);
  //margin-left: (-200px * 0.5);
  //margin-left: (-200px + 55);
  margin-left: (-200px - 55);
}
```

## scss中的混合

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-SASS中的混合</title>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    /*
    1.SASS中的混合
    SASS中的混合和LESS中也一样, 只是定义格式和调用的格式不同
    LESS中混合定义: .混合名称{} 或者 .混合名称(){}
    LESS中混合调用: .混合名称; 或者 .混合名称();

    SASS中混合定义: @mixin 混合名称{}; 或者 @mixin 混合名称(){};
    SASS中混合调用: @include 混合名称; 或者 @include 混合名称();
    */
</script>
</body>
</html>
```

```scss
@mixin center(){
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.father{
  width: 300px;
  height: 300px;
  background: red;
  @include center();
  .son{
    width: 200px;
    height: 200px;
    background: blue;
    @include center();
  }
}
```

## scss中带参数的混合

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>07-SASS中带参数混合</title>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
<script>
    /*
    1.SASS中带参数的混合
    SASS中带参数的混合和LESS中也一样
    1.1不带默认值形参
    1.2带默认值形参
    1.3给指定参数赋值
    */
</script>
</body>
</html>
```

```scss
/*
@mixin whc($w, $h, $c){
  width: $w;
  height: $h;
  background: $c;
}
 */
@mixin whc($w: 100px, $h: 100px, $c: #000){
  width: $w;
  height: $h;
  background: $c;
}
.box1{
  //width: 300px;
  //height: 300px;
  //background: red;
  @include whc(300px, 300px, red);
}
.box2{
  //width: 200px;
  //height: 200px;
  //background: blue;
  //@include whc(200px, 200px, blue);
  //@include whc();
  @include whc($c: blue);
}
```

## scss中的可变参数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08-SASS中的可变参数</title>
</head>
<body>
<div></div>
<script>
    /*
    1.SASS中的可变参数
    SASS中的可变参数和LESS中也一样,
    只不过由于SASS不是使用JS实现的, 所以不能直接在混合中使用arguments
    必须通过 $args...的格式来定义可变参数, 然后通过$args来使用

    注意点: 和LESS一样可变参数必须写在形参列表的最后
    */
</script>
</body>
</html>
```

```scss
/*
@mixin animate($name, $time, $mode, $delay){
  transition: $name $time $mode $delay;
}
 */
/*
@mixin animate($args...){
  transition: $args;
}
 */
@mixin animate($name, $time, $args...){
  transition: $name $time $args;
}
div{
  width: 200px;
  height: 200px;
  background: red;
  //transition: all 4s linear 0s;
  @include animate(all, 4s, linear, 0s);
}
div:hover{
  width: 400px;
  height: 400px;
  background: blue;
}
```

## scss导入其他的scss文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09-SASS导入其它sass文件</title>
</head>
<body>
<div></div>
<script>
    /*
    1..scss文件中导入其它.scss文件
    和LESS一样SASS文件中也支持导入其它SASS文件

    其实原生的CSS也支持通过@import导入其它的CSS文件, 只不过不常用

    不常用的原因在于原生的@import导入其它的CSS文件,
    只有执行到@import时浏觅器才会去下载对应 css文件，这导致请求次数变多,页面加载起来特别慢

    而LESS和SASS中的@import是直接将导入的文件拷贝到当前文件中生成一份CSS, 所以只会请求一次, 速度更快
    */
</script>
</body>
</html>
```

```scss
@import "06";

div{
  width: 200px;
  height: 200px;
  background: red;
  @include center;
}
```

## scss内置函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-SASS内置函数</title>
</head>
<body>
<div></div>
<script>
    /*
    1.SASS中的内置函数
    和LESS一样, SASS中也提供了很多内置函数方便我们使用
    */

    // 字符串函数
    /*
    unquote（$string）：删除字符串中的引号；
    quote（$string）：给字符串添加引号；
    To-upper-case（$string）：将字符串小写字母转换为大写字母
    To-lower-case（$string）：将字符串大写字母转换为小写字母
    */
    // 数值函数
    /*
    percentage($value)：将不带单位的数转换成百分比值；
    round($value)：将数值四舍五入，转换成一个最接近的整数；
    ceil($value)：向上取整；
    floor($value)：向下取整；
    abs($value)：取数的绝对值；
    min($numbers…)：找出几个数值之间的最小值；
    max($numbers…)：找出几个数值之间的最大值；
    random(): 获取随机数
    */
    // 颜色函数
    /*
    rgb($red,$green,$blue)：根据红、绿、蓝三个值创建一个颜色；
    rgba($red,$green,$blue,$alpha)：根据红、绿、蓝和透明度值创建一个颜色；
    red($color)：从一个颜色中获取其中红色值；
    green($color)：从一个颜色中获取其中绿色值；
    blue($color)：从一个颜色中获取其中蓝色值；
    mix($color-1,$color-2,[$weight])：把两种颜色混合在一起。
    */
    // 列表函数
    /*
    length($list)：返回一个列表的长度值;
    nth($list, $n)：返回一个列表中指定的某个标签值;
    join($list1, $list2, [$separator])：将两个列给连接在一起，变成一个列表；
    append($list1, $val, [$separator])：将某个值放在列表的最后；
    zip($lists…)：将几个列表结合成一个多维的列表；
    index($list, $value)：返回一个值在列表中的位置值。
    */
</script>
</body>
</html>
```

```scss
@function square($num){
  @return $num * $num + px;
}
div{
  width: square(2);
  height: 200px;
  background: mix(red, blue);
}
```

## scss的层级结构

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>11-SASS中的层级结构</title>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    /*
    1.SASS中的层级结构
    和LESS一样支持嵌套, 默认情况下嵌套的结构会转换成后代选择器
    和LESS一样也支持通过&符号不转换成后代选择器
    */
</script>
</body>
</html>
```

```scss
.father{
  width: 300px;
  height: 300px;
  background: red;
  &:hover{
    width: 100px;
    height: 100px;
    background: yellow;
  }
  .son{
    width: 200px;
    height: 200px;
    background: blue;
  }
}
```

## scss的继承

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-SASS中的继承</title>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    /*
    1.SASS中的继承
    SASS中的继承和LESS中的继承一样, 都是通过并集选择器来实现的, 只不过格式不一样而已

    混合和继承区别
    混合是直接拷贝, 有多少个地方用到就会拷贝多少份
    继承是通过并集选择器, 不会拷贝只会保留一份
    */

</script>
</body>
</html>
```

```scss
/*
@mixin center(){
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
 */
.center{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.father{
  @extend .center;
  width: 300px;
  height: 300px;
  background: red;
  //@include center;
  .son{
    @extend .center;
    width: 200px;
    height: 200px;
    background: blue;
    //@include center;
  }
}
```

## scss中的条件判断

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13-SASS中的条件判断</title>
    <link rel="stylesheet" href="css/13.css">
</head>
<body>
<div></div>
<script>
    /*
    1.SASS中的条件判断
    和LESS一样SASS中也支持条件判断, 只不过SASS中的条件判断支持得更为彻底
    SASS中支持
    @if(条件语句){}
    @else if(条件语句){}
    ... ...
    @else(条件语句){}
    SASS中当条件不为false或者null时就会执行{}中的代码
    和LESS一样SASS中的条件语句支持通过> >= < <= ==进行判断
    */
</script>
</body>
</html>
```

```scss
@mixin triangle($dir, $width, $color){
  width: 0;
  height: 0;
  border-width: $width;
  border-style: solid solid solid solid;
  @if($dir == Up){
    border-color: transparent transparent $color transparent;
  }@else if($dir == Down){
    border-color: $color transparent transparent transparent;
  }@else if($dir == Left){
    border-color: transparent $color transparent transparent;
  }@else if($dir == Right){
    border-color: transparent transparent transparent $color;
  }
}
div{
  //width: 0;
  //height: 0;
  //border-width: 10px 10px 10px 10px;
  //border-style: solid solid solid solid;
  //border-color: #000 transparent transparent transparent;
  //@include triangle(50px, blue);
  //@include triangle(Up, 50px, blue);
  @include triangle(Left, 50px, blue);
}
```

## scss中的循环

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>14-SASS中的循环</title>
    <link rel="stylesheet" href="css/14.css">
</head>
<body>
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
    <li>8</li>
    <li>9</li>
    <li>10</li>
</ul>
<script>
    /*
    1.SASS中的循环
    SASS比LESS牛逼的地方就在于SASS中直接支持循环语句, 而LESS中需要通过混合+条件判断自己实现
    SASS中支持两种循环, 分别是for循环和while循环

    2.for循环
    @for $i from 起始整数 through 结束整数{}
    @for $i from 起始整数 to 结束整数{}
    两者的区别 through包头包尾, to包头不包尾

    3.while循环
    @while(条件语句){}
    */
</script>
</body>
</html>
```

```scss
ul{
  li{
    width: 100%;
    height: 50px;
    border: 1px solid #000;
    font-size: 20px;
    color: #fff;
    background: red;
    /*
    &:nth-child(5){
      background: blue;
    }
    &:nth-child(6){
      background: blue;
    }
    &:nth-child(7){
      background: blue;
    }
    &:nth-child(8){
      background: blue;
    }
     */
    //@for $i from 5 through 8{
    //@for $i from 5 to 8{
    $i:5;
    @while($i <= 8){
      &:nth-child(#{$i}){
        background: deepskyblue;
      }
      $i:$i+1;
    }
  }
}
```

