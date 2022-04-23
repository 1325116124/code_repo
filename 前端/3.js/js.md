# js基础

## 匿名函数

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>78-JavaScript-匿名函数</title>
    <script>
        /*
        1.什么是匿名函数?
        匿名函数就是没有名称的函数

        2.匿名函数的注意点
        匿名函数不能够只定义不使用

        3.匿名函数的应用场景
        3.1作为其他函数的参数
        3.2作为其他函数的返回值
        3.3作为一个立即执行的函数
        */
        /*
        // 有名称的函数
        // function say() {
        //     console.log("hello lnj");
        // }
        // let say = function() {
        //     console.log("hello lnj");
        // }

        // 匿名函数
        function() {
            console.log("hello lnj");
        }
        */

        // 3.1作为其他函数的参数
        /*
        function test(fn) { // let fn = say;
            fn();
        }
        test(function () {
            console.log("hello world");
        });
        */

        // 3.2作为其他函数的返回值
        /*
        function test() {
            return function () {
                console.log("hello lnj");
            };
        }
        let fn = test(); // let fn = say;
        fn();
        */

        // 3.3作为一个立即执行的函数
        // 注意点: 如果想让匿名函数立即执行, 那么必须使用()将函数的定义包裹起来才可以
        (function () {
            console.log("hello it666");
        })();
    </script>
</head>
<body>

</body>
</html>
```

## 箭头函数

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>79-JavaScript-箭头函数</title>
    <script>
        /*
        1.什么是箭头函数?
        箭头函数是ES6中新增的一种定义函数的格式
        目的: 就是为了简化定义函数的代码
        let arr = new Array();
        let arr = [];

        2.在ES6之前如何定义函数
        function 函数名称(形参列表){
            需要封装的代码;
        }
        let 函数名称 = function(形参列表){
            需要封装的代码;
        }

        3.从ES6开始如何定义函数
        let 函数名称 = (形参列表) =>{
            需要封装的代码;
        }

        4.箭头函数的注意点
        4.1在箭头函数中如果只有一个形参, 那么()可以省略
        4.2在箭头函数中如果{}中只有一句代码, 那么{}也可以省略
        */
        /*
        // function say() {
        //     console.log("hello lnj");
        // }
        let say = () => {
            console.log("hello lnj");
        }
        say();
        */

        // function say(name) {
        //     console.log("hello  " + name);
        // }
        // let say = (name) => {
        //     console.log("hello  " + name);
        // }
        // let say = name => {
        //     console.log("hello  " + name);
        // }
        let say = name => console.log("hello  " + name);
        say("it666");
    </script>
</head>
<body>

</body>
</html>
```

## 变量作用域问题（let\var）

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>80-JavaScript-变量作用域</title>
    <script>
        /*
        1.在JavaScript中定义变量有两种方式
        ES6之前: var 变量名称;
        ES6开始: let 变量名称;
        */
        // 2.两种定义变量方式的区别
        // 2.1是否能够定义同名变量
        /*
        1.通过var定义变量,可以重复定义同名的变量,并且后定义的会覆盖先定义的
        var num = 123;
        var num = 456;
        console.log(num);

        2.2如果通过let定义变量,  "相同作用域内"不可以重复定义同名的变量
        let num = 123;
        let num = 456; // 报错
        */

        // 2.2是否能够先使用后定义
        /*
        2.3通过var定义变量, 可以先使用后定义(预解析)
        console.log(num);
        var num = 123;

        2.4通过let定义变量, 不可以先使用再定义(不会预解析)
        console.log(num); // 报错
        let num = 123;
        */

        // 2.3是否能被{}限制作用域
        /*
        2.5无论是var还是let定义在{}外面都是全局变量
        var num = 123;
        let num = 123;

        2.6将var定义的变量放到一个单独的{}里面, 还是一个全局变量
        {
            var num = 123;
        }
        console.log(num);  //不会报错

        2.7将let定义的变量放到一个单独的{}里面, 是一个局部变量
        {
            let num = 123;
        }
        console.log(num);  //会报错
        */

        /*
        1.在JavaScript中{}外面的作用域, 我们称之为全局作用域

        2.在JavaScript中函数后面{}中的的作用域, 我们称之为"局部作用域"
        3.在ES6中只要{}没有和函数结合在一起, 那么应该"块级作用域"
        4.块级作用域和局部作用域区别
        4.1在块级作用域中通过var定义的变量是全局变量
        4.2在局部作用域中通过var定义的变量是局部变量

        5.无论是在块级作用域还是在局部作用域, 省略变量前面的let或者var就会变成一个全局变量
        */
        /*
        {
            // 块级作用域
        }
        if(false){
            // 块级作用域
        }
        while (false){
            // 块级作用域
        }
        for(;;){
            // 块级作用域
        }
        do{
            // 块级作用域
        }while (false);
        switch () {
            // 块级作用域
        }
        function say() {
            // 局部作用域
        }
        */

        /*
        {
            // 块级作用域
            var num = 123; // 全局变量
        }
        console.log(num);

        function test() {
            var value = 666; // 局部变量
        }
        test();
        console.log(value);
        */
        /*
        if(true){
            var num = 666;
        }
        console.log(num);
        */

        /*
        {
            // var num = 678; // 全局变量
            // let num = 678; // 局部变量
            num = 678; // 全局变量
        }
        console.log(num);
        */
        function test() {
            // var num = 123; // 局部变量
            // let num = 123; // 局部变量
            num = 123; // 全局变量
        }
        test();
        console.log(num);
    </script>
</head>
<body>

</body>
</html>
```

## es6之前的作用域链

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>83-JavaScript-作用域链上</title>
    <script>
        /*
        注意点: 初学者在研究"作用域链"的时候最好将ES6之前和ES6分开研究

        1.需要明确:
        1.ES6之前定义变量通过var
        2.ES6之前没有块级作用域, 只有全局作用域和局部作用域
        3.ES6之前函数大括号外的都是全局作用域
        4.ES6之前函数大括号中的都是局部作用域

        2.ES6之前作用域链
        1.1.全局作用域我们又称之为0级作用域
        2.2.定义函数开启的作用域就是1级/2级/3级/...作用域
        2.3.JavaScript会将这些作用域链接在一起形成一个链条, 这个链条就是作用域链
          0  --->  1 ---->  2  ---->  3 ----> 4
        2.4.除0级作用域以外, 当前作用域级别等于上一级+1

        3.变量在作用域链查找规则
        3.1先在当前找, 找到就使用当前作用域找到的
        3.2如果当前作用域中没有找到, 就去上一级作用域中查找
        3.3以此类推直到0级为止, 如果0级作用域还没找到, 就报错
        */

        // 全局作用域 / 0级作用域
        // var num = 123;
        function demo() {
            // 1级作用域
            // var num = 456;
            function test() {
                // 2级作用域
                // var num = 789;
                console.log(num);
            }
            test();
        }
        demo();
    </script>
</head>
<body>

</body>
</html>
```

## es6作用域链

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>84-JavaScript-作用域链上</title>
    <script>
        /*
        注意点: 初学者在研究作用域的时候最好将ES6之前和ES6分开研究

        1.需要明确:
        1.ES6定义变量通过let
        2.ES6除了全局作用域、局部作用域以外, 还新增了块级作用域
        3.ES6虽然新增了块级作用域, 但是通过let定义变量并无差异(都是局部变量)

        2.ES6作用域链
        1.1.全局作用域我们又称之为0级作用域
        2.2.定义函数或者代码块都会开启的作用域就是1级/2级/3级/...作用域
        2.3.JavaScript会将这些作用域链接在一起形成一个链条, 这个链条就是作用域链
          0  --->  1 ---->  2  ---->  3 ----> 4
        2.4.除0级作用域以外, 当前作用域级别等于上一级+1

        3.变量在作用域链查找规则
        3.1先在当前找, 找到就使用当前作用域找到的
        3.2如果当前作用域中没有找到, 就去上一级作用域中查找
        3.3以此类推直到0级为止, 如果0级作用域还没找到, 就报错
        */

        // 全局作用域 / 0级作用域
        // let num = 123;
        {
            // 1级作用域
            // let num = 456;
            function test() {
                // 2级作用域
                // let num = 789;
                console.log(num);
            }
            test();
        }
    </script>
</head>
<body>

</body>
</html>
```

## 预解析

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>85-JavaScript-预解析</title>
    <script>
        /*
        1.什么是预解析?
        浏览器在执行JS代码的时候会分成两部分操作：预解析以及逐行执行代码
        也就是说浏览器不会直接执行代码, 而是加工处理之后再执行,
        这个加工处理的过程, 我们就称之为预解析

        2.预解析规则
        2.1将变量声明和函数声明提升到当前作用域最前面
        2.2将剩余代码按照书写顺序依次放到后面

        3.注意点
        通过let定义的变量不会被提升(不会被预解析)
        */
        /*
        // 预解析之前
        console.log(num); //undefined
        var num = 123;
        // 预解析之后
        var num;
        console.log(num);
        num = 123;
        */
        /*
        // 不会预解析之前
        console.log(num); // 报错
        let num = 456;
        */

        // ES6之前定义函数的格式
        /*
        console.log(say);
        say();
        // ES6之前的这种定义函数的格式, 是会被预解析的, 所以可以提前调用
        function say() {
            console.log("hello it666");
        }
        */
        /*
        // 预解析之后的代码
        function say() {
            console.log("hello it666");
        }
        say();
        */

        /*
        console.log(say);
        say(); // say is not a function
        // 如果将函数赋值给一个var定义的变量, 那么函数不会被预解析, 只有变量会被预解析
        var say = function() {
            console.log("hello itzb");
        }
        */
        /*
        var say; //undefined
        say();
        say = function() {
            console.log("hello itzb");
        }
        */

        // ES6定义函数的格式
        say(); // say is not defined
        let say = () => {
            console.log("hello itzb");
        }

    </script>
</head>
<body>

</body>
</html>
```

## 创建默认对象

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>90-JavaScript-创建默认对象</title>
    <script>
        /*
        1.JavaScript中提供了一个默认的类Object, 我们可以通过这个类来创建对象
        2.由于我们是使用系统默认的类创建的对象, 所以系统不知道我们想要什么属性和行为, 所以我们必须手动的添加我们想要的属性和行为
        3.如何给一个对象添加属性
        对象名称.属性名称 = 值;
        4.如何给一个对象添加行为
        对象名称.行为名称 = 函数;
        */
        // 创建对象的第一种方式
        /*
        let obj = new Object();
        obj.name = "lnj";
        obj.age = 33;
        obj.say = function () {
            console.log("hello world");
        }
        console.log(obj.name);
        console.log(obj.age);
        obj.say();
        */

        // 创建对象的第二种方式
        /*
        let obj = {}; // let obj = new Object();
        obj.name = "lnj";
        obj.age = 33;
        obj.say = function () {
            console.log("hello world");
        }
        console.log(obj.name);
        console.log(obj.age);
        obj.say();
        */

        // 创建对象的第三种方式
        // 注意点: 属性名称和取值之间用冒号隔开, 属性和属性之间用逗号隔开
        let obj = {
            name: "lnj",
            age: 33,
            say: function () {
                console.log("hello world");
            }
        };
        console.log(obj.name);
        console.log(obj.age);
        obj.say();
    </script>
</head>
<body>

</body>
</html>
```

## 函数和方法的区别

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>91-JavaScript-函数和方法区别</title>
    <script>
        /*
        1.什么是函数?
        函数就是没有和其它的类显示的绑定在一起的, 我们就称之为函数

        2.什么是方法?
        方法就是显示的和其它的类绑定在一起的, 我们就称之为方法

        3.函数和方法的区别
        3.1函数可以直接调用, 但是方法不能直接调用, 只能通过对象来调用
        3.2函数内部的this输出的是window, 方法内部的this输出的是当前调用的那个对象

        4.无论是函数还是方法, 内部都有一个叫做this的东东
        this是什么? 谁调用了当前的函数或者方法, 那么当前的this就是谁
        */
        /*
        function demo() {
            // console.log("hello demo");
            console.log(this);
        }
        // demo();
        window.demo();
        */

        let obj = {
            name: "lnj",
            test: function () {
                // console.log("hello test");
                console.log(this);
            }
        }
        // test();
        obj.test();
        /* */
    </script>
</head>
<body>

</body>
</html>
```

## 工厂函数

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>93-JavaScript-工厂函数</title>
    <script>
        /*
        1.什么是工厂函数?
        工厂函数就是专门用于创建对象的函数, 我们就称之为工厂函数
        */
        /*
        let obj = {
            name: "lnj",
            age: 33,
            say: function () {
                console.log("hello world");
            }
        };
        let obj = {
            name: "zs",
            age: 44,
            say: function () {
                console.log("hello world");
            }
        };
        */
        function createPerson(myName, myAge) {
            let obj = new Object();
            obj.name = myName;
            obj.age = myAge;
            obj.say = function () {
                console.log("hello world");
            }
            return obj;
        }
        let obj1 = createPerson("lnj", 34);
        let obj2 = createPerson("zs", 44);
        console.log(obj1);
        console.log(obj2);
    </script>
</head>
<body>

</body>
</html>
```

## 构造函数

**普通的函数中定义构造函数**

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>94-JavaScript-构造函数</title>
    <script>
        /*
        1.什么是构造函数
        构造函数和工厂函数一样, 都是专门用于创建对象的
        构造函数本质上是工厂函数的简写

        2.构造函数和工厂函数的区别
        2.1构造函数的函数名称首字母必须大写
        2.2构造函数只能够通过new来调用
        */
        function Person(myName, myAge) {
            // let obj = new Object();  // 系统自动添加的
            // let this = obj; // 系统自动添加的
            this.name = myName;
            this.age = myAge;
            this.say = function () {
                console.log("hello world");
            }
            // return this; // 系统自动添加的
        }
        /*
        1.当我们new Person("lnj", 34);系统做了什么事情
        1.1会在构造函数中自动创建一个对象
        1.2会自动将刚才创建的对象赋值给this
        1.3会在构造函数的最后自动添加return this;
        */
        let obj1 = new Person("lnj", 34);
        let obj2 = new Person("zs", 44);
        console.log(obj1);
        console.log(obj2);
    </script>
</head>
<body>

</body>
</html>
```

## 普通构造函数的解析和问题

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>95-JavaScript-构造函数2</title>
    <script>
        /*
        function Person(myName, myAge) {
            // let obj = new Object();  // 系统自动添加的
            // let this = obj; // 系统自动添加的
            this.name = myName;
            this.age = myAge;
            this.say = function () {
                // 方法中的this谁调用就是谁, 所以当前是obj1调用, 所以当前的this就是obj1
                // console.log("hello world");
                console.log(this.name, this.age);
            }
            // return this; // 系统自动添加的
        }
        let obj1 = new Person("lnj", 34);
        // console.log(obj1.name);
        // console.log(obj1.age);
        obj1.say();

        let obj2 = new Person("zs", 44);
        obj2.say();
        */
        function Person(myName, myAge) {
            // let obj = new Object();  // 系统自动添加的
            // let this = obj; // 系统自动添加的
            this.name = myName;
            this.age = myAge;
            this.say = function () {
                console.log("hello world");
            }
            // return this; // 系统自动添加的
        }
        let obj1 = new Person("lnj", 34);
        let obj2 = new Person("zs", 44);
        // 由于两个对象中的say方法的实现都是一样的, 但是保存到了不同的存储空间中
        // 所以有性能问题
        console.log(obj1.say === obj2.say); // false

        /*
        function demo() {
            console.log("demo");
        }
        // 通过三个等号来判断两个函数名称, 表示判断两个函数是否都存储在同一块内存中
        console.log(demo === demo); // true
        */

    </script>
</head>
<body>

</body>
</html>
```

## 构造函数优化（方法的优化）

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>96-JavaScript-构造函数优化上</title>
    <script>
        function mySay() {
            console.log("hello world");
        }
        function Person(myName, myAge) {
            // let obj = new Object();  // 系统自动添加的
            // let this = obj; // 系统自动添加的
            this.name = myName;
            this.age = myAge;
            this.say = mySay;
            // return this; // 系统自动添加的
        }
        let obj1 = new Person("lnj", 34);
        let obj2 = new Person("zs", 44);
        console.log(obj1.say === obj2.say); // true

        /*
        1.当前这种方式解决之后存在的弊端
        1.1阅读性降低了
        1.2污染了全局的命名空间
        */
    </script>
</head>
<body>

</body>
</html>
```

## 构造函数优化二

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>97-JavaScript-构造函数优化中</title>
    <script>
        // function mySay() {
        //     console.log("hello world");
        // }
        let fns = {
            mySay: function () {
                console.log("hello world");
            }
        }
        function Person(myName, myAge) {
            // let obj = new Object();  // 系统自动添加的
            // let this = obj; // 系统自动添加的
            this.name = myName;
            this.age = myAge;
            this.say = fns.mySay;
            // return this; // 系统自动添加的
        }
        let obj1 = new Person("lnj", 34);
        let obj2 = new Person("zs", 44);
        console.log(obj1.say === obj2.say); // true
        /*
        let fns = {
            test: function () {
                console.log("test");
            }
        }
        // 由于test函数都是属于同一个对象, 所以返回true
        console.log(fns.test === fns.test); // true
        */
    </script>
</head>
<body>

</body>
</html>
```

## 构造函数优化三

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>98-JavaScript-构造函数优化下</title>
    <script>
        // let fns = {
        //     mySay: function () {
        //         console.log("hello world");
        //     }
        // }
        function Person(myName, myAge) {
            // let obj = new Object();  // 系统自动添加的
            // let this = obj; // 系统自动添加的
            this.name = myName;
            this.age = myAge;
            // this.say = fns.mySay;
            // return this; // 系统自动添加的
        }
        Person.prototype = {
            say: function () {
                console.log("hello world");
            }
        }
        let obj1 = new Person("lnj", 34);
        obj1.say();
        let obj2 = new Person("zs", 44);
        obj2.say();
        console.log(obj1.say === obj2.say); // true
        // console.log(Person.prototype);
    </script>
</head>
<body>

</body>
</html> 
```

## prototype

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>99-JavaScript-prototype特点</title>
    <script>
        /*
        1.prototype特点
        1.1存储在prototype中的方法可以被对应构造函数创建出来的所有对象共享
        1.2prototype中除了可以存储方法以外, 还可以存储属性
        1.3prototype如果出现了和构造函数中同名的属性或者方法, 对象在访问的时候, 访问到的是构造函中的数据

        2.prototype应用场景
        prototype中一般情况下用于存储所有对象都相同的一些属性以及方法
        如果是对象特有的属性或者方法, 我们会存储到构造函数中
        */
        function Person(myName, myAge) {
            this.name = myName;
            this.age = myAge;
            this.currentType = "构造函数中的type";
            this.say = function () {
                console.log("构造函数中的say");
            }
        }
        Person.prototype = {
            currentType: "人",
            say: function () {
                console.log("hello world");
            }
        }
        let obj1 = new Person("lnj", 34);
        obj1.say();
        console.log(obj1.currentType);
        let obj2 = new Person("zs", 44);
        obj2.say();
        console.log(obj2.currentType);
    </script>
</head>
<body>

</body>
</html>
```

## 对象三角恋关系

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>100-JavaScript-对象三角恋关系</title>
    <script>
        /*
        1.每个"构造函数"中都有一个默认的属性, 叫做prototype
          prototype属性保存着一个对象, 这个对象我们称之为"原型对象"
        2.每个"原型对象"中都有一个默认的属性, 叫做constructor
          constructor指向当前原型对象对应的那个"构造函数"
        3.通过构造函数创建出来的对象我们称之为"实例对象"
          每个"实例对象"中都有一个默认的属性, 叫做__proto__
          __proto__指向创建它的那个构造函数的"原型对象"
        */
        function Person(myName, myAge) {
            this.name = myName;
            this.age = myAge;
        }
        let obj1 = new Person("lnj", 34);

        console.log(Person.prototype);
        console.log(Person.prototype.constructor);
        console.log(obj1.__proto__);
    </script>
</head>
<body>

</body>
</html>
```

## Function函数

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>101-JavaScript-Function函数</title>
    <script>
      /*
        1.JavaScript中函数是引用类型(对象类型), 既然是对象,
          所以也是通过构造函数创建出来的,"所有函数"都是通过Function构造函数创建出来的对象
        2.JavaScript中只要是"函数"就有prototype属性
         "Function函数"的prototype属性指向"Function原型对象"
        3.JavaScript中只要"原型对象"就有constructor属性
          "Function原型对象"的constructor指向它对应的构造函数
        4.Person构造函数是Function构造函数的实例对象, 所以也有__proto__属性
          Person构造函数的__proto__属性指向"Function原型对象"
       */

      function Person(myName, myAge) {
        this.name = myName;
        this.age = myAge;
      }
      let obj1 = new Person("lnj", 34);

      console.log(Function);
      console.log(Function.prototype);
      console.log(Function.prototype.constructor);
      console.log(Function === Function.prototype.constructor); // true

      console.log(Person.__proto__);
      console.log(Person.__proto__ === Function.prototype); // true
    </script>
  </head>
  <body></body>
</html>
```

## Object函数

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>102-JavaScript-Object函数</title>
    <script>
        /*
        1. JavaScript函数是引用类型(对象类型), 所以Function函数也是对象
        2."Function构造函数"也是一个对象, 所以也有__proto__属性
          "Function构造函数"__proto__属性指向"Function原型对象"
        3. JavaScript中还有一个系统提供的构造函数叫做Object
           只要是函数都是"Function构造函数"的实例对象
        4.只要是对象就有__proto__属性, 所以"Object构造函数"也有__proto__属性
          "Object构造函数"的__proto__属性指向创建它那个构造函数的"原型对象"
        5.只要是构造函数都有一个默认的属性, 叫做prototype
          prototype属性保存着一个对象, 这个对象我们称之为"原型对象"
        6.只要是原型对象都有一个默认的属性, 叫做constructor
          constructor指向当前原型对象对应的那个"构造函数"
         */
        function Person(myName, myAge) {
            this.name = myName;
            this.age = myAge;
        }
        let obj1 = new Person("lnj", 34);
        // console.log(Function.__proto__);
        // console.log(Function.__proto__ === Function.prototype); // true

        // console.log(Object);
        // console.log(Object.__proto__);
        // console.log(Object.__proto__ === Function.prototype); // true
        // console.log(Object.prototype);
        // console.log(Object.prototype.constructor);

        // console.log(Object.prototype.constructor === Object); // true
        // console.log(Object.prototype.__proto__); // null
    </script>
</head>
<body>

</body>
</html>
```

## 函数对象关系

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>103-JavaScript-函数对象关系</title>
    <script>
        /*
        1.所有的构造函数都有一个prototype属性, 所有prototype属性都指向自己的原型对象
        2,所有的原型对象都有一个constructor属性, 所有constructor属性都指向自己的构造函数
        3.所有函数都是Function构造函数的实例对象
        4.所有函数都是对象, 包括Function构造函数
        5.所有对象都有__proto__属性
        6.普通对象的__proto__属性指向创建它的那个构造函数对应的"原型对象"
        7.所有对象的__proto__属性最终都会指向"Object原型对象"
        8."Object原型对象"的__proto__属性指向NULL
         */
        function Person(myName, myAge) {
            this.name = myName;
            this.age = myAge;
        }
        let obj1 = new Person("lnj", 34);

        console.log(Function.prototype.__proto__);
        console.log(Person.prototype.__proto__);
        console.log(Function.prototype.__proto__ === Person.prototype.__proto__);
        console.log(Function.prototype.__proto__ === Object.prototype);
        console.log(Person.prototype.__proto__ === Object.prototype);
    </script>
</head>
<body>

</body>
</html>
```

## 原型链

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>104-JavaScript-原型链</title>
    <script>
        /*
        1.对象中__proto__组成的链条我们称之为原型链
        2.对象在查找属性和方法的时候, 会先在当前对象查找
          如果当前对象中找不到想要的, 会依次去上一级原型对象中查找
          如果找到Object原型对象都没有找到, 就会报错
         */
        function Person(myName, myAge) {
            this.name = myName;
            this.age = myAge;
            // this.currentType = "构造函数中的type";
            // this.say = function () {
            //     console.log("构造函数中的say");
            // }
        }

        Person.prototype = {
            // 注意点: 为了不破坏原有的关系, 在给prototype赋值的时候, 需要在自定义的对象中手动的添加constructor属性, 手动的指定需要指向谁
            constructor: Person,
            // currentType: "人",
            // say: function () {
            //     console.log("hello world");
            // }
        }
        let obj1 = new Person("lnj", 34);
        // obj1.say();
        console.log(obj1.currentType);
        // console.log(Person.prototype.constructor);
    </script>
</head>
<body>

</body>
</html>
```

## 属性注意点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>105-JavaScript-属性注意点</title>
    <script>
        function Person(myName, myAge) {
            this.name = myName;
            this.age = myAge;
        }
        Person.prototype = {
            constructor: Person,
            currentType: "人",
            say: function () {
                console.log("hello world");
            }
        }
        let obj = new Person("lnj", 34);
        // console.log(obj.currentType); // "人"
        // console.log(obj.__proto__.currentType); // "人"

        // 注意点: 在给一个对象不存在的属性设置值的时候, 不会去原型对象中查找, 如果当前对象没有就会给当前对象新增一个不存在的属性
        obj.currentType = "新设置的值";
        console.log(obj.currentType); // 新设置的值
        console.log(obj.__proto__.currentType); // "人"
    </script>
</head>
<body>

</body>
</html>
```

## 封装性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>106-JavaScript-封装性</title>
    <script>
        /*
        1.局部变量和局部函数
        无论是ES6之前还是ES6, 只要定义一个函数就会开启一个新的作用域
        只要在这个新的作用域中, 通过let/var定义的变量就是局部变量
        只要在这个新的作用域中, 定义的函数就是局部函数

        2.什么是对象的私有变量和函数
        默认情况下对象中的属性和方法都是公有的, 只要拿到对象就能操作对象的属性和方法
        外界不能直接访问的变量和函数就是私有变量和是有函数
        构造函数的本质也是一个函数, 所以也会开启一个新的作用域, 所以在构造函数中定义的变量和函数就是私有和函数
        */
        /*
        3.什么是封装?
        封装性就是隐藏实现细节，仅对外公开接口

        4.为什么要封装?
        4.1不封装的缺点：当一个类把自己的成员变量暴露给外部的时候,那么该类就失去对属性的管理权，别人可以任意的修改你的属性
        4.2封装就是将数据隐藏起来,只能用此类的方法才可以读取或者设置数据,不可被外部任意修改. 封装是面向对象设计本质(将变化隔离)。这样降低了数据被误用的可能 (提高安全性和灵活性)
        */

        /*
        function demo() {
            var num = 123;
            let value = 456;
            function test() {
                console.log("test");
            }

            // console.log(num);
            // console.log(value);
            // test();
        }
        demo();
        console.log(num);
        console.log(value);
        test();
        */

        function Person() {
            this.name = "lnj";
            // this.age = 34;
            let age = 34;
            this.setAge = function (myAge) {
                if(myAge >= 0){
                    age = myAge;
                }
            }
            this.getAge = function () {
                return age;
            }
            this.say = function () {
                console.log("hello world");
            }
            /*
            // 由于构造函数也是一个函数, 所以也会开启一个新的作用域
            // 所以在构造函数中通过var/let定义的变量也是局部变量
            // 所以在构造函数中定义的函数也是局部函数
            var num = 123;
            let value = 456;
            function test() {
                console.log("test");
            }
            */
        }
        let obj = new Person();
        // 结论: 默认情况下对象的属性和方法都是公开的, 只要拿到对象就可以操作对象的属性和方法
        // console.log(obj.name);
        // obj.age = -3;
        // console.log(obj.age);
        // obj.say();

        // console.log(age);
        obj.setAge(-3);
        console.log(obj.getAge());
    </script>
</head>
<body>

</body>
</html>
```

## 私有属性注意点

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>107-JavaScript-私有属性注意点</title>
    <script>
        function Person() {
            this.name = "lnj";
            let age = 34;
            this.setAge = function (myAge) {
                if (myAge >= 0) {
                    age = myAge;
                }
            }
            this.getAge = function () {
                return age;
            }
            this.say = function () {
                console.log("hello world");
            }
        }
        let obj = new Person();
        // 1.操作的是私有属性(局部变量)
        obj.setAge(-3);
        console.log(obj.getAge());

        /*
        // 注意点:
        // 在给一个对象不存在的属性设置值的时候, 不会去原型对象中查找, 如果当前对象没有就会给当前对象新增一个不存在的属性
        // 由于私有属性的本质就是一个局部变量, 并不是真正的属性, 所以如果通过 对象.xxx的方式是找不到私有属性的, 所以会给当前对象新增一个不存在的属性
        */
        // 2.操作的是公有属性
        obj.age = -3;
        console.log(obj.age);

    </script>
</head>
<body>

</body>
</html>
```

## 属性方法分类

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>108-JavaScript-属性方法分类</title>
    <script>
        /*
        1.在JavaScript中属性和方法分类两类
        1.1实例属性/实例方法
        在企业开发中通过实例对象访问的属性, 我们就称之为实例属性
        在企业开发中通过实例对象调用的方法, 我们就称之为实例方法
        1.2静态属性/静态方法
        在企业开发中通过构造函数访问的属性, 我们就称之为静态属性
        在企业开发中通过构造函数调用的方法, 我们就称之为静态方法
        */
        function Person() {
            this.name = "lnj";
            this.say = function () {
                console.log("hello world");
            }
        }
        /*
        // 通过构造函数创建的对象, 我们称之为"实例对象"
        let obj = new Person();
        console.log(obj.name);
        obj.say();

        obj.age = 34;
        console.log(obj.age);
        obj.eat = function () {
            console.log("eat");
        }
        obj.eat();
        */

        // 构造函数也是一个"对象", 所以我们也可以给构造函数动态添加属性和方法
        Person.num = 666;
        Person.run = function () {
            console.log("run");
        }
        console.log(Person.num);
        Person.run();
    </script>
</head>
<body>

</body>
</html>
```

## 继承方式一

**通过手动更改构造函数的prototype和prototype.constructor来实现继承关系**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>109-JavaScript-继承方式一</title>
    <script>
        function Person() {
            this.name = null;
            this.age = 0;
            this.say = function () {
                console.log(this.name, this.age);
            }
        }
        let per = new Person();
        per.name = "lnj";
        per.age = 34;
        per.say();

        // 在企业开发中如果构造函数和构造函数之间的关系是is a关系, 那么就可以使用继承来优化代码, 来减少代码的冗余度
        // 学生 is a 人 , 学生是一个人
        function Student() {
            // this.name = null;
            // this.age = 0;
            // this.say = function () {
            //     console.log(this.name, this.age);
            // }
            this.score = 0;
            this.study = function () {
                console.log("day day up");
            }
        }
        Student.prototype = new Person();
        Student.prototype.constructor = Student;

        let stu = new Student();
        stu.name = "zs";
        stu.age = 18;
        stu.score = 99;
        stu.say();
        stu.study();
    </script>
</head>
<body>

</body>
</html>
```

## 继承方式一的弊端

**如果父构造函数中需要传递参数，那么在子构造函数中传递父构造函数需要的参数比较麻烦**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>110-JavaScript-继承方式一弊端</title>
    <script>
        function Person(myName, myAge) {
            this.name = myName;
            this.age = myAge;
            this.say = function () {
                console.log(this.name, this.age);
            }
        }
       // let per = new Person("lnj", 34);
       //  per.say();

        // 学生 is a 人 , 学生是一个人
        function Student(myName, myAge, myScore) {
            this.score = myScore;
            this.study = function () {
                console.log("day day up");
            }
        }
        // let stu = new Student();
        // stu.name = "zs";
        // stu.age = 18;
        // stu.score = 99;
        // stu.say();
        // stu.study();
    </script>
</head>
<body>

</body>
</html>
```

# DOM

## DOM开篇

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01-JavaScript-DOM开篇</title>
    <script>
        /*
        1.什么是window?
        window：是一个全局对象, 代表浏览器中一个打开的窗口, 每个窗口都是一个window对象

        2.什么是document?
        document是window的一个属性, 这个属性是一个对象
        document: 代表当前窗口中的整个网页,
        document对象保存了网页上所有的内容, 通过document对象就可以操作网页上的内容
        */

        /*
        3.什么是DOM?
        DOM 定义了访问和操作 HTML文档(网页)的标准方法
        DOM全称: Document Object Model, 即文档模型对象
        所以学习DOM就是学习如何通过document对象操作网页上的内容
        */

        console.log(window.document);
        console.log(typeof window.document);
        console.log(window.document.title);
    </script>
</head>
<body>

</body>
</html>
```

## 获取DOM元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-JavaScript-获取DOM元素上</title>
</head>
<body>
<!--
1.在JavaScript中HTML标签也称之为DOM元素
2.使用document的时候前面不用加window
var num = 666;
window.num;
num;
同理可证
window.document;
document;
-->
<div class="father">
    <form>
        <input type="text" name="test">
        <input type="password" name="test">
    </form>
</div>
<div class="father" id="box">我是div</div>

<script>
    /*
    1.通过id获取指定元素
    由于id不可以重复, 所以找到了就会将找到的标签包装成一个对象返回给我们, 找不到就返回Null
    注意点: DOM操作返回的是一个对象, 这个对象是宿主类型对象(浏览器提供的对象)
    */
    /*
    let oDiv = document.getElementById("box");
    console.log(oDiv);
    console.log(typeof oDiv);
    */

    /*
    2.通过class名称获取
    由于class可以重复, 所以找到了就返回一个存储了标签对象的数组, 找不到就返回一个空数组
    */
    /*
    let oDivs = document.getElementsByClassName("father");
    console.log(oDivs);
    */

    /*
    3.通过name名称获取
    由于name可以重复, 所以找到了就返回一个存储了标签对象的数组, 找不到就返回一个空数组
    注意点:
    getElementsByName  在不同的浏览器其中工作方式不同。在IE和Opera中， getElementsByName()  方法还会返回那些 id 为指定值的元素。
    */
    /*
    let oDivs = document.getElementsByName("test");
    console.log(oDivs);
    */

    /*
    4.通过标签名称获取
    由于标签名称可以重复, 所以找到了就返回一个存储了标签对象的数组, 找不到就返回一个空数组
    */
    /*
    let oDivs =  document.getElementsByTagName("div");
    console.log(oDivs);
    */

    /*
    5.通过选择器获取
    querySelector只会返回根据指定选择器找到的第一个元素
    */
    /*
    // let oDiv = document.querySelector("#box");
    // let oDiv = document.querySelector(".father");
    let oDiv = document.querySelector("div>form");
    console.log(oDiv);
    */

    /*
    6.通过选择器获取
    querySelectorAll会返回指定选择器找到的所有元素
    */
    let oDivs = document.querySelectorAll(".father");
    console.log(oDivs);
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>03-JavaScript-获取DOM元素下</title>
</head>
<body>
<div>
    <h1>1</h1>
    <h2>2</h2>
    <p class="item">3</p>
    <p>4</p>
    <span>5</span>
</div>
<script>
    // 1.获取指定元素所有的子元素
    let oDiv = document.querySelector("div");
    // children属性获取到的是指定元素中所有的子元素
    console.log(oDiv.children);
    // childNodes属性获取到的是指定元素中所有的节点
    // console.log(oDiv.childNodes);
    for(let node of oDiv.childNodes){
        // console.log(node.nodeType);
        // if(node.nodeType === 1){
        if(node.nodeType === Node.ELEMENT_NODE){
            console.log(node);
        }
    }

    /*
    2.什么是节点?
    DOM对象(document), 这个对象以树的形式保存了界面上所有的内容
    HTML页面每一部分都是由节点(标签(元素),文本,属性)
    */

    /*
    // 3.获取指定节点中的第一个子节点
    // let oDiv = document.querySelector("div");
    // console.log(oDiv.firstChild);
    //   获取指定元素中的第一个子元素
    // console.log(oDiv.firstElementChild);

    // 4.获取指定节点中最后一个子节点
    // console.log(oDiv.lastChild);
    // 4.获取指定元素中最后一个子元素
    // console.log(oDiv.lastElementChild);
    */

    // 5.通过子元素获取父元素/父节点
    let item = document.querySelector(".item");
    console.log(item.parentElement);
    console.log(item.parentNode);
    // let parentEle = item.parentElement || item.parentNode;
    // console.log(parentEle);

    // 6.获取相邻上一个节点
    // console.log(item.previousSibling);
    //   获取相邻上一个元素
    // console.log(item.previousElementSibling);

    // 7.获取相邻下一个节点
    console.log(item.nextSibling);
    //   获取相邻下一个元素
    console.log(item.nextElementSibling);
</script>
</body>
</html>
```

## 节点的增删改查

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-JavaScript-元素增删改查</title>
</head>
<body>
<div>
    <h1>我是标题</h1>
    <p>我是段落</p>
</div>
<script>
    // 1.创建节点
    // let oSpan = document.createElement("span");
    // console.log(oSpan);
    // console.log(typeof oSpan);

    // 2.添加节点
    // 注意点: appendChild方法会将指定的元素添加到最后
    // let oDiv = document.querySelector("div");
    // oDiv.appendChild(oSpan)
    // let oA = document.createElement("a");
    // oDiv.appendChild(oA);

    // 3.插入节点
    // let oSpan = document.createElement("span");
    // let oDiv = document.querySelector("div");
    // let oH1 = document.querySelector("h1");
    // let oP = document.querySelector("p");
    // // oDiv.insertBefore(oSpan, oH1);
    // oDiv.insertBefore(oSpan, oP);

    // 5.删除节点
    // 注意点: 在js中如果想要删除某一个元素, 只能通过对应的父元素来删除
    //         元素是不能够自杀的
    // console.log(oSpan.parentNode);
    // oSpan.parentNode.removeChild(oSpan);
    // oDiv.parentNode.removeChild(oDiv);

    // 5.克隆节点
    // 注意点: cloneNode方法默认不会克隆子元素, 如果想克隆子元素需要传递一个true
    let oDiv = document.querySelector("div");
    // let newDiv =  oDiv.cloneNode();
    let newDiv =  oDiv.cloneNode(true);
    console.log(newDiv);
</script>
</body>
</html>
```

## 元素属性操作

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-JavaScript-元素属性操作</title>
</head>
<body>
<img src="images/1.jpg" alt="我是alt222" title="我是title" nj="666">
<script>
    /*
    无论是通过document创建还是查询出来的标签,系统都会将元素包装成一个对象返回给我们,
    系统在包装这个对象的时候会自动将元素的属性都包装到这个对象中,
    所以只要拿到这个对象就可以拿到标签属性,操作标签属性
    */
    /*
    1.如何获取元素属性
    2.如何修改元素属性
    3.如何新增元素属性
    4.如何删除元素属性
    */
    /*
    // let oImg = document.querySelector("img");
    // let oImg = document.createElement("img");
    console.dir(oImg);
    */

    // 1.如何获取元素属性
    /*
    let oImg = document.querySelector("img");
    // console.log(oImg.alt);
    // console.log(oImg.getAttribute("alt"));
    // 注意点: 通过对象.属性名称的方式无法获取到自定义属性的取值
    //         通过getAttribute方法可以获取到自定义属性的取值
    console.log(oImg.nj);
    console.log(oImg.getAttribute("nj"));
    */

    // 2.如何修改元素属性
    /*
    let oImg = document.querySelector("img");
    // oImg.title = "新的title";
    // oImg.setAttribute("title", "新的title222");
    // 注意点和获取元素属性一样
    // oImg.nj = "123";
    oImg.setAttribute("nj", "123");
    */

    // 3.如何新增元素属性
    /*
    let oImg = document.querySelector("img");
    // oImg.it666 = "itzb";
    // 注意点: setAttribute方法如果属性不存在就是新增, 如果属性存在就是修改
    oImg.setAttribute("it666", "itzb");
    */

    // 4.如何删除元素属性
    let oImg = document.querySelector("img");
    // oImg.alt = "";
    // oImg.removeAttribute("alt");
    // 注意点和获取元素属性一样
    // oImg.nj = "";
    oImg.removeAttribute("nj");
</script>
</body>
</html>
```

## 元素内容操作

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>06-JavaScript-元素内容操作</title>
</head>
<body>
<div>
    我是div
    <h1>我是标题</h1>
    <p>我是段落</p>
</div>
<script>
   // 1.获取元素内容
   /*
   1.innerHTML获取的内容包含标签, innerText/textContent获取的内容不包含标签
   2.innerHTML/textContent获取的内容不会去除两端的空格, innerText获取的内容会去除两端的空格
   */
   /*
   let oDiv = document.querySelector("div");
   console.log(oDiv.innerHTML);
   console.log(oDiv.innerText);
   console.log(oDiv.textContent);
   */

   // 2.设置元素内容
   /*
   特点:
   无论通过innerHTML/innerText/textContent设置内容, 新的内容都会覆盖原有的内容
   区别:
   如果通过innerHTML设置数据, 数据中包含标签, 会转换成标签之后再添加
   如果通过innerText/textContent设置数据, 数据中包含标签, 不会转换成标签, 会当做一个字符串直接设置
   */
    let oDiv = document.querySelector("div");
   // oDiv.innerHTML = "123";
   // oDiv.innerText = "456";
   // oDiv.textContent = "789";
   //  oDiv.innerHTML = "<span>我是span</span>";
   //  oDiv.innerText = "<span>我是span</span>";
   //  oDiv.textContent = "<span>我是span</span>";

    setText(oDiv, "www.it666.com");
    function setText(obj, text) {
        if("textContent" in obj){
            obj.textContent = text;
        }else{
            obj.innerText = text;
        }
    }
</script>
</body>
</html>
```

## 操作元素样式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>07-JavaScript-操作元素样式</title>
    <style>
        .box{
            width: 200px;
            height: 200px;
            background-color: red;
        }
    </style>
</head>
<body>
<div class="box"></div>
<script>
    // 1.设置元素样式
    /*
    let oDiv = document.querySelector("div");
    // 第一种方式
    // 注意点: 由于class在JS中是一个关键字, 所以叫做className
    // oDiv.className = "box";
    // 第二种方式
    // 注意点: 过去CSS中通过-连接的样式, 在JS中都是驼峰命名
    // 注意点: 通过JS添加的样式都是行内样式, 会覆盖掉同名的CSS样式
    oDiv.style.width = "300px";
    oDiv.style.height = "300px";
    oDiv.style.backgroundColor = "blue";
    */

    // 2.获取元素样式
    let oDiv = document.querySelector("div");
    // oDiv.style.width = "300px";
    // 注意点: 通过style属性只能过去到行内样式的属性值, 获取不到CSS设置的属性值
    // console.log(oDiv.style.width);
    // 注意点: 如果想获取到CSS设置的属性值, 必须通过getComputedStyle方法来获取
    // getComputedStyle方法接收一个参数, 这个参数就是要获取的元素对象
    // getComputedStyle方法返回一个对象, 这个对象中就保存了CSS设置的样式和属性值
    let style = window.getComputedStyle(oDiv);
    console.log(style.width);
</script>
</body>
</html>
```

## DOM事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08-JavaScript-DOM事件</title>
</head>
<body>
<button>我是按钮</button>
<a href="http://www.it666.com">我是a标签</a>
<script>
    /*
    1.什么是事件?
    用户和浏览器之间的交互行为我们就称之为事件,	比如：点击，移入/移出

    2.如何给元素绑定事件?
    在JavaScript中所有的HTML标签都可以添加事件
    元素.事件名称 = function(){};
    当对应事件被触发时候就会自动执行function中的代码
    */
    let oBtn = document.querySelector("button");
    oBtn.onclick = function () {
        alert("按钮被点击了");
    }
    // 注意点: 如果给元素添加了和系统同名的事件, 我们添加的事件不会覆盖系统添加的事件
    let oA = document.querySelector("a");
    oA.onclick = function () {
        alert("a标签被点击了");
        // 以下代码的含义: 用我们添加的事件覆盖掉系统同名的事件
        return false;
    }
</script>
</body>
</html>
```

## 定时器

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09-JavaScript-定时器</title>
</head>
<body>
<button id="start">开始</button>
<button id="close">结束</button>
<script>
    /*
    在JavaScript中有两种定时器, 一种是重复执行的定时器, 一种是只执行一次的定时器
    */
    // 1.重复执行的定时器
    /*
    // setInterval(function () {
    //     console.log("随便写点");
    // }, 1000);
    let startBtn = document.querySelector("#start");
    let id = null;
    startBtn.onclick = function () {
        id = setInterval(function () {
            console.log("随便写点");
        }, 1000);
    }
    let closeBtn = document.querySelector("#close");
    closeBtn.onclick = function () {
        clearInterval(id);
    }
    */

    // 2.只执行一次的定时器
    // window.setTimeout(function () {
    //     console.log("随便写点");
    // }, 5000);
    let startBtn = document.querySelector("#start");
    let closeBtn = document.querySelector("#close");
    let id = null;
    startBtn.onclick = function () {
        id = window.setTimeout(function () {
            console.log("随便写点");
        }, 5000);
    }
    closeBtn.onclick = function () {
        clearTimeout(id);
    }
</script>
</body>
</html>
```

## 关闭广告

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>10-JavaScript-关闭广告</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            position: fixed;
            left: 0;
            bottom: 0;
        }
        div>img:first-child{
            position: absolute;
            right: 0;
            top: -17px;
        }
    </style>
</head>
<body>
<div>
    <img src="images/close.jpg" alt="">
    <img src="images/sina-ad.png" alt="">
</div>
<script>
    let oImgBtn = document.querySelector("div>img:first-child");
    oImgBtn.onclick = function () {
        // 1.找到按钮的父元素
        let oDiv = this.parentNode;
        // console.log(oDiv);
        // 2.删除广告元素
        oDiv.parentNode.removeChild(oDiv);
    }
</script>
</body>
</html>
```

## 图片展示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>11-JavaScript-图片展示</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 670px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        ul{
            list-style: none;
            display: flex;
            justify-content: space-between;
        }
        ul>li>img{
            width: 120px;
            vertical-align: bottom;
        }
    </style>
</head>
<body>
<div>
    <img src="images/ad1.jpg" alt="">
    <ul>
        <li><img src="images/ad1.jpg" alt=""></li>
        <li><img src="images/ad2.jpg" alt=""></li>
        <li><img src="images/ad3.jpg" alt=""></li>
        <li><img src="images/ad4.jpg" alt=""></li>
        <li><img src="images/ad5.jpg" alt=""></li>
    </ul>
</div>
<script>
    let oImg = document.querySelector("div>img");
    let oItems = document.querySelectorAll("li>img");
    // console.log(oItems);
    for(let item of oItems){
        /*
        item.onclick = function () {
            // console.log(this.src);
            oImg.src = this.src;
        }
        */
        item.onclick = change;
    }
    function change() {
        // console.log(this.src);
        oImg.src = this.src;
    }

    /*
    let obj1 = {name:"zs"};
    let obj2 = {name:"ls"};
    obj1.say = function () {
        console.log("hello");
    }
    obj2.say = function () {
        console.log("hello");
    }
    console.log(obj1.say === obj2.say); // false
    */

    /*
    let obj1 = {name:"zs"};
    let obj2 = {name:"ls"};
    function say() {
        console.log("hello");
    }
    obj1.say = say;
    obj2.say = say;
    console.log(obj1.say === obj2.say); // true
    */
</script>
</body>
</html>
```

## 简易轮播图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-JavaScript-简易轮播图</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 670px;
            border: 1px solid #000;
            margin: 100px auto;
            position: relative;
        }
        div>img{
            vertical-align: bottom;
        }
        p{
            position: absolute;
            width: 100%;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            justify-content: space-between;
        }
        p>span{
            display: inline-block;
            width: 30px;
            height: 60px;
            line-height: 60px;
            text-align: center;
            font-size: 40px;
            color: #fff;
            background: rgba(0,0,0,0.5);
        }
    </style>
</head>
<body>
<div>
    <img src="images/ad1.jpg" alt="">
    <p>
        <span id="pre">&lt;</span>
        <span id="next">&gt;</span>
    </p>
</div>
<script>
    // 1.获取到需要操作的元素
    let oImg = document.querySelector("img");
    let preBtn = document.querySelector("#pre");
    let nextBtn = document.querySelector("#next");
    // 2.定义数组保存所有需要展示的图片
    // 注意点: 在企业开发中都是通过网络请求去服务器上获取
    let images = [
        "images/ad1.jpg",
        "images/ad2.jpg",
        "images/ad3.jpg",
        "images/ad4.jpg",
        "images/ad5.jpg",
    ];
    let currentIndex = 0;
    let maxIndex = images.length - 1;
    // 3.监听按钮的点击事件
    preBtn.onclick = function () {
        currentIndex--;
        if(currentIndex < 0){
            currentIndex = maxIndex;
        }
        oImg.src = images[currentIndex];
    }
    nextBtn.onclick = function () {
        currentIndex++;
        if(currentIndex > maxIndex){
            currentIndex = 0;
        }
        oImg.src = images[currentIndex];
    }
</script>
</body>
</html>
```

## 移动事件

**onmouseenter / onmouseover**

**onmouseleave / onmouseout**

**onmousemove**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>13-JavaScript-移入移出事件</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 300px;
            height: 300px;
            background: red;
        }
    </style>
</head>
<body>
<div></div>
<script>
    let oDiv = document.querySelector("div");
    // 1.移入事件
    // oDiv.onmouseover = function () {
    //     console.log("移入事件");
    // }
    // 注意点: 对于初学者来说, 为了避免未知的一些BUG, 建议使用onmouseenter
    // oDiv.onmouseenter = function () {
    //     console.log("移入事件");
    // }

    // 2.移出事件
    // oDiv.onmouseout = function () {
    //     console.log("移出事件");
    // }
    // 注意点: 对于初学者来说, 为了避免未知的一些BUG, 建议使用onmouseleave
    // oDiv.onmouseleave = function () {
    //     console.log("移出事件");
    // }
    // 3.移动事件
    oDiv.onmousemove = function () {
        console.log("移动事件");
    }
</script>
</body>
</html>
```

## 移入移出菜单

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>14-JavaScript-移入移出菜单</title>
    <link rel="stylesheet" href="fonts/iconfont.css">
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            position: fixed;
            right: 0;
            top: 50%;
            transform: translateY(-50%);
        }
        ul{
            list-style: none;
            width: 50px;
            height: 150px;
            background: skyblue;
            border-radius: 5px;
        }
        ul>li{
            width: 50px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            border-bottom: 1px solid #ccc;
            color: #fff;
            font-size: 20px;
        }
        ul>li:last-child{
            border-bottom: none;
        }
        .subMenu{
            width: 250px;
            height: 270px;
            padding: 20px;
            box-sizing: border-box;
            background: #ccc;
            border-radius: 5px;
            position: absolute;
            top: 50%;
            transform: translateY(-50%);
            right: 60px;
            display: none;
        }
        .subMenu>img{
            width: 210px;
        }
    </style>
</head>
<body>
<div>
    <ul>
        <li class="iconfont icon-qq"></li>
        <li class="iconfont icon-weixin" id="weixin"></li>
        <li class="iconfont icon-youjian"></li>
    </ul>
    <div class="subMenu">
        <p>微信二维码</p>
        <img src="images/qrcode.png" alt="">
    </div>
</div>
<script>
    let weixin = document.querySelector("#weixin");
    let subMenu = document.querySelector(".subMenu");
    weixin.onmouseenter = function () {
        subMenu.style.display = "block";
    }
    weixin.onmouseleave = function () {
        subMenu.style.display = "none";
    }
</script>
</body>
</html>
```

## 商品展示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>15-JavaScript-商品展示</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 430px;
            margin: 100px auto;
            border: 1px solid #000;
        }
        ul{
            list-style: none;
            display: flex;
            justify-content: space-between;
        }
        ul>li>img{
            width: 80px;
            height: 80px;
            vertical-align: bottom;
        }
        ul>li{
            border: 2px solid transparent;
            box-sizing: border-box;
        }
        .current{
            border: 2px solid skyblue;
        }
    </style>
</head>
<body>
<div>
    <img src="images/pic1.jpg" alt="">
    <ul>
        <li><img src="images/pic1.jpg" alt=""></li>
        <li><img src="images/pic2.jpg" alt=""></li>
        <li><img src="images/pic3.jpg" alt=""></li>
        <li><img src="images/pic4.jpg" alt=""></li>
        <li><img src="images/pic5.jpg" alt=""></li>
    </ul>
</div>
<script>
    // 1.拿到需要操作的元素
    let oImg = document.querySelector("div>img");
    let oItems = document.querySelectorAll("li>img");
    // 2.给每一个小图添加移入和移出事件
    for(let item of oItems){
        // 监听移入事件
        item.onmouseenter = function () {
            // 1.给当前小图的父元素添加边框
            this.parentNode.className = "current";
            // 2.将当前小图的图片地址设置给大图
            oImg.src = this.src;
        }
        // 监听移出事件
        item.onmouseleave = function () {
            // 1.移除当前小图父元素的边框
            this.parentNode.className = "";
        }
    }
</script>
</body>
</html>
```

## 表单校验

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>16-JavaScript-表单校验</title>
</head>
<body>
<form action="http://www.it666.com">
    <input type="text" placeholder="请输入账号" name="userName">
    <input type="password" placeholder="请输入密码" name="userPwd">
    <input type="submit" value="注册">
</form>
<script>
    /*
    需求:
    1.账号和密码必须大于等于6位
    2.如果账号密码的长度不够就改变input输入框的背景颜色
    3.如果用户输入的账号或者密码不符合需求, 那么就不能提交
    */
    // 1.拿到需要操作的元素
    let oUserName = document.querySelector("input[type=text]");
    let oPassword = document.querySelector("input[type=password]");
    let oSubmit = document.querySelector("input[type=submit]");
    // 2.监听注册按钮的点击
    oSubmit.onclick = function () {
        // 1.拿到输入的用户名, 判断长度是否大于6位
        // 注意点: 如果想获取input中输入的内容, 必须通过value属性来获取
        // console.log(oUserName.innerHTML);
        // console.log(oUserName.innerText);
        // console.log(oUserName.textContent);
        if(oUserName.value.length < 6){
            oUserName.style.background = "red";
            return  false;
        }else{
            oUserName.style.background = "#fff";
        }
        if(oPassword.value.length < 6){
            oPassword.style.background = "red";
            return  false;
        }else{
            oPassword.style.background = "#fff";
        }
    }
</script>
</body>
</html>
```

## 焦点事件

**onfocus**

**onblur**

**onchange**

**oninput**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>17-JavaScript-焦点事件</title>
</head>
<body>
<input type="text">
<script>
    let oInput = document.querySelector("input");
    // 1.监听input获取焦点
    // oInput.onfocus = function () {
    //     console.log("获取到了焦点");
    // }
    // 2.监听input失去焦点
    // oInput.onblur = function () {
    //     console.log("失去了焦点");
    // }
    // 3.监听input内容改变
    // 注意点: onchange事件只有表单失去焦点的时候, 才能拿到修改之后的数据
    // oInput.onchange = function () {
    //     console.log(this.value);
    // }
    // oninput事件可以时时获取到用户修改之后的数据, 只要用户修改了数据就会调用(执行)
    // 注意点: oninput事件只有在IE9以及IE9以上的浏览器才能使用
    // 在IE9以下, 如果想时时的获取到用户修改之后的数据, 可以通过onpropertychange事件来实现
    oInput.oninput = function () {
        console.log(this.value);
    }
</script>
</body>
</html>
```

## 表单效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>18-JavaScript-表单效果</title>
</head>
<body>
<form action="http://www.it666.com">
    <input type="text" placeholder="请输入搜索关键字" name="text">
    <input type="submit" value="搜索"><!-- disabled="disabled"-->
</form>
<script>
    /*
    需求: 输入框中没有内容就禁用提交按钮
    */
    let oText = document.querySelector("input[type=text]");
    let oSubmit = document.querySelector("input[type=submit]");
    // 在JS中如果HTML标签的属性名称和取值名称一样的时候, 那么JS会返回true/false
    // console.log(oSubmit.disabled);
    oSubmit.disabled = true;
    
    oText.oninput = function () {
        // console.log(this.value.length);
        oSubmit.disabled = this.value.length === 0;
    }
    // 通过代码给input设置数据
    // 注意点: 如果是通过代码给input设置的数据, 那么不会触发oninput事件
    oText.value = "abc";
</script>
</body>
</html>
```

## Tab选项卡

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>19-JavaScript-Tab选项卡</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 400px;
            height: 300px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .top{
            list-style: none;
            width: 100%;
            height: 50px;
            line-height: 50px;
            text-align: center;
            display: flex;
            justify-content: space-between;
        }
        .top>li{
            width: 80px;
            height: 100%;
            background: skyblue;
            border-right: 1px solid #ccc;
        }
        .top>li:last-child{
            border-right: none;
        }
        .bottom{
            width: 100%;
            height: 250px;
        }
        .bottom>.content{
            width: 100%;
            height: 100%;
            display: none;
        }
        .bottom>.content:nth-child(1){
            background: red;
            display: block;
        }
        .bottom>.content:nth-child(2){
            background: blue;
        }
        .bottom>.content:nth-child(3){
            background: green;
        }
        .bottom>.content:nth-child(4){
            background: yellow;
        }
        .bottom>.content:nth-child(5){
            background: purple;
        }
        .current{
            background: red !important;
        }
    </style>
</head>
<body>
<div class="box">
    <ul class="top">
        <li class="current">新闻</li>
        <li>视频</li>
        <li>音乐</li>
        <li>军事</li>
        <li>财经</li>
    </ul>
    <div class="bottom">
        <div class="content">新闻的内容</div>
        <div class="content">视频的内容</div>
        <div class="content">音乐的内容</div>
        <div class="content">军事的内容</div>
        <div class="content">财经的内容</div>
    </div>
</div>
<script>
    // 1.拿到需要操作的元素
    let oItems = document.querySelectorAll("li");
    let oDivs = document.querySelectorAll(".content");
    // 定义变量保存上一次设置的索引
    let previousIndex = 0;
    // 2.给所有的选项卡添加点击事件
    for(let i = 0; i < oItems.length; i++){
        let item = oItems[i];
        // item.setAttribute("index", i);
        item.onclick = function() {
            // 清空上一次对应的元素对应的样式
            let preItem = oItems[previousIndex];
            preItem.className = "";
            let preDiv = oDivs[previousIndex];
            preDiv.style.display = "none";

            // 设置当前这一次的元素对应的样式
            // let index = this.getAttribute("index");
            this.className = "current";
            let oDiv = oDivs[i];
            oDiv.style.display = "block";

            // 保存当前这一次的索引
            previousIndex = i;
        };
    }
</script>
</body>
</html>
```

## 闭包

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>20-JavaScript-闭包</title>
</head>
<body>
<script>
    /*
    1.什么是闭包(closure)?
    闭包是一种特殊的函数。

    2.如何生成一个闭包?
    当一个内部函数引用了外部函数的数据(变量/函数)时, 那么内部的函数就是闭包
    所以只要满足"是函数嵌套"、"内部函数引用外部函数数据"

    3.闭包特点:
    只要闭包还在使用外部函数的数据, 那么外部的数据就一直不会被释放
    也就是说可以延长外部函数数据的生命周期

    4.闭包注意点:
    当后续不需要使用闭包时候, 一定要手动将闭包设置为null, 否则会出现内存泄漏
    */
    /*
    function test() {
        var i = 666; // 局部变量
    } // 只要代码执行到了大括号结束, i这个变量就会自动释放
    console.log(i); // i is not defined
    */
    function test() {
        var i = 666;
        // 由于demo函数满足闭包的两个条件, 所以demo函数就是闭包
        return function demo() {
            console.log(i);
        }
    }
    let fn = test();
    fn(); // 666
</script>
</body>
</html>
```

## 排他思想

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>25-JavaScript-排它思想</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            width: 400px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        .current{
            background: red;
        }
    </style>
</head>
<body>
<ul>
    <li class="current">我是第1个li</li>
    <li>我是第2个li</li>
    <li>我是第3个li</li>
    <li>我是第4个li</li>
    <li>我是第5个li</li>
</ul>
<script>
    /*
    1.什么是排它思想?
    清除其它非选中元素的样式, 只设置当前选中元素样式
    */
    let oItems = document.querySelectorAll("li");
    let previousIndex = 0;
    /*
    for(let i = 0; i < oItems.length; i++){
        let item = oItems[i];
        item.onclick = function () {
            // 排它
            // for(let j = 0; j < oItems.length; j++){
            //     let li = oItems[j];
            //     li.className = "";
            // }
            let preItem = oItems[previousIndex];
            preItem.className = "";

            this.className = "current";
            previousIndex = i;
        }
    }
    */
    for(var i = 0; i < oItems.length; i++) {
        var item = oItems[i];
        (function (index) {
            item.onclick = function () {
                // 排它
                var preItem = oItems[previousIndex];
                preItem.className = "";

                this.className = "current";
                previousIndex = index;
            }
        })(i);
    }
</script>
</body>
</html>
```

## Tab选项卡面向对象版本

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>26-JavaScript-Tab选项卡面向对象版</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        #tab{
            width: 400px;
            height: 300px;
            border: 1px solid #000;
            margin: 100px auto;
        }
        #tab_top{
            list-style: none;
            width: 100%;
            height: 50px;
            line-height: 50px;
            text-align: center;
            display: flex;
            justify-content: space-between;
        }
        #tab_top>li{
            width: 80px;
            height: 100%;
            background: skyblue;
            border-right: 1px solid #ccc;
        }
        #tab_top>li:last-child{
            border-right: none;
        }
        #tab_bottom{
            width: 100%;
            height: 250px;
        }
        #tab_bottom>.tab-content{
            width: 100%;
            height: 100%;
            display: none;
        }
        .selected{
            background: red !important;
        }
    </style>
</head>
<body>
<!--
在前端开发中如果id名称是由多个单词组成的, 那么建议使用下划线来连接
在前端开发中如果class名称是由多个单词组成的, 那么建议使用中划线来连接
-->
<div id="tab">
    <ul id="tab_top">
        <li class="tab-item selected">新闻</li>
        <li class="tab-item">视频</li>
        <li class="tab-item">音乐</li>
        <li class="tab-item">军事</li>
        <li class="tab-item">财经</li>
    </ul>
    <div id="tab_bottom">
        <div class="tab-content">新闻的内容</div>
        <div class="tab-content">视频的内容</div>
        <div class="tab-content">音乐的内容</div>
        <div class="tab-content">军事的内容</div>
        <div class="tab-content">财经的内容</div>
    </div>
</div>
<script>
    class Tab{
        constructor(){
            this.oTabItems = document.querySelectorAll(".tab-item");
            this.oTabContents = document.querySelectorAll(".tab-content");
            this.oTabContents[0].style.display = "block";
            this.previousIndex = 0;
        }
        addClickEvent(){
            for(let i = 0; i < this.oTabItems.length; i++){
                let tabItem = this.oTabItems[i];
                tabItem.onclick = () => {
                    this._change(i);
                }
            }
        }
        addMoveEvent(){
            for(let i = 0; i < this.oTabItems.length; i++){
                let tabItem = this.oTabItems[i];
                tabItem.onmousemove = () => {
                    this._change(i);
                }
            }
        }
        /*如果在方法名称前面加上_, 代表告诉其他的程序员这个是一个私有的方法, 不要调用
        * 注意点: 仅仅是告诉别人这个是一个私有的方法, 并不是真正的是一个私有的方法*/
        _change(i){
            // 1.排它
            let preTabItem = this.oTabItems[this.previousIndex];
            preTabItem.className = preTabItem.className.replace("selected", "");
            let preTabContent = this.oTabContents[this.previousIndex];
            preTabContent.style.display = "none";

            // 2.设置当前的样式
            let curTabItem = this.oTabItems[i];
            curTabItem.className = curTabItem.className + " selected";
            let curTabContent = this.oTabContents[i];
            curTabContent.style.display = "block";

            // 3.保存当前的索引
            this.previousIndex = i;
        }
    }
    let tab = new Tab();
    // tab.addClickEvent();
    tab.addMoveEvent();
</script>
</body>
</html>
```

## 全选反选事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>27-JavaScript-全选反选</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .music{
            width: 400px;
            box-shadow: 0 0 5px #000;
            margin: 100px auto;
            padding-left: 20px;
            padding-right: 20px;
            box-sizing: border-box;
        }
        .music>h3{
            line-height: 40px;
            text-align: center;
            border-bottom: 1px solid #ccc;
        }
        .music>ul>li{
            list-style: none;
            line-height: 30px;
            border-bottom: 1px solid #ccc;
        }
        .music>div{
            line-height: 40px;
            text-align: center;
        }
    </style>
</head>
<body>
<div class="music">
    <h3>歌曲排行榜</h3>
    <ul>
        <li><input type="checkbox">醒着做梦</li>
        <li><input type="checkbox">男人不应该让女人流泪</li>
        <li><input type="checkbox">女人不应该让男人太累</li>
        <li><input type="checkbox">狂浪</li>
        <li><input type="checkbox">生僻字</li>
        <li><input type="checkbox">沙漠骆驼</li>
        <li><input type="checkbox">最美的期待</li>
        <li><input type="checkbox">光年之外</li>
    </ul>
    <div>
        <button id="all_select">全选</button>
        <button id="cancel_select">取消全选</button>
        <button id="reveser_select">反选</button>
    </div>
</div>
<script>
    // 1.拿到需要操作的元素
    let oItems = document.querySelectorAll("input");
    let allSelectBtn = document.querySelector("#all_select");
    let cancelSelectBtn = document.querySelector("#cancel_select");
    let reveserSelectBtn = document.querySelector("#reveser_select");
    // 2.监听全选按钮的点击
    allSelectBtn.onclick = function () {
        oItems.forEach(function (item) {
            item.checked = true;
        });
    }
    cancelSelectBtn.onclick = function () {
        oItems.forEach(function (item) {
            item.checked = false;
        });
    }
    reveserSelectBtn.onclick = function () {
        oItems.forEach(function (item) {
            item.checked = !item.checked;
        });
    }
</script>
</body>
</html>
```

## Date对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>28-JavaScript-Date对象</title>
</head>
<body>
<script>
    // 1.获取当前时间
    // let date = new Date();
    // console.log(date);
    // 2.获取当前时间距离1970年1月1日（世界标准时间）起的毫秒
    // console.log(Date.now());
    // let date = new Date();
    // console.log(date.valueOf());

    // 3.创建指定时间
    // let date1 = new Date("2019-11-11 09:08:07");
    // console.log(date1);
    // 注意点: 在创建指定时间的时候, 如果月份是单独传入的, 那么会多一个月
    // let date2 = new Date(2019, 10, 11, 9, 8, 7);
    // console.log(date2);

    // 4.获取指定时间年月日时分秒
    // let date = new Date();
    // console.log(date);
    // console.log(date.getFullYear());
    // 注意点; 通过getMonth方法获取到的月份会少一个月
    // console.log(date.getMonth() + 1);
    // console.log(date.getDate());
    // console.log(date.getHours());
    // console.log(date.getMinutes());
    // console.log(date.getSeconds());

    // 5.时间格式化
    let date = new Date();
    let res = formartDate(date);
    console.log(res);
    // console.log(date);
    // 2019-4-19 18:17:06
    function formartDate(date) {
        return `${date.getFullYear()}-${date.getMonth() + 1}-${date.getDate()} ${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`;
    }
</script>
</body>
</html>
```

## 京东秒杀

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>30-JavaScript-京东秒杀</title>
    <link rel="stylesheet" href="fonts/iconfont.css">
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 190px;
            height: 270px;
            color: #fff;
            margin: 100px auto;
            background: #d00;
            text-align: center;
            padding-top: 40px;
            box-sizing: border-box;
        }
        .box>h3{
            font-size: 26px;
        }
        .box>p:nth-of-type(1){
            margin-top: 5px;
            color: rgba(255,255,255,0.5);
        }
        .box>i{
            display: inline-block;
            margin-top: 5px;
            margin-bottom: 5px;
            font-size: 40px;
        }
        .box>.time{
            margin-top: 10px;
            display: flex;
            justify-content: center;
        }
        .time>div{
            width: 40px;
            height: 40px;
            line-height: 40px;
            text-align: center;
            font-weight: bold;
            background: #333;
            position: relative;
        }
        .time>.minute{
            margin: 0 10px;
        }
        .time>div::before{
            content: "";
            display: block;
            width: 100%;
            height: 2px;
            background: #d00;
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
        }
    </style>
</head>
<body>
<div class="box">
    <h3>京东秒杀</h3>
    <p>FLASH DEALS</p>
    <i class="iconfont icon-lightningbshandian"></i>
    <p>本场距离结束还剩</p>
    <div class="time">
        <div class="hour">00</div>
        <div class="minute">00</div>
        <div class="second">00</div>
    </div>
</div>
<script>
    // 1.获取到需要操作的元素
    let oHour = document.querySelector(".hour");
    let oMinute = document.querySelector(".minute");
    let oSecond = document.querySelector(".second");

    // 2.获取时间的差值
    let remDate = new Date("2019-4-20 15:00:00");
    setTime(remDate);

    // 3.将差值设置给元素
    setInterval(function () {
        setTime(remDate);
    }, 1000);

    function setTime(remDate) {
        let obj = getDifferTime(remDate);
        // console.log(obj);
        oHour.innerText = obj.hour;
        oMinute.innerText = obj.minute;
        oSecond.innerText = obj.second;
    }
    function getDifferTime(remDate, curDate = new Date()) {
        // 1.得到两个时间之间的差值(毫秒)
        let differTime = remDate - curDate;
        // let differTime = remDate.valueOf() - curDate.valueOf();
        // 2.得到两个时间之间的差值(秒)
        let differSecond = differTime / 1000;
        // 3.利用相差的总秒数 / 每一天的秒数 = 相差的天数
        let day = Math.floor(differSecond / (60 * 60 * 24));
        day = day >= 10 ? day : "0" + day;
        // 4.利用相差的总秒数 / 小时 % 24;
        let hour = Math.floor(differSecond / (60 * 60) % 24);
        hour = hour >= 10 ? hour : "0" + hour;
        // 5.利用相差的总秒数 / 分钟 % 60;
        let minute = Math.floor(differSecond / 60 % 60);
        minute = minute >= 10 ? minute : "0" + minute;
        // 6.利用相差的总秒数 % 秒数
        let second = Math.floor(differSecond % 60);
        second = second >= 10 ? second : "0" + second;
        return {
            day: day,
            hour: hour,
            minute: minute,
            second: second,
        }
    }
</script>
</body>
</html>
```

## 时钟效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>31-JavaScript-时钟效果</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .clock{
            width: 300px;
            height: 600px;
            margin: 0 auto;
            background: url("images/clock.png");
            position: relative;
        }
        .clock>.box{
            width: 120px;
            height: 120px;
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            bottom: 220px;
        }
        .hour, .minute, .second{
            width: 6px;
            height: 120px;
            background: url("images/hour.png");
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
        }
        .minute{
            background: url("images/minute.png");
        }
        .second{
            background: url("images/second.png");
        }
    </style>
</head>
<body>
<div class="clock">
    <div class="box">
        <div class="hour"></div>
        <div class="minute"></div>
        <div class="second"></div>
    </div>
</div>
<script>
    // 1.获取到需要操作的元素
    let oHour = document.querySelector(".hour");
    let oMinute = document.querySelector(".minute");
    let oSecond = document.querySelector(".second");

    setTime();
    setInterval(function () {
        setTime();
    }, 1000);

    function setTime() {
        // 2.获取到当前的时间
        let date = new Date();
        // 3.根据当前的时间设置时针分针秒针旋转的角度
        oHour.style.transform = `rotate(${date.getHours() * 30}deg)`;
        oMinute.style.transform = `rotate(${date.getMinutes() * 6}deg)`;
        oSecond.style.transform = `rotate(${date.getSeconds() * 6}deg)`;
    }
</script>
</body>
</html>
```

## 匀速动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>33-JavaScript-匀速动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 100px;
            height: 100px;
            background: red;
        }
        .line1, .line2{
            width: 500px;
            height: 20px;
            background: blue;
        }
        .line2{
            width: 200px;
            background: purple;
        }
    </style>
</head>
<body>
<button id="start1">开始到500</button>
<button id="start2">开始到200</button>
<button id="end">结束</button>
<div class="box"></div>
<div class="line1"></div>
<div class="line2"></div>
<script>
    // 1.拿到需要操作的元素
    let startBtn1 = document.querySelector("#start1");
    let startBtn2 = document.querySelector("#start2");
    let endBtn = document.querySelector("#end");
    let oDiv = document.querySelector(".box");
    let timerId = null;
    
    // 2.监听按钮的点击事件
    /*
    startBtn1.onclick = function () {
        clearInterval(timerId);
        // 0.定义变量记录终点的位置
        let target = 500;
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(oDiv.style.marginLeft) || 0;
            // 2.定义变量记录步长
            let step = 13;
            // 3.计算新的位置
            begin += step;
            // console.log(begin, target);
            console.log(target - begin, step);
            // if(begin >= target){
            if(Math.abs(target - begin) <= Math.abs(step)){
                clearInterval(timerId);
                begin = target;
            }
            // 4.重新设置元素的位置
            oDiv.style.marginLeft = begin + "px";
        }, 100);
    }
    startBtn2.onclick = function () {
        clearInterval(timerId);
        // 0.定义变量记录终点的位置
        let target = 200;
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(oDiv.style.marginLeft) || 0;
            // 2.定义变量记录步长
            let step = -13;
            // 3.计算新的位置
            begin += step;
            // console.log(begin, target);
            console.log(Math.abs(target - begin), Math.abs(step));
            // if(begin >= target){
            if(Math.abs(target - begin) <= Math.abs(step)){
                clearInterval(timerId);
                begin = target;
            }

            // 4.重新设置元素的位置
            oDiv.style.marginLeft = begin + "px";
        }, 100);
    }
    */
    endBtn.onclick = function () {
        clearInterval(timerId);
    }
    startBtn1.onclick = function () {
        linearAnimation(oDiv, 500);
    }
    startBtn2.onclick = function () {
        linearAnimation(oDiv, 200);
    }
    function linearAnimation(ele, target) {
        clearInterval(timerId);
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(ele.style.marginLeft) || 0;
            // 2.定义变量记录步长
            //         0  -  500 = -500    13
            //         500 -  200 = 300    -13
            let step = (begin - target) > 0 ? -13 : 13;
            // 3.计算新的位置
            begin += step;
            console.log(Math.abs(target - begin), Math.abs(step));
            if(Math.abs(target - begin) <= Math.abs(step)){
                clearInterval(timerId);
                begin = target;
            }

            // 4.重新设置元素的位置
            ele.style.marginLeft = begin + "px";
        }, 100);
    }
</script>
</body>
</html>
```

## 缓动动画

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>34-JavaScript-缓动动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 100px;
            height: 100px;
            background: red;
        }
        .line1, .line2{
            width: 500px;
            height: 20px;
            background: blue;
        }
        .line2{
            width: 200px;
            background: purple;
        }
    </style>
</head>
<body>
<button id="start1">开始到500</button>
<button id="start2">开始到200</button>
<button id="end">结束</button>
<div class="box"></div>
<div class="line1"></div>
<div class="line2"></div>
<script>
    // 1.拿到需要操作的元素
    let startBtn1 = document.querySelector("#start1");
    let startBtn2 = document.querySelector("#start2");
    let endBtn = document.querySelector("#end");
    let oDiv = document.querySelector(".box");
    let timerId = null;
    // 2.监听按钮的点击事件
    endBtn.onclick = function () {
        clearInterval(timerId);
    }
    startBtn1.onclick = function () {
        easeAnimation(oDiv, 500);
    }
    startBtn2.onclick = function () {
        easeAnimation(oDiv, 200);
    }
    function easeAnimation(ele, target) {
        clearInterval(timerId);
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(ele.style.marginLeft) || 0;
            // 2.定义变量记录步长
            // 公式: (结束位置 - 开始位置) * 缓动系数(0 ~1)
            let step = (target - begin) * 0.3;
            console.log(step);
            // 3.计算新的位置
            begin += step;
            if(Math.abs(Math.floor(step)) <= 1){
                clearInterval(timerId);
                begin = target;
            }
            // 4.重新设置元素的位置
            ele.style.marginLeft = begin + "px";
        }, 100);
    }
</script>
</body>
</html>
```

## 轮播图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>35-JavaScript-轮播图</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 670px;
            height: 300px;
            border: 1px solid #000;
            margin: 100px auto;
            position: relative;
            overflow: hidden;
        }
        ul{
            list-style: none;
            display: flex;
        }
        p{
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            width: 100%;
            display: flex;
            justify-content: space-between;
        }
        p>span{
            width: 30px;
            height: 60px;
            line-height: 60px;
            text-align: center;
            background: rgba(0,0,0,0.5);
            color: #fff;
            font-size: 40px;
        }
        ul>li>img {
            display: inline-block;
        }
    </style>
</head>
<body>
<div class="box">
    <ul>
        <li><img src="images/ad1.jpg" alt=""></li>
        <li><img src="images/ad2.jpg" alt=""></li>
        <li><img src="images/ad3.jpg" alt=""></li>
    </ul>
    <p>
        <span class="left">&lt;</span>
        <span class="right">&gt;</span>
    </p>
</div>
<script>
    // 1.拿到需要操作的元素
    let oLeft = document.querySelector(".left");
    let oRight = document.querySelector(".right");
    let oUl = document.querySelector("ul");
    let oItems = document.querySelectorAll("ul>li");
    let imgWidth = parseFloat(getComputedStyle(oItems[0]).width);
    let style = getComputedStyle(oItems[0])
    let currentIndex = 0;

    // 2.监听按钮的点击
    oRight.onclick = function () {
        currentIndex++;
        if(currentIndex > oItems.length - 1){
            currentIndex = 0;
        }
        // oUl.style.marginLeft = -currentIndex * imgWidth + "px";
        // linearAnimation(oUl, -currentIndex * imgWidth);
        console.log(imgWidth)
        easeAnimation(oUl, -currentIndex * imgWidth);
    }
    oLeft.onclick = function () {
        currentIndex--;
        if(currentIndex < 0){
            currentIndex = oItems.length - 1;
        }
        // oUl.style.marginLeft = -currentIndex * imgWidth + "px";
        // linearAnimation(oUl, -currentIndex * imgWidth);
        easeAnimation(oUl, -currentIndex * imgWidth);
    }

    let timerId = null;
    function linearAnimation(ele, target) {
        clearInterval(timerId);
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(ele.style.marginLeft) || 0;
            // 2.定义变量记录步长
            //         0  -  500 = -500    13
            //         500 -  200 = 300    -13
            let step = (begin - target) > 0 ? -13 : 13;
            // 3.计算新的位置
            begin += step;
            console.log(Math.abs(target - begin), Math.abs(step));
            if(Math.abs(target - begin) <= Math.abs(step)){
                clearInterval(timerId);
                begin = target;
            }

            // 4.重新设置元素的位置
            ele.style.marginLeft = begin + "px";
        }, 100);
    }
    function easeAnimation(ele, target) {
        clearInterval(timerId);
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(ele.style.marginLeft) || 0;
            // 2.定义变量记录步长
            // 公式: (结束位置 - 开始位置) * 缓动系数(0 ~1)
            let step = (target - begin) * 0.3;
            console.log(target);
            // 3.计算新的位置
            begin += step;
            if(Math.abs(Math.floor(step)) <= 1){
                clearInterval(timerId);
                begin = target;
            }
            // 4.重新设置元素的位置
            ele.style.marginLeft = begin + "px";
        }, 100);
    }
</script>
</body>
</html>
```

## 自动轮播

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>37-JavaScript-自动轮播</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 670px;
            height: 300px;
            border: 1px solid #000;
            margin: 100px auto;
            position: relative;
            overflow: hidden;
        }
        ul{
            list-style: none;
            display: flex;
        }
        p{
            position: absolute;
            left: 0;
            top: 50%;
            transform: translateY(-50%);
            width: 100%;
            display: flex;
            justify-content: space-between;
        }
        p>span{
            width: 30px;
            height: 60px;
            line-height: 60px;
            text-align: center;
            background: rgba(0,0,0,0.5);
            color: #fff;
            font-size: 40px;
        }
    </style>
    <script src="js/animation.js"></script>
</head>
<body>
<div class="box">
    <ul>
        <li><img src="images/ad1.jpg" alt=""></li>
        <li><img src="images/ad2.jpg" alt=""></li>
        <li><img src="images/ad3.jpg" alt=""></li>
        <li><img src="images/ad1.jpg" alt=""></li>
    </ul>
    <p>
        <span class="left">&lt;</span>
        <span class="right">&gt;</span>
    </p>
</div>
<script>
    // 1.拿到需要操作的元素
    let oBox = document.querySelector(".box");
    let oLeft = document.querySelector(".left");
    let oRight = document.querySelector(".right");
    let oUl = document.querySelector("ul");
    let oItems = document.querySelectorAll("ul>li");
    let imgWidth = parseFloat(getComputedStyle(oItems[0]).width);
    let currentIndex = 0;

    // 2.监听按钮的点击
    oRight.onclick = function () {
        currentIndex++;
        if(currentIndex > oItems.length - 1){
            currentIndex = 0;
            // 快速的跳转到第一张
            oUl.style.marginLeft = -currentIndex * imgWidth + "px";
            currentIndex++;
        }
        easeAnimation(oUl, -currentIndex * imgWidth);
    }
    oLeft.onclick = function () {
        currentIndex--;
        if(currentIndex < 0){
            currentIndex = oItems.length - 1;
            // 快速的跳转到最后一张
            oUl.style.marginLeft = -currentIndex * imgWidth + "px";
            currentIndex--;
        }
        easeAnimation(oUl, -currentIndex * imgWidth);
    }

    // 3.自动轮播
    let id = setInterval(function () {
        oRight.onclick();
    }, 2000);
    oBox.onmouseenter = function(){
        // 关闭定时器
        clearInterval(id);
    };
    oBox.onmouseleave = function(){
        // 重新开启定时器
        id = setInterval(function () {
            oRight.onclick();
        }, 2000);
    };

</script>
</body>
</html>
```

## 匀速动画增强

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>38-JavaScript-匀速动画增强</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 100px;
            height: 100px;
            background: red;
        }
        .line1, .line2{
            width: 500px;
            height: 20px;
            background: blue;
        }
        .line2{
            width: 200px;
            background: purple;
        }
    </style>
</head>
<body>
<button id="start1">开始到500</button>
<button id="start2">开始到200</button>
<button id="end">结束</button>
<div class="box"></div>
<div class="line1"></div>
<div class="line2"></div>
<script>
    // 1.拿到需要操作的元素
    let startBtn1 = document.querySelector("#start1");
    let startBtn2 = document.querySelector("#start2");
    let endBtn = document.querySelector("#end");
    let oDiv = document.querySelector(".box");
    let timerId = null;

    // 2.监听按钮的点击事件
    endBtn.onclick = function () {
        clearInterval(timerId);
    }
    startBtn1.onclick = function () {
        // linearAnimation(oDiv, "marginTop", 500);
        // linearAnimation(oDiv, {"marginTop":500});
        // linearAnimation(oDiv, {"marginTop":500, "marginLeft": 300}, function () {
        //     alert("动画执行完毕之后执行的其它的操作");
        // });
        linearAnimation(oDiv, {"marginTop":500, "marginLeft": 300});
    }
    startBtn2.onclick = function () {
        linearAnimation(oDiv, 200);
    }
    // function linearAnimation(ele, attr ,target) {
    function linearAnimation(ele, obj, fn) {
        clearInterval(timerId);
        timerId = setInterval(function () {
            // flag变量用于标记是否所有的属性都执行完了动画
            let flag = true;
            for(let key in obj){
                let attr = key;
                let target = obj[key];

                // 1.拿到元素当前的位置
                // let begin = parseInt(ele.style.width) || 0;
                let style = getComputedStyle(ele);
                // let begin = parseInt(style.width) || 0;
                let begin = parseInt(style[attr]) || 0;
                // 2.定义变量记录步长
                let step = (begin - target) > 0 ? -13 : 13;
                // 3.计算新的位置
                begin += step;
                if(Math.abs(target - begin) > Math.abs(step)){
                    flag = false;
                }else{
                    begin = target;
                }
                // 4.重新设置元素的位置
                // ele.style.width = begin + "px";
                ele.style[attr] = begin + "px";
            }
            if(flag){
                clearInterval(timerId);
                // if(fn){
                //     fn();
                // }
                fn && fn();
            }
        }, 100);
    }
</script>
</body>
</html>
```

## 联动事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>39-JavaScript-联动动画</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .top{
            width: 320px;
            height: 302px;
            overflow: hidden;
        }
        .bottom{
            width: 320px;
            height: 102px;
            overflow: hidden;
        }
        .box{
            position: fixed;
            right: 0;
            bottom: 0;
        }
        span{
            display: inline-block;
            width: 50px;
            height: 50px;
            position: absolute;
            top: 0;
            right: 0;
        }
    </style>
</head>
<body>
<div class="box">
    <div class="top">
        <img src="images/top.jpg" alt="">
    </div>
    <div class="bottom">
        <img src="images/bottom.jpg" alt="">
    </div>
    <span></span>
</div>
<script src="js/animation2.js"></script>
<script>
    // 1.拿到需要操作的元素
    let oCloseBtn = document.querySelector("span");
    let oTopDiv = document.querySelector(".top");
    let oBottomDiv = document.querySelector(".bottom");
    let oBoxDiv = document.querySelector(".box");

    // 2.监听关闭按钮的点击事件
    oCloseBtn.onclick =  function () {
        easeAnimation(oTopDiv, {"height": 0}, function () {
            easeAnimation(oBoxDiv, {"width": 0}, function () {
                oBoxDiv.parentNode.removeChild(oBoxDiv);
            });
        });
    }
</script>
</body>
</html>
```

## 添加事件的三种方式

**on+事件名**

**addEventListener**

**attachEvent**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>39-JavaScript-添加事件三种方式</title>
</head>
<body>
<button id="btn">我是按钮</button>
<script>
    var oBtn = document.getElementById("btn");
    /*
    方式一:
    1.通过onxxx的方式来添加
    注意点: 由于是给属性赋值, 所以后赋值的会覆盖先赋值
    */
    /*
    oBtn.onclick = function () {
        alert("666");
    }
    oBtn.onclick = function () {
        alert("777");
    }
    let obj = {};
    obj.say = function () {
        console.log("123");
    }
    obj.say = function () {
        console.log("456");
    }
    obj.say();
    */

    /*
    方式二:
    2.通过addEventListener方法添加
    注意点:
    1.事件名称不需要添加on
    2.后添加的不会覆盖先添加的
    3.只支持最新的浏览器IE9
    */
    /*
    oBtn.addEventListener("click", function () {
        alert("666");
    });
    oBtn.addEventListener("click", function () {
        alert("777");
    });
    */

    /*
    方式三
    3.通过attachEvent方法添加
    注意点:
    1.事件名称必须加上on
    2.后添加的不会覆盖先添加的
    3.只支持低版本的浏览器
    */
    /*
    oBtn.attachEvent("onclick", function () {
        alert("666");
    });
    oBtn.attachEvent("onclick", function () {
        alert("777");
    });
    */

    addEvent(oBtn, "click", function () {
        alert("666");
    })
    addEvent(oBtn, "click", function () {
        alert("777");
    })
    function addEvent(ele, name, fn) {
        if(ele.attachEvent){
            ele.attachEvent("on"+name, fn);
        }else{
            ele.addEventListener(name, fn);
        }
    }
</script>
</body>
</html>
```

## 事件对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>40-JavaScript-事件对象</title>
</head>
<body>
<button id="btn">我是按钮</button>
<a href="http://www.it666.com">知播渔教育</a>
<script>
    /*
    1.什么是事件对象?
    事件对象就是一个系统自动创建的一个对象
    当注册的事件被触发的时候, 系统就会自动创建事件对象
    */
    /*
    2.事件对象的注意点:
    在高级版本的浏览器中, 会自动将事件对象传递给回到函数
    在低级版本的浏览器中, 不会自动将事件对象传递给回调函数
    在低级版本的浏览器中, 需要通过window.event来获取事件对象
     */
    /*
   var oBtn = document.getElementById("btn");
   oBtn.onclick = function (event) {
       // 兼容性的写法
       event = event || window.event;
       // alert("666");
       console.log(event);
       console.log(typeof event);
   }
   */
   let oA = document.querySelector("a");
    oA.onclick = function (event) {
        // 兼容性的写法
        event = event || window.event;

        alert("666");
        // 阻止默认行为
        return false; // 企业开发推荐

        // 注意点: preventDefault方法只支持高级版本的浏览器
        // event.preventDefault();
        // event.returnValue = false; // IE9以下的浏览器
    }
</script>
</body>
</html>
```

## 事件执行的三个阶段

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>41-JavaScript-事件执行的三个阶段</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 300px;
            height: 300px;
            background: red;
        }
        .son{
            width: 150px;
            height: 150px;
            background: blue;
        }
    </style>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    /*
    1.事件的三个阶段
    1.1.捕获阶段(从外向内的传递事件)
    1.2.当前目标阶段
    1.3.冒泡的阶段(从内向外的传递事件)

    2.注意点:
    三个阶段只有两个会被同时执行
    要么捕获和当前, 要么当前和冒泡

    3.为什么要么只能是捕获和当前, 要么只能是当前和冒泡?
    这是JS处理事件的历史问题
    早期各大浏览器厂商为占领市场, 以及对事件的理解不同
    后续W3C为了兼容, 将两种方式都纳入标准
    */
    /*
    1.如何设置事件到底是捕获还是冒泡?
    通过addEventListener方法, 这个方法接收三个参数
    第一个参数: 事件的名称
    第二个参数: 回调函数
    第三个参数: false冒泡  / true 捕获

    注意点:
    onXxx的属性, 不接收任何参数, 所以默认就是冒泡
    attachEvent方法, 只能接收两个参数, 所以默认就是冒泡
    */
    let oFDiv = document.querySelector(".father");
    let oSDiv = document.querySelector(".son");
    /*
    oFDiv.addEventListener("click", function () {
        console.log("father");
    }, false);
    oSDiv.addEventListener("click", function () {
        console.log("son");
    }, false);
    */
    oFDiv.onclick = function () {
        console.log("father");
    }
    oSDiv.onclick = function () {
        console.log("son");
    }
    document.body.onclick = function() {
        console.log("body")
    }
    /*
    IE 6.0:
    div -> body -> html -> document
    其他浏览器:
    div -> body -> html -> document -> window
    注意：
    不是所有的事件都能冒泡，以下事件不冒泡：blur、focus、load、unload
    */
</script>
</body>
</html>
```

## 事件冒泡的应用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>42-JavaScript-事件冒泡应用</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        ul{
            list-style: none;
            width: 300px;
            margin: 100px auto;
            border: 1px solid #000;
        }
        .selected{
            background: red;
        }
    </style>
</head>
<body>
<ul>
    <li class="selected">我是第1个li</li>
    <li>我是第2个li</li>
    <li>我是第3个li</li>
    <li>我是第4个li</li>
    <li>我是第5个li</li>
</ul>
<script>
    /*
    在企业开发中使用最多的就是事件冒泡
    默认情况下通过onXxx和attachEvent添加的事件也都是冒泡的事件
    */
    /*
    let oItems = document.querySelectorAll("ul>li");
    let currentItem = oItems[0];
    for(let item of oItems){
        item.onclick = change;
    }
    function change() {
        currentItem.className = "";
        this.className = "selected";
        currentItem = this;
    }

     */

    let oUl = document.querySelector("ul");
    let oLi = document.querySelector(".selected");
    oUl.onclick = function (event) {
        event = event || window.event;
        // console.log(event.target);
        oLi.className = "";
        let item = event.target;
        item.className = "selected";
        oLi = item;
    }
</script>
</body>
</html>
```

## 阻止事件冒泡

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>43-JavaScript-阻止事件冒泡</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 300px;
            height: 300px;
            background: red;
        }
        .son{
            width: 150px;
            height: 150px;
            background: blue;
        }
    </style>
</head>
<body>
<div class="father" id="father">
    <div class="son" id="son"></div>
</div>
<script>
    // 1.拿到需要操作的元素
    var oFDiv = document.getElementById("father");
    var oSDiv = document.getElementById("son");
    
    // 2.注册事件监听
    oFDiv.onclick = function () {
        console.log("father");
    }
    oSDiv.onclick = function (event) {
        event = event || window.event;
        // 注意点: stopPropagation方法只支持高级浏览器
        // event.stopPropagation();
        // event.cancelBubble = true; // 低级浏览器
        if(event.cancelBubble){
            event.cancelBubble = true;
        }else{
            event.stopPropagation();
        }
        console.log("son");
    }
</script>
</body>
</html>
```

## 移入移出事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>44-JavaScript-移入移出事件区别</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 300px;
            height: 300px;
            background: red;
        }
        .son{
            width: 150px;
            height: 150px;
            background: blue;
        }
    </style>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    /*
    1.onmouseover和onmouseenter的区别
    onmouseover移入到子元素,父元素的移入事件也会被触发
    onmouseenter移入到子元素,父元素的移入事件不会被触发
    */
    /*
    2.onmouseout和onmouseleave的区别
    onmouseout移出到子元素,父元素的移入事件也会被触发
    onmouseleave移出到子元素,父元素的移入事件不会被触发
    */
    let oFDiv = document.querySelector(".father");
    let oSDiv = document.querySelector(".son");
    /*
    oFDiv.onmouseover = function () {
        console.log("father");
    }
    oSDiv.onmouseover = function () {
        console.log("son");
    }
     */
    oFDiv.onmouseenter = function () {
        console.log("father");
    }
    oSDiv.onmouseenter = function () {
        console.log("son");
    }

    oFDiv.onmouseleave = function () {
        console.log("father");
    }
    oSDiv.onmouseleave = function () {
        console.log("son");
    }
</script>
</body>
</html>
```

## 位置获取

**offsetX/Y: 事件触发相对于当前元素自身的位置**

**clientX/Y: 事件触发相对于浏览器可视区域的位置**

**pageX/Y: 事件触发相对于整个网页的位置**

**screenX/Y: 事件触发相对于屏幕的位置，包括浏览器的上面部分**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>45-JavaScript-位置获取</title>
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
            margin-top: 800px;
        }
    </style>
</head>
<body>
<div id="box"></div>
<script>
    /*
    offsetX/offsetY: 事件触发相对于当前元素自身的位置
    clientX/clientY: 事件触发相对于浏览器可视区域的位置
    注意点: 可视区域是不包括滚动出去的范围的
    pageX/pageY:     事件触发相对于整个网页的位置
    主要点: 整个网页包括滚动出去的范围的
    screenX/screenY: 事件触发相对于屏幕的位置
    */
    var oDiv = document.getElementById("box");
    oDiv.onclick = function (event) {
        event = event || window.event;
        // console.log("offsetX", event.offsetX);
        // console.log("offsetY", event.offsetY);
        /*
        console.log("clientX", event.clientX);
        console.log("clientY", event.clientY);
        console.log("----------------------");
        console.log("pageX", event.pageX);
        console.log("pageY", event.pageY);
        */
        
        console.log(event.screenX);
        console.log(event.screenY);

        console.log("clientX", event.clientX);
        console.log("clientY", event.clientY);
        
    }
</script>
</body>
</html>
```

## 佩奇跟我走

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>46-JavaScript-佩奇跟我走</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        html, body{
            width: 100%;
            height: 100%;
            overflow: hidden;
        }
    </style>
</head>
<body>
<img src="images/pig.gif" alt="">
<script>
    let oImg = document.querySelector("img");
    let style = getComputedStyle(oImg);
    document.body.onmousemove = function (event) {
        event = event || window.event;
        // console.log(event.clientX);
        // console.log(event.clientY);
        oImg.style.marginLeft = event.clientX - parseFloat(style.width) / 2 + "px";
        oImg.style.marginTop = event.clientY - parseFloat(style.height) / 2 + "px";

        // oImg.style.marginLeft = event.pageX - parseFloat(style.width) / 2 + "px";
        // oImg.style.marginTop = event.pageY - parseFloat(style.height) / 2 + "px";
    }
     
     /*
    document.body.onmousemove = function (event) {
        event = event || window.event;
        // 注意点: clientX/clientY无论高级浏览器还是低级浏览器都支持
        //         pageX/pageY只有高级浏览器支持, 低级浏览器是不支持的(IE9以下)
        // console.log(event.clientX);
        // console.log(event.clientY);
        console.log(event.pageX);
        console.log(event.pageY);
    }
    */
</script>
</body>
</html>
```

## 正则表达式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>47-JavaScript-正则表达式</title>
</head>
<body>
<script>
    /*
    1.什么是正则表达式?
    正则表达式就是对字符串操作的一种逻辑公式

    2.正则表达式的作用?
    1.在字符串"查找"是否包含指定子串
    2.从字符串中"提取"指定子串
    3.对字符串中指定的内容进行"替换"
     */
    // 1.字符串查找
    /*
    let str = "123abc456";
    let index = str.indexOf("abc");
    let index = str.lastIndexOf("abc");
    let flag = str.includes("abc");
     */
    // 2.字符串提取
    /*
    let str = "123abc456";
    let startIndex = str.indexOf("a");
    console.log(str.substr(startIndex, "abc".length));
     */
    // 3.字符串替换
    /*
    let str = "123abc456";
    str.replace("abc", "it666");
     */

    // 1.利用正则表达式匹配(查找)
    /*
    let str = "123abc456";
    // 1.创建一个正则表达式对象
    // 2.指定匹配的规则
    // 注意点: 默认情况下在正则表达式中是区分大小写的
    let reg = new RegExp("A", "i");
    let res = reg.test(str);
    console.log(res);
     */
    /*
    let str = "abc2020-1-11def";
    // 通过构造函数创建正则表达式对象
    // let reg = new RegExp("\\d{4}-\\d{1,2}-\\d{1,2}");
    // 通过字面量来创建正则表达式对象
    let reg = /\d{4}-\d{1,2}-\d{1,2}/;
    let res = reg.test(str);
    console.log(res);
     */

    // 2.通过正则表达式提取符合规则的字符串
    /*
    let str = "abc2020-1-11def2019-11-11fdjsklf";
    // 注意点: 默认情况下在正则表达式中一旦匹配就会停止查找
    let reg = /\d{4}-\d{1,2}-\d{1,2}/g;
    let res = str.match(reg);
    console.log(res);
    console.log(res[0]);
    console.log(res[1]);
     */

    // 3.通过正则表达式替换符合规则的字符串
    let str = "abc2020-1-11def2019-11-11fdjsklf";
    let reg = /\d{4}-\d{1,2}-\d{1,2}/g;
    let newStr = str.replace(reg, "it666");
    console.log(str);
    console.log(newStr);
</script>
</body>
</html>
<!--
常用正则表达式合集:
验证数字：^[0-9]*$
验证n位的数字：^\d{n}$
验证至少n位数字：^\d{n,}$
验证m-n位的数字：^\d{m,n}$
验证零和非零开头的数字：^(0|[1-9][0-9]*)$
验证有两位小数的正实数：^[0-9]+(.[0-9]{2})?$
验证有1-3位小数的正实数：^[0-9]+(.[0-9]{1,3})?$
验证非零的正整数：^\+?[1-9][0-9]*$
验证非零的负整数：^\-[1-9][0-9]*$
验证非负整数（正整数 + 0）  ^\d+$
验证非正整数（负整数 + 0）  ^((-\d+)|(0+))$
验证长度为3的字符：^.{3}$
验证由26个英文字母组成的字符串：^[A-Za-z]+$
验证由26个大写英文字母组成的字符串：^[A-Z]+$
验证由26个小写英文字母组成的字符串：^[a-z]+$
验证由数字和26个英文字母组成的字符串：^[A-Za-z0-9]+$
验证由数字、26个英文字母或者下划线组成的字符串：^\w+$

验证用户密码:^[a-zA-Z]\w{5,17}$ 正确格式为：以字母开头，长度在6-18之间，只能包含字符、数字和下划线。
验证是否含有 ^%&',;=?$\" 等字符：[^%&',;=?$\x22]+
验证汉字：^[\u4e00-\u9fa5],{0,}$
验证Email地址：^\w+[-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$
验证InternetURL：^http://([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$ ；^[a-zA-z]+://(w+(-w+)*)(.(w+(-w+)*))*(?S*)?$
验证电话号码：^(\d3,4|\d{3,4}-)?\d{7,8}$：--正确格式为：XXXX-XXXXXXX，XXXX-XXXXXXXX，XXX-XXXXXXX，XXX-XXXXXXXX，XXXXXXX，XXXXXXXX。
验证身份证号（15位或18位数字）：^\d{15}|\d{}18$
验证一年的12个月：^(0?[1-9]|1[0-2])$ 正确格式为：“01”-“09”和“1”“12”
验证一个月的31天：^((0?[1-9])|((1|2)[0-9])|30|31)$    正确格式为：01、09和1、31。
整数：^-?\d+$
非负浮点数（正浮点数 + 0）：^\d+(\.\d+)?$
正浮点数   ^(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*))$
非正浮点数（负浮点数 + 0） ^((-\d+(\.\d+)?)|(0+(\.0+)?))$
负浮点数  ^(-(([0-9]+\.[0-9]*[1-9][0-9]*)|([0-9]*[1-9][0-9]*\.[0-9]+)|([0-9]*[1-9][0-9]*)))$
浮点数  ^(-?\d+)(\.\d+)?$
-->
```

## 高级日期格式化

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>48-JavaScript-高级日期格式化</title>
</head>
<body>
<script>
    let crtTime = new Date();
    /*
    y  年
    M  月
    d  天
    h 小时
    m 分钟
    s 秒钟
    */
    let res1 = dateFormart("yyyy-MM-dd hh:mm:ss", crtTime);
    let res2 = dateFormart("yyyy-MM-dd", crtTime);
    let res3 = dateFormart("hh:mm:ss", crtTime);
    console.log(res1);
    console.log(res2);
    console.log(res3);

    function dateFormart(fmt, date) {
        // 1.处理年
        // 1.1找到yyyy
        // +在正则表达式中表示匹配1个或多个连续的指定字符
        // let reg = /y+/;
        let yearStr = fmt.match(/y+/);
        if(yearStr){
            yearStr = yearStr[0];
            // 1.2获取到当前的年
            let yearNum = date.getFullYear() + ""; // 2019
            yearNum = yearNum.substr(4 - yearStr.length)
            // 1.3利用当前的年替换掉yyyy
            fmt = fmt.replace(yearStr, yearNum);
        }
        // 2.处理其他的时间
        let obj = {
            "M+" : date.getMonth() + 1,
            "d+" : date.getDate(),
            "h+" : date.getHours(),
            "m+" : date.getMinutes(),
            "s+" : date.getSeconds()
        };
        // 2.1遍历取出所有的时间
        for(let key in obj){
            // let reg = new RegExp("M+");
            let reg = new RegExp(`${key}`);
            // 取出格式化字符串中对应的格式字符 MM dd hh mm ss
            let fmtStr = fmt.match(reg);
            if(fmtStr){
                fmtStr = fmtStr[0];
                // 单独处理一位或者两位的时间
                if(fmtStr.length === 1){
                    // 一位
                    fmt = fmt.replace(fmtStr, obj[key]);
                }else{
                    // 两位
                    let numStr = "00" + obj[key];
                    //"00" + 4 = "004"
                    //"00" + 23 = "0023"
                    numStr = numStr.substr((obj[key] + "").length);
                    fmt = fmt.replace(fmtStr, numStr);
                }
            }
        }
        // 3.将格式化之后的字符串返回
        return fmt;
    }

    /**************************************时间格式化处理************************************/
    function dateFmt(fmt,date){
        let obj = {
            "M+" : date.getMonth()+1,                 //月份
            "d+" : date.getDate(),                    //日
            "h+" : date.getHours(),                   //小时
            "m+" : date.getMinutes(),                 //分
            "s+" : date.getSeconds(),                 //秒
        };
        if(/(y+)/.test(fmt))
            fmt=fmt.replace(RegExp.$1, (date.getFullYear()+"").substr(4 - RegExp.$1.length));
        for(let key in obj)
            if(new RegExp(`${key}`).test(fmt))
                fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (obj[key]) : (("00"+ obj[key]).substr((""+ obj[key]).length)));
        return fmt;
    }
</script>
</body>
</html>
```

## animation.js

```js
(function() {
    let timerId = null;
    function linearAnimation(ele, target) {
        clearInterval(timerId);
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(ele.style.marginLeft) || 0;
            // 2.定义变量记录步长
            //         0  -  500 = -500    13
            //         500 -  200 = 300    -13
            let step = (begin - target) > 0 ? -13 : 13;
            // 3.计算新的位置
            begin += step;
            console.log(Math.abs(target - begin), Math.abs(step));
            if(Math.abs(target - begin) <= Math.abs(step)){
                clearInterval(timerId);
                begin = target;
            }

            // 4.重新设置元素的位置
            ele.style.marginLeft = begin + "px";
        }, 100);
    }
    function easeAnimation(ele, target) {
        clearInterval(timerId);
        timerId = setInterval(function () {
            // 1.拿到元素当前的位置
            let begin = parseInt(ele.style.marginLeft) || 0;
            // 2.定义变量记录步长
            // 公式: (结束位置 - 开始位置) * 缓动系数(0 ~1)
            let step = (target - begin) * 0.3;
            console.log(step);
            // 3.计算新的位置
            begin += step;
            if(Math.abs(Math.floor(step)) <= 1){
                clearInterval(timerId);
                begin = target;
            }
            // 4.重新设置元素的位置
            ele.style.marginLeft = begin + "px";
        }, 100);
    }

    // 将函数绑定到window对象上, 这样全局就可以使用了
    window.linearAnimation = linearAnimation;
    window.easeAnimation = easeAnimation;
})();
```

## animation2.js

```js
(function() {
    // let timerId = null;
    function linearAnimation(ele, obj, fn) {
        clearInterval(ele.timerId);
        ele.timerId = setInterval(function () {
            let flag = true;
            for(let key in obj){
                let target = obj[key];
                // 1.拿到元素当前的位置
                let style = getComputedStyle(ele);
                let begin = parseFloat(style[key]) || 0;
                // 2.定义变量记录步长
                let step = (begin - target) > 0 ? -13 : 13;
                // 3.计算新的位置
                begin += step;
                console.log(Math.abs(target - begin), Math.abs(step));
                if(Math.abs(target - begin) > Math.abs(step)){
                    flag = false;
                }else{
                    begin = target;
                }
                // 4.重新设置元素的位置
                ele.style[key] = begin + "px";
            }
            if(flag){
                clearInterval(ele.timerId);
                fn && fn();
            }
        }, 100);
    }
    function easeAnimation(ele, obj, fn) {
        clearInterval(ele.timerId);
        ele.timerId = setInterval(function () {
            let flag = true;
            for(let key in obj){
                let target = obj[key];
                // 1.拿到元素当前的位置
                let style = getComputedStyle(ele);
                let begin = parseInt(style[key]) || 0;
                // 2.定义变量记录步长
                // 公式: (结束位置 - 开始位置) * 缓动系数(0 ~1)
                let step = (target - begin) * 0.3;
                // 3.计算新的位置
                begin += step;
                if(Math.abs(Math.floor(step)) > 1){
                   flag = false;
                }else{
                    begin = target;
                }
                // 4.重新设置元素的位置
                ele.style[key] = begin + "px";
            }
            if(flag){
                clearInterval(ele.timerId);
                fn && fn();
            }
        }, 100);
    }

    // 将函数绑定到window对象上, 这样全局就可以使用了
    window.linearAnimation = linearAnimation;
    window.easeAnimation = easeAnimation;
})();
```

## 贪吃蛇项目

见代码文件

## 小鱼打字项目

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>知播渔-小渔打字</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        html, body{
            width: 100%;
            height: 100%;
        }
        body{
            background: url("images/bg.jpg") center center no-repeat;
            background-size: cover;
            overflow: hidden;
        }
        img{
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
        .specter{
            width: 300px;
            height: 300px;
            background: url("images/yy.png") no-repeat;
            position: absolute;
            top: 1000px;
        }
        .specter>span{
            position: absolute;
            left: 70px;
            top: 200px;
            font-size: 60px;
            font-weight: bold;
            text-shadow: 5px 5px 5px #3e437a;
        }
    </style>
</head>
<body>
<img src="images/play.png" alt="">
<!--
注意点:
在谷歌浏览器中默认情况下不允许自动播放音乐, 只有用户和网页交互之后才可以播放音乐
解决方案一:
修改谷歌浏览器的配置
1.在地址栏输入: chrome://flags/
2.在搜索框输入: autoplay
3.修改autoplay的选项为 no user xxx
解决方案二:
让用户和网页交互之后再播放
-->
<audio src="media/bg.ogg" loop></audio>
<!--<div class="specter"><span>m</span></div>-->
<!--<div class="specter"><span>m</span></div>-->
<script>
    let oImg = document.querySelector("img");
    let oAudio = document.querySelector("audio");
    let list = []; // 定义数组保存所有创建出来的鬼魂对象
    oImg.onclick = function () {
        // 1.移出开始游戏按钮
        oImg.parentNode.removeChild(oImg);
        oAudio.play();
        // 2.创建幽灵
        setInterval(function () {
            let s =  new Specter();
            s.fly();
            list.push(s);
        }, 1000);
    };
    class Specter{
        constructor(){
            // 1.创建div并设置样式
            let oDiv = document.createElement("div");
            // oDiv.className = "specter";
            oDiv.style.top = "1000px";
            oDiv.style.left = Math.random() * 1500 + "px";
            // 2.创建span并设置内容
            let oSpan = document.createElement("span");
            // oSpan.innerText = "m";
            let key = this.generateKey();
            oSpan.innerText = key;
            oDiv.className = "specter " + key;
            // 3.将span添加到div中
            oDiv.appendChild(oSpan);
            // 4.将div添加到body中
            document.body.appendChild(oDiv);
            this.oDiv = oDiv;
        }
        bomb(){
            // 1.删除当前的幽灵
            document.body.removeChild(this.oDiv);
            // 2.删除当前幽灵对应的定时器
            clearInterval(this.timer);
        }
        fly(){
            // 1.获取幽灵当前的top值
            let offset = parseInt(this.oDiv.style.top);
            // console.log(offset);
            /*
            let self = this;
            // 2.开启定时器不断修改幽灵位置
            this.timer = window.setInterval(function () {
                // console.log(this);
                // console.log(self);
                offset -= 20;
                if(offset <= -300){
                    self.bomb();
                }
                self.oDiv.style.top = offset + "px";
            }, 200);
            */
            this.timer = window.setInterval(()=>{
                offset -= 20;
                if(offset <= -300){
                    this.bomb();
                }
                this.oDiv.style.top = offset + "px";
            }, 200);
        }
        generateKey(){
            let num = Math.floor(Math.random() * (90 - 65 + 1)) + 65;
            return String.fromCharCode(num);
        }
    }
    /*
    let num = getRandomIntInclusive(65, 90);
    let char = String.fromCharCode(num);
    console.log(char);

    function getRandomIntInclusive(min, max) {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min + 1)) + min;
    }
    */

    document.body.onkeydown = function (event) {
        // console.log("666");
        // console.log(event.keyCode);
        // console.log(event.key);
        let key = event.key.toUpperCase();
        // console.log(key);
        let oDiv = document.querySelector("." + key);
        // console.log(oDiv);
        // document.body.removeChild(oDiv);
        // 1.根据div找到这个div对应的鬼魂对象在数组中的位置
        let currentIndex = list.findIndex(function (currentValue) {
            return currentValue.oDiv === oDiv;
        });
        if(currentIndex === -1) return;
        // 2.根据位置取出对应的鬼魂对象
        let currentSpecter = list[currentIndex];
        // 3.将鬼魂对应的div从body中删除
        currentSpecter.bomb();
        list.splice(currentIndex, 1);
    }
    // console.log("AbC".toLowerCase());
    // console.log("aBc".toUpperCase());
</script>
</body>
</html>
```

# BOM

## BOM开篇

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>49-JavaScript-BOM开篇</title>
</head>
<body>
<script>
    /*
    1.什么是BOM?
    DOM就是一套操作HTML标签的API(接口/方法/属性)
    BOM就是一套操作浏览器的API(接口/方法/属性)

    2.BOM中常见的对象
    window: 代表整个浏览器窗口
    注意: window是BOM中的一个对象, 并且是一个顶级的对象(全局)
    Navigator: 代表当前浏览器的信息, 通过Navigator我们就能判断用户当前是什么浏览器
    Location:  代表浏览器地址栏的信息, 通过Location我们就能设置或者获取当前地址信息
    History:   代表浏览器的历史信息, 通过History来实现刷新/上一步/下一步
    注意点: 出于隐私考虑, 我们并不能拿到用户所有的历史记录, 只能拿到当前的历史记录
    Screen:   代表用户的屏幕信息
     */
    console.log(window.screen);
</script>
</body>
</html>
```

## navigator

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>50-JavaScript-Navigator</title>
</head>
<body>
<script>
    // Navigator: 代表当前浏览器的信息, 通过Navigator我们就能判断用户当前是什么浏览器
    console.log(window.navigator);
    var agent = window.navigator.userAgent;
    if(/chrome/i.test(agent)){
        alert("当前是谷歌浏览器");
    }else if(/firefox/i.test(agent)){
        alert("当前是火狐浏览器");
    }else if(/msie/i.test(agent)){
        alert("当前是低级IE浏览器");
    }else if("ActiveXObject" in window){
        alert("当前是高级IE浏览器");
    }
</script>
</body>
</html>
```

## location

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>51-JavaScript-Location</title>
</head>
<body>
<button id="btn1">获取</button>
<button id="btn2">设置</button>
<button id="btn3">刷新</button>
<button id="btn4">强制刷新</button>
<script>
    // Location:  代表浏览器地址栏的信息, 通过Location我们就能设置或者获取当前地址信息
    let oBtn1 = document.querySelector("#btn1");
    let oBtn2 = document.querySelector("#btn2");
    let oBtn3 = document.querySelector("#btn3");
    let oBtn4 = document.querySelector("#btn4");
    // 获取当前地址栏的地址
    oBtn1.onclick = function(){
        console.log(window.location.href);
    }
    // 设置当前地址栏的地址
    oBtn2.onclick = function(){
        window.location.href = "http://www.it666.com";
    }
    // 重新加载界面
    oBtn3.onclick = function(){
        window.location.reload();
    }
    oBtn4.onclick = function(){
        window.location.reload(true);
    }
</script>
</body>
</html>
```

## history

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>52-JavaScript-History</title>
</head>
<body>
<h1>我是第一个界面</h1>
<button id="btn1">前进</button>
<button id="btn2">刷新</button>
<a href="52-JavaScript-History2.html">新的界面222</a>
<script>
    // History:   代表浏览器的历史信息, 通过History来实现刷新/前进/后退
    // 注意点: 出于隐私考虑, 我们并不能拿到用户所有的历史记录, 只能拿到当前的历史记录
    let oBtn1 = document.querySelector("#btn1");
    let oBtn2 = document.querySelector("#btn2");
    // 前进
    /*
    注意点:
    只有当前访问过其它的界面, 才能通过forward/go方法前进
    如果给go方法传递1, 就代表前进1个界面, 传递2就代表进行2个界面
    */
    oBtn1.onclick = function () {
        // window.history.forward();
        window.history.go(1);
    }
    // 刷新
    // 如果给go方法传递0, 就代表刷新
    oBtn2.onclick = function () {
        window.history.go(0);
    }
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>52-JavaScript-History</title>
</head>
<body>
<h2>我是第2222个界面</h2>
<button id="btn1">后退</button>
<script>
    // History:   代表浏览器的历史信息, 通过History来实现刷新/上一步/下一步
    let oBtn1 = document.querySelector("#btn1");

    // 后退
    /*
   注意点:
   只有当前访问过其它的界面, back/go方法后退
   如果给go方法传递-1, 就代表后退1个界面, 传递-2就代表后退2个界面
   */
    oBtn1.onclick = function () {
        // window.history.back();
        window.history.go(-1);
    }
</script>
</body>
</html>
```

## 获取元素宽高

**getComputedStyle**

**currentStyle**

**style**

**offsetWidth/offsetHeight**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>53-JavaScript-获取元素宽高其它方式</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            padding: 50px;
            border: 50px solid #000;
            background: red;
            background-clip: content-box;
        }
    </style>
</head>
<body>
<div id="box"></div>
<script>
    /*
    1.通过getComputedStyle获取宽高
    1.1获取的宽高不包括 边框和内边距
    1.2即可以获取行内设置的宽高也可以获取CSS设置的宽高
    1.3只支持获取, 不支持设置
    1.4只支持IE9及以上浏览器
    */
    /*
    var oDiv = document.getElementById("box");
    // oDiv.style.width = "166px";
    // oDiv.style.height = "166px";
    var style = getComputedStyle(oDiv);
    // style.width = "166px";
    // style.height = "166px";
    console.log(style.width);
    console.log(style.height);
    */

    /*
    2.通过currentStyle属性获取宽高
    2.1获取的宽高不包括 边框和内边距
    2.2即可以获取行内设置的宽高也可以获取CSS设置的宽高
    2.3只支持获取, 不支持设置
    2.4只支持IE9以下浏览器
    */
    /*
    var oDiv = document.getElementById("box");
    // oDiv.style.width = "166px";
    // oDiv.style.height = "166px";
    var style = oDiv.currentStyle;
    style.width = "166px";
    style.height = "166px";
    // console.log(style);
    console.log(style.width);
    console.log(style.height);
    */

    /*
    3.通过style属性获取宽高
    3.1获取的宽高不包括 边框和内边距
    3.2只能获取内设置的宽高, 不能获取CSS设置的宽高
    3.3可以获取也可以设置
    3.4高级低级浏览器都支持
    */
    /*
    var oDiv = document.getElementById("box");
    oDiv.style.width = "166px";
    oDiv.style.height = "166px";
    console.log(oDiv.style.width);
    console.log(oDiv.style.height);
    */

    /*
    4.offsetWidth和offsetHeight
    4.1获取的宽高包含 边框 + 内边距 + 元素宽高
    4.2即可以获取行内设置的宽高也可以获取CSS设置的宽高
    4.3只支持获取, 不支持设置
    4.4高级低级浏览器都支持
    */
    /*
    var oDiv = document.getElementById("box");
    // oDiv.offsetWidth = "166px";
    // oDiv.offsetHeight = "166px";
    oDiv.style.width = "166px";
    oDiv.style.height = "166px";
    console.log(oDiv.offsetWidth);
    console.log(oDiv.offsetHeight);
    */

    /*
    1.getComputedStyle/currentStyle/style
    获取的宽高不包括 边框和内边距
    2.offsetWidth/offsetHeight
    获取的宽高包括 边框和内边距

    3.getComputedStyle/currentStyle/offsetXXX
    只支持获取, 不支持设置
    4.style
    可以获取, 也可以设置

    5.getComputedStyle/currentStyle/offsetXXX
    即可以获取行内,也可以获取外链和内联样式
    6.style
    只能获取行内样式
    */
</script>
</body>
</html>
```

## 获取元素的位置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>54-JavaScript-获取元素位置其它方式</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 200px;
            height: 200px;
            margin-top: 100px;
            margin-left: 100px;
            background: blue;
            overflow: hidden;
            position: relative;
        }
        .son{
            width: 100px;
            height: 100px;
            margin-top: 100px;
            margin-left: 100px;
            background: red;
        }
    </style>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    /*
    1.offsetLeft和offsetTop作用
    获取元素到第一个定位祖先元素之间的偏移位
    如果没有祖先元素是定位的, 那么就是获取到body的偏移位
    */
    let oSDiv = document.querySelector(".son");
    oSDiv.onclick = function () {
        console.log(oSDiv.offsetLeft);
        console.log(oSDiv.offsetTop);
    }

    /*
    2.课后练习
    按照offsetWidth和offsetHeight讲解思路, 自己整理offsetLeft/marginLeft/left异同
    */
</script>
</body>
</html>
```

## 获取定位的父元素offsetParent

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>55-JavaScript-offsetParent</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .grand-father{
            width: 300px;
            height: 300px;
            margin-top: 100px;
            margin-left: 100px;
            background: deeppink;
            overflow: hidden;
            position: relative;
        }
        .father{
            width: 200px;
            height: 200px;
            margin-top: 100px;
            margin-left: 100px;
            background: blue;
            overflow: hidden;
        }
        .son{
            width: 100px;
            height: 100px;
            margin-top: 100px;
            margin-left: 100px;
            background: red;
        }
    </style>
</head>
<body>
<div class="grand-father">
    <div class="father">
        <div class="son"></div>
    </div>
</div>
<script>
    /*
    1.offsetParent作用
    获取元素的第一个定位祖先元素
    如果没有祖先元素是定位的, 那么就是获取到的就是body
    */
    let oSDiv = document.querySelector(".son");
    oSDiv.onclick = function () {
        console.log(oSDiv.offsetParent);
    }
</script>
</body>
</html>
```

## client属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>56-JavaScript-client属性</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            padding: 50px;
            border: 70px solid #000;
            background: red;
            background-clip: content-box;
        }
    </style>
</head>
<body>
<div id="box"></div>
<script>
    /*
    1.offsetWidth = 宽度 + 内边距 + 边框
     offsetHeight = 高度 + 内边距 + 边框
    2.clientWidth = 宽度 + 内边距
     clientHeight = 高度 + 内边距
    */
    let oDiv = document.querySelector("div");
    // console.log(oDiv.clientWidth);
    // console.log(oDiv.clientHeight);

    /*
    1.offsetLeft/offsetTop: 距离第一个定位祖先元素偏移位 || body
    2.clientLeft/clientTop: 左边框 和 顶部边框
    */
    console.log(oDiv.clientLeft);
    console.log(oDiv.clientTop);
    oDiv.scrollWidth
</script>
</body>
</html>
```

## scroll属性

**scrollWidth**

**scrollHeight**

**scrollLeft**

**scrollTop**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>57-JavaScript-scroll属性</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 100px;
            height: 100px;
            padding: 50px;
            border: 50px solid #000;
            background: red;
            background-clip: content-box;
            color: deepskyblue;
            overflow: auto;
        }
    </style>
</head>
<body>
<div id="box">
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
    我是内容<br/>
</div>
<script>
    /*
    1.内容没有超出元素范围时
    scrollWidth: = 元素宽度 + 内边距宽度 == clientWidth
    scrollHeight: = 元素高度 + 内边距的高度 == clientHeight
    */
    let oDiv = document.querySelector("div");
    console.log(oDiv.scrollWidth);
    console.log(oDiv.scrollHeight);
    /*
    2.内容超出元素范围时
    scrollWidth: = 元素宽度 + 内边距宽度 + 超出的宽度
    scrollHeight: = 元素高度 + 内边距的高度 + 超出的高度
    */

    /*
    3.scrollTop / scrollLeft
    scrollTop: 超出元素内边距顶部的距离
    scrollLeft: 超出元素内边距左边的距离
    */
    // console.log(oDiv.scrollTop);
    oDiv.onscroll = function () {
        console.log(oDiv.scrollTop);
    }
</script>
</body>
</html>
```

## 获取网页的宽高

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>58-JavaScript-获取网页宽高</title>
</head>
<body>
<script>
    /*
    // 注意点: innerWidth/innerHeight只能在IE9以及IE9以上的浏览器中才能获取
    // console.log(window.innerWidth);
    // console.log(window.innerHeight);

    // 注意点: documentElement.clientWidth/documentElement.clientHeight
    // 可以用于在IE9以下的浏览器的标准模式中获取
    // 浏览器在渲染网页的时候有两种模式"标准模式"/"混杂模式/怪异模式"
    // 默认情况下都是以标准模式来进行渲染的(CSS1Compat)
    // 如果网页没有书写文档声明, 那么就会按照"混杂模式/怪异模式"来进行渲染的(BackCompat)
    // documentElement  --> HTML  --> 整个网页
    // console.log(document.documentElement);
    // console.log(document.documentElement.clientWidth);
    // console.log(document.documentElement.clientHeight);

    // 注意点: 在混杂模式中利用如下的方式获取可视区域的宽高
    // console.log(document.compatMode);// CSS1Compat
    // console.log(document.body.clientWidth);
    // console.log(document.body.clientHeight);
    */

    let {width, height} = getScreen();
    console.log(width);
    console.log(height);

    function getScreen() {
        let width, height;
        if(window.innerWidth){
            width = window.innerWidth;
            height = window.innerHeight;
        }else if(document.compatMode === "BackCompat"){
            width = document.body.clientWidth;
            height = document.body.clientHeight;
        }else{
            width = document.documentElement.clientWidth;
            height = document.documentElement.clientHeight;
        }
        return {
            width: width,
            height: height
        }
    }
</script>
</body>
</html>
```

## 星空背景

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>59-JavaScript-星空背景</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        html,body{
            width: 100%;
            height: 100%;
            background: #000;
            overflow: hidden;
        }
        span{
            display: inline-block;
            width: 30px;
            height: 30px;
            background: url("images/star.png") no-repeat;
            background-size: 100% 100%;
            animation: flash 1s alternate infinite;
            position: absolute;
        }
        @keyframes flash {
            from{
                opacity: 0;
            }
            to{
                opacity: 1;
            }
        }
        span:hover{
            transform: scale(3, 3) rotate(180deg) !important;
            transition: all 1s;
        }
    </style>
</head>
<body>
<span></span>
<script>
    // 1.获取屏幕宽高
    let {width, height} = getScreen();

    // 2.创建星星
    for(let i = 0; i < 200; i++){
        // 1.添加星星
        let oSpan = document.createElement("span");
        document.body.appendChild(oSpan);
        // 2.随机位置
        let x = Math.random() * width;
        let y = Math.random() * height;
        oSpan.style.left = x + "px";
        oSpan.style.top = y + "px";
        // 3.随机缩放
        let scale = Math.random() * 2;
        oSpan.style.transform = `scale(${scale}, ${scale})`
        // 4.随机动画延迟
        let rate = Math.random() * 2;
        oSpan.style.animationDelay = rate + "s";
    }
    function getScreen() {
        let width, height;
        if(window.innerWidth){
            width = window.innerWidth;
            height = window.innerHeight;
        }else if(document.compatMode === "BackCompat"){
            width = document.body.clientWidth;
            height = document.body.clientHeight;
        }else{
            width = document.documentElement.clientWidth;
            height = document.documentElement.clientHeight;
        }
        return {
            width: width,
            height: height
        }
    }
</script>
</body>
</html>
```

## 弹性导航

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>60-JavaScript-弹性导航</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .nav{
            width: 900px;
            height: 63px;
            background: url("images/doubleOne.png") no-repeat right center;
            border: 1px solid #000;
            margin: 100px auto;
            position: relative;
        }
        ul{
            position: relative;
            z-index: 999;
        }
        ul>li{
            list-style: none;
            float: left;
            width: 88px;
            height: 63px;
            line-height: 63px;
            text-align: center;
        }
        span{
            display: inline-block;
            width: 88px;
            height: 63px;
            background: url("images/tMall.png") no-repeat;
            position: absolute;
            left: 0;
            top: 0;
        }
    </style>
</head>
<body>
<div class="nav">
    <ul>
        <li>双11狂欢</li>
        <li>服装会场</li>
        <li>数码家电</li>
        <li>家具建材</li>
        <li>母婴童装</li>
        <li>手机会场</li>
        <li>美妆会场</li>
        <li>进口会场</li>
        <li>飞猪旅行</li>
    </ul>
    <span></span>
</div>
<script src="js/animation2.js"></script>
<script>
    // 1.拿到需要操作的元素
    let oItems = document.querySelectorAll("li");
    let oSpan = document.querySelector("span");
    // 2.监听菜单的点击事件
    for(let i = 0; i < oItems.length; i++){
        let item = oItems[i];
        item.onclick = function () {
            // console.log(this.offsetLeft);
            // oSpan.style.left = this.offsetLeft + "px";
            easeAnimation(oSpan, {"left": this.offsetLeft});
        }
    }
</script>
</body>
</html>
```

## 百度登录效果（拖拽移动）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>61-JavaScript-百度登录</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        p{
            background: deepskyblue;
            text-align: center;
        }
        html,body{
            width: 100%;
            height: 100%;
        }
        .mask{
            width: 100%;
            height: 100%;
            position: fixed;
            left: 0;
            top: 0;
            background: rgba(0,0,0,0.5);
            display: none;
        }
        .login{
            width: 400px;
            height: 300px;
            background: purple;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            display: none;
            cursor: move;
        }
        .login>span{
            display: inline-block;
            width: 50px;
            height: 50px;
            background: red;
            position: absolute;
            top: 0;
            right: 0px;
        }
    </style>
</head>
<body>
<p>我是p标签</p>
<a href="http://www.it666.com">官网</a>
<div class="mask"></div>
<div class="login">
    <span></span>
</div>
<script>
    // 1.拿到需要操作的元素
    let oBtn = document.querySelector("p");
    let oMaskDiv = document.querySelector(".mask");
    let oLoginDiv = document.querySelector(".login");
    let oCloseBtn = document.querySelector("span");
    // 2.监听点击事件
    oBtn.onclick = function () {
        oMaskDiv.style.display = "block";
        oLoginDiv.style.display = "block";
    }
    oCloseBtn.onclick = function () {
        oMaskDiv.style.display = "none";
        oLoginDiv.style.display = "none";
    }
    // 3.监听登录框的按下和移动事件
    oLoginDiv.onmousedown = function (event) {
        event = event || window.event;
        // 1.计算固定不变的距离
        let x = event.pageX - oLoginDiv.offsetLeft;
        let y = event.pageY - oLoginDiv.offsetTop;
        // 2.监听移动事件
        oLoginDiv.onmousemove = function (event) {
            event = event || window.event;
            // 3.计算移动之后的偏移位
            let offsetX = event.pageX - x;
            let offsetY = event.pageY - y;
            // 4.重新设置登录框的位置
            oLoginDiv.style.left = offsetX + "px";
            oLoginDiv.style.top = offsetY + "px";
        }
    }
    oLoginDiv.onmouseup = function () {
        oLoginDiv.onmousemove = null;
    }
</script>
</body>
</html>
```

## 大图展示（放大镜效果）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>62-JavaScript-大图展示</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 400px;
            height: 400px;
            border: 1px solid #000;
            margin-top: 100px;
            margin-left: 100px;
            position: relative;
        }
        .small-box>span{
            display: inline-block;
            width: 200px;
            height: 200px;
            background: rgba(0,0,0,0.5);
            position: absolute;
            left: 0;
            top: 0;
            display: none;
        }
        .big-box{
            width: 400px;
            height: 400px;
            border: 1px solid #000;
            overflow: hidden;
            position: absolute;
            left: 410px;
            top: 0;
            display: none;
        }
        .big-box>img{
            position: absolute;
            left: 0;
            top: 0;
        }
    </style>
</head>
<body>
<div class="box">
    <div class="small-box">
        <img src="images/small.jpg" alt="">
        <span></span>
    </div>
    <div class="big-box">
        <img src="images/big.jpg" alt="">
    </div>
</div>
<script>
    // 1.拿到需要操作的元素
    let oSmallDiv = document.querySelector(".small-box");
    let oBigDiv = document.querySelector(".big-box");
    let oMask = document.querySelector("span");
    let oBoxDiv = document.querySelector(".box");
    let oBigImg = document.querySelector(".big-box>img");
    // 2.监听小图的移入移出事件
    oSmallDiv.onmouseenter = function () {
        oMask.style.display = "block";
        oBigDiv.style.display = "block";
    }
    oSmallDiv.onmouseleave = function () {
        oMask.style.display = "none";
        oBigDiv.style.display = "none";
    }
    // 3.监听鼠标在小图中的移动事件
    oSmallDiv.onmousemove = function (event) {
        event = event || window.event;
        // 1.获取鼠标当前在小图中的位置
        let mouseX = event.pageX - oBoxDiv.offsetLeft;
        let mouseY = event.pageY - oBoxDiv.offsetTop;

        // 2.重新计算鼠标的位置
        let maskWidth = oMask.offsetWidth;
        let maskHeight = oMask.offsetHeight;
        mouseX = mouseX - maskWidth / 2;
        mouseY = mouseY - maskHeight / 2;

        // 3.安全校验
        mouseX = mouseX < 0 ? 0 : mouseX;
        mouseY = mouseY < 0 ? 0 : mouseY;

        let smallWidth = oSmallDiv.offsetWidth;
        let smallHeight = oSmallDiv.offsetHeight;
        let maxMouseX = smallWidth - maskWidth;
        let maxMouseY = smallHeight - maskHeight;

        mouseX = mouseX > maxMouseX ? maxMouseX : mouseX;
        mouseY = mouseY > maxMouseY ? maxMouseY : mouseY;

        // 4.将鼠标当前在小图中的位置设置给蒙版
        oMask.style.left = mouseX +"px";
        oMask.style.top = mouseY +"px";

        // 5.计算大图移动的距离
        // 蒙版移动的距离 / 蒙版最大能移动的距离 = 大图移动的距离 / 大图最大能移动的距离
        // 蒙版移动的距离 / 蒙版最大能移动的距离 * 大图最大能移动的距离 = 大图移动的距离
        let maxBigX = oBigDiv.offsetWidth - oBigImg.offsetWidth;
        let maxBigY = oBigDiv.offsetHeight - oBigImg.offsetHeight;

        let bigX = mouseX / maxMouseX * maxBigX;
        let bigY = mouseY / maxMouseY * maxBigY;

        // 6.设置大图移动的位置
        oBigImg.style.left = bigX + "px";
        oBigImg.style.top = bigY + "px";
    }
</script>
</body>
</html>
```

## 网页滚动距离

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>63-JavaScript-网页滚动距离</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        p{
            line-height: 50px;
            width: 200%;
        }
    </style>
</head>
<body>
<p>我是正文1</p>
<p>我是正文2</p>
<p>我是正文3</p>
<p>我是正文4</p>
<p>我是正文5</p>
<p>我是正文6</p>
<p>我是正文7</p>
<p>我是正文8</p>
<p>我是正文9</p>
<p>我是正文10</p>
<p>我是正文11</p>
<p>我是正文12</p>
<p>我是正文13</p>
<p>我是正文14</p>
<p>我是正文15</p>
<p>我是正文16</p>
<p>我是正文17</p>
<p>我是正文18</p>
<p>我是正文19</p>
<p>我是正文20</p>
<p>我是正文21</p>
<p>我是正文22</p>
<p>我是正文23</p>
<p>我是正文24</p>
<p>我是正文25</p>
<p>我是正文26</p>
<p>我是正文27</p>
<p>我是正文28</p>
<p>我是正文29</p>
<p>我是正文30</p>
<p>我是正文31</p>
<p>我是正文32</p>
<p>我是正文33</p>
<p>我是正文34</p>
<p>我是正文35</p>
<p>我是正文36</p>
<p>我是正文37</p>
<p>我是正文38</p>
<p>我是正文39</p>
<p>我是正文40</p>
<p>我是正文41</p>
<p>我是正文42</p>
<p>我是正文43</p>
<p>我是正文44</p>
<p>我是正文45</p>
<p>我是正文46</p>
<p>我是正文47</p>
<p>我是正文48</p>
<p>我是正文49</p>
<p>我是正文50</p>
<script>
    window.onscroll = function () {
        // 1.IE9以及IE9以上的浏览器
        // console.log(window.pageXOffset);
        // console.log(window.pageYOffset);

        // 2.标准模式下浏览器
        // console.log(document.documentElement.scrollTop);
        // console.log(document.documentElement.scrollLeft);

        // 3.混杂(怪异)模式下浏览器
        // console.log(document.body.scrollTop);
        // console.log(document.body.scrollLeft);

        let {x, y} = getPageScroll();
        console.log(x, y);

        function getPageScroll() {
            let x, y;
            if(window.pageXOffset){
                x = window.pageXOffset;
                y = window.pageYOffset;
            }else if(document.compatMode === "BackCompat"){
                x = document.body.scrollLeft;
                y = document.body.scrollTop;
            }else{
                x = document.documentElement.scrollLeft;
                y = document.documentElement.scrollTop;
            }
            return {
                x: x,
                y: y
            }
        }
    }
</script>
</body>
</html>
```

## 跟随广告

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>64-JavaScript-跟随广告</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        img{
            /*position: fixed;*/
            position: absolute;
            left: 0;
            /*top: 50%;*/
            /*transform: translateY(-50%);*/
        }
        p{
            text-align: center;
            line-height: 40px;
        }
    </style>
</head>
<body>
<img src="images/left_ad.png" alt="">
<p>我是正文1</p>
<p>我是正文2</p>
<p>我是正文3</p>
<p>我是正文4</p>
<p>我是正文5</p>
<p>我是正文6</p>
<p>我是正文7</p>
<p>我是正文8</p>
<p>我是正文9</p>
<p>我是正文10</p>
<p>我是正文11</p>
<p>我是正文12</p>
<p>我是正文13</p>
<p>我是正文14</p>
<p>我是正文15</p>
<p>我是正文16</p>
<p>我是正文17</p>
<p>我是正文18</p>
<p>我是正文19</p>
<p>我是正文20</p>
<p>我是正文21</p>
<p>我是正文22</p>
<p>我是正文23</p>
<p>我是正文24</p>
<p>我是正文25</p>
<p>我是正文26</p>
<p>我是正文27</p>
<p>我是正文28</p>
<p>我是正文29</p>
<p>我是正文30</p>
<p>我是正文31</p>
<p>我是正文32</p>
<p>我是正文33</p>
<p>我是正文34</p>
<p>我是正文35</p>
<p>我是正文36</p>
<p>我是正文37</p>
<p>我是正文38</p>
<p>我是正文39</p>
<p>我是正文40</p>
<p>我是正文41</p>
<p>我是正文42</p>
<p>我是正文43</p>
<p>我是正文44</p>
<p>我是正文45</p>
<p>我是正文46</p>
<p>我是正文47</p>
<p>我是正文48</p>
<p>我是正文49</p>
<p>我是正文50</p>
<script src="js/animation2.js"></script>
<script src="js/tools.js"></script>
<script>
    // 1.拿到需要操作的元素
    let oAdImg = document.querySelector("img");
    // 2.计算广告图片top的值
    let screenHeight = getScreen().height;
    let imgHeight = oAdImg.offsetHeight;
    let offsetY = (screenHeight - imgHeight) / 2;
    // 3.将计算之后的top值设置给广告图片
    // oAdImg.style.top = offsetY + "px";
    easeAnimation(oAdImg, {"top": offsetY});
    // 4.监听网页的滚动事件
    window.onscroll = function () {
        // 获取到网页滚动出去的距离
        let pageOffsetY = getPageScroll().y;
        easeAnimation(oAdImg, {"top": offsetY + pageOffsetY});
    }
</script>
</body>
</html>
```

## 吸顶效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>65-JavaScript-吸顶效果</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        img{
            width: 100%;
            vertical-align: top;
        }
    </style>
</head>
<body>
<img src="images/nba-top.jpg" alt="" id="top">
<img src="images/nba-nav.jpg" alt="" id="nav">
<img src="images/nba-body.jpg" alt="">
<script src="js/tools.js"></script>
<script>
    // 1.拿到需要操作的元素
    let oTop = document.querySelector("#top");
    let oNav = document.querySelector("#nav");
    // 2.获取顶部LOGO区域的高度
    let topHeight = oTop.offsetHeight;
    // 3.监听网页的滚动
    window.onscroll = function () {
        // 拿到滚动出去的距离
        let offsetY = getPageScroll().y;
        // 判断滚动出去的距离是否已经大于等于顶部LOGO的高度
        if(offsetY >= topHeight){
            oNav.style.position = "fixed";
            oNav.style.top = "0px";
            oNav.style.left = "0px";
        }else{
            oNav.style.position = "";
        }
    }
</script>
</body>
</html>
```

## 返回顶部

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>66-JavaScript-返回顶部</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        img{
            width: 100%;
            vertical-align: top;
        }
        span{
            display: inline-block;
            width: 50px;
            height: 50px;
            line-height: 50px;
            text-align: center;
            background: red;
            position: fixed;
            right: 10px;
            bottom: 10px;
            display: none;
        }
    </style>
</head>
<body>
<img src="images/nba-top.jpg" alt="" id="top">
<img src="images/nba-nav.jpg" alt="" id="nav">
<img src="images/nba-body.jpg" alt="">
<span>TOP</span>
<script src="js/tools.js"></script>
<script>
    // 1.拿到需要操作的元素
    let oBackBtn = document.querySelector("span");
    // 2.监听网页的滚动
    window.onscroll = function () {
        // 获取滚动出去的距离
        let offsetY = getPageScroll().y;
        // 判断是否需要显示回滚按钮
        if(offsetY >= 200){
            oBackBtn.style.display = "block";
        }else{
            oBackBtn.style.display = "none";
        }
    }
    let timerId = null;
    // 3.监听回滚按钮的点击
    oBackBtn.onclick = function () {
        clearInterval(timerId);
        timerId = setInterval(function () {
            let begin = getPageScroll().y;
            let target = 0;
            let step = (target - begin) * 0.3;
            begin += step;
            if(Math.abs(Math.floor(step)) <= 1){
                clearInterval(timerId);
                window.scrollTo(0, 0);
                return;
            }
            window.scrollTo(0, begin);
        }, 50);
        // window.scrollTo(x, y);
        // x表示让网页在水平方向滚动到什么位置
        // y表示让网页在垂直方向滚动到什么位置
    }
</script>
</body>
</html>
```

## 楼层效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>67-JavaScript-楼层效果</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        html,body{
            width: 100%;
            height: 100%;
        }
        ul{
            width: 100%;
            height: 100%;
        }
        ul>li{
            list-style: none;
            width: 100%;
            height: 100%;
            font-size: 100px;
            text-align: center;
        }
        ol{
            position: fixed;
            left: 10px;
            top: 50%;
            transform: translateY(-50%);
        }
        ol>li{
            list-style: none;
            width: 100px;
            line-height: 40px;
            text-align: center;
            border: 1px solid #000;
        }
        .selected{
            background: deepskyblue;
        }
    </style>
</head>
<body>
<ul>
    <li>我是第1层</li>
    <li>我是第2层</li>
    <li>我是第3层</li>
    <li>我是第4层</li>
    <li>我是第5层</li>
</ul>
<ol>
    <li class="selected">第1层</li>
    <li>第2层</li>
    <li>第3层</li>
    <li>第4层</li>
    <li>第5层</li>
</ol>
<script src="js/animation2.js"></script>
<script src="js/tools.js"></script>
<script>
    // 1.初始化楼层的颜色
    let oPages = document.querySelectorAll("ul>li");
    let colorArr = ['green', 'blue', 'purple', 'red', 'yellow'];
    for(let i = 0; i < oPages.length; i++){
        let page = oPages[i];
        page.style.background = colorArr[i];
    }
    // 2.实现点击谁就选中谁
    let oItems = document.querySelectorAll("ol>li");
    let currentItem = oItems[0];
    // 获取可视区域的高度
    let screenHeight = getScreen().height;

    let timerId = null;
    for(let i = 0; i < oItems.length; i++){
        let item = oItems[i];
        item.onclick = function () {
            currentItem.className = "";
            this.className = "selected";
            currentItem = this;
            // 实现滚动
            // window.scrollTo(0, i * screenHeight);
            // 注意点: 通过documentElement.scrollTop来实现网页滚动, 在设置值的时候不能添加单位
            // document.documentElement.scrollTop = i * screenHeight + "px";
            // document.documentElement.scrollTop = i * screenHeight;
            clearInterval(timerId);
            timerId = setInterval(function () {
                let begin = document.documentElement.scrollTop;
                let target = i * screenHeight;
                let step = (target - begin) * 0.2;
                begin += step;
                if(Math.abs(Math.floor(step)) <= 1){
                    clearInterval(timerId);
                    document.documentElement.scrollTop = i * screenHeight;
                    return;
                }
                document.documentElement.scrollTop = begin;
            }, 50);
        }
    }
</script>
</body>
</html>
```

## 橱窗效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>68-JavaScript-橱窗效果</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .box{
            width: 800px;
            height: 190px;
            border: 1px solid #000;
            margin: 100px auto;
            overflow: hidden;
        }
        ul{
            list-style: none;
            display: flex;
            position: relative;
        }
        ul img{
            vertical-align: top;
        }
        .progress{
            width: 100%;
            height: 30px;
            background: #ccc;
            position: relative;
        }
        .progress>.line{
            width: 100px;
            height: 100%;
            background: deeppink;
            border-radius: 15px;
            position: absolute;
            left: 0;
            top: 0;
        }
    </style>
</head>
<body>
<div class="box">
    <ul>
        <li><img src="images/img1.jpg" alt=""></li>
        <li><img src="images/img2.jpg" alt=""></li>
        <li><img src="images/img3.jpg" alt=""></li>
        <li><img src="images/img4.jpg" alt=""></li>
        <li><img src="images/img5.jpg" alt=""></li>
        <li><img src="images/img6.jpg" alt=""></li>
        <li><img src="images/img7.jpg" alt=""></li>
        <li><img src="images/img8.jpg" alt=""></li>
        <li><img src="images/img9.jpg" alt=""></li>
        <li><img src="images/img10.jpg" alt=""></li>
    </ul>
    <div class="progress">
        <div class="line"></div>
    </div>
</div>
<script>
    // 1.拿到需要操作的元素
    let oUl = document.querySelector("ul");
    let oItems = document.querySelectorAll("ul>li");
    let oProgress = document.querySelector(".progress");
    let oBox = document.querySelector(".box");
    let oLine = document.querySelector(".line");
    let oImg = document.querySelectorAll("ul>li>img");
    // 2.计算UL的宽度
    let ulWidth = oItems[0].offsetWidth * oItems.length;
    // 3.设置UL的宽度
    oUl.style.width = ulWidth + "px";
    // 4.计算滚动条的宽度
    // 滚动条的宽度 / 滚动条滚动范围 = 容器的宽度 / 内容的范围
    // 滚动条的宽度  = 容器的宽度 / 内容的范围 * 滚动条滚动范围
    let progressWidth = oProgress.offsetWidth;
    let boxWidth = oBox.offsetWidth;
    let lineWidth = boxWidth / ulWidth * progressWidth;
    // 5.设置滚动条的宽度
    oLine.style.width = lineWidth + "px";
    // 计算滚动条最大能够滚动的范围
    let maxLineX = progressWidth - lineWidth;
    // 计算图片最大能够滚动的范围
    let maxImgX = boxWidth - ulWidth;
    // 6.监听鼠标按下的事件
    oLine.onmousedown = function (event) {
        event = event || window.event;
        // 0.拿到滚动条当前的位置
        let begin = parseFloat(oLine.style.left) || 0;
        // 1.拿到鼠标在滚动条中按下的位置
        let mouseX = event.pageX - oBox.offsetLeft;
        // 7.监听鼠标移动的事件
        oLine.onmousemove = function (event) {
            event = event || window.event;
            // 2.拿到鼠标在滚动条中移动之后的位置
            let moveMouseX = event.pageX - oBox.offsetLeft;
            // 3.计算偏移位
            let offsetX = moveMouseX - mouseX + begin;
            // 4.安全校验
            offsetX = offsetX < 0 ? 0 : offsetX;
            offsetX = offsetX > maxLineX ? maxLineX : offsetX;
            // 5.重新设置滚动条的位置
            oLine.style.left = offsetX + "px";
            // 6.计算图片的滚动距离
            // 滚动条滚动的距离 / 滚动条最大能够滚动的范围 = 图片滚动的距离 / 图片最大能够滚动的范围
            // 滚动条滚动的距离 / 滚动条最大能够滚动的范围 * 图片最大能够滚动的范围 = 图片滚动的距离
            let imgOffsetX = offsetX / maxLineX * maxImgX;
            // 7.重新设置图片的位置
            oUl.style.left = imgOffsetX + "px";
        }
    }
    document.onmouseup = function () {
        oLine.onmousemove = null;
    }
    
</script>
</body>
</html>
```

## 函数防抖

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>69-JavaScript-函数防抖</title>
</head>
<body>
<input type="text">
<script>
    /*
    1.什么是函数防抖[debounce]?
    函数防抖是优化高频率执行js代码的一种手段
    可以让被调用的函数在一次连续的高频操作过程中只被调用一次

    2.函数防抖作用
    减少代码执行次数, 提升网页性能

    3.函数防抖应用场景
    oninput / onmousemove  / onscroll / onresize等事件
    */
    let oInput = document.querySelector("input");
    let timerId = null;
    // abc
    oInput.oninput = function () {
        timerId && clearTimeout(timerId);
        timerId = setTimeout(function () {
            console.log("发送网络请求");
        }, 1000);
        // console.log(this.value);
        // console.log("发送网络请求");
    }
</script>
</body>
</html>
```

## 函数防抖练习

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>71-JavaScript-函数防抖练习</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: red;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
    </style>
</head>
<body>
<div></div>
<script src="js/tools.js"></script>
<script>
    /*需求: 让div的宽高永远是可视区域宽高的一半*/
    let oDiv = document.querySelector("div");
    function resetSize(){
        let {width, height} = getScreen();
        oDiv.style.width = width / 2 + "px";
        oDiv.style.height = height / 2 + "px";
    }
    resetSize();
    // 监听可视区域尺寸的变化
    /*
    window.onresize = function () {
        resetSize();
        console.log("尺寸的变化");
    }
     */
    window.onresize = debounce(function () {
        resetSize();
        console.log("尺寸的变化");
    }, 1000);
</script>
</body>
</html>
```

## 函数节流

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>72-JavaScript-函数节流</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        div{
            width: 200px;
            height: 200px;
            background: red;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
        }
    </style>
</head>
<body>
<div></div>
<script src="js/tools.js"></script>
<script>
    /*
    1.什么是函数节流[throttle]?
    函数节流也是优化高频率执行js代码的一种手段
    可以减少高频调用函数的执行次数

    2.函数节流作用
    减少代码执行次数, 提升网页性能

    3.函数节流应用场景
    oninput / onmousemove  / onscroll / onresize等事件

    4.函数节流和函数防抖区别
    函数节流是减少连续的高频操作函数执行次数  (例如连续调用10次, 可能只执行3-4次)
    函数防抖是让连续的高频操作时函数只执行一次(例如连续调用10次, 但是只会执行1次)
    */

    let oDiv = document.querySelector("div");
    function resetSize(){
        let {width, height} = getScreen();
        oDiv.style.width = width / 2 + "px";
        oDiv.style.height = height / 2 + "px";
    }
    resetSize();
    // 监听可视区域尺寸的变化
    /*
    window.onresize = debounce(function () {
        resetSize();
        console.log("尺寸的变化");
    }, 1000);
     */
    let timerId = null;
    let flag = true;
    window.onresize = function () {
        if(!flag){ // if(false) if(true) if(false)
            return;
        }
        flag = false;
        timerId && clearTimeout(timerId);
        timerId = setTimeout(function () {
            flag = true;
            resetSize();
            console.log("尺寸的变化");
        }, 500);
        // resetSize();
        // console.log("尺寸的变化");
    }
</script>
</body>
</html>
```

## 工具函数

```js
(function () {
    function getScreen() {
        let width, height;
        if(window.innerWidth){
            width = window.innerWidth;
            height = window.innerHeight;
        }else if(document.compatMode === "BackCompat"){
            width = document.body.clientWidth;
            height = document.body.clientHeight;
        }else{
            width = document.documentElement.clientWidth;
            height = document.documentElement.clientHeight;
        }
        return {
            width: width,
            height: height
        }
    }
    function getPageScroll() {
        let x, y;
        if(window.pageXOffset){
            x = window.pageXOffset;
            y = window.pageYOffset;
        }else if(document.compatMode === "BackCompat"){
            x = document.body.scrollLeft;
            y = document.body.scrollTop;
        }else{
            x = document.documentElement.scrollLeft;
            y = document.documentElement.scrollTop;
        }
        return {
            x: x,
            y: y
        }
    }
    function addEvent(ele, name, fn) {
        if(ele.attachEvent){
            ele.attachEvent("on"+name, fn);
        }else{
            ele.addEventListener(name, fn);
        }
    }
    function getStyleAttr(obj, name) {
        if(obj.currentStyle){
            return obj.currentStyle[name];
        }else{
            return getComputedStyle(obj)[name];
        }
    }
    function debounce(fn, delay) { // fn = test
        let timerId = null;
        return function () {
            let self = this;
            let args = arguments;
            timerId && clearTimeout(timerId);
            timerId = setTimeout(function () {
                fn.apply(self, args);
            }, delay || 1000);
        }
    }
    function throttle(fn, delay) { // fn = test
        let timerId = null;
        let flag = true;
        return function () {
            if(!flag) return;
            flag = false;
            let self = this;
            let args = arguments;
            timerId && clearTimeout(timerId);
            timerId = setTimeout(function () {
                flag = true;
                fn.apply(self, args);
            }, delay || 1000);
        }
    }

    window.getScreen = getScreen;
    window.getPageScroll = getPageScroll;
    window.addEvent = addEvent;
    window.getStyleAttr = getStyleAttr;
    window.debounce = debounce;
    window.throttle = throttle;
})();
```

## 图片的加载问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图片的加载问题</title>
</head>
<body>
<script>
    /*
    // 1.创建一个img标签
    let oImg = document.createElement("img");
    // 2.设置img标签的图片地址
    oImg.src = "images/img01.jpg";
    // 3.将img标签添加到body中
    document.body.appendChild(oImg);
    console.log(oImg.offsetHeight);
    oImg.onload = function () {
        console.log(oImg.offsetHeight);
    }
     */
    /*
    preLoadImage("images/img01.jpg", function (oImg) {
        document.body.appendChild(oImg);
        console.log(oImg.offsetHeight);
    });
     */

    let urls = ["images/img01.jpg", "images/img02.jpg", "images/img03.jpg"];
    preLoadImages(urls, function (oImages) {
        console.log(oImages);
    });
    function preLoadImage(url, fn) {
        // 1.创建一个img标签
        let oImg = document.createElement("img");
        // 2.设置img标签的图片地址
        oImg.src = url;
        // 3.监听图片是否加载完成了
        oImg.onload = function () {
            fn(oImg);
        }
    }
    function preLoadImages(urls, fn) {
        // 1.定义变量记录总共有多少张图片需要加载
        let totalCount = urls.length;
        // 2.定义变量记录当前已经加载了多少张图片
        let count = 0;
        // 3.定义数组保存所有加载好的图片
        let oImages = [];
        // 4.循环遍历加载图片
        for(let i = 0; i < urls.length; i ++){
            let url = urls[i];
            preLoadImage(url, function (oImg) {
                oImages.push(oImg);
                count++;
                if(count === totalCount){
                    fn(oImages);
                }
            });
        }
    }
</script>
</body>
</html>
```

## 瀑布流项目

```js
window.onload = function () {
    let oMain = document.querySelector(".main");
    // 0.预加载图片
    let urls = [];
    for(let i = 1; i <= 40; i++){
        let index = i < 10 ? "0" + i : i;
        urls.push(`images/img${index}.jpg`);
    }
    preLoadImages(urls, function (oImages) {
        // 1.初始化图片
        let oItems = initImages(oMain, oImages);
        // 2.初始化容器宽度
        let cols = resetWidth(oMain, oItems);
        // 3.实现流式布局
        waterfall(oItems, cols);

        // 4.监听可视区域的变化
        window.onresize = throttle(function () {
            let oItems = document.querySelectorAll(".box");
            let cols = resetWidth(oMain, oItems);
            waterfall(oItems, cols);
        }, 500);
        // 5.监听图片加载事件
        loadImages(oMain, oItems);
    });
}
/**
 * 加载数据
 * @param oMain 存放数据的容器
 * @param oItems 加载之前的数据
 */
function loadImages(oMain, oItems) {
    // 1.监听网页的滚动事件
    window.onscroll = debounce(function () {
        // 1.拿到最后一个元素
        let lastItem = oItems[oItems.length - 1];
        // 2.拿到最后一个元素的offsetTop
        let lastItemTop = lastItem.offsetTop;
        // 3.拿到最后一个元素高度的一半
        let lastItemHeight = lastItem.offsetHeight / 2;
        // 4.拿到可视区域的高度
        let screenHeight = getScreen().height;
        // 5.拿到滚动出去的距离
        let offsetY = getPageScroll().y;
        // 6.判断是否需要加载新的图片
        if((lastItemTop + lastItemHeight) <= (screenHeight + offsetY)){
            // console.log("需要加载新的图片");
            oItems = initImages(oMain);
            let cols = resetWidth(oMain, oItems);
            waterfall(oItems, cols);
        }
    }, 500);
}

/**
 * 流式布局
 * @param oItems 需要布局的元素
 * @param cols 每一行的列数
 */
function waterfall(oItems, cols) {
    // 1.定义数组保存第一行所有元素的高度
    let rowHeight = [];
    // 2.遍历取出所有的图片
    for(let i = 0; i < oItems.length; i++){
        let item = oItems[i];
        if(i < cols){
            // 3.判断是否是第一行
            item.style.position  = "";
            rowHeight.push(item.offsetHeight);
            console.log(rowHeight);
        }else{
            // 4.如果不是第一行, 就按照指定规则来排版
            // 4.1找到第一行中最矮的那个元素
            let minHeight = Math.min.apply(this, rowHeight)
            // 4.2找到第一行中最矮那个元素的索引
            let minIndex = rowHeight.findIndex(function (value) {
                return value === minHeight;
            });
            // 4.3根据索引取出最矮的那个元素
            let minItem = oItems[minIndex];
            // 4.4获取到最矮那个元素的offsetLeft
            let minLeft = minItem.offsetLeft;
            // 4.5设置当前图片的位置
            item.style.position = "absolute";
            item.style.left = minLeft + "px";
            item.style.top = minHeight + "px";
            // 4.6修改当前列对应的高度
            rowHeight[minIndex] += item.offsetHeight;
        }
    }
    // console.log(rowHeight);
}

/**
 * 重新调整容器宽度
 * @param oMain 被调整的容器
 * @param oItems 容器中所有的元素
 * @returns {number} 一行多少列
 */
function resetWidth(oMain, oItems) {
    // 1.拿到可视区域的宽度
    let {width, height} = getScreen();
    // 2.获取到每一张图片的宽度
    let imgWidth = oItems[0].offsetWidth;
    // 3.计算一行可以放下多少张图片
    let cols = Math.floor(width / imgWidth);
    // 4.计算容器的宽度
    let mainWidth = cols * imgWidth;
    // 5.设置容器的宽度
    oMain.style.width = mainWidth + "px";
    oMain.style.margin = "0 auto";
    // 6.返回当前的列数
    return cols;
}
/**
 * 创建所有的图片
 */
function initImages(oMain, oImages) {
    // 1.创建图片
    for(let i = 0; i <= oImages.length - 1; i++){
        let oDiv = document.createElement("div");
        oDiv.className = "box";
        oMain.appendChild(oDiv);
        // document.body.appendChild(oDiv);
        // let oImg = document.createElement("img");
        // let index = i < 10 ? "0" + i : i;
        // oImg.src = `images/img${index}.jpg`;
        oDiv.appendChild(oImages[i]);
    }
    // 2.返回所有创建好的图片
    let oItems = document.querySelectorAll(".box");
    return oItems;
}

// 图片的预加载
function preLoadImage(url, fn) {
    // 1.创建一个img标签
    let oImg = document.createElement("img");
    // 2.设置img标签的图片地址
    oImg.src = url;
    // 3.监听图片是否加载完成了
    oImg.onload = function () {
        fn(oImg);
    }
}
function preLoadImages(urls, fn) {
    // 1.定义变量记录总共有多少张图片需要加载
    let totalCount = urls.length;
    // 2.定义变量记录当前已经加载了多少张图片
    let count = 0;
    // 3.定义数组保存所有加载好的图片
    let oImages = [];
    // 4.循环遍历加载图片
    for(let i = 0; i < urls.length; i ++){
        let url = urls[i];
        preLoadImage(url, function (oImg) {
            oImages.push(oImg);
            count++;
            if(count === totalCount){
                fn(oImages);
            }
        });
    }
}

```

## 视频播放器项目

```js
window.onload = function () {
    // 1.拿到需要操作的元素
    let oVideo = document.querySelector("video");
    let oFigure = document.querySelector("figure");
    let oTotalTime = document.querySelector(".total-time");
    let oPlay = document.querySelector("#play");
    let oCurrentTime = document.querySelector(".current-time");
    let oLine = document.querySelector(".line");
    let oProgress = document.querySelector(".progress");
    let oFullBtn = document.querySelector("#full");

    // 2.通过事件监听视频是否加载完毕了
    oVideo.oncanplay = function () {
        // 2.1隐藏加载动画
        oFigure.style.backgroundImage = "none";
        // 2.2显示视频
        oVideo.style.display = "block";
        // 2.3获取视频的总时长
        let obj = formatDate(oVideo.duration);
        // 2.4格式化获取到的时间 00:00:00
        oTotalTime.innerHTML = `${obj.hour}:${obj.minute}:${obj.second}`;
    }
    // 3.监听播放按钮的点击
    let flag = false;
    oPlay.onclick = function () {
        if(flag){
            flag = false;
            // 正在播放
            oPlay.className = "iconfont icon-bofang";
            // 切换图标
            // 暂停播放
            oVideo.pause();
        }else{
            flag = true;
            // 没有播放
            oPlay.className = "iconfont icon-zanting";
            // 切换图标
            // 播放视频
            oVideo.play();
        }
    }
    // 4.监听视频播放的进度
    oVideo.ontimeupdate = function () {
        // console.log("正在播放");
        // 4.1同步文本时间
        let obj = formatDate(oVideo.currentTime);
        oCurrentTime.innerHTML = `${obj.hour}:${obj.minute}:${obj.second}`;
        // 4.2同步进度条
        let res = oVideo.currentTime / oVideo.duration * 100;
        oLine.style.width = res + "%";
    }
    // 5.监听进度条的点击事件
    oProgress.onclick = function (event) {
        event = event || window.event;
        // 鼠标点击的位置 / 进度条的宽度 = 当前播放的时间 / 总时长
        // 鼠标点击的位置 / 进度条的宽度 * 总时长 = 当前播放的时间
        // 5.1获取鼠标当前点击的位置
        let currentTime = event.offsetX / oProgress.offsetWidth * oVideo.duration;
        // console.log(oVideo.duration);
        // console.log(currentTime);
        oVideo.currentTime = currentTime;
    }
    // 6.监听视频是否已经播放完毕了
    oVideo.onended = function () {
        // 6.0恢复播放时间
        oVideo.currentTime = 0;
        // 6.1恢复图标
        oPlay.className = "iconfont icon-bofang";
        // 6.2恢复时间
        oCurrentTime.innerHTML = "00:00:00";
        // 6.3恢复进度条
        oLine.style.width = "0%";
        // 6.4恢复标记
        flag = false;
    }
    // 7.监听全屏按钮的点击
    oFullBtn.onclick = function () {
        // oVideo.requestFullscreen();
        toFullVideo(oVideo);
    }
}
function toFullVideo(videoDom){
    if(videoDom.requestFullscreen){
        return videoDom.requestFullscreen();
    }else if(videoDom.webkitRequestFullScreen){
        return videoDom.webkitRequestFullScreen();
    }else if(videoDom.mozRequestFullScreen){
        return videoDom.mozRequestFullScreen();
    }else{
        return videoDom.msRequestFullscreen();
    }
}
function formatDate(time) {
    // 4.利用相差的总秒数 / 小时 % 24;
    let hour = Math.floor(time / (60 * 60) % 24);
    hour = hour >= 10 ? hour : "0" + hour;
    // 5.利用相差的总秒数 / 分钟 % 60;
    let minute = Math.floor(time / 60 % 60);
    minute = minute >= 10 ? minute : "0" + minute;
    // 6.利用相差的总秒数 % 秒数
    let second = Math.floor(time % 60);
    second = second >= 10 ? second : "0" + second;
    return {
        hour: hour,
        minute: minute,
        second: second,
    }
}
```

