```
function CountInputChanges() {
  const [value, setValue] = useState('');
  const [count, setCount] = useState(-1);

 useEffect(() => setCount(count + 1));
  const onChange = ({ target }) => setValue(target.value);

  return (
    <div>
 <input type="text" value={value} onChange={onChange} />
 <div>Number of changes: {count}</div>
 </div>
  )
}

```

##### 它生成一个无限循环的组件重新渲染。

    在初始渲染之后，useEffect()执行更新状态的副作用回调函数。状态更新触发重新渲染。重新渲染之后，useEffect()执行副作用回调并再次更新状态，这将再次触发重新渲染。

<image src="https://img-blog.csdnimg.cn/20210402081305129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxNDQ5MjQ1ODg0,size_16,color_FFFFFF,t_70" width=500>

### 解决方案
    1.通过依赖来解决
        useEffect(() => setCount(count + 1), [value]);
            * 加[value]作为useEffect的依赖，这样只有当[value]发生变化时，计数状态变量才会更新。这样做可以解决无限循环。
    2.使用 ref
        其思想是更新 Ref 不会触发组件的重新渲染。
```
    import { useEffect, useState, useRef } from "react";

    function CountInputChanges() {
        const [value, setValue] = useState("");
        const countRef = useRef(0);

        useEffect(() => countRef.current++);
        const onChange = ({ target }) => setValue(target.value);

    return (
        <div>
            <input type="text" value={value} onChange={onChange} />
            <div>Number of changes: {countRef.current}</div>
        </div>
    );
    }

```
* useEffect(() => countRef.current++) 每次由于value的变化而重新渲染后，countRef.current++就会返回。引用更改本身不会触发组件重新渲染。



###  无限循环和新对象引用

* 即使正确设置了useEffect()依赖关系，使用对象作为依赖关系时也要小心。

    例如，下面的组件CountSecrets监听用户在input中输入的单词，一旦用户输入特殊单词'secret'，统计 ‘secret’ 的次数就会加 1。
```
import { useEffect, useState } from "react";

function CountSecrets() {
  const [secret, setSecret] = useState({ value: "", countSecrets: 0 });

  useEffect(() => {
    if (secret.value === 'secret') {
 setSecret(s => ({...s, countSecrets: s.countSecrets + 1}));    }
 }, [secret]);
  const onChange = ({ target }) => {
    setSecret(s => ({ ...s, value: target.value }));
  };

  return (
    <div>
 <input type="text" value={secret.value} onChange={onChange} />
 <div>Number of secrets: {secret.countSecrets}</div>
 </div>
  );
}

```
当前输入 secret，secret.countSecrets的值就开始不受控制地增长。

这是一个无限循环问题。

为什么会这样？

    secret对象被用作useEffect(..., [secret])。在副作用回调函数中，只要输入值等于secret，就会调用更新函数
```
setSecret(s => ({...s, countSecrets: s.countSecrets + 1}));
```
这会增加countSecrets的值，但也会创建一个新对象。

secret现在是一个新对象，依赖关系也发生了变化。所以useEffect(..., [secret])再次调用更新状态和再次创建新的secret对象的副作用，以此类推。

### 避免将对象作为依赖项


### 总结
useEffect(callback, deps)是在组件渲染后执行callback(副作用)的 Hook。如果不注意副作用的作用，可能会触发组件渲染的无限循环。

生成无限循环的常见情况是在副作用中更新状态，没有指定任何依赖参数
```
useEffect(() => {
  // Infinite loop!
  setState(count + 1);
});
```
避免无限循环的一种有效方法是正确设置依赖项：
```
useEffect(() => {
  // No infinite loop
  setState(count + 1);
}, [whenToUpdateValue]);
```
另外，也可以使用 Ref，更新 Ref 不会触发重新渲染：
```
useEffect(() => {
  // No infinite loop
  countRef.current++;
});
```
无限循环的另一种常见方法是使用对象作为useEffect()的依赖项，并在副作用中更新该对象(有效地创建一个新对象)
```
useEffect(() => {
  // Infinite loop!
  setObject({
    ...object,
    prop: 'newValue'
  })
}, [object]);
```
避免使用对象作为依赖项，只使用特定的属性(最终结果应该是一个原始值)：
```
useEffect(() => {
  // No infinite loop
  setObject({
    ...object,
    prop: 'newValue'
  })
}, [object.whenToUpdateProp]);
```