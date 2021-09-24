https://juejin.cn/post/6956524370336415780

#### 安装react及其声明文件
```
npm i react react-dom

npm i -D @types/react @types/react-dom
```
```
// tsconfig.json
{
  "compilerOptions": {
    "jsx": "react"
  }
}
```
```
src/index.ts 修改文件拓展名 src/index.tsx
```


### 使用脚手架

```
npx create-react-app ts-react-app --template typescript

```

#### 如果项目需要单独配置或者覆盖原始 webpack 配置，该怎么办？
#### 方法一：
使用 npm run eject 命令将 webpack.config.js 暴露出来。然后在该配置文件中进行修改。但这是极端且不推荐的，原因有二：

该命令是不可逆的。一旦执行了此命令 webpack.config.js 文件就永久的暴露出来。
如果只是修改一个很小的配置项。是不是有点大动干戈了？


#### 方法二（推荐）：
利用 customize-cra 和 react-app-rewired 通过 config-overrides.js 文件覆盖原配置项。



### 组件

#### 函数式组件
在 TypeScript 中，函数组件需要为 props 定义类型。

##### 普通函数组件
```
import React from "react";
import { Button } from "antd";

interface Greeting {
  name: string;
  firstName: string;
  lastName: string;
}

const Hello = (props: Greeting) => <Button>hello {props.name}</Button>;

Hello.defaultProps = {
  firstName: "",
  lastName: "",
};

export default Hello;
```

##### React.FC
在 react 的声明文件中，对函数组件单独定义了一个类型 -- React.FC:
```
type React.FC<P = {}> = React.FunctionComponent<P>

```

现在使用 React.FC 重新定义一下 <Hello />：
```
const Hello: React.FC<Greeting> = ({ name, firstName, lastName }) => (
  <Button>hello {name}</Button>
);
```

##### 属性必须是可选的
在使用 React.FC后定义 defaultProps 时，默认属性必须是可选的（这和普通函数组件不同）：
```
interface Greeting {
  name: string;
  firstName?: string;
  lastName?: string;
}
```

#### 类组件
##### Component

类组件需要继承 Components 组件，在 react 的声明文件中，Component 被定义为泛型类
```
// P: 属性的类型，默认{}
// S: 状态的类型，默认{}
// SS: snapshot
(alias) class Component<P = {}, S = {}, SS = any>
```
##### 实现
```
// src/componets/demo/HellpClass.tsx
import React, { Component } from "react";
import { Button } from "antd";

interface Greeting {
  name: string;
  firstName: string;
  lastName: string;
}

interface State {
  count: number;
}

class HelloClass extends Component<Greeting, State> {
  // 初始化 state
  state: State = { count: 0 };
  // 默认属性值
  static defaultProps = {
    firstName: "",
    lastName: "",
  };
  render() {
    return <Button>hello {this.props.name}</Button>;
  }
}

export default HelloClass;
```

##### React.ComponentType
指定被包装组件的类型为 React.ComponentType（一种 React 预定义类型），既可以是类组件，也可以是函数组件：
```
type React.ComponentType<P = {}> = React.ComponentClass<P, any> | React.FunctionComponent
```

##### 实现
添加 `<HelloHOC />` 组件：
```
import React, { Component } from "react";
import HelloClass from "./HelloClass";

interface Loading {
  loading: boolean;
}

/*
 ** WrapperComponetn: 需要被包装的组件
 */
function HelloHOC<P>(WrapperComponetn: React.ComponentType<P>) {
  // 定义 props 为 P 和 Loading 的交叉类型
  return class extends Component<P & Loading> {
    render() {
      // 解构 props，拆分出 loading
      const { loading, ...props } = this.props;
      // {...props}：属性透传
      return loading ? (
        <div>Loading...</div>
      ) : (
        <WrapperComponetn {...(props as P)} />
      );
    }
  };
}

// 导出经过高阶组件包装后的组件
export default HelloHOC(HelloClass);
```
有个报错
我们在 index.tsx 中引入这个组件，这时会有一个报错：
```
import React from "react";
import ReactDOM from "react-dom";
import HelloHOC from "./components/demo/HelloHOC";

ReactDOM.render(
  <HelloHOC name="typescript" loading={true} />,
  document.querySelectorAll(".app")[0]
);

// ERROR! 因为 HelloClass 的静态属性 defaultProps 传不出来。
```

解决方案：将 defaultProps 设置为可选属性。
```
interface Greeting {
  name: string;
  firstName?: string;
  lastName?: string;
}
```
