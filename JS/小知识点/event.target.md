#### Event.target
触发事件的对象 (某个DOM元素) 的引用。当事件处理程序在事件的冒泡或捕获阶段被调用时，它与event.currentTarget不同。

#### event.currentTarget
Event 接口的只读属性 currentTarget 表示的，标识是当事件沿着 DOM 触发时事件的当前目标。它总是指向事件绑定的元素，而 Event.target 则是事件触发的元素。



语法：

```
event.target

event.target.nodeName 　　//获取事件触发元素标签名（li，p，div，img，button…）

event.target.id　　　　　　//获取事件触发元素id

event.target.className　　//获取事件触发元素classname

event.target.innerHTML　 //获取事件触发元素的内容（li）


[...event.target.className.inlcudes('.ico')]

```

