### async/await优点：

a. 它做到了真正的串行的同步写法，代码阅读相对容易

b. 对于条件语句和其他流程语句比较友好，可以直接写到判断条件里面

c. 处理复杂流程时，在代码清晰度方面有优势

### async/await缺点：

a. 无法处理promise返回的reject对象，要借助try...catch...

b. 用 await 可能会导致性能问题，因为 await 会阻塞代码，也许之后的异步代码并不依赖于前者，但仍然需要等待前者完成，导致代码失去了并发性。

c. try...catch...内部的变量无法传递给下一个try...catch...,Promise和then/catch内部定义的变量，能通过then链条的参数传递到下一个then/catch，但是async/await的try内部的变量，如果用let和const定义则无法传递到下一个try...catch...，只能在外层作用域先定义好。

### promise的一些问题：
a. 一旦执行，无法中途取消，链式调用多个then中间不能随便跳出来

b. 错误无法在外部被捕捉到，只能在内部进行预判处理，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部

c. Promise内部如何执行，监测起来很难，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）



