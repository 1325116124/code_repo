# js高级

## H5新增的存储方法

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>18-H5新增存储方案</title>
</head>
<body>
<button class="save">存点cookie</button>
<button class="send">发送请求</button>

<button class="btn1">保存</button>
<button class="btn2">删除</button>
<button class="btn3">修改</button>
<button class="btn4">查找</button>
<button class="btn5">清空</button>

<script src="js/jquery-1.12.4.js"></script>
<script src="myAjax.js"></script>
<script>
    /*
    1.什么是SessionStorage和LocalStorage
    和Cookie一样, SessionStorage和LocalStorage也是用于存储网页中的数据的

    2.Cookie、 SessionStorage、LocalStorage区别
    2.1生命周期(同一浏览器下)
    Cookie生命周期:         默认是关闭浏览器后失效, 但是也可以设置过期时间
    SessionStorage生命周期: 仅在当前会话(窗口)下有效，关闭窗口或浏览器后被清除, 不能设置过期时间
    LocalStorage生命周期:   除非被清除，否则永久保存

    2.2容量
    Cookie容量:         有大小(4KB左右)和个数(20~50)限制
    SessionStorage容量: 有大小限制(5M左右) http://dev-test.nemikor.com/web-storage/support-test/
    LocalStorage容量:   有大小限制(5M左右) http://dev-test.nemikor.com/web-storage/support-test/

    2.3网络请求
    Cookie网络请求:         每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题
    SessionStorage网络请求: 仅在浏览器中保存，不参与和服务器的通信
    LocalStorage网络请求:   仅在浏览器中保存，不参与和服务器的通信

    3.Cookie、 SessionStorage、LocalStorage应用场景
    Cookie:         判断用户是否登录
    LocalStorage:   购物车
    sessionStorage: 表单数据

    4.注意点:
    无论通过以上那种方式存储的数据, 切记不能将敏感数据直接存储到本地
    * */

    $('.save').click(function () {
        document.cookie = "myName=it666;path=/;domain=127.0.0.1;";
    });
    $('.send').click(function () {
        ajax("18-cookie.php",
            {},
            3000,
            function (xhr) {
                console.log("请求成功", xhr.responseText);
            }, function () {
                console.log("请求失败");
            });
    });

    $('.btn1').click(function () {
        // sessionStorage.setItem("name", "lnj");
        // sessionStorage.setItem("age", "34");
        localStorage.setItem("name", "lnj");
    });
    $('.btn2').click(function () {
        sessionStorage.removeItem("name");
    });
    $('.btn3').click(function () {
        sessionStorage.setItem("name", "it666");
    });
    $('.btn4').click(function () {
        let value = sessionStorage.getItem("name");
        alert(value);
    });
    $('.btn5').click(function () {
        sessionStorage.clear();
    });

</script>
</body>
</html>
```

## 同源策略

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>19-同源策略</title>
    <script src="js/jquery-1.12.4.js"></script>
</head>
<body>
<button class="send">发送请求</button>
<script>
    /*
    1.什么是同源策略?
    同源策略（Same origin policy）是一种约定，它是浏览器最核心也最基本的安全功能
    所谓同源是指: 协议，域名，端口都相同,就是同源, 否则就是跨域

    http://www.it666.com:80/index.html
    协议: http/https/...
    一级域名: it666.com/itzb.com
    二级域名: www/study/edu/...
    端口号: 80/3306/...

    // 协议+一级域名+二级域名+端口号都相同, 所以同源
    http://www.it666.com:80/index.html
    http://www.it666.com:80/detail.html

    // 协议不同, 所以不同源, 是跨域
    http://www.it666.com:80/index.html
    https://www.it666.com:80/index.html

    // 一级域名不同, 所以不同源, 是跨域
    http://www.it666.com:80/index.html
    http://www.itzb.com:80/index.html

    // 二级域名不同, 所以不同源, 是跨域
    http://www.it666.com:80/index.html
    http://edu.it666.com:80/index.html

    // 端口号不同, 所以不同源, 是跨域
    http://www.it666.com:80/index.html
    http://www.it666.com:8090/index.html

    2.同源策略带来的影响
    在同源策略下, 浏览器只允许Ajax请求同源的数据, 不允许请求不同源的数据
    但在企业开发中, 一般情况下为了提升网页的性能, 网页和数据都是单独存储在不同服务器上的
    这时如果再通过Ajax请求数据就会拿不到跨域数据

    3.跨域解决方案
    jsonp
    document.domain+iframe
    location.hash + iframe
    window.name + iframe
    window.postMessage
    flash等第三方插件
    * */

    /*
    当前的网页地址: http://127.0.0.1:80/jQuery/Ajax/19-%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5.html
    请求的资源地址: http://127.0.0.1:80/jQuery/Ajax/19-SameOriginPolicy.php

    当前的网页地址: http://127.0.0.1:63342/jQuery/Ajax/19-%E5%90%8C%E6%BA%90%E7%AD%96%E7%95%A5.html
    请求的资源地址: http://127.0.0.1:80/jQuery/Ajax/19-SameOriginPolicy.php
    */
    $('.send').click(function () {
        $.ajax({
            url:"http://127.0.0.1:80/jQuery/Ajax/19-SameOriginPolicy.php",
            success: function (msg) {
                console.log(msg);
            },
            error: function () {
                console.log("请求失败");
            }
        });
    });
</script>
</body>
</html>
```

## jsonp的原理

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>20-jsonp原理</title>
</head>
<body>
<button>我是按钮</button>
<script>
    /*
    1.什么是JSONP?
    JSONP让网页从别的地址（跨域的地址）那获取资料，即跨域读取数据

    2.JSONP实现跨域访问的原理
    2.1在同一界面中可以定义多个script标签
    2.2同一个界面中多个script标签中的数据可以相互访问
    2.3可以通过script的src属性导入其它资源
    2.4通过src属性导入其它资源的本质就是将资源拷贝到script标签中
    2.5script的src属性不仅能导入本地资源, 还能导入远程资源
    2.6由于script的src属性没有同源限制, 所以可以通过script的src属性来请求跨域数据
    * */
    // let num = 666;
    // function demo() {
    //     console.log("demo");
    // }
</script>
<!--
<script src="20-jsonp.js">
    // let num = 777;
    // function test() {
    //     console.log("test777");
    // }
</script>
-->
<!--<script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>-->
<script src="http://127.0.0.1:80/jQuery/Ajax/20-jsonp.php"></script>
<script>
    // console.log(num);
    // demo();

    // console.log(num);
    // test();

    // $("button").click(function () {
    //     alert("按钮被点击了");
    // });

    /*
    当前网页的地址: http://127.0.0.1:63342/jQuery/Ajax/20-jsonp%E5%8E%9F%E7%90%86.html
    远程资源的地址: http://127.0.0.1:80/jQuery/Ajax/20-jsonp.php
    */
    console.log(num);
</script>
</body>
</html>
```

## jsonp的优化

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>21-JSONP优化</title>
</head>
<body>
<!--<script src="http://127.0.0.1:80/jQuery/Ajax/20-jsonp.php?cb=test"></script>-->
<script>
    /*
    优化一
    1.在企业开发中通过JSONP来获取跨域的数据,
    一般情况下服务器返回的都不会是一个变量, 而是一个函数的调用
    */

    /*
    优化二
    2.当前服务器返回的函数调用名称写死了
    服务器返回函数叫什么名称, 我们本地就必须定义一个叫什么名称的函数
    */
    /*
    解决方案:
    通过URL参数的方式来动态指定函数名称
    */

    /*
    优化三
    3.由于script标签默认是同步, 前面的script标签没有加载完数据, 后面的script标签就不会被执行
    所以请求数据的script标签必须放到后面
    */
    /*
    解决方案:
    通过JS动态创建script标签, 因为JS动态创建的script标签默认就是异步的,
    不用等到前面的标签加载完就可以执行后面的script标签
    */

    let oScript = document.createElement("script");
    oScript.src = "http://127.0.0.1:80/jQuery/Ajax/20-jsonp.php?cb=test";
    document.body.appendChild(oScript);

    function test(data) {
        console.log(data);
    }
</script>
<!--
<script src="http://127.0.0.1:80/jQuery/Ajax/20-jsonp.php?cb=test">
    // demo(666);
    // test(666);
</script>
-->
</body>
</html>
```

## jquery中jsonp的使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>22-jQuery中jsonp使用</title>
</head>
<body>
<script src="js/jquery-1.12.4.js"></script>
<script>
    /*
    当前网页的地址: http://127.0.0.1:63342/jQuery/Ajax/22-jQuery%E4%B8%ADjsonp%E4%BD%BF%E7%94%A8.html
    当前资源的地址: http://127.0.0.1:80/jQuery/Ajax/22-jsonp.php
    */
    $.ajax({
        url: "http://127.0.0.1:80/jQuery/Ajax/22-jsonp.php",
        data:{
            "teacher": "lnj",
            "age": 34
        },
        dataType: "jsonp", // 告诉jQuery需要请求跨域的数据
        jsonp: "cb",  // 告诉jQuery服务器在获取回调函数名称的时候需要用什么key来获取
        jsonpCallback: "lnj", // 告诉jQuery服务器在获取回调函数名称的时候回调函数的名称是什么
        success: function (msg) {
            console.log(msg);
        }
    });
</script>
</body>
</html>
```

## 封装jsonp

```js
function obj2str(obj) {
    // 生成随机因子
    obj.t = (Math.random() + "").replace(".", "");
    let arr = [];
    for(let key in obj){
        arr.push(key + "=" + encodeURI(obj[key]));
    }
    let str = arr.join("&");
    // console.log(str);
    return str;
}
function myJSONP(options) {
    options = options || {};
    // http://127.0.0.1/jQuery/Ajax/22-jsonp.php?cb=lnj&teacher=lnj&age=34&_=1559735634387
    // http://127.0.0.1/jQuery/Ajax/22-jsonp.php?cb=lnj&teacher=lnj&age=34&t=08520581619221432
    // 1.生成URL地址
    let url = options.url;
    if(options.jsonp){
        url += "?" + options.jsonp + "=";
    }else{
        url += "?callback=";
    }

    let callbackName = ("jQuery" + Math.random()).replace(".", "");
    if(options.jsonpCallback){
        callbackName = options.jsonpCallback;
        url += options.jsonpCallback;
    }else{
        // console.log(callbackName);
        url += callbackName;
    }
    if(options.data){
        let str = obj2str(options.data);
        url += "&" + str;
    }
    // console.log(url);

    // 2.获取跨域的数据
    let oScript = document.createElement("script");
    oScript.src = url;
    document.body.appendChild(oScript);

    // 3.定义回调函数
    window[callbackName] = function (data) {
        // 删除已经获取了数据的script标签
        document.body.removeChild(oScript);
        // 将获取到的数据返回给外界
        options.success(data);
    }
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>23-JSONP封装</title>
</head>
<body>
<script src="myJsonp.js"></script>
<script>
    myJSONP({
            url: "http://127.0.0.1:80/jQuery/Ajax/22-jsonp.php",
            data:{
                "teacher": "lnj",
                "age": 34
            },
            jsonp: "cb",  // 告诉jQuery服务器在获取回调函数名称的时候需要用什么key来获取
            jsonpCallback: "lnj", // 告诉jQuery服务器在获取回调函数名称的时候回调函数的名称是什么
            success: function (msg) {
                console.log(msg);
            }
        });
</script>
</body>
</html>
```

## js是单线程的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>25-JS是单线程的</title>
</head>
<body>
<script>
    /*
    1.JS是单线程的
    所以JS中的代码都是串行的, 前面没有执行完毕后面不能执行
    */
    /*
    2.同步代码和异步代码
    除了"事件绑定的函数"和"回调函数"以外的都是同步代码

    2.1程序运行会从上至下依次执行所有的同步代码
    2.2在执行的过程中如果遇到异步代码会将异步代码放到事件循环中
    2.3当所有同步代码都执行完毕后, JS会不断检测 事件循环中的异步代码是否满足条件
    2.4一旦满足条件就执行满足条件的异步代码
    * */
    /*
    2.为什么JS是单线程的?
    javaScript的单线程，与它的用途有关。
    作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。
    这决定了它只能是单线程，否则会带来很复杂的同步问题。

    例如: 如果JS是多线程的
    现在有一个线程要修改元素中的内容, 一个线程要删除该元素, 这时浏览器应该以哪个线程为准？
    */
    console.log("1"); // 同步代码
    setTimeout(function () { // 异步代码
        console.log("2");
    }, 500);
    console.log("3"); // 同步代码
    alert("666"); // 同步代码
    // 1 / 3 / 2
    /*
    // 系统添加代码模拟
    let arr = [setTimeout(function () { // 异步代码
        console.log("2");
    }, 500)];
    事件循环
    let index = 0;
    let length = arr.length;
    while(true){
        let item = arr[index];
        if(item.time === 500){
            执行异步代码
        }
        index++;
        if(index === length){
            index = 0;
        }
    }
    */
    /*
    扩展阅读: https://segmentfault.com/a/1190000015042127
    * */
</script>
</body>
</html>
```

## promise基本概念

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>26-promise基本概念</title>
</head>
<body>
<script>
    /*
    1.什么是promise?
    promise是ES6中新增的异步编程解决方案, 在代码中的表现是一个对象
    * */

    // 需求: 从网络上加载3个资源, 要求加载完资源1才能加载资源2, 加载完资源2才能加载资源3
    //       前面任何一个资源加载失败, 后续资源都不加载

    /*
    function request(fn) {
        setTimeout(function () {
            fn("拿到的数据");
        }, 1000);
    }
    request(function (data) {
        console.log(data, 1);
        request(function (data) {
            console.log(data, 2);
            request(function (data) {
                console.log(data, 3);
            });
        });
    });
    */
    /*
    2.promise作用
    企业开发中为了保存异步代码的执行顺序, 那么就会出现回调函数层层嵌套
    如果回调函数嵌套的层数太多, 就会导致代码的阅读性, 可维护性大大降低
    promise对象可以将异步操作以同步流程来表示, 避免了回调函数层层嵌套(回调地狱)
    * */
    function request() {
        return new Promise(function (resolve, reject) {
            setTimeout(function () {
                resolve("拿到的数据");
            }, 1000);
        });
    }
    request().then(function (data) {
        console.log(data, 1);
        return request();
    }).then(function (data) {
        console.log(data, 2);
        return request();
    }).then(function (data) {
        console.log(data, 3);
    });
</script>
</body>
</html>
```

## promise基本使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>27-promise基本使用</title>
</head>
<body>
<script>
    /*
    1.什么是Promise?
    Promise是ES6中新增的一个对象,
    通过Promise就可以实现 用同步的流程来表示异步的操作
    通过Promise就可以 避免回调函数层层嵌套(回调地狱)问题

    2.如何创建Promise对象?
    new Promise(function(resolve, reject){});
    promise对象不是异步的, 只要创建promise对象就会立即执行存放的代码

    3.Promise是如何实现 通过同步的流程来表示异步的操作的?
    promise对象是通过状态的改变来实现的, 只要状态发生改变就会自动触发对应的函数

    4.Promise对象三种状态
    pending:   默认状态，只要没有告诉promise任务是成功还是失败就是pending状态
    fulfilled(resolved): 只要调用resolve函数, 状态就会变为fulfilled, 表示操作成功
    rejected:  只要调用rejected函数, 状态就会变为rejected, 表示操作失败
    注意点: 状态一旦改变既不可逆, 既从pending变为fulfilled, 那么永远都是fulfilled
                                  既从pending变为rejected, 那么永远都是rejected

    5.监听Promise状态改变
    我们还可以通过函数来监听状态的变化
    resolved --> then()
    rejected --> catch()
    * */
    // console.log("1");
    let promise = new Promise(function (resolve, reject) {
        console.log("2");
        reject();
        // resolve();
    });
    // console.log("3");
    // console.log(promise);
    promise.then(function () {
        console.log("then");
    });
    promise.catch(function () {
        console.log("catch");
    });
</script>
</body>
</html>
```

## promise的then方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>28-promise-then方法</title>
</head>
<body>
<script>
    /*
    0.then方法
    then方法接收两个参数,
    第一个参数是状态切换为成功时的回调,
    第二个参数是状态切换为失败时的回调
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        // resolve(); // 将状态修改为成功
        reject(); // 将状态修改为失败
    });
    promise.then(function () {
        console.log("成功");
    }, function () {
        console.log("失败");
    });
    */

    /*
    1.then方法
    在修改promise状态时, 可以传递参数给then方法中的回到函数
    * */
    /*
    // resolve = success, reject = error;
    let promise = new Promise(function (resolve, reject) {
        // resolve("111"); // 将状态修改为成功 success("111");
        reject("aaa"); // 将状态修改为失败  error("aaa");
    });
    // promise.then(function (data) {
    //     console.log("成功", data);
    // }, function (data) {
    //     console.log("失败", data);
    // });
    function success(data) {
        console.log(data);
    }
    function error(data) {
        console.log(data);
    }
    promise.then(success, error);
    */

    /*
    2.then方法
    同一个promise对象可以多次调用then方法,
    当该promise对象的状态时所有then方法都会被执行
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        // resolve(); // 将状态修改为成功
        reject(); // 将状态修改为失败
    });
    promise.then(function () {
        console.log("成功1");
    }, function () {
        console.log("失败1");
    });
    promise.then(function () {
        console.log("成功2");
    }, function () {
        console.log("失败2");
    });
    */

    /*
    3.then方法
    then方法每次执行完毕后会返回一个新的promise对象
    */
    /*
    let promise = new Promise(function (resolve, reject) {
        resolve(); // 将状态修改为成功
        // reject(); // 将状态修改为失败
    });
    let p2 = promise.then(function () {
        console.log("成功1");
    }, function () {
        console.log("失败1");
    });
    console.log(p2);
    console.log(promise === p2);
    */

    /*
    4.then方法
    可以通过上一个promise对象的then方法给下一个promise对象的then方法传递参数
    注意点:
    无论是在上一个promise对象成功的回调还是失败的回调传递的参数,
    都会传递给下一个promise对象成功的回调
    */
    /*
    let promise = new Promise(function (resolve, reject) {
        // resolve("111"); // 将状态修改为成功
        reject("aaa"); // 将状态修改为失败
    });
    let p2 = promise.then(function (data) {
        console.log("成功1", data);
        return "222";
    }, function (data) {
        console.log("失败1", data);
        return "bbb";
    });
    p2.then(function (data) {
        console.log("成功2", data);
    }, function (data) {
        console.log("失败2", data);
    });
    */

    /*
    5.then方法
    如果then方法返回的是一个Promise对象, 那么会将返回的Promise对象的
    执行结果中的值传递给下一个then方法
    * */
    let promise = new Promise(function (resolve, reject) {
        resolve("111"); // 将状态修改为成功
        // reject("aaa"); // 将状态修改为失败
    });
    let ppp = new Promise(function (resolve, reject) {
        // resolve("222"); // 将状态修改为成功
        reject("bbb"); // 将状态修改为失败
    });
    let p2 = promise.then(function (data) {
        console.log("成功1", data);
        return ppp;
    }, function (data) {
        console.log("失败1", data);
        return "bbb";
    });
    p2.then(function (data) {
        console.log("成功2", data);
    }, function (data) {
        console.log("失败2", data);
    });
</script>
</body>
</html>
```

## promise的catch方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>29-promise-catch方法</title>
</head>
<body>
<script>
    /*
    0.catch方法
    catch 其实是 then(undefined, () => {}) 的语法糖
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        // resolve(); // 将状态修改为成功
        reject(); // 将状态修改为失败
    });
    promise.catch(function () {
        console.log("abc");
    });
    */

    /*
    2.catch方法
    注意点: 如果需要分开监听, 也就是通过then监听成功通过catch监听失败
    那么必须使用链式编程, 否则会报错
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        // resolve(); // 将状态修改为成功
        reject(); // 将状态修改为失败
    });
    // promise.then(function () {
    //     console.log("成功");
    // }).catch(function () {
    //     console.log("失败");
    // });

    promise.then(function () {
        console.log("成功");
    });
    promise.catch(function () {
        console.log("失败");
    });

    // promise.then(function () {
    //     console.log("成功");
    // }, function () {
    //     console.log("失败");
    // });
    */

    /*
    3.catch方法
    不使用链式编程的原因是
    1.如果promise的状态是失败, 但是没有对应失败的监听就会报错
    2.then方法会返回一个新的promise, 新的promise会继承原有promise的状态
    3.如果新的promise状态是失败, 但是没有对应失败的监听也会报错
    * */
    let promise = new Promise(function (resolve, reject) {
        // resolve(); // 将状态修改为成功
        reject(); // 将状态修改为失败
    });
    let p2 = promise.then(function () {
        console.log("成功");
    });
    console.log(p2);
    promise.catch(function () {
        console.log("失败1");
    });
    p2.catch(function () {
        console.log("失败2");
    });

</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>30-Promise-catch方法</title>
</head>
<body>
<script>
    /*
    1.catch方法
    和then一样, 在修改promise状态时, 可以传递参数给catch方法中的回到函数
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        reject("111");
    });
    promise.catch(function (data) {
        console.log(data);
    });
    */

    /*
    2.catch方法
    和then一样, 同一个promise对象可以多次调用catch方法,
    当该promise对象的状态时所有catch方法都会被执行
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        reject();
    });
    promise.catch(function () {
        console.log("失败1");
    });
    promise.catch(function () {
        console.log("失败2");
    });
    promise.catch(function () {
        console.log("失败3");
    });
    */

    /*
    3.catch方法
    和then一样, catch方法每次执行完毕后会返回一个新的promise对象
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        reject();
    });
    let p2 = promise.catch(function () {
        console.log("失败1");
    });
    console.log(p2);
    console.log(promise === p2);
    */

    /*
    4.catch方法
    和then方法一样, 上一个promise对象也可以给下一个promise成功的传递参数
    注意点:
    无论是在上一个promise对象成功的回调还是失败的回调传递的参数,
    都会传递给下一个promise对象成功的回调
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        reject();
    });
    let p2 = promise.catch(function () {
        console.log("失败1");
        return "it666";
    });
    p2.then(function (data) {
        console.log("成功2", data);
    }, function (data) {
        console.log("失败2", data);
    });
    */

    /*
    5.catch方法
    和then一样, catch方法如果返回的是一个Promise对象, 那么会将返回的Promise对象的
    执行结果中的值传递给下一个catch方法
    * */
    
    let promise = new Promise(function (resolve, reject) {
        reject();
    });
    let ppp = new Promise(function (resolve, reject) {
        resolve("1111");
        // reject("abcd");
    });
    let p2 = promise.catch(function () {
        console.log("失败1");
        return ppp;
    });
    p2.then(function (data) {
        console.log("成功2", data);
    }, function (data) {
        console.log("失败2", data);
    });
    

    /*
    6.catch方法
    和then方法第二个参数的区别在于, catch方法可以捕获上一个promise对象then方法中的异常
    * */
    /*
    let promise = new Promise(function (resolve, reject) {
        resolve();
    });
    // promise.then(function () {
    //     console.log("成功");
    //     xxx
    // }, function () {
    //     console.log("失败");
    // });
    promise.then(function () {
        console.log("成功");
        xxx
    }).catch(function (e) {
        console.log("失败", e);
    });
    */
</script>
</body>
</html>
```

## 异常处理

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>31-异常处理</title>
</head>
<body>
<script>
    /*
    1.JS中的异常
    简单粗暴就是有错误出现
    由于JS是单线程的, 编写的代码都是串行的,
    所以一旦前面代码出现错误,程序就会被中断, 后续代码就不会被执行

    2.JS中的异常处理
    2.1自身编写代码问题, --> 手动修复BUG
    2.2外界原因问题, --> try{}catch{}
    对于一些可预见的异常, 我们可以使用try{}catch{}来处理,

    3.JS中如何进行异常处理
    利用try{}catch{}来处理异常可以保证程序不被中断, 也可以记录错误原因以便于后续优化迭代更新
    try {
        可能遇到的意外的代码
    }
    catch(e) {
        捕获错误的代码块
    }
    * */
    // function say(){
    //     console.log("say");
    // }
    console.log("1");
    try {
        say();
    }catch (e) {
        console.log(e);
    }
    console.log("2");
</script>
</body>
</html>
```

## promise的练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>35-promise练习</title>
</head>
<body>
<script>
    /*
    需求:
    一次加载一张图片添加到body中. 前面图片加载失败后面图片不加载
    */
    let arr = [
        "http://www.it666.com/files/system/block_picture_1555415767.png",
        "http://www.it666.com/files/system/block_picture_1555422597.jpg",
        "http://wwww.it666.com/files/system/block_picture_1555419713.jpg"
    ];
    function loadImage(url) {
        return new Promise(function (resolve, reject) {
           let oImg = new Image();
           oImg.src = url;
           oImg.onload = function () {
               resolve(oImg);
           }
           oImg.onerror = function () {
               reject("图片加载失败");
           }
        });
    }
    /*
    let p2 = loadImage(arr[0]).then(function (oImg) {
        // console.log(oImg);
        console.log("1");
        document.body.appendChild(oImg);
        return loadImage(arr[1]);
    }, function (msg) {
        console.log(msg);
    });
    let p3 = p2.then(function (oImg) {
        console.log("2");
        // console.log(oImg);
        document.body.appendChild(oImg);
        return loadImage(arr[2]);
    }, function (msg) {
        console.log(msg);
    });
    p3.then(function (oImg) {
        console.log("3");
        // console.log(oImg);
        document.body.appendChild(oImg);
    }, function (msg) {
        console.log(msg);
    });
    */

    loadImage(arr[0]).then(function (oImg) {
        // console.log(oImg);
        console.log("1");
        document.body.appendChild(oImg);
        return loadImage(arr[1]);
    }).then(function (oImg) {
        console.log("2");
        // console.log(oImg);
        document.body.appendChild(oImg);
        return loadImage(arr[2]);
    }).then(function (oImg) {
        console.log("3");
        // console.log(oImg);
        document.body.appendChild(oImg);
    }).catch(function (msg) {
        console.log(msg);
    });
</script>
</body>
</html>
```

## promise的all方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>36-promise-all方法</title>
</head>
<body>
<script>
    /*
    需求:
    1.无序加载图片, 每加载成功一张就添加一张
    2.无序加载图片, 只有所有图片都加载成功才添加, 有一张图片失败都不添加
    */
    let arr = [
        "http://www.it666.com/files/system/block_picture_1555415767.png",
        "http://www.it666.com/files/system/block_picture_1555422597.jpg",
        "http://www.it666.com/files/system/block_picture_1555419713.jpg"
    ];
    function loadImage(url) {
        return new Promise(function (resolve, reject) {
            let oImg = new Image();
            let time = Math.random() * 1000;
            // console.log(time);
            setTimeout(function () {
                oImg.src = url;
            }, time);
            // oImg.src = url;
            oImg.onload = function () {
                resolve(oImg);
            }
            oImg.onerror = function () {
                reject("图片加载失败了");
            }
        });
    }
    /*
    for(let i = 0; i < arr.length; i++){
        loadImage(arr[i]).then(function (oImg) {
            console.log(i);
            document.body.appendChild(oImg);
        }, function (msg) {
            console.log(msg);
        });
    }
    */
    /*
    Promise的all静态方法:
    1.all方法接收一个数组,
    2.如果数组中有多个Promise对象,只有都成功才会执行then方法,
    并且会按照添加的顺序, 将所有成功的结果重新打包到一个数组中返回给我们
    3.如果数组中不是Promise对象, 那么会直接执行then方法

    应用场景: 批量加载, 要么一起成功, 要么一起失败
    * */
    /*
    let p1 = new Promise(function (resolve, reject) {
        // resolve("111");
        reject("aaa");
    });
    let p2 = new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve("222");
        }, 5000);
    });
    let p3 = new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve("333");
        }, 3000);
    });
    Promise.all([p1, p2, p3]).then(function (result) {
        console.log("成功", result);
    }, function (err) {
        console.log("失败", err);
    });
    */

    Promise.all([loadImage(arr[0]), loadImage(arr[1]),loadImage(arr[2])])
        .then(function (result) {
            // console.log(result);
            result.forEach(function (oImg) {
                document.body.appendChild(oImg);
            });
        })
        .catch(function (e) {
            console.log(e);
        });
</script>
</body>
</html>
```

## promise的race方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>38-promise-race方法</title>
</head>
<body>
<script>
    /*
    Promise的race静态方法:
    1.all方法接收一个数组,
    2.如果数组中有多个Promise对象, 谁先返回状态就听谁的, 后返回的会被抛弃
    3.如果数组中不是Promise对象, 那么会直接执行then方法

    应用场景: 接口调试, 超时处理
    * */
    /*
    let p1 = new Promise(function (resolve, reject) {
        // resolve("111");
        reject("aaa");
    });
    let p2 = new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve("222");
            // reject("bbb");
        }, 5000);
    });
    let p3 = new Promise(function (resolve, reject) {
        setTimeout(function () {
            resolve("333");
            // reject("ccc");
        }, 3000);
    });
    Promise.race([p1, p2, p3]).then(function (value) {
        console.log("成功", value);
    }).catch(function (e) {
        console.log("失败", e);
    });
    */

    let url = "http://www.it666.com/files/system/block_picture_1555415767.png";
    function loadImage(url) {
        return new Promise(function (resolve, reject) {
            let oImg = new Image();
            setTimeout(function () {
                oImg.src = url;
            }, 5000);
            oImg.onload = function () {
                resolve(oImg);
            }
            oImg.onerror = function () {
                reject("图片加载失败了");
            }
        });
    }
    function timeout() {
        return new Promise(function (resolve, reject) {
            setTimeout(function () {
                reject("超时了");
            }, 3000);
        });
    }
    Promise.race([loadImage(url), timeout()]).then(function (value) {
        console.log("成功", value);
    }).catch(function (e) {
        console.log("失败", e);
    });
</script>
</body>
</html>
```

## promise改造ajax

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>40-Promise-改造Ajax</title>
</head>
<body>
<script>
    function obj2str(data) {
        data = data || {};
        data.t = new Date().getTime();
        var res = [];
        for(var key in data){
            res.push(encodeURIComponent(key)+"="+encodeURIComponent(data[key]));
        }
        return res.join("&");
    }
    function ajax(option) {
        return new Promise(function (resolve, reject) {
            // 0.将对象转换为字符串
            var str = obj2str(option.data);
            // 1.创建一个异步对象
            var xmlhttp, timer;
            if (window.XMLHttpRequest){
                xmlhttp=new XMLHttpRequest();
            }else{
                xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
            }
            // 2.设置请求方式和请求地址
            if(option.type.toLowerCase() === "get"){
                xmlhttp.open(option.type, option.url+"?"+str, true);
                // 3.发送请求
                xmlhttp.send();
            }else{
                xmlhttp.open(option.type, option.url,true);
                // 注意点: 以下代码必须放到open和send之间
                xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
                xmlhttp.send(str);
            }

            // 4.监听状态的变化
            xmlhttp.onreadystatechange = function (ev2) {
                if(xmlhttp.readyState === 4){
                    clearInterval(timer);
                    // 判断是否请求成功
                    if(xmlhttp.status >= 200 && xmlhttp.status < 300 ||
                        xmlhttp.status === 304){
                        // 5.处理返回的结果
                        // console.log("接收到服务器返回的数据");
                        // option.success(xmlhttp);
                        resolve(xmlhttp);
                    }else{
                        // console.log("没有接收到服务器返回的数据");
                        // option.error(xmlhttp);
                        reject(xmlhttp);
                    }
                }
            }
            // 判断外界是否传入了超时时间
            if(option.timeout){
                timer = setInterval(function () {
                    console.log("中断请求");
                    xmlhttp.abort();
                    clearInterval(timer);
                }, option.timeout);
            }
        });
    }
</script>
<script>
    ajax({
        type:"post",
        url:"40.php",
        // success: function (xhr) {
        //     let str = xhr.responseText;
        //     let json = JSON.parse(str);
        //     console.log(json);
        // },
        // error: function (xhr) {
        //     console.log(xhr.status);
        // }
    }).then(function (xhr) {
        let str = xhr.responseText;
        let json = JSON.parse(str);
        console.log(json);
    }).catch(function (xhr) {
        console.log(xhr.status);
    });
</script>
</body>
</html>
```

## fetch的基本使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>41-fetch基本使用</title>
</head>
<body>
<script>
    /*
    1.什么是fetch?
    和Ajax一样都是用于请求网络数据的
    fetch是ES6中新增的, 基于Promise的网络请求方法

    2.fetch基本使用
    fetch(url, {options})
    .then()
    .catch();

    http://127.0.0.1/jQuery/Ajax/41.php
    */
    /*
    fetch("http://127.0.0.1/jQuery/Ajax/41.php?teacher=lnj&age=34", {
        method: "get"
    }).then(function (res) {
        // console.log(res.text());
        // return res.text();
        return res.json();
    }).then(function (data) {
        console.log(data);
        console.log(typeof data);
    }).catch(function (e) {
        console.log(e);
    });
    */
    fetch("http://127.0.0.1/jQuery/Ajax/41.php", {
        method: "post",
        body: JSON.stringify({teacher:"zq", age:666})
    }).then(function (res) {
        // console.log(res.text());
        // return res.text();
        return res.json();
    }).then(function (data) {
        console.log(data);
        console.log(typeof data);
    }).catch(function (e) {
        console.log(e);
    });
</script>
</body>
</html>
```

## fetch的简单封装

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>42-fetch简单封装</title>
</head>
<body>
<script>
    class EasyHttp{
        static obj2str(data) {
            data = data || {};
            data.t = new Date().getTime();
            var res = [];
            for(var key in data){
                res.push(encodeURIComponent(key)+"="+encodeURIComponent(data[key]));
            }
            return res.join("&");
        }
        static get(url, params){
            return new Promise(function (resolve, reject) {
                let newUrl = url;
                if(params !== undefined && params instanceof Object){
                    let str = EasyHttp.obj2str(params);
                    newUrl += "?" + str;
                }
                fetch(newUrl, {
                    method: "get"
                }).then(function (res) {
                    resolve(res.json());
                }).catch(function (e) {
                    reject(e);
                })
            })
        }
        static post(url, params){
            return new Promise(function (resolve, reject) {
                fetch(url, {
                    method: "post",
                    body: JSON.stringify(params)
                }).then(function (res) {
                    resolve(res.json());
                }).catch(function (e) {
                    reject(e);
                })
            })
        }
    }
    let obj = {
        teacher: "lnj",
        age: 34
    }
    EasyHttp.post("http://127.0.0.1/jQuery/Ajax/41.php", obj)
    .then(function (data) {
        console.log(data);
    })
    .catch(function (e) {
        console.log(e);
    });
</script>
</body>
</html>
```

## axios网络请求库

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>43-axios网络请求库</title>
</head>
<body>
<script src="js/axios.js"></script>
<script>
    /*
    1.什么是axios?
    Axios 是一个基于 promise 的 HTTP 库网络请求插件

    2.axios特点
    2.1可以用在浏览器和 node.js 中
    2.2支持 Promise API
    2.3自动转换 JSON 数据
    2.4客户端支持防御 XSRF
    */
    /*
    // axios.get("http://127.0.0.1/jQuery/Ajax/41.php")
    // axios.get("http://127.0.0.1/jQuery/Ajax/41.php?teacher=lnj&age=34")
    axios.post("http://127.0.0.1/jQuery/Ajax/41.php", {
        teacher: "lnj",
        age: 666
    })
        .then(function (res) {
            console.log(res.data);
        })
        .catch(function (e) {
            console.log(e);
        });
    */

    /*
    3.全局的 axios 默认值
    在企业开发中项目分为 :开发阶段和部署阶段, 这两个阶段项目存储的位置是不同的
    项目上线前存储在企业内部测试服务器上, 项目上线后存储在企业正式服务器上
    所以如果每次请求都将请求的地址写在请求中, 那么项目上线时需要大量修改请求地址
    为了解决这个问题, 我们可以配置一个全局URL根地址, 项目上线时只需要修改根地址即可
    例如: 上线前地址是: http://127.0.0.1/jQuery/Ajax/41.php
          上线后地址是: http://192.199.13.14/jQuery/Ajax/41.php
    */
    axios.defaults.timeout = 2000;
    axios.defaults.baseURL = "http://127.0.0.1";
    axios.post("/jQuery/Ajax/41.php", {
        teacher: "lnj",
        age: 666
    })
        .then(function (res) {
            console.log(res.data);
        })
        .catch(function (e) {
            console.log(e);
        });
</script>
</body>
</html>
```

## Symbol基本概念

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>44-Symbol基本概念</title>
</head>
<body>
<script>
    /*
    1.什么Symbol?
    Symbol是ES6中新增的一种数据类型, 被划分到了基本数据类型中
    基本数据类型: 字符串、数值、布尔、undefined、null、Symbol
    引用数据类型: Object

    2.Symbol的作用
    用来表示一个独一无二的值

    3.如果生成一个独一无二的值?
    let xxx = Symbol();
    */
    /*
    4.为什么需要Symbol?
    在企业开发中如果需要对一些第三方的插件、框架进行自定义的时候
    可能会因为添加了同名的属性或者方法, 将框架中原有的属性或者方法覆盖掉
    为了避免这种情况的发生, 框架的作者或者我们就可以使用Symbol作为属性或者方法的名称

    5.如何区分Symbol?
    在通过Symbol生成独一无二的值时可以设置一个标记
    这个标记仅仅用于区分, 没有其它任何含义
    */
    // let xxx = Symbol();
    // let yyy = Symbol();
    // console.log(xxx === yyy);

    /*
    let obj = {
        name: "lnj",
        say: function () {
            console.log("say");
        }
    }
    obj.name = "it666";
    console.log(obj.name);
    obj.say = function () {
        console.log("test");
    }
    obj.say();
    */
    let name = Symbol("name");
    let say = Symbol("say");
    let obj = {
        // 注意点: 如果想使用变量作为对象属性的名称, 那么必须加上[]
        [name]: "lnj",
        [say]: function () {
            console.log("say");
        }
    }
    // obj.name = "it666";
    obj[Symbol("name")] = "it666";
    console.log(obj);
</script>
</body>
</html>
```

## Symbol注意点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>45-Symbol注意点</title>
</head>
<body>
<script>
    // 1.通过Symbol生成独一无二值时需要在后面加上(), 但是前面不能加new, 因为它不是引用类型
    // let xxx = Symbol(); // 正确
    // let xxx = new Symbol(); // 错误

    // 2.通过Symbol生成独一无二值时传入的字符串仅仅是一个标记, 方便我们阅读代码, 没有其它任何意义
    // let xxx = Symbol("name");

    // 3.做类型转换的时候不能转换成数值
    // let xxx = Symbol("name");
    // console.log(String(xxx));
    // console.log(Boolean(xxx));
    // console.log(Number(xxx));

    // 4.不能做任何运算
    // let xxx = Symbol("name");
    // console.log(xxx + "abc");
    // console.log(xxx + 123);

    // 5.Symbol生成的值作为属性或方法名称时, 一定更要保存下来, 否则后续无法使用
    // let name = Symbol("name");
    // let obj = {
    //     // [name]: "lnj"
    //     [Symbol("name")]: "it666"
    // }
    // // console.log(obj[name]);
    // console.log(obj[Symbol("name")]);

    // 6.for循环无法遍历出Symbol的属性和方法
    let name = Symbol("name");
    let say = Symbol("say");
    let obj = {
        // 注意点: 如果想使用变量作为对象属性的名称, 那么必须加上[]
        [name]: "lnj",
        [say]: function () {
            console.log("say");
        },
        age: 34,
        gender: "man",
        hi: function () {
            console.log("hi");
        }
    }
    // for(let key in obj){
    //     console.log(key);
    // }
    console.log(Object.getOwnPropertySymbols(obj));
</script>
</body>
</html>
```

## iterator基本概念

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>46-Iterator基本概念</title>
</head>
<body>
<script>
    /*
    1.什么是Iterator?
    Iterator又叫做迭代器, 是一种接口
    这里的接口和现实中接口一样, 是一种标准一种规范
    例如: 电脑的USB接口有电脑USB接口的标准和规范, 正式因为有了标准和规范
          所以A厂商生成的USB线可以插到B厂商电脑的USB接口上

    它规定了不同数据类型统一访问的机制, 这里的访问机制主要指数据的遍历
    在ES6中Iterator接口主要供for...of消费

    2.默认情况下以下数据类型都实现的Iterator接口
    Array/Map/Set/String/TypedArray/函数的 arguments 对象/NodeList 对象
    */
    /*
    1.只要一个数据已经实现了Iterator接口, 那么这个数据就有一个叫做[Symbol.iterator]的属性
    2.[Symbol.iterator]的属性会返回一个函数
    3.[Symbol.iterator]返回的函数执行之后会返回一个对象
    4.[Symbol.iterator]函数返回的对象中又一个名称叫做next的方法
    5.next方法每次执行都会返回一个对象{value: 1, done: false}
    6.这个对象中存储了当前取出的数据和是否取完了的标记
    */
    /*
    // let arr = [1, 3, 5];
    // console.log(arr[Symbol.iterator]);
    // let it = arr[Symbol.iterator]();
    // console.log(it);
    // console.log(it.next());
    // console.log(it.next());
    // console.log(it.next());
    // console.log(it.next());

    // for(let value of arr){
    //     console.log(value);
    // }

    // let obj = {
    //     name: "lnj",
    //     age: 34,
    //     gender: "man"
    // }
    // console.log(obj[Symbol.iterator]);
    // for(let value of obj){
    //     console.log(value);
    // }
    */

    class MyArray{
        constructor(){
            for(let i = 0; i < arguments.length; i++){
                // this[0] = 1;
                // this[1] = 3;
                // this[2] = 5;
                this[i] = arguments[i];
            }
            this.length = arguments.length;
        }
        [Symbol.iterator](){
            let index = 0;
            let that = this;
            return {
                next(){
                    if(index < that.length){
                        return {value: that[index++], done: false}
                    }else{
                        return {value: that[index], done: true}
                    }
                }
            }
        }
    }
    let arr = new MyArray(1, 3, 5);
    // console.log(arr);
    // console.log(arr[0]);
    // arr[0] = 666;
    // console.log(arr);
    for(let value of arr){
        console.log(value);
    }
    // console.log(arr[Symbol.iterator]);
    // let it = arr[Symbol.iterator]();
    // console.log(it);
    // console.log(it.next());
    // console.log(it.next());
    // console.log(it.next());
    // console.log(it.next());
</script>
</body>
</html>
```

## iterator应用场景

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>47-Iterator应用场景</title>
</head>
<body>
<script>
    // 1.解构赋值
    class MyArray{
        constructor(){
            for(let i = 0; i < arguments.length; i++){
                // this[0] = 1;
                // this[1] = 3;
                // this[2] = 5;
                this[i] = arguments[i];
            }
            this.length = arguments.length;
        }
        [Symbol.iterator](){
            let index = 0;
            let that = this;
            return {
                next(){
                    if(index < that.length){
                        return {value: that[index++], done: false}
                    }else{
                        return {value: that[index], done: true}
                    }
                }
            }
        }
    }
    // let arr = [1, 3];
    // let arr = new MyArray(1, 3);
    // let [x, y, z] = arr;
    // console.log(x, y, z);

    // 2.扩展运算符
    // let arr1 = [1, 3];
    // let arr2 = [2, 4];
    let arr1 = new MyArray(1, 3);
    let arr2 = new MyArray(2, 4);
    let arr3 = [...arr1, ...arr2];
    console.log(arr3);
</script>
</body>
</html>
```

## generator函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>48-Generator函数基本概念</title>
</head>
<body>
<script>
    /*
    1.什么是Generator函数?
    Generator 函数是 ES6 提供的一种异步编程解决方案
    Generator 函数内部可以封装多个状态, 因此又可以理解为是一个状态机

    2.如何定义Generator函数
    只需要在普通函数的function后面加上*即可

    3.Generator函数和普通函数区别
    3.1调用Generator函数后, 无论函数有没有返回值, 都会返回一个迭代器对象,
    3.2调用Generator函数后, 函数中封装的代码不会立即被执行

    4.真正让Generator具有价值的是yield关键字
    4.1在Generator函数内部使用yield关键字定义状态
    4.2并且yield关键字可以让 Generator内部的逻辑能够切割成多个部分。
    4.3通过调用迭代器对象的next方法执行一个部分代码,
       执行哪个部分就会返回哪个部分定义的状态

    5.在调用next方法的时候可以传递一个参数, 这个参数会传递给上一个yield
    */
    function* gen() {
        console.log("123");
        let res = yield "aaa";

        console.log(res);
        console.log("567");
        yield 1 + 1;

        console.log("789");
        yield true;
    }
    let it = gen();
    // console.log(it);
    console.log(it.next());
    console.log(it.next("it666"));
    // console.log(it.next());
    // console.log(it.next());

    // 注意点: yield关键字只能在Generator函数中使用, 不能在普通函数中使用
    // function say() {
    //     yield "abc";
    // }
    // say();
</script>
</body>
</html>
```

## generator函数的应用场景

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>49-Generator函数应用场景</title>
</head>
<body>
<script>
    /*
    应用场景, 让函数返回多个值
    * */
    /*
    function calculate(a, b) {
        let sum = a + b;
        let sub = a - b;
        return [sum, sub];
    }
    */
    function* calculate(a, b) {
        yield a + b;
        yield a - b;
    }
    let it = calculate(10, 5);
    console.log(it.next().value);
    console.log(it.next().value);
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>50-Generator函数应用场景</title>
</head>
<body>
<script>
    /*
    1. 应用场景: 利用 Generator 函数，可以在任意对象上快速部署 Iterator 接口
    */
    /*
    Generator 函数特点
    1.Generator 函数也是一个函数
    2.Generator 函数会返回一个迭代器对象
    3.迭代器对象有next方法
    4.next方法每次执行都会返回一个对象{value: xxx, done: false}
    */
    /*
    function* gen() {
        yield "aaa";
        yield "bbb";
        yield "ccc";
    }
    let it = gen();
    // console.log(it);
    console.log(it.next());
    */

    /*
   1.必须有一个叫做[Symbol.iterator]的属性
   2.[Symbol.iterator]的属性会返回一个函数
   3.[Symbol.iterator]返回的函数执行之后会返回一个可迭代对象
   4.[Symbol.iterator]函数返回的对象中又一个名称叫做next的方法
   5.next方法每次执行都会返回一个对象{value: xxx, done: false}
   */
    /*
    let obj = {
        name: "lnj",
        age: 34,
        gender: "man",
        [Symbol.iterator](){
            let keys = Object.keys(this);
            // console.log(keys);
            let index = 0;
            let that = this;
            return {
                next(){
                    if(index < keys.length){
                        return {value: that[keys[index++]], done: false};
                    }else{
                        return {value: undefined, done: true};
                    }
                }
            }
        }
    }
    // console.log(obj[Symbol.iterator]);
    // let it = obj[Symbol.iterator]();
    // console.log(it);
    // console.log(it.next());
    // console.log(it.next());
    // console.log(it.next());
    // console.log(it.next());
    for(let value of obj){
        console.log(value);
    }
    */

    let obj = {
        name: "lnj",
        age: 34,
        gender: "man"
    }
    function* gen(){
        let keys = Object.keys(obj);
        for(let i = 0; i < keys.length; i++){
            yield obj[keys[i]];
        }
    }
    obj[Symbol.iterator] = gen;
    // console.log(obj[Symbol.iterator]);
    let it = obj[Symbol.iterator]();
    // console.log(it);
    console.log(it.next());
    console.log(it.next());
    console.log(it.next());
    console.log(it.next());
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>51-Generator函数应用场景</title>
</head>
<body>
<script>
    /*
    应用场景: 用同步的流程来表示异步的操作
    */
    /*
    function request(fn) {
        setTimeout(function () {
            fn("拿到的数据");
        }, 1000);
    }
    request(function (data) {
        console.log("1", data);
        request(function (data) {
            console.log("2", data);
            request(function (data) {
                console.log("3", data);
            });
        });
    });
    */
    function request() {
        return new Promise(function (resolve, reject) {
            setTimeout(function () {
                resolve("拿到的数据");
            }, 1000);
        });
    }
    /*
    request().then(function (data) {
        console.log(data, 1);
        return request();
    }).then(function (data) {
        console.log(data, 2);
        return request();
    }).then(function (data) {
        console.log(data, 3);
    });
    */
    function* gen() {
        yield request();
        yield request();
        yield request();
    }
    let it = gen();
    // console.log(it.next().value);
    it.next().value.then(function (data) {
        console.log(data, 1);
        return it.next().value;
    }).then(function (data) {
        console.log(data, 2);
        return it.next().value;
    }).then(function (data) {
        console.log(data, 3);
    });
</script>
</body>
</html>
```

## async函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>52-async函数</title>
</head>
<body>
<script>
    function request() {
        return new Promise(function (resolve, reject) {
            setTimeout(function () {
                resolve("拿到的数据");
            }, 1000);
        });
    }
    /*
    function* gen() {
        yield request();
        yield request();
        yield request();
    }
    let it = gen();
    it.next().value.then(function (data) {
        console.log(data, 1);
        return it.next().value;
    }).then(function (data) {
        console.log(data, 2);
        return it.next().value;
    }).then(function (data) {
        console.log(data, 3);
    });
    */
    async function gen() {
        let res1 = await request();
        console.log(res1, 1);
        let res2 = await request();
        console.log(res2, 2);
        let res3 = await request();
        console.log(res3, 3);
    }
    gen();
    /*
    1.async函数
    async函数是ES8中新增的一个函数, 用于定义一个异步函数
    async函数函数中的代码会自动从上至下的执行代码

    2.await操作符
    await操作符只能在异步函数 async function 中使用
    await表达式会暂停当前 async function 的执行，等待 Promise 处理完成。
    若 Promise 正常处理(fulfilled)，其回调的resolve函数参数作为 await 表达式的值，然后继续执行 async function。
    * */
</script>
</body>
</html>
```

# Zepto

## zepto初体验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-zepto初体验</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: skyblue;
        }
    </style>
    <!--<script src="js/jquery-3.1.1.js"></script>-->
    <script src="js/zepto.js"></script>
    <script src="js/event.js"></script>
</head>
<body>
<button>我是按钮</button>
<div></div>
<script>
    /*
    1. Zepto?
    Zepto 是一个轻量级的针对现代高级浏览器的 JavaScript库，
    它与jQuery有着类似的api, 如果你会用jQuery，那么你也会用Zepto

    2.既然和jQuery差不多, 为什么还需要Zepto?
    2.1jQuery更多是在PC端被应用，Zepto更多是在移动端被应用；
       也正是因为jQuery用在PC端, 所以jQuery考虑了很多低级浏览器的的兼容性问题, ,所以代码更多体积更大；
       也正是因为Zepto用在移动端, 所以Zepto则是直接抛弃了低级浏览器的适配问题 , 所以代码更少体积更小；
    2.2综上所述: Zepto其实就是专门用于移动端的轻量级的jQuery

    3.官方网址:
    英文版：http://zeptojs.com/
    中文版：http://www.css88.com/doc/zeptojs_api/
    */
    /*
    let oBtn = document.querySelector("button");
    oBtn.onclick = function () {
        $("div").css({backgroundColor: "red"});
    }
    */
    /*
    4.Zepto的特点
    Zepto采用了模块化的开发, 将不同的功能放到了不同的模块中,
    在使用的过程中我们可以按需导入, 也就是需要什么功能就导入什么模块
    * */
    $("button").click(function () {
        $("div").css({backgroundColor: "red"});
    });
    /*
    5.Zepto注意点
    如果是从官方网站下载的, 那么已经包含了默认的一些模块
    如果是从github下载的, 那么需要我们自己手动导入每一个模块
    当然后续学习了NodeJS后, 我们也可以自己定制
    * */
</script>
</body>
</html>
```

## zepto选择器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-zepto选择器</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: red;
            margin-bottom: 20px;
        }
        .one{
            background: green;
        }
        #two{
            background: blue;
        }
    </style>
    <script src="js/zepto.js"></script>
    <script src="js/event.js"></script>
    <script src="js/selector.js"></script>

</head>
<body>
<button>我是按钮</button>
<div></div>
<div class="one"></div>
<div id="two"></div>
<script>
    /*
    1.Zepto选择器
    Zepto是模块化开发的, zepto.js核心模块中只包含了基础功能
    如果想使用高级的选择器必须引入高级选择器模块
    * */
    $("button").click(function () {
        // $("div").css({backgroundColor: "yellow"});
        // $(".one").css({backgroundColor: "yellow"});
        // $("#two").css({backgroundColor: "yellow"});
        $("div:first").css({backgroundColor: "yellow"});
    });
</script>
</body>
</html>
```

## zepto动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-zepto动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: red;
            /*display: none;*/
        }
    </style>
    <script src="js/zepto.js"></script>
    <script src="js/event.js"></script>
    <script src="js/fx.js"></script>
    <script src="js/fx_methods.js"></script>
    <!--<script src="js/jquery-3.1.1.js"></script>-->

</head>
<body>
<button>我是按钮</button>
<div></div>
<script>
    /*
    1.zepto动画
    Zepto是模块化开发的, zepto.js核心模块中只包含了基础功能
    如果想使用动画必须引入动画模块

    2.zepto动画注意点
    由于zepto是一个轻量级的针对现代高级浏览器的 JavaScript库
    不需要兼容低级浏览器, 所以zepto中的动画是通过CSS3属性来实现动画的
    而jQuery中是通过DOM来实现动画的
    * */
    $("button").click(function () {
        // $("div").animate({marginLeft: 500}, 2000);
        // $("div").hide(2000);
        // $("div").show(2000);
        $("div").toggle(2000);
    });
</script>
</body>
</html>
```

## zepto-tap事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            margin: 0 auto;
            background: red;
        }
    </style>
    <!--<script src="js/jquery-3.1.1.js"></script>-->
    <script src="js/zepto.js"></script>
    <script src="js/event.js"></script>
    <script src="js/touch.js"></script>


</head>
<body>
<div></div>
<script>
    /*
    1.无论PC端还是移动端都支持click事件
    而且不仅仅是jQuery和Zepto支持, 原生的JS也支持
    * */
    /*
    let oDiv = document.querySelector("div");
    oDiv.onclick = function () {
        console.log("被点击了");
    }
    */
    /*
    $("div").click(function () {
        console.log("被点击了");
    });
    */
    /*
    2.移动端click事件注意点
    在企业开发中如果需要在移动端监听点击事件, 一般不会使用click来监听
    因为移动端的事件很多(单击/双击/轻扫/捏合/拖拽等等)
    所以如果通过click来监听,系统需要花费100~300毫秒判断到底是什么事件
    而移动端对事件的响应速度要求很高, 事件响应越快用户体验就越好
    所以如果需要在移动端监听点击事件, 那么请使用tap事件

    3.tap事件
    tap事件是Zepto自己封装的一个事件, 解决了原生click事件100~300毫秒的延迟问题
    * */
    $("div").tap(function () {
        console.log("被点击了");
    });
</script>
</body>
</html>
```

## 移动端Touch事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>05-移动端Touch事件</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: red;
        }
    </style>
</head>
<body>
<div></div>
<script>
    /*
    1.Zepto是如何实现tap事件的?
    虽然tap事件是Zepto自己封装的事件, 但是无论如何封装肯定都是通过原生JS来实现的
    在原生的JS中专门为移动端新增了几个事件67
    touchstart: 手指按下
    touchmove:  手指在元素上移动
    touchend :  手指抬起

    2.注意点:
    这几个事件只支持移动端, 不支持PC端
    * */
    let oDiv = document.querySelector("div");
    oDiv.ontouchstart = function () {
        console.log("手指按下");
    }
    oDiv.ontouchend = function () {
        console.log("手指抬起");
    }
    oDiv.ontouchmove = function () {
        console.log("手指移动");
    }
</script>
</body>
</html>
```

## 移动端Touch事件对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>06-移动端Touch事件对象</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 150px;
            height: 150px;
            display: inline-block;
            background: red;
        }
        .box2{
            background: blue;
        }
    </style>
</head>
<body>
<div class="box1"></div>
<div class="box2"></div>
<script>
    /*
    1.Touch事件对象
    移动端的touch事件也是一个事件, 所以被触发的时候系统也会自动传递一个事件对象给我们

    2.移动端touch事件对象中比较重要的三个子对象
    touches:        当前屏幕上所有手指的列表
    targetTouches:  保存了元素上所有的手指里列表
    changedTouches: 当前屏幕上刚刚接触的手指或者离开的手指
    */
    /*
    let oDiv1 = document.querySelector(".box1");
    oDiv1.ontouchstart = function (event) {
        console.log("touches1", event.touches);
        console.log("targetTouches1", event.targetTouches);
    }
    let oDiv2 = document.querySelector(".box2");
    oDiv2.ontouchstart = function (event) {
        console.log("touches2", event.touches);
        console.log("targetTouches2", event.targetTouches);
    }
    */
    /*
    let oDiv1 = document.querySelector(".box1");
    oDiv1.ontouchstart = function (event) {
        // console.log("touches1", event.touches);
        console.log("targetTouches1", event.targetTouches);
    }
    oDiv1.ontouchend = function (event) {
        // console.log("touches1", event.touches);
        console.log("targetTouches1", event.targetTouches);
    }
    */
    let oDiv1 = document.querySelector(".box1");
    oDiv1.ontouchstart = function (event) {
        console.log("按下", event.changedTouches);
    }
    oDiv1.ontouchend = function (event) {
        console.log("抬起", event.changedTouches);
    }
    /*
    touches和targetTouches
    如果都是将手指按到了同一个元素上, 那么这两个对象中保存的内容是一样的
    如果是将手指按到了不同的元素上, 那么这个两个对象中保存的内容不一样
    touches保存的是所有元素中的手指, 而targetTouches保存的是当前元素中的手指

    changedTouches
    在ontouchstart中保存的是刚刚新增的手指
    在ontouchend中保存的是刚刚离开的手指
    * */
</script>
</body>
</html>
```

## 移动端Touch事件位置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>07-移动端Touch事件位置</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 1000px;
            height: 100px;
            margin-left: 50px;
            margin-top: 50px;
            background: linear-gradient(to right, red, green);
        }
    </style>
</head>
<body>
<div></div>
<script>
    /*
    1.手指的位置
    1.screenX/screenY是相对于屏幕左上角的偏移位
    2.clientX/clientY是相对于可视区域左上角的偏移位
    3.pageX/pageY是相对于内容左上角的偏移位
    * */
    let oDiv = document.querySelector("div");
    oDiv.ontouchstart = function (event) {
        // console.log(event.targetTouches[0]);
        // console.log(event.targetTouches[0].screenX);
        // console.log(event.targetTouches[0].screenY);
        console.log(event.targetTouches[0].clientX); // 11
        console.log(event.targetTouches[0].clientY); // 63
        console.log(event.targetTouches[0].pageX);  // 686
        console.log(event.targetTouches[0].pageY);  // 63
    }
</script>
</body>
</html>
```

## 移动端手指位置练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>08-移动端手指位置练习</title>
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
<div></div>
<script>
    /*需求: 通过手指拖拽div*/
    let oDiv = document.querySelector("div");
    let startX = 0;
    let startY = 0;
    let flag = false;
    oDiv.ontouchstart = function (event) {
        if(flag){return}
        flag = true;
        startX = event.targetTouches[0].clientX;
        startY = event.targetTouches[0].clientY;
    }
    oDiv.ontouchmove = function (event) {
        let moveX = event.targetTouches[0].clientX;
        let moveY = event.targetTouches[0].clientY;
        let offsetX = moveX - startX;
        let offsetY = moveY - startY;
        oDiv.style.transform = `translate(${offsetX}px, ${offsetY}px)`;
    }
</script>
</body>
</html>
```

## zepto-tap事件原理

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>09-zepto-tap事件原理</title>
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
    <script src="js/myTap.js"></script>
</head>
<body>
<div></div>
<script>
    /*
    1.单击事件特点
    1.1只有一根手指
    1.2按下和离开时间不能太久 100ms
    1.3按下和离开距离不能太远 5px
    * */
    let oDiv = document.querySelector("div");
    /*
    let startX = 0;
    let startY = 0;
    let startTime = 0;
    oDiv.ontouchstart = function (event) {
        // 1.判断当前元素中有几根手指
        if(event.targetTouches.length > 1){
            return;
        }
        // 2.拿到手指按下的位置
        startX = event.targetTouches[0].clientX;
        startY = event.targetTouches[0].clientY;
        // 3.拿到手指按下的时间
        startTime = Date.now();
    }
    oDiv.ontouchend = function (event) {
        // 1.判断有几根手指离开了
        if(event.changedTouches.length > 1){
            return;
        }
        // 2.拿到离开手指的位置
        let endX = event.changedTouches[0].clientX;
        let endY = event.changedTouches[0].clientY;
        // 3.判断手指离开的位置和按下位置的距离
        if(Math.abs(endX - startX) > 5 ||
            Math.abs(endY - startY) > 5){
            return;
        }
        // 4.拿到手指离开的时间
        let endTime = Date.now();
        // 5.判断手指离开的时间和按下的时间
        if(endTime - startTime > 100){
            return;
        }
        console.log("单击事件");
    }
    */
    Tap(oDiv, function () {
        console.log("点击事件");
    });
</script>
</body>
</html>
```

```js
(function (window) {
    function Tap(dom, fn) {
        if(!(dom instanceof HTMLElement)){
            throw new Error("请传入一个DOM元素");
        }
        let startX = 0;
        let startY = 0;
        let startTime = 0;
        dom.ontouchstart = function (event) {
            // 1.判断当前元素中有几根手指
            if(event.targetTouches.length > 1){
                return;
            }
            // 2.拿到手指按下的位置
            startX = event.targetTouches[0].clientX;
            startY = event.targetTouches[0].clientY;
            // 3.拿到手指按下的时间
            startTime = Date.now();
        }
        dom.ontouchend = function (event) {
            // 1.判断有几根手指离开了
            if(event.changedTouches.length > 1){
                return;
            }
            // 2.拿到离开手指的位置
            let endX = event.changedTouches[0].clientX;
            let endY = event.changedTouches[0].clientY;
            // 3.判断手指离开的位置和按下位置的距离
            if(Math.abs(endX - startX) > 5 ||
                Math.abs(endY - startY) > 5){
                return;
            }
            // 4.拿到手指离开的时间
            let endTime = Date.now();
            // 5.判断手指离开的时间和按下的时间
            if(endTime - startTime > 100){
                return;
            }
            // console.log("单击事件");
            fn && fn();
        }
    }
    window.Tap = Tap;
})(window);
```

## 移动端点透问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>10-移动端点透问题</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            text-align: center;
            font-size: 40px;
        }
        .click{
            width: 300px;
            height: 300px;
            background: red;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            top: 100px;
        }
        .tap{
            width: 200px;
            height: 200px;
            background: blue;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            top: 150px;
        }
    </style>
</head>
<body>
<div class="click">click</div>
<div class="tap">tap</div>
<script>
    /*
    1.移动端点透问题
    当一个元素放覆盖了另一个元素, 覆盖的元素监听touch事件,而下面的元素监听click事件
    并且touch事件触发后覆盖的元素就消失了, 那么就会出现点透问题

    2.移动端点透问题出现的原因
    2.1当手指触摸到屏幕的时候，系统生成两个事件，一个是touch 一个是click
    2.2touch事件先执行,执行完后从文档上消失
    2.3click事件有100~300ms延迟, 所以后执行.
    2.4但click事件执行的时候触发的元素已经消失了, 对应的位置现在是下面的元素, 所以就触发了下面元素的click事件

    3.移动端点透问题解决方案
    在touch事件中添加event.preverDefault(); 阻止事件扩散
    * */
    let oClick = document.querySelector(".click");
    let oTap = document.querySelector(".tap");

    oTap.ontouchstart = function (event) {
        this.style.display = "none";
        event.preventDefault(); //  阻止事件扩散
    }
    oClick.onclick = function () {
        console.log("click");
    }
</script>
</body>
</html>
```

## 移动端点透问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-移动端点透问题</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            text-align: center;
            font-size: 40px;
        }
        .click{
            width: 300px;
            height: 300px;
            background: red;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            top: 100px;
        }
        .tap{
            width: 200px;
            height: 200px;
            background: blue;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            top: 150px;
        }
    </style>
    <!-- <script src="js/zepto.js"></script>
     <script src="js/touch.js"></script>-->
    <script src="js/fastclick.js"></script>
</head>
<body>
<div class="click">click</div>
<div class="tap">tap</div>
<script>
    /*
    1.移动端点透问题三种解决方案
    1.1在touch事件中添加event.preverDefault(); 阻止事件扩散
    1.2使用Zepto, 但是需要注意老版本的Zepto也会出现点透问题
    1.3使用Fastclick, 最早解决点透问题的插件
    * */
    if ('addEventListener' in document) {
        document.addEventListener('DOMContentLoaded', function() {
            FastClick.attach(document.body);
        }, false);
    }

    let oClick = document.querySelector(".click");
    let oTap = document.querySelector(".tap");
    /*
    oTap.ontouchstart = function (event) {
        this.style.display = "none";
        // event.preventDefault(); //  阻止事件扩散
    }
    */
    /*
    $(oTap).tap(function () {
        oTap.style.display = "none";
    });
    */
    // 注意点: 这里的click事件并不是原生的click事件, 是使用的fastclick中的click
    //         这里的click事件已经解决了100~300ms延迟的问题
    oTap.addEventListener("click", function () {
        oTap.style.display = "none";
    });
    oClick.onclick = function () {
        console.log("click");
    }
</script>
</body>
</html>
```

## zepto滑动事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>12-zepto-滑动事件</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            background: red;
            margin-left: 100px;
            margin-top: 100px;
        }
    </style>
    <script src="js/zepto.js"></script>
    <script src="js/event.js"></script>
    <script src="js/touch.js"></script>
    <script src="js/fx.js"></script>

</head>
<body>
<div></div>
<script>
    /*
    1.什么是轻扫事件?
    手指快速的往左边滑动/或者右边滑动/或者上边滑动/或者下边滑动
    * */
    // $("div").swipe(function () {
    //     console.log("轻扫");
    // });
    $("div").swipeLeft(function () {
        // console.log("向左边轻扫");
        $(this).animate({marginLeft: "0"}, 2000);
    });
    $("div").swipeRight(function () {
        // console.log("向右边轻扫");
        $(this).animate({marginLeft: "100px"}, 2000);
    });
    $("div").swipeUp(function () {
        // console.log("向上边轻扫");
        $(this).animate({marginTop: "0"}, 2000);
    });
    $("div").swipeDown(function () {
        // console.log("向下边轻扫");
        $(this).animate({marginTop: "100px"}, 2000);
    });
</script>
</body>
</html>
```

## 滑动事件练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>13-滑动事件练习</title>
    <style>
        *{
            margin: 0;
            padding: 0;
            touch-action: none;
        }
        .box{
            overflow: hidden;
        }
        .box>.box-top{
            height: 50px;
            position: relative;
        }
        .box>.box-top>ul{
            list-style: none;
            width: 100%;
            display: flex;
        }
        .box>.box-top>ul>li{
            height: 50px;
            line-height: 50px;
            text-align: center;
            flex-grow: 1;
            background: #6c6c6c;
        }
        .box>.box-top>ul>.active{
            color: #f40;
            background: #ccc;
        }
        .box>.box-top>.line{
            width: 50%;
            height: 2px;
            background: #f40;
            position: absolute;
            left: 0;
            bottom: 0;
        }
        .box>.box-bottom{
            width: 200%;
            display: flex;
            position: relative;
            top: 0;
            left: 0;
        }
        .box>.box-bottom>ul{
            list-style: none;
            flex-grow: 1;
        }
        .box>.box-bottom>ul>li{
            line-height: 30px;
            border-bottom: 1px solid #ccc;
        }
    </style>
    <script src="js/zepto.js"></script>
    <script src="js/event.js"></script>
    <script src="js/touch.js"></script>
    <script src="js/fx.js"></script>
    <script src="js/selector.js"></script>
</head>
<body>
<div class="box">
    <div class="box-top">
        <ul>
            <li class="active">新闻</li>
            <li>科技</li>
        </ul>
        <p class="line"></p>
    </div>
    <div class="box-bottom">
        <ul class="list1">
            <li>我是新闻1</li>
            <li>我是新闻2</li>
            <li>我是新闻3</li>
            <li>我是新闻4</li>
            <li>我是新闻5</li>
            <li>我是新闻6</li>
            <li>我是新闻7</li>
            <li>我是新闻8</li>
            <li>我是新闻9</li>
            <li>我是新闻10</li>
        </ul>
        <ul class="list2">
            <li>我是军事1</li>
            <li>我是军事2</li>
            <li>我是军事3</li>
            <li>我是军事4</li>
            <li>我是军事5</li>
            <li>我是军事6</li>
            <li>我是军事7</li>
            <li>我是军事8</li>
            <li>我是军事9</li>
            <li>我是军事10</li>
        </ul>
    </div>
</div>
<script>
    $(".box-top li").on("nj:click",function () {
        $(this).addClass("active").siblings().removeClass("active");
        let currentIndex = $(this).index();
        $(".line").animate({left: currentIndex * $(this).width() + "px"}, 500);
        $(".box-bottom").animate({left: -currentIndex * $(".list1").width() + "px"}, 500);
    });
    $(".box-top li").click(function () {
        $(this).trigger("nj:click");
    });
    $(".box").swipeLeft(function () {
        $(".box-top li:last").trigger("nj:click");
    });
    $(".box").swipeRight(function () {
        $(".box-top li:first").trigger("nj:click");
    });
</script>
</body>
</html>
```

# Swiper

## 移动端轮播图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>01-移动端轮播图</title>
    <style>
        *{
            margin: 0;
            padding: 0;
            touch-action: none;
        }
        div{
            width: 100%;
            position: relative;
            overflow: hidden;
        }
        ul{
            list-style: none;
            width: 500%;
            display: flex;
            justify-content: flex-start;
            position: relative;
            left: 0;
            top: 0;
        }
        ul>li{
            flex: 1;
        }
        ul>li>img{
            width: 100%;
            vertical-align: bottom;
        }
        div>p{
            width: 100%;
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            justify-content: space-between;
            /*background: deepskyblue;*/
            /*
            告诉浏览器当前元素不需要接收事件
            注意点: 如果父元素不接收事件, 那么默认子元素也不能接收事件
                    如果子元素需要接收事件, 那么必须单独设置为auto
            */
            pointer-events: none;
        }
        div>p>span{
            display: inline-block;
            width: 30px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            font-weight: bold;
            font-size: 30px;
            color: #fff;
            background: rgba(0,0,0,0.3);
            pointer-events: auto;
        }
        ol{
            list-style: none;
            position: absolute;
            left: 50%;
            bottom: 20px;
            transform: translateX(-50%);
            display: flex;
            justify-content: flex-start;
        }
        ol>li{
            width: 20px;
            height: 20px;
            background: #fff;
            border-radius: 50%;
            margin: 0 5px;
        }
        ol>.active{
            background: #f40;
        }
    </style>
    <script src="js/zepto.js"></script>
    <script src="js/selector.js"></script>
    <script src="js/event.js"></script>
    <script src="js/touch.js"></script>
    <script src="js/fx.js"></script>
</head>
<body>
<div>
    <ul>
        <li><img src="images/img1.jpg" alt=""></li>
        <li><img src="images/img2.jpg" alt=""></li>
        <li><img src="images/img3.jpg" alt=""></li>
        <li><img src="images/img4.jpg" alt=""></li>
        <li><img src="images/img1.jpg" alt=""></li>
    </ul>
    <p>
        <span class="left-btn">&lt;</span>
        <span class="right-btn">&gt;</span>
    </p>
    <ol>
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
    </ol>
</div>
<script>
    ;(function($){
        $.extend($.fn, {
            stop: function(){
                this.css({transition: "none"});
                return this
            },
            isAnimation: function(){
                let time = $("ul").css("transition-duration");
                time = parseFloat(time);
                return time !== 0;
            }
        })
    })(Zepto)
</script>
<script>
    // 1.定义变量保存对当前的索引
    let currentIndex = 0;
    // 2.定义变量保存图片宽度
    let itemWidth = $("ul>li").width();
    // 3.定义变量保存最大的索引
    let maxIndex = $("ul>li").length - 1;
    // 4.监听按钮点击
    $(".left-btn").click(function () {
        clearInterval(timerId);
        if($(this).isAnimation()){return}
        currentIndex--;
        if(currentIndex < 0){
            $("ul").css({left: -maxIndex * itemWidth});
            currentIndex = maxIndex - 1;
        }
        $("ul").animate({left: -currentIndex * itemWidth}, 500, function () {
            autoPlay();
        });
        $("ol>li").eq(currentIndex).addClass("active").siblings().removeClass();
    });
    $(".right-btn").click(function () {
        clearInterval(timerId);
        if($(this).isAnimation()){return}
        currentIndex++;
        if(currentIndex > maxIndex){
            $("ul").css({left: 0});
            currentIndex = 1;
        }
        $("ul").animate({left: -currentIndex * itemWidth}, 500, function () {
            autoPlay();
        });
        let index = currentIndex === 4 ? 0 : currentIndex;
        $("ol>li").eq(index).addClass("active").siblings().removeClass();
    });
    /*
    注意点:
    在移动端中是没有移入和移出事件的
    * */
    /*
    let oUl = document.querySelector("ul");
    oUl.onmouseenter = function () {
        console.log("移入");
    }
    oUl.onmouseleave = function () {
        console.log("移出");
    }
    */
    // 5.实现自动轮播
    let timerId = null;
    function autoPlay() {
        timerId = setInterval(function () {
            $(".right-btn").click();
        }, 2000);
    }
    autoPlay();
    $("ul").get(0).ontouchstart = function () {
        clearInterval(timerId);
    }
    $("ul").get(0).ontouchend = function () {
        autoPlay();
    }
    // 6.实现轻扫切换
    $("ul").swipeLeft(function () {
        $(".right-btn").click();
    });
    $("ul").swipeRight(function () {
        $(".left-btn").click();
    });
</script>
</body>
</html>
```

## swiper的基本使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-swiper基本使用</title>
    <link rel="stylesheet" href="css/swiper.css">
    <script src="js/swiper.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .swiper-container{
            width: 400px;
            height: 300px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .swiper-container>ul{
            list-style: none;
        }
    </style>
</head>
<body>
<div class="swiper-container">
    <ul class="swiper-wrapper">
        <li class="swiper-slide">Slide 1</li>
        <li class="swiper-slide">Slide 2</li>
        <li class="swiper-slide">Slide 3</li>
    </ul>
</div>
<script>
    /*
     1.什么是swiper?
     Swiper是纯javascript打造的滑动特效插件，面向PC、平板电脑等移动终端。
     Swiper能实现触屏焦点图、触屏Tab切换等常用效果。
     Swiper开源、免费、稳定、使用简单、功能强大，是架构移动终端网站的重要选择！
     
     2.如何使用:
     2.1引入swiper对应的css和js文件
     2.2按照框架的需求搭建三层结构
     2.3创建一个Swiper对象, 将容器元素传递给它
      */
    let mySwiper = new Swiper ('.swiper-container');
</script>
</body>
</html>
```

## swiper的高级使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-swiper高级使用</title>
    <link rel="stylesheet" href="css/swiper.css">
    <script src="js/swiper.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .swiper-container{
            width: 400px;
            height: 300px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .swiper-container>ul{
            list-style: none;
        }
        .swiper-pagination-bullet{
            background: red;
        }
        .swiper-button-next{
            background: red;
        }
    </style>
</head>
<body>
<div class="swiper-container">
    <ul class="swiper-wrapper">
        <li class="swiper-slide">Slide 1</li>
        <li class="swiper-slide">Slide 2</li>
        <li class="swiper-slide">Slide 3</li>
    </ul>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>
    <!-- 如果需要导航按钮 -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>
</div>
<script>
    let mySwiper = new Swiper ('.swiper-container', {
        // 如果需要分页器
        pagination: {
            el: '.swiper-pagination',
        },
        // 如果需要前进后退按钮
        navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
        },
        loop: true, // 循环模式选项
        // autoplay:true, // 自动轮播
        // autoplay: {
        //     delay: 1000,//1秒切换一次
        // },
        speed:5000, //设置切换速度
    });
</script>
</body>
</html>
```

## swiper移动端轮播图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>04-移动端轮播图</title>
    <link rel="stylesheet" href="css/swiper.css">
    <script src="js/swiper.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
            touch-action: none;
        }
        div{
            width: 100%;
        }
        ul{
            list-style: none;
        }
        ul>li>img{
            width: 100%;
            vertical-align: bottom;
        }
        .my-bullet{
            display: inline-block;
            width: 20px;
            height: 20px;
            background: #fff;
            border-radius: 50%;
            margin: 0 5px;
        }
        .my-bullet-active{
            background: #f40;
            opacity: 1;
        }
        .swiper-button-prev, .swiper-button-next{
            width: 30px;
            height: 50px;
            background: rgba(0,0,0,0.3);
        }
        .swiper-button-prev::before{
            content: "<";
            display: inline-block;
            width: 30px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            font-weight: bold;
            font-size: 30px;
            color: #fff;
        }
        .swiper-button-next::before{
            content: ">";
            display: inline-block;
            width: 30px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            font-weight: bold;
            font-size: 30px;
            color: #fff;
        }
    </style>
</head>
<body>
<div class="swiper-container">
    <ul class="swiper-wrapper">
        <li class="swiper-slide"><img src="images/img1.jpg" alt=""></li>
        <li class="swiper-slide"><img src="images/img2.jpg" alt=""></li>
        <li class="swiper-slide"><img src="images/img3.jpg" alt=""></li>
        <li class="swiper-slide"><img src="images/img4.jpg" alt=""></li>
    </ul>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>
    <!-- 如果需要导航按钮 -->
    <div class="swiper-button-prev"></div>
    <div class="swiper-button-next"></div>
</div>
<script>
    let mySwiper = new Swiper ('.swiper-container', {
        // 如果需要分页器
        pagination: {
            el: '.swiper-pagination',
            bulletClass : 'my-bullet',
            bulletActiveClass: 'my-bullet-active',
        },
        // 如果需要前进后退按钮
        navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
        },
        loop: true, // 循环模式选项
        autoplay: {
            delay: 2000,
        },
    });
</script>
</body>
</html>
```

## swiper-animation

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>05-swiper-animation</title>
    <link rel="stylesheet" href="css/swiper.css">
    <link rel="stylesheet" href="css/swiper.animate.min.css">
    <script src="js/swiper.js"></script>
    <script src="js/swiper.animate1.0.3.min.js"></script>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        p{
            width: 200px;
            height: 200px;
            line-height: 200px;
            text-align: center;
            background: #f00;
            margin: 0 auto;
        }
        @keyframes myFadeIn {
            0% {
                opacity: 0;
                transform: scale(2);
            }

            100% {
                opacity: 1;
                transform: scale(0.5);
            }
        }

        .myFadeIn {
            -webkit-animation-name: myFadeIn;
            animation-name: myFadeIn
        }
    </style>
</head>
<body>
<div class="swiper-container">
    <div class="swiper-wrapper">
        <div class="swiper-slide">
            <!--
            swiper-animate-effect：切换效果，例如 fadeInUp
            swiper-animate-duration：可选，动画持续时间（单位秒），例如 0.5s
            swiper-animate-delay：可选，动画延迟时间（单位秒），例如 0.3s
            -->
            <p class="ani" swiper-animate-effect="myFadeIn">内容</p>
        </div>
        <div class="swiper-slide">Slide 2</div>
        <div class="swiper-slide">Slide 3</div>
    </div>
</div>
<script>
    var mySwiper = new Swiper ('.swiper-container', {
        on:{
            init: function(){
                swiperAnimateCache(this); //隐藏动画元素 
                swiperAnimate(this); //初始化完成开始动画
            },
            slideChangeTransitionEnd: function(){
                swiperAnimate(this); //每个slide切换结束时也运行当前slide动画
            }
        }
    });
</script>
</body>
</html>
```

## 酷狗移动端页面

**项目文件见具体的代码**

## 动画插件代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-AnimateCSS使用</title>
    <link rel="stylesheet" href="css/animate.css">
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: red;
            margin: 100px auto;
            /*animation-iteration-count: 3;*/
            /*animation-delay: 6s;*/
        }
        @keyframes myFadeIn {
            from {
                opacity: 0;
                transform: scale(2);
            }

            to {
                opacity: 1;
                transform: scale(1);
            }
        }

        .myFadeIn {
            -webkit-animation-name: myFadeIn;
            animation-name: myFadeIn;
        }
    </style>
</head>
<body>
<!--animated这个类名是animated.css的基类, 但凡需要通过animated.css来添加动画, 都需要添加这个基类-->
<!--<div class="animated"></div>-->
<!--<div class="animated bounce"></div>-->
<!--<div class="animated bounce infinite delay-3s"></div>-->
<!--<div class="animated bounce"></div>-->
<div class="animated myFadeIn"></div>
<script>
    /*
        1.什么是Animate.css?
        其实swiper-animate就是参考Animate.css演变出来的一个插件,
        Animate.css和swiper-animate一样都是用于快速添加动画的,
        所以会用swiper-animate就会用Animate.css

        2.Animate.css的使用:
        2.1引入animate.css的文件
        2.2给需要执行动画的元素添加类名
    */
</script>
</body>
</html>
```

## wow.js的基本使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-WOWJS使用</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 400px;
            height: 200px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .box1{
            width: 200px;
            height: 200px;
            background: red;
            float: left;
        }
        .box2{
            width: 200px;
            height: 200px;
            background: blue;
            float: right;
        }
    </style>
    <link rel="stylesheet" href="css/animate.css">
    <script src="js/wow.js"></script>
</head>
<body>
<div class="box">
    <!--
    wow这个类名是一个基类, 如果想通过wow.js添加动画, 那么就必须写上这个基类
    这里的slideInLeft就是Animate.css中的动画名称, 只要是Animate.css中的动画再wow.js中都可以使用
    -->
    <div class="box1 wow slideInLeft" data-wow-duration="5s"></div>
    <div class="box2 wow slideInRight" data-wow-delay="5s" data-wow-iteration="2"></div>
</div>
<script>
    /*
    1.什么是WOW.js?
    WOW.js是对animate.css的扩充, 让页面滚动更有趣
    通过WOW.js，可以在页面滚动的过程中逐渐释放动画效果。
    简单点理解: (wow.js + animate.css) 约等于  (swiper.js + swiper.animate.css)
    只不过swiper更加强大, 企业中使用频率更高, 所以重点掌握swiper

    2.wow.js使用
    2.1引入animate.css
    2.2引入wow.js
    2.3给需要执行动画的元素添加类名
    2.4在JavaScript中初始化wow.js
     */
    new WOW().init();
</script>
</body>
</html>
```

