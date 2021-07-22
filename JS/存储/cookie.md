###  cookie

HTTP Cookie（也叫 Web Cookie 或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。

http是无状态协议，它对于事务处理没有记忆能力。

可以使用Cookie来解决无状态的问题，第一次访问的时候给客户端发送一个Cookie，当客户端再次来的时候，拿着Cookie(通行证)，那么服务器就知道这个是”老用户“。


#### 限制访问 Cookie

有两种方法可以确保 Cookie 被安全发送，并且不会被意外的参与者或脚本访问：Secure 属性和HttpOnly 属性。

* 标记为 Secure 的 Cookie 只应通过被 HTTPS 协议加密过的请求发送给服务端，

* JavaScript Document.cookie API 无法访问带有 HttpOnly 属性的cookie；此类 Cookie 仅作用于服务器。例如，持久化服务器端会话的 Cookie 不需要对 JavaScript 可用，而应具有 HttpOnly 属性。此预防措施有助于缓解跨站点脚本（XSS）攻击。

### 客户端设置cookie与服务端设置cookie有什么区别?

无论是客户端还是服务端, 都只能向自己的域或者更高级域设置cookie

服务端可以设置 httpOnly: true, 带有该属性的cookie客户端无法读取


### ajax请求到底会不会带上cookie?

默认情况下不会, 只有当设置了 credentials 时才会带上与请求同域的cookie, 并且服务端需要设置响应头 Access-Control-Allow-Credentials: true, 否则浏览器会报错, 拿不到响应