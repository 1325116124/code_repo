# React项目

## 准备工作

```js
//首先create-react-app react-admin_client创建项目
//然后下载serve中间件，npm run build
//在serve build可以在生产环境中运行
```

## 初始化项目目录

![image-20210730122031955](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210730122031955.png)

## 配置

```js
//安装antd: npm install antd --save
//在App.js中引入对应的组件，但此时没有相应的样式，如果在index.js中引入antd.css则会非常的大
//按需引入：npm install react-app-rewired babel-plugin-import
//npm install less less-loader
//创建一个config.overrides.js文件
//    config.overrides.js
const {override,fixBabelImports,addLessLoader} = require("customize-cra")

module.exports = override(
    //针对antd实现按需打包：根据import来打包（使用babel-plugin-import）
    fixBabelImports('import',{
      libraryName:'antd',
      libraryDirectory:'es',
      style:'css',//自动打包相关的样式
    }),
    //使用less-loader对源码中的less的变量进行重新指定
    addLessLoader({
      modifyVars: { '@primary-color': '#1DA57A' },
      javascriptEnabled: true,
    },)
)
//再将package.json中的代码段换成：
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-scripts eject"
},
```

## 引入路由

- **npm install react-router-dom**
- **在pages页面中新建页面，并暴露组件**
- **在App.js中引入并配置**

```js
//根组件
import React from "react"
import {Button} from 'antd'
import {BrowserRouter,Link,Route,Switch} from "react-router-dom";

import Login from "./pages/login/login";
import Admin from "./pages/admin/admin";

export default class App extends React.Component{
  render(){
    return(
        <div>
         <BrowserRouter>
           <Switch>
             <Route path={'/login'} component={Login}></Route>
             <Route path={'/'} component={Admin}></Route>
           </Switch>
         </BrowserRouter>
        </div>
    )
  }
}
```

## 引入antd组件进行布局

```js
import React from "react"
import { Form, Input, Button } from 'antd';
import { UserOutlined, LockOutlined } from '@ant-design/icons';

import "./login.less"
import logo from "./images/bizhi3.png"
/*
登录的路由组件
 */
export default class Login extends React.Component{
  onFinish = () => {

  }
  render(){
    return (
        <div className="login">
          <header className='login-header'>
            <img src={logo} alt=""/>
            <h1>React项目: 后台管理系统</h1>
          </header>
          <section className='login-content'>
            <h2>用户登录</h2>
            <Form
                name="normal_login"
                className="login-form"
                initialValues={{
                  remember: true,
                }}
                onFinish={this.onFinish}
            >
              <Form.Item name="username">
                <Input prefix={<UserOutlined className="site-form-item-icon" style={{color: 'rgba(0,0,0,0.25)'}}/>} placeholder="用户名" />
              </Form.Item>
              <Form.Item name="password">
                <Input
                    prefix={<LockOutlined className="site-form-item-icon" style={{color: 'rgba(0,0,0,0.25)'}}/>}
                    type="password"
                    placeholder="密码"
                />
              </Form.Item>
              <Form.Item>
                <Button type="primary" htmlType="submit" className="login-form-button">
                  Log in
                </Button>
              </Form.Item>
            </Form>
          </section>
        </div>
    )
  }
}
```

## 表单验证

```js
import React from "react"
import { Form, Input, Button } from 'antd';
import { UserOutlined, LockOutlined } from '@ant-design/icons';

import "./login.less"
import logo from "./images/bizhi3.png"
/*
登录的路由组件
 */
export default class Login extends React.Component{
  onFinish = (values) => {
    console.log('Received values of form: ', values);
  };
  render(){
    return (
        <div className="login">
          <header className='login-header'>
            <img src={logo} alt=""/>
            <h1>React项目: 后台管理系统</h1>
          </header>
          <section className='login-content'>
            <h2>用户登录</h2>
            <Form
                name="normal_login"
                className="login-form"
                initialValues={{
                  remember: true,
                }}
                onFinish={this.onFinish}
            >
              <Form.Item
                  name="username"
                  //声明式验证
                  rules={[
                    {
                      required: true,
                      message: '用户名必须输入！',
                    },
                    {
                      min: 4,
                      message: '用户名至少四位',
                    },
                    {
                      max: 12,
                      message: '用户名最多12位',
                    },
                    {
                      pattern:/^[a-zA-Z0-9_]+$/,
                      message: '用户名必须是英文、数字或下划线组成',
                    },
                  ]}
              >
                <Input prefix={<UserOutlined className="site-form-item-icon" />} placeholder="用户名" />
              </Form.Item>
              <Form.Item
                  name="password"
                  rules={[
                    {
                      required: true,
                      message: 'Please input your Password!',
                    },
                  ]}
              >
                <Input
                    prefix={<LockOutlined className="site-form-item-icon" />}
                    type="password"
                    placeholder="密码"
                />
              </Form.Item>

              <Form.Item>
                <Button type="primary" htmlType="submit" className="login-form-button">
                  Log in
                </Button>
              </Form.Item>
            </Form>
          </section>
        </div>
    )
  }
}
```

## 自定义规则

```js
import React from "react"
import { Form, Input, Button } from 'antd';
import { UserOutlined, LockOutlined } from '@ant-design/icons';

import "./login.less"
import logo from "./images/bizhi3.png"
/*
登录的路由组件
 */
export default class Login extends React.Component{
  onFinish = (values) => {
    console.log('Received values of form: ', values);
  };
  //自定义规则
  validatorPwd = (rules,value,callback) => {
    // callback必须被调用，主要是用来返回错误或成功信息
    if(!value){
      callback("密码必须输入")
    }else if(value.length<=4){
      callback("密码至少为四位")
    }else if(value.length>=12){
      callback("密码至多12位")
    }else if(!/^[a-zA-Z0-9_]+$/.test(value)){
      callback("密码必须是英文、数字或下划线组成")
    }else {
      callback()
    }
  }
  render(){
    return (
        <div className="login">
          <header className='login-header'>
            <img src={logo} alt=""/>
            <h1>React项目: 后台管理系统</h1>
          </header>
          <section className='login-content'>
            <h2>用户登录</h2>
            <Form
                name="normal_login"
                className="login-form"
                initialValues={{
                  remember: true,
                }}
                onFinish={this.onFinish}
            >
              <Form.Item
                  name="password"
                  rules={[
                    {
                      validator:this.validatorPwd
                    },
                  ]}
              >
                <Input
                    prefix={<LockOutlined className="site-form-item-icon" />}
                    type="password"
                    placeholder="密码"
                />
              </Form.Item>

              <Form.Item>
                <Button type="primary" htmlType="submit" className="login-form-button">
                  Log in
                </Button>
              </Form.Item>
            </Form>
          </section>
        </div>
    )
  }
}
```

## 提交表单数据

```js
import React from "react"
import { Form, Input, Button } from 'antd';
import { UserOutlined, LockOutlined } from '@ant-design/icons';

import "./login.less"
import logo from "./images/bizhi3.png"
/*
登录的路由组件
 */
export default class Login extends React.Component{
  onFinish = (values) => {
    console.log('Received values of form: ', values);
  };
  onFinishFailed = ({ values, errorFields, outOfDate }) => {
    console.log('111',outOfDate)
  }
  render(){
    return (
        <div className="login">
            <Form
                name="normal_login"
                className="login-form"
                initialValues={{
                  remember: true,
                }}
                onFinish={this.onFinish}
                onFinishFailed={this.onFinishFailed}
            >
              <Form.Item>
                <Button type="primary" htmlType="submit" className="login-form-button">
                  Log in
                </Button>
              </Form.Item>
            </Form>
          </section>
        </div>
    )
  }
}
```

## 封装axios并且封装请求模块

![image-20210731123327042](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210731123327042.png)

```js
//能发送异步异步ajax请求的函数模块，封装axios模块
//函数的返回值是一个promise对象
import axios from "axios";
export default function ajax(url,data={},type="GET"){
  if(type === 'GET'){
    return axios.get(url,{params:data})
  }else {
    return axios.post(url,data)
  }
}
```

```js
/*
要求：能根据接口文档定义接口请求方式
包含应用中所有接口请求函数的模块
每个函数的返回值都是promise
 */
import ajax from "./ajax";

export const reqLogin = (username,password) => ajax('/login',{username,password},'POST')

export const reqAddUser = (user) => ajax('/manage/user/add',user,'POST')

```

## 在package.json中配置proxy解决跨域

```js
  "proxy": "http://localhost:5000"
```

## 数据的请求接收

```js
export default class Login extends React.Component{
  onFinish = async (values) => {
    try{
      const response = await reqLogin(values.username,values.password)
      console.log("成功",response.data)
    }catch (err){
      console.log(err);
    }
    // .then(response => {
    //   console.log("成功",response.data)
    // }).catch(err => {
    //   console.log("失败",err)
    // }) //alt + <
    /*
    async和await：
    作用：1.简化promise对象的使用：不用再使用then()来指定成功/失败的回调函数
    以同步编码方式实现异步流程
    2.哪里写await：在返回promise的表达式左侧写await，不想要promise，想要promise异步执行的成功的value数据
    3.哪里写async：await所在函数定义的左侧
    */

  };

```

## 优化ajax

```js
export default function ajax(url,data={},type="GET"){
  let promise = {}
  return new Promise((resolve, reject) => {
    if(type === 'GET'){
      promise = axios.get(url,{params:data})
    }else {
      promise = axios.post(url,data)
    }
    promise.then(response => {
      resolve(response)
    }).catch(error => {
      message.error("请求出错了:" +  error.message )
    })
  })
}
```

## 实现登录

```js
export default class Login extends React.Component{
  onFinish = async (values) => {
    const result = await reqLogin(values.username,values.password)
    if(result.status === 0){
      message.success("登录成功")
      this.props.history.replace("/")
    }else {
      message.error(result.msg)
    }
  }
}
```

## 将user保存在内存中（实现未登录跳转到登录）

```js
//   memoryyUtils.js
const memoryUtils = {
  user:{}
}
export default memoryUtils

//   admin.js
import React from "react"
import memoryUtils from "../../utils/memoryUtils";
import {Redirect} from "react-router-dom"
/*
后台管理的路由组件
 */
export default class Admin extends React.Component{
  render(){
    const user = memoryUtils.user
    if(!user || !user._id){
      return <Redirect to={'/login'}></Redirect>
    }
    return (
        <div>Hello {user.username}</div>
    )
  }
}
```

## 利用localstorage将数据存储到浏览器上

```js
 //storageUtils.js 利用了store插件
import store from "store" //可以适配所有的浏览器，并且语法简洁
const USER_KEY = 'user_key'
const storageUtils = {
  /*
  保存User
   */
  saveUser(user){
    // localStorage.setItem(USER_KEY,JSON.stringify(user))
    store.set(USER_KEY,user)
  },
  /*
  获取User
   */
  getUser(){
    // return JSON.parse(localStorage.getItem(USER_KEY) || '{}')
    return store.get(USER_KEY) || {}
  },
  /*
  删除User
   */
  removeUser(){
    // localStorage.removeItem(USER_KEY)
    store.remove(USER_KEY)
  }
}
export default storageUtils
/*
在登录的时候将数据存储在内存中和localstorage中，然后进行跳转，并判断如果localstorage中有值，则重定向到admin中
在index.js中获取localstorage的数据然后存到内存中
*/
```

## 菜单组件和配置admin二级路由

![image-20210801002728574](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210801002728574.png)

```js
import React from "react"
import memoryUtils from "../../utils/memoryUtils";
import LeftNav from "../../components/left-nav/left-nav";
import Header from "../../components/header/header";
import {Redirect,Switch,Route} from "react-router-dom"
import Home from "../home/home";
import Product from "../product/product";
import Role from "../role/role";
import User from "../user/user";
import Pie from "../charts/pie";
import Bar from "../charts/bar";
import Line from "../charts/line";
import Category from "../category/category";
import { Layout } from 'antd';
const { Footer, Sider, Content } = Layout;
/*
后台管理的路由组件
 */


export default class Admin extends React.Component{
  render(){
    const user = memoryUtils.user
    if(!user || !user._id){
      return <Redirect to={'/login'}></Redirect>
    }
    return (
        <Layout style={{height:"100%"}}>
          <Sider>
            <LeftNav></LeftNav>
          </Sider>
          <Layout>
            <Header>Header</Header>
            <Content style={{backgroundColor:"#fff"}}>
              <Switch>
                <Route path='/home' component={Home}></Route>
                <Route path='/category' component={Category}></Route>
                <Route path='/product' component={Product}></Route>
                <Route path='/role' component={Role}></Route>
                <Route path='/user' component={User}></Route>
                <Route path='/charts/bar' component={Bar}></Route>
                <Route path='/charts/line' component={Line}></Route>
                <Route path='/charts/pie' component={Pie}></Route>
                {/*设置默认的路由跳转，即一开始就跳转到Home*/}
                <Redirect to={'/home'}></Redirect>
              </Switch>
            </Content>
            <Footer style={{textAlign:'center',color:'#ccc    '}}>推荐使用谷歌浏览器，可以获得更佳页面操作体验</Footer>
          </Layout>
        </Layout>
    )
  }
}
```

## 动态生成菜单（权限不同）

![image-20210801011755436](C:\Users\86157\AppData\Roaming\Typora\typora-user-images\image-20210801011755436.png)

```js
import {
  AppstoreOutlined,
  MenuUnfoldOutlined,
  MenuFoldOutlined,
  PieChartOutlined,
  ContainerOutlined,
  UserOutlined,
  SafetyCertificateOutlined,
  AreaChartOutlined,
  BarChartOutlined,
  LineChartOutlined,
  HomeOutlined
} from '@ant-design/icons';
const menuList = [
  {
    title: '首页', // 菜单标题名称
    key: '/home', // 对应的path
    icon: <HomeOutlined />, // 图标名称
    isPublic: true, // 公开的
  },
  {
    title: '商品',
    key: '/products',
    icon: <AppstoreOutlined />,
    children: [ // 子菜单列表
      {
        title: '品类管理',
        key: '/category',
        icon: <MenuUnfoldOutlined />
      },
      {
        title: '商品管理',
        key: '/product',
        icon: <MenuFoldOutlined />
      },
    ]
  },

  {
    title: '用户管理',
    key: '/user',
    icon: <UserOutlined />
  },
  {
    title: '角色管理',
    key: '/role',
    icon: <SafetyCertificateOutlined />
  },

  {
    title: '图形图表',
    key: '/charts',
    icon: <AreaChartOutlined />,
    children: [
      {
        title: '柱形图',
        key: '/charts/bar',
        icon: <BarChartOutlined />
      },
      {
        title: '折线图',
        key: '/charts/line',
        icon: <LineChartOutlined />
      },
      {
        title: '饼图',
        key: '/charts/pie',
        icon: <PieChartOutlined />
      },
    ]
  },
  {
    title: '订单管理',
    key: '/order',
    icon: <ContainerOutlined />,
  },
]

//默认暴露的可以起任意的名字
export default menuList


//侧边栏
import React from 'react'
import {Link} from "react-router-dom";
import "./left-nav.less"
import logo from "../../assets/images/bg1.jpg"
import menuList from "../../config/menuConfig";
import { Menu, Button } from 'antd';

const { SubMenu } = Menu;
export default class LeftNav extends React.Component {
  state = {
    collapsed: false,
  };
  getMenuNodes = (menuList) => {
    //根据权限返回的菜单可能不一致
    return menuList.map(item => {
      if(!item.children){
        return(
            <Menu.Item key={item.key} icon={item.icon}>
              <Link to={item.key}>
                {item.title}
              </Link>
            </Menu.Item>
        )
      }else {
        return (
            <SubMenu key={item.key} icon={item.icon} title={item.title}>
              {this.getMenuNodes(item.children)}
            </SubMenu>
        )
      }
    })
  }

  toggleCollapsed = () => {
    this.setState({
      collapsed: !this.state.collapsed,
    });
  };
  render() {
    return (
        <div className='left-nav'>
          <Link to='/'>
            <header className='left-nav-header'>
              <img src={logo} alt=""/>
              <h1>阳洪后台</h1>
            </header>
          </Link>
          <Menu
              defaultSelectedKeys={['1']}
              defaultOpenKeys={['sub1']}
              mode="inline"
              theme="dark"
              inlineCollapsed={this.state.collapsed}
          >
            {
              this.getMenuNodes(menuList)
            }
          </Menu>
        </div>
    )
  }
}
```

## 用reduce+递归实现动态菜单

```js
getMenuNodes = (menuList) => {
    return menuList.reduce((pre,item) => {
      if(!item.children){
        pre.push((
            <Menu.Item key={item.key} icon={item.icon}>
              <Link to={item.key}>
                {item.title}
              </Link>
            </Menu.Item>
        ))
      }else {
        pre.push((
            <SubMenu key={item.key} icon={item.icon} title={item.title}>
              {this.getMenuNodes(item.children)}
            </SubMenu>
        ))
      }
      return pre
    },[])
```

## 利用withRouter对选中菜单高亮进行优化

```js
import React from 'react'
import {Link,withRouter} from "react-router-dom";
import "./left-nav.less"
import logo from "../../assets/images/bg1.jpg"
import menuList from "../../config/menuConfig";
import { Menu, Button } from 'antd';

const { SubMenu } = Menu;
class LeftNav extends React.Component {
  constructor() {
    super();
    this.state = {
      collapsed: false,
    };

  }
  getMenuNodes_map = (menuList) => {
    //根据权限返回的菜单可能不一致
    return menuList.map(item => {
      if(!item.children){
        return(
            <Menu.Item key={item.key} icon={item.icon}>
              <Link to={item.key}>
                {item.title}
              </Link>
            </Menu.Item>
        )
      }else {
        return (
            <SubMenu key={item.key} icon={item.icon} title={item.title}>
              {this.getMenuNodes_map(item.children)}
            </SubMenu>
        )
      }
    })
  }

  getMenuNodes = (menuList) => {
    return menuList.reduce((pre,item) => {
      if(!item.children){
        pre.push((
            <Menu.Item key={item.key} icon={item.icon}>
              <Link to={item.key}>
                {item.title}
              </Link>
            </Menu.Item>
        ))
      }else {
        pre.push((
            <SubMenu key={item.key} icon={item.icon} title={item.title}>
              {this.getMenuNodes(item.children)}
            </SubMenu>
        ))
      }
      return pre
    },[])
  }
  toggleCollapsed = () => {
    this.setState({
      collapsed: !this.state.collapsed,
    });
  };
  render() {
    const path = this.props.location.pathname
    return (
        <div className='left-nav'>
          <Link to='/'>
            <header className='left-nav-header'>
              <img src={logo} alt=""/>
              <h1>阳洪后台</h1>
            </header>
          </Link>
          <Menu
              selectedKeys={path}
              mode="inline"
              theme="dark"
              inlineCollapsed={this.state.collapsed}
          >
            {
              this.getMenuNodes(menuList)
            }
          </Menu>
        </div>
    )
  }
}
/*
withRouter高阶组件：
包装非路由组件，返回一个新的组件
新的组件向非路由组件传递三个属性：history/location/match
 */

export default withRouter(LeftNav)

```

## dangerouslySetInnerHTML

**dangerouslySetInnerHTML**

**`dangerouslySetInnerHTML` 是 React 为浏览器 DOM 提供 `innerHTML` 的替换方案。通常来讲，使用代码直接设置 HTML 存在风险，因为很容易无意中使用户暴露于[跨站脚本（XSS）](https://en.wikipedia.org/wiki/Cross-site_scripting)的攻击。因此，你可以直接在 React 中设置 HTML，但当你想设置 `dangerouslySetInnerHTML` 时，需要向其传递包含 key 为 `__html` 的对象，以此来警示你。例如：**

```js
function createMarkup() {
  return {__html: 'First &middot; Second'};
}

function MyComponent() {
  return <div dangerouslySetInnerHTML={createMarkup()} />;
}
```

## antd4的表格的一些特殊用法

```js
/*
比如table中的column中有个很有用的render属性
form中有一个getFieldValue和resetFields
*/
```

## 一次性发送多个请求高效

```js
//一次性发送多个请求，只有都成功了，才正常处理
const results = await Promise.all([req1,req2])
const cName1 = results[0]
const cName2 = results[1]
```

## 富文本编辑组件

```js
react-draft-wysiwyg
```

