# typescript

## 开篇

```js
/*
1.什么是TypeScript(TS)？
TypeScript简称TS
TS和JS之间的关系其实就是Less/Sass和CSS之间的关系
就像Less/Sass是对CSS进行扩展一样, TS也是对JS进行扩展
就像Less/Sass最终会转换成CSS一样, 我们编写好的TS代码最终也会换成JS

2.为什么需要TypeScript?
因为JavaScript是弱类型, 很多错误只有在运行时才会被发现
而TypeScript是强类型, 它提供了一套静态检测机制, 可以帮助我们在编译时就发现错误
... ...

3.TypeScript特点
支持最新的JavaScript新特特性
支持代码静态检查
支持诸如C,C++,Java,Go等后端语言中的特性
(枚举、泛型、类型转换、命名空间、声明文件、类、接口等)
... ...

江哥忠告:
不要学习TypeScript, 因为它的学习成本很低
不要学习TypeScript, 因为它能减少团队无效沟通
不要学习TypeScript, 因为它能让你的代码更健壮
不要学习TypeScript, 因为它能帮助你快速掌握其它后端语言
不要学习TypeScript, 因为你会迷恋它
* */
```

## 如何编译ts

```html
<!-- 
1.初始化一个自动打包的webpack项目
2.通过tsc --init初始化Typescript配置文件
3.通过npm install typescript ts-loader安装对应的loader
4.修改webpack的配置文件
entry:"./src/js/index.ts"
resolve:{
extensions:['.tsx','.ts','.js']
}
rules: [
    // ts编译配置
    {
    test: /\.tsx?$/,
    use: 'ts-loader',
    exclude: /node_modules/
    }
]
```

## 基础类型

```typescript
/* TypeScript支持与JavaScript几乎相同的数据类型，此外还提供了实用的枚举类型方便我们使用*/
// 数值类型 number
let val1:number; // 定义了一个名称叫做val1的变量, 这个变量中将来只能存储数值类型的数据
val1 = 123;
// val1 = "123"; // 会报错
// 注意点: 其它的用法和JS一样
// val1 = 0x11;
// val1 = 0o11;
// val1 = 0b11;
console.log(val1);

// 布尔类型 boolean
let val2:boolean;
val2 = true;
// val2 = 1; // 会报错
console.log(val2);

// 字符串类型 string
let val3:string;
val3 = "123";
val3 = `val1=${val1}, val2==${val2}`;
console.log(val3);
```

## 数组和元组类型

```typescript
// 数组类型
// 方式一
// 需求: 要求定义一个数组, 这个数组中将来只能存储数值类型的数据
let arr1:Array<number>; // 表示定义了一个名称叫做arr1的数组, 这个数组中将来只能够存储数值类型的数据
arr1 = [1, 3, 5];
// arr1 = ['a', 3, 5]; // 报错
console.log(arr1);

// 方式二
// 需求: 要求定义一个数组, 这个数组中将来只能存储字符串类型的数据
let arr2:string[]; // 表示定义了一个名称叫做arr2的数组, 这个数组中将来只能够存储字符串类型的数据
arr2 = ['a', 'b', 'c'];
// arr2 = [1, 'b', 'c']; // 报错
console.log(arr2);

// 联合类型
let arr3:(number | string)[];// 表示定义了一个名称叫做arr3的数组, 这个数组中将来既可以存储数值类型的数据, 也可以存储字符串类型的数据
arr3 = [1, 'b', 2, 'c'];
// arr3 = [1, 'b', 2, 'c', false]; // 报错
console.log(arr3);

// 任意类型
let arr4:any[]; // 表示定义了一个名称叫做arr4的数组, 这个数组中将来可以存储任意类型的数据
arr4 = [1, 'b', false];
console.log(arr4);

// 元祖类型
// TS中的元祖类型其实就是数组类型的扩展
// 元祖用于保存定长定数据类型的数据
let arr5:[string, number, boolean]; // 表示定义了一个名称叫做arr5的元祖, 这个元祖中将来可以存储3个元素, 第一个元素必须是字符串类型, 第二个元素必须是数字类型, 第三个元素必须是布尔类型
arr5 = ['a', 1, true];
// arr5 = ['a', 1, true, false]; // 超过指定的长度会报错
arr5 = ['a', 1, true];
console.log(arr5);
```

## 枚举类型

```js
/*
枚举类型是TS为JS扩展的一种类型, 在原生的JS中是没有枚举类型的
枚举用于表示固定的几个取值
例如: 一年只有四季、人的性别只能是男或者女
*/
enum Gender{ // 定义了一个名称叫做Gender的枚举类型, 这个枚举类型的取值有两个, 分别是Male和Femal
    Male,
    Femal
}
let val:Gender; // 定义了一个名称叫做val的变量, 这个变量中只能保存Male或者Femal
val = Gender.Male;
val = Gender.Femal;
// val = 'nan'; // 报错
// val  = false;// 报错
// 注意点: TS中的枚举底层实现的本质其实就是数值类型, 所以赋值一个数值不会报错
// val = 666; // 不会报错
// console.log(Gender.Male); // 0
// console.log(Gender.Femal);// 1

// 注意点: TS中的枚举类型的取值, 默认是从上至下从0开始递增的
//         虽然默认是从0开始递增的, 但是我们也可以手动的指定枚举的取值的值
// 注意点: 如果手动指定了前面枚举值的取值, 那么后面枚举值的取值会根据前面的值来递增
// console.log(Gender.Male); // 6
// console.log(Gender.Femal);// 7

// 注意点: 如果手动指定了后面枚举值的取值, 那么前面枚举值的取值不会受到影响
// console.log(Gender.Male); // 0
// console.log(Gender.Femal);// 6

// 注意点: 我们还可以同时修改多个枚举值的取值, 如果同时修改了多个, 那么修改的是什么最后就是什么
// console.log(Gender.Male); // 8
// console.log(Gender.Femal);// 6

// 我们可以通过枚举值拿到它对应的数字
console.log(Gender.Male); // 0
// 我们还可以通过它对应的数据拿到它的枚举值
console.log(Gender[0]); // Male


// 探究底层实现原理
/*
var Gender;
(function (Gender) {
 // Gender[key] = value;
    Gender[Gender["Male"] = 0] = "Male";
    Gender[Gender["Femal"] = 1] = "Femal";
})(Gender || (Gender = {}));

let Gender = {};
Gender["Male"] = 0;
Gender[0] = "Male";
Gender["Femal"] = 1;
Gender[1] = "Femal";
* */
```

## any-void类型

```js
// any类型
// any表示任意类型, 当我们不清楚某个值的具体类型的时候我们就可以使用any
// 一般用于定义一些通用性比较强的变量, 或者用于保存从其它框架中获取的不确定类型的值
// 在TS中任何数据类型的值都可以负责给any类型
// let value:any; // 定义了一个可以保存任意类型数据的变量
// value = 123;
// value = "abc";
// value = true;
// value = [1, 3, 5];


// void类型
// void与any正好相反, 表示没有任何类型, 一般用于函数返回值
// 在TS中只有null和undefined可以赋值给void类型
function test():void {
    console.log("hello world");
}
test();

let value:void; // 定义了一个不可以保存任意类型数据的变量, 只能保存null和undefined
// value = 123; // 报错
// value = "abc";// 报错
// value = true;// 报错
// 注意点: null和undefined是所有类型的子类型, 所以我们可以将null和undefined赋值给任意类型
// value = null; // 不会报错
value = undefined;// 不会报错
```

## never和object类型

```js
// Never类型
// 表示的是那些永不存在的值的类型
// 一般用于抛出异常或根本不可能有返回值的函数
// function demo():never {
//     throw new Error('报错了');
// }
// demo();

// function demo2():never {
//     while (true){}
// }
// demo2();

// Object类型
// 表示一个对象
let obj:object; // 定义了一个只能保存对象的变量
// obj = 1;
// obj = "123";
// obj = true;
obj = {name:'lnj', age:33};
console.log(obj);
```

## 类型断言(其他语言的强制类型转换)

```js
/*
1.什么是类型断言?
TS中的类型断言和其它编程语言的类型转换很像, 可以将一种类型强制转换成另外一种类型
类型断言就是告诉编译器, 你不要帮我们检查了, 相信我，我知道自己在干什么。

例如: 我们拿到了一个any类型的变量, 但是我们明确的知道这个变量中保存的是字符串类型
      此时我们就可以通过类型断言告诉编译器, 这个变量是一个字符串类型
      此时我们就可以通过类型断言将any类型转换成string类型, 使用字符串类型中相关的方法了
* */
let str:any = 'it666';
// 方式一
// let len = (<string>str).length;
// 方式二
// 注意点: 在企业开发中推荐使用as来进行类型转换(类型断言)
//         因为第一种方式有兼容性问题, 在使用到了JSX的时候兼容性不是很好
let len = (str as string).length;
console.log(len);
```

## 变量声明和解构

```js
/*
TS中变量声明和解构
和ES6一样, 移步"从零玩转JavaScript核心+新特性③ "
10-JavaScript变量定义-ES6(掌握)
60-JavaScript-变量作用域-ES6(掌握)
72-JavaScript-数组解构赋值-ES6(掌握)
87-JavaScript-函数扩展运算符-ES6(掌握)
88-JavaScript-函数形参默认值-ES6(掌握)
134-JavaScript-对象解构赋值-ES6(掌握)
* */
```

## 接口

### 接口约束属性

```js
/*
1.什么是接口类型?
和number,string,boolean,enum这些数据类型一样,
接口也是一种类型, 也是用来约束使用者的
如果是下面这种方式，那么属性名和传入的对象的属性名必须相互对应，因为内部是通过解构赋值来实现的
* */
// 定义一个接口类型
interface FullName{
    firstName:string
    lastName:string
}
let obj = {
    firstName:'Jonathan',
    lastName:'Lee'
    // lastName:18
};
// 需求: 要求定义一个函数输出一个人完整的姓名, 这个人的姓必须是字符串, 这个人的名也必须是一个字符
function say({firstName, lastName}:FullName):void {
    console.log(`我的姓名是:${firstName}_${lastName}`);
}
say(obj);
```

### 可选属性和索引签名

```js
// 定义一个接口
interface FullName{
    firstName:string
    lastName:string
    middleName?:string
    [propName:string]:any
}
// 需求: 如果传递了middleName就输出完整名称, 如果没有传递middleName, 那么就输出firstName和lastName
function say({firstName, lastName, middleName}:FullName):void {
    // console.log(`我的姓名是:${firstName}_${lastName}`);
    if(middleName){
        console.log(`我的姓名是:${firstName}_${middleName}_${lastName}`);
    }else{
        console.log(`我的姓名是:${firstName}_${lastName}`);
    }
}
// 注意点: 如果使用接口来限定了变量或者形参, 那么在给变量或者形参赋值的时候,
// 赋予的值就必须和接口限定的一模一样才可以, 多一个或者少一个都不行

// say({firstName:'Jonathan'});
// say({firstName:'Jonathan', lastName:'Lee', middleName:"666"});

// 注意点: 但是在企业开发中可以多一个也可能少一个
// 少一个或多个怎么做? 可选属性
// say({firstName:'Jonathan', lastName:'Lee', middleName:"666"}s);
// say({firstName:'Jonathan', lastName:'Lee'});
// 多一个或者多多个怎么做? 如果绕开TS检查
// 方式一: 使用类型断言
// say({firstName:'Jonathan', lastName:'Lee', middleName:"666", abc:'abc'} as FullName);
// 方式二: 使用变量
// let obj = {firstName:'Jonathan', lastName:'Lee', middleName:"666", abc:'abc'};
// say(obj);
// 方式三: 使用索引签名
say({firstName:'Jonathan', lastName:'Lee', middleName:"666", abc:'abc', 123:123, def:"def"});
```

### 索引签名和只读属性

```js
// 1.什么是索引签名?
// 索引签名用于描述那些“通过索引得到”的类型，比如arr[10]或obj["key"]
/*
interface FullName {
    [propName:string]:string
}
let obj:FullName = {
    // 注意点: 只要key和value满足索引签名的限定即可, 无论有多少个都无所谓
    firstName:'Jonathan',
    lastName:'Lee',
    // middleName:false // 报错
    // false: '666' // 无论key是什么类型最终都会自动转换成字符串类型, 所以没有报错
}
 */
// interface stringArray {
//     [propName:number]:string
// }
/*
let arr:stringArray = {
    0:'a',
    1:'b',
    2:'c'
};
 */
// let arr:stringArray = ['a', 'b', 'c'];
// console.log(arr[0]);
// console.log(arr[1]);
// console.log(arr[2]);

// 2.什么是只读属性?
// 让对象属性只能在对象刚刚创建的时候修改其值
/*
interface FullName {
    firstName:string
    readonly lastName:string
}
let myName:FullName = {
    firstName: 'Jonathan',
    lastName: 'Lee'
};
myName.lastName = 'Wang';
console.log(myName);
 */
/*
// TS内部对只对属性进行了扩展, 扩展出来了一个只读数组
// let arr2:Array<string> = ['a', 'b', 'c'];
let arr2:ReadonlyArray<string> = ['a', 'b', 'c'];
console.log(arr2[0]); // a
arr2[0] = '666';
console.log(arr2[0]); // 666
 */
```

### 函数接口和混合类型接口

```typescript
// 函数接口
// 我们除了可以通过接口来限定对象以外, 我们还可以使用接口来限定函数
/*
interface SumInterface {
    (a:number, b:number):number
}
let sum:SumInterface = function (x:number, y:number):number {
    return x + y;
}
let res = sum(10, 20);
console.log(res);
 */

// 混合类型接口
// 约定的内容中既有对象属性, 又有函数
// 要求定义一个函数实现变量累加
/*
let count = 0; // 会污染全局空间
function demo() {
    count++;
    console.log(count);
}
demo();
demo();
demo();
 */
/*
let demo = (()=>{ // 使用闭包确实可以解决污染全局空间的问题, 但是对于初学者来说不太友好
    let count = 0;
    return ()=>{
        count++;
        console.log(count);
    }
})();
demo();
demo();
demo();
 */
// 在JS中函数的本质是什么? 就是一个对象
// let demo = function () {
//     demo.count++;
// }
// demo.count = 0;
// demo();
// demo();
// demo();
interface CountInterface {
    ():void
    count:number
}
let getCounter = (function ():CountInterface {
    /*
    CountInterface接口要求数据既要是一个没有参数没有返回值的函数
                              又要是一个拥有count属性的对象
    fn作为函数的时候符合接口中函数接口的限定 ():void
    fn作为对象的时候符合接口中对象属性的限定  count:number
    * */
    let fn = <CountInterface>function () {
        fn.count++;
        console.log(fn.count);
    }
    fn.count = 0;
    return fn;
})();
getCounter();
getCounter();
getCounter();
```

### 接口的继承

```typescript
// 接口的继承
// TS中的接口和JS中的类一样是可以继承的
interface LengthInterface {
    length:number
}
interface WidthInterface {
    width:number
}
interface HeightInterface {
    height:number
}
interface RectInterface extends LengthInterface,WidthInterface,HeightInterface {
    // length:number
    // width:number
    // height:number
    color:string
}
let rect:RectInterface = {
    length:10,
    width:20,
    height:30,
    color:'red'
}
```

## 函数

### 函数的定义

**ts中函数的定义和js相同，除了类型的定义**

```typescript
// TS中的函数
// TS中的函数大部分和JS相同
/*
// 命名函数
function say1(name) {
    console.log(name);
}
// 匿名函数
let say2 = function (name) {
    console.log(name);
}
// 箭头函数
let say3 = (name) => {
    console.log(name);
}
 */
// 命名函数
function say1(name:string):void {
    console.log(name);
}
// 匿名函数
let say2 = function (name:string):void {
    console.log(name);
}
// 箭头函数
let say3 = (name:string):void =>{
    console.log(name);
}
```

### 函数声明和函数重载

```typescript
// TS函数完整格式
// 在TS中函数的完整格式应该是由函数的定义和实现两个部分组成的
/*
// 定义一个函数
let AddFun:(a:number, b:number)=>number;
// 根据定义实现函数
AddFun = function (x:number, y:number):number {
    return x + y;
};
let res = AddFun(10, 20);
console.log(res);
*/
/*
// 一步到位写法
let AddFun:(a:number, b:number)=>number =
function (x:number, y:number):number {
    return x + y;
};
let res = AddFun(20, 20);
console.log(res);
*/
/*
// 根据函数的定义自动推导对应的数据类型
let AddFun:(a:number, b:number)=>number =
    function (x, y) {
        return x + y;
    };
let res = AddFun(20, 20);
console.log(res);
*/

// TS函数声明
/*
// 先声明一个函数
type AddFun = (a:number, b:number)=>number;
// 再根据声明去实现这个函数
// let add:AddFun = function (x:number, y:number):number {
//     return x + y;
// };
let add:AddFun = function (x, y) {
    return x + y;
};
let res = add(30, 20);
console.log(res);
*/
// TS函数重载
// 函数的重载就是同名的函数可以根据不同的参数实现不同的功能
/*
function getArray(x:number):number[] {
    let arr = [];
    for(let i = 0; i <= x; i++){
        arr.push(i);
    }
    return arr;
}
function getArray(str:string):string[] {
    return str.split('');
}
 */
// 定义函数的重载
function getArray(x:number):number[];
function getArray(str:string):string[];
// 实现函数的重载
function getArray(value:any):any[] {
    if(typeof value === 'string'){
        return value.split('');
    }else{
        let arr = [];
        for(let i = 0; i <= value; i++){
            arr.push(i);
        }
        return arr;
    }
}
// let res = getArray(10);
let res = getArray('www.it666.com');
console.log(res);
```

### 可选参数和默认参数

```typescript
// 可选参数
// 需求: 要求定义一个函数可以实现2个数或者3个数的加法
/*
function add(x:number, y:number, z?:number):number {
    return x + y + (z ? z : 0);
}
// let res = add(10, 20);
let res = add(10, 20, 30);
console.log(res);
 */
/*
// 可选参数可以配置函数重载一起使用, 这样可以让函数重载变得更加强大
function add(x:number, y:number):number;
function add(x:number, y:number, z:number):number;
function add(x:number, y:number, z?:number) {
    return x + y + (z ? z : 0);
}
let res = add(10, 20, 30);
console.log(res);
*/
/*
function add(x:number, y?:number, z?:number):number {
// function add(x:number, y?:number, z:number):number { // 可选参数后面只能跟可选参数
    return x + (y ? y : 0) + (z ? z : 0);
}
// let res = add(10); // 可选参数可以是一个或多个
console.log(res);
*/

// 默认参数
// 详见"从零玩转JavaScript核心+新特性③"
// 88-JavaScript-函数形参默认值-ES6(掌握)
/*
function add(x:number, y:number=10):number {
    return x + y;
}
// let res = add(10);
let res = add(10, 30);
console.log(res);
*/

// 剩余参数
// 详见"从零玩转JavaScript核心+新特性③"
// 87-JavaScript-函数扩展运算符-ES6(掌握)
/*
function add(x:number, ...ags:number[]) {
    console.log(x);
    console.log(ags);
}
add(10, 20, 30, 40, 50)
 */
```

## 泛型(C++中的模板)

### 泛型使用

```typescript
/*
1.什么是泛型?
- 在编写代码的时候我们既要考虑代码的健壮性, 又要考虑代码的灵活性和可重用性
通过TS的静态检测能让我们编写的代码变得更加健壮, 但是在变得健壮的同时却丢失了灵活性和可重用性
所以为了解决这个问题TS推出了泛型的概念
- 通过泛型不仅可以让我们的代码变得更加健壮, 还能让我们的代码在变得健壮的同时保持灵活性和可重用性
* */
// 需求: 定义一个创建数组的方法, 可以创建出指定长度的数组, 并且可以用任意指定的内容填充这个数组
/*
let getArray = (value:number, items:number = 5):number[]=>{
    return new Array(items).fill(value);
};
// let arr = getArray(6, 3);
let arr = getArray("abc", 3); // 报错
console.log(arr);
 */
/*
let getArray = (value:any, items:number = 5):any[]=>{
    return new Array(items).fill(value);
};
// let arr = getArray("abc", 3);
let arr = getArray(6, 3);
// console.log(arr);
// 当前存储的问题:
// 1.编写代码没有提示, 因为TS的静态检测不知道具体是什么类型
// 2.哪怕代码写错了也不会报错, 因为TS的静态检测不知道具体是什么类型
let res = arr.map(item=>item.length); // ['abc', 'abc', 'abc'] => [3, 3, 3]
console.log(res);
 */

// 需求:要有代码提示, 如果写错了要在编译的时候报错
let getArray = <T>(value:T, items:number = 5):T[]=>{
    return new Array(items).fill(value);
};
// let arr = getArray<string>('abc');
// let arr = getArray<number>(6);
// 注意点: 泛型具体的类型可以不指定
//         如果没有指定, 那么就会根据我们传递的泛型参数自动推导出来
let arr = getArray('abc');
// let arr = getArray(6);
let res = arr.map(item=>item.length);
console.log(res);
```

### 泛型约束

**泛型配合接口一起使用，对泛型的数据做进一步的约束**

```typescript
/*
1.什么是泛型约束?
默认情况下我们可以指定泛型为任意类型
但是有些情况下我们需要指定的类型满足某些条件后才能指定
那么这个时候我们就可以使用泛型约束
* */
// 需求: 要求指定的泛型类型必须有Length属性才可以
interface LengthInterface{
    length:number
}
let getArray = <T extends LengthInterface>(value:T, items:number = 5):T[]=>{
    return new Array(items).fill(value);
};
let arr = getArray<string>('abc');
// let arr = getArray<number>(6);
let res = arr.map(item=>item.length);
```

### 在泛型约束中使用类型参数

**K extends keyof  T：意思是K必须是T中的key**

```typescript
/*
1.在泛型约束中使用类型参数?
一个泛型被另一个泛型约束, 就叫做泛型约束中使用类型参数
* */
// 需求: 定义一个函数用于根据指定的key获取对象的value
// interface KeyInterface{
//     [key:string]:any
// }
let getProps = <T, K extends keyof T>(obj:T, key:K):any=>{
    return obj[key];
}
let obj = {
    a:'a',
    b:'b'
}
// 代码不够健壮, 明明obj中没有c这个key但是却没有报错
// let res = getProps(obj, "c");
let res = getProps(obj, "a");
console.log(res);
```

## 类

### ts中的类

```typescript
/*
TS中的类和ES6中的类'几乎'一样
观看本节课程之前建议先观看:
"从零玩转JavaScript核心+新特性③"
126-JavaScript-ES6类和对象(掌握)
127-JavaScript-ES6类和对象注意点上(掌握)
128-JavaScript-ES6类和对象注意点下(掌握)
129-JavaScript-ES6继承(掌握)
... ...
* */
class Person {
    name:string; // 和ES6区别, 需要先定义实例属性, 才能够使用实例属性
    age:number;
    constructor(name:string, age:number){
        this.name = name;
        this.age = age;
    }
    say():void{
        console.log(`我的名称叫${this.name}, 我的年龄是${this.age}`);
    }
    static food:string; // 静态属性
    static eat():void{ // 静态方法
        console.log(`我正在吃${this.food}`);
    }
}
let p = new Person('lnj', 34);
p.say();
Person.food = '蛋挞';
Person.eat();

class Student extends Person{
    book:string;
    constructor(name:string, age:number, book:string){
        super(name, age);
        this.book = book;
    }
    say():void{
        console.log(`我是重写之后的say-${this.name}${this.age}${this.book}`);
    }
    static eat():void{
        console.log(`我是重写之后的eat-${this.food}`);
    }
}
let stu = new Student('zs', 18, '从零玩转');
stu.say();
Student.food = '冰淇淋';
Student.eat();
```

### 类属性修饰符

```typescript
// public(公开的)      :
// 如果使用public来修饰属性, 那么表示这个属性是公开的
// 可以在类的内部使用, 也可以在子类中使用, 也可以在外部使用

// protected(受保护的) :
// 如果使用protected来修饰属性, 那么表示这个属性是受保护的
// 可以在类的内部使用, 也可以在子类中使用

// private(私有的)     :
// 如果使用private来修饰属性, 那么表示这个属性是私有的
// 可以在类的内部使用

// readonly(只读的)    :
/*
class Person {
    public name:string; // 默认情况下就是public的
    protected age:number;
    private gender:string;
    constructor(name:string, age:number, gender:string){
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
    say():void{
        console.log(`name=${this.name},age=${this.age},gender=${this.gender}`);
    }
}
class Student extends Person{
    constructor(name:string, age:number, gender:string){
       super(name,age,gender);
    }
    say():void{
        // console.log(`name=${this.name}`);
        // console.log(`age=${this.age}`);
        // console.log(`gender=${this.gender}`);
    }
}
let p = new Person('lnj',34, 'male');
p.say();
// console.log(p.age);
// console.log(p.gender);

let stu = new Student('zs', 18, 'female');
stu.say();
// console.log(stu.age);
 */

/*
class Demo {
    readonly name:string;
    constructor(name:string){
        this.name = name;
    }
    static food:string;
}
let demo = new Demo('123');
console.log(demo.name);
// demo.name = 'abc';
console.log(demo.name);
*/
```

### 类方法修饰符

```typescript
// public :
// 如果使用public来修饰方法, 那么表示这个方法是公开的
// 可以在类的内部使用, 也可以在子类中使用, 也可以在外部使用

// protected :
// 如果使用protected来修饰方法, 那么表示这个方法是受保护的
// 可以在类的内部使用, 也可以在子类中使用

// private
// 如果使用private来修饰方法, 那么表示这个方法是私有的
// 可以在类的内部使用

/*
class Person {
    name:string;
    age:number;
    gender:string;
    constructor(name:string, age:number, gender:string){
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
    public sayName():void{
        console.log(`name=${this.name}`);
    }
    protected sayAge():void{
        console.log(`age=${this.age}`);
    }
    private sayGender():void{
        console.log(`gender=${this.gender}`);
    }
    say():void{
        this.sayName();
        this.sayAge();
        this.sayGender();
    }
}
class Student extends Person {
    constructor(name: string, age: number, gender: string) {
        super(name, age, gender);
    }
    say():void{
        this.sayName();
        this.sayAge();
        this.sayGender();
    }
}
let p = new Person('lnj', 34, 'male');
p.say();
p.sayName();
p.sayAge();
p.sayGender();
let stu = new Student('zs', 18, 'female');
stu.say();
*/

/*
需求: 有一个基类, 所有的子类都需要继承于这个基类, 但是我们不希望别人能够通过基类来创建对象
* */
/*
class Person {
    name:string;
    age:number;
    gender:string;
    protected constructor(name:string, age:number, gender:string){
        this.name = name;
        this.age = age;
        this.gender = gender;
    }
    say():void{
        console.log(`name=${this.name},age=${this.age},gender=${this.gender}`);
    }
}
class Student extends Person {
    constructor(name: string, age: number, gender: string) {
        super(name, age, gender);
    }
}
let p = new Person('lnj', 34, 'male');
let stu = new Student('zs', 18, 'female');
 */
```

### 类可选属性和参数属性

```typescript
// 可选属性
// 和接口中的可选属性一样, 可传可不传的属性
/*
class Person {
    // 注意点: 在TS中如果定义了实例属性, 那么就必须在构造函数中使用, 否则就会报错
    name:string;
    age?:number; // 可选属性
    constructor(name:string, age?:number){
        this.name = name;
        this.age = age;
    }
    // setNameAndAge(name:string, age:number){
    //     this.name = name;
    //     this.age = age;
    // }
}
let p = new Person('lnj');
console.log(p);
*/

// 参数属性
// 一句话搞定实例属性的接收和定义

/*
class Person {
    name:string;
    age:number;
    constructor(name:string, age?:number){
        this.name = name;
        this.age = age;
    }
}
* */
class Person {
    constructor(public name:string,public age:number){
    }
}
let p = new Person('lnj', 34);
console.log(p);
```

### 类存取器

```typescript
/*
1.什么是存取器?
通过getters/setters来截取对对象成员的访问
* */
class Person {
    private _age:number = 0;
    set age(val:number){
        console.log('进入了set age方法');
        if(val<0){
            throw new Error('人的年龄不能小于零');
        }
        this._age = val;
    }
    get age():number{
        console.log('进入了get age方法');
        return this._age;
    }
}
let p = new Person();
p.age = 34;
// p.age = -6; // p.age(-6);
console.log(p.age);
```

### 抽象类

```typescript
/*
1.什么是抽象类?
抽象类是专门用于定义哪些不希望被外界直接创建的类的
抽象类一般用于定义基类
抽象类和接口一样用于约束子类

2.抽象类和接口区别?
接口中只能定义约束, 不能定义具体实现
而抽象类中既可以定义约束, 又可以定义具体实现
* */
/*
class Person {
    name:string;
    age: number;
    protected constructor(name:string, age:number){
        this.name = name;
        this.age = age;
    }
}
class Student extends Person{
    constructor(name:string, age:number){
        super(name, age);
    }
}
// let p = new Person('lnj', 34);
let stu = new Student('lnj', 34);
console.log(stu);
 */

abstract class Person {
    abstract name:string;
    abstract say():void;
    eat():void{
        console.log(`${this.name}正在吃东西`);
    }
}
// let p = new Person();
class Student extends Person{
    name:string = 'lnj';
    say():void{
        console.log(`我的名字是${this.name}`);
    }
}
let stu = new Student();
stu.say();
stu.eat();
```

### 类和接口

```typescript
// 类"实现"接口
/*
interface PersonInterface {
    name:string;
    say():void;
}
// 只要实现的某一个接口, 那么就必须实现接口中所有的属性和方法
class Person implements PersonInterface{
    name:string = 'lnj';
    say():void{
        console.log(`我的名字叫:${this.name}`);
    }
}
let p = new Person();
p.say();
*/

// 接口"继承"类
class Person {
    // protected name:string = 'lnj';
    name:string = 'lnj';
    age:number = 34;
    protected say():void{
        console.log(`name = ${this.name}, age = ${this.age}`);
    }
}
// let p = new Person();
// p.say();
// 注意点: 只要一个接口继承了某个类, 那么就会继承这个类中所有的属性和方法
//         但是只会继承属性和方法的声明, 不会继承属性和方法实现
// 注意点: 如果接口继承的类中包含了protected的属性和方法, 那么就只有这个类的子类才能实现这个接口
interface PersonInterface extends Person{
    gender:string;
}

class Student extends Person implements PersonInterface{
    gender:string = 'male';
    name:string = 'zs';
    age:number = 18;
    say():void{
        console.log(`name = ${this.name}, age = ${this.age}, gender = ${this.gender}`);
    }
}
let stu = new Student();
stu.say();
```

### 类和泛型

```typescript
// 泛型类
class Chache<T> {
    arr:T[] = [];
    add(value:T):T{
        this.arr.push(value);
        return value;
    }
    all():T[]{
        return this.arr;
    }
}
let chache = new Chache<number>();
chache.add(1);
chache.add(3);
chache.add(5);
console.log(chache.all());
```

### 接口合并现象

```typescript
// 27-接口合并现象
// 当我们定义了多个同名的接口时, 多个接口的内容会自动合并
interface TestInterface {
    name:string;
}
interface TestInterface {
    age:number;
}
/*
interface TestInterface {
    name:string;
    age:number;
}
* */

class Person implements TestInterface{
    age:number = 19;
    name:string = 'lnj';
}
```

## 枚举

### 数字枚举和字符串枚举

```typescript
// TS中支持两种枚举, 一种是数字枚举, 一种是字符串枚举
// 1.数字枚举
// 默认情况下就是数字枚举
/*
enum Gender{
    Male,
    Female
}
console.log(Gender.Male);
console.log(Gender.Female);
 */

// 2.数字枚举注意点
/*
// 数字枚举的取值默认从0开始递增
// 数字枚举的取值可以是字面量, 也可以是常量, 也可以是计算的结果
const num = 666;
function getNum() {
    return 888;
}
enum Gender{
    // Male = 6,
    // Male = num, // 注意点: 如果使用常量给前面的枚举值赋值了, 那么后面的枚举值也需要手动的赋值
    // Female = 8
    Male = getNum(), // 注意点: 如果使用计算结果给前面的枚举值赋值了, 那么后面的枚举值也需要手动的赋值
    Female = 123
}
*/

//3.枚举反向映射
/*
// 可以根据枚举值获取到原始值
// 也可以根据原始值获取到枚举值
enum Gender{
    Male,
    Female
}
console.log(Gender.Male); // 0
console.log(Gender[0]); // Male
 */


// 4.字符串枚举
/*
enum Gender{
    Male = 'www.it666.com',
    Female = 'www.itzb.com' // 注意点: 如果使用字符串给前面的枚举值赋值了, 那么后面的枚举值也必须手动赋值
}
console.log(Gender.Male);
console.log(Gender.Female);
*/

// 5.字符串枚举注意点
/*
// 注意点: 如果使用字符串给前面的枚举值赋值了, 那么后面的枚举值也必须手动赋值
// 注意点: 和数字枚举不一样, 字符串枚举不能使用常量或者计算结果给枚举值赋值
// 注意点: 虽然字符串枚举不能够使用常量或者计算结果给枚举值赋值, 但是它可以使用内部的其它枚举值来赋值
const str = 'lnj';
function getStr() {
    return 'abc';
}
enum Gender{
    Male = 'www.it666.com',
    // Male = str,
    // Male = getStr(),
    Female = 'www.itzb.com',
    Yao = Female
}
console.log(Gender.Female);
console.log(Gender.Yao);
*/


// 6.异构枚举
// 枚举中既包含数字又包含字符串, 我们就称之为异构枚举
enum Gender{
    Male = 6,
    Female = 'nv'
}

console.log(Gender.Male);
console.log(Gender.Female);
console.log(Gender[6]);
// 注意点: 如果是字符串枚举, 那么无法通过原始值获取到枚举值
// console.log(Gender['nv']);
console.log(Gender);
```

### 枚举成员类型和联合类型

```typescript
// 1.枚举成员类型
// 我们就可以把枚举成员当做类型来使用
/*
enum Gender{
    Male = 'www.it666.com',
    Female = 'www.itzb.com'
}
interface TestInterface {
    age: Gender.Male
}
class Person implements TestInterface{
    age: Gender.Male
    // age: Gender.Female // 由于类型不匹配, 所以会报错
    // age: 0 // 注意点: 由于数字枚举的本质就是数值, 所以这里写一个数值也不会报错

    // age: Gender.Male
    // age: Gender.Female
    // age: 'www.it666.com' // 注意点: 如果是字符串枚举, 那么只能是枚举成员的值, 不能是其它的值
    // age: string
}
*/


// 2.联合枚举类型
// 2.1什么是联合类型?
// 联合类型就是将多种数据类型通过|连接起来
// 我们可以把枚举类型当做一个联合类型来使用

/*
let value:(number | string); // (number | string)联合类型
value = 1;
value = 6;
value = "123";
 */

enum Gender{
    Male ,
    Female
}
interface TestInterface {
    age: Gender // age: (Gender.Male | Gender.Female)
}
class Person implements TestInterface{
    // age: Gender.Male
    age: Gender.Female
}
```

### 运行时和常量枚举

```typescript
// 运行时枚举
// 枚举在编译之后是一个真实存储的对象, 所以可以在运行时使用
// 而像接口这种只是用来做约束做静态检查的代码, 编译之后是不存在的
/*
interface TestInterface {
    name:string;
    age:number;
}
enum Gender{
    Male,
    Female
}
*/

// 常量枚举
// 普通枚举和常量枚举的区别
// 普通枚举会生成真实存在的对象
// 常量枚举不会生成真实存在的对象, 而是利用枚举成员的值直接替换使用到的地方
enum Gender1{
    Male,
    Female
}
console.log(Gender1.Male === 0);

const enum Gender2{
    Male,
    Female
}
console.log(Gender2.Male === 0);
```

## 类型推断和兼容性

### 自动类型推断

```typescript
// 1.什么是自动类型推断?
// 不用明确告诉编译器具体是什么类型, 编译器就知道是什么类型

// 1.根据初始化值自动推断
/*
// 如果是先定义在初始化, 那么是无法自动推断的
// let value;
// value = 123;
// value = false;
// value = 'abc';

// 如果是定义的同时初始化, 那么TS就会自动进行类型推荐
// let value = 123; // let value:number = 123;
// value = 456;
// value = false;
// value = 'abc';

let arr = [1, 'a']; // let arr:(number | string) = [1, 'a'];
arr = ['a', 'b', 'c', 1, 3, 5, false]
 */

// 2.根据上下文类型自动推断
window.onmousedown = (event)=>{
    console.log(event.target);
}
```

### 类型兼容性

```typescript
// 基本兼容性
/*
interface TestInterface {
    name:string;
}

let p1 = {name:'lnj'};
let p2 = {age:18};
let p3 = {name:'lnj', age:18};

let t:TestInterface;
t = p1;
t = p2;
t = p3; // 可多不可少
 */
/*
interface TestInterface {
    name:string;
    children:{
        age:number
    };
}

let p1 = {name:'lnj', children:{age:18}};
let p2 = {name:'lnj',children:{age:'abc'}};

let t:TestInterface;
t = p1;
t = p2; // 会递归检查
 */
```

### 函数兼容性

```typescript
// 参数个数
/*
let fn1 = (x:number, y:number)=>{};
let fn2 = (x:number)=>{};
fn1 = fn2;
fn2 = fn1; // 可少不可多
 */

// 参数类型
/*
let fn1 = (x:number)=>{};
let fn2 = (x:number)=>{};
let fn3 = (x:string)=>{};
fn1 = fn2;
fn2 = fn1;
fn1 = fn3; // 必须一模一样
fn3 = fn1;
 */

// 返回值类型
/*
let fn1 = ():number=> 123;
let fn2 = ():number=> 456;
let fn3 = ():string=> 'abc';
fn1 = fn2;
fn2 = fn1;
fn1 = fn3; // 必须一模一样
fn3 = fn1;
 */

// 函数双向协变
/*
// 参数的双向协变
// let fn1 = (x:(number | string)) =>{};
// let fn2 = (x:number) =>{};
// fn1 = fn2;
// fn2 = fn1;

// 返回值双向协变
let fn1 = (x:boolean):(number | string) => x ? 123 : 'abc';
let fn2 = (x:boolean):number => 456;
// fn1 = fn2; // 但是可以将返回值是具体类型的赋值给联合类型的
fn2 = fn1; // 不能将返回值是联合类型的赋值给具体类型的
 */

// 函数重载
function add(x:number, y:number):number;
function add(x:string, y:string):string;
function add(x, y) {
    return x + y;
}

function sub(x:number, y:number):number;
function sub(x, y) {
    return x - y;
}

// let fn = add;
// fn = sub; // 不能将重载少的赋值给重载多的

let fn = sub;
fn = add; // 可以将重载多的赋值给重载少
```

### 枚举兼容性

```typescript
// 数字枚举与数值兼容
/*
enum Gender{
    Male,
    Female
}
let value:Gender;
value = Gender.Male;
value = 1
 */

// 数字枚举与数字枚举不兼容
/*
enum Gender{
    Male, // 0
    Female // 1
}
enum Animal{
    Dog, // 0
    Cat // 1
}
let value:Gender;
value = Gender.Male;
value = Animal.Dog; // 报错
*/

// 字符串枚举与字符串不兼容
/*
enum Gender{
    Male = 'abc',
    Female  = 'def'
}
let value:Gender;
value = Gender.Male;
value = 'abc';
 */
```

### 类兼容性

```typescript
// 只比较实例成员, 不比较类的构造函数和静态成员
/*
class Person {
    public name:string;
    // public age:number;
    public static age:number;
    constructor(name:string, age:number){}
}
class Animal {
    public name:string;
    constructor(name:string){}
}
let p: Person;
let a: Animal;
p = a;
a = p; // 可多不少
 */

// 类的私有属性和受保护属性会响应兼容性
/*
class Person {
    protected name:string;
}
class Animal {
    protected name:string;
}
let p: Person;
let a: Animal;
p = a;
a = p; // 可多不少
 */
```

### 泛型兼容性

```typescript
// 泛型只影响使用的部分, 不会影响声明的部分
interface TestInterface<T> {
    // age:T;
}
let t1: TestInterface<number>; // age:number
let t2: TestInterface<string>; // age:string
t1 = t2;
t2 = t1;
```

## 高级类型

### 交叉和联合类型

```typescript
/*
1.交叉类型
格式: type1 & type2 & ...
交叉类型是将多个类型合并为一个类型
* */
let mergeFn = <T, U>(arg1:T, arg2:U):(T & U)=>{
    let res = {} as (T & U);
    res = Object.assign(arg1, arg2);
    return res;
};
let res = mergeFn({name:'lnj'}, {age:18});
console.log(res);

/*
2.联合类型
格式: type1 | type2 | ...
联合类型是多个类型中的任意一个类型
* */
let value: (string | number);
value = 'abc';
value = 123;
```

### 类型保护

```typescript
/*
3.类型保护
对于联合类型的变量，在使用时如何确切告诉编译器它是哪一种类型
通过类型断言或者类型保护
* */
let getRandomValue = ():(string | number)=>{
    let num = Math.random();
    return (num >= 0.5) ? 'abc' : 123.123;
}
// let value = getRandomValue();
// console.log(value);
/*
// 虽然通过类型断言可以确切的告诉编译器当前的变量是什么类型,
// 但是每一次使用的时候都需要手动的告诉编译器, 这样比较麻烦, 冗余代码也比较多
if((value as string).length){
    console.log((value as string).length);
}else{
    console.log((value as number).toFixed());
}
 */
/*
// 定义了一个类型保护函数, 这个函数的'返回类型'是一个布尔类型
// 这个函数的返回值类型是, 传入的参数 + is 具体类型
function isString(value:(string | number)): value is string {
    return typeof value === 'string';
}
if(isString(value)){
    console.log(value.length);
}else{
    console.log(value.toFixed());
}
 */
/*
// 除了可以通过定义类型保护函数的方式来告诉编译器使用时联合类型的变量具体是什么类型以外
// 我们还可以使用typeof来实现类型保护
// 注意点:
// 如果使用typeof来实现类型保护, 那么只能使用 === / !==
// 如果使用typeof来实现类型保护, 那么只能保护 number/string/boolean/symbol类型
if(typeof value === 'string'){
    console.log(value.length);
}else{
    console.log(value.toFixed());
}
 */
// 除了可以通过typeof类实现类型保护以外, 我们还可以通过instanceof来实现类型保护
class Person {
    name:string = 'lnj';
}
class Animal {
    age: number = 18;
}
let getRandomObject = ():(Person | Animal)=>{
    let num = Math.random();
    return (num >= 0.5) ? new Person() : new Animal();
};
let obj = getRandomObject();
console.log(obj);
if(obj instanceof Person){
    console.log(obj.name);
}else{
    console.log(obj.age);
}
```

### null和undefined

```typescript
/*
1.null和undefined
TypeScript具有两种特殊的类型， null和 undefined，它们分别具有值null和undefined.
* */
// 默认情况下我们可以将 null和 undefined赋值给任意类型
// 默认情况下null和 undefined也可以相互赋值
// 注意点: 在企业开发中, 如果不想把 null和 undefined赋值给其它的类型
//         或者不想让 null和 undefined相互赋值, 那么我们就可以开启strictNullChecks
/*
let value1:null;
let value2:undefined;
value1 = value2;
value2 = value1;

let value3:number;
value3 = value1;
value3 = value2;
 */

// 如果我们开启了strictNullChecks, 还想把null和 undefined赋值给其它的类型
// 那么我们就必须在声明的时候使用联合类型
/*
let value:(number | null | undefined);
value = null;
value = undefined;
 */

// 对于可选属性和可选参数而言, 如果开启了strictNullChecks, 那么默认情况下数据类型就是联合类型
// 就是当前的类型 + undefined类型
class Person {
    name?:string
}
function say(age?:number) {

}
```

### 去除null或undefined检测

```typescript
// 去除 null或 undefined检测
function getLength(value:(string | null | undefined)) {
    value = 'abc';
    return ()=>{
        // return value.length; // 报错
        // return (value || '').length;
        // return (value as string).length;
        // 我们可以使用!来去除null和undefined
        // !的含义就是这个变量一定不是null和undefined
        return value!.length;
    }
}
let fn = getLength('www.it66.com');
let res = fn();
console.log(res);
```

### 类型别名

```typescript
/*
1.什么是类型别名?
类型别名就是给一个类型起个新名字, 但是它们都代表同一个类型
例如: 你的本名叫张三, 你的外号叫小三, 小三就是张三的别名, 张三和小三都表示同一个人
* */
// 给string类型起了一个别名叫做MyString, 那么将来无论是MyString还是string都表示string
// type MyString = string;
// let value:MyString;
// value = 'abc';
// value = 123;
// value = false;

// 类型别名也可以使用泛型
// type MyType<T> = {x:T, y:T};
// let value:MyType<number>;
// value = {x:123, y:456};
// value = {x:'123', y:456};
// value = {x:false, y:456};

// 可以在类型别名类型的属性中使用自己
/*
type MyType = {
    name:string,
    // 一般用于定义一些树状结构或者嵌套结构
    children?:MyType
}
let value:MyType = {
    name:'one',
    children:{
        name:'one',
        children:{
            name:'one',
        }
    }
}
 */

// 接口和类型别名是相互兼容的
type MyType = {
    name:string
}
interface MyInterface {
    name:string
}
let value1:MyType = {name:'lnj'};
let value2:MyInterface = {name:'zs'};
value1 = value2;
value2 = value1;
```

### 类型别名和接口

```typescript
// 接口和类型别名异同
// 1.都可以描述属性或方法
// type MyType = {
//     name:string;
//     say():void;
// }
// interface MyInterface {
//     name:string;
//     say():void;
// }

// 2.都允许拓展
/*
interface MyInterface {
    name:string;
    say():void;
}
interface MyInterface2 extends MyInterface{
    age:number;
}
let value:MyInterface2 = {
    name:'lnj',
    age:18,
    say():void{

    }
}
 */

/*
type MyType = {
    name:string;
    say():void;
}
type MyType2 = MyType & {
    age:number;
}
let value:MyType2 = {
    name:'lnj',
    age: 18,
    say():void{

    }
}
 */


// 3.type 可以声明基本类型别名，联合类型，元组等类型, interface不能
// type MyType1 = boolean;
// type MyType2 = string | number;
// type MyType3 = [string, boolean, number];

// 4.type不会自动合并
// interface MyInterface {
//     name:string
// }
// interface MyInterface {
//     age:number
// }
// let value:MyInterface  ={
//     name:'lnj',
//     age:18
// }

// type MyType = {
//     name:string
// }
// type MyType = {
//     age:number
// }
```

### 字面量类型

```typescript
/*
1.什么是字面量?
字面量就是源代码中一个固定的值
例如数值字面量: 1,2,3,...
例如字符串字面量: 'a','abc',...

2.在TS中我们可以把字面量作为具体的类型来使用
当使用字面量作为具体类型时, 该类型的取值就必须是该字面量的值
* */
// type MyNum = 1;
// let value1:MyNum = 1;
// let value2:MyNum = 2;
```

### 可辨识联合

```typescript
/*
1.什么是可辨识联合
具有共同的可辨识特征。
一个类型别名, 包含了具有共同的可辨识特征的类型的联合。
* */
interface Square {
    kind: "square"; // 共同的可辨识特征
    size: number;
}
interface Rectangle {
    kind: "rectangle"; // 共同的可辨识特征
    width: number;
    height: number;
}
interface Circle {
    kind: "circle"; // 共同的可辨识特征
    radius: number;
}
/*
Shape就是一个可辨识联合
因为: 它的取值是一个联合
因为: 这个联合的每一个取值都有一个共同的可辨识特征
* */
type Shape = (Square | Rectangle | Circle);

function aera(s: Shape) {
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.width * s.height;
        case "circle": return  Math.PI * s.radius ** 2; // **是ES7中推出的幂运算符
    }
}
```

### 可辨识联合完整性约束

```typescript
interface Square {
    kind: "square"; // 共同的可辨识特征
    size: number;
}
interface Rectangle {
    kind: "rectangle"; // 共同的可辨识特征
    width: number;
    height: number;
}
interface Circle {
    kind: "circle"; // 共同的可辨识特征
    radius: number;
}
/*
Shape就是一个可辨识联合
因为: 它的取值是一个联合
因为: 这个联合的每一个取值都有一个共同的可辨识特征
* */
type Shape = (Square | Rectangle | Circle);
/*
在企业开发中如果相对可辨识联合的完整性进行检查, 那么我们可以使用
方式一: 给函数添加返回值 + 开启strictNullChecks
方式二: 添加default + never
* */
function MyNever(x: never):never {
    throw new Error('可辨识联合处理不完整' + x);
}
function aera(s: Shape):number{
    switch (s.kind) {
        case "square": return s.size * s.size;
        case "rectangle": return s.width * s.height;
        case "circle": return  Math.PI * s.radius ** 2; // **是ES7中推出的幂运算符
        default:return MyNever(s)
    }
}
```

### 索引访问操作符

```typescript
// 通过[]索引类型访问操作符, 我们就能得到某个索引的类型
// class Person {
//     name:string;
//     age:number;
// }
// type MyType = Person['name'];

// 应用场景
// 需求: 获取指定对象, 部分属性的值, 放到数组中返回
/*
let obj = {
    name:'lnj',
    age:18,
    gender:true
}
function getValues<T, K extends keyof T>(obj:T, keys:K[]):T[K][] {
    let arr = [] as T[K][];
    keys.forEach(key=>{
        arr.push(obj[key]);
    })
    return arr;
}
let res = getValues(obj, ['name', 'age']);
console.log(res);
*/

// 索引访问操作符注意点
// 不会返回null/undefined/never类型
interface TestInterface {
    a:string,
    b:number,
    c:boolean,
    d:symbol,
    e:null,
    f:undefined,
    g:never
}
type MyType = TestInterface[keyof TestInterface];
```

### 映射类型上

```typescript
/*
1.什么是映射类型?
根据旧的类型创建出新的类型, 我们称之为映射类型
* */
/*
interface TestInterface1{
    name:string,
    age:number
}
interface TestInterface2 {
    readonly name:string,
    readonly age:number
}
*/
interface TestInterface1{
    name:string,
    age:number
}
interface TestInterface2{
    readonly name?:string,
    readonly age?:number
}
type ReadonlyTestInterface<T> = {
    // [P in keyof T]作用: 遍历出指定类型所有的key, 添加到当前对象上
    // readonly [P in keyof T]: T[P]
    // readonly [P in keyof T]?: T[P]
    -readonly [P in keyof T]-?: T[P]
}
type MyType = ReadonlyTestInterface<TestInterface2>


// 我们可以通过+/-来指定添加还是删除 只读和可选修饰符


// 由于生成只读属性和可选属性比较常用, 所以TS内部已经给我们提供了现成的实现
// Readonly / Partial

type MyType2 = Readonly<TestInterface1>
type MyType3 = Partial<TestInterface1>
type MyType4 = Partial<Readonly<TestInterface1>
```

### 映射类型下

```typescript
// Pick映射类型
// 将原有类型中的部分内容映射到新类型中
/*
interface TestInterface {
    name:string,
    age:number
}
type MyType = Pick<TestInterface, 'name'>
 */

// Record映射类型
// 他会将一个类型的所有属性值都映射到另一个类型上并创造一个新的类型
type Animal = 'person' | 'dog' | 'cat';
interface TestInterface {
    name:string;
    age:number;
}
type MyType = Record<Animal, TestInterface>

let res:MyType = {
    person:{
        name:'zs',
        age:18
    },
    dog:{
        name:'wc',
        age:3
    },
    cat:{
        name:'mm',
        age:2
    }
}
```

### 映射类型推断

```typescript
/*
由映射类型进行推断
对于Readonly， Partial和 Pick的映射类型, 我们可以对映射之后的类型进行拆包,
还原映射之前的类型, 这种操作我们称之为拆包
* */
interface MyInterface {
    name:string;
    age:number;
}
type MyType<T> = {
   +readonly [P in keyof T]: T[P];
}

type test = MyType<MyInterface>;

type UnMyType<T> = {
    -readonly [P in keyof T]: T[P];
}

type test2 = UnMyType<test>;
```

### 分布式条件类型

```typescript
/*
1.条件类型(三目运算)
判断前面一个类型是否是后面一个类型或者继承于后面一个类型
如果是就返回第一个结果, 如果不是就返回第二个结果
语法: T extends U ? X : Y;
* */
// type MyType<T> = T extends string ? string : any;
// type res = MyType<boolean>

/*
2.分布式条件类型?
被检测类型是一个联合类型的时候, 该条件类型就被称之为分布式条件类型
* */
// type MyType<T> = T extends any ? T : never;
// type res = MyType<string | number | boolean>;

// 从T中剔除可以赋值给U的类型。 Exclude
// type MyType<T, U> = T extends U ? never : T;
// type res = MyType<string | number | boolean, number>
// type res = Exclude<string | number | boolean, number>

// 提取T中可以赋值给U的类型。 Extract
// type res = Extract<string | number | boolean, number | string>

// 从T中剔除null和undefined。 NonNullable
// type res = NonNullable<string | null | boolean | undefined>

// 获取函数返回值类型。 ReturnType
// type res = ReturnType<(()=>number)>

// 获取一个类的构造函数参数组成的元组类型。 ConstructorParameters
// class Person {
//     constructor(name:string, age:number){}
// }
// type res = ConstructorParameters<typeof Person>;

// 获得函数的参数类型组成的元组类型。 Parameters
function say(name:string, age:number, gender:boolean) {
}
type res = Parameters<typeof say>;
```

### infer关键字

```typescript
/*
1.infer关键字
条件类型提供了一个infer关键字, 可以让我们在条件类型中定义新的类型
* */
// 需求: 定义一个类型, 如果传入的是数组, 就返回数组的元素类型,
//                     如果传入的是普通类型, 则直接返回这个类型
// type MyType<T> = T extends any[] ? T[number] : T;
// type res = MyType<string[]>;
// type res = MyType<number>;

// type MyType<T> = T extends Array<infer U> ? U : T;
// type res = MyType<string[]>;
// type res = MyType<number>;
```

### unknown

```typescript
/*
 1.什么是unknown类型?
 unknown类型是TS3.0中新增的一个顶级类型, 被称作安全的any
* */
// 1.任何类型都可以赋值给unknown类型
// let value:unknown;
// value = 123;
// value = "abc";
// value = false;

// 2.如果没有类型断言或基于控制流的类型细化, 那么不能将unknown类型赋值给其它类型
// let value1:unknown = 123;
// let value2:number;
// value2 = value1;
// value2 = value1 as number;
// if(typeof value1 === 'number'){
//     value2 = value1;
// }

// 3.如果没有类型断言或基于控制流的类型细化, 那么不能在unknown类型上进行任何操作
// let value1:unknown = 123;
// value1++;
// (value1 as number)++;
// if(typeof value1 === 'number'){
//      value1++;
// }

// 4.只能对unknown类型进行 相等或不等操作, 不能进行其它操作(因为其他操作没有意义)
// let value1:unknown = 123;
// let value2:unknown = 123;
// console.log(value1 === value2);
// console.log(value1 !== value2);
// console.log(value1 >= value2); // 虽然没有报错, 但是不推荐, 如果想报错提示, 可以打开严格模式

// 5.unknown与其它任何类型组成的交叉类型最后都是其它类型
// type MyType = number & unknown;
// type MyType = unknown & string;

// 6.unknown除了与any以外, 与其它任何类型组成的联合类型最后都是unknown类型
// type MyType = unknown | any;
// type MyType = unknown | number;
// type MyType = unknown | string | boolean;

// 7.never类型是unknown类型的子类型
// type MyType = never extends unknown ? true : false;

// 8.keyof unknown等于never
// type MyType = keyof unknown;

// 9.unknown类型的值不能访问其属性,方法,创建实例
// class Person {
//     name:string = 'lnj';
//     say():void{
//         console.log(`name = ${this.name}`);
//     }
// }
// let p:unknown = new Person();
// p.say();
// console.log(p.name);


// 10.使用映射类型时, 如果遍历的是unknown类型, 那么不会映射任何属性
// type MyType<T> = {
//     [P in keyof T]:any
// }
// type res = MyType<unknown>
```

### symbol

```typescript
/*
和ES6中的Symbol一样
从零玩转JS新特性+流行框架⑥
32-Symbol类型(掌握)
33-Symbol注意点(掌握)
* */
```

### 迭代器和生成器

```typescript
/*
和ES6迭代器一样
for...of
从零玩转JavaScript核心+新特性③
140-JavaScript-数组高级API上(掌握)
从零玩转JS新特性+流行框架⑥
34-Iterator接口(掌握)
35-Iterator接口应用场景(掌握)
* */
let someArray = [1, "string", false];
for (let entry of someArray) {
    console.log(entry); // 1, "string", false
}
/*
生成器
当生成目标为ES5或ES3，迭代器只允许在Array类型上使用。
在非数组值上使用 for..of语句会得到一个错误，
就算这些非数组值已经实现了Symbol.iterator属性。
为了解决这个问题, 编译器会生成一个简单的for循环做为for..of循环
* */
```

## 模块和命名空间

### 模块系统

```typescript
/*
TS中的模块几乎和ES6和Node中的模块一致
从零玩转NodeJS核心+原理⑩
27-NodeJS自定义模块(掌握)
28-Node模块导出数据几种方式(掌握)
29-exports和module.exports区别(掌握)
30-NodeJS-Require注意点(掌握)

从零玩转Webpack4+实现原理⑾
85-ES6-模块化上(掌握)
86-ES6-模块化中(掌握)
87-ES6-模块化下(掌握)
* */
/*
1.ES6模块
1.1分开导入导出
export xxx;
import {xxx} from "path";

1.2一次性导入导出
export {xxx, yyy, zzz};
import {xxx, yyy, zzz} from "path";

1.3默认导入导出
export default xxx;
import xxx from "path";
*/
/*
2.Node模块
1.1通过exports.xxx = xxx导出
通过const xxx = require("path");导入
通过const {xx, xx} = require("path");导入

1.2通过module.exports.xxx = xxx导出
通过const xxx = require("path");导入
通过const {xx, xx} = require("path");导入
* */
/*
ES6的模块和Node的模块是不兼容的, 所以TS为了兼容两者就推出了
export = xxx;
import xxx = require('path');
* */
import obj = require("./55/test");
console.log(obj);
```

### 命名空间

```typescript
/*
1.什么是命名空间?
命名空间可以看做是一个微型模块,
当我们先把相关的业务代码写在一起, 又不想污染全局空间的时候, 我们就可以使用命名空间
本质就是定义一个大对象, 把变量/方法/类/接口...的都放里面

2.命名空间和模块区别
在程序内部使用的代码, 可以使用命名空间封装和防止全局污染
在程序内部外部使用的代码, 可以使用模块封装和防止全局污染
总结: 由于模块也能实现相同的功能, 所以大部分情况下用模块即可
* */
// namespace Validation {
//     const lettersRegexp = /^[A-Za-z]+$/;
//     export const LettersValidator  = (value) =>{
//         return lettersRegexp.test(value);
//     }
// }

/// <reference path="./56/test.ts" />
console.log(Validation.LettersValidator('abc'));
console.log(Validation.LettersValidator(123));
```

### 声明合并上

```typescript
/*
在ts当中接口和命名空间是可以重名的, ts会将多个同名的合并为一个
* */
// 1.接口
/*
interface TestInterface {
    name:string;
}
interface TestInterface {
    age:number;
}
// interface TestInterface {
//     name:string;
//     age:number;
// }
class Person implements TestInterface{
    name:string;
    age:number;
}
 */

// 1.1同名接口如果属性名相同, 那么属性类型必须一致
/*
interface TestInterface {
    name:string;
}
interface TestInterface {
    name:number;
}
 */
// 1.2同名接口如果出现同名函数, 那么就会成为一个函数的重载
/*
interface TestInterface {
    getValue(value:number):number;
}
interface TestInterface {
    getValue(value:string):number;
}
let obj:TestInterface = {
    getValue(value:any):number{
        if(typeof value === 'string'){
            return value.length;
        }else{
            return value.toFixed();
        }
    }
}
console.log(obj.getValue("abcdef"));
console.log(obj.getValue(3.14));
*/

// 2.命名空间
/*
namespace Validation{
    export let name:string = 'lnj';
}
namespace Validation{
    export let age:number = 18;
}
console.log(Validation.name);
console.log(Validation.age);
*/

// 1.1同名的命名空间中不能出现同名的变量,方法等
/*
namespace Validation{
    export let name:string = 'lnj';
    export let say = ()=> "abc";
}
namespace Validation{
    export let name:string = 'zs';
    export let say = ()=> "abc";
}
 */
// 1.2同名的命名空间中其它命名空间没有通过export导出的内容是获取不到的
namespace Validation{
    let name:string = 'lnj';
}
namespace Validation{
    export let say = ()=> {
        console.log(`name = ${name}`);
    };
}
Validation.say();
```

### 声明合并下

```typescript
// 除了同名的接口和命名空间可以合并以外
// 命名空间还可以和同名的类/函数/枚举合并
// 命名空间和类合并
// 注意点: 类必须定义在命名空间的前面
// 会将命名空间中导出的方法作为一个静态方法合并到类中
/*
class Person {
    say():void{
        console.log('hello world');
    }
}
namespace Person{
    export const hi = ():void=>{
        console.log('hi');
    }
}
console.dir(Person);
 */

// 命名空间和函数合并
// 注意点: 函数必须定义在命名空间的前面
/*
function getCounter() {
    getCounter.count++;
    console.log(getCounter.count);
}
namespace getCounter{
    export let count:number = 0;
}
 */

// 命名空间和枚举合并
// 注意点: 没有先后顺序的要求
/*
enum Gender {
    Male,
    Female
}
namespace Gender{
    export const Yao:number = 666;
}
console.log(Gender);
 */
```

## 装饰器

### 装饰器

```typescript
/*
1.什么是装饰器?
- Decorator 是 ES7 的一个新语法，目前仍处于提案中，
- 装饰器是一种特殊类型的声明，它能够被附加到类，方法， 访问器，属性或参数上
- 被添加到不同地方的装饰器有不同的名称和特点
    + 附加到类上, 类装饰器
    + 附加到方法上,方法装饰器
    + 附加到访问器上,访问器装饰器
    + 附加到属性上,属性装饰器
    + 附加到参数上,参数装饰器

2. 装饰器基本格式
2.1普通装饰器
2.2装饰器工厂
2.3装饰器组合

3.如何在TS中使用装饰器?
在TS中装饰器也是一项实验性的特性, 所以要使用装饰器需要手动打开相关配置
修改配置文件 experimentalDecorators
* */
function test(target) {
    console.log('test');
}
// 如果一个函数返回一个回调函数, 如果这个函数作为装饰器来使用, 那么这个函数就是装饰器工厂
function demo() {
    console.log('demo out');
    return (target)=>{
        console.log('demo in');
    }
}
function abc(target) {
    console.log('abc');
}
function def() {
    console.log('def out');
    return (target)=>{
        console.log('def in');
    }
}

// 1.给Person这个类绑定了一个普通的装饰器
// 这个装饰器的代码会在定义类之前执行, 并且在执行的时候会把这个类传递给装饰器
// @test
// 2.给Person这个类绑定了一个装饰器工厂
// 在绑定的时候由于在函数后面写上了(), 所以会先执行装饰器工厂拿到真正的装饰器
// 真正的装饰器会在定义类之前执行, 所以紧接着又执行了里面
// @demo()
// 3.普通的装饰器可以和装饰器工厂结合起来一起使用
// 结合起来一起使用的时候, 会先从上至下的执行所有的装饰器工厂, 拿到所有真正的装饰器
// 然后再从下至上的执行所有的装饰器

//  demo out / def out / abc / def in / demo in / test
@test
@demo()
@def()
@abc
class Person {

}
```

### 类装饰器

```typescript
/*
1.类装饰器
- 类装饰器在类声明之前绑定（紧靠着类声明）。
- 类装饰器可以用来监视，修改或替换类定义
- 在执行类装饰器函数的时候, 会把绑定的类作为其唯一的参数传递给装饰器
- 如果类装饰器返回一个新的类，它会新的类来替换原有类的定义

2.装饰器和装饰器工厂区别
时候可以传递自定义参数
* */
/*
function test(target:any) {
    // console.log(target);
    target.prototype.personName = 'lnj';
    target.prototype.say = ():void=>{
        console.log(`my name is ${target.prototype.personName}`);
    }
}
*/
function test<T extends {new (...args:any[]):{}}>(target:T) {
    return class extends target {
        name:string = 'lnj';
        age:number = 18;
    }
}
/*
@test
class Person {

}
interface Person{
    say():void;
}
let p = new Person();
p.say();
 */

@test
class Person {

}
let p = new Person();
console.log(p);
```

### defineProperty

```typescript
/*
Object.defineProperty()
可以直接在一个对象上定义一个新属性，
或者修改一个对象的现有属性，并返回此对象。
* */
// 定义一个新的属性
/*
let obj = {age:18};
Object.defineProperty(obj, 'name', {
    value:'lnj'
});
console.log(obj);
 */

// 修改原有属性
/*
let obj = {age:18};
Object.defineProperty(obj, 'age', {
    value:34
});
console.log(obj);
 */

// 修改属性配置-读写
/*
let obj = {age:18};
Object.defineProperty(obj, 'age', {
    writable:false
})
obj.age = 34;
console.log(obj.age);
 */


// 修改属性配置-迭代
/*
let obj = {age:18, name:'lnj'};
Object.defineProperty(obj, 'name', {
    enumerable: false
})
for(let key in obj){
    console.log(key);
}
 */

// 修改属性配置-配置
let obj = {age:18, name:'lnj'};
Object.defineProperty(obj, 'name', {
    enumerable:false,
    configurable: false
});
Object.defineProperty(obj, 'name', {
    enumerable:true,
    configurable: false
});
for(let key in obj){
    console.log(key);
}
```

### 方法装饰器

```typescript
/*
1.方法装饰器
- 方法装饰器写在在一个方法的声明之前（紧靠着方法声明）。
- 方法装饰器可以用来监视，修改或者替换方法定义。

- 方法装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
    + 对于静态方法而言就是当前的类, 对于实力方法而言就是当前的实例
    + 被绑定方法的名字。
    + 被绑定方法的属性描述符。
* */
/*
function test(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    // console.log(target);
    // console.log(propertyKey);
    // console.log(descriptor);
    descriptor.enumerable = false;
}
class Person {
    // @test
    sayName():void{
        console.log('my name is lnj');
    }
    @test
    sayAge():void{
        console.log('my age is 34');
    }
    // @test
    static say():void{
        console.log('say hello world');
    }
}
let p = new Person();
for(let key in p){
    console.log(key);
}
 */
function test(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    descriptor.value = ():void=>{
        console.log('my name is it666');
    };
}
class Person {
    @test
    sayName():void{
        console.log('my name is lnj');
    }
}
let p = new Person();
p.sayName();
```

### 访问器装饰器

```typescript
/*
1.访问器装饰器
- 访问器装饰器声明在一个访问器的声明之前（紧靠着访问器声明）。
访问器装饰器应用于访问器的 属性描述符并且可以用来监视，修改或替换一个访问器的定义

- 访问器装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
    + 对于静态成员来说是类的构造函数，对于实例成员是类的原型对象。
    + 成员的名字。
    + 成员的属性描述符。

- 注意: 
TypeScript不允许同时装饰一个成员的get和set访问器。
取而代之的是，一个成员的所有装饰的必须应用在文档顺序的第一个访问器上
* */
function test(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    // console.log(target);
    // console.log(propertyKey);
    // console.log(descriptor);
    descriptor.set = (value:string)=>{
        target.myName = value;
    }
    descriptor.get = ():string=>{
        return target.myName;
    }
}
class Person {
    private _name:string; // lnj
    constructor(name:string){
        this._name = name;
    }
    @test
    get name():string{
        return this._name;
    }
    set name(value:string){
        this._name = value;
    }
    
}
let p = new Person('lnj');
p.name = 'zs';
console.log(p.name);
console.log(p);
```

### 属性装饰器

```typescript
/*
1.属性装饰器
- 属性装饰器写在一个属性声明之前（紧靠着属性声明）
- 属性装饰器表达式会在运行时当作函数被调用，传入下列2个参数：
    + 对于静态属性来说就是当前的类, 对于实例属性来说就是当前实例
    + 成员的名字。
* */
function test(target:any, proptyName:string) {
    console.log(target);
    console.log(proptyName);
    target[proptyName] = 'lnj';
}
class Person {
    // @test
    static age:number;
    @test
    name?:string;
}
let p = new Person();
console.log(p);
```

### 参数装饰器

```typescript
/*
1.参数装饰器
- 参数装饰器写在一个参数声明之前（紧靠着参数声明）。
- 参数装饰器表达式会在运行时当作函数被调用，传入下列3个参数：
    + 对于静态成员来说是当前的类，对于实例成员是当前实例。
    + 参数所在的方法名称。
    + 参数在参数列表中的索引。
* */
/*
其它
属性装饰器,参数装饰器最常见的应用场景就是配合元数据(reflect-metadata),
在不改变原有结构的同时添加一些额外的信息
但是元数据目前也是在提案中, 也还没有纳入正式的标准
所以对于装饰器而言, 我们只需要了解即可,
因为提案中的所有内容将来都是有可能被修改的
因为提案中的所有内容目前都有兼容性问题
* */
function test(target:any, proptyName:string, index:number) {
    console.log(target);
    console.log(proptyName);
    console.log(index);
}
class Person {
    say(age:number,@test name:string):void{

    }
}
```

## 混入

```typescript
// 1.对象混入
/*
let obj1 = {name:'lnj'};
let obj2 = {age:34};
Object.assign(obj1, obj2);
console.log(obj1);
console.log(obj2);
 */

// 2.类混入
// 需求: 定义两个类, 将两个类的内容混入到一个新的类中
class Dog {
    name:string = 'wc';
    say():void{
        console.log('wang wang');
    }
}
class Cat {
    age:number = 3;
    run():void{
        console.log('run run');
    }
}
// 注意点: 一次只能继承一个类
// class Animal extends Dog, Cat{
//
// }
class Animal implements Dog, Cat{
    name:string;
    age:number;
    say:()=>void;
    run:()=>void;
}
function myMixin(target:any, from:any[]) {
    from.forEach((fromItem)=>{
        Object.getOwnPropertyNames(fromItem.prototype).forEach((name)=>{
            target.prototype[name] = fromItem.prototype[name];
        })
    })
}
myMixin(Animal, [Dog, Cat]);
let a = new Animal();
console.log(a);
a.say();
a.run();
// console.log(a.name);
// console.log(a.age);
```

## 声明文件

###  声明

```typescript
/*
1.什么是声明?
- 在企业开发中我们不可避免的需要引用第三方的 JS 的库,
  但是默认情况下TS是不认识我们引入的这些JS库的
  所以在使用这些JS库的时候, 我们就要告诉TS它是什么, 它怎么用
- 如何告诉TS呢?
  那就是通过声明来告诉
* */
declare const $:(selector:string)=>{
    width():number;
    height():number;
    ajax(url:string, config:{}):void;
};
console.log($);
console.log($('.main').width());
console.log($('.main').height());
```

### 声明文件

```typescript
declare let myName:string;
declare function say(name:string, age:number):void;
// 注意点: 声明中不能出现实现
declare class Person {
    name:string;
    age:number;
    constructor(name:string, age:number);
    say():void;
}

//index.js实现文件
let myName = 'lnj';
function say(name, age) {
    console.log(`name is ${name}, age is ${age}`);
}
class Person {
    name = 'lnj';
    age = 34;
    constructor(name, age){
        this.name = name;
        this.age = age;
    }
    say(){
        console.log(`name is ${this.name}, age is ${this.age}`);
    }
}
```

### 声明安装

```typescript
/*
1.对于常用的第三方库, 其实已经有大神帮我们编写好了对应的声明文件
  所以在企业开发中, 如果我们需要使用一些第三方JS库的时候我们只需要安装别人写好的声明文件即可
2.TS声明文件的规范 @types/xxx
  例如: 想要安装jQuery的声明文件, 那么只需要npm install @types/jquery 即可
* */

```

## 类型的查找

我们这里先给大家介绍另外的一种typescript文件：.d.ts文件

我们之前编写的typescript文件都是.ts文件，这些文件最终会输出.js 文件，也是我们通常编写代码的地方；

还有另外一种文件.d.ts文件，它是用来做类型的声明(declare)。它仅仅用来做类型检测，告知typescript我们有哪些类型；

那么typescript会在哪里查找我们的类型声明呢？

内置类型声明；

外部定义类型声明；

自己定义类型声明

## 外部定义类型声明和自定义声明

![image-20220301200258150](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301200258150.png)

![image-20220301200308595](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301200308595.png)

![image-20220301200424285](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20220301200424285.png)

**coderwhy.d.ts**

```ts
// 声明模块
declare module 'lodash' {
  export function join(arr: any[]): void
}

// 声明变量/函数/类
declare let whyName: string
declare let whyAge: number
declare let whyHeight: number

declare function whyFoo(): void

declare class Person {
  name: string
  age: number
  constructor(name: string, age: number)
}

// 声明文件
declare module '*.jpg'
declare module '*.jpeg'
declare module '*.png'
declare module '*.svg'
declare module '*.gif'

// 声明命名空间
declare namespace $ {
  export function ajax(settings: any): any
}
```

