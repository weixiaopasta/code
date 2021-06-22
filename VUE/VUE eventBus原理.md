解题思路：
EventBus 是消息传递的一种方式，基于一个消息中心，订阅和发布消息的模式，称为发布订阅者模式。

on('name', fn)订阅消息，name: 订阅的消息名称， fn: 订阅的消息
emit('name', args)发布消息, name: 发布的消息名称， args: 发布的消息

代码实现：
```
class Bus {
  constructor () {
    this.callbacks = {}
  }
  $on(name,fn) {
    this.callbacks[name] = this.callbacks[name] || []
    this.callbacks[name].push(fn)
  }
  $emit(name,args) {
    if(this.callbacks[name]){
       //存在遍历所有callback
       this.callbacks[name].forEach(cb => cb(args))
    }
  }
}

```

使用

```
const EventBus = new EventBusClass()
EventBus.on('fn1', function(msg) {
    alert(`订阅的消息是：${msg}`);
});
EventBus.emit('fn1', '你好，世界！');
```