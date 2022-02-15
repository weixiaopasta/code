#### 深拷贝

```
const myDeepCopy = JSON.parse(JSON.stringify(myOriginal));
```

### 缺点

```
循环引用：JSON.stringify() 的对象中如果有循环引用会抛出异常 Converting circular structure to JSON。
其他数据类型：JSON.stringify() 无法拷贝 Map、Set、RegExp 这些特殊数据类型。
函数：JSON.stringify() 会默认移除函数。
过滤为undefined的值
```
