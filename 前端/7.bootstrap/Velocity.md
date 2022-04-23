# Velocity插件的使用

## 开篇

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08-Velocity开篇</title>
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
    <script src="js/jquery-3.1.1.js"></script>
    <script src="js/velocity.js"></script>

</head>
<body>
<div class="box"></div>
<script>
    /*
    1.什么是Velocity?
    Velocity 是一个简单易用、性能极高、功能丰富的轻量级JS动画库。
    它能和 jQuery/Zepto 完美协作，并和$.animate()有相同的 API， 但它不依赖 jQuery，可单独使用。
    Velocity 不仅包含了 $.animate() 的全部功能， 还拥有：颜色动画、转换动画(transforms)、循环、 缓动、SVG 动画、和 滚动动画 等特色功能

    官方地址: https://github.com/julianshapiro/velocity
    中文文档: http://shouce.jb51.net/velocity/index.html

    2.GSAP基本使用
    1.1导入Velocity文件
    1.2利用Velocity实现动画
    * */
    // 1.单独使用
    /*
    let oDiv = document.querySelector(".box");
    Velocity(oDiv, {
        height: "300px"
    }, {
        duration: 3000
    });
    */

    // 2.配合jQuery使用
    // 注意点: 必须先导入jQuery, 再导入velocity
    $(".box").velocity({
        height: "300px"
    }, {
        duration: 3000
    });
</script>
</body>
</html>
```

## Velocity配置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09-Velocity常用配置</title>
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
    <script src="js/jquery-3.1.1.js"></script>
    <script src="js/velocity.js"></script>
</head>
<body>
<div class="box"></div>
<script>
    $(".box").velocity({
        marginLeft: "500px"
    }, {
        duration: 3000,
        // 设置动画开始之前的延迟时间
        // delay: 2000,
        // 设置动画循环的次数
        // 注意点: 从初始位置到指定位置再到初始的位置算一次
        // loop: 2,
        // 设置动画运动的节奏
        // easing: "easeInOutQuint",
        // 设置动画结束之后元素的状态
        // display: "none",
        // visibility: "hidden"
        // 设置动画队列
        // 注意点: 只要设置了动画队列动画就不会自动执行
        queue: "a"
    });
	//执行两个动画队列
    $(".box").velocity({
        marginTop: "500px"
    }, {
        duration: 3000,
        queue: "b"
    });
    $(".box").dequeue("a");
    setTimeout(function () {
        $(".box").dequeue("b");
    }, 3000)
</script>
</body>
</html>
```

## velocity常用事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-Velocity常用事件</title>
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
    <script src="js/jquery-3.1.1.js"></script>
    <script src="js/velocity.js"></script>
</head>
<body>
<div class="box"></div>
<script>
    $(".box").velocity({
        marginLeft: "500px"
    }, {
        duration: 3000,
        delay: 2000,
        begin: function(elements) {
            console.log("动画开始了", elements);
        },
        complete: function(elements) {
            console.log("动画结束了", elements);
        },
        /*
        elements：当前执行动画的元素，可以用$(elements)来获取
        complete：整个动画过程执行到百分之多少，该值是递增的，注意：该值为一个十进制数值 并不带单位 (%)
        remaining：整个动画过程还剩下多少毫秒，该值是递减的
        start：动画开始时的绝对时间 (Unix time)
        tweenValue：动画执行过程中 两个动画属性之间的补间值
        * */
        progress: function(elements, complete, remaining, start, tweenValue) {
            // console.log("动画正在执行");
            console.log((complete * 100) + "%");
        }
    });
</script>
</body>
</html>
```

