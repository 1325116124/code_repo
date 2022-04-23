# Ts+React

## 初始化项目

```tsx
npx create-react-app react-ts --template=typescript
```

## Ts中组件的使用

```ts
//接口当作对象来使用，其中对于可选属性为对象的使用要用?.的方式进行调用，否则会报错
//componeny中的第一个any为props属性
import React,{Component} from "react";
interface IUser{
    age:number
}
interface IProps {
    name:string
    age?:number
    auth?:boolean
    user?:IUser
}

export default class Lee extends Component<IProps, any>{
    render() {
        return (
            <>
                <h1>Lee</h1>
                <h1>{this.props.name}</h1>
                <h1>{this.props.user?.age}</h1>
            </>
        )
    }
}

```

## state的使用

```ts
import React,{Component} from "react";
interface IState {
    counter:number
}

export default class Lee extends Component<any, IState>{
    constructor(props:any,context:any) {
        super(props,context);
        this.state = {
            counter:0
        }
    }
    render() {
        return (
            <>
                <h1>Lee</h1>
                <h1>{this.state.counter}</h1>
            </>
        )
    }
}

```

