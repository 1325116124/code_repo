# ScrollMagic

## 开篇

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>11-ScrollMagic开篇</title>
    <script src="js/ScrollMagic.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        header{
            width: 100%;
            height: 100px;
            background: #000;
        }
        div{
            width: 100%;
            height: 200px;
        }
        .section1{
            background: red;
        }
        .section2{
            background: green;
        }
        .section3{
            background: blue;
        }
        .section4{
            background: deeppink;
        }
        footer{
            width: 100%;
            height: 2000px;
            background: #000;
        }
    </style>
</head>
<body>
<header></header>
<div class="section1"></div>
<div class="section2"></div>
<div class="section3"></div>
<div class="section4"></div>
<footer></footer>
<script>
    /*
    1.什么是ScrollMagic?
    ScrollMagic是一个神奇的滚动效果的插件.
    如果你想在特定的滚动位置开始一个动画，并且让动画同步滚动条的动作，
    或者把元素粘在一个特定的滚动位置，那么这款插件正是你需要的。
    使用 ScrollMagic，你可以很容易地让动画和滚动条的动作同步。
　　使用 ScrollMagic，你可以很容易地把视差效果添加到您的网站中。

    2.ScrollMagic特点:
    优化性能
    **轻量级（压缩后只有6KB）
    灵活可扩展
    **兼容移动设备
    强大的事件管理
    **支持响应式网页设计
    面向对象的编程方式和链式编程方式
    代码可读性强
    支持两个方向的滚动（even different on one page）
    支持在div容器中滚动（一个页面可以支持多个div）
    完善的调试和日志记录功能

    3.官网地址: http://ScrollMagic.io
    https://github.com/janpaepke/ScrollMagic
      官方文档: http://scrollmagic.io/docs/index.html
    * */
    // 1.创建一个控制器对象
    let controller = new ScrollMagic.Controller();
    // 2.创建一个场景对象
    let scene = new ScrollMagic.Scene({
        // 告诉ScrollMagic从什么位置开始当前的场景
        // offset: 100,
        offset: 0,
        // 告诉ScrollMagic当前的场景的有效范围
        duration: 200
    });
    // 告诉ScrollMagic哪一个元素需要固定
    // scene.setPin(".section1");
    scene.setPin(".section1", {pushFollowers: false});
    // 3.将场景对象添加到控制器对象
    controller.addScene(scene);
</script>
</body>
</html>
```

## ScrollMagic场景配置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-ScrollMagic场景配置</title>
    <script src="js/ScrollMagic.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        header{
            width: 100%;
            height: 100px;
            background: #000;
        }
        div{
            width: 100%;
            height: 200px;
        }
        .section1{
            background: red;
        }
        .section2{
            background: green;
        }
        .section3{
            background: blue;
        }
        .section4{
            background: deeppink;
        }
        footer{
            width: 100%;
            height: 2000px;
            background: #000;
        }
    </style>
</head>
<body>
<header></header>
<div class="section1"></div>
<div class="section2"></div>
<div class="section3"></div>
<div class="section4"></div>
<footer></footer>
<script>
    // 官方文档: http://scrollmagic.io/docs/index.html
    // 1.创建一个控制器对象controller
    let controller = new ScrollMagic.Controller();
    // 2.创建一个场景对象scene
    let scene = new ScrollMagic.Scene({
        offset: 0,
        // 告诉ScrollMagic当前的场景从哪一个元素开始
        // triggerElement: "footer",
        // triggerElement: ".section3",
        triggerElement: "header",
        // 告诉ScrollMagic当前的场景从指定元素相对于视口的什么位置开始
        // triggerHook: "onEnter",
        // triggerHook: "onCenter",
        triggerHook: "onLeave",
        duration: 200,
        reverse: false,
    });
    // 告诉ScrollMagic哪一个元素需要固定
    // scene.setPin(".section1", {pushFollowers: false});
    scene.setPin(".section1");
    // 3.将场景对象添加到控制器对象
    controller.addScene(scene);
</script>
</body>
</html>
```

## ScrollMagic配合GSAP使用:动画和滚动条进行绑定 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13-ScrollMagic-GSAP动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        header{
            width: 100%;
            height: 100px;
            background: #000;
        }
        div{
            width: 100%;
            height: 200px;
        }
        .section1{
            background: red;
        }
        .section2{
            background: green;
        }
        .section3{
            background: blue;
        }
        .section4{
            background: deeppink;
        }
        footer{
            width: 100%;
            height: 2000px;
            background: #000;
        }
        p{
            width: 100px;
            height: 100px;
            background: purple;
            margin: 0 auto;
        }
    </style>
    <script src="js/ScrollMagic.js"></script>
    <script src="js/TweenMax.js"></script>
    <!-- 要在ScrollMagic中使用Tween必须还要引入animation.gsap -->
    <script src="js/animation.gsap.js"></script>
</head>
<body>
<header></header>
<div class="section1">
    <p class="anim"></p>
</div>
<div class="section2"></div>
<div class="section3"></div>
<div class="section4"></div>
<footer></footer>
<script>
    // 1.创建一个控制器对象controller
    let controller = new ScrollMagic.Controller();
    // 2.创建一个场景对象scene
    let scene = new ScrollMagic.Scene({
        offset: 100,
        duration: 200,
    });
    // 告诉ScrollMagic哪一个元素需要固定
    scene.setPin(".section1");
    /*
    // 创建一个GSAP动画
    let tm = TweenMax.to(".anim", 3, {
        width: 200,
        height: 200
    });
    scene.setTween(tm);
    */
    //创建方式二：直接将参数放进setTween中
    scene.setTween(".anim", 3, {
        width: 200,
        height: 200
    });
    // 3.将场景对象添加到控制器对象
    controller.addScene(scene);
</script>
</body>
</html>
```

## ScrollMagic配合velocity使用：动画在触发场景下自动进行

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>14-ScrollMagic-Velocity动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        header{
            width: 100%;
            height: 100px;
            background: #000;
        }
        div{
            width: 100%;
            height: 200px;
        }
        .section1{
            background: red;
        }
        .section2{
            background: green;
        }
        .section3{
            background: blue;
        }
        .section4{
            background: deeppink;
        }
        footer{
            width: 100%;
            height: 2000px;
            background: #000;
        }
        p{
            width: 100px;
            height: 100px;
            background: purple;
            margin: 0 auto;
        }
    </style>
    <script src="js/ScrollMagic.js"></script>
    <script src="js/velocity.js"></script>
    <!-- 需要引入animation.velocity -->
    <script src="js/animation.velocity.js"></script>
</head>
<body>
<header></header>
<div class="section1">
    <p class="anim"></p>
</div>
<div class="section2"></div>
<div class="section3"></div>
<div class="section4"></div>
<footer></footer>
<script>
    // 1.创建一个控制器对象controller
    let controller = new ScrollMagic.Controller();
    // 2.创建一个场景对象scene
    let scene = new ScrollMagic.Scene({
        offset: 100,
        // 注意点: 如果需要结合Velocity来使用, 那么在创建场景的时候就不能指定有效范围
        // duration: 200,
    });
    // 告诉ScrollMagic哪一个元素需要固定
    scene.setPin(".section1");
    scene.setVelocity(".anim", {
        width: "200px",
        height: "200px"
    }, {
        duration: 3000
    });
    // 3.将场景对象添加到控制器对象
    controller.addScene(scene);
</script>
</body>
</html>
```

## ScrollMagic常用的事件函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>15-ScrollMagic事件</title>
  <style>
      *{
          margin: 0;
          padding: 0;
      }
      header{
          width: 100%;
          height: 100px;
          background: #000;
      }
      div{
          width: 100%;
          height: 200px;
      }
      .section1{
          background: red;
      }
      .section2{
          background: green;
      }
      .section3{
          background: blue;
      }
      .section4{
          background: deeppink;
      }
      footer{
          width: 100%;
          height: 2000px;
          background: #000;
      }
  </style>
  <script src="js/ScrollMagic.js"></script>
  <script src="js/velocity.js"></script>
  <script src="js/animation.velocity.js"></script>
</head>
<body>
<header></header>
<div class="section1"></div>
<div class="section2"></div>
<div class="section3"></div>
<div class="section4"></div>
<footer></footer>
<script>
  // 1.创建一个控制器对象controller
  let controller = new ScrollMagic.Controller();
  // 2.创建一个场景对象scene
  let scene = new ScrollMagic.Scene({
    offset: 100,
    duration: 200,
  });
  // 告诉ScrollMagic哪一个元素需要固定
  scene.setPin(".section1");
  scene.on("start", function (event) {
    console.log("进入场景");
  });
  scene.on("end", function (event) {
    console.log("离开场景");
  });
  scene.on("progress", function (event) {
    console.log("场景执行过程中" + event.progress);
  });
  // 3.将场景对象添加到控制器对象
  controller.addScene(scene);
</script>
</body>
</html>
```

