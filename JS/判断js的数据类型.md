### 1.typeof  

适合函数对象和基本类型

返回值为字符串

number、string、boolean、null、undefined、object［对象］、Symbol

```
  console.log("测试number:"+typeof 1); // number
  console.log("测试string:"+typeof "str"); // string
  console.log("测试false:"+typeof false); // boolean
  console.log("测试null:"+typeof null);// object
  console.log("测试undefined:"+typeof undefined);// undefined
  console.log("测试Object:"+typeof new Object());// object
  console.log("测试Object:"+typeof new Array()); // object
  console.log("看看typeof NaN是啥:"+typeof NaN); // number
  console.log("我想看看数组［1，2，3]类型:"+typeof [1,2,3]); //object
  console.log("看看function是啥:"+typeof function(){}); // function
```


### instanceof


instanceof的原理是检查右边构造函数的prototype属性，是否在左边对象的原型链上。有一种特殊情况，就是左边对象的原型链上，只有null对象。这时，instanceof判断会失真。

也就是说指定对象是否是某个构造函数的实例，最后返回布尔值，这个对整个原型链上的对象都是有效的，由于instanceof对整个原型链上的对象都有效，因此同一个实例对象，可能会对多个构造函数都返回true！

http://javascript.ruanyifeng.com/oop/prototype.html



### Object.prototype.toString 

<img src="https://img-blog.csdn.net/20160903185915333">


### constructor属性

所有实例对象都有constructor属性，constructor属性指向prototype对象所在的构造函数，就是说指向创建这个实例的构造函数

```
function A(){}
	let c = new A();
    c.constructor == A // true
	c.constructor == A.prototype.constructor // true
```

```
function A(){}
	A.prototype = new Array();
	A.prototype.constructor = A // 此处为重点哦 如果没有 那么下边的答案是反过来的

	let c = new A();
	console.log(c.constructor == Array) // false
	console.log(c.constructor == A) // true
```

###5、duck type（鸭子类型）
比如判断一个对象是否是数组，可以看这个对象是否拥有length()等方法，不禁想到类数组转数组的方法


