JS isPrototypeOf()方法：检测一个对象是否存在于另一个对象的原型链中


prototypeObject.isPrototypeOf(object);


```
function isInstanceOf(target1, target2) {
  let proto = Object.getPrototypeOf(target1)
  if(!proto){return false}
  if (proto === target2.prototype) { return true }
  //递归去原型链上找
  return isInstanceOf(proto, target2)
}

```


```
function isInstanceOf(target1, target2) {
  return target2.prototype.isPrototypeOf(target1)
}

```