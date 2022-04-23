# GSAP插件学习

## GSAP开篇

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-GSAP开篇</title>
    <script src="js/TweenMax.js"></script>   
</head>
<body>
<div class="box"></div>
<script>
    /*
    1.什么是ScrollMagic?
    ScrollMagic是一个滚动视差插件
    ScrollMagic本身比较简单，只包含2个类：
    crollMagic.Controller 一个控制器类，用于总体的调度 ；
    ScrollMagic.Scene 一个场景类，用于设计具体的变换。

    需要注意的是，它本身并没有集成 animation的控制方法，动画的实现，需要引入插件 GSAP 或者是 Velocity
    * */
    /*
    1.什么是GSAP?
    GSAP(GreenSock Animation Platform)是一个从flash时代一直发展到今天的专业动画库

    2.GSAP优点
    1、速度快。GSAP专门优化了动画性能，使之实现和CSS一样的高性能动画效果。
    2、轻量与模块化。模块化与插件式的结构保持了核心引擎的轻量，TweenLite包非常小（基本上低于7kb）。GSAP提供了TweenLite, TimelineLite, TimelineMax 和 TweenMax不同功能的动画模块，你可以按需使用。
    3、没有依赖。
    4、灵活控制。不用受限于线性序列，可以重叠动画序列，你可以通过精确时间控制，灵活地使用最少的代码实现动画。

    3.GSAP版本
    GSAP提供4个库文件供用户使用
    1.TweenLite：这是GSAP动画平台的核心部分，使用它可以用来实现大部分的动画效果，适合来实现一些元素的简单动画效果。
    2.TimelineLite：一个强大的，轻量级的序列工具，它就如一个存放补间动画的容器，可以很容易的整体控制补间动画，且精确管理补间动画彼此之间的时间关系。比如动画的各种状态，Pause，reverse，restart，speed up，slow down，seek time，add labels等。它适合来实现一些复杂的动画效果。
    3.TimelineMax：扩展TimelineLite，提供完全相同的功能再加上有用的（但非必需）功能，如repeat，repeatDelay，yoyo，currentLabel()等。TimelineMax的目标是成为最终的全功能工具，而不是轻量级的。
    4.TweenMax：可以完成TimelineLite做的每一件事，并附加非必要的功能，如repeat，yoyo，repeatDelay(重复延迟)等。它也包括许多常见的插件，如CSSPlugin，这样您就不需自行载入多个文件。侧重于全功能的，而不是轻量级的。
    >>建议在开发中使用TweenMax这个全功能的js文件，它包括了GreenSock动画平台的所有核心的功能。

    官网地址：http://www.greensock.com/
    github地址：https://github.com/greensock/GreenSock-JS/
    中文网: https://www.tweenmax.com.cn/
    * */
    // 1.创建TweenMax对象
    /*
    第一个参数: 需要执行动画的元素
    第二个参数: 动画执行的时长
    第三个参数: 具体执行动画的属性
    * */
    // new TweenMax(".box", 3, {x: 500});
    // 2.利用静态方法执行动画
    // 从当前位置到指定位置
    // TweenMax.to(".box", 3, {x: 500});
    // 从指定位置到当前位置
    // TweenMax.from(".box", 3, {x: 500});
    // 从第一个指定的位置到第二个指定的位置
    TweenMax.fromTo(".box", 3, {x: 500}, {x: 200});
</script>
</body>
</html>
```

## GSPA交叉动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-GSAP交叉动画</title>
    <script src="js/TweenMax.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            background: red;
        }
        .box2{
            background: blue;
        }
        .box3{
            background: green;
        }
    </style>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
<script>
    //可以控制多个元素发生动画，并且可以设置延迟时间
    // TweenMax.staggerTo([".box1", ".box2", ".box3"], 3, {x: 500}, 3);
    // TweenMax.staggerFrom([".box1", ".box2", ".box3"], 3, {x: 500}, 3);
    TweenMax.staggerFromTo([".box1", ".box2", ".box3"], 3, {x: 500}, {x: 200}, 3);
</script>
</body>
</html>
```

## GSPA动画属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-GSAP动画属性</title>
    <script src="js/TweenMax.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            background: red;
        }
    </style>
</head>
<body>
<div class="box"></div>
<script>
    new TweenMax(".box", 3, {
        // 设置动画开始之前的延迟时间
        // delay: 2,
        // 设置动画初识值
        startAt: {
            x: 100
        },
        // 设置动画结束值
        css: {
            x: 500,
        },
        // 设置动画重复执行的次数
        // 无限重复 -1
        repeat: 2,
        // 设置动画重复执行的往返动画
        yoyo: true,
        // 设置重复动画开始之前的延迟时间
        repeatDelay: 3,
        // 设置动画执行的节奏
        ease: Bounce.easeOut,
        yoyoEase: Bounce.easeOut
    });
</script>
</body>
</html>
```

## 循环动画

```html
<!-- 通过设置cycle属性 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-GSAP循环动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            display: inline-block;
            width: 50px;
            height: 50px;
            background: #ccc;
            border: 1px solid #000;
            border-radius: 10px;
        }
    </style>
    <script src="js/TweenMax.js"></script>
</head>
<body>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<div class="box"></div>
<script>
    let oDivs = document.querySelectorAll(".box");
    TweenMax.staggerTo(oDivs, 3, {
        cycle: {
            height: [100, 150, 200],
            backgroundColor: ["red", "green", "blue"]
        }
    }, 3);
</script>
</body>
</html>
```

## GSAP动画常用事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
      *{
          margin: 0;
          padding: 0;
      }
      div{
          width: 100px;
          height: 100px;
          background: red;
      }
  </style>
  <script src="js/TweenMax.js"></script>
</head>
<body>
<div class="box"></div>
<script>
  let obj = {name:"lnj"}
  TweenMax.to(".box",3,{
    x:500,
    delay:3,
    //onStart:动画开始事件
    onStart:function (a,b,c){
      console.log(this)
    },
    //传递给onStart函数的形参
    onStartParams:["123","456","789"],
    //替换onStart中的this
    onStartScope:obj
  })
</script>
</body>
</html>
```

## GSAP动画常用方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-GSAP动画常用方法</title>
    <script src="js/TweenMax.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            background: red;
        }
    </style>
</head>
<body>
<div class="box"></div>
<button class="start">开始</button>
<button class="paused">暂停</button>
<button class="toggle">切换</button>
<button class="restart">重新开始</button>
<script>
    let tm = TweenMax.to(".box", 3, {
        x: 500,
        paused: true
    });
    // console.log(tm);
    let oStartBtn = document.querySelector(".start");
    oStartBtn.onclick = function () {
        //动画开始
        tm.play();
    }

    let oPauseBtn = document.querySelector(".paused");
    oPauseBtn.onclick = function () {
        //动画暂停
        tm.pause();
    }

    let oToggleBtn = document.querySelector(".toggle");
    oToggleBtn.onclick = function () {
        //动画切换
        // tm.paused(true);
        // tm.paused(false);
        // console.log(tm.paused());
        tm.paused(!tm.paused());
    }

    let oRestartBtn = document.querySelector(".restart");
    oRestartBtn.onclick = function () {
        //动画重新开始
        tm.restart();
    }
</script>
</body>
```

## GSAP动画管理

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <style>
      *{
          margin: 0;
          padding: 0;
      }
      div{
          width: 100px;
          height: 100px;
          background: #ccc;
          border: 1px solid #000;
      }
  </style>
  <script src="js/TweenMax.js"></script>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
<script>
    //该对象自动计算延迟时间，有点像动画队列
  let tm = new TimelineMax();
  tm.add(TweenMax.to(".box1",3,{
    x:400
  }))
  tm.add(TweenMax.to(".box2",3,{
    x:300
  }))
  tm.add(TweenMax.to(".box3",3,{
    x:200
  }))
</script>
</body>
</html>
```

