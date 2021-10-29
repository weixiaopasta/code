https://ishare.58corp.com/articleDetail?id=67251


一、从声明一个组件开始
函数式组件声明方式
方式一（推荐）: 将组件用React.FC< P > 这个官方的函数组件泛型接口来声明。
优势（1）props 自带 children 声明 （2）defaultProps、propsTypes、displayName 等的所有react组件静态属性能够获得 IDE 的友好提示。
```
  interface Props {
    text:string
    theme:Theme
  }
  const Button:React.FC<Props> = (props) => {
      const { text } = props;
      return <div><button>{text}</button></div>;
  };
  ```
方式二 : 自行处理 children 声明
```
interface Props {
    text:string
    theme:Theme
    children?: ReactNode
  }
  const Button = (props:Props) => {
      const { text, children } = props;
      return <div><button>{text}</button>{children}</div>;
  };
  ```
方式三 : 利用 PropsWithChildren< P > 处理 children。效果同方式二一样。
```
 interface Props {
    text:string
    theme:Theme
  }
  const Button = (props: React.PropsWithChildren<Props>) => {
      const { text, children } = props;
      return <div><button>{text}</button>{children}</div>;
  };
  ```
| | | | |
| ----------- | ----------- | ----------- |----------- |
| |React.FC（推荐）|	自行处理children属性|	PropsWithChildren
自动children声明	|是	|否|	是
react静态方法声明	|是|	否|	否



巧用映射类型提升 interface 编写效率

同态：只会作用于目标原有属性，不会创建新属性
```
interface Props {
    a: string,
    b: number
  }

  // 把所有的属性变成了只读
  type ReadonlyObj = Readonly<Props>

  // 把所有属性变成可选
  type PartiaObj = Partial<Props>

  // 把所有属性变成必选
  type RequiredProps = Required<Props>

  // 抽取子集
  type PickObj = Pick<Props, 'a' | 'b'>
  ```
非同态：用来创建新属性
```
  //创建同类型的不同属性
  type RecordObj = Record<'x'
   | 'y', string>
```

ts文档上对Record的介绍不多，但却经常用到，Record是一个很好用的工具类型。

https://zhuanlan.zhihu.com/p/356662885


Record`<K,T>`构造具有给定类型T的一组属性K的类型。在将一个类型的属性映射到另一个类型的属性时，Record非常方便。

他会将一个类型的所有属性值都映射到另一个类型上并创造一个新的类型.

```
interface EmployeeType {
    id: number
    fullname: string
    role: string
}
 
let employees: Record<number, EmployeeType> = {
    0: { id: 1, fullname: "John Doe", role: "Designer" },
    1: { id: 2, fullname: "Ibrahima Fall", role: "Developer" },
    2: { id: 3, fullname: "Sara Duckson", role: "Developer" },
}
 
```
Record的工作方式相对简单。在这里，它期望数字作为类型，属性值的类型是EmployeeType，因此具有id，fullName和role字段的对象。
```
//demo1
type petsGroup = 'dog' | 'cat' | 'fish';
interface IPetInfo {
    name:string,
    age:number,
}

type IPets = Record<petsGroup, IPetInfo>;

const animalsInfo:IPets = {
    dog:{
        name:'dogName',
        age:2
    },
    cat:{
        name:'catName',
        age:3
    },
    fish:{
        name:'fishName',
        age:5
    }
}
```
可以看到 IPets 类型是由 Record<petsGroup, IPetInfo>返回的。将petsGroup中的每个值(‘dog’ | ‘cat’ | ‘fish’)都转为 IPetInfo 类型。


当然也可以自己在第一个参数后追加额外的值，如下面：
```
//demo2
type petsGroup = 'dog' | 'cat' | 'fish';
interface IPetInfo {
    name:string,
    age:number,
}

type IPets = Record<petsGroup | 'otherAnamial', IPetInfo>;

const animalsInfo:IPets = {
    dog:{
        name:'dogName',
        age:2
    },
    cat:{
        name:'catName',
        age:3
    },
    fish:{
        name:'fishName',
        age:5
    },
    otherAnamial:{
        name:'otherAnamialName',
        age:10
    }
}

type IPets = Record<petsGroup | 'otherAnamial', IPetInfo>; 中除了petsGroup的值之外，还追加了 'otherAnamial’这个值。
```


#### 四、DOM 事件处理
用 React 同学应该都知道，它里边的 DOM 事件都是合成事件（经过了 react 框架跨浏览器的兼容处理），与原生事件对象并不一致。所以得用 React 官方的事件类型去修饰，才能获得事件对象属性方法的代码提示等高级功能。
比如：
```
  import React, { MouseEvent } from 'react';

  export default function App() {
      const handleTap = (e:MouseEvent) => {
          e.stopPropagation();
      };

      return <button onClick={handleTap}>click</button>;
  }
    常用 Event 事件对象类型：

    ClipboardEvent<T = Element> 剪贴板事件对象

    DragEvent<T = Element> 拖拽事件对象

    ChangeEvent<T = Element> Change 事件对象

    KeyboardEvent<T = Element> 键盘事件对象

    MouseEvent<T = Element> 鼠标事件对象

    TouchEvent<T = Element> 触摸事件对象

    WheelEvent<T = Element> 滚轮事件对象

    AnimationEvent<T = Element> 动画事件对象

    TransitionEvent<T = Element> 过渡事件对象
```

####  五、与第三方库和谐相处（组件库或者 JS 库）
技巧: 巧借第三方本身所提供的TS 类型，来扩展自己的功能。
示例需求：扩展 antd 的按钮组件，支持鼠标移入按钮上方出现详细的提示文案的feature。
```
    import React, { MouseEvent } from 'react';
    import { Tooltip, Button, ButtonProps } from 'antd';

    //利用用Antd自带的ButtonProps配合React的{...props}扩展属性，在不丧失Button的可配置性的前提下实现了鼠标提示功能。
    interface Props extends ButtonProps{
      tipText?:string
    }
    const TipButton:React.FC<Props> = (props) => {

        return <Tooltip title={props.tipText}>
            <Button {...props}>click</Button>;
        </Tooltip>;
    };

    export default TipButton;
```


#### 尝试解决一些常见 TS 问题

```
一. 找不到模块“xxx”或其相应的类型声明。
常见场景：当你在安装一些 npm 模块时，比如:


//shell
npm install lodash

//index.tsx
import _ from 'lodash';


**原因**：主包里**缺少模块的声明文件**。 package.json=>types 字段为“”,导入声明自动降级为 any 类型，导致不能获得友好的代码提示及类型校验。
**解决方案**：首先，可以直接尝试安装对应的声明文件，主流的第三方库，比如 jquery、lodash、redux 等都有官方发布专门的类型声明包。（也可以[点这查询](https://www.typescriptlang.org/dt/search?search=lodash)有没有对应的类型声明包）


// shell
npm install @types/lodash -D


如果实在没有，那就直接分析源码及对应 api 尝试自己动手撸一个吧，这是你对开源社区作出贡献的好时机。

```
```
二、类型“Window & typeof globalThis”上不存在属性“xxx”

**常见场景**：当你想声明一个全局变量时，


window.globalConfig = {};//ts error


原因：window 或者 globalThis 这些全局对象上并没有相应的属性声明。
解决方案：新建 global.d.ts 全局类型声明文件，用来扩展全局的变量声明


// global.d.ts 放在当前项目的任意文件夹都可以（一般全局的放在根路径下比较好找），而且global的前缀可以随意命名。
  declare interface Window {
    globalConfig:{
      color?:string
    }
  }

// index.tsx
  window.globalConfig = {
      color: 'red',
  };


引入的全局库（非 umd 库），比如通过< script src='https://ukg-jquery.min.js'>引入，直接使用全局变量，ts会提示 $ 变量不存在，下边这样声明就可以了。


  // jq.d.ts
  declare namespace ${
       function toast(text:string):void
  }

  // index.tsx
   $.toast('ts不报错啦');


```

```
三、类型“xxx”上不存在属性“b”。
常见场景：给对象添加属性或者，读取调用对象属性或方法


obj.a = 'xxx';//[ts] 类型“xxx”上不存在属性“b”。


解决方案：
    方法 1：补全属性类型（理想情况）


    interface Obj{
        a?:string
    }


    方法 2：类型断言 + 交叉类型（&）。(对象是由很多处理返回得来的，不好直接修改类型)


    type Obj = {a:string} & typeof obj
    (obj as Obj).a = 'xxx';//ts校验通过

    (obj as any).a = 'xxx';//不推荐使用any


```

```
四、在类型 “xxx” 上找不到具有类型为 “string” 的参数的索引签名
**常见场景**：遍历对象场景时
**原因**：遍历过程中的 key 属性只约束为 string 类型，太宽泛了，应该只限定为对象存在的属性字符串，访问未知的属性会报错（上边 3 有讲到）。


  interface Item{
    a:string
  }
  const item:Item = {
      a: 1,
    };

  Object.keys(item).map((key) => item[key])


**解决方案：** 利用 keyof（索引类型查询操作符）限定范围

    interface Row{
           a:string
    }
    const obj:Row = {
           a: 'xxx',
    }
    Object.keys(obj).map((k:keyof Row) => obj[k]);     
```
```
五、tsx 文件的类型断言写法注意
类型断言 <> 语法 tsx 不兼容，与元素或者组件的使用写法有冲突。所以应始终用 as 做断言。

```

六、as const 的用处


**示例**：字符串数组转字符串联合类型。
**难点**：ts的类型检查是发生在编译阶段，想让运行时可以随时改变的值（即使是const定义的，也只是保持内存地址不变）初始化编译时的类型，常理来说除非时光倒流，但是as const可以做到。
**原理**：断言为一个永久的只读常量，ts就可以放心的将值转为类型。
```
const arr = ['name', 'age', 'location', 'email'];
// 通过类型操作之后得到下面这种类型
type arrType = 'name' | 'age' | 'location' | 'email'

```
```
  const arr = ['name', 'age', 'location', 'email'] as const;

  type arrType = typeof arr[number]

  const str:arrType = 'age';

  console.log(str);
```

#### Q&A
```
一、interface(接口) 与 type（类型别名）怎么选择？
相同点：都可以用来描述对象或者函数的形状，甚至都可以实现字段扩展，interface 可以 extends，type 可以用 &（交叉类型）做混入。
不同点：
1.术业有专攻。
interface 的优势在擅长描述对象类型的结构。可以实现与class无缝结合，比如可以实现 interface extends class。
type 主要在定义简单类型、联合类型、交叉类型、元组时使用。
2.声明合并。 interface 支持，type 不支持
3.编译器提示不同。

type Alias = { num: number } 
interface Interface { num: number; } 
declare function aliased(arg: Alias): Alias;
declare function interfaced(arg: Interface): Interface; 
鼠标悬停在 interfaced 上，显示它返回的是 Interface 名字，但悬停在 aliased 上时，显示的却是对象字面量类型。
总结：如果你无法通过接口来描述一个类型并且需要使用联合类型或元组类型，这时通常会使用类型别名，所以原则上是优先使用接口。

```
```
二、extends (继承) 与 &（交叉类型）怎么区分使用？
相同点：都可以用来扩展字段。
不同点：
用途：extends 适用于典型的面向对象场景，有清晰的父子继承关系
& 适用于做 mixin（混入），层级是扁平的。
处理共有属性的表现：extends 会报错，& 则是会转为 never 类型

```

```
三、unkown 与 any 有什么区别？
相同点：都是 TS 里的Top Types(顶级类型) ，任何类型的值都可以赋给这样的类型。
不同点：
语意方面：unkown 代表暂时类型未知，any 代表着可以接受任意的赋值与操作。
安全性：如果某个值的类型为 any，我们可以对其执行任意操作，包括随意的运算或者方法调用。unkown 则不可以，只支持任意类型的赋值，不支持任意的操作，操作时需要缩小范围。相对而言，unkown 更安全一些。
总结：unkown 代表着暂时未知的类型，以后一旦类型确定了，还有机会弥补。any 则就是基本上等同于放弃了治疗，让 ts 的类型检查彻底失去了作用。
```

```
四、理性看待 any类型
观点：存在不一定合理，但是一定有其存在的意义。所以限制 any 的使用范围可能是比完全禁止使用它更有意义。
现状：市面上还是存在非常多没有类型检查的 javascript 项目，当你的项目与一些不是 ts 写的项目发生合作时，any 是降级兼容所必须的一个类型。
使用 any 的推荐场景：

当你引入一个非ts的类库时，可以将其核心的 api 对象（比如 jquery 的$）暂时设为 any，以逃避ts的类型检查。
当你只是为了说服 TypeScript 我的代码运行没有问题，而需要写一个长长的很耗时的类型时，ps（TS只是一个帮助咱们项目进行类型检查的工具，记住，有时没必要和工具死磕到底，只在你觉得有收益的地方用好即可）。
在你改一个不熟悉的ts项目，但工时紧张，认为any非用不可时。可以暂时尝试将它设为unkown，后续再优化，而非直接设为any。
总结： any类型是把双刃剑，它的存在，很大程度体现了 ts 的灵活性，但需要谨记不用过度滥用。
```