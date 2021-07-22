https://localforage.docschina.org/
#### LOCALFORAGE
改进的离线存储


// 设置值
```
localforage.setItem('key', 'value').then(doSomethingElse);
```
// localForage 同样支持回调函数
```
localforage.setItem('key', 'value', doSomethingElse);
```

localForage 是一个 JavaScript 库，通过简单类似 localStorage API 的异步存储来改进你的 Web 应用程序的离线体验。它能存储多种类型的数据，而不仅仅是字符串。

localForage 有一个优雅降级策略，若浏览器不支持 IndexedDB 或 WebSQL，则使用 localStorage。在所有主流浏览器中都可用：Chrome，Firefox，IE 和 Safari（包括 Safari Mobile）。

localForage 提供回调 API 同时也支持 ES6 Promises API，你可以自行选择。

