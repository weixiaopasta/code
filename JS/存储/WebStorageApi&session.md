### Web Storage API

Web Storage API 提供机制， 使浏览器能以一种比使用Cookie更直观的方式存储键/值对。

Web Storage 包含如下两种机制：


1. sessionStorage 为每一个给定的源（given origin）维持一个独立的存储区域，该存储区域在页面会话期间可用（即只要浏览器处于打开状态，包括页面重新加载和恢复）。
2. localStorage 同样的功能，但是在浏览器关闭，然后重新打开后数据仍然存在。

### 过期时间

sessionStorage 属性允许你访问一个，对应当前源的 session Storage 对象。它与 localStorage 相似，不同之处在于 localStorage 里面存储的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除。

打开多个相同的URL的Tabs页面，会创建各自的sessionStorage。
关闭对应浏览器窗口（Window）/ tab，会清除对应的sessionStorage。


### 存储格式
seesionStorage只能存储字符串的数据，对于JS中常用的数组或对象却不能直接存储，我们可以通过JSON.stringify()将json数据类型转化成字符串，再存储到storage中就可以了，获取数据时再使用JSON.parse()将读取的字符串转换成对象即可。（数组本质上也是对象类型）

### 浏览器窗口概念

引入了一个“浏览器窗口”的概念，sessionStorage是在同源的同窗口中，始终存在的数据，也就是说只要这个浏览器窗口没有关闭，即使刷新页面或进入同源另一个页面，数据仍然存在，关闭窗口后，sessionStorage就会被销毁，同时“独立”打开的不同窗口，即使是同一页面，sessionStorage对象也是不同的


### 区别
sessionStorage、localStorage和cookie的区别 
共同点：都是保存在浏览器端、且同源的 
区别： 
1、cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下 
2、存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大 
3、数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭之前有效；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭 
4、作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localstorage在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的 
5、web Storage支持事件通知机制，可以将数据更新的通知发送给监听者 
6、web Storage的api接口使用更方便


### Cookie与Session的区别

cookie数据存放在客户的浏览器上，session数据放在服务器上；
cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗，考虑到安全应当使用session；
session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能。考虑到减轻服务器性能方面，应当使用COOKIE；
单个cookie在客户端的限制是3K，就是说一个站点在客户端存放的COOKIE不能超过3K；