#### for ... in

语句以任意顺序迭代对象的可枚举属性

for...in语句以任意顺序遍历一个对象的除Symbol以外的可枚举属性。

提示：for...in不应该用于迭代一个关注索引顺序的 Array。

没有return  会忽略break 和 continue

### for ... of 
语句遍历可迭代对象定义要迭代的数据。

for...of语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句

对于for...of的循环，可以由break, throw  continue    或return终止。在这些情况下，迭代器关闭。