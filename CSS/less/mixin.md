https://www.jianshu.com/p/8c510a3bc8db

### less playground

https://lesstester.com/


#### 调用mixin

当调用 mixin 时，括号是可选的

#### 不把 Mixin 输出到编译后的 CSS 中

* 如果你想用 mixin 但又不想把它输出到编译结果中，在定义 mixin 的时候，在 mixin 后面加上括号
```
.my-mixin {
  color: black;
}
// 注意 my-other-mixin 后面加了括号
.my-other-mixin() {
  background: white;
}

.class {
  .my-mixin;
  .my-other-mixin;
}
```
编译输出结果:
```
.my-mixin {
  color: black;
}
//可以看到这一行并没有输出上面定义的 my-other-mixin. 
但是下面的 .class 中却含有 background: white;
.class {
  color: black;
  background: white;
}
```
#### 命名空间
如果要在更复杂的选择器中混入属性，可以叠加多个id或类。
```
#outer {
  .inner {
    color: red;
  }
}

.c {
  #outer > .inner;
}
```

### 符号 `>` 和空格都可以省略

```
// 下面的写法效果是一样的
#outer > .inner;
#outer > .inner();
#outer .inner;
#outer .inner();
#outer.inner;
#outer.inner();
```


#### 携带多个参数的 mixin
多个参数之间用 逗号 或 分号 隔开，推荐用 分号

#### 命名参数

```
.mixin(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}


.class1 {
  .mixin(@margin: 20px; @color: #33acfe); 
  // 调用 mixin 时通过指定形参名称传递实参值，并没有按照定义  
  // mixin 时的顺序 先 color  其次 margin 最后 padding 的
  // 顺序来传值
}
.class2 {
  .mixin(#efca44; @padding: 40px); 
  // 按原有的形参顺序传值时，可以不指定形参名
}
```


#### @arguments变量
@arguments 在 mixins 中有独特的含义，它包含了传入的所有参数，当该 mixin 被调用时，如果不想单独的写每个参数时会比较有用。

```
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000) {
  -webkit-box-shadow: @arguments;
     -moz-box-shadow: @arguments;
          box-shadow: @arguments;
}
.big-block {
  .box-shadow(2px; 5px);
}

```

####  高级参数 和 @rest 变量
当一个 mixin 携带有可变数量的参数时，可以使用 ...
在变量名后面使用该操作符将会把这些参数赋给变量

```
.mixin(...) { // 匹配第 0 - N 个 arguments
.mixin() {    // 匹配第 0 个 arguments
.mixin(@a: 1) { // 匹配第 0 -1 个 arguments
.mixin(@a: 1; ...) { // 匹配 0 - N 个 arguments
.mixin(@a; ...) { // 匹配 1 - N 个 arguments
此外：


.mixin(@a; @rest...) {
  // @rest 绑定到 @a 之后的所有参数
  // @arguments 绑定到所有参数
}

```


#### 模式匹配

有时候我们想根据传入的参数改变一个 mixin 的行为。让我们从一个基础的例子开始:

```
.mixin(@switch; @color) {...}

.class {
  .mixin(@switch; #888);
}

```

我们想根据 @switch 的不同让 mixin 表现出不同的结果。我们可以如下定义 .mixin:

* 符号`@_`代表任意值 

```
.mixin(dark; @color) {
  color: darken(@color, 10%);
}
.mixin(light; @color) {
  color: lighten(@color, 10%);
}
.mixin(@_; @color) {
  display: block;
}
```
使用：

```
@switch: light;

.class {
  .mixin(@switch; #888);
}
```

编译得到的CSS结果为:

```
.class {
  color: #a2a2a2;
  display: block;
}
```

#### 我们逐步分析是怎么编译出来的：

第一个 mixin 不匹配，因为第一个参数为 dark
第二个 mixin 匹配，因为它的第一个参数为 light
第三个 mixin 匹配，因为它的第一个参数接受任意值
只有匹配的 mixin 才会被采用。变量匹配并绑定到任意值。变量以外的任何内容都只和等于自身的值匹配。


#### 我们也可以匹配参数个数，看下面的例子:
```
.mixin(@a) {
  color: @a;
}
.mixin(@a; @b) {
  color: fade(@a; @b);
}
```

如果我们调用 .mixin 时只传入了一个参数, 我们会得到第一个 .mixin 的编译结果.如果传入了两个参数，就会得到第二个 .mixin 的编译结果


#### 作为函数的 mixins

```
.mixin() {
  @width: 100%;
  @height: 200px;
}

.caller {
  .mixin();
  width: @width;
  height: @height;
}
```
编译结果为:

```
.caller {
  width: 100%;
  height: 200px;
}
```

因此，mixin中定义的变量可以作为其返回值。这允许我们创建一个可以像函数一样使用的mixin
例如:

```
.average(@x, @y) {
  @average: ((@x + @y) / 2);
}
div {
  .average(16px, 50px); // 调用 mixin
  padding: @average;
}
```

编译结果为:

```
div {
  padding: 33px;
}
```


