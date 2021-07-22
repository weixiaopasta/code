（1）JavaScript 表达式也可以在.less 文件中使用. 可以通过反引号的方式使用:
```
@var: `"hello".toUpperCase() + '!'`;  // @var :"HELLO!"
```
（2）也可以同时使用字符串插值和避免编译
```
@str: "hello";
@var: ~`"@{str}".toUpperCase() + '!'`; //@var: HELLO!;
```
（3）可以访问JavaScript的环境
```
@height: `document.body.clientHeight`;
```
（4）将一个JavaScript字符串解析成16进制的颜色值，可以使用 color 函数

```
@color: color(`window.colors.baseColor`);
@darkcolor: darken(@color, 10%);
```



