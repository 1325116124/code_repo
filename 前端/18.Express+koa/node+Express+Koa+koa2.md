# 后端服务器搭建

## node搭建服务器

### 回顾

```js
/*
1.如何通过原生NodeJS搭建Web服务器
2.如何处理Get请求, 如何处理POST请求
3.如何处理不同路径(路由)
4.如何处理(获取)请求参数
* */
// 1.导入http模块
const http = require('http');
const queryString = require('querystring');

// 2.通过http模块创建服务对象
const server = http.createServer();
// 3.通过服务对象监听用户请求
server.on('request', (req, res)=>{
    console.log('接收到请求');
    // 1.获取请求类型
    let method = req.method.toLowerCase();
    // 2.获取请求路径
    let url = req.url;
    let path = url.split('?')[0];
    // 3.获取请求参数
    let params = '';
    if(method === 'get'){
        // 4.处理请求参数
        params = url.split('?')[1];
        params = queryString.parse(params);
        // 5.处理路由
        if(path === '/login'){
            console.log('处理登录请求', params);
        }else if(path === '/register'){
            console.log('处理注册请求', params);
        }
    }else if(method === 'post'){
        // 4.处理请求参数
        req.on('data', (chuck)=>{
            params += chuck;
        });
        req.on('end', ()=>{
            params = queryString.parse(params);
            // 5.处理路由
            if(path === '/login'){
                console.log('post处理登录请求', params);
            }else if(path === '/register'){
                console.log('post处理注册请求', params);
            }
        });
    }
});
// 4.指定监听的端口号
server.listen(3000);
```

### nodemon

```js
/*
1.什么是nodemon？
nodemon 是一个监视服务端应用程序文件改变的包，
一旦nodemon发现我们修改了服务器文件，它就会自动重启服务
这样我们就可以省去 ctrl+c 停止服务-> 启动服务，这个步骤

2.nodemon使用
2.1本地安装
npm i nodemon
npx nodemon app.js

2.2全局安全
npm i nodemon -g
nodemon app.js
* */
```

### node回顾

**返回静态资源**

```js
const path = require('path');
const fs = require('fs');
const mime = require('./mime');
/**
 * 读取静态资源
 * @param req  请求对象
 * @param res  响应对象
 * @param rootPath 静态资源所在的目录
 */
function readFile(req, res, rootPath) {
    // 1.获取静态资源地址
    // http://127.0.0.1:3000/login.html?name=lnj&pwd=123456;
    let fileName = req.url.split('?')[0];
    let filePath = path.join(rootPath, fileName);
    // 2.判断静态资源是否存在
    let isExists = fs.existsSync(filePath);
    if(!isExists){
        return
    }
    // 3.获取静态资源的后缀名
    let fileExt = path.extname(filePath);
    // 4.根据文件的后缀获取对应的类型
    let type = mime[fileExt];
    // 5.对文本类型进行特殊处理
    if(type.startsWith('text')){
        type += '; charset=utf-8;'
    }
    // 5.告诉客户端返回数据的类型
    res.writeHead(200, {
        'Content-Type': type
    });
    // 6.加载静态资源并返回静态资源
    fs.readFile(filePath, (err, content)=>{
        if(err){
            res.end('Server Error');
            return
        }
        res.end(content);
    });
}
module.exports = {
    readFile
}
```

**返回动态网页**

```js
function renderHTML(req, res, fileName) {
    ejs.renderFile(fileName, req.data, function(err, str){
        res.writeHead(200, {
            'Content-Type': 'text/html; charset=utf-8;'
        });
        res.end(str);
    });
}
module.exports = {
    renderHTML
}
```

### 搭建目录项目结构

![image-20210726001135216](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726001135216.png)

```html
<!--
bin目录中的www.js文件是创建的服务器文件
-->
<script>
/*服务端配置文件*/
/*在这个文件中提供一个最简单的服务端服务即可*/
const http = require('http');
const serverHandle = require('../app');
const PORT = 3000;
const server = http.createServer();
server.on('request', serverHandle);
server.listen(PORT);
</script>

<!--
app.js中实现了serverHandle函数，处理真正的请求，其中的initParams初始化请求的url地址，并将数据存储在req中
-->
<script>
/*服务端业务逻辑的核心文件*/
/*处理各种请求*/
const queryString = require('querystring');
const goodsRouterHandle = require('./router/goods');
const userRouterHandle = require('./router/user');

const initParams = (req) =>{
    // 准备 请求方式 / 请求路径 / 请求参数
    // 1.处理请求方式
    req.method = req.method.toLowerCase();
    // 2.处理请求路径
    req.path = req.url.split('?')[0];
    // 3.处理请求参数
    return new Promise((resolve, reject)=>{
        if(req.method === 'get'){
            let params = req.url.split('?')[1];
            req.query = queryString.parse(params);
            resolve();
        }else if(req.method === 'post'){
            let params = '';
            req.on('data', (chunk)=>{
                params += chunk;
            });
            req.on('end', ()=>{
                req.body = queryString.parse(params);
                resolve();
            });
        }
    });
}

const serverHandle = (req, res)=>{
    res.writeHead(200, {
        'Content-Type':'application/json; charset=utf-8;'
    });
    // 1.准备各种请求参数
    initParams(req).then(()=>{
        // 2.处理各种路由
        let goodsData = goodsRouterHandle(req, res);
        if(goodsData){
            res.end(JSON.stringify(goodsData));
            return
        }

        let userData = userRouterHandle(req, res);
        if(userData){
            res.end(JSON.stringify(userData));
            return
        }

        res.writeHead(404, {
            'Content-Type':'text/plain; charset=utf-8;'
        });
        res.end('404 Not Found');
    })
};
module.exports = serverHandle;
</script>

<!--
router文件夹存放处理各个不同部分路由的js文件
goods:包含了对应goods的url的相关操作
user：包含了对应user的url的相关操作
routerConst:包含了所有的url常量
-->
<script>
const GOODS_LIST = '/api/goods/list';
const GOODS_DETAIL = '/api/goods/detail';
const GOODS_EDIT = '/api/goods/edit';
const GOODS_NEW = '/api/goods/new';

const USER_LOGIN = '/api/user/login';
const USER_REGISTER = '/api/user/register';
const USER_INFO = '/api/user/info';

module.exports = {
    GOODS_LIST,
    GOODS_DETAIL,
    GOODS_EDIT,
    GOODS_NEW,
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
}
/*
各种路由地址
操作商品接口
/api/goods/list
/api/goods/detail
/api/goods/edit
/api/goods/new
操作用户接口
/api/user/login
/api/user/register
/api/user/info
... ...
* */
</script>
<script>
const {
    GOODS_LIST,
    GOODS_DETAIL,
    GOODS_EDIT,
    GOODS_NEW,} = require('./routerConst');

const goodsRouterHandle = (req, res)=>{
    if(req.method === 'get' && req.path === GOODS_LIST){
        // 处理商品列表
    }else if(req.method === 'get' && req.path === GOODS_DETAIL){
        // 处理商品详情
    }else if(req.method === 'get' && req.path === GOODS_EDIT){
        // 处理编辑商品
    }else if(req.method === 'post' && req.path === GOODS_NEW){
        // 处理新建商品
    }
}
module.exports = goodsRouterHandle;
</script>
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {
    SuccessModel,
    ErrorModel
} = require('../model/ResultModel');

const userRouterHandle = (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        // return {
        //     code: 200,
        //     msg: '登录成功'
        // }
        return new SuccessModel('登录成功', {name:'lnj', age: 33});
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // 处理注册
        // return {
        //     code: 200,
        //     msg: '注册成功'
        // }
        return new ErrorModel('注册失败',{})
    }else if(req.method === 'get' && req.path === USER_INFO){
        // 处理获取用户信息
        // return {
        //     code: 200,
        //     msg: '获取用户信息成功',
        //     data: {
        //         name:'lnj',
        //         age: 33
        //     }
        // }
        return new SuccessModel('获取用户信息成功', {name:'lnj', age: 33})
    }
}

module.exports = userRouterHandle;
</script>

<!--
model:存储了一些常用对象的定义，比如返回的成功对象和错误对象
-->
<script>
class BaseModel {
    constructor(msg, data){
        this.msg = msg;
        this.data = data;
    }
}
class SuccessModel extends BaseModel{
    constructor(msg, data){
        super(msg, data);
        this.code = 200;
    }
}
class ErrorModel extends BaseModel{
    constructor(msg, data){
        super(msg, data);
        this.code = -1;
    }
}
module.exports = {
    SuccessModel,
    ErrorModel
}
</script>
```

### cross-env的使用

```js
/*服务端业务逻辑的核心文件*/
/*处理各种请求*/
/*
1.不同平台设置环境变量方式
例如：windows下配置NODE_ENV
#首先查看NODE_ENV是否存在
set NODE_ENV
#如果不存在则添加环境变量
set NODE_ENV=production
#某些时候需要删除环境变量
set NODE_ENV=

例如：Linux下配置NODE_ENV
#首先查看NODE_ENV是否存在
echo $NODE_ENV
#如果不存在则添加环境变量
export NODE_ENV=production
#某些时候需要删除环境变量
unset NODE_ENV

2.什么是cross-env?
cross-env是一款运行跨平台的设置和使用环境变量的脚本。

3.为什么要使用cross-env
因为我们在自定义配置环境变量的时候，
由于在不同的环境下，配置的方式也是不同
所以我们需要使用cross-env来统一配置方式

4.cross-env使用
npm install --save-dev cross-env
"scripts": {
  "dev": "cross-env NODE_ENV=dev nodemon ./bin/www.js",
  "build": "cross-env NODE_ENV=pro nodemon ./bin/www.js",
}
npm run dev
* */
```

### 操作mysql数据库封装

![image-20210726004204691](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726004204691.png)

```html
<!-- 
config:存放所有的配置文件，现在存放了数据库的配置文件
-->
<script>
let MYSQL_CONFIG;

if(process.env.NODE_ENV === 'dev'){
    MYSQL_CONFIG = {
        host     : '127.0.0.1',
        port     : '3306',
        user     : 'root',
        password : 'root',
        database : 'demo'
    }
}else if(process.env.NODE_ENV === 'pro'){
    MYSQL_CONFIG = {
        host     : '127.0.0.1',
        port     : '3306',
        user     : 'root',
        password : 'root',
        database : 'demo'
    }
}
module.exports = MYSQL_CONFIG;
</script>

<!--
db：操作数据库的文件夹，其中存储操作数据库的文件，现在存放了mysql的js文件
-->
<script>
// 1.导入mysql驱动
const mysql      = require('08-操作MySQL封装/db/mysql');
const MYSQL_CONFIG = require('../config/db');
// 2.创建连接对象
const connection = mysql.createConnection(MYSQL_CONFIG);
// 3.连接MySQL数据库
connection.connect();
// 4.操作MySQL数据库

const exc = (sql) =>{
    return new Promise((resolve, reject)=>{
        connection.query(sql, function (error, results) {
            if (error){
                reject(error);
            }else{
                resolve(results);
            }
        });
    });
};

module.exports = exc;
</script>
<!--
在路由中导入并编写sql语句进行操作
-->
<!-- user.js -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {
    SuccessModel,
    ErrorModel
} = require('../model/ResultModel');
const exc = require('../db/mysql');

const userRouterHandle = (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        return new SuccessModel('登录成功', {name:'lnj', age: 33});
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        let sql = `insert into user (username, password) values ('lnj', 123456)`;
        exc(sql).then((results)=>{
            console.log(results);
        }).catch((err)=>{
            console.log(err);
        });
        // 处理注册
        return new ErrorModel('注册失败',{})
    }else if(req.method === 'get' && req.path === USER_INFO){
        // 处理获取用户信息
        return new SuccessModel('获取用户信息成功', {name:'lnj', age: 33})
    }
};
module.exports = userRouterHandle;
</script>
```

### 显示登录注册界面

![image-20210726004808900](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726004808900.png)

```html
<!-- 
public:存储所有静态资源的目录
utils:存储所有工具类的目录
-->
<!-- 因为之前的readFile是异步的，所有将他用Promise包起来 -->
<!-- staticServer.js -->
<script>
const path = require('path');
const fs = require('fs');
const mime = require('./mime');
/**
 * 读取静态资源
 * @param req  请求对象
 * @param res  响应对象
 * @param rootPath 静态资源所在的目录
 */
function readFile(req, res, rootPath) {
    // 1.获取静态资源地址
    // http://127.0.0.1:3000/login.html?name=lnj&pwd=123456;
    let fileName = req.url.split('?')[0];
    let filePath = path.join(rootPath, fileName);
    // 2.判断静态资源是否存在
    let isExists = fs.existsSync(filePath);
    if(!isExists){
        return
    }
    // 3.获取静态资源的后缀名
    let fileExt = path.extname(filePath);
    // 4.根据文件的后缀获取对应的类型
    let type = mime[fileExt];
    // 5.对文本类型进行特殊处理
    if(type.startsWith('text')){
        type += '; charset=utf-8;'
    }
    // 5.告诉客户端返回数据的类型
    res.writeHead(200, {
        'Content-Type': type
    });
    // 6.加载静态资源并返回静态资源
    return new Promise((resolve, reject)=>{
        fs.readFile(filePath, (err, content)=>{
            if(err){
                res.end('Server Error');
                reject(err);
            }else{
                res.end(content);
                resolve();
            }
        });
    });
}
module.exports = {
    readFile
}
</script>
<!-- 在app.js中用async和await来接收 -->
<script>
/*服务端业务逻辑的核心文件*/
/*处理各种请求*/
const queryString = require('querystring');
const goodsRouterHandle = require('./router/goods');
const userRouterHandle = require('./router/user');
const staticServer = require('./utils/staticServer');
const path = require('path');
const rootPath = path.join(__dirname, 'public');

// 准备各种请求参数
const initParams = (req) =>{
    // 准备 请求方式 / 请求路径 / 请求参数
    // 1.处理请求方式
    req.method = req.method.toLowerCase();
    // 2.处理请求路径
    req.path = req.url.split('?')[0];
    // 3.处理请求参数
    return new Promise((resolve, reject)=>{
        if(req.method === 'get'){
            let params = req.url.split('?')[1];
            req.query = queryString.parse(params);
            resolve();
        }else if(req.method === 'post'){
            let params = '';
            req.on('data', (chunk)=>{
                params += chunk;
            });
            req.on('end', ()=>{
                req.body = queryString.parse(params);
                resolve();
            });
        }
    });
}

// 处理各种请求
const serverHandle = async (req, res)=>{
    // 1.返回静态网页
    await staticServer.readFile(req, res, rootPath);
    // 2.处理API请求
    res.writeHead(200, {
        'Content-Type':'application/json; charset=utf-8;'
    });
    // 1.准备各种请求参数
    initParams(req).then(()=>{
        // 2.处理各种路由
        // 2.1处理商品相关路由
        let goodsData = goodsRouterHandle(req, res);
        if(goodsData){
            res.end(JSON.stringify(goodsData));
            return
        }
        // 2.2处理用户相关路由
        let userData = userRouterHandle(req, res);
        if(userData){
            res.end(JSON.stringify(userData));
            return
        }
        // 2.3没有匹配到任何路由
        res.writeHead(404, {
            'Content-Type':'text/plain; charset=utf-8;'
        });
        res.end('404 Not Found');
    })
};
module.exports = serverHandle;
</script>
```

### ajv前后端校验数据

```html
<!--
1.什么是JSON Schema?
- JSON Schema定义了JSON格式的规范
- 在企业开发中通常都是多人团队开发, 所以为了提升开发效率
  我们就需要制定各种标准, 而JSON Schema就是专门用于制定JSON的标准的

2.什么是Ajv
- 虽然开发之前我们就制定了标准, 但是无论是前端开发人员还是后端开发人员
  都不能盲目的相信对方, 所以在开发过程中我们还需要对制定的规范进行校验
- 在NodeJS中我们可以通过Ajv来校验前端传递过来的JSON数据是否符合我们制定的JSON Schema规范
https://www.npmjs.com/package/ajv
-->
```

![image-20210726102818119](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726102818119.png)

```html
<!-- validator存放数据格式的校验 -->
<script>
const userSchema = {
    type: "object",
    properties: {
        username: {
            type: "string",
            pattern: '^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\\.[a-zA-Z0-9_-]+)+$',
            maxLength: 255,
            minLength: 3
        },
        password: {
            type: "string",
            pattern: '^[A-Za-z0-9]{6,20}$',
            maxLength: 20,
            minLength: 6
        },
        gender: {
            type: "string",
            pattern: '[1,2,3]',
            maxLength: 1,
            minLength: 1
        }
    },
    required: ["username", "password"]
}
module.exports = userSchema;
</script>
<!-- 安装ajv，再使用ajv中的方法将schema和data传进行做校验 -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {
    SuccessModel,
    ErrorModel
} = require('../model/ResultModel');
const exc = require('../db/mysql');
const Ajv = require('ajv');
const ajv = new Ajv();
const userSchema = require('../validator/userValidator');

const userRouterHandle = (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        return new SuccessModel('登录成功', {name:'lnj', age: 33});
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // let sql = `insert into user (username, password) values ('lnj', 123456)`;
        // exc(sql).then((results)=>{
        //     console.log(results);
        // }).catch((err)=>{
        //     console.log(err);
        // });
        let valid = ajv.validate(userSchema, req.body);
        console.log(valid);
        console.log(ajv.errors);
        // 处理注册
        return new ErrorModel('注册失败',{})
    }else if(req.method === 'get' && req.path === USER_INFO){
        // 处理获取用户信息
        return new SuccessModel('获取用户信息成功', {name:'lnj', age: 33})
    }
};

module.exports = userRouterHandle;
</script>
```

### 后端分层架构

![image-20210726103537261](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726103537261.png)

```html
<!-- router处理界面，controller处理业务逻辑，service处理和数据库相关的操作 -->
<!-- user.js -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {
    SuccessModel,
    ErrorModel
} = require('../model/ResultModel');
const {userValidate, userExists} = require('../controller/userController');

const userRouterHandle = async (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        return new SuccessModel('登录成功', {name:'lnj', age: 33});
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // 1.校验数据是否正确
        let valid = userValidate(req.body);
        // 2.判断当前注册的用户是否存储
        let isExists = await userExists(req.body.username);
        // 3.判断是否可以注册
        if(valid && !isExists){
            console.log('可以注册');
        }else{
            console.log('不可以注册');
        }
        // 处理注册
        return new ErrorModel('注册失败',{})
    }else if(req.method === 'get' && req.path === USER_INFO){
        // 处理获取用户信息
        return new SuccessModel('获取用户信息成功', {name:'lnj', age: 33})
    }
};

module.exports = userRouterHandle;
</script>
<!-- userController.js -->
<script>
const Ajv = require('ajv');
const ajv = new Ajv();
const userSchema = require('../validator/userValidator');
const {getUser} = require('../service/userService');

function userValidate(data) {
    return ajv.validate(userSchema, data);
}

async function userExists(username){
    let users = await getUser(username);
    return users.length !== 0;
}

module.exports = {
    userValidate,
    userExists
}
</script>
<!-- userService.js -->
<script>
const exc = require('../db/mysql');

async function getUser(username) {
    let sql = `select * from user where username = '${username}'`;
    let results = await exc(sql);
    return results;
}
module.exports = {
    getUser
}
</script>
```

### 后端架构分层完善

![image-20210726104454278](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726104454278.png)

```html
<!-- 在config中的errorConst配置了返回错误信息的对象 -->
<script>
module.exports = {
    userDataFail: {
        code: 1001,
        msg: '数据不符合预期'
    },
    userExistsFail: {
        code: 1002,
        msg: '用户已经存在'
    }
};
</script>
<!-- router中的user.js -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {
    SuccessModel,
    ErrorModel
} = require('../model/ResultModel');
const {registerUser} = require('../controller/userController');

const userRouterHandle = async (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        return new SuccessModel('登录成功', {name:'lnj', age: 33});
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        let result = await registerUser(req.body);
        // 处理注册
        return result;
    }else if(req.method === 'get' && req.path === USER_INFO){
        // 处理获取用户信息
        return new SuccessModel('获取用户信息成功', {name:'lnj', age: 33})
    }
};

module.exports = userRouterHandle;
</script>
<!-- controller中的userController.js -->
<script>
const Ajv = require('ajv');
const ajv = new Ajv();
const userSchema = require('../validator/userValidator');
const {getUser} = require('../service/userService');
const {SuccessModel, ErrorModel} = require('../model/ResultModel');
const {userDataFail, userExistsFail} = require('../config/errorConst');

/**
 * 校验用户数据是否正确
 * @param data 被校验的数据
 * @returns {boolean | PromiseLike<any>}
 */
function userValidate(data) {
    return ajv.validate(userSchema, data);
}

/**
 * 检查用户是否存在
 * @param username 被检查的用户名称
 * @returns {Promise<boolean>}
 */
async function userExists(username){
    let users = await getUser(username);
    return users.length !== 0;
}

async function registerUser(data){
// 1.校验数据是否正确
    let valid = userValidate(data);
    if(!valid){
        return new ErrorModel(userDataFail);
    }
    // 2.判断当前注册的用户是否存储
    let isExists = await userExists(data.username);
    // 3.判断是否可以注册
    if(valid && !isExists){
        console.log('可以注册');
    }else{
        return new ErrorModel(userExistsFail);
    }
}

module.exports = {
    userValidate,
    userExists,
    registerUser
}
</script>
<!-- service中的userService.js -->
<script>
const exc = require('../db/mysql');

/**
 * 根据用户名获取用户信息你
 * @param username 被获取的用户名
 * @returns {Promise<*>}
 */
async function getUser(username) {
    let sql = `select * from user where username = '${username}'`;
    let results = await exc(sql);
    return results;
}
module.exports = {
    getUser
}
</script>
```

### 用户注册

```html
<!-- userController.js -->
<script>
const Ajv = require('ajv');
const ajv = new Ajv();
const userSchema = require('../validator/userValidator');
const {getUser, createUser} = require('../service/userService');
const {SuccessModel, ErrorModel} = require('../model/resultModel');
const {userDataFail, userExistsFail, userRegisterFail} = require('../config/errorConst');

/**
 * 校验用户数据是否正确
 * @param data 被校验的数据
 * @returns {boolean | PromiseLike<any>}
 */
function userValidate(data) {
    return ajv.validate(userSchema, data);
}

/**
 * 检查用户是否存在
 * @param username 被检查的用户名称
 * @returns {Promise<boolean>}
 */
async function userExists(username){
    let users = await getUser(username);
    return users.length !== 0;
}

/**
 * 用户注册
 * @param data 用户数据
 * @returns {Promise<ErrorModel|*>}
 */
async function registerUser(data){
// 1.校验数据是否正确
    let valid = userValidate(data);
    if(!valid){
        return new ErrorModel(userDataFail);
    }
    // 2.判断当前注册的用户是否存储
    let isExists = await userExists(data.username);
    // 3.判断是否可以注册
    if(valid && !isExists){
        try {
            await createUser(data);
            return new SuccessModel({msg:'注册成功'});
        }catch (e) {
            return new ErrorModel(userRegisterFail);
        }
    }else{
        return new ErrorModel(userExistsFail);
    }
}

module.exports = {
    registerUser
}
</script>
<!-- userService.js -->
<script>
const exc = require('../db/mysql');

/**
 * 根据用户名获取用户信息你
 * @param username 被获取的用户名
 * @returns {Promise<*>}
 */
async function getUser(username) {
    let sql = `select * from user where username = '${username}'`;
    let results = await exc(sql);
    return results;
}
async function createUser({username, password, gender}){
    let sql = `insert into user (username, password, gender) values('${username}','${password}','${gender}');`;
    let results = await exc(sql);
    return results;
}
module.exports = {
    getUser,
    createUser
}
</script>
```

### 用户注册密码加密

![image-20210726105820892](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726105820892.png)

```html
<!-- 在工具类中创建crypto.js加密方法 -->
<script>
// 1.导入加密模块
const crypto = require('14-用户注册-加密/utils/crypto');
const secret = 'com.it666';
// 2.创建加密方法
function _md5(password) {
    /*
    MD5加密（加密不可逆）
    MD5的全称是Message-Digest Algorithm 5（信息-摘要算法）。 128位长度。
    目前MD5是一种不可逆算法。具有很高的安全性
    什么叫做不可逆?
    不可以通过加密之后的内容还原加密之前的内容, 我们就称之为不可逆
    * */
    // 1.指定加密方式
    const md5 = crypto.createHash('md5')
    // 2.指定需要加密的内容和加密之后输出的格式
    const hash = md5.update(password) // 指定需要加密的内容
                    .digest('hex'); // 指定加密之后输出的格式
    /*
    注意点:
    MD5加密, 只要加密的内容没有发生变化, 那么加密之后的内容就不会发生变化
    所以正式因为如此, 虽然MD5是不可逆的, 但是可以暴力破解
    正式因为如此, 所以仅仅通过MD5加密也不安全
    所以我们在加密之前应该对原始数据进行加盐操作
    什么叫做加盐?
    给原始数据混入一些其它数据
    * */
    // console.log(hash); // e80b5017098950fc58aad83c8c14978e
                       // e80b5017098950fc58aad83c8c14978e
    return hash;
}
function generatePwd(password){
    password = password + secret;
    let hash = _md5(password);
    // console.log(hash); // 4167228cfbe1daa78e88c41bf357618e --> abcdefcom.it666
    return hash;
}
module.exports = generatePwd;
// _md5('abcdef');
// generatePwd('abcdef');
/*
数据库:
source(原始值)   target(加密之后值)
abcdef           e80b5017098950fc58aad83c8c14978e
* */
</script>
<!-- 在controller中的userController.js中使用 -->
<script>
const Ajv = require('ajv');
const ajv = new Ajv();
const userSchema = require('../validator/userValidator');
const {getUser, createUser} = require('../service/userService');
const {SuccessModel, ErrorModel} = require('../model/resultModel');
const {userDataFail, userExistsFail, userRegisterFail} = require('../config/errorConst');
const generatePwd = require('../utils/crypto');

/**
 * 校验用户数据是否正确
 * @param data 被校验的数据
 * @returns {boolean | PromiseLike<any>}
 */
function userValidate(data) {
    return ajv.validate(userSchema, data);
}

/**
 * 检查用户是否存在
 * @param username 被检查的用户名称
 * @returns {Promise<boolean>}
 */
async function userExists(username){
    let users = await getUser(username);
    return users.length !== 0;
}

/**
 * 用户注册
 * @param data 用户数据
 * @returns {Promise<ErrorModel|*>}
 */
async function registerUser({username, password, gender}){
// 1.校验数据是否正确
    let valid = userValidate({username, password, gender});
    if(!valid){
        return new ErrorModel(userDataFail);
    }
    // 2.判断当前注册的用户是否存储
    let isExists = await userExists(username);
    // 3.判断是否可以注册
    if(valid && !isExists){
        try {
            await createUser({username, password: generatePwd(password), gender});
            return new SuccessModel({msg:'注册成功'});
        }catch (e) {
            return new ErrorModel(userRegisterFail);
        }
    }else{
        return new ErrorModel(userExistsFail);
    }
}

module.exports = {
    registerUser
}
</script>
```

### 用户登录

```html
<!-- router中的user.js -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {
    SuccessModel,
    ErrorModel
} = require('../model/resultModel');
const {registerUser, loginCheck} = require('../controller/userController');

const userRouterHandle = async (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        let result = await loginCheck(req.body);
        return result;
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // 注册用户
        let result = await registerUser(req.body);
        // 返回注册结果
        return result;
    }else if(req.method === 'get' && req.path === USER_INFO){
        // 处理获取用户信息
        return new SuccessModel('获取用户信息成功', {name:'lnj', age: 33})
    }
};

module.exports = userRouterHandle;
</script>
<!-- controller中的userController.js -->
<script>
const Ajv = require('ajv');
const ajv = new Ajv();
const userSchema = require('../validator/userValidator');
const {getUser, createUser} = require('../service/userService');
const {SuccessModel, ErrorModel} = require('../model/resultModel');
const {userDataFail, userExistsFail, userRegisterFail, userLoginFail} = require('../config/errorConst');
const generatePwd = require('../utils/crypto');

/**
 * 校验用户数据是否正确
 * @param data 被校验的数据
 * @returns {boolean | PromiseLike<any>}
 */
function userValidate(data) {
    return ajv.validate(userSchema, data);
}

/**
 * 检查用户是否存在
 * @param username 被检查的用户名称
 * @returns {Promise<boolean>}
 */
async function userExists(username){
    let users = await getUser(username);
    return users.length !== 0;
}

/**
 * 用户注册
 * @param data 用户数据
 * @returns {Promise<ErrorModel|*>}
 */
async function registerUser({username, password, gender}){
// 1.校验数据是否正确
    let valid = userValidate({username, password, gender});
    if(!valid){
        return new ErrorModel(userDataFail);
    }
    // 2.判断当前注册的用户是否存储
    let isExists = await userExists(username);
    // 3.判断是否可以注册
    if(valid && !isExists){
        try {
            // 密码加密之后再存储
            await createUser({username, password: generatePwd(password), gender});
            return new SuccessModel({msg:'注册成功'});
        }catch (e) {
            return new ErrorModel(userRegisterFail);
        }
    }else{
        return new ErrorModel(userExistsFail);
    }
}

/**
 * 登录
 * @param username  用户名
 * @param password  密码
 * @returns {Promise<ErrorModel|*|SuccessModel|*>}
 */
async function loginCheck({username, password}){
    // 由于存储的密码是加密的, 所以登录时也要用加密的密码去登录
    let users = await getUser(username, generatePwd(password));
    if(users.length !== 0){
        return new SuccessModel({msg: '登录成功', data: users[0]});
    }else{
        return new ErrorModel(userLoginFail);
    }
}
module.exports = {
    registerUser,
    loginCheck
}
</script>
<!-- service中的userService.js -->
<script>
const exc = require('../db/mysql');

/**
 * 根据用户名获取用户信息你
 * @param username 被获取的用户名
 * @returns {Promise<*>}
 */
async function getUser(username, password) {
    if(password){
        let sql = `select * from user where username = '${username}' and password = '${password}'`;
        let results = await exc(sql);
        return results;
    }else{
        let sql = `select * from user where username = '${username}'`;
        let results = await exc(sql);
        return results;
    }
}
async function createUser({username, password, gender}){
    let sql = `insert into user (username, password, gender) values('${username}','${password}','${gender}');`;
    let results = await exc(sql);
    return results;
}
module.exports = {
    getUser,
    createUser
}
</script>
```

### 客户端保存登录状态

```html
<!--
1.为什么要存储登录状态?
因为在企业开发中有一些操作是只有登录之后才能操作的
例如: 编辑用户信息, 查看用户订单等
所以我们在登录之后需要存储用户的登录状态,
以后在涉及到一些敏感操作的时候,
我们就可以通过这个状态来判断用户是否已经登录
来决定是否让用户进行相关敏感操作

2.如何存储用户登录状态?
2.1客户端存储 Cookie
2.2服务端存储 Session

3.Cookie特点
- 我们可以在客户端中对cookie进行增删改查, 我们也可以在服务端中对cookie进行增删改查
- 每次发送网络请求, 客户端都会自动将当前域名的cookie发送给服务端

服务端通过res.setHeader("Set-Cookie")来对cookie进行设置
-->
<!-- user.js 如果登录成功 -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {registerUser, loginCheck} = require('../controller/userController');
const userRouterHandle = async (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        let result = await loginCheck(req.body);
        // 保存登录状态
        if(result.code === 200){
            res.setHeader('Set-Cookie',`username=${req.body.username}; path=/;`)
        }
        return result;
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // 注册用户
        let result = await registerUser(req.body);
        // 返回注册结果
        return result;
    }else if(req.method === 'get' && req.path === USER_INFO){

    }
};

module.exports = userRouterHandle;
</script>
```

### 客户端保存登录状态-安全问题

```html
<!--
注意点: 由于Cookie既可以在服务端修改, 又可以在客户端修改, 所以存在安全隐患
所以我们在服务端设置Cookie的时候, 可以通过httpOnly来指定只能在服务端修改
不能在客户端修改
注意点: 虽然我们通过服务端保存了登录状态, 但是并没有给登录状态设置过期时间,
所以还是存在安全隐患, 所以我们在保存登录状态的时候, 还需要设置过期时间
注意点: 在客户端保存用户的用户名明文其实也是不安全的, 所以在在保存登录状态的时候
应该加密之后再保存
-->
<!-- router中的user.js -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {registerUser, loginCheck} = require('../controller/userController');
const generatePwd = require('../utils/crypto');

const getCookieExpires = () =>{
    let date = new Date();
    date.setTime(date.getTime() + (24 * 60 * 60 * 1000));
    return date.toGMTString();
}
const userRouterHandle = async (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        let result = await loginCheck(req.body);
        // 保存登录状态
        if(result.code === 200){
            /*
            注意点: 由于Cookie既可以在服务端修改, 又可以在客户端修改, 所以存在安全隐患
                    所以我们在服务端设置Cookie的时候, 可以通过httpOnly来指定只能在服务端修改
                    不能在客户端修改
            注意点: 虽然我们通过服务端保存了登录状态, 但是并没有给登录状态设置过期时间,
                    所以还是存在安全隐患, 所以我们在保存登录状态的时候, 还需要设置过期时间
            注意点: 在客户端保存用户的用户名明文其实也是不安全的, 所以在在保存登录状态的时候
                    应该加密之后再保存
            * */
            res.setHeader('Set-Cookie',`username=${generatePwd(req.body.username)}; path=/; httpOnly; expires=${getCookieExpires()}`);
        }
        return result;
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // 注册用户
        let result = await registerUser(req.body);
        // 返回注册结果
        return result;
    }else if(req.method === 'get' && req.path === USER_INFO){

    }
};

module.exports = userRouterHandle;
</script>
```

### 服务端保存登陆状态上

```html
<!--
在app.js中查看前端请求得cookie中是否带有无关紧要得数据，如果没有，那么我们为他随机分配并设置
-->
<script>
/*服务端业务逻辑的核心文件*/
/*处理各种请求*/
const queryString = require('querystring');
const goodsRouterHandle = require('./router/goods');
const userRouterHandle = require('./router/user');
const staticServer = require('./utils/staticServer');
const path = require('path');
const rootPath = path.join(__dirname, 'public');

const getCookieExpires = () =>{
    let date = new Date();
    date.setTime(date.getTime() + (24 * 60 * 60 * 1000));
    return date.toGMTString();
}
// 准备各种请求参数
const initParams = (req, res) =>{
    // 准备 请求方式 / 请求路径 / 请求参数
    // 1.处理请求方式
    req.method = req.method.toLowerCase();
    // 2.处理请求路径
    req.path = req.url.split('?')[0];
    // 3.处理Cookie
    // console.log(req.headers.cookie); // username=lnj; gender=man
    req.cookie = {};
    if(req.headers.cookie){
        req.headers.cookie.split(';').forEach((item)=>{
            let keyvalue = item.split('=');
            req.cookie[keyvalue[0]] = keyvalue[1];
        });
    }
    // 4.获取用户的唯一标识
    req.userId = req.cookie.userId;
    if(!req.userId){
        req.userId = `${Date.now()}_${Math.random()}_it666`;
        res.setHeader('Set-Cookie',`userId=${req.userId}; path=/; httpOnly; expires=${getCookieExpires()}`);
    }
    // 3.处理请求参数
    return new Promise((resolve, reject)=>{
        if(req.method === 'get'){
            let params = req.url.split('?')[1];
            req.query = queryString.parse(params);
            resolve();
        }else if(req.method === 'post'){
            let params = '';
            req.on('data', (chunk)=>{
                params += chunk;
            });
            req.on('end', ()=>{
                console.log(params);
                req.body = queryString.parse(params);
                resolve();
            });
        }
    });
}
const setEnd = (res, data) =>{
    res.writeHead(200, {
        'Content-Type':'application/json; charset=utf-8;'
    });
    res.end(JSON.stringify(data));
}
// 处理各种请求
const serverHandle = async (req, res)=>{
    // 1.返回静态网页
    await staticServer.readFile(req, res, rootPath);
    // 2.处理API请求
    res.setEnd = setEnd;
    // 1.准备各种请求参数
    initParams(req, res).then( async ()=>{
        // 2.处理各种路由
        // 2.1处理商品相关路由
        let goodsData = goodsRouterHandle(req, res);
        if(goodsData){
            // res.end(JSON.stringify(goodsData));
            res.setEnd(res, goodsData);
            return
        }
        // 2.2处理用户相关路由
        let userData = await userRouterHandle(req, res);
        if(userData){
            // res.end(JSON.stringify(userData));
            res.setEnd(res, userData);
            return
        }
        // 2.3没有匹配到任何路由
        res.writeHead(404, {
            'Content-Type':'text/plain; charset=utf-8;'
        });
        res.end('404 Not Found');
    })
};
module.exports = serverHandle;
</script>
```

### 服务端保存登录状态下

```html
<!--
在app.js中定义一个全局的容器，在给用户分配得唯一的用户id时，将它存储在容器中，并为他分配一个空间存储用户相关的信息，并挂载到req中
-->
<!-- app.js -->
<script>
/*服务端业务逻辑的核心文件*/
/*处理各种请求*/
const queryString = require('querystring');
const goodsRouterHandle = require('./router/goods');
const userRouterHandle = require('./router/user');
const staticServer = require('./utils/staticServer');
const path = require('path');
const rootPath = path.join(__dirname, 'public');
// 定义变量作为session的容器
const SESSION_CONTAINER = {};
/**
 * 生成Cookie过期时间
 * @returns {*}
 */
const getCookieExpires = () =>{
    let date = new Date();
    date.setTime(date.getTime() + (24 * 60 * 60 * 1000));
    return date.toGMTString();
}
// 准备各种请求参数
const initParams = (req, res) =>{
    // 准备 请求方式 / 请求路径 / 请求参数
    // 1.处理请求方式
    req.method = req.method.toLowerCase();
    // 2.处理请求路径
    req.path = req.url.split('?')[0];
    // 5.处理请求参数
    return new Promise((resolve, reject)=>{
        if(req.method === 'get'){
            let params = req.url.split('?')[1];
            req.query = queryString.parse(params);
            resolve();
        }else if(req.method === 'post'){
            let params = '';
            req.on('data', (chunk)=>{
                params += chunk;
            });
            req.on('end', ()=>{
                console.log(params);
                req.body = queryString.parse(params);
                resolve();
            });
        }
    });
}
const initCookieSession = (req, res) =>{
    // 1.处理Cookie
    req.cookie = {};
    if(req.headers.cookie){
        req.headers.cookie.split(';').forEach((item)=>{
            let keyValue = item.split('=');
            req.cookie[keyValue[0]] = keyValue[1];
        });
    }
    // 2.获取用户的唯一标识
    req.userId = req.cookie.userId;
    if(!req.userId){
        req.userId = `${Date.now()}_${Math.random()}_it666`;
        // 给当前用户分配容器
        SESSION_CONTAINER[req.userId] = {};
        res.setHeader('Set-Cookie',`userId=${req.userId}; path=/; httpOnly; expires=${getCookieExpires()}`);
    }
    if(!SESSION_CONTAINER[req.userId]){
        // 给当前用户分配容器
        SESSION_CONTAINER[req.userId] = {};
    }
    req.session = SESSION_CONTAINER[req.userId];
    console.log(req.session);
}
/**
 * 封装返回数据方法
 * @param res  响应对象
 * @param data 返回的数据
 */
const setEnd = (res, data) =>{
    res.writeHead(200, {
        'Content-Type':'application/json; charset=utf-8;'
    });
    res.end(JSON.stringify(data));
}
// 处理各种请求
const serverHandle = async (req, res)=>{
    // 0.准备cookie和session
    initCookieSession(req, res);
    // 1.返回静态网页
    await staticServer.readFile(req, res, rootPath);
    // 2.处理API请求
    res.setEnd = setEnd;
    // 1.准备各种请求参数
    initParams(req, res).then( async ()=>{
        // 2.处理各种路由
        // 2.1处理商品相关路由
        let goodsData = goodsRouterHandle(req, res);
        if(goodsData){
            res.setEnd(res, goodsData);
            return
        }
        // 2.2处理用户相关路由
        let userData = await userRouterHandle(req, res);
        if(userData){
            res.setEnd(res, userData);
            return
        }
        // 2.3没有匹配到任何路由
        res.writeHead(404, {
            'Content-Type':'text/plain; charset=utf-8;'
        });
        res.end('404 Not Found');
    });
};
module.exports = serverHandle;</script>

<!-- router中的user.js中操作session对象 -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {registerUser, loginCheck} = require('../controller/userController');

const userRouterHandle = async (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        let result = await loginCheck(req.body);
        if(result.code === 200){
            req.session.username = result.data.username;
            req.session.password = result.data.password;
            req.session.gender = result.data.gender;
        }
        return result;
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // 注册用户
        let result = await registerUser(req.body);
        // 返回注册结果
        return result;
    }else if(req.method === 'get' && req.path === USER_INFO){

    }
};

module.exports = userRouterHandle;
</script>
```

### 全局变量存储登录状态的问题

```js
/*
1.当前服务端存储存在的问题
- 操作系统会给每一个应用程序分配一块存储空间,
  在32位操作系统上,这块空间的大小是1.6G左右
  在64位操作系统上,这块空间的大小是3G左右
    + 当前的Session是一个全局变量, 全局变量使用的就是当前应用程序分配到的存储空间
      所以当前的这种服务端存储登录状态的方式也会出'现存不下'的情况
    + 当前的Session是一个全局变量, 全局变量的特点是应用程序启动时分配存储空间,
      应用程序关闭时释放存储空间, 所以全局变量中存储的数据会随着服务端应用程序的重启而消失
      而在企业开发中, 我们每次对项目进行更新都需要重启, 运维人员日常运维也可能会经常重启
      这样就会导致频繁的要求用户登录, 带来'不好的用户体验'
- 现在的服务器性能都比较优越, 内存比较大, CPU也是多核多任务的,
  所以如果一台机器上如果只运行一个NodeJS进程会对资源造成极大的浪费
  所以在企业开发中我们会在一台机器上跑多个NodeJS进程,来提升效率和使用率
  但是每个进程之间的内存是相互隔离的,所以就会导致在登录状态'无法共享'

2.如何解决当前Session的问题?
- 要想解决当前Session的问题, 首先我们要知道Session有哪些特点
    + 数据量不会太大, 存储的都是一些常用信息
    + 访问频率极高, 对性能要求极高, 每次操作都会验证Session
    + 不害怕丢失, 丢失之后再次登录即可
- 如何满足如上特点的同时解决存在的问题?
    + Redis可以搭建集群突破内存限制
    + 只要Redis不重启数据就不会消失
    + 存储在Redis中的数据, 无论哪个NodeJS进程都可以访问
    + Redis性能极好, 速度极快
*/
```

### Redis工具类的封装

![image-20210726145417534](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726145417534.png)

```html
<!-- redis的配置服务在config中的db.js中定义 -->
<script>
let MYSQL_CONFIG;
let REDIS_CONFIG;

if(process.env.NODE_ENV === 'dev'){
    MYSQL_CONFIG = {
        host     : '127.0.0.1',
        port     : '3306',
        user     : 'root',
        password : 'root',
        database : 'demo'
    }
    REDIS_CONFIG ={
        host     : '127.0.0.1',
        port     : '6379',
    }
}else if(process.env.NODE_ENV === 'pro'){
    MYSQL_CONFIG = {
        host     : '127.0.0.1',
        port     : '3306',
        user     : 'root',
        password : 'root',
        database : 'demo'
    }
    REDIS_CONFIG ={
        host     : '127.0.0.1',
        port     : '6379',
    }
}
module.exports = {
    MYSQL_CONFIG,
    REDIS_CONFIG
};
</script>
<!-- db文件夹中的redis.js -->
<script>
// 1.导入Redis模块
const redis = require("21-Redis工具类封装/db/redis");
const {REDIS_CONFIG} = require('../config/db');

// 2.建立Redis连接
const client = redis.createClient(REDIS_CONFIG.port, REDIS_CONFIG.host);
client.on("error", function(error) {
    console.error(error);
});

// 3.封装保存数据和获取数据的方法
function redisSet(key, value){
    if(typeof value === 'object'){
        value = JSON.stringify(value);
    }
    client.set(key, value, redis.print);
}
function redisGet(key){
    return new Promise((resolve, reject)=>{
        client.get("key", (err, value)=>{
            if(err){
                reject(err);
            }
            try {
                resolve(JSON.parse(value));
            }catch (e) {
                resolve(value);
            }
        });
    });
}

module.exports = {
    redisSet,
    redisGet
}

</script>
```

### Redis存储登录状态

```html
<!-- redis存储登录状态user.js -->
<script>
const {
    USER_LOGIN,
    USER_REGISTER,
    USER_INFO
} = require('./routerConst');
const {registerUser, loginCheck} = require('../controller/userController');
const {redisSet} = require('../db/redis');

const userRouterHandle = async (req, res)=>{
    if(req.method === 'post' && req.path === USER_LOGIN){
        // 处理登录
        let result = await loginCheck(req.body);
        // 存储登录状态
        if(result.code === 200){
            req.session.username = result.data.username;
            req.session.password = result.data.password;
            req.session.gender = result.data.gender;
            // 同步到Redis中
            redisSet(req.userId, req.session);
        }
        return result;
    }else if(req.method === 'post' && req.path === USER_REGISTER){
        // 注册用户
        let result = await registerUser(req.body);
        // 返回注册结果
        return result;
    }else if(req.method === 'get' && req.path === USER_INFO){

    }
};

module.exports = userRouterHandle;
</script>
<!-- app.js中打印redis中存储的数据 -->
<script>
/*服务端业务逻辑的核心文件*/
/*处理各种请求*/
const queryString = require('querystring');
const goodsRouterHandle = require('./router/goods');
const userRouterHandle = require('./router/user');
const staticServer = require('./utils/staticServer');
const path = require('path');
const rootPath = path.join(__dirname, 'public');
const {redisGet} = require('./db/redis');

// 定义变量作为session的容器
// const SESSION_CONTAINER = {};
/**
 * 生成Cookie过期时间
 * @returns {*}
 */
const getCookieExpires = () =>{
    let date = new Date();
    date.setTime(date.getTime() + (24 * 60 * 60 * 1000));
    return date.toGMTString();
}
/**
 * 准备各种请求参数
 * @param req 请求对象
 * @param res 响应对象
 * @returns {Promise<any>}
 */
const initParams = (req, res) =>{
    // 准备 请求方式 / 请求路径 / 请求参数
    // 1.处理请求方式
    req.method = req.method.toLowerCase();
    // 2.处理请求路径
    req.path = req.url.split('?')[0];
    // 5.处理请求参数
    return new Promise((resolve, reject)=>{
        if(req.method === 'get'){
            let params = req.url.split('?')[1];
            req.query = queryString.parse(params);
            resolve();
        }else if(req.method === 'post'){
            let params = '';
            req.on('data', (chunk)=>{
                params += chunk;
            });
            req.on('end', ()=>{
                console.log(params);
                req.body = queryString.parse(params);
                resolve();
            });
        }
    });
}
/**
 * 初始化Cookie和Session
 * @param req 请求对象
 * @param res 响应对象
 */
const initCookieSession =async (req, res) =>{
    // 1.处理Cookie
    req.cookie = {};
    if(req.headers.cookie){
        req.headers.cookie.split(';').forEach((item)=>{
            let keyValue = item.split('=');
            req.cookie[keyValue[0]] = keyValue[1];
        });
    }
    // 2.获取用户的唯一标识
    req.userId = req.cookie.userId;
    if(!req.userId){
        req.userId = `${Date.now()}_${Math.random()}_it666`;
        // 给当前用户分配容器
        // SESSION_CONTAINER[req.userId] = {};
        req.session = {};
        res.setHeader('Set-Cookie',`userId=${req.userId}; path=/; httpOnly; expires=${getCookieExpires()}`);
    }
    // if(!SESSION_CONTAINER[req.userId]){
    //     // 给当前用户分配容器
    //     SESSION_CONTAINER[req.userId] = {};
    // }
    if(!req.session){
        req.session = await redisGet(req.userId) || {};
    }
    console.log(req.session);
    // req.session = SESSION_CONTAINER[req.userId];
}
/**
 * 封装返回数据方法
 * @param res  响应对象
 * @param data 返回的数据
 */
const setEnd = (res, data) =>{
    res.writeHead(200, {
        'Content-Type':'application/json; charset=utf-8;'
    });
    res.end(JSON.stringify(data));
}
// 处理各种请求
const serverHandle = async (req, res)=>{
    // 0.准备cookie和session
    await initCookieSession(req, res);
    // 1.返回静态网页
    await staticServer.readFile(req, res, rootPath);
    // 2.处理API请求
    res.setEnd = setEnd;
    // 1.准备各种请求参数
    initParams(req, res).then( async ()=>{
        // 2.处理各种路由
        // 2.1处理商品相关路由
        let goodsData = goodsRouterHandle(req, res);
        if(goodsData){
            res.setEnd(res, goodsData);
            return
        }
        // 2.2处理用户相关路由
        let userData = await userRouterHandle(req, res);
        if(userData){
            res.setEnd(res, userData);
            return
        }
        // 2.3没有匹配到任何路由
        res.writeHead(404, {
            'Content-Type':'text/plain; charset=utf-8;'
        });
        res.end('404 Not Found');
    });
};
module.exports = serverHandle;
</script>
```

### 安全问题-sql注入

```html
<!--
1.什么是SQL注入?
SQL注入是一种古老的攻击方式，
SQL注入是通过利用一些查询语句的漏洞,
让我们的应用程序执行不正确的SQL语句的一种攻击方式

2.如何房子SQL注入?
执行SQL语句之前过滤掉特殊字符
-->
<!-- 通过mysql中的escape方法将用户输入的数据字符进行转义 -->
<!-- mysql.js -->
<script>
// 1.导入mysql驱动
const mysql      = require('23-安全问题-SQL注入/db/mysql');
const {MYSQL_CONFIG} = require('../config/db');
// 2.创建连接对象
const connection = mysql.createConnection(MYSQL_CONFIG);
// 3.连接MySQL数据库
connection.connect();
// 4.操作MySQL数据库

const exc = (sql) =>{
    return new Promise((resolve, reject)=>{
        connection.query(sql, function (error, results) {
            if (error){
                reject(error);
            }else{
                resolve(results);
            }
        });
    });
};

module.exports = {
    exc,
    escape: mysql.escape
};
</script>

<!-- service中的userService.js -->
<script>
const {exc, escape} = require('../db/mysql');

/**
 * 根据用户名获取用户信息你
 * @param username 被获取的用户名
 * @returns {Promise<*>}
 */
async function getUser(username, password) {
    username = escape(username);
    password = escape(password);
    if(password){
        let sql = `select * from user where username = ${username} and password = ${password}`;
        console.log(sql);
        let results = await exc(sql);
        return results;
    }else{
        let sql = `select * from user where username = '${username}'`;
        let results = await exc(sql);
        return results;
    }
}
async function createUser({username, password, gender}){
    let sql = `insert into user (username, password, gender) values('${username}','${password}','${gender}');`;
    let results = await exc(sql);
    return results;
}
module.exports = {
    getUser,
    createUser
}
</script>
```

### 用sequelize替换mysql

![image-20210726152527340](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726152527340.png)

```html
<!-- 更改config中db.js中的配置 -->
<script>
let MYSQL_CONFIG;
let REDIS_CONFIG;

if(process.env.NODE_ENV === 'dev'){
    // MYSQL_CONFIG = {
    //     host     : '127.0.0.1',
    //     port     : '3306',
    //     user     : 'root',
    //     password : 'root',
    //     database : 'demo'
    // }
    MYSQL_CONFIG = {
        databaseName: 'demo',
        databaseUserName: 'root',
        databasePassword: 'root',
        conf:{
            host: '127.0.0.1', // MySQL服务器地址
            port: 3306, // MySQL服务器端口号
            dialect: 'mysql', // 告诉Sequelize当前要操作的数据库类型
            pool: {
                max: 5, // 最多有多少个连接
                min: 0, // 最少有多少个连接
                idle: 10000, // 当前连接多久没有操作就断开
                acquire: 30000, // 多久没有获取到连接就断开
            }
        }
    }
    REDIS_CONFIG ={
        host     : '127.0.0.1',
        port     : '6379',
    }
}else if(process.env.NODE_ENV === 'pro'){
    // MYSQL_CONFIG = {
    //     host     : '127.0.0.1',
    //     port     : '3306',
    //     user     : 'root',
    //     password : 'root',
    //     database : 'demo'
    // }
    MYSQL_CONFIG = {
        databaseName: 'demo',
        databaseUserName: 'root',
        databasePassword: 'root',
        conf:{
            host: '127.0.0.1', // MySQL服务器地址
            port: 3306, // MySQL服务器端口号
            dialect: 'mysql', // 告诉Sequelize当前要操作的数据库类型
            pool: {
                max: 5, // 最多有多少个连接
                min: 0, // 最少有多少个连接
                idle: 10000, // 当前连接多久没有操作就断开
                acquire: 30000, // 多久没有获取到连接就断开
            }
        }
    }
    REDIS_CONFIG ={
        host     : '127.0.0.1',
        port     : '6379',
    }
}
module.exports = {
    MYSQL_CONFIG,
    REDIS_CONFIG
};
</script>
<!-- 在db文件下创建seq.js文件 -->
<script>
// 1.导入Sequelize
const Sequelize = require('sequelize');
const {MYSQL_CONFIG} = require('../config/db');

// 2.配置连接信息
/*
第一个参数: 要操作的数据库名称
第二个参数: 数据库用户名
第三个参数: 数据库密码
第四个参数: 其它的配置信息
* */
const seq = new Sequelize(
    MYSQL_CONFIG.databaseName,
    MYSQL_CONFIG.databaseUserName,
    MYSQL_CONFIG.databasePassword,
    MYSQL_CONFIG.conf);

module.exports = seq;
</script>
<!-- 在db文件夹下创建model文件夹用来存储定义表的结构文件 -->
<script>
const Sequelize = require('sequelize');
const seq = require('../seq');

/*
第一个参数: 用于指定表的名称
第二个参数: 用于指定表中有哪些字段
第三个参数: 用于配置表的一些额外信息
* */
/*
注意点:
1.sequelize在根据模型创建表的时候, 会自动将我们指定的表的名称变成复数
2.sequelize在根据模型创建表的时候, 会自动增加两个字段 createAt/updateAt
* */
let User = seq.define('user', {
    id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true
    },
    username: {
        type: Sequelize.STRING, // varchar(255)
        allowNull: false,
        unique: true
    },
    password: {
        type: Sequelize.STRING, // varchar(255)
        allowNull: false,
        unique: false
    },
    gender: {
        type: Sequelize.ENUM(['男', '女', '妖']),
        defaultValue: '妖'
    }
}, {
    freezeTableName: true, // 告诉sequelize不需要自动将表名变成复数
    timestamps: true // 不需要自动创建createAt/updateAt这两个字段
});

module.exports = User;
</script>
<!-- 在db文件夹下创建sync.js文件 -->
<script>
const seq = require('./seq');

// 0.导入模型
require('./model/user');

// 1.测试配置是否正确
seq.authenticate()
    .then(()=>{
        console.log('ok');
    })
    .catch((err)=>{
        console.log(err);
    });

// 2.执行同步方法, 创建表
seq.sync().then(()=>{
    console.log('sync ok');
    process.exit();
});
</script>
```

### sequelize完善注册登录

```html
<!-- 重新用sequelize的语法重写service中的userService中的方法 -->
<script>
// const {exc, escape} = require('../db/mysql');
const User = require('../db/model/user');

/**
 * 根据用户名获取用户信息你
 * @param username 被获取的用户名
 * @returns {Promise<*>}
 */
async function getUser(username, password) {
    if(password){
        let results = await User.findAll({
            where:{
                username:username,
                password:password
            }
        });
        return results;
    }else{
        let results = await User.findAll({
            where:{
                username:username
            }
        });
        return results;
    }
}
async function createUser({username, password, gender}){
    let results = await User.create({
        username:username,
        password:password,
        gender:gender
    });
    return results['dataValues'];
}
module.exports = {
    getUser,
    createUser
}
</script>
```

### 日志记录

```html
<!--使用node的fs模块创建文件日志并进行写入记录,在utils中创建log.js文件 -->
<script>
const fs = require('fs');
const path = require('path');

function createWriteStream() {
    const fullPath = createDirPath();
    const fullFileName = path.join(fullPath, 'access.log');
    const writeStream = fs.createWriteStream(fullFileName);
    return writeStream;
}
function createDirPath() {
    const date = new Date();
    const dirName = `${date.getFullYear()}_${date.getMonth() + 1}_${date.getDay()}`;
    const fullPath = path.join(__dirname, '../log', dirName);
    // console.log(fullPath);
    if(!fs.existsSync(fullPath)){
        fs.mkdirSync(fullPath);
    }
    return fullPath;
}
const writeStream = createWriteStream();
function writeLog(log) {
    writeStream.write(log + '\n');
}
module.exports = writeLog;
</script>
<!-- 使用log.js -->
<script>
writeLog(`${req.method}--${req.url}--${req.headers['user-agent']}`)	
</script>
```

### 日志分析

```html
<!-- 对统计的日志信息做细致的分析 -->
<!-- 需要用到readline中间件 -->
<script>
const fs = require('fs');
const path = require('path');
const readline = require('readline');

function createReadStream() {
    const fullPath = createDirPath();
    const fullFileName = path.join(fullPath, 'access.log');
    const readStream = fs.createReadStream(fullFileName);
    return readStream;
}
function createDirPath() {
    const date = new Date();
    const dirName = `${date.getFullYear()}_${date.getMonth() + 1}_${date.getDay()}`;
    const fullPath = path.join(__dirname, '../log', dirName);
    // console.log(fullPath);
    if(!fs.existsSync(fullPath)){
        fs.mkdirSync(fullPath);
    }
    return fullPath;
}
const readStream = createReadStream();
const readObject =  readline.createInterface({
    input: readStream
});
let totalCount = 0;
let chromeCount = 0;
readObject.on('line', (lineData)=>{
    if(!lineData){
        return
    }
    totalCount++;
    if(lineData.indexOf('Chrome') >= 0){
        chromeCount++;
    }
});
readObject.on('close', ()=>{
    console.log(chromeCount / totalCount);
});
</script>
```

## Express搭建服务器

### 开篇

```html
<!--
1.什么是Express?
Express是一个基于NodeJS的Web Server开发框架, 能够帮助我们快速的搭建Web服务器

2.为什么需要Express?
2.1利用原生的NodeJS开发Web服务器,
我们需要处理很多繁琐且没有技术含量的内容
例如: 获取路由->解析路由->分配路由->处理路由等
但是有了Express之后, 就能帮助我们省去大量繁琐的环节, 让我们只用关注核心业务逻辑
2.2利用原生的NodeJS开发Web服务器,
我们需要自己手动去实现静态/动态资源处理, get/post参数的解析, cookie的解析, 日志处理等
但是有了Express之后, 已经有现成的插件帮我们实现了上述功能
2.3所以作为单身的程序猿(媛), 如果你还想留一些时间去约会, 那么Express是你的最佳选择

3.永不过时的Express
3.1Express最早的版本是在2010年发布的, 目前最新的版本是5.0是在2015年左右发布的
3.2虽然Express是一个古老的框架, 但是它并没有过时
    + 因为: 公司老项目仍然在使用
    + 因为: 目前比较火的KOA就是Express原班人马打造的(几乎有这相同的API)
    + 因为: 目前比较火的EggJS就是KOA打造的
    + 所以: 学会Express能够帮助你很好的维护公司的老项目
    + 所以: 学会Express能够帮助你更快的学习KOA和EggJS
* */
/*
1.如何使用Express?
1.1手动安装手动配置
1.2利用Express脚手架工具安装使用(Express-generator)

2.手动安装手动配置
https://www.npmjs.com/package/express
-->
```

### 基本使用

```js
/*
1.如何使用Express?
1.1手动安装手动配置
1.2利用Express脚手架工具安装使用(Express-generator)

2.手动安装手动配置
https://www.npmjs.com/package/express
* */
// 1.导入express
const express = require('express');
// 2.调用express方法, 创建服务端实例对象
const app = express()

app.get('/', (req, res, next)=>{
    res.writeHead(200, {
        'Content-Type': 'text/plain; charset=utf-8;'
    });
    res.end('www.it666.com');
});

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
```

### 处理静态网页

```html
<!-- 
直接调用express静态static方法，将存放静态资源的public目录的路径传给它即可 
app.use(express.static(url))
-->
<script>
// 1.导入express
const express = require('express');
const path = require('path');

// 2.调用express方法, 创建服务端实例对象
const app = express();

// 处理静态资源
app.use(express.static(path.join(__dirname, 'public')));

app.get('/', (req, res, next)=>{
    res.writeHead(200, {
        'Content-Type': 'text/plain; charset=utf-8;'
    });
    res.end('www.it666.com');
});

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
</script>
```

### 处理动态网页

```html
<!-- 
使用ejs模板引擎 
通过app.set告诉express相关的配置
通过res中的render方法来渲染模板
-->
<script>
// 1.导入express
const express = require('express');
const path = require('path');

// 2.调用express方法, 创建服务端实例对象
const app = express();

// 处理静态资源
app.use(express.static(path.join(__dirname, 'public')));

// 处理动态资源
// 1.告诉express动态资源存储在什么地方
app.set('views', path.join(__dirname, 'views'));
// 2.告诉express动态网页使用的是什么模板引擎
app.set('view engine', 'ejs');
// 3.监听请求, 返回渲染之后的动态网页
app.get('/', (req, res, next)=>{
    // 注意点: express给请求对象和影响对象添加了很多自定义的方法
    res.render('index', {msg:'www.it666.com'});
});

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
</script>
```

### 处理路由上

```html
<!-- 
处理路由的第一种方式是通过app.get/app.post来处理
-->
<script>
// 1.导入express
const express = require('express');
const path = require('path');

// 2.调用express方法, 创建服务端实例对象
const app = express();

// 处理静态资源
app.use(express.static(path.join(__dirname, 'public')));

// 处理动态资源
// 1.告诉express动态资源存储在什么地方
app.set('views', path.join(__dirname, 'views'));
// 2.告诉express动态网页使用的是什么模板引擎
app.set('view engine', 'ejs');
// 3.监听请求, 返回渲染之后的动态网页
app.get('/', (req, res, next)=>{
    // 注意点: express给请求对象和影响对象添加了很多自定义的方法
    res.render('index', {msg:'www.it666.com'});
});

// 通过express处理路由方式一
app.get('/api/goods/list', (req, res, next)=>{
    res.end('it666.list.get');
});
app.get('/api/user/login', (req, res, next)=>{
    // 注意点: 响应对象的json方法是express给响应对象扩展的
    //         这个方法会自动将对象转换成字符串之后返回
    //         这个方法还会自动帮助我们设置响应头
    res.json({
        name:'lnj',
        age:33,
        method: 'get'
    });
});
app.post('/api/goods/detail', (req, res, next)=>{
    res.end('it666.detail.post');
});
app.post('/api/user/register', (req, res, next)=>{
    res.json({
        name:'lnj',
        age:33,
        method: 'post'
    });
});

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
</script>
```

### 处理路由下

```html
<!-- 
方式二是分模块的方式，创建router文件目录，在里面定义user.js负责处理user的路由
-->
<script>
const express = require('express');
const router = express.Router();
// 会将注册的地址和当前的地址拼接在一起来匹配
// /api/user/login
router.get('/login', (req, res, next)=>{
    // 注意点: 响应对象的json方法是express给响应对象扩展的
    //         这个方法会自动将对象转换成字符串之后返回
    //         这个方法还会自动帮助我们设置响应头
    res.json({
        name:'lnj',
        age:33,
        method: 'get'
    });
});
router.post('/register', (req, res, next)=>{
    res.json({
        name:'lnj',
        age:33,
        method: 'post'
    });
});
module.exports = router;
</script>
<!-- 在app.js中使用 -->
<script>
// 1.导入express
const express = require('express');
const path = require('path');
const userRouter = require('./router/user');

// 2.调用express方法, 创建服务端实例对象
const app = express();

// 处理静态资源
app.use(express.static(path.join(__dirname, 'public')));

// 处理动态资源
// 1.告诉express动态资源存储在什么地方
app.set('views', path.join(__dirname, 'views'));
// 2.告诉express动态网页使用的是什么模板引擎
app.set('view engine', 'ejs');
// 3.监听请求, 返回渲染之后的动态网页
app.get('/', (req, res, next)=>{
    // 注意点: express给请求对象和影响对象添加了很多自定义的方法
    res.render('index', {msg:'www.it666.com'});
});

// 通过express处理路由方式二
app.use('/api/user', userRouter);

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
</script>
```

### 处理路由参数

```html
<!--
总结：1.get请求的参数直接通过req.query即可
2.post请求的参数需要设置一些东西才能从req.body中获取参数
-->
<script>
app.get('/get', (req, res, next)=>{
    // express会将get的请求参数转换成对象之后, 放到请求对象的query属性中
    console.log(req.query);
});
app.use(express.json()); // 告诉express能够解析 application/json类型的请求参数
app.use(express.urlencoded({extended: false})); // 告诉express能够解析 表单类型的请求参数 application/x-www-form-urlencoded
// express会将解析之后, 转换成对象的post请求参数放到请求对象的body属性中
app.post('/post', (req, res, next)=>{
    console.log(req.body);
});
</script>
```

### 处理cookie

```html
<!--
设置cookie通过res.cookie来设置
获取cookie通过req.cookies来获取，但是需要通过一个中间件cookie-parser
-->
<script>
const cookieParser = require('cookie-parser')
// 处理cookie
app.get('/setCookie', (req, res, next)=>{
    res.cookie('username', 'lnj', {httpOnly: true, path: '/', maxAge: 10000});
    res.end();
});
app.use(cookieParser());
app.get('/getCookie', (req, res, next)=>{
    console.log(req.cookies);
});
</script>
```

### next方法

```js
// 1.导入express
const express = require('express');
const path = require('path');
const userRouter = require('./router/user');
const cookieParser = require('cookie-parser')

// 2.调用express方法, 创建服务端实例对象
const app = express();

// express-next方法
/*
1.use既可以处理没有路由地址的请求, 也可以处理有路由地址请求
2.use既可以处理get请求, 也可以处理post请求
3.在处理请求的时候是从上至下的判断的, 哪一个先满足就哪一个来处理
4.如果在处理请求的回调函数中没有调用next方法, 那么处理完之后就不会继续往下判断了
5.如果在处理请求的回调函数中调用了next方法,那么处理完之后还会继续往下判断
* */
app.use((req, res, next)=>{
    console.log('use1 没有路由地址');
    next();
});
app.use('/',(req, res, next)=>{
    console.log('use2 有路由地址');
    next();
});
app.get('/api',(req, res, next)=>{
    console.log('get1 /api');
    next();
});
app.get('/api/user',(req, res, next)=>{
    console.log('get2 /api/user');
    next();
});
app.post('/api',(req, res, next)=>{
    console.log('post1 /api');
    next();
});
app.post('/api/user',(req, res, next)=>{
    console.log('post2 /api/user');
    next();
});

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
```

### next方法的正确使用方式

```js
// 1.导入express
const express = require('express');
const path = require('path');
const userRouter = require('./router/user');
const cookieParser = require('cookie-parser');
const createError = require('http-errors');

// 2.调用express方法, 创建服务端实例对象
const app = express();
/*
默认情况下会从上至下的匹配路由处理方法, 一旦匹配到了就会执行,
执行完毕之后如果没有调用next就停止,
执行完毕之后如果调用了next就继续向下匹配
* */
// next方法正确打开姿势
/*
通过next方法, 我们可以将同一个请求的多个业务逻辑拆分到不同的方法中处理
这样可以提升代码的可读性和可维护性, 以及保证代码的单一性
* */
app.get('/api/user/info',
    (req, res, next)=>{
        console.log('验证用户是否登陆');
        next();
    },
    (req, res, next)=>{
        console.log('用户已经登陆, 可以查看用户信息');
    });

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
```

### 错误处理

```html
<!-- 
利用app.use可以对没有被匹配的路由进行错误处理
-->
<script>
// 1.导入express
const express = require('express');
const path = require('path');
const userRouter = require('./router/user');
const cookieParser = require('cookie-parser');
const createError = require('http-errors');

// 2.调用express方法, 创建服务端实例对象
const app = express();

// express错误处理
app.get('/api/user/login', (req, res, next)=>{
    res.end('login');
});
app.get('/api/user/register', (req, res, next)=>{
    res.end('register');
});
/*
由于在处理请求的时候会从上至下的匹配
由于前面的处理方法都没有调用next方法, 所以处理完了就不会再继续向下匹配了
由于use没有指定路由地址, 由于use既可以处理get请求, 又可以处理post请求
所以只要前面的路由都没有匹配到, 就会执行下面的use
* */
app.use((req, res, next)=>{
    next(createError(404));
});
app.use((err, req, res, next)=>{
    console.log(err.status, err.message);
    res.end(err.message);
});

// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
</script>
```

### 中间件

```js
// 1.导入express
const express = require('express');
const path = require('path');
const userRouter = require('./router/user');
const cookieParser = require('cookie-parser');
const createError = require('http-errors');

// 2.调用express方法, 创建服务端实例对象
const app = express();

app.get('/api/user/info',
    (req, res, next)=>{
        console.log(' 验证用户是否登陆');
        next();
    },
    (req, res, next)=>{
        console.log('用户已经登陆, 可以查看用户信息');
        res.json({name:'lnj', age:66});
    });

app.get('/api/user/info',(req, res, next)=>{
        console.log('验证用户是否登陆');
        next();
    });
app.get('/api/user/info',(req, res, next)=>{
        console.log('用户已经登陆, 可以查看用户信息');
        res.json({name:'lnj', age:66});
    });
/*
1.什么是中间件?
- 中间件的本质就是一个函数, 这个函数接收3个参数request请求对象、response响应对象、next函数
- 当请求进来，会从第一个中间件开始进行匹配。如果匹配则进入，如果不匹配，则向后依次对比匹配

2.中间件的作用?
- 将一个请求的处理过程，分发到多个环节中，目的效率高，便于维护。即每个环节专门干一件事

3.中间件的分类
3.1应用级别中间件
绑定到app实例上的中间件
例如: app.get / app.post

3.2路由级别中间件
绑定到router实例上的中间件
例如: router.get / router.post

3.3错误处理中间件
与其他中间件函数的定义基本相同，
不同之处在于错误处理函数多了一个变量：err，即它有4个变量：err, req, res, next

3.4内置中间件
express.static()、express.json()、express.urlencoded()、...

3.5第三方中间件
cookie-parser、...
 */
// 3.告诉服务端需要监听哪一个端口
app.listen(3000, ()=>{
    console.log('listen ok');
});
```

### 脚手架工具的使用

![image-20210726202400094](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726202400094.png)

```html
<!-- app.js -->
<script>
// 导入了一些第三方的模块
var createError = require('http-errors');
var express = require('express');
var path = require('path');
var cookieParser = require('cookie-parser');
var logger = require('morgan');

// 导入了处理路由的模块
var indexRouter = require('./routes/index');
var usersRouter = require('./routes/users');

// 创建了服务端实例对象
var app = express();

// 处理动态网页
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(logger('dev'));
// 处理post请求参数
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
// 解析cookie
app.use(cookieParser());

// 处理静态网页
app.use(express.static(path.join(__dirname, 'public')));

// 注册处理路由模块
app.use('/', indexRouter);
app.use('/users', usersRouter);

// 处理错误
// catch 404 and forward to error handler
app.use(function(req, res, next) {
  next(createError(404));
});
// error handler
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
</script>
```

### 实现注册登录

![image-20210726203551646](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210726203551646.png)

```js
//只需要将路由换成express的router即可，在app.js中引入，并创建表
const express = require('express');
const router = express.Router();
const {registerUser, loginCheck} = require('../controller/userController');

router.post('/login',async (req, res, next)=>{
    // 处理登录
    let result = await loginCheck(req.body);
    // 存储登录状态
    // if(result.code === 200){
    //     req.session.username = result.data.username;
    //     req.session.password = result.data.password;
    //     req.session.gender = result.data.gender;
    //     // 同步到Redis中
    //     redisSet(req.userId, req.session);
    // }
    return res.json(result);
});
router.post('/register',async (req, res, next)=>{
    // 注册用户
    let result = await registerUser(req.body);
    // 返回注册结果
    return res.json(result);
});

module.exports = router;
```

### express存储登录状态

```html
<!--
1.在原生的NodeJS中如何存储登录状态
1.1通过全局变量存储
1.2通过Redis存储

2.在原生的NodeJS中如何通过全局变量存储
2.1首先要生成一个无关紧要的userId
2.2然后将这个无关紧要的userId通过cookie存储在客户端中
2.2然后将这个userId作为key, 将登录后的数据作为值, 存储到全局变量中

然后就可以通过req.sesstion来操作了

3.在express中如何通过全局变量存储
使用express-session这个库, 就能够自动帮我们完事原生NodeJS中我们实现的所有代码
-->
 <!-- app.js -->
<script>
// 导入了一些第三方的模块
const createError = require('http-errors');
const express = require('express');
const path = require('path');
const cookieParser = require('cookie-parser');
const logger = require('morgan');
const session = require('express-session');
require('./db/sync');
// 导入了处理路由的模块
const usersRouter = require('./routes/user');

// 创建了服务端实例对象
const app = express();

// 处理动态网页
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(logger('dev'));
// 处理post请求参数
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
// 解析cookie
app.use(cookieParser());
// 注册保存登录状态的中间件
app.use(session({
  name: 'userId',
  secret: 'COM.it666.*?', // 用于生成无关紧要的userId的
  cookie: { path:'/', httpOnly:true, maxAge: 24 * 60 * 60 * 1000 }
}));

/*
1. name - cookie的名字（原属性名为 key）。（默认：’connect.sid’）
2. store - session存储实例
3. secret - 用它来对session cookie签名，防止篡改
4. cookie - session cookie设置 （默认：{ path: ‘/‘, httpOnly: true,secure: false, maxAge: null }）
5. genid - 生成新session ID的函数 （默认使用uid2库）
6. rolling - 在每次请求时强行设置cookie，这将重置cookie过期时间（默认：false）
7. resave - 强制保存session即使它并没有变化 （默认： true, 建议设为：false）
8. proxy - 当设置了secure cookies（通过”x-forwarded-proto” header ）时信任反向代理。当设定为true时，
”x-forwarded-proto” header 将被使用。当设定为false时，所有headers将被忽略。当该属性没有被设定时，将使用Express的trust proxy。
9. saveUninitialized - 强制将未初始化的session存储。当新建了一个session且未设定属性或值时，它就处于未初始化状态。在设定一个cookie前，这对于登陆验证，减轻服务端存储压力，权限控制是有帮助的。（默认：true）
10. unset - 控制req.session是否取消（例如通过 delete，或者将它的值设置为null）。这可以使session保持存储状态但忽略修改或删除的请求（默认：keep）
* */
// 处理静态网页
app.use(express.static(path.join(__dirname, 'public')));

// 注册处理路由模块
app.use('/api/user', usersRouter);

// 处理错误
app.use(function(req, res, next) {
  next(createError(404));
});
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;

</script>
```

### express通过redis存储登录状态

```html
<!--
通过connect-redis中间件，前提使需要express和express-session中间件
-->
<!-- 在db中redis.js把redis的连接对象暴露出来 -->
<script>
// 1.导入Redis模块
const redis = require("redis");
const {REDIS_CONFIG} = require('../config/db');

// 2.建立Redis连接
const client = redis.createClient(REDIS_CONFIG.port, REDIS_CONFIG.host);
client.on("error", function(error) {
    console.error(error);
});

module.exports = client;
</script>
<!-- 
在app.js中的app.use(session({

}))
中使用
-->
<script>
// 导入了一些第三方的模块
const createError = require('http-errors');
const express = require('express');
const path = require('path');
const cookieParser = require('cookie-parser');
const logger = require('morgan');
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redisClient = require('./db/redis');

require('./db/sync');
// 导入了处理路由的模块
const usersRouter = require('./routes/user');

// 创建了服务端实例对象
const app = express();

// 处理动态网页
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');

app.use(logger('dev'));
// 处理post请求参数
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
// 解析cookie
app.use(cookieParser());
app.use(session({
  name: 'userId',
  secret: 'COM.it6666.*?', // 用于生成无关紧要的userId的密钥
  cookie: { path:'/', httpOnly: true, maxAge: 24 * 60 * 60 * 1000 },
  store: new RedisStore({ client: redisClient })
}));

// 处理静态网页
app.use(express.static(path.join(__dirname, 'public')));

// 注册处理路由模块
app.use('/api/user', usersRouter);

// 处理错误
app.use(function(req, res, next) {
  next(createError(404));
});
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
</script>
```

### 日志记录

```html
<!--
通过使用morgan中间件来记录日志
创建一个写入流并进行配置即可将日志记录在文件中
-->
<script>
// 导入了一些第三方的模块
const createError = require('http-errors');
const express = require('express');
const cookieParser = require('cookie-parser');
const logger = require('morgan');
const path = require('path');
const fs = require('fs');
const session = require('express-session');
const RedisStore = require('connect-redis')(session);
const redisClient = require('./db/redis');

require('./db/sync'); // 初始化数据库表
// 导入了处理路由的模块
const usersRouter = require('./routes/user');

// 创建了服务端实例对象
const app = express();

// 处理动态网页
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');
/*
在express中我们可以通过morgan来记录日志
我们只需要安装morgan, 导入morgan, 注册morgan的中间件即可
在注册morgan中间件的时候需要指定日志的模式, 不同的模式记录的内容也不同
默认情况下morgan会将日志输出到控制台中, 当然我们也可以通过配置让它把日志写入到文件中
* */
// create a write stream (in append mode)
const accessLogStream = fs.createWriteStream(path.join(__dirname, 'log/access.log'), { flags: 'a' });
app.use(logger('combined', {
  stream: accessLogStream
}));
// 处理post请求参数
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
// 解析cookie
app.use(cookieParser());
// 保存登录状态
app.use(session({
  name: 'userId',
  secret: 'COM.it6666.*?', // 用于生成无关紧要的userId的密钥
  cookie: { path:'/', httpOnly: true, maxAge: 24 * 60 * 60 * 1000 },
  store: new RedisStore({ client: redisClient })
}));

// 处理静态网页
app.use(express.static(path.join(__dirname, 'public')));

// 注册处理路由模块
app.use('/api/user', usersRouter);

// 处理错误
app.use(function(req, res, next) {
  next(createError(404));
});
app.use(function(err, req, res, next) {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get('env') === 'development' ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render('error');
});

module.exports = app;
</script>
```

## Koa2搭建服务器

### Koa2开篇

```js
/*
1.什么是Express?
Express是第一代最流行的web框架，它对Node.js的http进行了封装
但是它是'基于ES5'的语法，内部实现异步代码，只有一个方法：'回调'

2.什么是KOA1.x?
随着新版Node.js开始支持ES6，Express的团队又'基于ES6'重新编写了下一代web框架koa。
和Express相比，koa 1.x'使用generator实现异步'.
用generator实现异步比回调简单了不少，但是generator的本意并不是异步
https://www.it666.com/my/course/69  从36课时开始

3.什么是KOA2.x?
koa团队并没有止步于koa 1.x，他们非常超前地'基于ES7'开发了koa2，
和koa 1相比，koa2完全'使用Promise并配合async来实现异步'

4.Express、Koa1.x、Koa2.x区别
4.1最大的区别就是内部实现异步的方式不同
- Express使用回调函数实现异步, 容易出现回调地狱问题, 但是语法更老兼容性更好
- Koa1.x使用generator实现异步, 解决了回调地域问题, 但是generator的本意并不是异步
- Koa2.x使用Promise并配合async来实现异步, 解决了回调地域问题, 但是语法太新兼容性不好
4.2第二大的区别就是重量级不同
- Express中内置了很多封装好的功能, 而Koa中将这些功能都封装到了独立的模块中
  想要使用这些功能必须先安装对应的模块才能使用, 所以Koa比Express更轻量级
* */
```

###  koa2的基本使用

```js
/*
1.如何使用Koa?
1.1手动安装手动配置
1.2利用Koa脚手架工具安装使用(Koa-generator)

2.手动安装手动配置
https://www.npmjs.com/package/koa
* */
// 1.导入Koa
const Koa = require('koa');
// 2.创建服务端实例对象
const app = new Koa();

// response
app.use(ctx => {
    ctx.body = 'Hello Koa';
});

// 3.指定监听的端口
app.listen(3000);
```

### koa2处理静态资源(koa-static)

```js
/*
1.Koa如何处理静态资源?
Koa是一个轻量级的框架, 它将所有的附加功能都封装到了独立的模块中
所以想要使用这些功能, 都必须先安装才能使用

https://www.npmjs.com/package/koa-static
* */
// 1.导入Koa
const Koa = require("koa");
const serve = require("koa-static"); // 导入处理静态资源的模块
// 2.创建服务端实例对象
const app = new Koa();

app.use(serve("public")); // 注册处理静态资源的中间件
// response
app.use((ctx) => {
  ctx.body = "Hello Koa";
});

// 3.指定监听的端口
app.listen(3000);
```

### Koa2处理动态网页（koa-views）

```js
/*
1.Koa如何处理动态资源?
Koa是一个轻量级的框架, 它将所有的附加功能都封装到了独立的模块中
所以想要使用这些功能, 都必须先安装才能使用

https://www.npmjs.com/package/koa-views
* */
// 1.导入Koa
const Koa = require("koa");
const serve = require("koa-static"); // 导入处理静态资源的模块
const views = require("koa-views"); // 导入处理动态资源的模块

// 2.创建服务端实例对象
const app = new Koa();

app.use(serve("public")); // 注册处理静态资源的中间件
app.use(views("views", { extension: "ejs" }));

// response
// koa中的ctx就是express中的req,res的结合体
app.use(async (ctx, next) => {
  // ctx.body = 'Hello Koa';
  await ctx.render("index", { msg: "com.it666.www" });
});

// 3.指定监听的端口
app.listen(3000);
```

### Koa2处理路由（koa-router）

```js
/*
1.Koa如何处理路由?
Koa是一个轻量级的框架, 它将所有的附加功能都封装到了独立的模块中
所以想要使用这些功能, 都必须先安装才能使用

https://www.npmjs.com/package/koa-router
https://github.com/ZijianHe/koa-router
* */
// 1.导入Koa
const Koa = require('koa');
const serve = require('koa-static'); // 导入处理静态资源的模块
const views = require('koa-views'); // 导入处理动态资源的模块
const Router = require('koa-router'); // 导入处理路由的模块
const router = new Router(); // 创建路由对象

// 2.创建服务端实例对象
const app = new Koa();

app.use(serve('public')); // 注册处理静态资源的中间件
app.use(views('views', {extension: 'ejs'})); // 注册处理动态资源的中间件

// koa中的ctx就是express中的req,res的结合体
// app.use( async (ctx, next) => {
//     // ctx.body = 'Hello Koa';
//     await ctx.render('index', {msg: 'com.it666.www'});
// });

// 处理路由
router.get('/api/goods/list', (ctx, next)=>{
    ctx.body = 'get /api/goods/list'; // ctx.body === res.writeHeader + res.end
});
router.get('/api/user/login', (ctx, next)=>{
    ctx.body = {
        method: 'get',
        name: 'lnj',
        age: 66
    }
});
router.post('/api/goods/detail', (ctx, next)=>{
    ctx.body = 'post /api/goods/detail';
});
router.post('/api/user/register', (ctx, next)=>{
    ctx.body = {
        method: 'post',
        name: 'it666',
        age: 33
    }
});

app
    .use(router.routes()) // 启动路由功能
    .use(router.allowedMethods()); // 自动设置响应头

// 3.指定监听的端口
app.listen(3000);
```

### Koa2处理get请求的参数

- 传统get：request.query
- 动态路由：ctx.params

```js
/*
1.Koa如何处理Get请求参数?
- 处理传统get参数
- 处理动态路由形式get参数
* */
// 1.导入Koa
const Koa = require('koa');
const serve = require('koa-static'); // 导入处理静态资源的模块
const views = require('koa-views'); // 导入处理动态资源的模块
const Router = require('koa-router'); // 导入处理路由的模块
const router = new Router(); // 创建路由对象

// 2.创建服务端实例对5象
const app = new Koa();

router.get('/user', (ctx, next)=>{
    let request = ctx.request;
    console.log(request.query); // 获取转换成对象之后的get请求参数
    console.log(request.querystring); // 获取字符串形式的get请求参数
});
router.get('/login/:name/:age', (ctx, next)=>{
    console.log(ctx.params);
});
app.use(serve('public')); // 注册处理静态资源的中间件
app.use(views('views', {extension: 'ejs'})); // 注册处理动态资源的中间件

// 处理路由

app
    .use(router.routes()) // 启动路由功能
    .use(router.allowedMethods()); // 自动设置响应头

// 3.指定监听的端口
app.listen(3000);
```

### Koa2处理post请求参数

- 安装koa-bodyparser中间件
- request.body

```js
/*
1.Koa如何处理Post请求参数?
- 借助koa-bodyparser中间件
- koa-bodyparser中间件会将post请求参数转换成对象之后放到请求对象的body中
* */
// 1.导入Koa
const Koa = require('koa');
const serve = require('koa-static'); // 导入处理静态资源的模块
const views = require('koa-views'); // 导入处理动态资源的模块
const Router = require('koa-router'); // 导入处理路由的模块
const router = new Router(); // 创建路由对象
const bodyParser = require('koa-bodyparser'); // 导入处理post请求参数的模块

// 2.创建服务端实例对象
const app = new Koa();

app.use(serve('public')); // 注册处理静态资源的中间件
app.use(views('views', {extension: 'ejs'})); // 注册处理动态资源的中间件

app.use(bodyParser()); // 注册处理post请求参数的中间件
// 处理路由
router.post('/user', (ctx, next)=>{
    let request = ctx.request;
    console.log(request.body);
});

app
    .use(router.routes()) // 启动路由功能
    .use(router.allowedMethods()); // 自动设置响应头

// 3.指定监听的端口
app.listen(3000);
```

### Koa2处理cookie

- ctx.cookies.set
- ctx.cookies.get

```js
/*
1.Koa如何处理cookie?
- Koa中处理cookie不需要引入其他模块, 只要拿到ctx对象就可以操作cookie

https://demopark.github.io/koa-docs-Zh-CN/
https://demopark.github.io/koa-docs-Zh-CN/api/context.html
* */
// 1.导入Koa
const Koa = require('koa');
const serve = require('koa-static'); // 导入处理静态资源的模块
const views = require('koa-views'); // 导入处理动态资源的模块
const Router = require('koa-router'); // 导入处理路由的模块
const router = new Router(); // 创建路由对象
const bodyParser = require('koa-bodyparser'); // 导入处理post请求参数的模块

// 2.创建服务端实例对象
const app = new Koa();

app.use(serve('public')); // 注册处理静态资源的中间件
app.use(views('views', {extension: 'ejs'})); // 注册处理动态资源的中间件
app.use(bodyParser()); // 注册处理post请求参数的中间件

// 处理路由
router.get('/setCookie', (ctx, next)=>{
    // ctx.cookies.set('name', 'lnj', {
    //     path: '/',
    //     httpOnly: true,
    //     maxAge: 24 * 60 * 60 * 1000
    // });
    // 注意点: 在koa中不能给cookie设置中文的值
    let value = new Buffer('李南江').toString('base64');
    ctx.cookies.set('userName', value, {
        path: '/',
        httpOnly: true,
        maxAge: 24 * 60 * 60 * 1000
    });
    // let value = new Buffer('李南江').toString('base64');
    // console.log(value); // 5p2O5Y2X5rGf
    // let res = new Buffer('5p2O5Y2X5rGf', 'base64').toString();
    // console.log(res);
});
router.get('/getCookie', (ctx, next)=>{
    // console.log(ctx.cookies.get('name'));
    let value = ctx.cookies.get('userName');
    let res = new Buffer(value, 'base64').toString();
    console.log(res);
});
app
    .use(router.routes()) // 启动路由功能
    .use(router.allowedMethods()); // 自动设置响应头

// 3.指定监听的端口
app.listen(3000);
```

### Koa2处理错误（koa-onerror）

```js
/*
1.Koa如何处理错误?
- 使用koa-onerror模块
https://www.npmjs.com/package/koa-onerror
* */
// 1.导入Koa
const Koa = require('koa');
const serve = require('koa-static'); // 导入处理静态资源的模块
const views = require('koa-views'); // 导入处理动态资源的模块
const Router = require('koa-router'); // 导入处理路由的模块
const router = new Router(); // 创建路由对象
const bodyParser = require('koa-bodyparser'); // 导入处理post请求参数的模块
const onerror = require('koa-onerror'); // 导入处理错误的模块

// 2.创建服务端实例对象
const app = new Koa();
onerror(app); // 告诉koa-onerror我们需要捕获所有服务端实例对象的错误

app.use(serve('public')); // 注册处理静态资源的中间件
app.use(views('views', {extension: 'ejs'})); // 注册处理动态资源的中间件
app.use(bodyParser()); // 注册处理post请求参数的中间件

// 处理路由
router.get('/api/user/login', (ctx, next)=>{
    ctx.body = '我是登录';
});
router.get('/api/user/register', (ctx, next)=>{
    ctx.body = '我是注册';
});
app
    .use(router.routes()) // 启动路由功能
    .use(router.allowedMethods()); // 自动设置响应头

// 处理错误
app.use((err, ctx) => {
    console.log(err.status, err.message);
    ctx.body = err.message;
});

// 3.指定监听的端口
app.listen(3000);
```

### Koa2脚手架（koa-generator）

- **创建命令：koa2 demo**

### cookie跨域问题

```js
/*
1.Cookie的跨域问题
Cookie是不能跨域使用的, 所以在前后端分离开发的时候, 我们当前的代码就会出现问题
例如:
前端地址: http://192.168.0.107:3001/login.html
后端地址: http://127.0.0.1:3000/api/user/test

2.什么是跨域?
https://www.it666.com:3000
http://www.it666.com:3000

协议/一级域名/二级域名/端口号 有一个不同就是跨域
由于3000端口和3001端口不同, 所以就是跨域
所以我们在3000端口设置的cookie3001是不能使用的
    我们在3001端口设置的cookie3000也是不能使用的

3.如何解决前后端分离Cookie的跨域问题?
通过Nginx反向代理
http://nginx.org/en/download.html
* */
/*
1.正向代理
顺着请求的方向进行的代理, 我们称之为正向代理
例如: 访问谷歌
我是一个用户，我访问不了谷歌, 但是我可以访问一台海外的服务器, 这台海外服务器又可以访问谷歌
那么'我'就可以先访问'海外的服务器', 再通过'海外的服务器'访问谷歌, 这就是正向代理

2.正向代理特点
- 在正向代理中, 代理服务器是为用户服务的,
  (对于谷歌来说, 它并不知道真正访问自己的是谁, 只知道有一台服务器访问了自己)

3.正向代理的用途
- 访问原来无法访问的资源，如google
- 对客户端访问授权，上网进行认证
- ... ...
* */
/*
1.反向代理
反向代理和正向代理正好相反, 反向代理服务器是为'服务器'服务的
 (对于用户来说, 它并不知道自己真正访问的是谁, 只知道自己访问了一台服务器)

2.反向代理的用途
- 负载均衡，通过反向代理服务器来优化网站的负载
- 前后端分离, 统一请求地址
* */
```

### nginx解决跨域

```js
/*
1.Nginx安装和使用
1.1安装
下载解压即可
http://nginx.org/en/download.html
1.2使用
修改配置文件
worker_processes 4; // CPU核数
location / {  // 请求根路径代理的地址
	proxy_pass http://192.168.0.107:3001;
}
location /api { // 请求/api代理的地址
	proxy_pass http://127.0.0.1:3000;
	proxy_set_header Host $host;
}
* */
```

## PM2上线node程序

### PM2的基本使用

```js
const http = require('http');

/*
1.如何上线Node编写的项目?
上线项目需要考虑的几个问题
1.1 服务稳定性, 不会因为程序的某个错误或异常导致项目停止服务
1.2 线上日志记录, 除了记录访问日志以外, 我们还需要记录错误日志和自定义日志
1.3 充分利用服务器资源, Node是单线程的, 服务器是多核的, 一台服务器只运行一个Node程序太浪费资源

2.如何解决上述问题?
通过PM2
2.1 PM2的进程守护可以在程序崩溃后自动重启
2.2 PM2自带日志记录的功能, 可以很方便的记录错误日志和自定义日志
2.3 PM2能够启动多个Node进程, 充分利用服务器资源

3.PM2使用
npm install pm2 -g
pm2 --version
pm2 start app.js
* */
/*
1.常用命令
pm2 start app.js|config     // 启动应用程序
pm2 list                    // 列出启动的所有的应用程序
pm2 restart appName|appId   // 重启应用程序
pm2 info appName|appId      // 查看应用程序详细信息
pm2 log appName|appId       // 显示指定应用程序的日志
pm2 monit appName|appId     // 监控应用程序
pm2 stop appName|appId      // 停止应用程序
pm2 delete appName|appId    // 关闭并删除所有应用
* */
// 2.通过http模块创建服务对象
const server = http.createServer();
// 3.通过服务对象监听用户请求
server.on('request', (req, res)=>{
  console.log('接收到请求');
  // 1.获取请求类型
  let method = req.method.toLowerCase();
  // 2.获取请求路径
  let url = req.url;
  let path = url.split('?')[0];
  // 3.处理请求
  if(method === 'get'){
    // 3.处理路由
    // if(path === '/login'){
    // }
  }
  res.writeHead(200, {
    'Content-Type': 'text/plain; charset=utf-8;'
  });
  res.end('it666.com');
});
// 4.指定监听的端口号
server.listen(3000);
```

### PM2的常用指令

```js
/*
1.如何上线Node编写的项目?
上线项目需要考虑的几个问题
1.1 服务稳定性, 不会因为程序的某个错误或异常导致项目停止服务
1.2 线上日志记录, 除了记录访问日志以外, 我们还需要记录错误日志和自定义日志
1.3 充分利用服务器资源, Node是单线程的, 服务器是多核的, 一台服务器只运行一个Node程序太浪费资源

2.如何解决上述问题?
通过PM2
2.1 PM2的进程守护可以在程序崩溃后自动重启
2.2 PM2自带日志记录的功能, 可以很方便的记录错误日志和自定义日志
2.3 PM2能够启动多个Node进程, 充分利用服务器资源

3.PM2使用
npm install pm2 -g
pm2 --version
pm2 start app.js
* */
/*
1.常用命令
pm2 start app.js|config     // 启动应用程序
pm2 list                    // 列出启动的所有的应用程序
pm2 restart appName|appId   // 重启应用程序
pm2 info appName|appId      // 查看应用程序详细信息
pm2 log appName|appId       // 显示指定应用程序的日志
pm2 monit appName|appId     // 监控应用程序
pm2 stop appName|appId      // 停止应用程序
pm2 delete appName|appId    // 关闭并删除所有应用
* */
```

### PM2解决了进程守护的问题

- PM2在程序发生错误的时候会自动重启程序

### PM2的常用配置

```js
/*
1.PM2常用配置
pm2.conf.json
{
  "apps":{ 
    "name":"应用程序名称", 
    "script": "入口文件名称",
    "watch": true, //发现文件更新是否需要自动重启
    "ignore_watch": [ //指定一些文件发生变化时不需要自动重启
      "node_modules",
      "logs"
    ],
    "error_file": "logs/错误日志文件名称", 
    "out_file": "logs/自定义日志文件名称",
    "log_date_format": "YYYY-MM-DD HH:mm:ss"
  }
}
pm2 start pm2.conf.json
* */
```

### PM2负载均衡

```js
/*
1.PM2多进程
在配置文件中增加 instances 配置, 想启动几个Node进程就写几
注意点: 不能超过服务器CPU的核数
* */
/*
1.PM2常用配置
pm2.conf.json
{
  "apps":{ 
    "name":"应用程序名称", 
    "script": "入口文件名称",
    "watch": true, //发现文件更新是否需要自动重启
    "ignore_watch": [ //指定一些文件发生变化时不需要自动重启
      "node_modules",
      "logs"
    ],
    "error_file": "logs/错误日志文件名称", 
    "out_file": "logs/自定义日志文件名称",
    "log_date_format": "YYYY-MM-DD HH:mm:ss",
    "instance":4 //启动的进程数量
  }
}
pm2 start pm2.conf.json
* */
```

## Egg.js搭建服务器

### Egg.js开篇

```js
/*
1.什么是Egg.js?
- Express是基于ES5的web开发框架
- Koa1.x是Express原班人马打造的基于ES6的web开发框架
- Koa2.x是Express原班人马打造的基于ES7的web开发框架
- Egg是'阿里爸爸'基于Koa的'有约束和规范'的'企业级web开发框架'
- 三个框架之间的关系其实是一部'编程界的进化论'

2. Egg.js发展史
2013 年蚂蚁的 chair 框架，可以视为 egg 的前身。
2015 年 11 月，在苏千的召集下，阿里各 BU(Business Unit) 的前端骨干齐聚黄龙，闭门共建。
2016 年中，广泛使用在绝大部分阿里的前端 Node.js 应用。
2016 年 09 月，在 JSConf China 2016 上亮相并宣布开源。
2017 年初，官网文档 egg - 为企业级框架和应用而生 亮相，并将在本月发布 egg@1.0 版本。

3.Egg.js在阿里的应用
- 阿里旗下蚂蚁金服，天猫，UCWeb，村淘，神马搜索等项目的基础框架都是在 egg 之上扩展的
上图
上图

4.为什么是Egg, 而不是Express或Koa?
4.1  Express 和 Koa没有约束和规范, 会导致团队的沟通成本和项目的维护成本变高
     EggJS有约束和规范, 会大大降低团队的沟通成本和项目的维护成本
4.2  阿里内部大量企业级项目使用egg开发, 实践出真知
4.3  Node社区5位国人核心贡献者4人在阿里, 技术有保障
4.4  阿里前端安全专家，负责 egg-security 等类库, 安全有保障
     ... ...

5.什么是有约束和规范?
和ESLint检查JS代码一样, 有一套标准, 必须严格遵守这套标准 ,否则就会报错
https://eggjs.org/zh-cn/basics/structure.html

6.什么是MVC?
M(Model)     :处理应用程序'数据逻辑'的部分(service)
V(View)      :处理数据显示的部分(静态/动态网页)
C(Controller):处理应用程序业务逻辑, 数据和页面的桥梁(controller)

推荐阅读: https://github.com/atian25/blog/issues/18
* */
/*
1.Egg.js使用
1.1手动安装手动配置
1.2利用egg脚手架工具安装使用(egg-init)

2.手动安装手动配置
1.1创建Egg项目
npm init --y
npm i egg --save  #egg模块就是egg.js的核心模块
npm i egg-bin --save-dev    #egg-bin模块, 这个模块是用于快速启动项目, 用于本地开发调试的模块
"dev": "egg-bin dev"

相关参考文档:
https://eggjs.org/zh-cn/intro/quickstart.html
https://eggjs.org/zh-cn/core/development.html

1.2编写Egg项目
egg-example
   ├ app
       ├── controller
       └── router.js
   ├ config
        ├──config.default.js
* */
```

### Egg.js的基本使用

![image-20210828004637951](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210828004637951.png)

- config.default.js

```js
exports.keys = 'COM.it666.*?'; // 用于生成客户端中保存的userId
```

- router.js

```js
// 在router.js中必须暴露出去一个方法, 这个方法接收一个参数, 这个参数就是服务端的实例对象
module.exports = app => {
    /*
    {
      env: 'local',
      name: 'egg-example',
      baseDir: 'C:\\Users\\Jonathan_Lee\\Desktop\\Node_Common\\egg-example',
      subdomainOffset: 2,
      config: '<egg config>',
      controller: '<egg controller>',
      httpclient: '<egg httpclient>',
      loggers: '<egg loggers>',
      middlewares: '<egg middlewares>',
      router: '<egg router>',
      serviceClasses: '<egg serviceClasses>'
    }
    * */
    // console.log(app);
    // 1.从服务端的实例对象中解构出处理路由的对象和处理控制器的对象
    const {router, controller} = app;
    // 2.利用处理路由的对象监听路由的请求
    //   由于EggJS是基于KOA的, 所以监听方式和KOA一样
    /*
    在EggJS中不用导入控制器, 只要拿到了从服务器实例中解构出来的控制器对象
    就相当于拿到了controller目录, 我们就可以通过点语法拿到这个目录中的文件
    只要拿到了controller目录中的文件, 我们就可以通过点语法拿到这个文件中的方法
    * */
    router.get('/', controller.home.index);
}
```

- home.js

```js
const Controller = require('egg').Controller;

class HomeController extends Controller{
    async index(){
        /*
        在EggJS中, EggJS会自动给控制器的this挂载一些属性
        this.ctx: 当前请求的上下文 Context 对象的实例，通过它我们可以拿到框架封装好的处理当前请求的各种便捷属性和方法。
        this.app: 当前应用 Application 对象的实例，通过它我们可以拿到框架提供的全局对象和方法。
        this.service：应用定义的 Service，通过它我们可以访问到抽象出的业务层，等价于 this.ctx.service 。
        this.config：应用运行时的配置项。
        this.logger：logger 对象，上面有四个方法（debug，info，warn，error），分别代表打印四个不同级别的日志，使用方法和效果与 context logger 中介绍的一样，但是通过这个 logger 对象记录的日志，在日志前面会加上打印该日志的文件路径，以便快速定位日志打印位置。
        * */
        this.ctx.body = 'www.it666.com';
    }
}

module.exports = HomeController;
```

### Egg.js处理get/post请求参数

- router.js

```js
//在router.js中必须暴露出去一个方法，这个方法接收一个参数，这个参数就是服务端的实例对象
module.exports = app => {
  const {router,controller} = app;
  router.get("/",controller.home.index);
  router.get("/user",controller.home.getQuery);
  router.get("/register/:name/:age",controller.home.getParams);
  router.post("/login",controller.home.getBody);
}
// {
//   env: 'local',
//       name: 'egg-example',
//     baseDir: 'D:\\web\\project\\egg-example',
//     subdomainOffset: 2,
//     config: '<egg config>',
//     controller: '<egg controller>',
//     httpclient: '<egg httpclient>',
//     loggers: '<egg loggers>',
//     middlewares: '<egg middlewares>',
//     router: '<egg router>',
//     serviceClasses: '<egg serviceClasses>'
// }
```

- home.js

```js
const Controller = require('egg').Controller;

class HomeController extends Controller{
    async index(){
        this.ctx.body = 'www.it666.com';
    }
    async getQuery(){
        // 获取传统get请求参数
        // this.ctx.request.query
        let query = this.ctx.query;
        this.ctx.body = query;
    }
    async getParams(){
        // 获取动态路由形式的get请求参数
        let params = this.ctx.params;
        this.ctx.body = params;
    }
    async getBody(){
        // 获取post请求参数
        let body = this.ctx.request.body;
        this.ctx.body = body;
    }
}

module.exports = HomeController;
```

- 处理post参数的时候要防范csrf所有需要先在配置中关闭安全防护

```js
// exports.keys = 'COM.it666.*?'; // 用于生成客户端中保存的userId
module.exports = {
    keys: 'COM.it666.*?',
    security: {
        csrf: {
            ignoreJSON: true, // 默认为 false，当设置为 true 时，将会放过所有 content-type 为 `application/json` 的请求
        },
    },
};
```

### Egg.js处理静态资源

![image-20210831093518493](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831093518493.png)

- **在app目录下面创建一个public目录，将静态资源存放在里面即可**

### Egg.js处理动态资源

![image-20210831100244874](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831100244874.png)

```js
/*
    1.EggJS如何处理动态资源?
    1.1什么是插件?
    EggJS中的插件就是特殊的中间件
    用来处理那些和请求无关独立的业务逻辑
    https://eggjs.org/zh-cn/basics/plugin.html

    1.2插件的使用
    npm i egg-view-ejs --save
    在config目录下新建plugin.js
    exports.ejs={ enable:true, package:'egg-view-ejs', };
    在config.default.js中新增如下配置
    view:{mapping:{'.html':'ejs'}}
    在app目录中新建view目录, 将动态网页放到这个目录中
    在控制器中通过上下文render方法渲染
    * */
```

### Egg.js处理网络数据

![image-20210831105127664](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831105127664.png)

- **创建service目录**
- **利用this.ctx.curl发送get/post请求**

```js
const Service = require('egg').Service;

class HomeService extends Service{
    async findNews(){
        // 在Service定义的方法中处理数据库和网络的数据即可
        /*
        和控制器一样, Service类的this上也挂载了很多属性
        this.ctx: 当前请求的上下文 Context 对象的实例，通过它我们可以拿到框架封装好的处理当前请求的各种便捷属性和方法。
        this.app: 当前应用 Application 对象的实例，通过它我们可以拿到框架提供的全局对象和方法。
        this.service：应用定义的 Service，通过它我们可以访问到其他业务层，等价于 this.ctx.service 。
        this.config：应用运行时的配置项。
        this.logger：logger 对象，上面有四个方法（debug，info，warn，error），分别代表打印四个不同级别的日志，使用方法和效果与 context logger 中介绍的一样，但是通过这个 logger 对象记录的日志，在日志前面会加上打印该日志的文件路径，以便快速定位日志打印位置
        * */
        /*
        Service的上下文属性上还挂载了一些其它的属性
        this.ctx.curl 发起网络调用。
        this.ctx.service.otherService 调用其他 Service。
        this.ctx.db 发起数据库调用等， db 可能是其他插件提前挂载到 app 上的模块。
        * */
        // 发送get不带参数的请求
        // let response = await this.ctx.curl('http://127.0.0.1:3000/getUser');
        // 发送get带参数的请求
        // let response = await this.ctx.curl('http://127.0.0.1:3000/getUser2?name=it666&age=66');
        // 发送post不带参数的请求
        // let response = await this.ctx.curl('http://127.0.0.1:3000/getNews', {
        //     method: 'post'
        // });
        // 发送post带参数的请求
        let response = await this.ctx.curl('http://127.0.0.1:3000/getNews2', {
            method: 'post',
            data:{
                name:'lnj',
                age:33
            }
        });
        let data = JSON.parse(response.data);
        console.log("HomeService", data);
        return data;
    }
}

module.exports = HomeService;
```

### Egg.js中service的注意点

```js
/*
注意点:
1.service目录必须放在app目录中
2.service目录支持多级目录, 如果是多级目录, 那么在调用的时候可以使用链式调用
  this.ctx.service.abc.def.text.xxx();
3.service中的js文件, 如果是以_或者首字母都是大写, 那么在调用的时候必须转换成驼峰命名
  get_user.js --- getUser
  GetUser.js --- getUser  
* */
```

### Egg.js处理cookie

```js
const Controller = require('egg').Controller;

class HomeController extends Controller{
    async setCookie(){
        this.ctx.cookies.set('name', 'lnj', {
            path:'/',
            maxAge: 24 * 60 * 60 * 1000,
            httpOnly: true,
            /*
            在EggJS中, 为了安全着想, 阿里的安全专家建议我们在设置Cookie的时候, 给保存的数据生成一个签名
            将来获取数据的时候, 再利用获取到的数据生成一个签名, 和当初保存的时候的签名进行对比
            如果一致, 表示保存在客户端的数据没有被篡改, 如果不一致表示保存在客户端的数据已经被篡改
            * */
            signed: true, // 根据config/config.default.js中的keys来生成签名
            encrypt: true // 让EggJS帮我们加密之后再保存
        });
        this.ctx.body = '设置成功';
    }
    async getCookie(){
        let cookie = this.ctx.cookies.get('name', {
            signed: true,
            encrypt: true
        });
        this.ctx.body = `获取Cookie成功 = ${cookie}`;
    }
}

module.exports = HomeController;
```

### Egg.js处理日志

```js
async test(){
    /*
        注意点:
        默认只会输出 INFO 及以上（WARN 和 ERROR）的日志到文件中。
        如果需要输出debug日志, 那么就必须修改config.default.js配置文件
        https://eggjs.org/zh-cn/core/logger.html#%E6%97%A5%E5%BF%97%E7%BA%A7%E5%88%AB
        * */
    /*
        EggJS中如何切割日志?
        在EggJS中不用我们手动的去切割, 默认情况下EggJS就会自动帮我们切割
        默认情况下每一天就是一个新的日志文件
        例如: common-error.log
              common-error.log.2022-12-12
        * */
    this.ctx.logger.debug('我是debug日志');
    this.ctx.logger.info('我是info日志');
    this.ctx.logger.warn('我是warn日志');
    this.ctx.logger.error('我是error日志');
}
```

### Egg.js处理定时任务

- **在app中创建schedule目录**

![image-20210831151344656](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831151344656.png)

```js
const Subscription = require('egg').Subscription;

class UpdateCache extends Subscription {
    // 通过 schedule 属性来设置定时任务的执行间隔等配置
    static get schedule() {
        return {
            interval: '3s', // 间隔3秒执行一次
            type: 'all', //  all表示当前服务器上所有相同的Node进程都执行
        };
    }

    // subscribe 是真正定时任务执行时被运行的函数
    async subscribe() {
       let response = await this.ctx.curl('http://127.0.0.1:3000/getMsg');
       let data = new Buffer(response.data).toString();
        console.log(data);
    }
}

module.exports = UpdateCache;
```

### Egg.js启动自定义

**当我们需要在项目启动之后立马执行定时任务的时候可以使用启动自定义**

- **首先在项目文件下创建一个入口文件app.js**

![image-20210831152541919](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831152541919.png)

```js
// app.js
class AppBootHook {
    constructor(app) {
        this.app = app;
    }
    // 这个方法会在EggJS程序启动完毕之后执行
    async serverDidReady() {
        // 注意点: 这里传递的不是方法名称, 而是需要被执行的那个定时任务文件的名称
       await this.app.runSchedule('updateMessage')
    }
}

module.exports = AppBootHook;
```

### Egg.js框架扩展上

- **在app目录下创建extend目录**
- **在extend目录下创建四个js文件：application.js、request.js、response.js、context.js**

![image-20210831154837032](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831154837032.png)

```js
//   aplication.js
module.exports = {
    myTest(param) {
        // this 就是 app 对象，在其中可以调用 app 上的其他方法，或访问属性
        return `自定义扩展中的方法被调用了${param}`;
    },
};

//   context.js
module.exports = {
    myTest(param) {
        // this 就是 ctx 对象，在其中可以调用 ctx 上的其他方法，或访问属性
        return `自定义扩展中的方法被调用了${param}`;
    },
};

//   request.js
module.exports = {
    myTest(param) {
        // this 就是 request 对象，在其中可以调用 request 上的其他方法，或访问属性
        return `自定义扩展中的方法被调用了${param}`;
    },
};

//   response.js
module.exports = {
    myTest(param) {
        // this 就是 response 对象，在其中可以调用 response 上的其他方法，或访问属性
        return `自定义扩展中的方法被调用了${param}`;
    },
};
```

### Egg.js框架扩展下

- **工具类的定义使用**

- **在extend目录下创建helper.js**

![image-20210831155242159](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831155242159.png)

```js
const crypto = require('crypto');
module.exports = {
    md5:(password) => {
        // 1.指定加密方式
        const md5 = crypto.createHash('md5')
        // 2.指定需要加密的内容和加密之后输出的格式
        const hash = md5.update(password) // 指定需要加密的内容
            .digest('hex'); // 指定加密之后输出的格式
        return hash;
    }
};
```

### Egg.js中间件

**在Egg.js中自定义中间件：**

- **在app中创建middleware目录**
- **在里面定义中间件**

![image-20210831162349064](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831162349064.png)

```js
//clientCheck.js
/**
 * 自定义中间件
 * @param options 是一个对象
 * @param app     服务器实例对象
 */
// { ua: /Chrome/ }
module.exports = (options, app) =>{
    return async (ctx, next) =>{
        // 1.获取客户端的请求信息
       let userAgent =  ctx.get('user-agent');
       // 2.判断客户端是否是谷歌浏览器
       let flag = options.ua.test(userAgent);
       if(flag){
           ctx.status = 401;
           ctx.body = '不支持当前的浏览器';
       }else{
           next();
       }
    }
}
```

**注册全局使用(项目启动自动调用)：**

- **config中进行全局配置**

```js
// 配置需要的中间件，数组顺序即为中间件的加载顺序
// 注意点: 这里的中间件名称就是文件名称
// middleware: [ 'clientCheck' ],
// 这里的key也是中间件文件的名称 
// 这里的值将来就会传递给中间件的options
// clientCheck: {
//     ua: /Chrome/
// },
```

**在路由中使用（局部使用）:**

- **用app.middleware.中间件的方式获取并传参**

```js
//router.js
module.exports = app => {
    // 1.从服务端的实例对象中解构出处理路由的对象和处理控制器的对象
    const {router, controller} = app;
    // 2.处理路由
    /*
    1.EggJS中间件
    EggJS是基于KOA的, 所以EggJS的中间件形式和 Koa 的中间件形式是一样的
    只不过EggJS规定我们需要将中间件写到特殊的目录中
    只不过EggJS中为中间件提供了多种使用方式
    https://eggjs.org/zh-cn/basics/middleware.html
    * */
    let clientCheck = app.middleware.clientCheck({ua:/Chrome/});
    router.get('/test', clientCheck, controller.home.test);
    router.get('/', controller.home.index);
}
```

### Egg.js国际化

- **首先在config目录下创建一个locale文件，然后创建需要的语言js文件**

![image-20210831164903678](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831164903678.png)

```js
module.exports = {
    Email:'email',
    userName: 'userName',
    password: 'password'
}

//zh-CN.js
module.exports = {
    Email:'邮箱',
    userName: '用户名',
    password: '密码'
}
```

**调用使用：**

- **this.ctx.__(key)**

```js
const Controller = require('egg').Controller;

class HomeController extends Controller{
    async test(){
        return await this.ctx.render('index', {msg: this.ctx.__('Email')})
    }
}
module.exports = HomeController;
```

**如何切换语言:**

![image-20210831165037931](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210831165037931.png)

### Egg.js操作mysql数据库

- **安装插件：npm i egg-mysql**
- **在config.default.js中进行配置**
- **在plugin.js中进行配置**

```js
mysql : {
        // 单数据库信息配置
        client: {
            // host
            host: '127.0.0.1',
            // 端口号
            port: '3306',
            // 用户名
            user: 'root',
            // 密码
            password: 'root',
            // 数据库名
            database: 'demo',
        },
        // 是否加载到 app 上，默认开启
        app: true,
        // 是否加载到 agent 上，默认关闭
        agent: false,
    }
```

```js
module.exports = {
    ejs :{
        enable:true,
        package:'egg-view-ejs'
    },
    mysql : {
        enable: true,
        package: 'egg-mysql',
    }
}
```

**在service层中调用app实例上的mysql方法**

```js
const Service = require('egg').Service;

class HomeService extends Service{
    async insertUser({name, age}){
        try {
            let res = await this.ctx.app.mysql.insert('user', {name:name, age:age});
            return res.affectedRows === 1;
        }catch (e) {
            console.error(e);
            return false;
        }
    }
}

module.exports = HomeService;
```

### Egg.js操作sequelize

- **安装egg-sequelize和mysql**
- **在config中进行配置**
- **在app目录下创建一个model目录，里面创建模型的js文件**
- **在service中使用this.ctx.model.xxx.xxx进行使用**

![image-20210901001804787](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210901001804787.png)

**user.js**

```js
module.exports = app => {
    const { STRING, INTEGER, DATE } = app.Sequelize;

    const User = app.model.define('user', {
        id: { type: INTEGER, primaryKey: true, autoIncrement: true },
        name: STRING(30),
        age: INTEGER,
        created_at: DATE,
        updated_at: DATE,
    });

    return User;
    // this.ctx.model.User.create()
};
```

**service/home.js**

```js
const Service = require('egg').Service;

class HomeService extends Service{
    async insertUser({name, age}){
        try {
            let res = await this.ctx.model.User.create({name:name, age:age});
            return res.dataValues;
        }catch (e) {
            return '插入失败';
        }
    }
}

module.exports = HomeService;
```

### Egg.js中的配置文件

- **首先要npm install cross-env中间件**

```js
/*
    1.EggJS配置文件
    EggJS给我们提供了多种配置文件, 方便我们在不同的阶段中使用
    config.prod.js     // 只有线上环境会加载
    config.test.js     // 只有测试环境会加载
    config.local.js     // 只有开发环境会加载
    config.default.js  // 所有环境都会加载
    如果出现同名的配置, 后面三个配置文件中的配置会覆盖default中的配置
    https://eggjs.org/zh-cn/basics/config.html

    2.如何设置当前环境?
    EGG_SERVER_ENV=xxx
    https://eggjs.org/zh-cn/basics/env.html
* */
```

**package.json**

![image-20210901090628245](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210901090628245.png)

**配置文件如下**

![image-20210901090656497](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210901090656497.png)

```js
//config.local.js
module.exports = {
    sequelize : {
        dialect: 'mysql',
        host: '127.0.0.1',
        port: 3306,
        user: 'root',
        password: 'root',
        database: 'demo',
    }
};
//config.prod.js
module.exports = {
    sequelize : {
        dialect: 'mysql',
        host: '127.0.0.1',
        port: 3306,
        user: 'root',
        password: 'root',
        database: 'prod',
    }
};
```

### Egg.js脚手架

```js
/*
1.如何通过EggJS的脚手架来创建Egg项目
1.1在全局安装脚手架工具  npm i egg-init -g
1.2新建一个项目文件夹, 在这个文件夹中执行 npm init egg --type=simple
1.3执行初始化指令之后全程回车(也可以输入内容  项目名称/项目描述/项目作者)
1.4进入项目文件夹, 执行 npm install 安装相关的依赖
1.5可以通过 npm run dev / npm run test / npm run start运行项目
run dev    在开发模式下运行
run test   在调试模式下运行
run start  在上线环境中运行
* */
```

### Egg.js-CSRF安全防范

**默认请求任何一个接口都会在cookie下设置一个csrfToken的数据，所以在之后的每一次请求中都必须将其携带上**

```js
// https://eggjs.org/zh-cn/core/security.html#%E5%AE%89%E5%85%A8%E5%A8%81%E8%83%81-csrf-%E7%9A%84%E9%98%B2%E8%8C%83
'use strict';

const Controller = require('egg').Controller;
class UserController extends Controller {
    async register() {
        const { ctx } = this;
        /*
        想要拿到表单提交的数据, 那么在提交数据的时候必须将服务端给客户端设置的csrfToken也传递过来才可以
        * */
        console.log(ctx.request.body);
    }
}
module.exports = UserController;
```

### Egg.js校验前端数据

- **首先下载egg-ajv，npm install egg-ajv --save**
- **根据文档手册进行配置**
- **在app目录下创建一个schema目录，然后目录中定义校验数据的模型**
- **用this.ctx.validate进行校验**

![image-20210901131924902](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210901131924902.png)

```js
// controller中的home.js
const Controller = require('egg').Controller;

class UserController extends Controller {
  async register() {
    const { ctx } = this;
    const res = await this.ctx.validate('schema.user', this.ctx.request.body);
    console.log(res);
  }
}

module.exports = UserController;

```

### Egg.js统一接口响应格式

- **在context下进行扩展，添加一些success和error方法，进行请求响应格式的统一**
- **在helper上定义一些常用的方法和常量，比如一些自定义的响应code**
- **在controller中进行调用**

![image-20210901141038448](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210901141038448.png)

**context.js：定义一些成功和失败的返回方法**

```js
module.exports = {
    success(data, code=200, msg='成功') {
        // this 就是 ctx 对象，在其中可以调用 ctx 上的其他方法，或访问属性
        this.body = {
            code:code,
            msg:msg,
            data:data
        }
    },
    error(code=500, msg='错误') {
        // this 就是 ctx 对象，在其中可以调用 ctx 上的其他方法，或访问属性
        this.body = {
            code:code,
            msg:msg
        }
    }
};
```

**helper.js：定义一些常见的状态码**

```js
module.exports = {
    errorCode: {
        // 成功状态码
        200: '请求成功。客户端向服务器请求数据，服务器返回相关数据',
        201: '资源创建成功。客户端向服务器提供数据，服务器创建资源',
        202: '请求被接收。但处理尚未完成',
        204: '客户端告知服务器删除一个资源，服务器移除它',
        206: '请求成功。但是只有部分回应',
        // 错误状态码
        400: '请求无效。数据不正确，请重试',
        401: '请求没有权限。缺少API token，无效或者超时',
        403: '用户得到授权，但是访问是被禁止的。',
        404: '发出的请求针对的是不存在的记录，服务器没有进行操作。',
        406: '请求失败。请求头部不一致，请重试',
        410: '请求的资源被永久删除，且不会再得到的。',
        422: '请求失败。请验证参数',
        // 服务器错误状态码
        500: '服务器发生错误，请检查服务器。',
        502: '网关错误',
        503: '服务不可用，服务器暂时过载或维护。',
        504: '网关超时',
    }
};
```

**controller下的user.js**

```js
'use strict';
const Controller = require('egg').Controller;
class UserController extends Controller {
    async register() {
        const { ctx } = this;
        // console.log(ctx.request.body);
        // 1.校验数据是否符合预期
        let res =  await ctx.validate('schema.user', ctx.request.body);
        // console.log('校验结果', res);
        // 2.根据校验结果做出对应的处理
        if(res){
            // 将校验通过的数据交给Service存储到数据库中
        }else{
            // 告诉前端数据不符合预期
            // this.ctx.body = {
            //     code:400,
            //     msg:'数据不符合预期'
            // }
            ctx.error(400, ctx.helper.errorCode[400]);
        }
    }
}
module.exports = UserController;
```

### Egg.js保存注册数据

- **首先下载sequelize及其依赖，然后进行配置**
- **在app目录下创建一个model目录，然后定义一个模型**
- **利用sequelize的属性验证其进行数据的第三次验证**
- **在service层中进行this.ctx.model.User.create进行调用**

![image-20210901213549206](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210901213549206.png)

```js
// schema/user.js
const userSchema = {
  type: 'object',
  properties: {
    username: {
      type: 'string',
      pattern: '^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\\.[a-zA-Z0-9_-]+)+$',
      maxLength: 255,
      minLength: 3,
    },
    password: {
      type: 'string',
      pattern: '^[A-Za-z0-9]{6,20}$',
      maxLength: 20,
      minLength: 6,
    },
    gender: {
      type: 'string',
      pattern: '[男,女,妖]',
    },
  },
  required: [ 'username', 'password' ],
};
module.exports = userSchema;
```

### Egg.js加密工具封装

- **将密码以加密后的方式进行存储**
- **在helper中进行封装**

```js
const crypto = require('crypto');
module.exports = {
  errorCode: {
    // 成功状态码
    200: '请求成功。客户端向服务器请求数据，服务器返回相关数据',
    201: '资源创建成功。客户端向服务器提供数据，服务器创建资源',
    202: '请求被接收。但处理尚未完成',
    204: '客户端告知服务器删除一个资源，服务器移除它',
    206: '请求成功。但是只有部分回应',
    // 错误状态码
    400: '请求无效。数据不正确，请重试',
    401: '请求没有权限。缺少API token，无效或者超时',
    403: '用户得到授权，但是访问是被禁止的。',
    404: '发出的请求针对的是不存在的记录，服务器没有进行操作。',
    406: '请求失败。请求头部不一致，请重试',
    410: '请求的资源被永久删除，且不会再得到的。',
    422: '请求失败。请验证参数',
    // 服务器错误状态码
    500: '服务器发生错误，请检查服务器。',
    502: '网关错误',
    503: '服务不可用，服务器暂时过载或维护。',
    504: '网关超时',
  },
  _md5(password) {
  // 1.指定加密方式
    const md5 = crypto.createHash('md5');
    // 2.指定需要加密的内容和加密之后输出的格式
    const hash = md5.update(password) // 指定需要加密的内容
      .digest('hex'); // 指定加密之后输出的格式
    return hash;
  },
  generatePwd(password) {
    password = password + this.config.keys;
    const hash = this._md5(password);
    // console.log(hash); // 4167228cfbe1daa78e88c41bf357618e --> abcdefcom.it666
    return hash;
  },
};
```

### Egg.js防止重复注册

- **创建表的时候将字段改为unique**
- **或者先查询用户名是否已经存在**

```js
const Service = require('egg').Service;

class UserService extends Service {
  async createUser({ username, password, gender }) {
    const users = await this.getUser(username);
    if (users.length === 0) {
      const res = await this.ctx.model.User.create({ username, password: this.ctx.helper.generatePwd(password), gender });
      return res.dataValues;
    }
    throw new Error('当前用户已存在');
  }
  async getUser(username) {
    const res = await this.ctx.model.User.findAll({
      where: {
        username,
      },
    });
    return res;
  }
}

module.exports = UserService;
```

### Egg.js实现登录

- **带上csrfToken**
- **定义处理路由**
- **定义controller的处理函数**
- **定义service中的方法**

```js
const Service = require('egg').Service;

class UserService extends Service {
  async createUser({ username, password, gender }) {
    const users = await this.getUser(username);
    if (users.length === 0) {
      const res = await this.ctx.model.User.create({ username, password: this.ctx.helper.generatePwd(password), gender });
      return res.dataValues;
    }
    throw new Error('当前用户已存在');
  }
  async getUser(username, password) {
    if (password) {
      const res = await this.ctx.model.User.findOne({
        where: {
          username,
          password: this.ctx.helper.generatePwd(password),
        },
      });
      if (res) {
        return res.dataValues;
      }
      throw new Error('用户名或密码不正确');
    }
    const res = await this.ctx.model.User.findAll({
      where: {
        username,
      },
    });
    return res;
  }
  async findUser({ username, password }) {
    const user = await this.getUser(username, password);
    return user;
  }
}

module.exports = UserService;
```

### Egg.js保存登录状态

- **npm i egg-session-redis egg-redis --save**
- **使用this.ctx.session进行存储和获取(会自动设置cookie并且自动存入redis，自动从redis中取出)**

```js
const Controller = require('egg').Controller;

class UserController extends Controller {
  async register() {
    const { ctx } = this;
    // 1.校验数据是否符合预期
    const res = await ctx.validate('schema.user', ctx.request.body);
    // 2.根据校验结果做出对应的处理
    if (res) {
      // 将校验通过的数据交给Service存储到数据库中
      try {
        console.log('交给service处理');
        const res = await ctx.service.user.createUser(ctx.request.body);
        ctx.success(res);
      } catch (e) {
        ctx.error(400, e.message);
      }
    } else {
      // 告诉前端数据不符合预期
      ctx.error(400, ctx.helper.errorCode[400]);
    }
  }
  async login() {
    const { ctx } = this;
    try {
      const res = await ctx.service.user.findUser(ctx.request.body);
      ctx.session.user = res;
      ctx.success(res);
    } catch (e) {
      ctx.error(202, e.message);
    }
  }
  async test() {
    console.log(this.ctx.session.user);
  }
}

module.exports = UserController;
```

### Egg.js单元测试开篇

```js
/*
  1.什么是单元测试?
  单元测试是指对软件中的最小可测试单元进行检查和验证
  2.什么是最小可测试单元?
  一个函数, 一个类, 一个文件, 这些都可以称之为最小可测试单元
  具体需要根据实际情况去判定其具体含义, 一般情况下我们以函数作为最小单元即可
  3.单元测试有什么用?
  保证代码的正确性
  保存程序的稳定性
  增强自信心
  公司领导要求(红绿灯)

  4.在EggJS中如何进行单元测试
  4.1EggJS使用Mocha测试框架和power-assert断言库来进行单元测试
  Mocha:        https://mochajs.org/
  作用 : 提供了编写测试代码的方法
  power-assert: https://github.com/power-assert-js/power-assert
  作用 : 判断测试结果是否正确

  4.2EggJS还抽取了一个叫做egg-mock的辅助模块, 配合Mocha和power-assert进行测试
  egg-mock:     https://www.npmjs.com/package/egg-mock
  作用 : 帮助我们能够在单元测试中模拟app, context, cookie, session, 网络请求等

  4.3EggJS规定了测试文件的存放路径和文件名称
  test
  ├── controller
  │   └── home.test.js
  ├── hello.test.js
  └── service
      └── user.test.js

  4.4EggJS规定我们使用 egg-bin test来运行我们编写的测试文件
  * */
```

### Egg.js测试模板和声明生命周期钩子

```js
'use strict';
/*
app   :服务端实例对象
mock  :egg提供给我们的辅助模块对象
assert:断言库对象
* */
const { app, mock, assert } = require('egg-mock/bootstrap');
/*
在测试文件中一个describe函数就是一组相关的测试
也就是说我们需要把一组相关的测试写到一个describe函数中
第一个参数: 这组测试的名称
第二个参数: 编写这组测试具体代码的地方
* */
describe('test/app/controller/user.test.js', () => {
  /*
  在一个describe方法中的一个it就是一个测试用例
  一个it可以用来测试一个方法(函数)
  第一个参数: 给当前的这个测试取的名称
  第二个参数: 编写具体测试代码的函数
  * */
  // it('should assert', () => {
  //
  // });
  /*
  Mocha测试库的生命周期钩子
  单个测试用例的
  before -> beforeEach -> it -> afterEach -> after
  多个测试用例
  before -> beforeEach -> it -> afterEach -> beforeEach -> it -> afterEach -> after
  before ->
  beforeEach -> it -> afterEach
  beforeEach -> it -> afterEach
  beforeEach -> it -> afterEach
  -> after

  生命周期钩子的作用:
  我们可以在测试用例执行之前去申请一些资源
  我们可以在测试用例执行之后去释放申请的资源
  例如: 我们需要测试数据库, 那么我们可以在测试之前往数据库中添加一些测试数据
        然后在测试完成之后删除这些测试数据
  * */
  before(()=>{
    console.log('before');
  });
  beforeEach(()=>{
    console.log('beforeEach');
  });
  it('具体的测试用例', () => {
    console.log('it');
  });
  it('具体的测试用例', () => {
    console.log('it');
  });
  it('具体的测试用例', () => {
    console.log('it');
  });
  afterEach(()=>{
    console.log('afterEach');
  });
  after(()=>{
    console.log('after');
  });
});
```

### Egg.js同步测试和异步测试

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/controller/user.test.js', () => {
  /*
  Mocha: 同步测试和异步测试
  * */
  it('同步测试', () => {
    // console.log('it');
    // 模拟了一个上下文
    let ctx = app.mockContext({
      session:{name:'lnj'}
    });
    // 希望上下文中有session, 并且保存了name, 并且name是lnj
    // 以下代码的含义: 断定上下文中有session, session中有name,name取值是lnj
    assert(ctx.session.name === 'lnj');
  });
  it('异步测试-promise', ()=>{
    return app.httpRequest().get('/public/login.html').expect(200);
  });
  it('异步测试-callback', (done)=>{
     app.httpRequest().get('/public/login.html').expect(200, done);
  });
  it('异步测试-async+await', async ()=>{
    await app.httpRequest().get('/public/login.html').expect(200);
  });
});
```

### Egg.js测试控制器

- **首先app.mockCsrf()是用来避免csrf的防范**
- **after是用来清空数据，以免影响下一次的测试**

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/controller/user.test.js', () => {
  it('测试注册-成功', async () => {
    app.mockCsrf();
    let user = {username:'123@qq.com', password:'abc123', gender:'男'};
    let response = await app.httpRequest().post('/api/user/register').send(user);
    assert(response.body.code === 200);
  });
  it('测试注册-用户名不符合预期', async () => {
    app.mockCsrf();
    let user = {username:'jonathan', password:'abc123', gender:'男'};
    let response = await app.httpRequest().post('/api/user/register').send(user);
    assert(response.body.code === 400);
  });
  it('测试注册-密码不符合预期', async () => {
    app.mockCsrf();
    let user = {username:'234@qq.com', password:'123', gender:'男'};
    let response = await app.httpRequest().post('/api/user/register').send(user);
    assert(response.body.code === 400);
  });
  after(async ()=>{
    await app.model.User.destroy({truncate:true, force: true});
  })
});
```

### Egg.js测试service

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/service/user.test.js', () => {
    it('测试创建用户-成功', async () => {
        // 测试创建成功
        let ctx = app.mockContext();
        let user = {username:'123@qq.com', password:'abc123', gender:'男'};
        let res = await ctx.service.user.createUser(user);
        assert(res.username === '123@qq.com');

        // 测试用户名重复情况
        try {
            await ctx.service.user.createUser(user);
        }catch (e) {
            assert(e);
        }
    });
    after(async ()=>{
        await app.model.User.destroy({truncate:true, force: true});
    });
});
```

### Egg.js测试application

![image-20210902094731768](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210902094731768.png)

```js
'use strict';

const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/controller/home.test.js', () => {
  it('application', async () => {
    app.set('name', 'it666');
    assert(app.get('name') === 'it666');
  });
});
```

### Egg.js测试context

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/extend/context.test.js', () => {
    it('测试context', async () => {
        let ctx = app.mockContext();
        ctx.error();
        assert(ctx.body.code === 500);
        assert(ctx.body.msg === '错误');
    });
});
```

### Egg.js测试request

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/extend/request.test.js', () => {
    it('测试request', async () => {
        let ctx = app.mockContext({
            headers:{
                // 'user-agent':'Chrome'
                'user-agent':'Mozilla'
            }
        });
       // assert(ctx.request.isChrome() === true);
       assert(ctx.request.isChrome() === false);
    });
});
```

### Egg.js测试response

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/extend/request.test.js', () => {
    it('测试request', async () => {
        let ctx = app.mockContext();
        assert(ctx.response.isSuccess() === true);
    });
});
```

### Egg.js测试helper

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/extend/helper.test.js', () => {
    it('测试helper', async () => {
        let ctx = app.mockContext();
        assert(ctx.helper.generatePwd('123') !== '123');
    });
});
```

### Egg.js测试schedule

```js
'use strict';
const { app, mock, assert } = require('egg-mock/bootstrap');

describe('test/app/schedule/updateMessage.test.js', () => {
    it('测试updateMessage', async () => {
        await app.runSchedule('updateMessage');
        assert(app.msg === 'lnj+1');
        await app.runSchedule('updateMessage');
        assert(app.msg === 'lnj+2');
    });
});
```

### Egg.js生成测试报告

- **通过npm run cov自动生成测试报告**

![image-20210902101209431](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210902101209431.png)

