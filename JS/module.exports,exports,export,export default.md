module.exports与exports属于common JS规范，
export与 export default ES6模块规范

使用方面
module.exports与exports
CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的module.exports属性。

```
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;

```
上面代码通过module.exports输出变量x和函数addX。

require方法用于加载模块。

```
var example = require('./example.js');

console.log(example.x); // 5
console.log(example.addX(1)); // 6

```

##### exports 是module.exports的引用

var exports = module.exports;

总结来说就三句话;

module.exports初始值为一个空对象

exports是指向module.exports的引用

require()返回的是module.exports


#### export与 export default的使用
```
// profile.js
var firstName = ‘Michael’;
var lastName = ‘Jackson’;
var year = 1958;

export {firstName, lastName, year};
```
需要特别注意的是，export命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

```
// 写法一 export var m = 1;

// 写法二 var m = 1; export {m};

// 写法三 var n = 1; export {n as m};

```

export default 命令
使用export default命令，为模块指定默认输出。

export default的使用
1.export default 用于规定模块的默认对外接口
2.很显然默认对外接口只能有一个，所以 export default 在同一个模块中只能出现一次
3.export default只能直接输出，不能先定义再输出。

4.其在 import 方式上也和 export 存在一定区别

#### export的输出与import输入

```
export function output() {
    // ...
}
import {output} from './example'

```
#### export default的输出与import输入
```
export default function output() {
    // ...
}
import output from './example'
```

总结 从以上两种 import 方式即可看出，export default 的 import 方式不需要使用大括号包裹。因为对于 export default 其输出的本来就只有一个接口，提供的是模块的默认接口，自然不需要使用大括号包裹