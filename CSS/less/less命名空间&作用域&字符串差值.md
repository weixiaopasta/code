有时候，我们可能更好地组织CSS或是单纯的为了更好地封装，我们会将会一些变量或是混合模块进行打包操作，为了后续进行复用

```
#bundle {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover { background-color: white }
  }
  .tab { ... }
  .citation { ... }
}
```
当我们想要在某一个地方引入button的样式的时候：
```
#header a {
  color: orange;
  #bundle > .button;
}
```

### 作用域
less的作用域和其他编程语言十分的相似，首先在本地的变量和混合模块进行查找，如果没有找到的话，就会去父及作用域查找，直到找到为止。

```
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

#footer {
  color: @var; // red  
}
```

### 字符串差值

变量可以用类似ruby和php的方式嵌入到字符串中，像@{name}这样的结构

```
@base-url: "http://assets.fnord.com";
background-image: url("@{base-url}/images/bg.png");
```