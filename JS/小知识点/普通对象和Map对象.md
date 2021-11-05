
####   普通对象

```
const names = {
    1: 'One',
    2: 'Two',
};

Object.keys(names) // => ['1', '2']

```
上面代码中的key是Number类型, 但是却被 隐式 转换为了String类型


#### map 
key 可以是任意类型

#### number
```
const numbersMap = new Map();

numbersMap.set(1, 'one');
numbersMap.set(2, 'two');

[...numbersMap.keys()]; // => [1, 2]
```
#### Boolean
当然也可以使用booleans作为map的key
```
const booleansMap = new Map();

booleansMap.set(true, "Yep");
booleansMap.set(false, "Nope");

[...booleansMap.keys()]; // => [true, false]
```

#### object

当想存储一些object-related数据的时候, 使用plain objects是不可能的, 这时候我们不得不使用键值对数组(an array of object-value tuples):
```
const foo = { name: 'foo' };
const bar = { name: 'bar' };

const kindOfMap = [
  [foo, 'Foo related data'],
  [bar, 'Bar related data'],
];
```
复制代码很明显, 当我们如果要访问foo或者bar的时候需要遍历对象, 这样性能不好, 复杂度O(n) :


```
function getByKey(kindOfMap, key) {
  for (const [k, v] of kindOfMap) {
    if (key === k) {
        return v;
    }
  }
  return undefined;
}

getByKey(kindOfMap, foo); // => 'Foo related data'
```


这时候我们可以使用map的一个特殊版本WeakMap来make life easy, WeakMap可以将object作为key, 并且访问时候性能好, 复杂度为O(1):

#### map 优点 

##### map可以更舒适的遍历
```
遍历对象时候我们可能会使用Object.entries
const colorsHex = {
  'white': '#FFFFFF',
  'black': '#000000'
};

for (const [color, hex] of Object.entries(colorsHex)) {
  console.log(color, hex);
}

而map则原本就是可遍历的:
const colorsHexMap = new Map();

colorsHexMap.set('white', '#FFFFFF');
colorsHexMap.set('black', '#000000');

for (const [color, hex] of colorsHexMap) {
  console.log(color, hex);
}
// 'white' '#FFFFFF'
// 'black' '#000000'
```

##### map有size属性
```

当我们要计算一个plain object中有多少个属性的时候通常会先用Object.keys
const exams = {
  'John Smith': '10 points',
  'Jane Doe': '8 points',
};

Object.keys(exams).length; // => 2


相比plain object, map自带size属性, 很方便
const examsMap = new Map([
  ['John Smith', '10 points'],
  ['Jane Doe', '8 points'],
]);
  
examsMap.size; // => 2
```
#### 总结

map不是plain object的一种替代, 而是plain object的一种补充, 当下面2种场景使用map就很重要:
```
运行时不确定key名的
key名是不同类型的

使用了map数据结构后便可以享受其提供的遍历特性比如 size, iterable.

 ```
#### WeakMap
```
const foo = { name: 'foo' };
const bar = { name: 'bar' };

const mapOfObjects = new WeakMap();

mapOfObjects.set(foo, 'Foo related data');
mapOfObjects.set(bar, 'Bar related data');

mapOfObjects.get(foo); // => 'Foo related data'
```
复制代码WeakMap的key只能是plain object, 而且是弱引用, 即引用的对象被回收, 则WeakMap也会自动移除对应的键值对, 因此WeakMap内部有多少成员取决于垃圾回收有没有进行, 所以它无法确保成员个数, 也不支持遍历, 用以下例子来解释下弱引用:

```
var wm = new WeakMap();
var element = document.querySelector(".element");
wm.set(element, {name: 'elName'});
wm.get(element) // {name: 'elName'}
element.parentNode.removeChild(element);
element = null;
wm.get(element) // undefined

```
此外WeakMap还可以用来实现class的私有属性
```
const privateData = new WeakMap();
class Person {    
    constructor(name, age) {        
        privateData.set(this, { name: name, age: age });    
    }
    
    getName() {
        return privateData.get(this).name;
    }
    getAge() {
        return privateData.get(this).age;    
    }
}

export default Person // privateData从外部无法访问
```

我们将bmi属性添加到了原有对象上, 但修改原有的对象可能会导致意外行为, 也可能这个对象被freeze()或者不可覆盖, 所以我们不得不再声明一个变量来存储这些额外的信息, 如果是一个对象还好, 如果是多个对象的话, 我们需要一个Array来存储这些额外信息的话就涉及到了索引的对应关系, 这是有风险的。 这时候我们可以使用 WeakMap 来完美解决我们的问题
```
let person1 = new Person('evle', 18, 'male', 1.8, 70);
let person2 = new Person('max', 20, 'female', 1.75, 68);
let personList = [person1, person2];

let vm = new Map();
personList.forEach( person => {
    vm.set(person, {bmi: person.weight / (person.height * 2)})
})

// 当我们想知道person2的bim指数时只需要get就可以了
vm.get(person2)
```

####  什么时候使用map 

如果一个对象的key 直到运行之前都是未知的,请用map
```
function isMap(value) {
  return value.toString() === '[object Map]';
}

const actorMap = new Map();

actorMap.set('name', 'Harrison Ford');
actorMap.set('toString', 'Actor: Harrison Ford');

// Works!
isMap(actorMap); // => true
```




