# 打造企业后端开发

## 在koa中使用ts的配置

```html
<!--
1.创建Node项目
npm init --y
2.生成配置tsconfig.json
npm install typescript ts-node --save-d
tsc --init
3.安装相关依赖
npm install koa --save
npm install @types/node @types/koa --save-d
npm install cross-env --save
npm install nodemon --save-d
4.配置package.json
"dev": "cross-env NODE_ENV=dev nodemon -e ts --exec ts-node app.ts"
5.编写koa代码
-->
```

## Koa-TS头文件使用细节

```html
<!--
1.使用别人编写好的头文件细节
安装好别人编写好的头文件之后 @type/xxx
- 如果是使用ES Module导出, 那么使用ES Module导入
- 如果是使用Node Module导出, 那么使用Node Module导入
- 如果是使用TS Module导出, 那么使用TS Module导入, 但是也可以使用ES Module或者Node Module导入
-->
<!--
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
-->
<!--
2.Node模块
1.1通过exports.xxx = xxx导出
通过const xxx = require("path");导入
通过const {xx, xx} = require("path");导入

1.2通过module.exports.xxx = xxx导出
通过const xxx = require("path");导入
通过const {xx, xx} = require("path");导入
-->
<!--
ES6的模块和Node的模块是不兼容的, 所以TS为了兼容两者就推出了
export = xxx;
import xxx = require('path');
-->
```

```js
// const Koa = require('koa'); // Node Module导入
// import Koa from 'koa'; // ES Module导入
import Koa = require("koa"); // TS Module导入

const app = new Koa();

// response
app.use((ctx:any) => {
    ctx.body = 'Hello Koa';
});

app.listen(3000, ()=>{
    console.log('listen 3000 OK');
});
```

## Koa-Ts路由的使用

```html
<!--
1.安装相关依赖
npm install koa-router --save
npm isntall @types/koa-router --save-d
2.编写相关代码
-->
```

**router/index.ts**

```js
import Router = require("koa-router");
const router:Router = new Router();

router.get('/', (ctx:any)=>{
    ctx.body = 'router index';
});
router.get('/home', (ctx:any)=>{
    ctx.body = 'router home';
});
export default router;
```

**app.js**

```ts
import Koa = require("koa"); // TS Module导入
import index from './routers/index';

const app = new Koa();

// response
// app.use((ctx:any) => {
//     ctx.body = 'Hello Koa';
// });
app.use(index.routes());
app.listen(3000, ()=>{
    console.log('listen 3000 OK');
});
```

## 创建Egg类型的ts项目

```html
<!--
1.安装Egg脚手架
npm i egg-init -g
2.通过脚手架创建Egg TS项目
egg-init xxx --type=ts
cd xxx
npm install
-->
```

## Egg.js中使用sequelize

**查看官方文档进行配置**

## Egg-TS sequelize-typescript的使用

- **npm install sequelize-typescript**
- **npm install egg-sequelize-ts**
- **利用ts的语法编写模型**

**新版的sequelize中删除了addSchema方法，因此无法使用不更新的sequelize-typescript插件，因此只能够使用5.2.2版本的egg-sequelize**

```ts
/*
'use strict';

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
};
 */
/**
 * @desc 用户表
 */
import { AutoIncrement, Column, DataType, Model, PrimaryKey, Table } from 'sequelize-typescript';
@Table({
    modelName: 'user'
})
export class User extends Model<User> {

    @PrimaryKey
    @AutoIncrement
    @Column({
        type: DataType.INTEGER,
        comment: '用户ID',
    })
    id: number;

    @Column({
        type: DataType.STRING,
        comment: '用户姓名',
    })
    name: string;

    @Column({
        type: DataType.INTEGER,
        comment: '用户邮箱'
    })
    age: number;

    @Column({
        field: 'created_at'
    })
    createdAt: Date;

    @Column({
        field: 'updated_at'
    })
    updatedAt: Date;
};
export default () => User;
```

## Sequelize-typescript语法

```html
<!--
1.@Table
https://github.com/RobinBuschmann/sequelize-typescript#table-api
@Table({
  timestamps: true,
  ...
})
https://github.com/demopark/sequelize-docs-Zh-CN/blob/v5/models-definition.md 配置
let User = sequelize.define('user', {
}, {
    // 原生sequelize定义表方法的这里就是sequelize-typescript中的@Table
});
-->
<!--
2.@Column
https://github.com/RobinBuschmann/sequelize-typescript#column-api
@Column({
    type: DataType.INTEGER,
    primaryKey: true,
    autoIncrement: true,
    allowNull: false,
    unique: true,
    comment: '这是表的主键',
    ...
})
id: number;

https://github.com/demopark/sequelize-docs-Zh-CN/blob/v5/models-definition.md 模型定义
let User = sequelize.define('user', {
    id: {
        type: Sequelize.INTEGER,
        primaryKey: true,
        autoIncrement: true
    }
}, {});
-->
<!--
3.列类型快捷装饰器
@AllowNull(allowNull?: boolean)	设置attribute.allowNull（默认为true）
@AutoIncrement	设置attribute.autoIncrement=true
@Unique	设置attribute.unique=true
@Default(value: any)	设置attribute.defaultValue为指定值
@PrimaryKey	套 attribute.primaryKey=true
@Comment(value: string)	设置attribute.comment为指定的字符串


@Column({
    type: DataType.INTEGER,
    primaryKey: true,
    autoIncrement: true,
    allowNull: false,
    unique: true,
    comment: '这是表的主键',
    ...
})
id: number;

@PrimaryKey
@AutoIncrement
@AllowNull(false)
@Unique
@Comment( '这是表的主键')
@Column({type: DataType.INTEGER});
id: number;

注意点: 结合使用时把@Column放到最下面
-->
<!--
4.自动类型推断
可以从javascript类型自动推断出以下类型。其他必须明确定义。
js类型	    推断类型
string	    STRING
boolean	    BOOLEAN
number	    INTEGER
Date	    DATE
Buffer	    BLOB
-->
<!--
5.其它
- 大部分用户和sequelize都一样, 企业开发中只需对照文档阅读使用即可
- 除此之外因为sequelize不是使用ts编写的, 所以在整合的时候需要做大量的配置和修改
  对于小白而言可能有些难度, 如果你想再简单一点, 那么你还可以在TS中使用typeorm来操作数据
- typeorm和sequelize都是数据库ORM框架, 但是typeorm是使用TS编写的, 所以在集成的时候相对简单
  但是由于typeorm比较新, 所以还有很多不足,以及遇到问题的时候相关资料也并不是很多
  所以有空的时候建议可以尝试, 但如果个人能力还不够, 那么当前还不建议集成到企业级项目中
https://www.npmjs.com/package/typeorm
-->
```

## sequelize-cli的使用

**迁移数据库的时候需要**

```html
<!--
https://eggjs.org/zh-cn/tutorials/sequelize.html
1.安装 sequelize-cli
npm install --save-dev sequelize-cli
2.编写.sequelizerc 配置文件
'use strict';
const path = require('path');
module.exports = {
  config: path.join(__dirname, 'database/config.json'),
  'migrations-path': path.join(__dirname, 'database/migrations'),
  'seeders-path': path.join(__dirname, 'database/seeders'),
  'models-path': path.join(__dirname, 'app/model'),
};
3.初始化配置文件
npx sequelize init:config
4.修改配置文件
// database/config.json -> config/config.[env].ts
5.根据配置文件创建数据库
npx sequelize db:create
set NODE_ENV=production
npx sequelize db:create
6.创建迁移文件
npx sequelize migration:generate --name=users
7.修改迁移文件为TS语法
8.在package.json中新增执行TS迁移文件脚本
"sequelize-cli-ts": "node -r ts-node/register ./node_modules/sequelize-cli/lib/sequelize"
9.执行迁移文件
npm run sequelize-cli-ts db:migrate
-->
```

## 后端数据校验

- **npm install egg-validate --save**
- **默认会给ctx绑定上validate对象**
- **在app目录下创建validate目录，然后定义校验规则**
- **在controller中使用ctx.validate(规则文件，数据)进行校验**
- **如果需要自定义错误信息，则必须使用format正则表达式作为校验规则，再通过message设置报错信息**

![image-20210905203654640](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210905203654640.png)

**validate/normalUserRules**

```ts
export default {
    /*
    username:{
        type:'string',
        trim: true,
        min: 6,
        message: '用户名至少是6位'
    }
     */
    /*
    username:{
        type:'myUserName',
        trim: true,
    }
     */
    username:{
        type:'string',
        trim: true,
        format: /^[a-zA-Z0-9]{6,}$/,
        message: '用户名至少是6位'
    }
}

```

**router.ts**

```ts
import { Application } from 'egg';

export default (app: Application) => {
  const { controller, router } = app;
  // 自定义校验规则
  // app.validator.addRule('myUserName', (_rule, value:string) => {
  //   if(value.length < 6){
  //     return '用户名至少是6位'
  //   }
  // });

  router.get('/', controller.home.index);
  router.post('/register', controller.user.create);
};

```

**controller/user.ts**

```ts
import { Controller } from 'egg';
import NormalUserRule from '../validate/normalUserRule'

export default class UserController extends Controller {
    public async create() {
        const { ctx } = this;
        const data = ctx.request.body;
        try {
            ctx.validate(NormalUserRule, data);
            ctx.body = '注册';
        }catch (e) {
            console.log(e);
        }
    }
}
```

## 普通邮箱手机注册校验

- **写好三种注册所需要的校验规则的文件**
- **在controller中定义枚举和方法**

![image-20210905210337858](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210905210337858.png)

**controller/user.ts**

```ts
import { Controller } from 'egg';
import NormalUserRule from '../validate/normalUserRule'
import EmailUserRule from '../validate/emailUserRule'
import PhoneUserRule from '../validate/phoneUserRule'
const enum RegisterTypeEnum {
    Normal = 'normal',
    Email = 'email',
    Phone = 'phone'
}

export default class UserController extends Controller {
    public async create() {
        const { ctx } = this;
        try {
            this.validateUserInfo();
            ctx.body = '注册';
        }catch (e) {
            if(e.errors){
                ctx.body = e.errors;
            }else{
                ctx.body = e.message;
            }
        }
    }
    private validateUserInfo(){
        const { ctx } = this;
        const data = ctx.request.body;
        const registerType = data.registerType;
        switch (registerType) {
            case RegisterTypeEnum.Normal:
                ctx.validate(NormalUserRule, data);
                break;
            case RegisterTypeEnum.Email:
                ctx.validate(EmailUserRule, data);
                break;
            case RegisterTypeEnum.Phone:
                ctx.validate(PhoneUserRule, data);
                break;
            default:
                throw new Error('注册类型不存在');
        }
    }
}
```

## 生成图形验证码

- **npm install --save svg-captcha**
- **在controller下创建util.ts文件**

- **导包进行调用**

```ts
import { Controller } from 'egg';
import svgCaptcha = require('svg-captcha');

export default class UtilController extends Controller {
    public async imageCode() {
        const { ctx } = this;
        const c = svgCaptcha.create({
            size: 4, // 验证码长度
            width:160,// 验证码图片宽度
            height:60,// 验证码图片高度
            fontSize: 50, // 验证码文字大小
            ignoreChars: '0oO1ilI', // 验证码字符中排除内容 0o1i
            noise: 4, // 干扰线条的数量
            color: true, // 验证码的字符是否有颜色，默认没有，如果设定了背景，则默认有
            background: '#eee' // 验证码图片背景颜色
        });
        console.log(c); // {text: 验证码字符串, data: 验证码svg图片}
        ctx.body = c.data;
    }
}
```

## 保存图形验证码

- **通过ctx.session进行存储**
- **存储在redis中，因此需要安装npm i egg-session-redis egg-redis --save，再进行配置**

```ts
import { Controller } from 'egg';
import svgCaptcha = require('svg-captcha');

export default class UtilController extends Controller {
    public async imageCode() {
        const { ctx } = this;
        // 1.生成验证码
        const c = svgCaptcha.create({
            size: 4, // 验证码长度
            width:160,// 验证码图片宽度
            height:60,// 验证码图片高度
            fontSize: 50, // 验证码文字大小
            ignoreChars: '0oO1ilI', // 验证码字符中排除内容 0o1i
            noise: 4, // 干扰线条的数量
            color: true, // 验证码的字符是否有颜色，默认没有，如果设定了背景，则默认有
            background: '#eee' // 验证码图片背景颜色
        });
        // 2.保存生成的验证码
        ctx.session.captcha = {
            code: c.text,
            expire: Date.now() + 60 * 1000 // 验证码1分钟之后过期
        };

        // 3.将验证码发送给客户端
        ctx.body = c.data;
    }
}
```

## 验证图形验证码

- **封装一个验证的方法，将code取出，并进行判断**

```ts
import { Controller } from 'egg';
import svgCaptcha = require('svg-captcha');

export default class UtilController extends Controller {
    public async verifyImageCode(){
        const { ctx } = this;
        // 1.取出服务端中保存的验证码和过期时间
        const serverCaptcha = ctx.session.captcha;
        const serverCode = serverCaptcha.code;
        const serverExpire = serverCaptcha.expire;
        // 2.获取客户端传递过来的验证码
        const {clientCode} = ctx.query;
        // console.log(serverCode, serverExpire, clientCode);
        if(Date.now() > serverExpire){
            ctx.body = '验证码已经过期';
        }else if(serverCode !== clientCode){
            ctx.body = '验证码不正确';
        }else{
            ctx.body = '验证通过';
        }
    }
    public async imageCode() {
        const { ctx } = this;
        // 1.生成验证码
        const c = svgCaptcha.create({
            size: 4, // 验证码长度
            width:160,// 验证码图片宽度
            height:60,// 验证码图片高度
            fontSize: 50, // 验证码文字大小
            ignoreChars: '0oO1ilI', // 验证码字符中排除内容 0o1i
            noise: 4, // 干扰线条的数量
            color: true, // 验证码的字符是否有颜色，默认没有，如果设定了背景，则默认有
            background: '#eee' // 验证码图片背景颜色
        });
        // 2.保存生成的验证码
        ctx.session.captcha = {
            code: c.text,
            expire: Date.now() + 60 * 1000 // 验证码1分钟之后过期
        };
        // 3.将验证码发送给客户端
        ctx.body = c.data;
    }
}
```

## 图形验证码的封装

![image-20210906002348582](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210906002348582.png)

- **首先创建util目录，将方法定义**
- **再通过helper调用util下的方法**
- **最后通过controller调用ctx.helper**

**util/imageCode.tx**

```ts
import svgCaptcha = require('svg-captcha');

export default {
    createImageCode(ctx){
        // 1.生成验证码
        const c = svgCaptcha.create({
            size: 4, // 验证码长度
            width:160,// 验证码图片宽度
            height:60,// 验证码图片高度
            fontSize: 50, // 验证码文字大小
            ignoreChars: '0oO1ilI', // 验证码字符中排除内容 0o1i
            noise: 4, // 干扰线条的数量
            color: true, // 验证码的字符是否有颜色，默认没有，如果设定了背景，则默认有
            background: '#eee' // 验证码图片背景颜色
        });
        // 2.保存生成的验证码
        console.log(c.text);
        ctx.session.captcha = {
            code: c.text,
            expire: Date.now() + 60 * 1000 // 验证码1分钟之后过期
        };
        // 3.将验证码发送给客户端
        return c.data;
    },
    verifyImageCode(ctx, clientCode){
        // 1.取出服务端中保存的验证码和过期时间
        const serverCaptcha = ctx.session.captcha;
        let serverCode;
        let serverExpire;
        try {
            serverCode = serverCaptcha.code;
            serverExpire = serverCaptcha.expire;
        }catch (e) {
            // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
            ctx.session.captcha = null;
            throw new Error('请重新获取验证码');
        }

        if(Date.now() > serverExpire){
            // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
            ctx.session.captcha = null;
            throw new Error('验证码已经过期');
        }else if(serverCode !== clientCode){
            // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
            ctx.session.captcha = null;
            throw new Error('验证码不正确');
        }
        // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
        ctx.session.captcha = null;
    }
}
```

**helper.ts**

```ts
import ImageCode from '../util/imageCode';

module.exports = {
    createImageCode() {
        return ImageCode.createImageCode(this.ctx);
    },
    verifyImageCode(clientCode){
        ImageCode.verifyImageCode(this.ctx, clientCode);
    }
};
```

## 发送邮件验证码

- **npm  install nodemailer**
- **创建发送者对象**
- **创建需要发送的内容**
- **发送邮件**

```ts
import { Controller } from 'egg';
const nodemailer = require("nodemailer");

export default class UtilController extends Controller {
    public async imageCode() {
        const { ctx } = this;
        ctx.body = ctx.helper.createImageCode();
    }
    public async emailCode(){
        // 1.创建发送者对象
        let transporter = nodemailer.createTransport({
            host: "smtp.qq.com",
            port: 465,
            secure: true, // true for 465, false for other ports
            auth: {
                user: '97606813@qq.com', // 发送邮件的邮箱
                pass: `pbgybtocebtsbicd`, // 邮箱对应的授权码
            },
        });
        // 2.创建需要发送的内容
        let code = Math.random().toString(16).slice(2, 6).toUpperCase();;
        let info = {
            from: '97606813@qq.com', // 谁发的
            to: "97606814@qq.com", // 发给谁
            subject: "知播渔管理后台注册验证码", // 邮件标题
            text: `您正在注册知播渔管理后台系统, 您的验证码是:${code}`, // 邮件内容
        };
        // 3.发送邮件
        transporter.sendMail(info, (err, data)=>{
            if(err){
                console.log('发送邮件失败:', err);
            }else{
                console.log('发送邮件成功:', data);
            }
        })
    }
}
```

## 发送邮件方法封装

![image-20210907141152012](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210907141152012.png)

- **将之前发送邮件的方法封装在emailCode中，再将验证的方法也添加进去**
- **然后在helper中调用emailCode中的方法**
- **接着在util上调用helper中的方法即可**

**emailCode.ts**

```ts
const nodemailer = require("nodemailer");

let transporter;
export default {
    // 创建发送邮件对象
    createTransporterInstance(ctx){
        if(transporter){
            return transporter;
        }
        transporter = nodemailer.createTransport({
            host: ctx.app.config.smtp.host,
            port: ctx.app.config.smtp.port,
            secure: true, // true for 465, false for other ports
            auth: {
                user: ctx.app.config.smtp.user, // 发送邮件的邮箱
                pass: ctx.app.config.smtp.pass, // 邮箱对应的授权码
            },
        });
        return transporter;
    },
    // 创建需要发送的内容
    createEmailInfo(ctx, to:string){
        // 1.生成验证码
        let code = Math.random().toString(16).slice(2, 6).toUpperCase();
        // 2.生成发送内容
        let info = {
            from: '97606813@qq.com', // 谁发的
            to: to, // 发给谁
            subject: "知播渔管理后台注册验证码", // 邮件标题
            text: `您正在注册知播渔管理后台系统, 您的验证码是:${code}`, // 邮件内容
        };
        // 3.保存验证码
        ctx.session.email = {
            code: code,
            expire: Date.now() + 60 * 1000 // 验证码1分钟之后过期
        };
        return info;
    },
    // 发送邮件
    async sendEmailCode(ctx, to:string){
        const transporter = this.createTransporterInstance(ctx);
        const info = this.createEmailInfo(ctx, to);
        return new Promise((resolve, reject)=>{
            transporter.sendMail(info, (err, data)=>{
                if(err){
                    reject(err);
                }else{
                    resolve(data);
                }
            })
        });
    },
    verifyEmailCode(ctx, clientCode){
        // 1.取出服务端中保存的验证码和过期时间
        const serverCaptcha = ctx.session.email;
        let serverCode;
        let serverExpire;
        try {
            serverCode = serverCaptcha.code;
            serverExpire = serverCaptcha.expire;
        }catch (e) {
            // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
            ctx.session.email = null;
            throw new Error('请重新获取验证码');
        }

        if(Date.now() > serverExpire){
            // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
            ctx.session.email = null;
            throw new Error('验证码已经过期');
        }else if(serverCode !== clientCode){
            // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
            ctx.session.email = null;
            throw new Error('验证码不正确');
        }
        // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
        ctx.session.email = null;
    }
}
```

**helper.ts**

```ts
import ImageCode from '../util/imageCode';
import EmailCode from '../util/emailCode';

module.exports = {
    createImageCode() {
        return ImageCode.createImageCode(this.ctx);
    },
    verifyImageCode(clientCode){
        ImageCode.verifyImageCode(this.ctx, clientCode);
    },
    async sendEmailCode(to:string){
        return await EmailCode.sendEmailCode(this.ctx, to);
    },
    verifyEmailCode(clientCode){
        EmailCode.verifyEmailCode(this.ctx, clientCode);
    }
};
```

**controller/util.ts**

```ts
import { Controller } from 'egg';

export default class UtilController extends Controller {
    public async imageCode() {
        const { ctx } = this;
        ctx.body = ctx.helper.createImageCode();
    }
    public async emailCode(){
        const {ctx} = this;
        try {
            const {email} = ctx.query;
            const data = await ctx.helper.sendEmailCode(email);
            ctx.success(data);
        }catch (e) {
            ctx.error(400, e.message);
        }
    }
}
```

## 短信发送的封装

- **需要使用到阿里云的短信发送服务器，需要购买并且配置**
- **根据阿里云提供的SDK进行配置**
- **首先在util下进行一个smscode的方法的封装**
- **在helper中进行一个工具类smsCode的调用并再次封装**
- **在controller中进行一个调用**

**util/smsCode.ts**

```ts
const Core = require('@alicloud/pop-core');

let transporter;
export default {
    // 创建发送短信对象
    createTransporterInstance(ctx){
        if(transporter){
            return transporter;
        }
        transporter = new Core({
            accessKeyId: ctx.app.config.sms.accessKeyId,
            accessKeySecret: ctx.app.config.sms.secretAccessKey,
            endpoint: 'https://dysmsapi.aliyuncs.com',
            apiVersion: '2017-05-25'
        });
        return transporter;
    },
    // 创建需要发送的内容
    createSmsInfo(ctx, to:string){
        // 1.生成验证码
        let code = Math.random().toString(16).slice(2, 6).toUpperCase();
        let jsonCode = {code: code};
        // 2.生成发送内容
        let info = {
            "RegionId": "cn-hangzhou",
            "PhoneNumbers": to,
            "SignName": "知播渔教育科技有限公司",
            "TemplateCode": "SMS_196652342",
            "TemplateParam": JSON.stringify(jsonCode)
        };
        // 3.保存验证码
        ctx.session.sms = {
            code: code,
            expire: Date.now() + 60 * 1000 // 验证码1分钟之后过期
        };
        return info;
    },
    // 发送短信
    async sendSmsCode(ctx, to:string){
        const transporter = this.createTransporterInstance(ctx);
        const info = this.createSmsInfo(ctx, to);
        const requestOption = {
            method: 'POST'
        };
        return new Promise((resolve, reject)=>{
            transporter.request('SendSms', info, requestOption).then((result) => {
                resolve(result);
            }, (ex) => {
                reject(ex);
            })
        });
    },
    verifySmsCode(ctx, clientCode){
        // 1.取出服务端中保存的验证码和过期时间
        const serverCaptcha = ctx.session.sms;
        let serverCode;
        let serverExpire;
        try {
            serverCode = serverCaptcha.code;
            serverExpire = serverCaptcha.expire;
        }catch (e) {
            throw new Error('请重新获取验证码');
        }

        if(Date.now() > serverExpire){
            throw new Error('验证码已经过期');
        }else if(serverCode !== clientCode){
            throw new Error('验证码不正确');
        }
        // 注意点: 验证码无论验证成功还是失败, 都只能使用一次
        ctx.session.sms = null;
    }
}
```

**helper.ts**

```ts
import ImageCode from '../util/imageCode';
import EmailCode from '../util/emailCode';
import SmsCode from '../util/smsCode';

module.exports = {
    createImageCode() {
        return ImageCode.createImageCode(this.ctx);
    },
    verifyImageCode(clientCode){
        ImageCode.verifyImageCode(this.ctx, clientCode);
    },
    async sendEmailCode(to:string){
        return await EmailCode.sendEmailCode(this.ctx, to);
    },
    verifyEmailCode(clientCode){
        EmailCode.verifyEmailCode(this.ctx, clientCode);
    },
    async sendSmsCode(to:string){
        return await SmsCode.sendSmsCode(this.ctx, to);
    },
    verifySmsCode(clientCode){
        SmsCode.verifySmsCode(this.ctx, clientCode);
    }
};
```

**controller/util.ts**

```ts
import { Controller } from 'egg';

export default class UtilController extends Controller {
    public async imageCode() {
        const { ctx } = this;
        ctx.body = ctx.helper.createImageCode();
    }
    public async emailCode(){
        const {ctx} = this;
        try {
            const {email} = ctx.query;
            const data = await ctx.helper.sendEmailCode(email);
            ctx.success(data);
        }catch (e) {
            ctx.error(400, e.message);
        }
    }
    public async smsCode(){
        const {ctx} = this;
        try {
            const {phone} = ctx.query;
            const data = await ctx.helper.sendSmsCode(phone);
            ctx.success(data);
        }catch (e) {
            ctx.error(400, e.message);
        }
    }
}
```

## ts报错property does not exist on value of type

```ts
//在用TypeScript写angular2或者ionic2项目时，导入原来JavaScript代码，有时出现“property does not exist on value of type”问题

//即该对象找不到此属性，原因是ts是静态语言，类型是需要定义的，未定义就有可能找不到。

//最简单的解决方式是：加 as any

//eg：y.x报错，则改为
//(y as any).x
```

## 实现不同类型注册

- **创建service/user.ts**
- **在其中定义查找是否存在用户和添加用户两个方法**
- **注意：在config中添加数据库的时区配置**

- **注意：本次创建表的方式是通过sequelize-cli进行数据库的迁移的方式进行的创建**

```ts
import { Service } from 'egg';

/**
 * Test Service
 */
export default class User extends Service {

    public async createUser({username, email, phone, password}) {
        if(username){
            // 普通注册
            return await this.createUserByUserName(username, password);
        }else if(email){
            // 邮箱注册
            return await this.createUserByEmail(email, password);
        }else if(phone){
            // 手机注册
            return await this.createUserByPhone(phone, password);
        }
    }
    private async createUserByUserName(username, password){
        password = this.ctx.helper.encryptText(password);
        // 1.查询当前用户是否存在
        const user = await this.findUser({username:username});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create({
            username:username,
            password:password
        });
        return data['dataValues'];
    }
    private async createUserByEmail(email, password){
        password = this.ctx.helper.encryptText(password);
        // 1.查询当前用户是否存在
        const user = await this.findUser({email:email});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create({
            email:email,
            password:password
        });
        return data['dataValues'];
    }
    private async createUserByPhone(phone, password){
        password = this.ctx.helper.encryptText(password);
        // 1.查询当前用户是否存在
        const user = await this.findUser({phone:phone});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create({
            phone:phone,
            password:password
        });
        return data['dataValues'];
    }
    private async findUser(options){
        return await this.ctx.model.User.findOne({where: options});
    }
}
```

**config中数据库的配置**

```ts
config.sequelize = {
    dialect: 'mysql',
    host: '127.0.0.1',
    username: 'root',
    password: 'root',
    port: 3306,
    database: 'it666',
    // 注意点: 如果需要使用时间戳, 那么就必须指定当前的时区, 否则会相差8个小时
    timezone: '+08:00'
  };
```

# 打造企业前端开发

## 创建Vue-Ts项目

- **vue create project**
- **配置的时候选择typescript即可**

## Vue知识点的回顾

```vue
<template>
  <div class="home">
    <div>123</div>
    <div>{{message}}</div>
    <button @click="myFn">按钮</button>
    <div>{{msg}}</div>
    <div>{{msg}}</div>
    <Son :parentData="456456" @parentfn="myFn"></Son>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import Son from "../components/Son.vue";
@Options({
  data:function() {
    return {
      message:"123456789",
      str:"123456789"
    }
  },
  components: {
    Son
  },
  methods:{
    myFn:function() {
      this.message = "789"
    }
  },
  watch:{
    message(newValue,oldValue){
      console.log(newValue, oldValue);
    }
  },
  computed:{
    msg(){
      return this.str.split("").reverse().join("");
    }
  }
})
export default class Home extends Vue {}
</script>

```

## 通过TS定义Vue组件

```vue
<template>
  <div class="home">
    <div>123</div>
    <div>{{message}}</div>
    <button @click="myFn">按钮</button>
    <div>{{msg}}</div>
    <div>{{msg}}</div>
    <Son :parentData="456456" @parentfn="myFn"></Son>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import Son from "../components/Son.vue";
@Options({
  data:function() {
    return {
      message:"123456789",
      str:"123456789"
    }
  },
  components: {
    Son
  },
  methods:{
    myFn:function() {
      this.message = "789"
    }
  },
  watch:{
    message(newValue,oldValue){
      console.log(newValue, oldValue);
    }
  },
  computed:{
    msg(){
      return this.str.split("").reverse().join("");
    }
  }
})
export default class Home extends Vue {}
</script>
```

## 使用vue-property-decorator定义Vue组件

```vue
<template>
  <div class="home">
    <p>首页</p>
    <p>{{message}}</p>
    <button @click="myFn">我是父组件的按钮</button>
    <p>{{msg}}</p>
    <p>{{msg}}</p>
    <p>{{msg}}</p>
    <p>{{msg}}</p>
    <Son :parentdata="message" @parentfn="myFn"></Son>
  </div>
</template>

<script>
/*
1.如果在Vue项目中使用了TypeScript,
那么除了可以像过去一样通过对象的方式来定义组件以外
还可以借助官方的vue-class-component库和vue-property-decorator库用类的方式来定义组件
vue-class-component：强化 Vue 组件，使用 TypeScript/装饰器 增强 Vue 组件
https://class-component.vuejs.org/
vue-property-decorator：在 vue-class-component 上增强更多的结合 Vue 特性的装饰器
https://github.com/kaorun343/vue-property-decorator
* */

// import Vue from 'vue'
// import Component from 'vue-class-component'

import {Vue, Component, Watch} from 'vue-property-decorator';
import Son from '../components/Son'

@Component({
  // 如果在类中找不到需要添加的内容, 那么就可以写在这个地方
  name: 'Home',
  components: {
    Son
  }
})
export default class Home extends Vue {
  // 数据绑定
  // 如果是通过类的方式来定义组件, 那么类中的属性就是过去data中定义的数据
  message = 'www.it666.com';
  str = '12345678';
  // 方法绑定
  // 如果是通过类的方式来定义组件, 那么类中的方法就是过去methods中定义的方法
  myFn(data){
    alert(`www.itzb.com +++ ${data}`);
  }
  // 计算属性
  // 如果是通过类的方式来定义组件, 那么我们类中的getter方法就是过去computed中的方法
  get msg(){
    console.log('执行了');
    return this.str.split("").reverse().join("");
  }
  // 第一个参数: 需要观察的属性名称
  // 第二个参数: 可选配置, 如果deep:true,表示深度监听
  @Watch('message', {deep: true})
  messageChange(newValue, oldValue){
    console.log(newValue, oldValue);
  }
}
</script>

```

**子组件的使用**

```vue
<template>
  <div class="home">
    <p>首页的子组件</p>
    <p>{{parentdata}}</p>
    <button @click="sonFn">子组件按钮</button>
  </div>
</template>

<script>
import {Vue, Component, Prop, Emit} from 'vue-property-decorator'

@Component
export default class TSSon extends Vue {
  @Prop(String) parentdata;
  @Emit('parentfn')
  sonFn(){
    // this.$emit('parentfn');
    return 666;
  }
}
</script>
```

## 注册界面-Tabs

```vue
<template>
    <div class="register_container">
        <div class="register_box">
            <h1>欢迎注册</h1>
            <el-tabs tab-position="left" style="height: 200px;">
                <el-tab-pane label="普通注册">普通注册</el-tab-pane>
                <el-tab-pane label="邮箱注册">邮箱注册</el-tab-pane>
                <el-tab-pane label="手机注册">手机注册</el-tab-pane>
            </el-tabs>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    @Component({
        name: "Register",
        components:{},
    })
    export default class Register extends Vue{

    }
</script>

<style lang="scss" scoped>
.register_container {
    width: 100%;
    height: 100%;
    background: url("../assets/bg.jpg") no-repeat;
    background-size: cover;
    .register_box{
        width: 500px;
        height: 300px;
        background: #fff;
        border-radius: 10px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        h1{
            text-align: center;
        }
    }
}
</style>
```

**element-ui的按需导入在plugin文件目录下的element.js中进行配置**

```js
import Vue from 'vue'
import { Button, Tabs, TabPane} from 'element-ui'

Vue.use(Button)
Vue.use(Tabs)
Vue.use(TabPane)
```

## 注册界面-form

```vue
<template>
    <div class="register_container">
        <div class="register_box">
            <h1>欢迎注册</h1>
            <el-tabs tab-position="left" style="height: 300px;">
                <el-tab-pane label="普通注册">
                    <el-form ref="form" :model="registerData" label-width="0px">
                        <el-form-item label="">
                            <el-input v-model="registerData.username" prefix-icon="el-icon-user"></el-input>
                        </el-form-item>
                        <el-form-item label="">
                            <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
                        </el-form-item>
                        <el-form-item label="">
                            <el-row>
                                <el-col :span="18">
                                    <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
                                </el-col>
                                <el-col :span="6">
                                    <img src="../assets/code.png" style="width: 120px; height: 40px">
                                </el-col>
                            </el-row>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
                        </el-form-item>
                        <el-form-item>
                            <el-checkbox v-model="registerData.checked">
                                <p>
                                    阅读并接受
                                    <a href="javascript:;">《知播渔用户协议》</a>
                                    及
                                    <a href="javascript:;">《知播渔隐私权保护声明》</a>
                                </p>
                            </el-checkbox>
                        </el-form-item>
                    </el-form>
                </el-tab-pane>
                <el-tab-pane label="邮箱注册">邮箱注册</el-tab-pane>
                <el-tab-pane label="手机注册">手机注册</el-tab-pane>
            </el-tabs>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    @Component({
        name: "Register",
        components:{},
    })
    export default class Register extends Vue{
        registerData = {
            username: '',
            password: '',
            captcha: '',
            registerType: 'normal',
            checked: false
        }
        onSubmit(){

        }
    }
</script>

<style lang="scss" scoped>
.register_container {
    width: 100%;
    height: 100%;
    background: url("../assets/bg.jpg") no-repeat;
    background-size: cover;
    .register_box{
        width: 600px;
        height: 400px;
        background: #fff;
        border-radius: 10px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        h1{
            text-align: center;
        }
        a{
            text-decoration: none;
        }
        .el-form{
            padding-right: 20px;
            box-sizing: border-box;
        }
    }
}
</style>
```

## Vue中使用ts的Ref

```vue
<template>
  <el-form ref="form" :model="registerData" label-width="0px">
    <el-form-item label>
      <el-input v-model="registerData.username" prefix-icon="el-icon-user"></el-input>
    </el-form-item>
    <el-form-item label>
      <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
    </el-form-item>
    <el-form-item label>
      <el-row>
        <el-col :span="18">
          <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
        </el-col>
        <el-col :span="6">
          <img src="http://127.0.0.1:7001/imagecode" ref="captchaImage" @click="updateCaptcha" />
        </el-col>
      </el-row>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
    </el-form-item>
    <el-form-item>
      <el-checkbox v-model="registerData.checked">
        <p>
          阅读并接受
          <a href="javascript:;">《知播渔用户协议》</a>
          及
          <a href="javascript:;">《知播渔隐私权保护声明》</a>
        </p>
      </el-checkbox>
    </el-form-item>
  </el-form>
</template>

<script lang="ts">
import { Component, Vue, Ref } from "vue-property-decorator";
@Component({
  name: "NormalForm",
  components: {}
})
export default class NormalForm extends Vue {
  registerData = {
    username: "",
    password: "",
    captcha: "",
    registerType: "normal",
    checked: false
  };
  //!:代表一定存在
  //?:代表可选（不一定存在）
  @Ref() readonly captchaImage!: HTMLImageElement;
  updateCaptcha() {
    // 虽然可以通过 as any解决报错的问题, 但是并不符合TS的思想
    // (this.$refs.captchaImage as any).src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
    this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
  }
  onSubmit() {}
}
</script>

<style lang="scss" scoped>
</style>
```

## 表单校验

```vue
<template>
    <el-form ref="form" :model="registerData" :rules="registerRules" label-width="0px">
        <el-form-item label="" prop="username">
            <el-input v-model="registerData.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item label="" prop="password">
            <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item label="" prop="captcha">
            <el-row>
                <el-col :span="18">
                    <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
                </el-col>
                <el-col :span="6">
                    <img src="http://127.0.0.1:7001/imagecode" ref="captchaImage" @click="updateCaptcha">
                </el-col>
            </el-row>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
        </el-form-item>
        <el-form-item prop="checked">
            <el-checkbox v-model="registerData.checked">
                <p>
                    阅读并接受
                    <a href="javascript:;">《知播渔用户协议》</a>
                    及
                    <a href="javascript:;">《知播渔隐私权保护声明》</a>
                </p>
            </el-checkbox>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    @Component({
        name: "NormalForm",
        components:{},
    })
    export default class NormalForm extends Vue{
        registerData = {
            username: '',
            password: '',
            captcha: '',
            registerType: 'normal',
            checked: true
        };
        validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        validateChecked = (rule: any, value:string, callback:any) => {
            if(!value){
                callback(new Error('请阅读并同意用户协议'));
            }else{
                callback();
            }
        };
        registerRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ],
            checked: [
                { validator: this.validateChecked, trigger: 'change' }
            ],
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        updateCaptcha(){
            // 虽然可以通过 as any解决报错的问题, 但是并不符合TS的思想
            // (this.$refs.captchaImage as any).src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        onSubmit(){

        };
    }
</script>
<style lang="scss" scoped>
</style>
```

## Message组件的使用

```vue
<template>
    <el-form ref="form" :model="registerData" :rules="registerRules" label-width="0px">
        <el-form-item label="" prop="username">
            <el-input v-model="registerData.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item label="" prop="password">
            <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item label="" prop="captcha">
            <el-row>
                <el-col :span="18">
                    <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
                </el-col>
                <el-col :span="6">
                    <img src="http://127.0.0.1:7001/imagecode" ref="captchaImage" @click="updateCaptcha">
                </el-col>
            </el-row>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
        </el-form-item>
        <el-form-item prop="checked">
            <el-checkbox v-model="registerData.checked">
                <p>
                    阅读并接受
                    <a href="javascript:;">《知播渔用户协议》</a>
                    及
                    <a href="javascript:;">《知播渔隐私权保护声明》</a>
                </p>
            </el-checkbox>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    @Component({
        name: "NormalForm",
        components:{},
    })
    export default class NormalForm extends Vue{
        registerData = {
            username: '',
            password: '',
            captcha: '',
            registerType: 'normal',
            checked: true
        };
        validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        validateChecked = (rule: any, value:string, callback:any) => {
            if(!value){
                callback(new Error('请阅读并同意用户协议'));
            }else{
                callback();
            }
        };
        registerRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ],
            checked: [
                { validator: this.validateChecked, trigger: 'change' }
            ],
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        updateCaptcha(){
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        @Ref() readonly form!: ElForm;
        onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    (this as any).$message.success('验证通过, 发送网络请求到服务端');
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            })
        };
    }
</script>
<style lang="scss" scoped>
</style>
```

**将message绑定到vue原型上**

```vue
import Vue from 'vue'
import { Button, Tabs, TabPane, Form, FormItem, Input, Row, Col, Checkbox, Message} from 'element-ui'

Vue.prototype.$message = Message;
Vue.use(Button)
Vue.use(Tabs)
Vue.use(TabPane)
Vue.use(Form)
Vue.use(FormItem)
Vue.use(Input)
Vue.use(Row)
Vue.use(Col)
Vue.use(Checkbox)
```

## 处理跨域

- **封装发送网络请求的方法**
- **后端使用egg-cors解决跨域问题**
- **前端进行axios携带cookie的配置**

**api/network**

```js
import axios from "axios";

// 进行一些全局配置
axios.defaults.baseURL = "http://127.0.0.1:7001";
axios.defaults.timeout = 5000;
axios.defaults.withCredentials = true; // 让axios发送请求的时候带上cookie

// 添加请求拦截器
axios.interceptors.request.use(
  function (config) {
    // 在发送请求之前做些什么
    return config;
  },
  function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
  }
);

// 添加响应拦截器
axios.interceptors.response.use(
  function (response) {
    // 对响应数据做点什么
    return response;
  },
  function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
  }
);
// 封装自己的get/post方法
export default {
  get: function (path = "", data = {}) {
    return new Promise(function (resolve, reject) {
      axios
        .get(path, {
          params: data,
        })
        .then(function (response) {
          resolve(response.data);
        })
        .catch(function (error) {
          reject(error);
        });
    });
  },
  post: function (path = "", data = {}) {
    return new Promise(function (resolve, reject) {
      axios
        .post(path, data)
        .then(function (response) {
          resolve(response.data);
        })
        .catch(function (error) {
          reject(error);
        });
    });
  },
  all: function (list: any[]) {
    return new Promise(function (resolve, reject) {
      axios
        .all(list)
        .then(
          axios.spread(function (...result) {
            // 两个请求现在都执行完成
            resolve(result);
          })
        )
        .catch(function (err) {
          reject(err);
        });
    });
  },
};

```

**components/NormalForm.vue**

```vue
<template>
    <el-form ref="form" :model="registerData" :rules="registerRules" label-width="0px">
        <el-form-item label="" prop="username">
            <el-input v-model="registerData.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item label="" prop="password">
            <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item label="" prop="captcha">
            <el-row>
                <el-col :span="18">
                    <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
                </el-col>
                <el-col :span="6">
                    <img src="http://127.0.0.1:7001/imagecode" ref="captchaImage" @click="updateCaptcha">
                </el-col>
            </el-row>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
        </el-form-item>
        <el-form-item prop="checked">
            <el-checkbox v-model="registerData.checked">
                <p>
                    阅读并接受
                    <a href="javascript:;">《知播渔用户协议》</a>
                    及
                    <a href="javascript:;">《知播渔隐私权保护声明》</a>
                </p>
            </el-checkbox>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    import {registerUser} from '../api/index';

    @Component({
        name: "NormalForm",
        components:{},
    })
    export default class NormalForm extends Vue{
        registerData = {
            username: '',
            password: '',
            captcha: '',
            registerType: 'normal',
            checked: true
        };
        validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        validateChecked = (rule: any, value:string, callback:any) => {
            if(!value){
                callback(new Error('请阅读并同意用户协议'));
            }else{
                callback();
            }
        };
        registerRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ],
            checked: [
                { validator: this.validateChecked, trigger: 'change' }
            ],
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        updateCaptcha(){
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        @Ref() readonly form!: ElForm;
        onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    /*
                    1.跨域问题
                    由于前端的地址是: http://127.0.0.1:8080
                        后端的地址是: http://127.0.0.1:7001
                    它们的端口号不同所以跨域了
                    2.如何解决跨域
                    egg-cors
                    https://www.npmjs.com/package/egg-cors
                    3.注意点:
                    如果在egg-cors插件中指定的允许跨域的地址是 http://127.0.0.1:8080
                    那么就只能是这个地址能够发送跨域请求, 其它地址不行
                    包括http://localhost:8080也不行
                    * */
                    /*
                    1.axios注意点
                    默认情况下, axios发送网络请求是不带cookie的
                    所以后端就无法拿到客户端独一无二的标识符
                    所以后端就无法获取存储的验证码
                    所以就提示: 重新获取验证码
                    2.如何解决
                    2.1首先在前端通过
                    axios.defaults.withCredentials = true;
                    告诉axios发送网络请求的时候需要携带cookie
                    2.2然后在后端通过egg-cors配置告诉服务器允许前端携带cookie
                    credentials: true
                    * */
                    registerUser(this.registerData)
                        .then((data)=>{
                            console.log(data);
                            (this as any).$message.success('注册成功');
                        })
                        .catch((e)=>{
                            (this as any).$message.error(e.message);
                        });
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        };
    }
</script>
<style lang="scss" scoped>
</style>
```

**后端config.default.ts**

```ts
import { EggAppConfig, EggAppInfo, PowerPartial } from 'egg';

export default (appInfo: EggAppInfo) => {
  const config = {} as PowerPartial<EggAppConfig>;

  // override config from framework / plugin
  // use for cookie sign key, should change to your own and keep security
  config.keys = appInfo.name + '_1595824007157_9704';

  // 跨域相关配置
  config.cors = {
    origin: 'http://127.0.0.1:8080', // 允许哪个地址跨域请求
    allowMethods: 'GET,HEAD,PUT,POST,DELETE,PATCH', // 允许哪些方法跨域请求
    credentials: true // 允许前端携带cookie
  };

  // add your egg config in here
  config.middleware = [];

  // add your special config in here
  const bizConfig = {
    sourceUrl: `https://github.com/eggjs/examples/tree/master/${appInfo.name}`,
  };

  // the return config will combines to EggAppConfig
  return {
    ...config,
    ...bizConfig,
  };
};
```

## 完善注册登录界面

- **返回的数据中去除密码**
- **前端判断返回的code是否是200**
- **如果验证码错误，自动刷新**

```vue
<template>
    <el-form ref="form" :model="registerData" :rules="registerRules" label-width="0px">
        <el-form-item label="" prop="username">
            <el-input v-model="registerData.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item label="" prop="password">
            <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item label="" prop="captcha">
            <el-row>
                <el-col :span="18">
                    <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
                </el-col>
                <el-col :span="6">
                    <img src="http://127.0.0.1:7001/imagecode" ref="captchaImage" @click="updateCaptcha">
                </el-col>
            </el-row>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
        </el-form-item>
        <el-form-item prop="checked">
            <el-checkbox v-model="registerData.checked">
                <p>
                    阅读并接受
                    <a href="javascript:;">《知播渔用户协议》</a>
                    及
                    <a href="javascript:;">《知播渔隐私权保护声明》</a>
                </p>
            </el-checkbox>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    import {registerUser} from '../api/index';

    @Component({
        name: "NormalForm",
        components:{},
    })
    export default class NormalForm extends Vue{
        registerData = {
            username: '',
            password: '',
            captcha: '',
            registerType: 'normal',
            checked: true
        };
        validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        validateChecked = (rule: any, value:string, callback:any) => {
            if(!value){
                callback(new Error('请阅读并同意用户协议'));
            }else{
                callback();
            }
        };
        registerRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ],
            checked: [
                { validator: this.validateChecked, trigger: 'change' }
            ],
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        updateCaptcha(){
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        @Ref() readonly form!: ElForm;
        onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    registerUser(this.registerData)
                        .then((data:any)=>{
                            if(data.code === 200){
                                this.$router.push('/login');
                            }else{
                                this.updateCaptcha();
                                (this as any).$message.error(data.msg);
                            }
                        })
                        .catch((e)=>{
                            this.updateCaptcha();
                            (this as any).$message.error(e.message);
                        });
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        };
    }
</script>
<style lang="scss" scoped>
</style>
```

## 完善邮箱注册

```vue
<template>
    <el-form ref="form" :model="registerData" :rules="registerRules" label-width="0px">
        <el-form-item label="" prop="username">
            <el-input v-model="registerData.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item label="" prop="password">
            <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item label="" prop="captcha">
            <el-row>
                <el-col :span="18">
                    <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
                </el-col>
                <el-col :span="6">
                    <img src="http://127.0.0.1:7001/imagecode" ref="captchaImage" @click="updateCaptcha">
                </el-col>
            </el-row>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
        </el-form-item>
        <el-form-item prop="checked">
            <el-checkbox v-model="registerData.checked">
                <p>
                    阅读并接受
                    <a href="javascript:;">《知播渔用户协议》</a>
                    及
                    <a href="javascript:;">《知播渔隐私权保护声明》</a>
                </p>
            </el-checkbox>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    import {registerUser} from '../api/index';

    @Component({
        name: "NormalForm",
        components:{},
    })
    export default class NormalForm extends Vue{
        registerData = {
            username: '',
            password: '',
            captcha: '',
            registerType: 'normal',
            checked: true
        };
        validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        validateChecked = (rule: any, value:string, callback:any) => {
            if(!value){
                callback(new Error('请阅读并同意用户协议'));
            }else{
                callback();
            }
        };
        registerRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ],
            checked: [
                { validator: this.validateChecked, trigger: 'change' }
            ],
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        updateCaptcha(){
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        @Ref() readonly form!: ElForm;
        onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    registerUser(this.registerData)
                        .then((data:any)=>{
                            if(data.code === 200){
                                this.$router.push('/login');
                            }else{
                                this.updateCaptcha();
                                (this as any).$message.error(data.msg);
                            }
                        })
                        .catch((e)=>{
                            this.updateCaptcha();
                            (this as any).$message.error(e.message);
                        });
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        };
    }
</script>
<style lang="scss" scoped>
</style>
```

## 完善手机注册

```vue
<template>
    <el-form ref="form" :model="registerData" :rules="registerRules" label-width="0px">
        <el-form-item label="" prop="phone">
            <el-input v-model="registerData.phone" prefix-icon="el-icon-phone-outline"></el-input>
        </el-form-item>
        <el-form-item label="" prop="password">
            <el-input v-model="registerData.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item label="" prop="captcha">
            <el-row>
                <el-col :span="18">
                    <el-input v-model="registerData.captcha" prefix-icon="el-icon-lock"></el-input>
                </el-col>
                <el-col :span="6">
                    <el-button @click="sendPhoneCode">发送验证码</el-button>
                </el-col>
            </el-row>
        </el-form-item>
        <el-form-item>
            <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
        </el-form-item>
        <el-form-item prop="checked">
            <el-checkbox v-model="registerData.checked">
                <p>
                    阅读并接受
                    <a href="javascript:;">《知播渔用户协议》</a>
                    及
                    <a href="javascript:;">《知播渔隐私权保护声明》</a>
                </p>
            </el-checkbox>
        </el-form-item>
    </el-form>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    import {registerUser, sendCode2Phone} from '../api/index';

    @Component({
        name: "EmailForm",
        components:{},
    })
    export default class EmailForm extends Vue{
        registerData = {
            phone: '',
            password: '',
            captcha: '',
            registerType: 'phone',
            checked: true
        };
        validatePhone = (rule: any, value:string, callback:any) => {
            const reg = /^1[3456789]\d{9}$/;
            if(!value){
                callback(new Error('请填写用户手机'));
            }else if(!reg.test(value)){
                callback(new Error('手机格式不正确'));
            }else{
                callback();
            }
        };
        validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        validateChecked = (rule: any, value:string, callback:any) => {
            if(!value){
                callback(new Error('请阅读并同意用户协议'));
            }else{
                callback();
            }
        };
        registerRules = {
            phone: [
                { validator: this.validatePhone, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ],
            checked: [
                { validator: this.validateChecked, trigger: 'change' }
            ],
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        updateCaptcha(){
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        sendPhoneCode(){
            sendCode2Phone({phone: this.registerData.phone})
                .then((data:any)=>{
                    if(data.code === 200){
                        (this as any).$message.success('验证码已发送');
                    }else{
                        (this as any).$message.error(data.msg);
                    }
                })
                .catch((e)=>{
                    (this as any).$message.error(e.message);
                });
        }
        @Ref() readonly form!: ElForm;
        onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    registerUser(this.registerData)
                        .then((data:any)=>{
                            if(data.code === 200){
                                this.$router.push('/login');
                            }else{
                                (this as any).$message.error(data.msg);
                            }
                        })
                        .catch((e)=>{
                            (this as any).$message.error(e.message);
                        });
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        };
    }
</script>
<style lang="scss" scoped>
</style>
```

**Register.vue**

```vue
<template>
    <div class="register_container">
        <div class="register_box">
            <h1>欢迎注册</h1>
            <el-tabs tab-position="left" style="height: 350px;">
                <el-tab-pane label="普通注册">
                    <NormalForm></NormalForm>
                </el-tab-pane>
                <el-tab-pane label="邮箱注册">
                    <EmailForm></EmailForm>
                </el-tab-pane>
                <el-tab-pane label="手机注册">
                    <PhoneForm></PhoneForm>
                </el-tab-pane>
            </el-tabs>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import NormalForm from '../components/NormalForm'
    import EmailForm from '../components/EmailForm'
    import PhoneForm from '../components/PhoneForm'
    @Component({
        name: "Register",
        components:{
            NormalForm,
            EmailForm,
            PhoneForm
        },
    })
    export default class Register extends Vue{

    }
</script>

<style lang="scss" scoped>
.register_container {
    width: 100%;
    height: 100%;
    background: url("../assets/bg.jpg") no-repeat;
    background-size: cover;
    .register_box{
        width: 600px;
        height: 420px;
        background: #fff;
        border-radius: 10px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        h1{
            text-align: center;
        }
        a{
            text-decoration: none;
        }
        .el-form{
            padding-right: 20px;
            box-sizing: border-box;
        }
    }
}
</style>
```

## 完善注册界面

- **进行tab切换的时候把数据进行清空**
- **将方法进行一个权限赋值，符合ts规范**

**Register.vue**

```vue
<template>
    <div class="register_container">
        <div class="register_box">
            <h1>欢迎注册</h1>
            <el-tabs tab-position="left" style="height: 350px;" @tab-click="handleClick">
                <el-tab-pane label="普通注册">
                    <NormalForm ref="normalForm"></NormalForm>
                </el-tab-pane>
                <el-tab-pane label="邮箱注册">
                    <EmailForm ref="emailForm"></EmailForm>
                </el-tab-pane>
                <el-tab-pane label="手机注册">
                    <PhoneForm ref="phoneForm"></PhoneForm>
                </el-tab-pane>
            </el-tabs>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import NormalForm from '../components/NormalForm'
    import EmailForm from '../components/EmailForm'
    import PhoneForm from '../components/PhoneForm'
    @Component({
        name: "Register",
        components:{
            NormalForm,
            EmailForm,
            PhoneForm
        },
    })
    export default class Register extends Vue{
        @Ref() readonly normalForm!: NormalForm;
        @Ref() readonly emailForm!: EmailForm;
        @Ref() readonly phoneForm!: PhoneForm;
        private handleClick(){
            this.normalForm.resetForm();
            this.emailForm.resetForm();
            this.phoneForm.resetForm();
        }
    }
</script>

<style lang="scss" scoped>
.register_container {
    width: 100%;
    height: 100%;
    background: url("../assets/bg.jpg") no-repeat;
    background-size: cover;
    .register_box{
        width: 600px;
        height: 420px;
        background: #fff;
        border-radius: 10px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        h1{
            text-align: center;
        }
        a{
            text-decoration: none;
        }
        .el-form{
            padding-right: 20px;
            box-sizing: border-box;
        }
    }
}
</style>
```

**components/EmailForm.vue**

```vue
<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    import {registerUser, sendCode2Email} from '../api/index';

    @Component({
        name: "EmailForm",
        components:{},
    })
    export default class EmailForm extends Vue{
        private registerData = {
            email: '',
            password: '',
            captcha: '',
            registerType: 'email',
            checked: true
        };
        private validateEmail = (rule: any, value:string, callback:any) => {
            const reg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
            if(!value){
                callback(new Error('请填写用户邮箱'));
            }else if(!reg.test(value)){
                callback(new Error('邮箱格式不正确'));
            }else{
                callback();
            }
        };
        private validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        private validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        private validateChecked = (rule: any, value:string, callback:any) => {
            if(!value){
                callback(new Error('请阅读并同意用户协议'));
            }else{
                callback();
            }
        };
        private registerRules = {
            email: [
                { validator: this.validateEmail, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ],
            checked: [
                { validator: this.validateChecked, trigger: 'change' }
            ],
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        private updateCaptcha(){
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        private sendEmailCode(){
            sendCode2Email({email: this.registerData.email})
                .then((data:any)=>{
                    if(data.code === 200){
                        (this as any).$message.success('验证码已发送');
                    }else{
                        (this as any).$message.error(data.msg);
                    }
                })
                .catch((e)=>{
                    (this as any).$message.error(e.message);
                });
        }
        @Ref() readonly form!: ElForm;
        private onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    registerUser(this.registerData)
                        .then((data:any)=>{
                            if(data.code === 200){
                                this.$router.push('/login');
                            }else{
                                (this as any).$message.error(data.msg);
                            }
                        })
                        .catch((e)=>{
                            (this as any).$message.error(e.message);
                        });
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        };

        public resetForm(){
            this.form.resetFields();
        }
    }
</script>
<style lang="scss" scoped>
</style>
```

## 登录界面-UI搭建

```vue
<template>
    <div class="login_container">
        <div class="login_box">
            <h1>欢迎登陆</h1>
            <el-form ref="form" :model="loginData" :rules="loginRules" label-width="0px">
                <el-form-item label="" prop="username">
                    <el-input v-model="loginData.username" prefix-icon="el-icon-user"></el-input>
                </el-form-item>
                <el-form-item label="" prop="password">
                    <el-input type="password" v-model="loginData.password" prefix-icon="el-icon-lock"></el-input>
                </el-form-item>
                <el-form-item label="" prop="captcha">
                    <el-row>
                        <el-col :span="18">
                            <el-input v-model="loginData.captcha" prefix-icon="el-icon-lock"></el-input>
                        </el-col>
                        <el-col :span="6">
                            <img src="http://127.0.0.1:7001/imagecode" ref="captchaImage" @click="updateCaptcha">
                        </el-col>
                    </el-row>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="onSubmit" style="width: 100%">注册</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    import {loginUser} from '../api/index';

    @Component({
        name: "Login",
        components:{},
    })
    export default class Login extends Vue{
        private loginData = {
            username: '',
            email: '',
            phone: '',
            password: '',
            captcha: '',
            type: 'normal',
            checked: true
        };
        private validateName = (rule: any, value:string, callback:any) => {
            const normalReg = /^[A-Za-z0-9]{6,}$/;
            const emailReg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
            const phoneReg = /^1[3456789]\d{9}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(emailReg.test(value)){
                this.loginData.email = this.loginData.username;
                this.loginData.type = 'email';
                callback();
            }else if(phoneReg.test(value)){
                this.loginData.phone = this.loginData.username;
                this.loginData.type = 'phone';
                callback();
            }else if(normalReg.test(value)){
                this.loginData.type = 'normal';
                callback();
            }else{
                callback(new Error('用户名格式不正确'));
            }
        };
        private validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        private validateCaptcha = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{4}$/;
            if(!value){
                callback(new Error('请填写验证码'));
            }else if(value.length < 4){
                callback(new Error('验证码至少是4位'));
            }else if(!reg.test(value)){
                callback(new Error('验证码只能是字母和数字'));
            }else{
                callback();
            }
        };
        private loginRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            captcha: [
                { validator: this.validateCaptcha, trigger: 'blur' }
            ]
        }
        @Ref() readonly  captchaImage!: HTMLImageElement;
        private updateCaptcha(){
            this.captchaImage.src = `http://127.0.0.1:7001/imagecode?r=${Math.random()}`;
        };
        @Ref() readonly form!: ElForm;
        private onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    loginUser(this.loginData)
                        .then((data:any)=>{
                            if(data.code === 200){
                                this.$router.push('/admin');
                            }else{
                                (this as any).$message.error(data.msg);
                            }
                        })
                        .catch((e)=>{
                            (this as any).$message.error(e.message);
                        });
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        };
    }
</script>

<style lang="scss" scoped>
    .login_container {
        width: 100%;
        height: 100%;
        background: url("../assets/bg.jpg") no-repeat;
        background-size: cover;
        .login_box{
            width: 600px;
            height: 420px;
            background: #fff;
            border-radius: 10px;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            h1{
                text-align: center;
            }
            a{
                text-decoration: none;
            }
            .el-form{
                padding: 0 20px;
                box-sizing: border-box;
            }
        }
    }
</style>
```

## 后端实现用户登录

**controller/user.ts**

```ts
import { Controller } from 'egg';
import NormalUserRule from '../validate/normalUserRule'
import EmailUserRule from '../validate/emailUserRule'
import PhoneUserRule from '../validate/phoneUserRule'
const enum TypeEnum {
    Normal = 'normal',
    Email = 'email',
    Phone = 'phone'
}
export default class UserController extends Controller {
    public async index(){
        const { ctx } = this;
        try {
            // 1.校验数据和验证码
            this.validateUserInfo();
            const data = ctx.request.body;
            ctx.helper.verifyImageCode(data.captcha);
            // 2.将校验通过的数据保存到数据库中
            const user = await ctx.service.user.getUser(data);
            ctx.success(user);
        }catch (e) {
            if(e.errors){
                ctx.error(400, e.errors);
            }else{
                ctx.error(400, e.message);
            }
        }
    }
    public async create() {
        const { ctx } = this;
        try {
            // 1.校验数据和验证码
            this.validateUserInfo();
            this.validateUserCode();
            // 2.将校验通过的数据保存到数据库中
            const data = await ctx.service.user.createUser(ctx.request.body);
            ctx.success(data);
        }catch (e) {
            if(e.errors){
                ctx.error(400, e.errors);
            }else{
                ctx.error(400, e.message);
            }
        }
    }
    private validateUserCode(){
        const { ctx } = this;
        const data = ctx.request.body;
        const type = data.type;
        switch (type) {
            case TypeEnum.Normal:
                // 校验当前的验证码是否正确
                ctx.helper.verifyImageCode(data.captcha);
                break;
            case TypeEnum.Email:
                ctx.helper.verifyEmailCode(data.captcha);
                break;
            case TypeEnum.Phone:
                ctx.helper.verifySmsCode(data.captcha);
                break;
            default:
                throw new Error('注册类型不存在');
        }
    }
    private validateUserInfo(){
        const { ctx } = this;
        const data = ctx.request.body;
        const type = data.type;
        switch (type) {
            case TypeEnum.Normal:
                // 校验数据的格式是否正确
                ctx.validate(NormalUserRule, data);
                break;
            case TypeEnum.Email:
                ctx.validate(EmailUserRule, data);
                break;
            case TypeEnum.Phone:
                ctx.validate(PhoneUserRule, data);
                break;
            default:
                throw new Error('注册类型不存在');
        }
    }
}
```

**service/user.ts**

```ts
import { Service } from 'egg';

/**
 * Test Service
 */
export default class User extends Service {
    public async getUser({username, email, phone, password}){
        password = this.ctx.helper.encryptText(password);
        let res;
        if(email){
            res = await this.findUser({email:email, password:password});
        }else if(phone){
            res = await this.findUser({phone:phone, password:password});
        }else if(username){
            res = await this.findUser({username:username, password:password});
        }
        try {
            return res.dataValues;
        }catch (e) {
            throw new Error('用户名或者密码不正确');
        }
    }
    public async createUser({username, email, phone, password}) {
        if(username){
            // 普通注册
            return await this.createUserByUserName(username, password);
        }else if(email){
            // 邮箱注册
            return await this.createUserByEmail(email, password);
        }else if(phone){
            // 手机注册
            return await this.createUserByPhone(phone, password);
        }
    }
    private async createUserByUserName(username, password){
        password = this.ctx.helper.encryptText(password);
        // 1.查询当前用户是否存在
        const user = await this.findUser({username:username});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create({
            username:username,
            password:password
        });
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
    private async createUserByEmail(email, password){
        password = this.ctx.helper.encryptText(password);
        // 1.查询当前用户是否存在
        const user = await this.findUser({email:email});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create({
            email:email,
            password:password
        });
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
    private async createUserByPhone(phone, password){
        password = this.ctx.helper.encryptText(password);
        // 1.查询当前用户是否存在
        const user = await this.findUser({phone:phone});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create({
            phone:phone,
            password:password
        });
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
    private async findUser(options){
        return await this.ctx.model.User.findOne({where: options});
    }
}
```

## 服务端保存登录状态

```ts
import { Controller } from 'egg';
import NormalUserRule from '../validate/normalUserRule'
import EmailUserRule from '../validate/emailUserRule'
import PhoneUserRule from '../validate/phoneUserRule'
const enum TypeEnum {
    Normal = 'normal',
    Email = 'email',
    Phone = 'phone'
}
export default class UserController extends Controller {
    public async isLogin(){
        const { ctx } = this;
        const user = ctx.session.user;
        if(user){
            ctx.success(user);
        }else{
            ctx.error(400, '还没有登录');
        }
    }
    public async index(){
        const { ctx } = this;
        try {
            // 1.校验数据和验证码
            this.validateUserInfo();
            const data = ctx.request.body;
            ctx.helper.verifyImageCode(data.captcha);
            // 2.将校验通过的数据保存到数据库中
            const user = await ctx.service.user.getUser(data);
            ctx.session.user = user;
            ctx.success(user);
        }catch (e) {
            if(e.errors){
                ctx.error(400, e.errors);
            }else{
                ctx.error(400, e.message);
            }
        }
    }
    public async create() {
        const { ctx } = this;
        try {
            // 1.校验数据和验证码
            this.validateUserInfo();
            this.validateUserCode();
            // 2.将校验通过的数据保存到数据库中
            const data = await ctx.service.user.createUser(ctx.request.body);
            ctx.success(data);
        }catch (e) {
            if(e.errors){
                ctx.error(400, e.errors);
            }else{
                ctx.error(400, e.message);
            }
        }
    }
    private validateUserCode(){
        const { ctx } = this;
        const data = ctx.request.body;
        const type = data.type;
        switch (type) {
            case TypeEnum.Normal:
                // 校验当前的验证码是否正确
                ctx.helper.verifyImageCode(data.captcha);
                break;
            case TypeEnum.Email:
                ctx.helper.verifyEmailCode(data.captcha);
                break;
            case TypeEnum.Phone:
                ctx.helper.verifySmsCode(data.captcha);
                break;
            default:
                throw new Error('注册类型不存在');
        }
    }
    private validateUserInfo(){
        const { ctx } = this;
        const data = ctx.request.body;
        const type = data.type;
        switch (type) {
            case TypeEnum.Normal:
                // 校验数据的格式是否正确
                ctx.validate(NormalUserRule, data);
                break;
            case TypeEnum.Email:
                ctx.validate(EmailUserRule, data);
                break;
            case TypeEnum.Phone:
                ctx.validate(PhoneUserRule, data);
                break;
            default:
                throw new Error('注册类型不存在');
        }
    }
}
```


## JWT简介

```html
<!--
1.什么是jwt?
JSON Web Token（JWT）是一种客户端保存登录状态的解决方案
优点和使用redis保存登录状态一样, 能更好的支持分布式架构


2.jwt格式
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJpZCI6NSwidXNlcm5hbWUiOiJ6aGFvbGl1IiwiZW1haWwiOm51bGwsInBob25lIjpudWxsLCJjcmVhdGVkQXQiOiIyMDIwLTA3LTIzVDA3OjEzOjUzLjAwMFoiLCJ1cGRhdGVkQXQiOiIyMDIwLTA3LTIzVDA3OjEzOjUzLjAwMFoiLCJpYXQiOjE1OTU0OTE5ODAsImV4cCI6MTU5NjA5Njc4MH0.
cQGJ72tEtBZdSWo9igyMTFI9V3mt07FfNEdZcNUAFZ8

Header 头部
Payload 负载
Signature 签名

例如: 现在JWT中保存的是张三, 那么当前的签名就是张三的指纹
      如果有人修改了JWT中保存的数据, 例如修改成了李四
      将来如果拿着李四和张三的指纹进行匹配, 那么就不匹配, 那么就怎么有人篡改了数据

https://jwt.io/

3.jwt弊端
- JWT 的最大缺点是，由于服务器不保存登录状态，因此无法在使用过程中废止某个登录状态，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。
- JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。
- 为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。
-->

<!--
1.如何使用jwt?
- 服务端生成jwt令牌,发送给客户端
- 客户端将令牌保存到cookie或者sessionStorage、localStorage中
- 客户端每次发送请求将令牌一并发送到服务端
- 服务端通过密钥验证令牌
-->
```

## jwt使用

- **npm install  jsonwebtoken**
- **调用jwt.sign()**
- **调用jwt.verify()进行校验**

```ts
import { Controller } from 'egg';
import NormalUserRule from '../validate/normalUserRule'
import EmailUserRule from '../validate/emailUserRule'
import PhoneUserRule from '../validate/phoneUserRule'
const jwt = require('jsonwebtoken');

const enum TypeEnum {
    Normal = 'normal',
    Email = 'email',
    Phone = 'phone'
}
export default class UserController extends Controller {
    public async isLogin(){
        const { ctx } = this;
        /*
        const user = ctx.session.user;
        if(user){
            ctx.success(user);
        }else{
            ctx.error(400, '还没有登录');
        }
         */
        const {token} = ctx.query;
        try {
            // 第一个参数: 需要校验的token
            // 第二个参数: 当初加密的密钥
           const user =  jwt.verify(token, this.config.keys)
            ctx.success(user);
        }catch (e) {
            ctx.error(400, e.message);
        }
    }
    public async index(){
        const { ctx } = this;
        try {
            // 1.校验数据和验证码
            this.validateUserInfo();
            const data = ctx.request.body;
            ctx.helper.verifyImageCode(data.captcha);
            // 2.将校验通过的数据保存到数据库中
            const user = await ctx.service.user.getUser(data);
            delete user.password;
            // ctx.session.user = user;
            // 3.生成JWT令牌
            // 第一个参数: 需要保存的数据
            // 第二个参数: 签名使用的密钥
            // 第三个参数: 额外配置
            const token = jwt.sign(user, this.config.keys, {expiresIn: '7 days'});
            user.token = token;
            ctx.success(user);
        }catch (e) {
            if(e.errors){
                ctx.error(400, e.errors);
            }else{
                ctx.error(400, e.message);
            }
        }
    }
    public async create() {
        const { ctx } = this;
        try {
            // 1.校验数据和验证码
            this.validateUserInfo();
            this.validateUserCode();
            // 2.将校验通过的数据保存到数据库中
            const data = await ctx.service.user.createUser(ctx.request.body);
            ctx.success(data);
        }catch (e) {
            if(e.errors){
                ctx.error(400, e.errors);
            }else{
                ctx.error(400, e.message);
            }
        }
    }
    private validateUserCode(){
        const { ctx } = this;
        const data = ctx.request.body;
        const type = data.type;
        switch (type) {
            case TypeEnum.Normal:
                // 校验当前的验证码是否正确
                ctx.helper.verifyImageCode(data.captcha);
                break;
            case TypeEnum.Email:
                ctx.helper.verifyEmailCode(data.captcha);
                break;
            case TypeEnum.Phone:
                ctx.helper.verifySmsCode(data.captcha);
                break;
            default:
                throw new Error('注册类型不存在');
        }
    }
    private validateUserInfo(){
        const { ctx } = this;
        const data = ctx.request.body;
        const type = data.type;
        switch (type) {
            case TypeEnum.Normal:
                // 校验数据的格式是否正确
                ctx.validate(NormalUserRule, data);
                break;
            case TypeEnum.Email:
                ctx.validate(EmailUserRule, data);
                break;
            case TypeEnum.Phone:
                ctx.validate(PhoneUserRule, data);
                break;
            default:
                throw new Error('注册类型不存在');
        }
    }
}

```
## 前端保存登录状态

- **首先在前端用sessionStorage进行数据的存储**
- **然后在每次请求的过程中将ytoken放在headers上增加Authorization属性**

**请求函数**

```ts
import axios from 'axios'

// 进行一些全局配置
axios.defaults.baseURL = 'http://127.0.0.1:7001';
axios.defaults.timeout = 5000;
axios.defaults.withCredentials = true; // 让axios发送请求的时候带上cookie

// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    config.headers.Authorization = sessionStorage.getItem('token');
    // 在发送请求之前做些什么
    return config
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error)
})

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error)
})
// 封装自己的get/post方法
export default {
    get: function (path = '', data = {}) {
        return new Promise(function (resolve, reject) {
            axios.get(path, {
                params: data
            })
                .then(function (response) {
                    resolve(response.data)
                })
                .catch(function (error) {
                    reject(error)
                })
        })
    },
    post: function (path = '', data = {}) {
        return new Promise(function (resolve, reject) {
            axios.post(path, data)
                .then(function (response) {
                    resolve(response.data)
                })
                .catch(function (error) {
                    reject(error)
                })
        })
    },
    all: function (list:any[]) {
        return new Promise(function (resolve, reject) {
            axios.all(list)
                .then(axios.spread(function (...result) {
                    // 两个请求现在都执行完成
                    resolve(result)
                }))
                .catch(function (err) {
                    reject(err)
                })
        })
    }
}

```

**前端保存**

```ts
private onSubmit(){
            this.form.validate((flag)=>{
                if(flag){
                    loginUser(this.loginData)
                        .then((data:any)=>{
                            if(data.code === 200){
                                /*
                                cookie: 如果存储的内容体积不大
                                sessionStorage: 如果不需要持久化存储
                                localStorage: 如果体积较大, 又需要持久化存储
                                * */
                                sessionStorage.setItem('token', data.data.token);
                                this.$router.push('/admin');
                            }else{
                                (this as any).$message.error(data.msg);
                            }
                        })
                        .catch((e)=>{
                            (this as any).$message.error(e.message);
                        });
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        };
```

**后端校验**

```ts
public async isLogin(){
        const { ctx } = this;
        const {token} = ctx.query;
        try {
            // 第一个参数: 需要校验的token
            // 第二个参数: 当初加密的密钥
           const user =  jwt.verify(token, this.config.keys)
            ctx.success(user);
        }catch (e) {
            ctx.error(400, e.message);
        }
    }
```

## 前端进行登录权限控制

**使用路由守卫**

```ts
import Vue from 'vue'
import VueRouter, {RouteConfig} from 'vue-router'
import Register from '../views/Register.vue'
import Login from '../views/Login.vue'
import Admin from '../views/Admin.vue'

Vue.use(VueRouter)

const routes: Array<RouteConfig> = [
    {
        path: '/register',
        name: 'Register',
        component: Register
    },
    {
        path: '/login',
        name: 'Login',
        component: Login
    },
    {
        path: '/admin',
        name: 'Admin',
        component: Admin
    }
]

const router = new VueRouter({
    mode: 'history',
    base: process.env.BASE_URL,
    routes
})
// 添加路由守卫, 实现权限控制
router.beforeEach((to, from, next) => {
    // 1.如果访问的是注册或者登录, 那么就放行
    if(to.path === '/login' || to.path === '/register'){
        return next();
    }
    // 2.获取当前的登录状态
    const token = sessionStorage.getItem('token');
    // 3.如果访问的是其它路由地址, 那么就需要判断是否已经登录
    //   如果已经登录, 那么就放行, 如果没有登录, 那么就强制跳转到登录界面
    if(!token){
        return next('/login');
    }
    next();
})
export default router
```

## 权限控制后端

- **首先定义一个中间件判断用户的token是否正确**
- **然后在请求时携带token，后端校验**

**middleware/auth.ts**

```ts
const jwt = require('jsonwebtoken');
module.exports = (options, app) => {
    return async function (ctx, next) {
        // 1.获取需要权限控制的路由地址
        const authUrls = options.authUrls;
        // 2.判断当前请求的路由地址是否需要权限控制
        if(authUrls.includes(ctx.url)){
            // 需要权限控制
            // 3.获取客户端传递过来的JWT令牌
            const token = ctx.get('authorization');
            // 4.判断客户端有没有传递JWT令牌
            if(token){
                try {
                    await jwt.verify(token, app.config.keys);
                    await next();
                }catch (e) {
                    ctx.error(400, '没有权限');
                }
            }else{
                ctx.error(400, '没有权限');
            }
        }else{
            // 不需要权限控制
            next();
        }
    }
};
```

**config.default.ts**

```ts
 // add your egg config in here
  config.middleware = ['auth'];
  config.auth = {
    authUrls: [
        '/users'
    ]
  };
```

## 第三方登录的前端界面

```vue
<template>
	<ul class="auth_box">
    	<li class="iconfont icon-qq" style="color: deepskyblue"></li>
    	<li class="iconfont icon-weixin" style="color: green"></li>
    	<li class="iconfont icon-weibo" style="color: red"></li>
    	<li class="iconfont icon-github" style="color:#000;">
        	<a href="http://127.0.0.1:7001/github"></a>
    	</li>
    </ul>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {ElForm} from 'element-ui/types/form';
    import {loginUser} from '../api/index';

    @Component({
        name: "Login",
        components:{},
    })
    export default class Login extends Vue{
        private loginData = {
            username: '',
            email: '',
            phone: '',
            password: '',
            captcha: '',
            type: 'normal',
            checked: true
        };
    }
</script>

<style lang="scss" scoped>
    .login_container {
        width: 100%;
        height: 100%;
        background: url("../assets/bg.jpg") no-repeat;
        background-size: cover;
        .login_box{
            width: 600px;
            height: 420px;
            background: #fff;
            border-radius: 10px;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            h1{
                text-align: center;
            }
            a{
                text-decoration: none;
            }
            .el-form{
                padding: 0 20px;
                box-sizing: border-box;
            }
            .auth_box{
                list-style: none;
                padding: 0;
                display: flex;
                justify-content: space-around;
                li{
                    font-size: 30px;
                    position: relative;
                    a{
                        display: block;
                        width: 100%;
                        height: 100%;
                        position: absolute;
                        left: 0;
                        top: 0;
                    }
                }
            }
        }
    }
</style>
```

## 第三方登录的流程

```html
<!--
1.什么是三方登录?
三方登录就是通过第三方应用程序的账号密码, 快速的获取用户相关的信息实现登录
例如: QQ登录
点击QQ登录按钮之后,就会要求用户输入QQ的账号和密码
只要用户输入了QQ的账号和密码, 我们就可以拿到用户的QQ信息
我们就可以通过用户的QQ信息来实现注册登录

2.三方登录存在的问题
如果让用户在我们的网站上输入QQ的账号和密码,
那么我们就可以窃取用户的QQ号, 做一些见不得人的事

3.如何解决这个问题?
通过OAuth协议进行授权

4.什么是OAuth协议?(Open Authorization)
OAuth协议为用户资源的授权提供了一个安全的、开放而又简易的标准。
与以往的授权方式不同之处是OAuth的授权不会使第三方触及到用户的帐号信息（如用户名与密码），
即第三方无需使用用户的用户名与密码就可以申请获得该用户资源的授权，
因此OAuth是安全可靠的

5.OAuth授权流程
（1）第三方应用请求资源所有者授权。
（2）资源所有者同意给第三方应用授权。
（3）第三方应用使用步骤2中获得的授权，向授权服务器申请令牌。
（4）授权服务器对第三方应用进行认证并确认无误后，同意发放令牌。
（5）第三方应用使用步骤4中发放的令牌向资源服务器申请获取资源。
（6）资源服务器确认令牌无误后，向第三方应用开放资源访问。
-->
<!--
1.OAth授权实现流程
(1)去到需要接入的网站申请接入
例如: 要实现Github登录, 就去到Github申请接入
https://github.com/settings/applications/new

(2)申请完接入之后会得到一个Id和Secret
例如: 申请完Github登录, 我们会得到
Client ID:018447869437696516f2
Client Secret:a8d79ecf739b43c01014531c579904577931c83f

(3)在自己的网站放上对应的第三方登录按钮

(4)当用户点击登录按钮之后, 按照文档要求带着申请到的id获取登录界面
   - 为什么要带id, 因为需要知道是你有没有接入权限
                   因为要告诉用户是给谁授权
 https://docs.github.com/en/developers/apps/authorizing-oauth-apps

 (5)当用户点击授权按钮之后, 会返回一个code给我们
    - 这个code就是用户同意授权的临时证据

 (6)我们拿着这个临时证据(code)去到授权服务器换取长久令牌
    - 授权服务器会验证用户是否真的同意, 如果同意会返回一个access_token给我们
    - 这个access_token就是长久令牌

 (7)我们拿着长久的令牌去到资源服务器获取授权用户相关的资源
-->
```

## 获取第三方的登录界面

- **首先去到第三方平台进行一个授权申请** 

- **发送get请求到指定的url并且带上一些参数返回登录界面**

```ts
import { Controller } from 'egg';
const queryString = require('querystring');

export default class GithubController extends Controller {
    public async loginView() {
        // 1.获取第三方登录界面
        // 发送get请求到https://github.com/login/oauth/authorize带上一些参数即可
        // client_id: Github可以根据这个client_id判断你有没有申请接入
        //            Github会根据这个client_id查询出对应的应用程序名称, 告诉用户正在给哪个程序授权
        // scope    : 授权范围
        const baseURL = 'https://github.com/login/oauth/authorize';
        const option = {
            client_id: '018447869437696516f2',
            scope: 'user'
        }
        const url = baseURL + '?' + queryString.stringify(option);
        // console.log(url);
        const {ctx} = this;
        ctx.redirect(url);
    }
}
```

## 获取用户信息

- **用户登录之后会跳转到一个自己设置的callback地址并带上一个code**
- **利用code和相关的参数信息，使用egg的curl进行获取令牌(access_token)的请求的发送**
- **拿着令牌去资源服务器获取数据**
- **返回的数据是buffer数据，再通过JSON.parse进行转换**

```ts
import { Controller } from 'egg';
const queryString = require('querystring');

export default class GithubController extends Controller {
    public async getLoginView() {
        // 1.获取第三方登录界面
        const baseURL = 'https://github.com/login/oauth/authorize';
        const option = {
            client_id: '018447869437696516f2',
            scope: 'user'
        }
        const url = baseURL + '?' + queryString.stringify(option);
        const {ctx} = this;
        ctx.redirect(url);
    }
    public async getAccessToken(){
        const {ctx} = this;
        // 1.拿到用户同意授权之后的code
        const {code} = ctx.query;
        // 2.利用code换取令牌(access_token)
        // 发送POST请求到https://github.com/login/oauth/access_token带上必要的参数
        const baseURL = 'https://github.com/login/oauth/access_token';
        const option = {
            client_id:'018447869437696516f2',
            client_secret:'a8d79ecf739b43c01014531c579904577931c83f',
            code:code
        }
        const result = await ctx.curl(baseURL, {
            method: 'POST',
            data: option,
            dataType: 'json',
            headers:{
                'Content-Type': 'application/json',
                'Accept':'application/json'
            }
        });
        const accessToken = result.data.access_token;
        // 3.拿着令牌去资源服务器获取数据
        this.getGithubUserIinfo(accessToken);
    }
    private async getGithubUserIinfo(accessToken){
        const {ctx} = this;
        const baseURL = 'https://api.github.com/user';
        const url = `${baseURL}?access_token=${accessToken}`;
        const result = await ctx.curl(url, {
            method: 'GET'
        });
        console.log(JSON.parse(result.data));
    }
}
```

## 第三方登录的BUG

- **在eggjs中进行权限控制的时候我们使用了一个自定义的全局中间件，但是中间件中有next(),如果next()执行的是一个异步函数，则会报404的错误**
- **在next()前面加上await即可**

```ts
const jwt = require('jsonwebtoken');
module.exports = (options, app) => {
    return async function (ctx, next) {
        // 1.获取需要权限控制的路由地址
        const authUrls = options.authUrls;
        // 2.判断当前请求的路由地址是否需要权限控制
        if(authUrls.includes(ctx.url)){
            // 需要权限控制
            // 3.获取客户端传递过来的JWT令牌
            const token = ctx.get('authorization');
            // 4.判断客户端有没有传递JWT令牌
            if(token){
                try {
                    await jwt.verify(token, app.config.keys);
                    await next();
                }catch (e) {
                    ctx.error(400, '没有权限');
                }
            }else{
                ctx.error(400, '没有权限');
            }
        }else{
            // 不需要权限控制
            await next();
        }
    }
};
```

## 数据库的设计

- **将用户的表进行更改，添加一个github字段，表示是否跟github账号进行一个关联**
- **添加一个auth的表，里面存储的是第三方平台登录的账户信息**

```ts
'use strict';
import {QueryInterface} from 'sequelize';

module.exports = {
  // 在执行数据库升级时调用的函数，创建 users 表
  up: async (queryInterface:QueryInterface, Sequelize) => {
    const { INTEGER, DATE, STRING } = Sequelize;
    await queryInterface.createTable('users', {
      id: {
        type: INTEGER,
        primaryKey: true,
        autoIncrement: true
      },
      username: {
        type:STRING(255),
        allowNull:true,
        unique:true,
      },
      email:{
        type:STRING(255),
        allowNull:true,
        unique:true
      },
      phone:{
        type:STRING(255),
        allowNull:true,
        unique:true
      },
      password: {
        type: STRING(255), // varchar(255)
        allowNull: false,
        unique: false,
      },
      created_at: {
        type: DATE
      },
      updated_at: {
        type: DATE
      },
    });
  },
  // 在执行数据库降级时调用的函数，删除 users 表
  down: async (queryInterface:QueryInterface) => {
    await queryInterface.dropTable('users');
  },
};

//auth
'use strict';
import {QueryInterface} from 'sequelize';

module.exports = {
    // 在执行数据库升级时调用的函数，创建 users 表
    up: async (queryInterface:QueryInterface, Sequelize) => {
        const { INTEGER, DATE, STRING } = Sequelize;
        await queryInterface.createTable('oauths', {
            id: {
                type: INTEGER,
                primaryKey: true,
                autoIncrement: true
            },
            access_token: { // 保存授权之后拿到的令牌
                type:STRING(255),
                allowNull:false,
            },
            provider:{  // 保存是哪个平台授权的QQ/Weixin/Weibo/Github
                type:STRING(255),
                allowNull:false,
            },
            uid:{   // 第三方平台返回的用户id
                type:INTEGER,
                allowNull:false,
                unique:true
            },
            user_id  : {    // 本地绑定用户的id
                type: INTEGER, // varchar(255)
                allowNull: false,
                unique: false,
                references:{
                    model:'users',
                    key:'id'
                }
            },
            created_at: {
                type: DATE
            },
            updated_at: {
                type: DATE
            },
        });
    },
    // 在执行数据库降级时调用的函数，删除 users 表
    down: async (queryInterface:QueryInterface) => {
        await queryInterface.dropTable('oauths');
    },
};
```

## sequelize-typescript和egg-sequelize-ts插件的不兼容问题

- **首先定义user和oauth表的一对多关系，根据sequelize的语法**
- **然后报错**
- **解决之后进行关联查询的测试**

```ts
/**
 * @desc 用户表
 */
import { Column, DataType, Model, Table, CreatedAt, UpdatedAt, HasMany} from 'sequelize-typescript';
import {OAuth} from './oauth';
/*
1.按照sequelize-typescript官方给定的方式建立完表与表之间的关系之后, 编译报错
报错的原因是因为我们并没有直接使用sequelize-typescript, 而是借助了egg-sequelize-ts这个插件来使用sequelize-typescript
也正是因为如此, 所以问题的原因在于这个插件有问题
2.问题的原因
在egg-sequelize-ts插件中, 它是根据 "sequelize-typescript": "^0.6.6"版本来编写的
而在我们的项目中, 我们使用的sequelize-typescript的版本是 1.1.0
所以正是因为如此, 导致了不兼容问题
* */
@Table({
    modelName: 'user'
})
export class User extends Model<User> {

    @Column({
        type: DataType.INTEGER,
        primaryKey: true,
        autoIncrement: true,
        unique: true,
        allowNull: false,
        comment: '用户ID',
    })
    id: number;

    @Column({
        type:DataType.STRING(255),
        allowNull:true,
        unique:true,
        comment: '用户姓名',
        validate:{
            is: /^[A-Za-z0-9]{6,}$/
        }
    })
    username: string;

    @Column({
        type:DataType.STRING(255),
        allowNull:true,
        unique:true,
        comment: '用户邮箱',
        validate:{
            isEmail:true
        }
    })
    email: string;

    @Column({
        type:DataType.STRING(255),
        allowNull:true,
        unique:true,
        comment: '用户手机',
        validate:{
            is:/^1[3456789]\d{9}$/
        }
    })
    phone: string;

    @Column({
        type:DataType.STRING(255),
        allowNull:false,
        unique:true,
        comment: '用户密码',
    })
    password: string;

    @Column({
        type:DataType.INTEGER,
        allowNull:true,
        unique:false,
        comment: '是否绑定授权账户',
    })
    github: number;

    @HasMany(()=>OAuth)
    oauths:OAuth[];

    @CreatedAt
    createdAt: Date;

    @UpdatedAt
    updatedAt: Date;
};
export default () => User;
```

```ts
/**
 * @desc 授权表
 */
import { Column, DataType, Model, Table, CreatedAt, UpdatedAt, ForeignKey, BelongsTo} from 'sequelize-typescript';
import {User} from './user'

@Table({
    modelName: 'oauth'
})
export class OAuth extends Model<OAuth> {

    @Column({
        type: DataType.INTEGER,
        primaryKey: true,
        autoIncrement: true,
        unique: true,
        allowNull: false,
        comment: '用户ID',
    })
    id: number;

    @Column({
        field:'access_token',
        type:DataType.STRING(255),
        allowNull:false,
        comment: '授权令牌',
    })
    accessToken: string;

    @Column({
        type:DataType.STRING(255),
        allowNull:false,
        comment: '授权来源',
    })
    provider: string;

    @Column({
        type:DataType.INTEGER,
        allowNull:false,
        unique:true,
        comment: '三方平台用户id',
    })
    uid: number;

    @ForeignKey(()=>User)
    @Column({
        field:'user_id',
        type: DataType.INTEGER,
        allowNull:false,
        unique:true,
    })
    userId: number;

    @BelongsTo(()=>User)
    user:User;

    @CreatedAt
    createdAt: Date;

    @UpdatedAt
    updatedAt: Date;
};
export default () => OAuth;
```

**关联查询的测试**

```ts
import { Controller } from 'egg';
import {User} from '../model/user';

export default class HomeController extends Controller {
  public async index() {
    const { ctx } = this;
    const data = await ctx.model.Oauth.findOne({
      where:{
        id:1
      },
      include:[
        {model:User}
      ]
    })
    console.log(data!.dataValues);
  }
}
```

## 直接登录

- **首先拿到github返回的用户数据之后，先去本地的数据库进行一个搜索**
- **如果有结果，则设置前端的cookie**
- **然后后端利用redirect直接跳转到admin路由**
- **前端使用js-cookie插件进行cookie的获取**

**controller/github.ts**

```ts
import { Controller } from 'egg';
const queryString = require('querystring');
const jwt = require('jsonwebtoken');

export default class GithubController extends Controller {
    public async getLoginView() {
        // 1.获取第三方登录界面
        const baseURL = 'https://github.com/login/oauth/authorize';
        const option = {
            client_id: '018447869437696516f2',
            scope: 'user'
        }
        const url = baseURL + '?' + queryString.stringify(option);
        const {ctx} = this;
        ctx.redirect(url);
    }
    public async getAccessToken(){
        const {ctx} = this;
        // 1.拿到用户同意授权之后的code
        const {code} = ctx.query;
        // 2.利用code换取令牌(access_token)
        // 发送POST请求到https://github.com/login/oauth/access_token带上必要的参数
        const baseURL = 'https://github.com/login/oauth/access_token';
        const option = {
            client_id:'018447869437696516f2',
            client_secret:'a8d79ecf739b43c01014531c579904577931c83f',
            code:code
        }
        const result = await ctx.curl(baseURL, {
            method: 'POST',
            data: option,
            dataType: 'json',
            headers:{
                'Content-Type': 'application/json',
                'Accept':'application/json'
            }
        });
        const accessToken = result.data.access_token;
        // 3.拿着令牌去资源服务器获取数据
        await this.getGithubUserIinfo(accessToken);
    }
    private async getGithubUserIinfo(accessToken){
        // 1.获取用户信息
        const {ctx} = this;
        const baseURL = 'https://api.github.com/user';
        const url = `${baseURL}?access_token=${accessToken}`;
        const result = await ctx.curl(url, {
            method: 'GET'
        });
        // console.log(JSON.parse(result.data));
        // ctx.body = 'hello';
        const data = JSON.parse(result.data);
        data.provider = 'github';
        await this.go2Admin(data);
    }
    private async go2Admin(data){
        const {ctx} = this;
        try {
            // 用户存在直接登录
            const user = await ctx.service.oauth.getUser(data);
            const token = jwt.sign(user, this.config.keys, {expiresIn: '7 days'});
            ctx.cookies.set('token', token, {
                path:'/',
                maxAge: 24 * 60 * 60 * 1000,
                // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
                httpOnly: false,
            });
            ctx.redirect('http://127.0.0.1:8080/admin');
        }catch (e) {
            // 用户不存在, 先注册再登录
        }

    }
}
```

**service/oauth.ts**

```ts
import { Service } from 'egg';
import {User} from "../model/user";

export default class Oauth extends Service {


    public async getUser({id, provider}) {
        const {ctx} = this;
        const data = await ctx.model.Oauth.findOne({
            where:{
                uid:id,
                provider:provider
            },
            include:[
                {model:User}
            ]
        });
        try {
            return data!.dataValues.user!.dataValues;
        }catch (e) {
            throw new Error('授权用户不存在');
        }
    }
}
```

- **修改前端的登录逻辑，不在sessionStorage中进行判断，而是在cookie中进行判断**

```ts
import axios from 'axios'

// 进行一些全局配置
axios.defaults.baseURL = 'http://127.0.0.1:7001';
axios.defaults.timeout = 5000;
axios.defaults.withCredentials = true; // 让axios发送请求的时候带上cookie

// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // config.headers.Authorization = sessionStorage.getItem('token');
    // 在发送请求之前做些什么
    return config
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error)
})

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error)
})
// 封装自己的get/post方法
export default {
    get: function (path = '', data = {}) {
        return new Promise(function (resolve, reject) {
            axios.get(path, {
                params: data
            })
                .then(function (response) {
                    resolve(response.data)
                })
                .catch(function (error) {
                    reject(error)
                })
        })
    },
    post: function (path = '', data = {}) {
        return new Promise(function (resolve, reject) {
            axios.post(path, data)
                .then(function (response) {
                    resolve(response.data)
                })
                .catch(function (error) {
                    reject(error)
                })
        })
    },
    all: function (list:any[]) {
        return new Promise(function (resolve, reject) {
            axios.all(list)
                .then(axios.spread(function (...result) {
                    // 两个请求现在都执行完成
                    resolve(result)
                }))
                .catch(function (err) {
                    reject(err)
                })
        })
    }
}
```

## 先注册再登录上

- **使用uuid生成独一无二的随机的用户名(npm install uuid npm install @types/uuid)**
- **密码指定一个初始密码即可，但是会碰到校验问题，先手动忽略校验问题**
- **创建用户**

```ts
private async go2Admin(data){
    const {ctx} = this;
    try {
        // 用户存在直接登录
        const user = await ctx.service.oauth.getUser(data);
        const token = jwt.sign(user, this.config.keys, {expiresIn: '7 days'});
        ctx.cookies.set('token', token, {
            path:'/',
            maxAge: 24 * 60 * 60 * 1000,
            // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
            httpOnly: false,
        });
        ctx.redirect('http://127.0.0.1:8080/admin');
    }catch (e) {
        // 用户不存在, 先注册再登录
        // 1.创建一个用户
        // ctx.body = uuidv4(); // dd171f66-f49d-4466-8884-f3f23746f643
        const data = {
            username: uuidv4() ,
            email:undefined, phone
            :undefined,
            password:'com.123456'
        }
        const user = await ctx.service.user.createUser(data);
        ctx.body = user;
        // 2.保存这个用户的授权信息
        // 3.直接登录(跳转到admin界面)
    }
}
```

## 先注册再登录下

- **在数据库中的github字段创建的时候为1**

- **更改用户的创建方法**

```ts
import { Service } from 'egg';

/**
 * Test Service
 */
export default class User extends Service {
    public async getUser({username, email, phone, password}){
        password = this.ctx.helper.encryptText(password);
        let res;
        if(email){
            res = await this.findUser({email:email, password:password});
        }else if(phone){
            res = await this.findUser({phone:phone, password:password});
        }else if(username){
            res = await this.findUser({username:username, password:password});
        }
        try {
            return res.dataValues;
        }catch (e) {
            throw new Error('用户名或者密码不正确');
        }
    }
    public async createUser(obj) {
        const {username, email, phone, password} = obj;
        obj.password = this.ctx.helper.encryptText(password);
        if(username){
            // 普通注册
            return await this.createUserByUserName(username, obj);
        }else if(email){
            // 邮箱注册
            return await this.createUserByEmail(email, obj);
        }else if(phone){
            // 手机注册
            return await this.createUserByPhone(phone, obj);
        }
    }
    private async createUserByUserName(username, obj){
        // 1.查询当前用户是否存在
        const user = await this.findUser({username:username});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create(obj);
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
    private async createUserByEmail(email, obj){
        // 1.查询当前用户是否存在
        const user = await this.findUser({email:email});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create(obj);
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
    private async createUserByPhone(phone, obj){
        // 1.查询当前用户是否存在
        const user = await this.findUser({phone:phone});
        if(user){
            throw new Error('当前用户已存在');
        }
        // 2.如果不存在才保存
        const data = await this.ctx.model.User.create(obj);
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
    private async findUser(options){
        return await this.ctx.model.User.findOne({where: options});
    }
}
```

## 先注册再登录下

- **先进行用户的注册**
- **然后进行oauth的注册**
- **最后设置token并跳转到admin界面中**

```ts
private async go2Admin(data, accessToken){
    const {ctx} = this;
    try {
        // 用户存在直接登录
        const user = await ctx.service.oauth.getOAuthUser(data);
        const token = jwt.sign(user, this.config.keys, {expiresIn: '7 days'});
        ctx.cookies.set('token', token, {
            path:'/',
            maxAge: 24 * 60 * 60 * 1000,
            // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
            httpOnly: false,
        });
        ctx.redirect('http://127.0.0.1:8080/admin');
    }catch (e) {
        // 用户不存在, 先注册再登录
        // 1.创建一个用户
        const userInfo = {
            username: uuidv4() ,
            password:'com.123456',
            github: 1
        };
        const user = await ctx.service.user.createUser(userInfo);
        // 2.保存这个用户的授权信息
        const oauthInfo = {
            accessToken: accessToken,
            provider: data.provider,
            uid:data.id,
            userId:user ? user.id : -1
        }
        await ctx.service.oauth.createOAuth(oauthInfo);
        // 3.直接登录(跳转到admin界面)
        const token = jwt.sign(user, this.config.keys, {expiresIn: '7 days'});
        ctx.cookies.set('token', token, {
            path:'/',
            maxAge: 24 * 60 * 60 * 1000,
            // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
            httpOnly: false,
        });
        ctx.redirect('http://127.0.0.1:8080/admin');
    }
}
```

## 三方登录-passport鉴权

- **安装插件:**

```ts
npm i --save egg-passport
npm i --save egg-passport-github
```

- **开启插件并进行配置(Eggjs官网文档)**

```ts
创建一个 GitHub OAuth Apps，得到 clientID 和 clientSecret 信息。
填写 callbackURL，如 http://127.0.0.1:7001/passport/github/callback
线上部署时需要更新为对应的域名
路径为配置的 options.callbackURL，默认为 /passport/${strategy}/callback
如应用部署在 Nginx/HAProxy 之后，需设置插件 proxy 选项为 true, 并检查以下配置：
代理附加 HTTP 头字段：x-forwarded-proto 与 x-forwarded-host
配置中 config.proxy 应设置为 true
```

- **挂载路由**

```ts
// const github = app.passport.authenticate('github', {});
// router.get('/passport/github', github);
// router.get('/passport/github/callback', github);
  const github = (app as any).passport.authenticate('github', {
    successRedirect: 'http://127.0.0.1:8080/admin'
  });
  router.get('/passport/github', github);
  router.get('/passport/github/callback', github);
```

- **在app.js中处理逻辑**

```ts
const jwt = require('jsonwebtoken');
import { v4 as uuidv4 } from 'uuid';

// app.js
module.exports = app => {
    app.passport.verify(async (ctx, user) => {
        // 从数据库中查找用户信息
        try {
            const existsUser = await ctx.service.oauth.getOAuthUser(user);
            const token = jwt.sign(existsUser, app.config.keys, {expiresIn: '7 days'});
            ctx.cookies.set('token', token, {
                path:'/',
                maxAge: 24 * 60 * 60 * 1000,
                // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
                httpOnly: false,
                signed:false,
            });
            return existsUser;
        }catch (e) {
            const userInfo = {
                username: uuidv4() ,
                password:'com.123456',
                github: 1
            };
            const newUser = await ctx.service.user.createUser(userInfo);
            const oauthInfo = {
                accessToken: user.accessToken,
                provider: user.provider,
                uid:user.id,
                userId:newUser ? newUser.id : -1
            };
            await ctx.service.oauth.createOAuth(oauthInfo);
            const token = jwt.sign(newUser, app.config.keys, {expiresIn: '7 days'});
            ctx.cookies.set('token', token, {
                path:'/',
                maxAge: 24 * 60 * 60 * 1000,
                // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
                httpOnly: false,
                signed:false,
            });
            return newUser;
        }
    });
};
```

## 三方登录的完结bug

- **服务端设置cookie的时候没有加签，则在获取的时候也必须指定不加签，否则会获取不到cookie中的信息**

```ts
const jwt = require('jsonwebtoken');
module.exports = (options, app) => {
    return async function (ctx, next) {
        // 1.获取需要权限控制的路由地址
        const authUrls = options.authUrls;
        // 2.判断当前请求的路由地址是否需要权限控制
        if(authUrls.includes(ctx.url)){
            // 需要权限控制
            // 3.获取客户端传递过来的JWT令牌
            // 注意点: 如果设置cookie的时候没有签名, 那么获取的时候也要告诉egg不需要签名, 否则会获取不到
            const token = ctx.cookies.get('token', {
                signed: false,
            });
            // 4.判断客户端有没有传递JWT令牌
            if(token){
                try {
                    await jwt.verify(token, app.config.keys);
                    await next();
                }catch (e) {
                    ctx.error(400, '没有权限');
                }
            }else{
                ctx.error(400, '没有权限');
            }
        }else{
            // 不需要权限控制
            await next();
        }
    }
};
```

## 补充知识

**平时在我们登录网站时，网站需要有一种手段，能够验证登录这的身份。常用的手段有2种，分别是签名cookie和令牌cookie。**

**签名cookie：网站可以在cookie中放置用户名，最近一次登录时间，以及其他有用的信息。除此之外，cookie中还会包含一个签名。网站收到cookie后，会对cookie中的信息和签名进行校验，依此来判断cookie中的信息有没有被修改过。**

**令牌cookie：网站会在每个登录用户的cookie中，加入一些随机字节，作为该用户的令牌。网站会记录令牌与用于直接的对应关系。当收到cookie后，网站会检查该用户是否已经登录。用户cookie中的令牌会定期更新，以增强安全性。**

## 前端-头部搭建

- **使用container布局即可**

```vue
<template>
    <el-container>
        <el-header>
            <div class="header-left"></div>
            <div class="header-right">
                <img src="../assets/lnj.jpg" alt="">
                <p>极客江南</p>
                <el-button>退出</el-button>
            </div>
        </el-header>
        <el-container>
            <el-aside width="200px">Aside</el-aside>
            <el-main>Main</el-main>
        </el-container>
    </el-container>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import {getUsers} from '../api/index';

    @Component({
        name: "Admin",
        components:{},
    })
    export default class Admin extends Vue{
        
    }
</script>

<style lang="scss" scoped>
.el-container{
    background: #ccc;
    width: 100%;
    height: 100%;
    .el-header{
        background: deepskyblue;
        display: flex;
        justify-content: space-between;
        .header-left{
            width: 200px;
            height: 60px;
            background: url("../assets/logo.png") center center no-repeat;
            background-size: 80% auto;
        }
        .header-right{
            display: flex;
            justify-content: space-between;
            align-items: center;
            img{
                width: 40px;
                height: 40px;
                border-radius: 50%;
                overflow: hidden;
            }
            p{
                padding-left: 10px;
                padding-right: 20px;
            }
        }
    }
    .el-aside{
        background: #fff;
    }
}
</style>
```

## 退出登录

- **绑定一个方法，点击清除cookie并且跳转到登录界面**

```vue
<template>
    <el-container>
        <el-header>
            <div class="header-left"></div>
            <div class="header-right">
                <img src="../assets/lnj.jpg" alt="">
                <p>极客江南</p>
                <el-button @click="logout">退出</el-button>
            </div>
        </el-header>
        <el-container>
            <el-aside width="200px">Aside</el-aside>
            <el-main>Main</el-main>
        </el-container>
    </el-container>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import Cookies from 'js-cookie'

    @Component({
        name: "Admin",
        components:{},
    })
    export default class Admin extends Vue{
        private logout(){
            Cookies.remove('token');
            this.$router.push('/login');
        }
    }
</script>
```

## 折叠菜单

- **使用elementui的导航菜单**
- **设置水平折叠**
- **设置路由跳转**

```vue
<template>
    <el-container>
        <el-header>
            <div class="header-left" @click="toggleCollapse"></div>
            <div class="header-right">
                <img src="../assets/lnj.jpg" alt="">
                <p>极客江南</p>
                <el-button @click="logout">退出</el-button>
            </div>
        </el-header>
        <el-container>
            <el-aside :width="isCollapse ? '65px' : '200px'">
                <!--垂直侧边栏-->
                <el-menu
                        default-active="2"
                        class="el-menu-vertical-demo"
                        background-color="#fff"
                        text-color="#666"
                        active-text-color="deepskyblue"
                        :collapse="isCollapse"
                        :collapse-transition="false"
                        :router="true">
                    <!--一级菜单-->
                    <el-submenu index="1">
                        <template slot="title">
                            <i class="el-icon-setting"></i>
                            <span>用户管理</span>
                        </template>
                        <!--二级菜单-->
                        <el-menu-item-group>
                            <el-menu-item index="/users">
                                <template slot="title">
                                    <i class="el-icon-user"></i>
                                    <span>用户列表</span>
                                </template>
                            </el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>
                    <!--一级菜单-->
                    <el-submenu index="2">
                        <template slot="title">
                            <i class="el-icon-collection"></i>
                            <span>权限管理</span>
                        </template>
                        <!--二级菜单-->
                        <el-menu-item-group>
                            <el-menu-item index="/roles">
                                <template slot="title">
                                    <i class="el-icon-view"></i>
                                    <span>角色列表</span>
                                </template>
                            </el-menu-item>
                            <el-menu-item index="/rights">
                                <template slot="title">
                                    <i class="el-icon-unlock"></i>
                                    <span>权限列表</span>
                                </template>
                            </el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <el-main>
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import Cookies from 'js-cookie'

    @Component({
        name: "Admin",
        components:{},
    })
    export default class Admin extends Vue{
        private isCollapse = false;
        private toggleCollapse(){
            this.isCollapse = !this.isCollapse;
        }
        private logout(){
            Cookies.remove('token');
            this.$router.push('/login');
        }
    }
</script>
```

## 折叠菜单的优化（使用v-for生成菜单）

- **创建一个菜单的config，根据v-for指令进行渲染**
- **将选中的菜单的路径保存到sessionStorage**

```vue
<template>
    <el-container>
        <el-header>
            <div class="header-left" @click="toggleCollapse"></div>
            <div class="header-right">
                <img src="../assets/lnj.jpg" alt="">
                <p>极客江南</p>
                <el-button @click="logout">退出</el-button>
            </div>
        </el-header>
        <el-container>
            <el-aside :width="isCollapse ? '65px' : '200px'">
                <!--垂直侧边栏-->
                <el-menu
                        default-active="2"
                        class="el-menu-vertical-demo"
                        background-color="#fff"
                        text-color="#666"
                        active-text-color="deepskyblue"
                        :collapse="isCollapse"
                        :collapse-transition="false"
                        :router="true"
                        :default-active="defaultActivePath">
                    <!--一级菜单-->
                    <el-submenu :index="item.menuName"
                                v-for="item in menus"
                                :key="item.menuName">
                        <template slot="title">
                            <i :class="item.icon"></i>
                            <span>{{item.menuName}}</span>
                        </template>
                        <!--二级菜单-->
                        <el-menu-item-group>
                            <el-menu-item :index="subItem.path"
                                          v-for="subItem in item.children"
                                          :key="subItem.menuName"
                                          @click="changeDefaultActivePath(subItem.path)">
                                <template slot="title">
                                    <i :class="subItem.icon"></i>
                                    <span>{{subItem.menuName}}</span>
                                </template>
                            </el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>
                </el-menu>
            </el-aside>
            <el-main>
                <router-view></router-view>
            </el-main>
        </el-container>
    </el-container>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import Cookies from 'js-cookie'

    @Component({
        name: "Admin",
        components:{},
    })
    export default class Admin extends Vue{
        private defaultActivePath = '';
        private menus = [
            {
                menuName:'用户管理',
                path: '',
                icon: 'el-icon-setting',
                children:[
                    {menuName:'用户列表',path: '/users', icon:'el-icon-user', children:[]}
                ]
            },
            {
                menuName:'权限管理',
                path: '',
                icon:'el-icon-collection',
                children:[
                    {menuName:'角色列表',path: '/roles', icon:'el-icon-view',children:[]},
                    {menuName:'权限列表',path: '/rights', icon:'el-icon-unlock',children:[]}
                ]
            }
        ]
        private isCollapse = false;
        private toggleCollapse(){
            this.isCollapse = !this.isCollapse;
        }
        private logout(){
            Cookies.remove('token');
            this.$router.push('/login');
        }
        private changeDefaultActivePath(path:string){
            this.defaultActivePath = path;
            sessionStorage.setItem('acitvePath', path);
        }
        created(): void {
            const path = sessionStorage.getItem('acitvePath');
            this.defaultActivePath = path ? path : '';
        }
    }
</script>
```

## 用户管理模块的结构搭建

```vue
<template>
    <div>
        <el-breadcrumb separator="/">
            <el-breadcrumb-item><a href="/admin" @click="resetDefaultActivePath">首页</a></el-breadcrumb-item>
            <el-breadcrumb-item>用户管理</el-breadcrumb-item>
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>

        <el-card class="box-card">
            卡片区域
        </el-card>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        }
    }
</script>

<style lang="scss" scoped>
.el-breadcrumb{
    padding-bottom: 20px;
}
</style>
```

## 用户管理头部搜索区域的搭建

```vue
<template>
    <div>
        <!--面包屑导航-->
        <el-breadcrumb separator="/">
            <el-breadcrumb-item><a href="/admin" @click="resetDefaultActivePath">首页</a></el-breadcrumb-item>
            <el-breadcrumb-item>用户管理</el-breadcrumb-item>
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>
        <!--内容卡片区域-->
        <el-card class="box-card">
            <el-row>
                <el-col :span="20">
                    <el-form :inline="true" :model="searchData" class="demo-form-inline">
                        <el-form-item label="">
                            <el-select v-model="searchData.role" placeholder="-所有角色-">
                                <el-option label="管理员" value="manger"></el-option>
                                <el-option label="普通用户" value="normal"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.origin" placeholder="-所有来源-">
                                <el-option label="本地注册" value="local"></el-option>
                                <el-option label="Github登录" value="github"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.type" placeholder="-所有用户-">
                                <el-option label="用户名" value="username"></el-option>
                                <el-option label="邮箱" value="email"></el-option>
                                <el-option label="手机" value="phone"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-input v-model="searchData.key" placeholder="关键字"></el-input>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="onSubmit">查询</el-button>
                            <el-button type="primary" @click="exportUsers">导出搜索结果</el-button>
                        </el-form-item>
                    </el-form>
                </el-col>
                <el-col :span="4">
                    <el-button type="primary" @click="addUser">添加用户</el-button>
                    <el-button type="primary" @click="importUsers">导入用户</el-button>
                </el-col>
            </el-row>
        </el-card>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{
        private searchData = {
            role:'',
            origin:'',
            type:'',
            key:''
        };
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        };
        private onSubmit(){

        };
        private exportUsers(){

        };
        private addUser(){

        };
        private importUsers(){

        };
    }
</script>

<style lang="scss" scoped>
.el-breadcrumb{
    padding-bottom: 20px;
}
</style>
```

## 中间表格区域的搭建

**作用域插槽的使用**

```vue
<template>
    <div>
        <!--面包屑导航-->
        <el-breadcrumb separator="/">
            <el-breadcrumb-item><a href="/admin" @click="resetDefaultActivePath">首页</a></el-breadcrumb-item>
            <el-breadcrumb-item>用户管理</el-breadcrumb-item>
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>
        <!--内容卡片区域-->
        <el-card class="box-card">
            <!--头部搜索区域-->
            <el-row>
                <el-col :span="20">
                    <el-form :inline="true" :model="searchData" class="demo-form-inline">
                        <el-form-item label="">
                            <el-select v-model="searchData.role" placeholder="-所有角色-">
                                <el-option label="管理员" value="manger"></el-option>
                                <el-option label="普通用户" value="normal"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.origin" placeholder="-所有来源-">
                                <el-option label="本地注册" value="local"></el-option>
                                <el-option label="Github登录" value="github"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.type" placeholder="-所有用户-">
                                <el-option label="用户名" value="username"></el-option>
                                <el-option label="邮箱" value="email"></el-option>
                                <el-option label="手机" value="phone"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-input v-model="searchData.key" placeholder="关键字"></el-input>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="onSubmit">查询</el-button>
                            <el-button type="primary" @click="exportUsers">导出搜索结果</el-button>
                        </el-form-item>
                    </el-form>
                </el-col>
                <el-col :span="4">
                    <el-button type="primary" @click="addUser">添加用户</el-button>
                    <el-button type="primary" @click="importUsers">导入用户</el-button>
                </el-col>
            </el-row>
            <!--中间表格区域-->
            <el-table
                    :data="tableData"
                    style="width: 100%"
                    :border="true"
                    :stripe="true">
                <el-table-column type="index">
                </el-table-column>
                <el-table-column
                        prop="username"
                        label="姓名">
                </el-table-column>
                <el-table-column
                        prop="email"
                        label="邮箱">
                </el-table-column>
                <el-table-column
                        prop="phone"
                        label="电话">
                </el-table-column>
                <el-table-column
                        prop="roleName"
                        label="角色">
                </el-table-column>
                <el-table-column label="状态">
                    <template slot-scope="scope">
                        <!-- {{scope.row.userState}}-->
                        <el-switch
                                v-model="scope.row.userState"
                                active-color="#13ce66"
                                inactive-color="#ff4949">
                        </el-switch>
                    </template>
                </el-table-column>
                <el-table-column label="操作">
                    <template slot-scope="scope">
                        <el-button type="primary" icon="el-icon-edit"></el-button>
                        <el-button type="danger" icon="el-icon-delete"></el-button>
                        <el-button type="warning" icon="el-icon-setting"></el-button>
                    </template>
                </el-table-column>
            </el-table>
        </el-card>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{
        private tableData = [
            {
                username:'jonathan',
                email:'97606813@qq.com',
                phone:'17301727164',
                roleName:'超级管理员',
                avatarURL: '',
                userState: true
            },
            {
                username:'it666',
                email:'97606814@qq.com',
                phone:'13554499311',
                roleName:'普通用户',
                avatarURL: '',
                userState: false
            }
        ]
        private searchData = {
            role:'',
            origin:'',
            type:'',
            key:''
        };
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        };
        private onSubmit(){

        };
        private exportUsers(){

        };
        private addUser(){

        };
        private importUsers(){

        };
    }
</script>

<style lang="scss" scoped>
.el-breadcrumb{
    padding-bottom: 20px;
}
</style>
```

## 底部分页区域

```vue
<template>
    <div>
        <!--底部分页区域-->
        <el-pagination
                :page-sizes="[100, 200, 300, 400]"
                :page-size="100"
                layout="total, sizes, prev, pager, next, jumper"
                :total="400">
        </el-pagination>
    </div>
</template>
```

## RESTfulAPI

```html
<!--
1.什么是Restful API?
Restful API它是rest式的接口，所以就要先了解什么是rest

2.什么是rest?
rest 不是一个技术，也不是一个协议
rest 是一组架构约束的条件和原则，满足这些约束和原则的设计就是 Rest
在REST规则中，有两个基础概念：对象、行为

3.什么是对象和行为
对象就是我们要操作的内容，例如要操作用户，那么用户就是对象
行为有4种常用的：查看、创建、编辑、删除
rest的提出者很巧妙的利用http现有方法来对应这4种行为：
GET - 查看
POST - 创建
PUT - 编辑
DELETE - 删除

4.如何定义Restful API?
4.1动宾结构
https://baike.baidu.com/item/%E5%8A%A8%E5%AE%BE%E7%BB%93%E6%9E%84/1879958?fr=aladdin
- 动词: GET/POST/PUT/DELETE
- 宾语: user/account/...
- 组合: getUser/postUser/putUser/deleteUser
和传统API设计不同的是在Restful API动词使用不同的请求方式来代替
例如:
- 查看用户
  + 过去 http://www.it666.com/getUser
  + GET  http://www.it666.com/users
- 新增用户
  + 过去 http://www.it666.com/addUser
  + POST http://www.it666.com/users
- 更新用户
  + 过去 http://www.it666.com/updateUser
  + PUT http://www.it666.com/users/123
- 删除用户
  + 过去 http://www.it666.com/deleteUser
  + DELETE http://www.it666.com/users/123

4.2宾语尽量使用复数
- 不推荐:http://www.it666.com/user
- 推荐:  http://www.it666.com/users

4.3尽量避免多级
- 不推荐:http://www.it666.com/users/123/3
- 推荐:  http://www.it666.com/users/123?category=3

4.4最好在接口路径中添加接口版本号
- http://www.it666.com/v2/users/123?category=3

5.Restful API返回状态
通过http状态码直接表示操作状态
1xx:临时响应
2xx:操作成功
3xx:重定向
4xx:客户端错误
5xx:服务端错误

6.为什么需要Restful API?
- 过去基本都是PC端网页, 过去项目基本都是混合开发(SSR服务端渲染),
  过去我们请求一个网址之后, 服务端会根据我们的请求在服务端处理完一系列的业务逻辑
  然后再将服务端渲染好的网页返回给我们.
- 但随着移动端越来越重要，如果再采用这种模式只会让服务端变得越来越复杂,
  所以就有了前后端分离, 就有了CSR, 所以为了规范统一为了降低沟通成本就需要一套API规范
- RESTful 是目前最流行的 API 设计规范，用于 Web 数据接口的设计。
  通过RESTful API就可以通过一套统一的接口为所有客户端提供web服务，实现前后端分离
-->
<!--
1xx （临时响应）表示临时响应并需要请求者继续执行操作的状态代码。
100 （继续） 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。
101 （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。
102 由WebDAV（RFC 2518）扩展的状态码，代表处理将被继续执行。

2xx （成功）表示成功处理了请求的状态代码。
200 （成功） 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。
201 （已创建） 请求成功并且服务器创建了新的资源。
202 （已接受） 服务器已接受请求，但尚未处理。
203 （非授权信息） 服务器已成功处理了请求，但返回的信息可能来自另一来源。
204 （无内容） 服务器成功处理了请求，但没有返回任何内容。
205 （重置内容） 服务器成功处理了请求，但没有返回任何内容。
206 （部分内容） 服务器成功处理了部分 GET 请求。
207 由WebDAV(RFC 2518)扩展的状态码，代表之后的消息体将是一个XML消息，并且可能依照之前子请求数量的不同，包含一系列独立的响应代码。

3xx （重定向） 表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。
300 （多种选择） 针对请求，服务器可执行多种操作。 服务器可根据请求者 (useragent)选择一项操作，或提供操作列表供请求者选择。
301 （永久移动） 请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或HEAD请求的响应）时，会自动将请求者转到新位置。
302 （临时移动） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
303 （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。
304 （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
305 （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。
307 （临时重定向） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。

4xx （请求错误） 这些状态代码表示请求可能出错，妨碍了服务器的处理。
400 （错误请求） 服务器不理解请求的语法。
401 （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
402 该状态码是为了将来可能的需求而预留的。
403 （禁止） 服务器拒绝请求。
404 （未找到） 服务器找不到请求的网页。
405 （方法禁用） 禁用请求中指定的方法。
406 （不接受） 无法使用请求的内容特性响应请求的网页。
407 （需要代理授权） 此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。
408 （请求超时）服务器等候请求时发生超时。
409 （冲突） 服务器在完成请求时发生冲突。 服务器必须在响应中包含有关冲突的信息。
410 （已删除） 如果请求的资源已永久删除，服务器就会返回此响应。
411 （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。
412 （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。
413 （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。
414 （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。这比较少见，通常的情况包括：本应使用POST方法的表单提交变成了GET方法，导致查询字符串（Query String）过长。
415 （不支持的媒体类型） 请求的格式不受请求页面的支持。
416 （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。
417 （未满足期望值） 服务器未满足”期望”请求标头字段的要求。

5xx （服务器错误）这些状态代码表示服务器在尝试处理请求时发生内部错误。这些错误可能是服务器本身的错误，而不是请求出错。
500 （服务器内部错误） 服务器遇到错误，无法完成请求。
501 （尚未实施） 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。
502 （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。
503 （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。
504 （网关超时） 服务器作为网关或代理，但是没有及时从上游服务器收到请求。
505 （HTTP 版本不受支持）服务器不支持请求中所用的 HTTP 协议版本。
-->
```

## 获取所有用户的接口

- **首先将router.ts中的路由进行一个封装**
- **定义一个restful api的接口(express中这样使用：res.statusCode = 404;)**

![image-20210914173315274](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210914173315274.png)

**router/account.ts**

```ts
module.exports = (app)=>{
    app.router.post('/register', app.controller.user.create);
    app.router.post('/login', app.controller.user.index);

    const github = (app as any).passport.authenticate('github', {
        successRedirect: 'http://127.0.0.1:8080/admin'
    });
    app.router.get('/passport/github', github);
    app.router.get('/passport/github/callback', github);
}
```

**router.ts**

```ts
import { Application } from 'egg';

export default (app: Application) => {
  const { controller, router } = app;

  require('./router/code')(app);
  require('./router/account')(app);

  router.get('/api/v1/users', controller.users.index);
};
```

**设置http状态码**

```ts
module.exports = {
    success(data, status = 200, msg = '成功') {
        this.status = status; // RESTful API的响应状态
        this.body = {
            code: status,
            msg:msg,
            data:data
        }
    },
    error(status = 500, msg='错误') {
        this.status = status;
        this.body = {
            code:status,
            msg:msg
        }
    }
};
```

## 渲染所有的用户

- **首先将表进行更新**
- **在前端通过判断res.status状态码来确定是否请求成功**

```ts
/**
 * @desc 用户表
 */
import { Column, DataType, Model, Table, CreatedAt, UpdatedAt, HasMany} from 'sequelize-typescript';
import {OAuth} from './oauth';

@Table({
    modelName: 'user'
})
export class User extends Model<User> {

    @Column({
        type: DataType.INTEGER,
        primaryKey: true,
        autoIncrement: true,
        unique: true,
        allowNull: false,
        comment: '用户ID',
    })
    id: number;

    @Column({
        type:DataType.STRING(255),
        allowNull:true,
        unique:true,
        comment: '用户姓名',
        validate:{
            is: /^[A-Za-z0-9\-]{6,}$/
        }
    })
    username: string;

    @Column({
        type:DataType.STRING(255),
        allowNull:true,
        unique:true,
        comment: '用户邮箱',
        validate:{
            isEmail:true
        }
    })
    email: string;

    @Column({
        type:DataType.STRING(255),
        allowNull:true,
        unique:true,
        comment: '用户手机',
        validate:{
            is:/^1[3456789]\d{9}$/
        }
    })
    phone: string;

    @Column({
        type:DataType.STRING(255),
        allowNull:false,
        unique:true,
        comment: '用户密码',
    })
    password: string;

    @Column({
        field:'user_state',
        type:DataType.BOOLEAN,
        allowNull:true,
        unique:false,
        comment: '用户是否可用',
    })
    userState: boolean;

    @Column({
        type:DataType.BOOLEAN,
        allowNull:true,
        unique:false,
        comment: '是否绑定授权账户',
    })
    github: boolean;

    @Column({
        field:'avatar_url',
        type:DataType.STRING,
        allowNull:true,
        unique:false,
        comment: '用户头像',
    })
    avatarURL: string;

    @HasMany(()=>OAuth)
    oauths:OAuth[];

    @CreatedAt
    createdAt: Date;

    @UpdatedAt
    updatedAt: Date;
};
export default () => User;
```

**前端请求**

```ts
created(): void {
    getUsers()
    .then((response:any)=>{
        // console.log(response.status);
        // console.log(response.data);
        if(response.status === 200){
            this.tableData = response.data.data;
        }else{
            (this as any).$message.error(response.data.msg);
        }
    })
    .catch((error)=>{
        (this as any).$message.error(error.response.data.msg);
    })
}
```

## 添加用户弹窗

**使用了Dialog组件**

```vue
<template>
    <div>
        <!--面包屑导航-->
        <el-breadcrumb separator="/">
            <el-breadcrumb-item><a href="/admin" @click="resetDefaultActivePath">首页</a></el-breadcrumb-item>
            <el-breadcrumb-item>用户管理</el-breadcrumb-item>
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>
        <!--内容卡片区域-->
        <el-card class="box-card">
            <!--头部搜索区域-->
            <el-row>
                <el-col :span="4">
                    <el-button type="primary" @click="addUser">添加用户</el-button>
                    <el-button type="primary" @click="importUsers">导入用户</el-button>
                </el-col>
            </el-row>   
        </el-card>
        <!--底部分页区域-->
        <!--添加用户对话框-->
        <el-dialog
                title="提示"
                :visible.sync="addUserDialogVisible"
                width="30%">
            <span>这是一段信息</span>
            <span slot="footer" class="dialog-footer">
                <el-button @click="addUserDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="addUserDialogVisible = false">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script lang="ts">
    import {Component, Vue} from 'vue-property-decorator';
    import {getUsers} from '../api/index';

    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{
        private addUserDialogVisible = false;
        private tableData = [];
        private searchData = {
            role:'',
            origin:'',
            type:'',
            key:''
        };
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        };
        private onSubmit(){

        };
        private exportUsers(){

        };
        private addUser(){
            this.addUserDialogVisible = true;
        };
        private importUsers(){

        };
        created(): void {
            getUsers()
                .then((response:any)=>{
                    if(response.status === 200){
                        this.tableData = response.data.data;
                    }else{
                        (this as any).$message.error(response.data.msg);
                    }
                })
                .catch((error)=>{
                    (this as any).$message.error(error.response.data.msg);
                })
        }
    }
</script>
```

## 添加用户的表单

**将之前的表单复制过来修改一下即可**

```vue
<template>
    <div>
        <!--面包屑导航-->
        <el-breadcrumb separator="/">
            <el-breadcrumb-item><a href="/admin" @click="resetDefaultActivePath">首页</a></el-breadcrumb-item>
            <el-breadcrumb-item>用户管理</el-breadcrumb-item>
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>
        <!--内容卡片区域-->
        <el-card class="box-card">
        <!--添加用户对话框-->
        <el-dialog
                title="提示"
                :visible.sync="addUserDialogVisible"
                width="30%"
                @close="closeAddUserDialog">
            <el-form ref="form" :model="userData" :rules="userRules" label-width="0px">
                <el-form-item label="" prop="username">
                    <el-input v-model="userData.username" prefix-icon="el-icon-user"></el-input>
                </el-form-item>
                <el-form-item label="" prop="email">
                    <el-input v-model="userData.email" prefix-icon="el-icon-message"></el-input>
                </el-form-item>
                <el-form-item label="" prop="phone">
                    <el-input v-model="userData.phone" prefix-icon="el-icon-phone-outline"></el-input>
                </el-form-item>
                <el-form-item label="" prop="password">
                    <el-input type="password" v-model="userData.password" prefix-icon="el-icon-lock"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="addUserDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="addUserDialogVisible = false">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {getUsers} from '../api/index';
    import {ElForm} from 'element-ui/types/form';

    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{
        private userData = {
            username:'',
            email:'',
            phone:'',
            password:''
        };
        private addUserDialogVisible = false;
        private tableData = [];
        private searchData = {
            role:'',
            origin:'',
            type:'',
            key:''
        };
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        };
        private onSubmit(){

        };
        private exportUsers(){

        };
        private addUser(){
            this.addUserDialogVisible = true;
        };
        private importUsers(){

        };

        private validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        private validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        private validateEmail = (rule: any, value:string, callback:any) => {
            const reg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
            if(!value){
                callback(new Error('请填写用户邮箱'));
            }else if(!reg.test(value)){
                callback(new Error('邮箱格式不正确'));
            }else{
                callback();
            }
        };
        private validatePhone = (rule: any, value:string, callback:any) => {
            const reg = /^1[3456789]\d{9}$/;
            if(!value){
                callback(new Error('请填写用户手机'));
            }else if(!reg.test(value)){
                callback(new Error('手机格式不正确'));
            }else{
                callback();
            }
        };
        private userRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            email: [
                { validator: this.validateEmail, trigger: 'blur' }
            ],
            phone: [
                { validator: this.validatePhone, trigger: 'blur' }
            ],
        }

        @Ref() readonly form!: ElForm;
        private closeAddUserDialog(){
            this.form.resetFields();
        }
        created(): void {
            getUsers()
                .then((response:any)=>{
                    if(response.status === 200){
                        this.tableData = response.data.data;
                    }else{
                        (this as any).$message.error(response.data.msg);
                    }
                })
                .catch((error)=>{
                    (this as any).$message.error(error.response.data.msg);
                })
        }
    }
</script>

<style lang="scss" scoped>
    .el-breadcrumb{
        padding-bottom: 20px;
    }
    .el-pagination{
        padding-top: 20px;
    }
</style>
```

## 添加用户的接口

- **首先定义一个新的数据校验的规则**
- **在controller.users中进行校验，然后创建用户**
- **在service中copy之前的创建用户的代码，进行用户的添加**

**validator**

```ts
export default {
    username: {
        type: 'string',
        trim: true,
        // 只能是数字或字母
        format: /^[A-Za-z0-9]{6,}$/,
        message: '用户名不符合要求'
    },
    password: {
        type: 'string',
        trim: true,
        // 必须是数字字母符号组合
        format: /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/,
        message: '密码不符合要求'
    },
    email:{
        type:'string',
        trim:true,
        format: /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/,
        message:'邮箱验不符合要求'
    },
    phone:{
        type:'string',
        trim:true,
        format: /^1[3456789]\d{9}$/,
        message:'手机不符合要求'
    }
}
```

**controller.users**

```ts
import { Controller } from 'egg';
import AddUserRule from '../validate/addUserRule'

export default class UserController extends Controller {
    public async index() {
        const { ctx } = this;
        try {
            const users = await ctx.service.users.getAll();
            ctx.success(users);
        }catch (e) {
            ctx.error(500, e.message);
        }
    }
    public async create(){
        const {ctx} = this;
        const data = ctx.request.body;
        try {
            // 1.校验数据和验证码
            ctx.validate(AddUserRule, data);
            // 2.将校验通过的数据保存到数据库中
            const user = await ctx.service.users.createUser(ctx.request.body);
            ctx.success(user);
        } catch (e) {
            if (e.errors) {
                ctx.error(400, e.errors);
            } else {
                ctx.error(400, e.message);
            }
        }
    }
}
```

**service**

```ts
import { Service } from 'egg';

/**
 * Test Service
 */
export default class Users extends Service {
    public async getAll() {
        return this.ctx.model.User.findAll();
    }
    public async createUser(obj){
        const {username, password, email, phone} = obj;
        obj.password = this.ctx.helper.encryptText(password);
        let user =  await this.ctx.model.User.findOne({where:{username:username}});
        if(user){
            throw new Error('用户名已存在');
        }
        user =  await this.ctx.model.User.findOne({where:{email:email}});
        if(user){
            throw new Error('邮箱已存在');
        }
        user =  await this.ctx.model.User.findOne({where:{phone:phone}});
        if(user){
            throw new Error('手机已存在');
        }
        const data = await this.ctx.model.User.create(obj);
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
}
```

## 添加用户打通

**清空form数据应该是在打开前进行清除**

```vue
<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {getUsers, createUsers} from '../api/index';
    import {ElForm} from 'element-ui/types/form';

    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{

        private tableData:any[] = [];
        private searchData = {
            role:'',
            origin:'',
            type:'',
            key:''
        };
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        };
        private onSubmit(){

        };
        private exportUsers(){

        };
        private importUsers(){

        };

        // 添加用户相关代码
        private userData = {
            username:'',
            email:'',
            phone:'',
            password:''
        };
        private addUserDialogVisible = false;
        private validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        private validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        private validateEmail = (rule: any, value:string, callback:any) => {
            const reg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
            if(!value){
                callback(new Error('请填写用户邮箱'));
            }else if(!reg.test(value)){
                callback(new Error('邮箱格式不正确'));
            }else{
                callback();
            }
        };
        private validatePhone = (rule: any, value:string, callback:any) => {
            const reg = /^1[3456789]\d{9}$/;
            if(!value){
                callback(new Error('请填写用户手机'));
            }else if(!reg.test(value)){
                callback(new Error('手机格式不正确'));
            }else{
                callback();
            }
        };
        private userRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            email: [
                { validator: this.validateEmail, trigger: 'blur' }
            ],
            phone: [
                { validator: this.validatePhone, trigger: 'blur' }
            ],
        }
        @Ref() readonly form?: ElForm;
        private addUser(){
            this.addUserDialogVisible = true;
            this.form ? this.form.resetFields() : '';
        };

        private createUser(){
            this.addUserDialogVisible = false;
            this.form!.validate((flag)=>{
                if(flag){
                    createUsers(this.userData)
                        .then((response:any)=>{
                            if(response.status === 200){
                                const user = response.data.data;
                                this.tableData.push(user);
                                (this as any).$message.success('添加用户成功');
                            }else{
                                (this as any).$message.error(response.data.msg);
                            }
                        })
                        .catch((error)=>{
                            (this as any).$message.error(error.response.data.msg);
                        })
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        }

        created(): void {
            getUsers()
                .then((response:any)=>{
                    if(response.status === 200){
                        this.tableData = response.data.data;
                    }else{
                        (this as any).$message.error(response.data.msg);
                    }
                })
                .catch((error)=>{
                    (this as any).$message.error(error.response.data.msg);
                })
        }
    }
</script>
```

## 删除用户的接口

**router.ts**

```ts
import { Application } from 'egg';

export default (app: Application) => {
  const { controller, router } = app;

  require('./router/code')(app);
  require('./router/account')(app);

  router.get('/api/v1/users', controller.users.index);
  router.post('/api/v1/users', controller.users.create);
  router.delete('/api/v1/users/:id', controller.users.destroy);
};
```

**controller/users.ts**

```ts
import { Controller } from 'egg';
import AddUserRule from '../validate/addUserRule'

export default class UsersController extends Controller {
    public async index() {
        const { ctx } = this;
        try {
            const users = await ctx.service.users.getAll();
            ctx.success(users);
        }catch (e) {
            ctx.error(500, e.message);
        }
    }
    public async create(){
        const {ctx} = this;
        const data = ctx.request.body;
        try {
            console.log(data);
            // 1.校验数据和验证码
            ctx.validate(AddUserRule, data);
            // 2.将校验通过的数据保存到数据库中
            const user = await ctx.service.users.createUser(data);
            ctx.success(user);
        } catch (e) {
            if (e.errors) {
                ctx.error(400, e.errors);
            } else {
                ctx.error(400, e.message);
            }
        }
    }
    public async destroy(){
        const {ctx} = this;
        const {id} = ctx.params;
        try {
            const user = await ctx.service.users.destroyUser(id);
            ctx.success(user);
        } catch (e) {
            ctx.error(400, e.message);
        }
    }
}
```

**service/users.ts**

```ts
import { Service } from 'egg';

/**
 * Test Service
 */
export default class Users extends Service {
    public async getAll() {
        return this.ctx.model.User.findAll();
    }
    public async createUser(obj){
        const {username, password, email, phone} = obj;
        obj.password = this.ctx.helper.encryptText(password);
        let user =  await this.ctx.model.User.findOne({where:{username:username}});
        if(user){
            throw new Error('用户名已存在');
        }
        user =  await this.ctx.model.User.findOne({where:{email:email}});
        if(user){
            throw new Error('邮箱已存在');
        }
        user =  await this.ctx.model.User.findOne({where:{phone:phone}});
        if(user){
            throw new Error('手机已存在');
        }
        const data = await this.ctx.model.User.create(obj);
        const userData = data['dataValues'];
        delete userData.password;
        return userData;
    }
    public async destroyUser(id){
        const user = await this.ctx.model.User.findByPk(id);
        if(user){
            const data = await this.ctx.model.User.destroy({
                where:{id:id}
            });
            if(data > 0){
                return user;
            }else{
                throw new Error('删除用户失败');
            }
        }else{
            throw new Error('删除的用户不存在');
        }
    }
}
```

## 删除用户打通

**api/network.ts**

```ts
import axios from 'axios'

// 进行一些全局配置
axios.defaults.baseURL = 'http://127.0.0.1:7001';
axios.defaults.timeout = 5000;
axios.defaults.withCredentials = true; // 让axios发送请求的时候带上cookie

// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // config.headers.Authorization = sessionStorage.getItem('token');
    // 在发送请求之前做些什么
    return config
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error)
});

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error)
});
// 封装自己的get/post方法
export default {
    get: function (path = '', data = {}) {
        return new Promise(function (resolve, reject) {
            axios.get(path, {
                params: data
            })
                .then(function (response) {
                    resolve(response)
                })
                .catch(function (error) {
                    reject(error)
                })
        })
    },
    post: function (path = '', data = {}) {
        return new Promise(function (resolve, reject) {
            axios.post(path, data)
                .then(function (response) {
                    resolve(response)
                })
                .catch(function (error) {
                    reject(error)
                })
        })
    },
    delete: function (path = '') {
        return new Promise(function (resolve, reject) {
            axios.delete(path)
                .then(function (response) {
                    resolve(response)
                })
                .catch(function (error) {
                    reject(error)
                })
        })
    },
    all: function (list:any[]) {
        return new Promise(function (resolve, reject) {
            axios.all(list)
                .then(axios.spread(function (...result) {
                    // 两个请求现在都执行完成
                    resolve(result)
                }))
                .catch(function (err) {
                    reject(err)
                })
        })
    }
}
```

**api/index.ts**

```ts
export const destroyUsers = (id:string)=>Network.delete(`/api/v1/users/${id}`);
```

**components/user.vue**

```vue
<template>
    <div>
        <!--内容卡片区域-->
        <el-card class="box-card">
            <!--中间表格区域-->
            <el-table
                    :data="tableData"
                    style="width: 100%"
                    :border="true"
                    :stripe="true">
                <el-table-column type="index">
                </el-table-column>
                <el-table-column
                        prop="username"
                        label="姓名">
                </el-table-column>
                <el-table-column
                        prop="email"
                        label="邮箱">
                </el-table-column>
                <el-table-column
                        prop="phone"
                        label="电话">
                </el-table-column>
                <el-table-column
                        prop="roleName"
                        label="角色">
                </el-table-column>
                <el-table-column label="状态">
                    <template slot-scope="scope">
                        <!-- {{scope.row.userState}}-->
                        <el-switch
                                v-model="scope.row.userState"
                                active-color="#13ce66"
                                inactive-color="#ff4949">
                        </el-switch>
                    </template>
                </el-table-column>
                <el-table-column label="操作">
                    <template slot-scope="scope">
                        <el-button type="primary" icon="el-icon-edit"></el-button>
                        <el-button type="danger" icon="el-icon-delete" @click="destroyUser(scope.row.id)"></el-button>
                        <el-button type="warning" icon="el-icon-setting"></el-button>
                    </template>
                </el-table-column>
            </el-table>
        </el-card>
    </div>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {getUsers, createUsers, destroyUsers} from '../api/index';
    import {ElForm} from 'element-ui/types/form';

    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{

        private tableData:any[] = [];
        private searchData = {
            role:'',
            origin:'',
            type:'',
            key:''
        };
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        };

        // 添加用户相关代码
        private userData = {
            username:'',
            email:'',
            phone:'',
            password:''
        };
        // 删除用户相关代码
        private destroyUser(id:string){
            destroyUsers(id)
                .then((response:any)=>{
                    if(response.status === 200){
                        const idx = this.tableData.findIndex((obj)=>{
                            return obj.id === id;
                        });
                        this.tableData.splice(idx, 1);
                        (this as any).$message.success('删除用户成功');
                    }else{
                        (this as any).$message.error(response.data.msg);
                    }
                })
                .catch((error)=>{
                    (this as any).$message.error(error.response.data.msg);
                })
        }
    }
</script>
```

## 编辑用户表单

- **添加用户的时候邮箱和手机号不是必须的**

```ts
private validateEmail = (rule: any, value:string, callback:any) => {
    const reg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
    if(value && !reg.test(value)){
        callback(new Error('邮箱格式不正确'));
    }else{
        callback();
    }
};
private validatePhone = (rule: any, value:string, callback:any) => {
    const reg = /^1[3456789]\d{9}$/;
    if(value && !reg.test(value)){
        callback(new Error('手机格式不正确'));
    }else{
        callback();
    }
};
```

- **静态网页的搭建**

```vue
<template>
    <div>            
        <!--编辑用户对话框-->
        <el-dialog
                title="编辑用户"
                :visible.sync="editUserDialogVisible"
                width="30%">
            <el-form ref="form" :model="editData" :rules="userRules" label-width="0px">
                <el-form-item label="" prop="username">
                    <el-input v-model="editData.username" prefix-icon="el-icon-user"></el-input>
                </el-form-item>
                <el-form-item label="" prop="email">
                    <el-input v-model="editData.email" prefix-icon="el-icon-message"></el-input>
                </el-form-item>
                <el-form-item label="" prop="phone">
                    <el-input v-model="editData.phone" prefix-icon="el-icon-phone-outline"></el-input>
                </el-form-item>
                <el-form-item label="" prop="password">
                    <el-input type="password" v-model="editData.password" prefix-icon="el-icon-lock"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editUserDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="editUser">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script lang="ts">
    // 编辑用户相关代码
    private editData = {
        username:'',
        email:'',
        phone:'',
        password:''
    };
    private editUserDialogVisible = false;
    private editUser(){
        this.editUserDialogVisible = false;
    }
    private showEditUserDialog(user:any){
        this.editUserDialogVisible = true;
        this.editData = user;
    }
</script>
```

## 编辑用户接口

- **编辑用户的接口**

```ts
router.put('/api/v1/users/:id', controller.users.update);
```

- **controller/users.ts**

```ts
public async update(){
    const {ctx} = this;
    const {id} = ctx.params;
    const data = ctx.request.body;
    try {
        // 1.校验数据和验证码
        ctx.validate(EditUserRule, data);
        // 2.将校验通过的数据保存到数据库中
        const user = await ctx.service.users.updateUser(id, data);
        ctx.success(user);
    } catch (e) {
        if (e.errors) {
            console.log(e.errors, '----------');
            ctx.error(400, e.errors);
        } else {
            ctx.error(400, e.message);
        }
    }
}
```

- **service/users.ts**

```ts
public async updateUser(id, obj){
    const user = await this.ctx.model.User.findByPk(id);
    if(user){
        obj.username ? '' : delete obj.username;
        obj.password ? '' : delete obj.password;
        obj.email ? '' : delete obj.email;
        obj.phone ? '' : delete obj.phone;
        const data = await this.ctx.model.User.update(obj, {
            where:{
                id:id
            }
        });
        console.log('更新返回的结果',data);
        if(data.length > 0){
            return user;
        }else{
            throw new Error('更新用户失败');
        }
    }else{
        throw new Error('更新的用户不存在');
    }
}
```

## 编辑用户的打通

**这里主要是开发的过程中遇到的问题，没有什么知识点**

```vue
<template>
    <div>
        <!--面包屑导航-->
        <el-breadcrumb separator="/">
            <el-breadcrumb-item><a href="/admin" @click="resetDefaultActivePath">首页</a></el-breadcrumb-item>
            <el-breadcrumb-item>用户管理</el-breadcrumb-item>
            <el-breadcrumb-item>用户列表</el-breadcrumb-item>
        </el-breadcrumb>

        <!--内容卡片区域-->
        <el-card class="box-card">
            <!--头部搜索区域-->
            <el-row>
                <el-col :span="20">
                    <el-form :inline="true" :model="searchData" class="demo-form-inline">
                        <el-form-item label="">
                            <el-select v-model="searchData.role" placeholder="-所有角色-">
                                <el-option label="管理员" value="manger"></el-option>
                                <el-option label="普通用户" value="normal"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.origin" placeholder="-所有来源-">
                                <el-option label="本地注册" value="local"></el-option>
                                <el-option label="Github登录" value="github"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.type" placeholder="-所有用户-">
                                <el-option label="用户名" value="username"></el-option>
                                <el-option label="邮箱" value="email"></el-option>
                                <el-option label="手机" value="phone"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-input v-model="searchData.key" placeholder="关键字"></el-input>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="onSubmit">查询</el-button>
                            <el-button type="primary" @click="exportUsers">导出搜索结果</el-button>
                        </el-form-item>
                    </el-form>
                </el-col>
                <el-col :span="4">
                    <el-button type="primary" @click="showAddUserDialog">添加用户</el-button>
                    <el-button type="primary" @click="importUsers">导入用户</el-button>
                </el-col>
            </el-row>
            <!--中间表格区域-->
            <el-table
                    :data="tableData"
                    style="width: 100%"
                    :border="true"
                    :stripe="true">
                <el-table-column type="index">
                </el-table-column>
                <el-table-column
                        prop="username"
                        label="姓名">
                </el-table-column>
                <el-table-column
                        prop="email"
                        label="邮箱">
                </el-table-column>
                <el-table-column
                        prop="phone"
                        label="电话">
                </el-table-column>
                <el-table-column
                        prop="roleName"
                        label="角色">
                </el-table-column>
                <el-table-column label="状态">
                    <template slot-scope="scope">
                        <!-- {{scope.row.userState}}-->
                        <el-switch
                                v-model="scope.row.userState"
                                active-color="#13ce66"
                                inactive-color="#ff4949">
                        </el-switch>
                    </template>
                </el-table-column>
                <el-table-column label="操作">
                    <template slot-scope="scope">
                        <el-button type="primary" icon="el-icon-edit" @click="showEditUserDialog(scope.row)"></el-button>
                        <el-button type="danger" icon="el-icon-delete" @click="destroyUser(scope.row.id)"></el-button>
                        <el-button type="warning" icon="el-icon-setting"></el-button>
                    </template>
                </el-table-column>
            </el-table>
        </el-card>

        <!--底部分页区域-->
        <el-pagination
                :page-sizes="[100, 200, 300, 400]"
                :page-size="100"
                layout="total, sizes, prev, pager, next, jumper"
                :total="400">
        </el-pagination>

        <!--添加用户对话框-->
        <el-dialog
                title="添加用户"
                :visible.sync="addUserDialogVisible"
                width="30%">
            <el-form ref="form" :model="userData" :rules="addUserRules" label-width="0px">
                <el-form-item label="" prop="username">
                    <el-input v-model="userData.username" prefix-icon="el-icon-user"></el-input>
                </el-form-item>
                <el-form-item label="" prop="email">
                    <el-input v-model="userData.email" prefix-icon="el-icon-message"></el-input>
                </el-form-item>
                <el-form-item label="" prop="phone">
                    <el-input v-model="userData.phone" prefix-icon="el-icon-phone-outline"></el-input>
                </el-form-item>
                <el-form-item label="" prop="password">
                    <el-input type="password" v-model="userData.password" prefix-icon="el-icon-lock"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="addUserDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="createUser">确 定</el-button>
            </span>
        </el-dialog>

        <!--编辑用户对话框-->
        <el-dialog
                title="编辑用户"
                :visible.sync="editUserDialogVisible"
                width="30%">
            <el-form ref="form" :model="editData" :rules="editUserRules" label-width="0px">
                <el-form-item label="" prop="username">
                    <el-input v-model="editData.username" prefix-icon="el-icon-user"></el-input>
                </el-form-item>
                <el-form-item label="" prop="email">
                    <el-input v-model="editData.email" prefix-icon="el-icon-message"></el-input>
                </el-form-item>
                <el-form-item label="" prop="phone">
                    <el-input v-model="editData.phone" prefix-icon="el-icon-phone-outline"></el-input>
                </el-form-item>
                <el-form-item label="" prop="password">
                    <el-input type="password" v-model="editData.password" prefix-icon="el-icon-lock"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editUserDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="editUser">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {getUsers, createUsers, destroyUsers, updateUsers} from '../api/index';
    import {ElForm} from 'element-ui/types/form';

    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{

        private tableData:any[] = [];
        private searchData = {
            role:'',
            origin:'',
            type:'',
            key:''
        };
        private resetDefaultActivePath(){
            sessionStorage.removeItem('acitvePath');
        };
        private onSubmit(){

        };
        private exportUsers(){

        };
        private importUsers(){

        };

        // 添加用户相关代码
        private userData = {
            username:'',
            email:'',
            phone:'',
            password:''
        };
        private addUserDialogVisible = false;
        private validateName = (rule: any, value:string, callback:any) => {
            const reg = /^[A-Za-z0-9]{6,}$/;
            if(!value){
                callback(new Error('请填写用户名'));
            }else if(value.length < 6){
                callback(new Error('用户名至少是6位'));
            }else if(!reg.test(value)){
                callback(new Error('用户名只能是字母和数字'));
            }else{
                callback();
            }
        };
        private validatePass = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(!value){
                callback(new Error('请填写密码'));
            }else if(value.length < 6){
                callback(new Error('密码至少是8位'));
            }else if(!reg.test(value)){
                callback(new Error('密码必须包含字母数字和特殊符号'));
            }else{
                callback();
            }
        };
        private validatePass2 = (rule: any, value:string, callback:any) => {
            const reg = /^(?:(?=.*[0-9].*)(?=.*[A-Za-z].*)(?=.*[,\.#%'\+\*\-:;^_`].*))[,\.#%'\+\*\-:;^_`0-9A-Za-z]{8,}$/;
            if(value){
                if(value.length < 6){
                    callback(new Error('密码至少是8位'));
                }else if(!reg.test(value)){
                    callback(new Error('密码必须包含字母数字和特殊符号'));
                }
            }else{
                callback();
            }
        };
        private validateEmail = (rule: any, value:string, callback:any) => {
            const reg = /^[a-zA-Z0-9_-]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;
            if(value && !reg.test(value)){
                callback(new Error('邮箱格式不正确'));
            }else{
                callback();
            }
        };
        private validatePhone = (rule: any, value:string, callback:any) => {
            const reg = /^1[3456789]\d{9}$/;
            if(value && !reg.test(value)){
                callback(new Error('手机格式不正确'));
            }else{
                callback();
            }
        };
        private addUserRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass, trigger: 'blur' }
            ],
            email: [
                { validator: this.validateEmail, trigger: 'blur' }
            ],
            phone: [
                { validator: this.validatePhone, trigger: 'blur' }
            ],
        };
        @Ref() readonly form?: ElForm;
        private showAddUserDialog(){
            this.addUserDialogVisible = true;
            this.form ? this.form.resetFields() : '';
        };
        private createUser(){
            this.addUserDialogVisible = false;
            this.form!.validate((flag)=>{
                if(flag){
                    createUsers(this.userData)
                        .then((response:any)=>{
                            if(response.status === 200){
                                const user = response.data.data;
                                this.tableData.push(user);
                                (this as any).$message.success('添加用户成功');
                            }else{
                                (this as any).$message.error(response.data.msg);
                            }
                        })
                        .catch((error)=>{
                            (this as any).$message.error(error.response.data.msg);
                        })
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        }

        // 删除用户相关代码
        private destroyUser(id:string){
            destroyUsers(id)
                .then((response:any)=>{
                    if(response.status === 200){
                        const idx = this.tableData.findIndex((obj)=>{
                            return obj.id === id;
                        });
                        this.tableData.splice(idx, 1);
                        (this as any).$message.success('删除用户成功');
                    }else{
                        (this as any).$message.error(response.data.msg);
                    }
                })
                .catch((error)=>{
                    (this as any).$message.error(error.response.data.msg);
                })
        }

        // 编辑用户相关代码
        private editUserRules = {
            username: [
                { validator: this.validateName, trigger: 'blur' }
            ],
            password: [
                { validator: this.validatePass2, trigger: 'blur' }
            ],
            email: [
                { validator: this.validateEmail, trigger: 'blur' }
            ],
            phone: [
                { validator: this.validatePhone, trigger: 'blur' }
            ],
        };
        private editData = {
            id:'',
            username:'',
            email:'',
            phone:'',
            password:''
        };
        private editUserDialogVisible = false;
        private showEditUserDialog(user:any){
            this.editUserDialogVisible = true;
            this.form ? this.form.resetFields() : '';
            this.editData = Object.assign(this.editData, user);
        }
        private editUser(){
            this.editUserDialogVisible = false;
            this.form!.validate((flag)=>{
                if(flag){
                    updateUsers(this.editData.id, this.editData)
                        .then((response:any)=>{
                            if(response.status === 200){
                                const idx = this.tableData.findIndex((obj)=>{
                                    return obj.id === this.editData.id;
                                });
                                // 直接给数组的某一个索引赋值, 是不会触发Vue更新界面的
                                // this.tableData[idx] = this.editData;
                                this.$set(this.tableData, idx, this.editData);
                                (this as any).$message.success('更新用户成功');
                            }else{
                                (this as any).$message.error(response.data.msg);
                            }
                        })
                        .catch((error)=>{
                            (this as any).$message.error(error.response.data.msg);
                        })
                }else{
                    (this as any).$message.error('数据格式不对');
                }
            });
        }

        created(): void {
            getUsers()
                .then((response:any)=>{
                    if(response.status === 200){
                        this.tableData = response.data.data;
                    }else{
                        (this as any).$message.error(response.data.msg);
                    }
                })
                .catch((error)=>{
                    (this as any).$message.error(error.response.data.msg);
                })
        }
    }
</script>

<style lang="scss" scoped>
    .el-breadcrumb{
        padding-bottom: 20px;
    }
    .el-pagination{
        padding-top: 20px;
    }
</style>
```

## 用户头像上传（界面搭建）

- **使用upload组件**

```vue
<template>
    <div>
        <!--编辑用户对话框-->
        <el-dialog
                title="编辑用户"
                :visible.sync="editUserDialogVisible"
                width="30%">
            <el-form ref="form" :model="editData" :rules="editUserRules" label-width="0px">
                <el-form-item label="" prop="username" style="text-align: center">
                    <el-upload
                            class="avatar-uploader"
                            action="https://jsonplaceholder.typicode.com/posts/"
                            :show-file-list="false"
                            :on-success="handleAvatarSuccess"
                            :before-upload="beforeAvatarUpload">
                        <img v-if="imageUrl" :src="imageUrl" class="avatar">
                        <i v-else class="el-icon-plus avatar-uploader-icon"></i>
                    </el-upload>
                </el-form-item>
                <el-form-item label="" prop="username">
                    <el-input v-model="editData.username" prefix-icon="el-icon-user"></el-input>
                </el-form-item>
                <el-form-item label="" prop="email">
                    <el-input v-model="editData.email" prefix-icon="el-icon-message"></el-input>
                </el-form-item>
                <el-form-item label="" prop="phone">
                    <el-input v-model="editData.phone" prefix-icon="el-icon-phone-outline"></el-input>
                </el-form-item>
                <el-form-item label="" prop="password">
                    <el-input type="password" v-model="editData.password" prefix-icon="el-icon-lock"></el-input>
                </el-form-item>
            </el-form>
            <span slot="footer" class="dialog-footer">
                <el-button @click="editUserDialogVisible = false">取 消</el-button>
                <el-button type="primary" @click="editUser">确 定</el-button>
            </span>
        </el-dialog>
    </div>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {getUsers, createUsers, destroyUsers, updateUsers} from '../api/index';
    import {ElForm} from 'element-ui/types/form';

    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{
        // 上传用户头像相关代码
        // 上传成功之后的回调
        private handleAvatarSuccess(res:any, file:any) {
            this.editData.avatarURL = res;
        }
        // 上传之前的回调
        private beforeAvatarUpload(file:any) {
            const isJPG = file.type === 'image/jpeg';
            const isLt2M = file.size / 1024 / 1024 < 2;

            if (!isJPG) {
                (this as any).$message.error('上传头像图片只能是 JPG 格式!');
            }
            if (!isLt2M) {
                (this as any).$message.error('上传头像图片大小不能超过 2MB!');
            }
            return isJPG && isLt2M;
        }
    }
</script>

<style lang="scss" scoped>
    .el-breadcrumb{
        padding-bottom: 20px;
    }
    .el-pagination{
        padding-top: 20px;
    }
    .avatar-uploader{
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
        display: inline-block;
    }
    .avatar-uploader:hover {
        border-color: #409EFF;
    }
    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 178px;
        height: 178px;
        line-height: 178px;
        text-align: center;
    }
    .avatar {
        width: 178px;
        height: 178px;
        display: block;
    }
</style>

```

## 资源迁移注意事项

- **保存用户头像的地址的时候不需要加上服务器的ip地址，防止服务器变更后更改麻烦**

- **方法一：使用虚拟字段**

- **方法二：使用获取器**

```ts
@Column({
    // 虚拟字段(可以在代码文件中使用，但是不会定义在数据库中的字段)
    type: DataType.VIRTUAL,
    get(){
        return 'http://127.0.0.1:7001';
    }
})
baseURL: string;
```

```ts
@Column({
    field:'avatar_url',
    type:DataType.STRING,
    allowNull:true,
    unique:false,
    comment: '用户头像',
    get() { //首先将数据获取出来，然后进行批量的修改
        const rawValue = this.getDataValue('avatarURL');
        return rawValue ? 'http://127.0.0.1:7001' + rawValue : null;
    }
})
avatarURL: string;
```

## 用户状态修改

**绑定change时间，定义监听方法**

```vue
<template>
    <div>
        <!--内容卡片区域-->
        <el-card class="box-card">
            <!--头部搜索区域-->
            <el-row>
                <el-col :span="20">
                    <el-form :inline="true" :model="searchData" class="demo-form-inline">
                        <el-form-item label="">
                            <el-select v-model="searchData.role" placeholder="-所有角色-">
                                <el-option label="管理员" value="manger"></el-option>
                                <el-option label="普通用户" value="normal"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.origin" placeholder="-所有来源-">
                                <el-option label="本地注册" value="local"></el-option>
                                <el-option label="Github登录" value="github"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-select v-model="searchData.type" placeholder="-所有用户-">
                                <el-option label="用户名" value="username"></el-option>
                                <el-option label="邮箱" value="email"></el-option>
                                <el-option label="手机" value="phone"></el-option>
                            </el-select>
                        </el-form-item>
                        <el-form-item label="">
                            <el-input v-model="searchData.key" placeholder="关键字"></el-input>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="onSubmit">查询</el-button>
                            <el-button type="primary" @click="exportUsers">导出搜索结果</el-button>
                        </el-form-item>
                    </el-form>
                </el-col>
                <el-col :span="4">
                    <el-button type="primary" @click="showAddUserDialog">添加用户</el-button>
                    <el-button type="primary" @click="importUsers">导入用户</el-button>
                </el-col>
            </el-row>
            <!--中间表格区域-->
            <el-table
                    :data="tableData"
                    style="width: 100%"
                    :border="true"
                    :stripe="true">
                <el-table-column type="index">
                </el-table-column>
                <el-table-column
                        prop="username"
                        label="姓名">
                </el-table-column>
                <el-table-column
                        prop="email"
                        label="邮箱">
                </el-table-column>
                <el-table-column
                        prop="phone"
                        label="电话">
                </el-table-column>
                <el-table-column
                        prop="roleName"
                        label="角色">
                </el-table-column>
                <el-table-column label="状态">
                    <template slot-scope="scope">
                        <!-- {{scope.row.userState}}-->
                        <el-switch
                                v-model="scope.row.userState"
                                active-color="#13ce66"
                                inactive-color="#ff4949"
                                @change="changeUserState(scope.row)">
                        </el-switch>
                    </template>
                </el-table-column>
                <el-table-column label="操作">
                    <template slot-scope="scope">
                        <el-button type="primary" icon="el-icon-edit" @click="showEditUserDialog(scope.row)"></el-button>
                        <el-button type="danger" icon="el-icon-delete" @click="destroyUser(scope.row.id)"></el-button>
                        <el-button type="warning" icon="el-icon-setting"></el-button>
                    </template>
                </el-table-column>
            </el-table>
        </el-card>
    </div>
</template>

<script lang="ts">
    import {Component, Vue, Ref} from 'vue-property-decorator';
    import {getUsers, createUsers, destroyUsers, updateUsers} from '../api/index';
    import {ElForm} from 'element-ui/types/form';

    @Component({
        name: "Users",
        components:{},
    })
    export default class Users extends Vue{
        // 修改用户状态相关代码
        private changeUserState(user:any){
            updateUsers(user.id, user)
                .then((response:any)=>{
                    if(response.status === 200){
                        (this as any).$message.success('更新用户状态成功');
                    }else{
                        user.userState = !user.userState;
                        (this as any).$message.error('更新用户状态失败');
                    }
                })
                .catch((error)=>{
                    user.userState = !user.userState;
                    (this as any).$message.error('更新用户状态失败');
                });
        }
</script>
```

## 用户状态打通

**当用户状态不可用的时候无法进行登录**

```ts
public async index() {
    const {ctx} = this;
    try {
        // 1.校验数据和验证码
        this.validateUserInfo();
        const data = ctx.request.body;
        ctx.helper.verifyImageCode(data.captcha);
        // 2.将校验通过的数据保存到数据库中
        const user = await ctx.service.user.getUser(data);
        delete user.password;
        // 校验用户是否可用
        if(!user.userState){
            return ctx.error(400, '用户已经注销');
        }
        // 3.生成JWT令牌
        const token = jwt.sign(user, this.config.keys, {expiresIn: '7 days'});
        // user.token = token;
        ctx.cookies.set('token', token, {
            path:'/',
            maxAge: 24 * 60 * 60 * 1000,
            // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
            httpOnly: false,
            signed:false,
        });
        ctx.success(user);
    } catch (e) {
        if (e.errors) {
            ctx.error(400, e.errors);
        } else {
            ctx.error(400, e.message);
        }
    }
}
```

## 用户头像上传(使用文件的方式进行上传)

```ts
public async posts(){
    const {ctx} = this;
    // 1.拿到上传过来的文件
    // 注意点: 在Egg中想要实现文件上传, 必须进行配置
    /*
        Egg的文件上传分为两种模式: File Mode / Stream Mode
        这两种模式的区别: 文件模式会先将前端传递过来的数据写入到缓存中, 然后再返回给我们
                          文件模式相对比较简单, 但是如果数据量较大, 它的性能会差一些
                          数据流模式不会先将前端传递过来的数据写入到缓存中
                          数据流模式相对比复杂一点, 但是如果数据量较大, 它的性能会好一些
        * */
    const file =  ctx.request.files[0];
    // 2.生成一个独一无二的文件名称
    const fileName =  ctx.helper.encryptText(file.filename + Date.now()) + path.extname(file.filename);
    // 3.生成存储文件的路径
    let filePath = path.join('/public/upload', fileName);
    const absFilePath = path.join(this.config.baseDir, 'app', filePath);
    // 4.写入文件
    const readStream =  fs.readFileSync(file.filepath);
    fs.writeFileSync(absFilePath, readStream);
    // 5.返回存储图片的路径
    filePath = filePath.replace(/\\/g, '/');
    ctx.success(filePath);
}
```

**前端(代码是选择的自动上传，可以看文档实现手动上传)**

```ts
// 上传用户头像相关代码
private handleAvatarSuccess(res:any, file:any) {
    if(res.code === 200){
        this.editData.avatarURL = res.data;
    }
}
private beforeAvatarUpload(file:any) {
    const isJPG = file.type === 'image/jpeg';
    const isLt2M = file.size / 1024 / 1024 < 2;

    if (!isJPG) {
        (this as any).$message.error('上传头像图片只能是 JPG 格式!');
    }
    if (!isLt2M) {
        (this as any).$message.error('上传头像图片大小不能超过 2MB!');
    }
    return isJPG && isLt2M;
}

```

## 后端操作excel（exceljs和Node XLSX）

**Node xlsx的导入实例**

```js
const xlsx = require("node-xlsx")
const fs = require("fs")
// 1.读取Excel文件
const workSheets = xlsx.parse(fs.readFileSync("./test.xlsx"));
// 2.获取到需要操作的那一页的对象
//   这个对象的data属性中就存储了这一页所有行的数据
const sheet1 = workSheets.length ? workSheets[0]:null
const sheet1Data = sheet1 ? sheet1.data:[]
const users = []
for(let i=0;i<sheet1Data.length;i++){
  //获取到所有的key
  const columnTitles = sheet1Data[0]
  //获取到当前行的数据
  const columnValues = sheet1Data[i]
  const user = {}
  for(let j=0;j<columnTitles.length;j++){
    user[columnTitles[j]] = columnValues[j]
  }
  users.push(user)
}
```

## excel导入用户

- **使用文件上传的组件**
- **然后定义一个接口，用于将excel中的数据添加到数据库**

**前端**

```vue
<el-col :span="4">
    <el-button type="primary" @click="showAddUserDialog">添加用户</el-button>
    <el-upload
               class="excel-uploader"
               action="http://127.0.0.1:7001/api/v1/importUser/"
               :show-file-list="false"
               :on-success="handleExcelSuccess"
               :before-upload="beforeExcelUpload"
               accept=".xls">
        <el-button type="primary">导入用户</el-button>
    </el-upload>
</el-col>

<script>
// 上传Excel相关代码
    private handleExcelSuccess(res:any, file:any) {
        console.log(res);
    }
    private beforeExcelUpload(file:any) {
        console.log(file.type);
        const isExcel = file.type === 'application/vnd.ms-excel';
        const isLt2M = file.size / 1024 / 1024 < 2;

        if (!isExcel) {
            (this as any).$message.error('只能上传Excel文件');
        }
        if (!isLt2M) {
            (this as any).$message.error('上传头像图片大小不能超过 2MB!');
        }
        return isExcel && isLt2M;
    }
</script>
```

**controller/users.ts(需要使用事务的回滚操作)**

```ts
public async importUser(){
    const {ctx} = this;
    let transaction;
    try {
        const file =  ctx.request.files[0];
        // 1.读取Excel文件
        const workSheets = xlsx.parse(fs.readFileSync(file.filepath));
        // 2.获取到需要操作的那一页的对象
        const sheet1 = workSheets.length ? workSheets[0] : null;
        const sheet1Data = sheet1 ? sheet1.data : [];
        const users:any[] = [];
        transaction = await ctx.model.transaction();
        for(let i = 1; i < sheet1Data.length; i++){
            // 获取到所有的key
            const cloumnTitles = sheet1Data[0];
            // 获取到当前行的数据
            const cloumnValues = sheet1Data[i];
            const user = {};
            for(let j = 0; j < cloumnTitles.length; j++){
                user[cloumnTitles[j]] = cloumnValues[j];
            }
            await ctx.service.users.createUser(user);
            users.push(user);
        }
        await transaction.commit();
        ctx.success(users);
    }catch (e) {
        await transaction.rollback();
        ctx.error(500, e.message);
    }
}
```

**service/users.ts**

```ts
public async createUser(obj){
    const {username, password, email, phone} = obj;
    obj.password = this.ctx.helper.encryptText(password);
    let user;
    if(username){
        user =  await this.ctx.model.User.findOne({where:{username:username}});
        if(user){
            throw new Error('用户名已存在');
        }
    }else{
        delete obj.username;
    }
    if(email){
        user =  await this.ctx.model.User.findOne({where:{email:email}});
        if(user){
            throw new Error('邮箱已存在');
        }
    }else{
        delete obj.email;
    }
    if(phone){
        user =  await this.ctx.model.User.findOne({where:{phone:phone}});
        if(user){
            throw new Error('手机已存在');
        }
    }else{
        delete obj.phone;
    }
    const data = await this.ctx.model.User.create(obj);
    const userData = data['dataValues'];
    delete userData.password;
    return userData;
}
```

## 导出所有的用户

**前端**

```html
 <a href="http://127.0.0.1:7001/api/v1/exportUser/">
     <el-button type="primary">导出所有用户</el-button>
</a>
```

**controller/user.ts**

```ts
public async exportUser(){
    const {ctx} = this;
    const users = await ctx.service.users.getAll();
    const user = users.length ? users[0].dataValues : null;
    const data:any[] = [];
    if(user){
        const cloumnTitles = Object.keys(user);
        data.push(cloumnTitles);
        users.forEach((user)=>{
            const temp:any[] = [];
            cloumnTitles.forEach((key)=>{
                temp.push(user[key]);
            });
            data.push(temp);
        });
        const buffer = xlsx.build([{name: "mySheetName", data: data}]);
        ctx.set('Content-Type', 'application/vnd.ms-excel');
        // ctx.set('Content-disposition', 'attachment; filename=foobar.pdf');
        ctx.attachment('user.xls');
        ctx.body = buffer;
    }
}
```

## 分页前端

```vue
<el-pagination
	:current-page="searchData.currentPage"
	:page-sizes="[5, 10, 20, 50]"
    :page-size="searchData.pageSize"
    :total="totalCount"
    layout="total, sizes, prev, pager, next, jumper"
    @size-change="handleSizeChange"
    @current-change="handleCurrentChange">
</el-pagination>
<script>
private handleSizeChange(currentSize:any){
    // console.log(currentSize);
    this.searchData.pageSize = currentSize;
    this.getUserList();
}
private handleCurrentChange(currentPage:any){
    // console.log(currentPage);
    this.searchData.currentPage = currentPage;
    this.getUserList();
}
</script>
```

## 分页后端

- **后端实现分页查询**
- **前端进行代码调整**

**controller/users.ts**

```ts
public async getUserList(obj){
    const currentPage = parseInt(obj.currentPage) || 1;
    const pageSize = parseInt(obj.pageSize) || 5;
    const users = await this.ctx.model.User.findAll({
        attributes:{
            exclude:['password', 'created_at', 'updated_at']
        },
        limit: pageSize,
        offset: (currentPage - 1) * pageSize
        //      (1 - 1) * 5 = 0
        //      (2 - 1) * 5 = 5
    });
    const totalCount = await this.ctx.model.User.findAndCountAll();
    return {users:users, totalCount:totalCount.count};
}
```

**前端**

```ts
// 分页相关代码
private getUserList(){
    getUsers(this.searchData)
        .then((response:any)=>{
        if(response.status === 200){
            this.tableData = response.data.data.users;
            this.totalCount = response.data.data.totalCount;
            console.log(response.data.data);
        }else{
            (this as any).$message.error(response.data.msg);
        }
    })
        .catch((error)=>{
        (this as any).$message.error(error.response.data.msg);
    });
}
created(): void {
    this.getUserList();
}
private handleSizeChange(currentSize:any){
    // console.log(currentSize);
    this.searchData.pageSize = currentSize;
    this.getUserList();
}
private handleCurrentChange(currentPage:any){
    // console.log(currentPage);
    this.searchData.currentPage = currentPage;
    this.getUserList();
}
```

## 搜索功能上

**使用sequelize的模糊查询**

```ts
public async getUserList(obj){
    const currentPage = parseInt(obj.currentPage) || 1;
    const pageSize = parseInt(obj.pageSize) || 5;
    const {role, origin, type, key} = obj;
    if(key && !role && !origin && !type){
        // 只有搜索的关键字, 那么就需要把包含这个关键的所有类型的用户都查询出来
        const users = await this.ctx.model.User.findAll({
            attributes:{
                exclude:['password', 'created_at', 'updated_at']
            },
            limit: pageSize,
            offset: (currentPage - 1) * pageSize,
            where: {
                [Op.or]: [
                    {username:{[Op.substring]: key}},
                    {email:{[Op.substring]: key}},
                    {phone:{[Op.substring]: key}},
                ],
            }
        });
        const totalCount = await this.ctx.model.User.findAndCountAll({
            where: {
                [Op.or]: [
                    {username:{[Op.substring]: key}},
                    {email:{[Op.substring]: key}},
                    {phone:{[Op.substring]: key}},
                ],
            }
        });
        return {users:users, totalCount:totalCount.count};
    }else{
        const users = await this.ctx.model.User.findAll({
            attributes:{
                exclude:['password', 'created_at', 'updated_at']
            },
            limit: pageSize,
            offset: (currentPage - 1) * pageSize
            //      (1 - 1) * 5 = 0
            //      (2 - 1) * 5 = 5
        });
        const totalCount = await this.ctx.model.User.findAndCountAll();
        return {users:users, totalCount:totalCount.count};
    }
}
```

## 搜索功能中

**当有其他附加条件的时候**

```ts
public async getUserList(obj){
    const currentPage = parseInt(obj.currentPage) || 1;
    const pageSize = parseInt(obj.pageSize) || 5;
    const {role, origin, type, key} = obj;
    const defaultCondition = {
        [Op.or]: [
            {username:{[Op.substring]: key}},
            {email:{[Op.substring]: key}},
            {phone:{[Op.substring]: key}},
        ],
    };
    if(key && !role && !origin && !type){
        // 只有搜索的关键字, 那么就需要把包含这个关键的所有类型的用户都查询出来
        const users = await this.ctx.model.User.findAll({
            attributes:{
                exclude:['password', 'created_at', 'updated_at']
            },
            limit: pageSize,
            offset: (currentPage - 1) * pageSize,
            where: defaultCondition
        });
        const totalCount = await this.ctx.model.User.findAndCountAll({
            where: defaultCondition
        });
        return {users:users, totalCount:totalCount.count};
    }else if(key || role || origin || type){
        // 如果有附加条件, 那么就必须同时满足多个条件
        const conditionList:any[] = [];
        if(key){
            conditionList.push(defaultCondition);
        }
        if(role){

        }
        if(origin){
            // {github: true}
            // {local: true}
            conditionList.push({[origin]:true});
        }
        if(type){
            // {username: {[Op.substring]: 123}}
            // {email: {[Op.substring]: 123}}
            // {phone: {[Op.substring]: 123}}
            conditionList.push({[type]:{[Op.substring]: key}});
        }
        const users = await this.ctx.model.User.findAll({
            attributes:{
                exclude:['password', 'created_at', 'updated_at']
            },
            limit: pageSize,
            offset: (currentPage - 1) * pageSize,
            where: {
                [Op.and]:conditionList
            }
        });
        const totalCount = await this.ctx.model.User.findAndCountAll({
            where: {
                [Op.and]:conditionList
            }
        });
        return {users:users, totalCount:totalCount.count};
    }else{
        const users = await this.ctx.model.User.findAll({
            attributes:{
                exclude:['password', 'created_at', 'updated_at']
            },
            limit: pageSize,
            offset: (currentPage - 1) * pageSize
            //      (1 - 1) * 5 = 0
            //      (2 - 1) * 5 = 5
        });
        const totalCount = await this.ctx.model.User.findAndCountAll();
        return {users:users, totalCount:totalCount.count};
    }
}
```

## 搜索功能下

**搜索的优化**

```ts
public async getUserList(obj){
    const currentPage = parseInt(obj.currentPage) || 1;
    const pageSize = parseInt(obj.pageSize) || 5;
    const {role, origin, type, key} = obj;
    const defaultCondition = {
        [Op.or]: [
            {username:{[Op.substring]: key}},
            {email:{[Op.substring]: key}},
            {phone:{[Op.substring]: key}},
        ],
    };
    if(key || role || origin || type){
        // 如果有附加条件, 那么就必须同时满足多个条件
        const conditionList:any[] = [];
        if(key){
            conditionList.push(defaultCondition);
        }
        if(role){

        }
        if(origin){
            // {github: true}
            // {local: true}
            conditionList.push({[origin]:true});
        }
        if(type){
            // {username: {[Op.substring]: 123}}
            // {email: {[Op.substring]: 123}}
            // {phone: {[Op.substring]: 123}}
            conditionList.push({[type]:{[Op.substring]: key}});
        }
        const users = await this.ctx.model.User.findAll({
            attributes:{
                exclude:['password', 'created_at', 'updated_at']
            },
            limit: pageSize,
            offset: (currentPage - 1) * pageSize,
            where: {
                [Op.and]:conditionList
            }
        });
        const totalCount = await this.ctx.model.User.findAndCountAll({
            where: {
                [Op.and]:conditionList
            }
        });
        return {users:users, totalCount:totalCount.count};
    }
    else{
        const users = await this.ctx.model.User.findAll({
            attributes:{
                exclude:['password', 'created_at', 'updated_at']
            },
            limit: pageSize,
            offset: (currentPage - 1) * pageSize
        });
        const totalCount = await this.ctx.model.User.findAndCountAll();
        return {users:users, totalCount:totalCount.count};
    }
}
```

## 前端操作excel

- **npm install xlsx(前端操作excel)**
- **npm install file-saver(为了保持文件下载的兼容性问题)**

```ts
private exportUser(){
    const user = this.tableData.length ? this.tableData[0] : null;
    const data:any[] = [];
    if(user){
        const cloumnTitles = Object.keys(user);
        data.push(cloumnTitles);
        this.tableData.forEach((user)=>{
            const temp:any[] = [];
            cloumnTitles.forEach((key)=>{
                temp.push(user[key]);
            });
            data.push(temp);
        });
    }
    // 1.根据二维数组生成表格中的数据
    const sheet = XLSX.utils.aoa_to_sheet(data);
    // 2.创建一个新的表格
    const workbook = XLSX.utils.book_new();
    // 3.把数据添加到表格中, 并给这个表格起一个名称
    XLSX.utils.book_append_sheet(workbook, sheet, 'user');
    // 4.将生成好的表格保存到本地
    // XLSX.writeFile(workbook, 'users.xls'); // 有兼容问题
    const wopts:any = { bookType:'xlsx', bookSST:false, type:'array' };
    const wbout = XLSX.write(workbook,wopts);
    saveAs(new Blob([wbout],{type:"application/octet-stream"}), "users.xlsx");
}
```

## 角色管理

**引入restful API的简写方式**

```ts
import { Application } from 'egg';

export default (app: Application) => {
  const { controller, router } = app;

  require('./router/code')(app);
  require('./router/account')(app);
  require('./router/users')(app);

  // router.get('/api/v1/roles', controller.roles.index);
  // router.post('/api/v1/roles', controller.roles.create);
  // router.delete('/api/v1/roles/:id', controller.roles.destroy);
  // router.put('/api/v1/roles/:id', controller.roles.update);
  router.resources('roles', '/api/v1/roles/', controller.roles)
};
```

## 显示编辑角色(前端)

```ts
// 分配角色相关代码
private addRoleDialogVisible = false;
private currentUser = {};
private roles = [];
private currentRoleId = "";
private showAddRoleDialog(user:any){
    this.addRoleDialogVisible = true;
    this.currentUser = user;
    getRoles({})
        .then((response:any)=>{
        if(response.status === 200){
            this.roles = response.data.data.roles;
            (this as any).$message.success('获取角色信息成功');
        }else{
            (this as any).$message.error('获取角色信息失败');
        }
    })
        .catch((error)=>{
        (this as any).$message.error('获取角色信息失败');
    });
}
```

## 角色的分配

- **后端创建表，定义controller和service**
- **前端拿到角色的id和用户的id发送请求进行存储**

```ts
/**
 * @desc 角色用户关系表
 */
import { Column, DataType, Model, Table, CreatedAt, UpdatedAt, ForeignKey} from 'sequelize-typescript';
import {User} from './user'
import {Role} from './role'

@Table
export class UserRole extends Model<UserRole> {

    @Column({
        type: DataType.INTEGER,
        primaryKey: true,
        autoIncrement: true,
        unique: true,
        allowNull: false,
        comment: '主键ID',
    })
    id: number;

    @ForeignKey(()=>User)
    @Column({
        field:'user_id',
        type: DataType.INTEGER,
        allowNull:false,
        unique:true,
    })
    userId: number;

    @ForeignKey(()=>Role)
    @Column({
        field:'role_id',
        type: DataType.INTEGER,
        allowNull:false,
        unique:true,
    })
    roleId: number;

    @CreatedAt
    createdAt: Date;

    @UpdatedAt
    updatedAt: Date;
};
export default () => UserRole;

//controller
import { Controller } from 'egg';
export default class UserRoleController extends Controller {
    public async create() {
        const {ctx} = this;
        const data = ctx.request.body;
        try {
            const user = await ctx.service.userRole.createUserRole(data);
            ctx.success(user);
        } catch (e) {
            ctx.error(400, e.message);
        }
    }import { Service } from 'egg';


export default class UserRole extends Service {

    public async createUserRole(obj) {
        try {
            const data = await this.ctx.model.UserRole.create(obj);
            const userRoleData = data['dataValues'];
            return userRoleData;
        }catch (e) {
            throw new Error('分配角色失败');
        }
    }
}
}

//service
import { Service } from 'egg';


export default class UserRole extends Service {

    public async createUserRole(obj) {
        try {
            const data = await this.ctx.model.UserRole.create(obj);
            const userRoleData = data['dataValues'];
            return userRoleData;
        }catch (e) {
            throw new Error('分配角色失败');
        }
    }
}
```

**前端**

```ts
private addRole(userId:string){
    // console.log(userId, this.currentRoleId);
    this.addRoleDialogVisible = false;
    const obj = {userId: userId, roleId: this.currentRoleId};
    createUserRole(obj)
        .then((response:any)=>{
        if(response.status === 200){
            this.getUserList();
            (this as any).$message.success('分配角色成功');
        }else{
            (this as any).$message.error('分配角色失败');
        }
    })
        .catch((error)=>{
        (this as any).$message.error('分配角色失败');
    });
}
```

## 显示角色(主要是前端)

```ts
 // private getCurrentRoleName(user:any){
private getCurrentRoleName(user: any){
    const roles = user.roles;
    const names:any[] = [];
    roles.forEach((role:any)=>{
        names.push(role.roleName);
    })
    return names.join(' | ');
}
private get currentRoleName(){
    if(JSON.stringify(this.currentUser) === '{}') return "";
    return this.getCurrentRoleName(this.currentUser);
}
```

## 删除角色

**解决一个困惑，由于使用的是sequelize，所有在使用多对多的关系的时候它自动进行了关联的查询，删除了关系表也就意味着删除了某个表中对另一个表的某条数据的引用，所以只需要删除关系表即可**

**后端**

```ts
//controller
public async destroy(){
    const {ctx} = this;
    const {userId} = ctx.params;
    const {roleId} = ctx.request.body;
    try {
        await ctx.service.userRole.destroyUser(userId, roleId);
        ctx.success();
    } catch (e) {
        ctx.error(400, e.message);
    }
}
//service
public async destroyUser(userId, roleId){
    try {
        const data = await this.ctx.model.UserRole.destroy({
            where:{userId:userId, roleId: roleId}
        });
        if(data <= 0){
            throw new Error('删除角色失败');
        }
    }catch (e) {
        throw new Error('删除角色失败');
    }
}
```

**前端**

```ts
private removeRole(userId:string){
    this.addRoleDialogVisible = false;
    const obj = {userId: userId, roleId: this.currentRoleId};
    destroyUserRole(userId, obj)
        .then((response:any)=>{
        if(response.status === 200){
            this.getUserList();
            (this as any).$message.success('移出角色成功');
        }else{
            (this as any).$message.error('移出角色失败');
        }
    })
        .catch((error)=>{
        (this as any).$message.error('移出角色失败');
    });
}
```

## 权限控制表的设计

```js
/*
常见权限:
菜单权限:能否使用某个菜单
权限名称/权限描述/权限是否可用/菜单对应的界面地址/层级关系
rights_name/rights_desc/rights_state/rights_path/rights_pid/rights_type
1 菜单权限 是否可以使用菜单 true null 0
2 用户管理 是否可以使用用户管理菜单 true null 1

路由权限:能否跳转到某个界面
权限名称/权限描述/权限是否可用/当前权限对应的路由地址/层级关系
rights_name/rights_desc/rights_state/rights_path/rights_pid/rights_type

请求权限:能否发送某个请求
权限名称/权限描述/权限是否可用/当前权限对应的请求地址/当前权限对应的请求类型/层级关系
rights_name/rights_desc/rights_state/rights_path/rights_method/rights_pid/rights_type

* */
```

## 显示权限列表

```vue
<el-tree :data="rightsArray"
         :props="defaultProps">
</el-tree>
<script>
// 添加权限相关代码
// 树形控件需要显示的内容
// 树形控件会自动遍历取出数组中的每一个对象
// 然后根据defaultProps的label指定的字段来显示对应的内容
// 然后根据defaultProps的children指定的字段来决定是否有下一级内容
private rightsArray = [{
	label: '一级 1',
	children: [{
    	label: '二级 1-1',
        children: [{
        	label: '三级 1-1-1'
        }]
    }]
}];
private defaultProps = {
    children: 'children',
    label: 'label'
};
</script>
```

## 获取权限列表

- **后端根据返回的数据库中的数据再进行过滤，配置成为elementui需要的类型**

```ts
public async index() {
    const {ctx} = this;
    try {
        if(JSON.stringify(ctx.query) !== '{}'){
            let rights = await ctx.service.rights.getRightsList(ctx.query);
            ctx.success(rights);
        }else{
            let rights = await ctx.service.rights.getAllRights();
            rights = rights.filter((outItem:any)=>{
                rights.forEach((inItem:any)=>{
                    if(outItem.id === inItem.pid){
                        outItem.children ? '' : outItem.children = [];
                        outItem.children.push(inItem);
                    }
                })
                return outItem.level === 0;
            });
            ctx.success(rights);
        }
    }catch (e) {
        ctx.error(500, e.message);
    }
}
```

- **前端更改设置即可**

```ts
 // 添加权限相关代码
private rightsArray = [{
    label: '一级 1',
    children: [{
        label: '二级 1-1',
        children: [{
            label: '三级 1-1-1'
        }]
    }]
}];
private defaultProps = {
    children: 'children',
    label: 'rightsName'
};
private addRightsDialogVisible = false;
private showRightsDialogVisible(){
    this.addRightsDialogVisible = true;
};
private getRightsList(){
    getRights({})
        .then((response:any)=>{
        if(response.status === 200){
            console.log(response.data.data);
            this.rightsArray = response.data.data;
            (this as any).$message.success('获取权限列表成功');
        }else{
            (this as any).$message.error(response.data.msg);
        }
    })
        .catch((error)=>{
        (this as any).$message.error(error.response.data.msg);
    });
}
```

## 设置选中状态

**查看element-ui的官方文档即可**

```vue
<el-tree :data="rightsArray"
         :props="defaultProps"
         :default-expand-all="true"
         show-checkbox
         ref="tree"
         node-key="id"
         :default-checked-keys="defaultCheckedKeys">
</el-tree>
<script>
private defaultCheckedKeys = [44]; // 指定默认选中的权限
private addRightsDialogVisible = false;
@Ref() readonly tree?: ElTree<any, any>;
private showRightsDialogVisible(){
	this.addRightsDialogVisible = true;
	this.tree ? this.tree.setCheckedKeys(this.defaultCheckedKeys) : '';
};
</script>
```

## 获取操作的权限

```ts
private addRights(){
    this.addRightsDialogVisible = false;
    // 1.获取当前所有选中和半选中的权限
    const allCheckedKeys = [...this.tree!.getCheckedKeys(), ...this.tree!.getHalfCheckedKeys()];
    // 2.获取新增的权限
    const addCheckedKeys = allCheckedKeys.filter((id)=>{
        return !this.defaultCheckedKeys.includes(id);
    });
    // 3.获取操作之后以前的剩余的权限
    const oldCheckedKeys = allCheckedKeys.filter((id)=>{
        return this.defaultCheckedKeys.includes(id);
    });
    // 4.获取删除的权限
    const removeCheckedKeys = this.defaultCheckedKeys.filter((id)=>{
        return !oldCheckedKeys.includes(id);
    });
    console.log('原来的', this.defaultCheckedKeys);
    console.log('新增的', addCheckedKeys);
    console.log('剩余的', oldCheckedKeys);
    console.log('删除的', removeCheckedKeys);
}
```

## 实现分配

**后端添加一张角色对应权限的表**

```ts
/**
 * @desc 角色权限关系表
 */
import { Column, DataType, Model, Table, CreatedAt, UpdatedAt, ForeignKey} from 'sequelize-typescript';
import {Rights} from './rights'
import {Role} from './role'

@Table
export class RoleRights extends Model<RoleRights> {

    @Column({
        type: DataType.INTEGER,
        primaryKey: true,
        autoIncrement: true,
        unique: true,
        allowNull: false,
        comment: '主键ID',
    })
    id: number;

    @ForeignKey(()=>Role)
    @Column({
        field:'role_id',
        type: DataType.INTEGER,
        allowNull:false,
    })
    roleId: number;

    @ForeignKey(()=>Rights)
    @Column({
        field:'rights_id',
        type: DataType.INTEGER,
        allowNull:false,
    })
    rightsId: number;

    @CreatedAt
    createdAt: Date;

    @UpdatedAt
    updatedAt: Date;
};
export default () => RoleRights;
```

**controller/roleRights.ts**

```ts
import { Controller } from 'egg';
export default class RoleRightsController extends Controller {
    public async create() {
        const {ctx} = this;
        const data = ctx.request.body;
        const {roleId, rightsIds} = data;
        let transaction;
        try {
            transaction = await ctx.model.transaction();
            for(let i = 0; i < rightsIds.length; i++){
                const obj = {roleId:roleId, rightsId:rightsIds[i]};
                await ctx.service.roleRights.createRoleRights(obj);
            }
            await transaction.commit();
            ctx.success();
        } catch (e) {
            await transaction.rollback();
            ctx.error(400, e.message);
        }
    }
    public async destroy(){
        const {ctx} = this;
        const {roleId} = ctx.params;
        const {rightsIds} = ctx.request.body;
        let transaction;
        try {
            transaction = await ctx.model.transaction();
            for(let i = 0; i < rightsIds.length; i++){
                await ctx.service.roleRights.destroyRoleRights(roleId, rightsIds[i]);
            }
            await transaction.commit();
            ctx.success();
        } catch (e) {
            await transaction.rollback();
            ctx.error(400, e.message);
        }
    }
}

```

**service/roleRights.ts**

```ts
import {Service} from 'egg';

export default class RoleRights extends Service {

    public async createRoleRights(obj) {
        try {
            const roleRights = await this.ctx.model.RoleRights.findOne({
                where: {
                    roleId: obj.roleId,
                    rightsId: obj.rightsId
                }
            });
            if (roleRights) return;
            const data = await this.ctx.model.RoleRights.create(obj);
            const userRoleData = data['dataValues'];
            return userRoleData;
        } catch (e) {
            throw new Error('分配权限失败');
        }
    }

    public async destroyRoleRights(roleId, rightsId) {
        const data = await this.ctx.model.RoleRights.destroy({
            where: {
                roleId: roleId,
                rightsId: rightsId
            }
        });
        if (data <= 0) {
            throw new Error('移出权限失败');
        }
    }
}
```

**前端**

```ts
private addRights(){
    this.addRightsDialogVisible = false;
    // 1.获取当前所有选中和半选中的权限
    const allCheckedKeys = [...this.tree!.getCheckedKeys(), ...this.tree!.getHalfCheckedKeys()];
    // 2.获取新增的权限
    const addCheckedKeys = allCheckedKeys.filter((id)=>{
        return !this.defaultCheckedKeys.includes(id);
    });
    // 3.获取操作之后以前的剩余的权限
    const oldCheckedKeys = allCheckedKeys.filter((id)=>{
        return this.defaultCheckedKeys.includes(id);
    });
    // 4.获取删除的权限
    const removeCheckedKeys = this.defaultCheckedKeys.filter((id: any)=>{
        return !oldCheckedKeys.includes(id);
    });
    // 5.新增权限
    if(addCheckedKeys.length > 0){
        createRoleRights({roleId:this.currentRole.id, rightsIds:addCheckedKeys})
            .then((response:any)=>{
            if(response.status === 200){
                (this as any).$message.success('添加权限成功');
            }else{
                (this as any).$message.error(response.data.msg);
            }
        })
            .catch((error)=>{
            (this as any).$message.error(error.response.data.msg);
        });
    }
    // 6.移出权限
    if(removeCheckedKeys.length > 0){
        destroyRoleRights(this.currentRole.id, {rightsIds:removeCheckedKeys})
            .then((response:any)=>{
            if(response.status === 200){
                (this as any).$message.success('移出权限成功');
            }else{
                (this as any).$message.error(response.data.msg);
            }
        })
            .catch((error)=>{
            (this as any).$message.error(error.response.data.msg);
        });
    }
}
```

## 获取默认权限

- **后端设置表之间的多对多的关系**

```ts
import { Service } from 'egg';
import {Rights} from "../model/rights";
const { Op } = require("sequelize");

/**
 * Test Service
 */
export default class Roles extends Service {
    public async getAllRoles(){
        const roles = await this.ctx.model.Role.findAll({
            attributes:{
                exclude:['createdAt', 'updatedAt']
            }
        });
        return roles;
    }
    public async getRolesList(obj) {
        const currentPage = parseInt(obj.currentPage) || 1;
        const pageSize = parseInt(obj.pageSize) || 5;
        const {key} = obj;
        if(key){
            const roles = await this.ctx.model.Role.findAll({
                attributes:{
                    exclude:['createdAt', 'updatedAt']
                },
                limit: pageSize,
                offset: (currentPage - 1) * pageSize,
                where: {
                    [Op.or]:[
                        {roleName:{[Op.substring]: key}},
                        {roleDesc:{[Op.substring]: key}},
                    ]
                }
            });
            const totalCount = await this.ctx.model.Role.findAndCountAll({
                where: {
                    [Op.or]:[
                        {roleName:{[Op.substring]: key}},
                        {roleDesc:{[Op.substring]: key}},
                    ]
                }
            });
            return {roles:roles, totalCount:totalCount.count};
        }
        else{
            const roles = await this.ctx.model.Role.findAll({
                attributes:{
                    exclude:['createdAt', 'updatedAt']
                },
                limit: pageSize,
                offset: (currentPage - 1) * pageSize
            });
            const totalCount = await this.ctx.model.Role.findAndCountAll();
            return {roles:roles, totalCount:totalCount.count};
        }
    }
    public async createRole(obj){
        try {
            const data = await this.ctx.model.Role.create(obj);
            const roleData = data['dataValues'];
            return roleData;
        }catch (e) {
            throw new Error("创建角色失败");
        }
    }
    public async destroyRole(id){
        const role = await this.ctx.model.Role.findByPk(id);
        if(role){
            const data = await this.ctx.model.Role.destroy({
                where:{id:id}
            });
            if(data > 0){
                return role;
            }else{
                throw new Error('删除角色失败');
            }
        }else{
            throw new Error('删除的角色不存在');
        }
    }
    public async updateRole(id, obj){
        const role = await this.ctx.model.Role.findByPk(id);
        if(role){
            const data = await this.ctx.model.Role.update(obj, {
                where:{
                    id:id
                }
            });
            if(data.length > 0){
                return role;
            }else{
                throw new Error('更新角色失败');
            }
        }else{
            throw new Error('更新的角色不存在');
        }
    }
}
```

**前端**

```ts
private showRightsDialogVisible(role:any){
    this.addRightsDialogVisible = true;
    this.defaultCheckedKeys = []; // 清空上一次的默认权限
    this.currentRole = role;
    this.currentRole.rights.forEach((item:any)=>{ // 生成这一次默认权限
        if(item.level === 2){
            this.defaultCheckedKeys.push(item.id);
        }
    });
    this.tree ? this.tree.setCheckedKeys(this.defaultCheckedKeys) : ''; // 设置这一次的默认权限
};
```

## 显示默认权限

**后端生成树形控件所需要的数据的格式**

```ts
public async index() {
    const {ctx} = this;
    try {
        if(JSON.stringify(ctx.query) !== '{}'){
            let rights = await ctx.service.rights.getRightsList(ctx.query);
            ctx.success(rights);
        }else{
            let rights = await ctx.service.rights.getAllRights();
            rights = rights.filter((outItem:any)=>{
                rights.forEach((inItem:any)=>{
                    if(outItem.id === inItem.pid){
                        outItem.children ? '' : outItem.children = [];
                        outItem.children.push(inItem);
                    }
                })
                return outItem.level === 0;
            });
            ctx.success(rights);
        }
    }catch (e) {
        ctx.error(500, e.message);
    }
}
```

## 显示默认权限(前端)

```vue
<el-table-column type="expand">
    <template slot-scope="scope">
        <el-row v-for="(item1, index1) in scope.row.rightsTree"
                :key="item1.id"
                :class="['bottom-border', index1 === 0 ? 'top-border': '', 'content-center']">
            <el-col :span="6">
                <el-tag type="danger">{{item1.rightsName}}</el-tag>
            </el-col>
            <el-col :span="18">
                <el-row v-for="(item2, index2) in item1.children"
                        :key="item2.id"
                        :class="[ index2 !== 0 ? 'top-border': '', 'content-center']">
                    <el-col :span="6">
                        <el-tag type="warning">{{item2.rightsName}}</el-tag>
                    </el-col>
                    <el-col :span="18">
                        <el-tag type="success"
                                v-for="(item3, index3) in item2.children"
                                :key="item3.id">{{item3.rightsName}}</el-tag>
                    </el-col>
                </el-row>
            </el-col>
        </el-row>
    </template>
</el-table-column>
```

## 登录时获取权限

- **通过关联查询将用户对应的角色和角色对应的权限查出来**
- **然后由于数据较大，cookie已经存不下token了，进行token的一个更改**

```ts
public async index() {
    const {ctx} = this;
    try {
        // 1.校验数据和验证码
        this.validateUserInfo();
        const data = ctx.request.body;
        ctx.helper.verifyImageCode(data.captcha);
        // 2.将校验通过的数据保存到数据库中
        const user = await ctx.service.user.getUser(data);
        delete user.password;
        // 校验用户是否可用
        if(!user.userState){
            return ctx.error(400, '用户已经注销');
        }
        // 3.生成JWT令牌
        const obj:any = {};
        obj.username = user.username;
        obj.email = user.email;
        obj.phone = user.phone;
        const token = jwt.sign(obj, this.config.keys, {expiresIn: '7 days'});
        ctx.cookies.set('token', token, {
            path:'/',
            maxAge: 24 * 60 * 60 * 1000,
            // 注意点: 如果httpOnly: true, 那么前端是无法获取这个cookie
            httpOnly: false,
            signed:false,
        });
        ctx.success(user);
    } catch (e) {
        if (e.errors) {
            ctx.error(400, e.errors);
        } else {
            ctx.error(400, e.message);
        }
    }
}
```

## 显示登录用户信息

**后端返回完整的用户数据**

```ts
public async getUser({username, email, phone, password}){
    password = this.ctx.helper.encryptText(password);
    let res;
    if(email){
        res = await this.findUser({email:email, password:password});
    }else if(phone){
        res = await this.findUser({phone:phone, password:password});
    }else if(username){
        res = await this.findUser({username:username, password:password});
    }
    try {
        // 注意点: 在sequelize中, 如果想返回虚拟字段的值, 那么就不能从dataValues中获取
        //          dataValues中保存的都是从数据库中查询到的数据
        return res;
    }catch (e) {
        throw new Error('用户名或者密码不正确');
    }
}
```

**前端保存到sessionStorage中**

```ts
private onSubmit(){
    this.form.validate((flag)=>{
        if(flag){
            loginUser(this.loginData)
                .then((response:any)=>{
                if(response.status === 200){
                    // 保存登录状态
                    sessionStorage.setItem('userInfo', JSON.stringify(response.data.data));
                    // 跳转到管理界面
                    this.$router.push('/admin');
                }else{
                    (this as any).$message.error(response.data.msg);
                }
            })
                .catch((error)=>{
                (this as any).$message.error(error.response.data.msg);
            });
        }else{
            (this as any).$message.error('数据格式不对');
        }
    });
};
```

**前端使用sessionStorage**

```ts
private logout(){
    Cookies.remove('token');
    sessionStorage.removeItem('acitvePath');
    sessionStorage.removeItem('userInfo');
    this.$router.push('/login');
}
private changeDefaultActivePath(path:string){
    this.defaultActivePath = path;
    sessionStorage.setItem('acitvePath', path);
}
private userInfo = {};
created(): void {
    const path = sessionStorage.getItem('acitvePath');
	this.defaultActivePath = path ? path : '';
	const userInfo = sessionStorage.getItem('userInfo');
	if(userInfo){
    	this.userInfo = JSON.parse(userInfo);
	}
}
```

## 菜单权限控制

- **根据sessionStorage中的用户信息使用计算属性获取菜单权限**

```ts
private get currentMenus(){
    for(let i = 0; i < this.userInfo.rightsTree.length; i++){
        const rights = this.userInfo.rightsTree[i];
        if(rights.rightsType === 'menu'){
            return rights.children;
        }
    }
}
```

- **根据计算属性渲染**

```vue
<el-submenu :index="item.rightsPath"
            v-for="item in currentMenus"
            :key="item.id">
    <template slot="title">
        <i class="el-icon-setting"></i>
        <span>{{item.rightsName}}</span>
    </template>
    <!--二级菜单-->
    <el-menu-item-group>
        <el-menu-item :index="subItem.rightsPath"
                        v-for="subItem in item.children"
                        :key="subItem.id"
                        @click="changeDefaultActivePath(subItem.rightsPath)">
            <template slot="title">
                <i class="el-icon-setting"></i>
                <span>{{subItem.rightsName}}</span>
            </template>
        </el-menu-item>
    </el-menu-item-group>
</el-submenu>
```

## 权限过滤(解决重名权限不加载的问题)

```ts
private async findUser(options){
    const user:any =  await this.ctx.model.User.findOne({
        where: options,
        include:[
            {
                model:Role,
                include:[
                    { model:Rights }
                ]
            }
        ],
    });
    // 1.获取当前登录用户拥有的所有权限
    let allRights:any[] = [];
    user.roles.forEach((role)=>{
        role.rights.forEach((item)=>{
            allRights.push(item);
        });
    });
    // 2.剔除重复的权限
    /*
        {id: 1, rightsName: '权限管理', rightsType:'menu'},
        {id: 2, rightsName: '角色列表', rightsType:'menu'},
        {id: 3, rightsName: '权限列表', rightsType:'menu'},
        {id: 4, rightsName: '权限管理', rightsType:'router'},
        {id: 5, rightsName: '角色列表', rightsType:'router'},
        {id: 6, rightsName: '权限列表', rightsType:'router'},
        {id: 7, rightsName: '权限管理', rightsType:'menu'},

         const temp = {
            权限管理:true
         };
        * */
    const temp = {};
    allRights = allRights.reduce((arr, item)=>{
        if(!temp[item.dataValues.id]){
            arr.push(item);
            temp[item.dataValues.id] = true;
        }
        return arr;
    }, []);
    // 3.生成权限树
    allRights = allRights.filter((outItem)=>{
        allRights.forEach((inItem)=>{
            if(outItem.dataValues.id === inItem.dataValues.pid){
                outItem.dataValues.children ? '' : outItem.dataValues.children = [];
                outItem.dataValues.children.push(inItem);
            }
        });
        if(outItem.dataValues.level === 0) return true;
    });
    user.dataValues.rightsTree = allRights;
    return user;
}
```

## 路由控制

```ts
// 添加路由守卫, 实现权限控制
router.beforeEach((to, from, next) => {
    // 1.如果访问的是注册或者登录, 那么就放行
    if(to.path === '/login' || to.path === '/register'){
        return next();
    }
    // 2.获取当前的登录状态
    const token = Cookies.get('token');
    // 3.如果访问的是其它路由地址, 那么就需要判断是否已经登录
    //   如果已经登录, 那么就放行, 如果没有登录, 那么就强制跳转到登录界面
    if(!token){
        return next('/login');
    }
    const routerRights = getRouterRights();
    const flag =  isNext(routerRights, to.path);
    if(flag){
        next();
    }else{
        next(false);
    }
});
const isNext = (routerRights:any, path:string)=>{
    if(routerRights.rightsPath === path) return true;
    if(routerRights.children){
        for(let i = 0; i < routerRights.children.length; i++){
            const item = routerRights.children[i];
            if(isNext(item, path)) return true;
        }
    }
    return false;
}
const getRouterRights = ()=>{
    const data = sessionStorage.getItem('userInfo');
    if(!data) return null;
    const userInfo = JSON.parse(data);
    const routerRights = userInfo.rightsTree.filter((rights)=>{
        if(rights.rightsType === 'router') return rights;
    });
    return routerRights[0];
}
```

## 请求权限的控制

```ts
import axios from 'axios'
const source = axios.CancelToken.source();

// 进行一些全局配置
axios.defaults.baseURL = 'http://127.0.0.1:7001';
axios.defaults.timeout = 5000;
axios.defaults.withCredentials = true; // 让axios发送请求的时候带上cookie

const isRequest = (actionRights:any, path:string, method:string)=>{
    if(actionRights.rightsPath === path && actionRights.rightsMethod === method) return true;
    if(actionRights.children){
        for(let i = 0; i < actionRights.children.length; i++){
            const item = actionRights.children[i];
            if(isRequest(item, path, method)) return true;
        }
    }
    return false;
}
const getActionRights = ()=>{
    const data = sessionStorage.getItem('userInfo');
    if(!data) return null;
    const userInfo = JSON.parse(data);
    const actionRights = userInfo.rightsTree.filter((rights:any)=>{
        if(rights.rightsType === 'action') return rights;
    });
    return actionRights[0];
};
const actionRights = getActionRights();

// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    const flag = isRequest(actionRights, config.url || '', config.method || '');
    if(!flag){
        config.cancelToken = source.token;
        source.cancel('没有对应的请求权限');
    }
    // 在发送请求之前做些什么
    return config
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error)
});
// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error)
});
```

## 请求权限控制的bug解决

- **部分请求不需要控制：比如登录或者注册接口**

  **这个bug可以通过判断前面是否带有/api/v1进行解决**

- **带参数的请求路径无法用等于号匹配，得用正则表达式进行匹配**

```ts
const isRequest = (actionRights:any, path:string, method:string)=>{
    const reg = new RegExp(`^${actionRights.rightsPath}(/[0-9]*)?$`, 'i');
    if(reg.test(path) && actionRights.rightsMethod === method) return true;
    if(actionRights.children){
        for(let i = 0; i < actionRights.children.length; i++){
            const item = actionRights.children[i];
            if(isRequest(item, path, method)) return true;
        }
    }
    return false;
}
const getActionRights = ()=>{
    const data = sessionStorage.getItem('userInfo');
    if(!data) return null;
    const userInfo = JSON.parse(data);
    const actionRights = userInfo.rightsTree.filter((rights:any)=>{
        if(rights.rightsType === 'action') return rights;
    });
    return actionRights[0];
};
const actionRights = getActionRights();

// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    const curPath = config.url || '';
    const curMethod = config.method || '';
    if(curPath.startsWith('/api/v1')){
        const flag = isRequest(actionRights, curPath, curMethod);
        if(!flag){
            config.cancelToken = source.token;
            source.cancel('没有对应的请求权限');
        }
    }
    // 在发送请求之前做些什么
    return config
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error)
});
```

## 后端的请求权限

- **后端请求权限设置是为了更加安全**
- **通过更改middleware中的auth中间件进行权限控制**
- **在登录的时候将用户信息存储在session中，在中间件中即可直接取出**

```ts
const isRequest = (actionRights:any, path:string, method:string)=>{
    const reg = new RegExp(`^${actionRights.rightsPath}(/[0-9]*)?$`, 'i');
    if(reg.test(path) && actionRights.rightsMethod === method) return true;
    if(actionRights.children){
        for(let i = 0; i < actionRights.children.length; i++){
            const item = actionRights.children[i];
            if(isRequest(item, path, method)) return true;
        }
    }
    return false;
}
const getActionRights = (ctx)=>{
    const userInfo = ctx.session.user;
    if(!userInfo) return null;
    const actionRights = userInfo.rightsTree.filter((rights:any)=>{
        if(rights.rightsType === 'action') return rights;
    });
    return actionRights[0];
};
let actionRights;
module.exports = (_options, app) => {
    return async function (ctx, next) {
        // /api/vi/users
        // /api/vi/users?page=1&pageSize=5;
        let curPath = ctx.url.toLowerCase();
        const curMethod = ctx.request.method.toLowerCase();
        if(!curPath.startsWith('/api/v1')){
            // 不需要权限控制
            await next();
            return;
        }
        if(!actionRights){
            actionRights = getActionRights(ctx);
        }
        if(!actionRights){
            ctx.error(400, '没有权限');
            return;
        }
        const idx = curPath.indexOf('?');
        if(idx !== -1){
            // /api/vi/users?page=1&pageSize=5; -> /api/vi/users
            curPath = curPath.substr(0, idx);
        }
        const flag = isRequest(actionRights, curPath, curMethod);
        if(flag){
            // 3.获取客户端传递过来的JWT令牌
            const token = ctx.cookies.get('token', {
                signed: false,
            });
            // 4.判断客户端有没有传递JWT令牌
            if(token){
                try {
                    await jwt.verify(token, app.config.keys);
                    await next();
                }catch (e) {
                    ctx.error(400, '没有权限');
                }
            }else{
                ctx.error(400, '没有权限');
            }
        }else{
            ctx.error(400, '没有权限');
        }
    }
};
```

