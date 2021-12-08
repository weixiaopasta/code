### @import
Less中，可以通过 @import指令来导入外部文件。@import指令可以放在代码中的任何位置，导入文件时的处理方式取决于文件的扩展名：
    * 如果扩展名是 .css，文件内容将被原样输出。
    * 如果是任何其他扩展名，将作为LESS文件被导入。
    * 如果没有扩展名，将给他添加一个 .less 的扩展名，并作为LESS文件被导入。

```
@import (keyword) "filename"
```
    keyword一共有六种： referrence
```
选项	含义
reference	使用文件，但不会输出其内容（即，文件作为样式库使用）
inline	对文件的内容不作任何处理，直接输出
less	无论文件的扩展名是什么，都将作为LESS文件被输出
css	无论文件的扩展名是什么，都将作为CSS文件被输出
once	文件仅被导入一次 （这也是默认行为）
multiple	文件可以被导入多次
optional	当文件不存在时，继续编译（即，该文件是可选的）
```
    "filename文件中定义的所有mixin和class可以被使用，但是并不会把这些mixin/class编译到最终的css文件中

一个@import指令可以使用一个或多个配置项，当使用多个配置项时，各配置项之间用逗号隔开。如：

@import (optional, reference) "foo.less";

### extend 关键字

extend pseudo-class是一个less专有的pseudo-class，并且使用css pseudo-class一样的语法。

该extend pseduo-class将把:extend前面的selector放到被extended的selector list中去，从而继承了被extended css selctor(class)的所有css属性。

注意selector nested in .hyperlink并未被输出！！
```
.hyperlink{
    color: blue;
    &:hover {
        color: red;
    }
    h2 { color: white; }
}
.other-hyperlink:extend(.hyperlink){};
将会编译输出（注意selector nested in .hyperlink并未被输出！！）：
.hyperlink,
    .other-hyperlink {
        color: blue;
    }
.hyperlink:hover {
    color: red;
}
.hyperlink h2 { color: white; }

```

.other-hyperlink:hover:extend(.hyperlink){};也可以使用
如果要将.hperlink中的nested rule也继承过来，你可以通过增加一个all 关键字来强制compiler来包含所有nested selectors
```
.other-hyperlink:hover:extend(.hyperlink all){};
```
也可以通过 把all关键字替换为你要具体嵌套的标签 比如h2
```
.other-hyperlink:hover:extend(.hyperlink h2){};
```

### nested -- 嵌套的
.other-hyperlink:hover:extend(.hyperlink all){};


#### 不带()的mixin实际上也就是一般的class，他们自然会在编译后的文件中输出出来，如果希望这个mixin(class)只作为在其他地方调用或者extend，而不会编译自己本身形成class的，那么最简单的方法是在class选择器后添加()即可。extend往往用于扩展重用一个静态的class,mixin则相当于函数的pattern，用于不同地方被重复使用


extend 解决代码重复问题

```
.btn { background: blue; color: white; } 
.round-button { 
    &:extend(.btn); 
    border-radius: 4px; 
}
```

编译以后我们就将获得下面干净的输出：
```
.btn, .round-button { background: blue; color: white; }
.round-button { border-radius: 4px; }

```



### extend 弊端- 如下代码不能工作

```
.feed { padding: 20px; }
@media (max-width: 1024px) { .sig { &:extend(.feed); } }
```

### extend 与 mixin的区别
```
当你只是希望在多个class中共享一些静态的style，那么extend是一个非常好的选择，但是minins在你需要增加一组动态的style时变得非常有用。Mixin是更高一级的抽象，类似于编程中的函数。
```

```
.btn(@color) { 
    background: @color; 
    border: 1px solid darken(@color, 15); 
    border-radius: 4px; 
    color: #fff; 
    &:hover { 
        background: lighten(@color, 10); 
        border-color: darken(@color, 5);
    } 
}
```
使用上面的mixin，我们就能够方便地定义一套不同的button：

```
.btn-success { .btn(#18682f); } 
.btn-alert { .btn(#9b6910); } 
.btn-error { .btn(#9b261a); }
```