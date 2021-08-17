### 1.不要更改 Hook 调用顺序
```
function FetchGame({ id }) {
  if (!id) {
    return 'Please select a game to fetch';
  }

  const [game, setGame] = useState({ 
    name: '',
    description: '' 
  });

  useEffect(() => {
    const fetchGame = async () => {
      const response = await fetch(`/api/game/${id}`);
      const fetchedGame = await response.json();
      setGame(fetchedGame);
    };
    fetchGame();
  }, [id]);

  return (
    <div> <div>Name: {game.name}</div> <div>Description: {game.description}</div> </div>
  ); 

```
问题发生在这一判断：
```
function FetchGame({ id }) {
 if (!id) { return 'Please select a game to fetch'; }  
   // ...
}
```

当id为空时，组件渲染'Please select a game to fetch'并退出，不调用任何 Hook。

但是，如果 id不为空（例如等于’1’），则会调用useState()和 useEffect()。

有条件地执行 Hook 可能会导致难以调试的意外错误。React Hook的内部工作方式要求组件在渲染之间总是以相同的顺序调用 Hook。

这正是钩子的第一条规则:不要在循环、条件或嵌套函数内调用 Hook。

   * 解决方法就是将条件判断放到 Hook 后面
   ```
   function FetchGame({ id }) {
  const [game, setGame] = useState({ 
    name: '',
    description: '' 
  });

  useEffect(() => {
    const fetchGame = async () => {
      const response = await fetch(`/api/game/${id}`);
      const fetchedGame = await response.json();
      setGame(fetchedGame);
    };
 if (id) {      fetchGame();     }  }, [id]);

 if (!id) { return 'Please select a game to fetch'; }
  return (
    <div>
 <div>Name: {game.name}</div>
 <div>Description: {game.description}</div>
 </div>
  );
}
   ```

   现在，无论id是否为空，useState()和useEffect() 总是以相同的顺序被调用，这就是 Hook 应该始终被调用的方式。

###  2.不要使用过时状态
```
function MyIncreaser() {
  const [count, setCount] = useState(0);

  const increase = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  const handleClick = () {
    increase(); 
    increase(); 
    increase(); 
 };

  return (
    <>
 <button onClick={handleClick}>Increase</button>
 <div>Counter: {count}</div>
 </>
  );
}

```

即使在handleClick()中3次调用了increase()，计数也只增加了1。

问题在于setCount(count + 1)状态更新器。当按钮被点击时，React调用setCount(count + 1)3次

setCount(count + 1)的第一次调用正确地将计数器更新为count + 1 = 0 + 1 = 1。但是，接下来的两次setCount(count + 1)调用也将计数设置为1，因为它们使用了过时的stale状态。

#### 通过使用函数方式更新状态来解决过时的状态。我们用setCount(count => count + 1)代替setCount(count + 1)：


```
function MyIncreaser() {
    const [count, setCount] = useState(0);

    const increase = useCallback(() => {
        setCount(count => count + 1); 
    },[]);

    const handleClick = () {
        increase();
        increase();
        increase();
    };

  return (
    <>
 <button onClick={handleClick}>Increase</button>
 <div>Counter: {count}</div>
 </>
  );
}

```

## 如果你使用当前状态来计算下一个状态，总是使用函数方式来更新状态:setValue(prevValue => prevValue + someResult)。

 ### 3.Hooks中的过时闭包
* 3.1 useEffect()

```
function WatchCount() {
  const [count, setCount] = useState(0);

  useEffect(function() {
    setInterval(function log() {
      console.log(`Count is: ${count}`);
    }, 2000);
  }, []);

  return (
    <div> {count} <button onClick={() => setCount(count + 1) }> Increase </button> </div>
  );
}

```

并点击几次增加按钮。然后看看控制台，每2秒出现一次Count is: 0，尽管count状态变量实际上已经增加了几次。

为什么会这样？

第一次渲染时，状态变量count初始化为0。

组件安装后，useEffect()调用 setInterval(log, 2000)计时器函数，该计时器函数计划每2秒调用一次log()函数。 在这里，闭包log()捕获到count变量为0。

之后，即使在单击Increase按钮时count增加，计时器函数每2秒调用一次的log()，使用count的值仍然是0。log()成为一个过时的闭包。
#### 解决方案是让useEffect()知道闭包log()依赖于count，并在count改变时正确处理间隔的重置

```
function WatchCount() {
  const [count, setCount] = useState(0);

  useEffect(function() {
    const id = setInterval(function log() {
      console.log(`Count is: ${count}`);
    }, 2000);
    return function() {
      clearInterval(id);
    }
 }, [count]);
  return (
    <div>
 {count}
 <button onClick={() => setCount(count + 1) }>
 Increase
 </button>
 </div>
  );
}
```

当一个返回基于前一个状态的新状态的回调函数被提供给状态更新函数时，React确保将最新的状态值作为该回调函数的参数提供
```
setCount(alwaysActualStateValue => newStateValue);
```
这就是为什么在状态更新过程中出现的过时装饰问题可以通过函数这种方式来解决。


4.总结
当闭包捕获过时的变量时，就会发生过时的闭包问题。

解决过时闭包的有效方法是正确设置React钩子的依赖项。或者，在失效状态的情况下，使用函数方式更新状态。


### 4.不要将状态用于基础结构数据

有一次，我需要在状态更新上调用副作用，在第一个渲染不用调用副作用。useEffect(callback, deps)总是在挂载组件后调用回调函数:所以我想避免这种情况。

我找到了以下的解决方案

```
function MyComponent() {
  const [isFirst, setIsFirst] = useState(true);
  const [count, setCount] = useState(0);

  useEffect(() => {
    if (isFirst) {
      setIsFirst(false);
      return;
    }
    console.log('The counter increased!');
  }, [count]);

  return (
    <button onClick={() => setCount(count => count + 1)}> Increase </button>
  );
}

```
状态变量isFirst用来判断是否是第一次渲染。一旦更新setIsFirst(false)，就会出现另一个无缘无故的重新渲染。

保持count状态是有意义的，因为界面需要渲染 count 的值。 但是，isFirst不能直接用于计算输出。

是否为第一个渲染的信息不应存储在该状态中。 基础结构数据，例如有关渲染周期（即首次渲染，渲染数量），计时器ID（setTimeout()，setInterval()），对DOM元素的直接引用等详细信息，应使用引用useRef()进行存储和更新。

我们将有关首次渲染的信息存储到 Ref 中：

```
 const isFirstRef = useRef(true);  const [count, setCount] = useState(0);

  useEffect(() => {
    if (isFirstRef.current) {
      isFirstRef.current = false;
      return;
    }
    console.log('The counter increased!');
  }, [count]);

  return (
    <button onClick={() => setCounter(count => count + 1)}>
 Increase
 </button>
  );
}

```
isFirstRef是一个引用，用于保存是否为组件的第一个渲染的信息。 isFirstRef.current属性用于访问和更新引用的值。

#### 重要说明：更新参考isFirstRef.current = false不会触发重新渲染。

#### 5.不要忘记清理副作用

很多副作用，比如获取请求或使用setTimeout()这样的计时器，都是异步的。

如果组件卸载或不再需要该副作用的结果，请不要忘记清理该副作用。

下面的组件有一个按钮。当按钮被点击时，计数器每秒钟延迟增加1

```
function DelayedIncreaser() {
  const [count, setCount] = useState(0);
  const [increase, setShouldIncrease] = useState(false);

  useEffect(() => {
    if (increase) {
      setInterval(() => {
        setCount(count => count + 1)
      }, 1000);
    }
  }, [increase]);

  return (
    <> <button onClick={() => setShouldIncrease(true)}> Start increasing </button> <div>Count: {count}</div> </>
  );
}

```

点击开始按钮。正如预期的那样，状态变量count每秒钟都会增加。

在进行递增操作时，单击umount 按钮，卸载组件。React会在控制台中警告更新卸载组件的状态。
<img src="https://img-blog.csdnimg.cn/img_convert/f218a7310e2f372690e727e873f8af84.png">

修复DelayedIncreaser很简单：只需从useEffect()的回调中返回清除函数：
```
 // ...
  useEffect(() => {
    if (increase) {
      const id = setInterval(() => {
        setCount(count => count + 1)
      }, 1000);
 return () => clearInterval(id);    }
  }, [increase]);

  // ...
}
```
也就是说，每次编写副作用代码时，都要问自己它是否应该清理。计时器，频繁请求(如上传文件)，sockets 几乎总是需要清理
