Console 对象提供对浏览器控制台的接入（如：Firefox 的 Web Console）。不同浏览器上它的工作方式是不一样的，但这里会介绍一些大都会提供的接口特性。

分类输出
不同类别信息的输出
```
console.log('文字信息');
console.info('提示信息');
console.warn('警告信息');
console.error('错误信息');
```

#### 分组输出
使用Console.group()和Console.groupEnd()包裹分组内容。

还可以使用Console.groupCollapsed()来代替Console.group()生成折叠的分组。
```
console.group('第一个组');
    console.log("1-1");
    console.log("1-2");
    console.log("1-3");
console.groupEnd();

console.group('第二个组');
    console.log("2-1");
    console.log("2-2");
    console.log("2-3");
console.groupEnd();
```

Console.group()还可以嵌套使用  可嵌套使用


#### 表格输出 
console.table()


#### 查看表格对象
console.dir() 


#### 条件输出

console.assert()

* 当第一个参数或返回值为真时，不输出内容
* 当第一个参数或返回值为假时，输出后面的内容并抛出异常

```
console.assert(true, "你永远看不见我");
console.assert((function() { return true;})(), "你永远看不见我");

console.assert(false, "你看得见我");
console.assert((function() { return false;})(), "你看得见我");
```

#### 追踪调用堆栈
使用Console.trace()来追踪函数被调用的过程，在复杂项目时调用过程非常多，用这个命令来帮你缕清。

```
function add(a, b) {
    console.trace("Add function");
    return a + b;
}

function add3(a, b) {
    return add2(a, b);
}

function add2(a, b) {
    return add1(a, b);
}

function add1(a, b) {
    return add(a, b);
}

var x = add3(1, 1);
```


### 格式化输出
占位符	含义
%s	字符串输出
%d or %i	整数输出
%f	浮点数输出
%o	打印javascript对象，可以是整数、字符串以及JSON数据

#### 样例：
```
var arr = ["小明", "小红"];

console.log("欢迎%s和%s两位新同学",arr[0],arr[1]);

console.log("圆周率整数部分：%d，带上小数是：%f",3.1415,3.1415);
```

#### 自定义样式
使用%c为打印内容定义样式,再输出信息前加上%c，后面写上标准的css样式，就可以为输出的信息添加样式了
```
console.log("%cMy stylish message", "color: red; font-style: italic");

console.log('%c this is a message','color:#0f0;')
console.log('%c this %c is a %c message','color:#f00;','font-size:20px;','color:blue;background:yellow;')
```

#### 在node服务中我们可以使用colors模块来控制样式

然而在shell中以上的方法就不行了

安装
```
npm install colors
```
使用方法非常简便，模块通过扩展 String.prototype 添加样式，而且支持链式调用。

```
var colors = require('colors');
 
console.log('hello'.green); // outputs green text 
console.log('i like cake and pies'.underline.red) // outputs red underlined text 
console.log('inverse the color'.inverse); // inverses the color 
console.log('OMG Rainbows!'.rainbow); // rainbow (彩虹)
console.log('Run the trap'.trap); // Drops the bass 
```