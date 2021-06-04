https://blog.csdn.net/gooodi/article/details/87110820

```
div {
  width: 100px;
  margin-left: auto;
}
```

上面的代码可以使得div靠近浏览器的右侧显示是为什么？

有点弱智的问题,竟然现在才知道...记录一下 

    The following constraints must hold among the used values of the other properties:

    'margin-left' + 'border-left-width' + 'padding-left' + 'width' + 'padding-right' +'border-right-width' + 'margin-right' = width of containing block

    你的属于其中一种情况：
    If there is exactly one value specified as 'auto', its used value follows from the equality.

也就是在上述等式中，只有你设置的margin-left: auto，那么margin-left将会被设置为满足上述等式，而等式的右边是容器盒子宽度，等式中的其它值(除过width)都为0，因此margin-left = width of containing block - width(div)

再来看一下我们经常使用的 margin: auto 水平居中的原理。

If both 'margin-left' and 'margin-right' are 'auto', their used values are equal. This horizontally centers the element with respect to the edges of the containing block.