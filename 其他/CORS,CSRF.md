#### CORS  跨域资源共享

AJAX 的跨域设计就是，只要表单可以发，AJAX 就可以直接发。

它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。

整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。

因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。


使用Cookie实现登录的另外一个问题就是跨域，现在往往都采用前后端分离的方式进行开发，在开发的过程中，前端和后端通常不在一个域下，由于浏览器的同源策略，Cookie不能传入到后端。至于同源策略，不明白的小伙伴可以问一下度娘，这里不过多介绍了。要解决这个问题，在前端、后端都要进行设置，在我的另一篇文章《前后端分离|关于登录状态那些事》中有详细的介绍。总体归纳为：

后端设置CORS允许跨域的域名，并且withCredentials设置为true；
前端在向后端发送请求时，也需要设置withCredentials = true;
这样，我们的Cookie就可以实现跨域了。不进行这些设置，Cookie跨域是不可能的，同源策略保证了我们Cookie的安全。



#### 浏览器将CORS请求分成两类：简单请求（simple request）和非简单请求（not-so-simple request）。



(1) 请求方法是以下三种方法之一：
```
HEAD
GET
POST
```
（2）HTTP的头信息不超出以下几种字段：
```
Accept
Accept-Language
Content-Language
Last-Event-ID
Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
```

这是为了兼容表单（form），因为历史上表单一直可以发出跨域请求。AJAX 的跨域设计就是，只要表单可以发，AJAX 就可以直接发。

凡是不同时满足上面两个条件，就属于非简单请求。

浏览器对这两种请求的处理，是不一样的。


#### 与JSONP的比较

CORS与JSONP的使用目的相同，但是比JSONP更强大。

JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

### 评论理解
1、cookie 可以跨域不可以跨域名，子域名也是可以共享顶级域名下的cookie的，response可以写入任意的domain，但是request无法将其他的域名下的cookie写入，document.cookie也是同样

2、关于Credentials，重要的不是会不会发送，而是cookie发送到服务端后如果response header中无Access-Control-Allow-Credentials: true，那么浏览器会报异常，js无法拿到跨域请求的response对象


正解，其实这两个开关一个管服务器，一个管客户端，客户端fetch设了credentials设了include,就一定会发送目标网站的cookie,服务器如果credentials即使不设为ture,照样会返回正确内容（取决于服务器开发工程师），但有一点，如果你服务器不设为true,浏览器就认为你违反了规则，把服务器返回的内容屏蔽掉（虽然屏蔽掉了），这就会出现，返回码是200，但无正确内容收到的情况。

如果只是个GET请求的话，浏览器并不知道这个未知的服务器是不是支持 Credentials，只能听开发者的，带上 cookie 去做请求，而服务器不返回 Access-Control-Allow-Credentials 则告诉浏览器：请直接报错，不要把响应暴露给前端脚本。

当一个请求在浏览器端发送出去后，服务端是会收到的并且也会处理和响应，只不过浏览器在解析这个请求的响应之后，发现不属于浏览器的同源策略（地址里面的协议、域名和端口号均相同）也没有包含正确的 CORS 响应头，返回结果被浏览器给拦截了


预检请求
预检请求是在发送实际的请求之前，客户端会先发送一个 OPTIONS 方法的请求向服务器确认，如果通过之后，浏览器才会发起真正的请求，这样可以避免跨域请求对服务器的用户数据造成影响。

看到这里你可能有疑问为什么上面的示例没有预检请求？因为 CORS 将请求分为了两类：简单请求和非简单请求。我们上面的情况属于简单请求，所以也就没有了预检请求。

让我们继续在看下简单请求和非简单请求是如何定义的。

请求方法为：GET、POST、HEAD，请求头 Content-Type 为：text/plain、multipart/form-data、application/x-www-form-urlencoded 的就属于 “简单请求” 不会触发 CORS 预检请求。


还有一点需要注意，如果我们在请求中设置了 credentials: "include" 服务端就不能设置 Access-Control-Allow-Origin: "*" 只能设置为一个明确的地址。

### 另 
发起 content-type=application/json 的非简单请求，这时候传参要注意为json字符串
这里可以看到，浏览器连续发送了两个jsonp.do请求 ， 第一个就是“预检请求”，类型为OPTIONS，因为我们设置了content-type这个属性，所以预检请求的Access-Control-Expose-Headers必须携带content-type，否则预检会失败。

#### CSRF 
https://blog.csdn.net/stpeace/article/details/53512283

CSRF跨站点请求伪造(Cross—Site Request Forgery)，跟XSS攻击一样，存在巨大的危害性，你可以这样来理解：
       攻击者盗用了你的身份，以你的名义发送恶意请求，对服务器来说这个请求是完全合法的，但是却完成了攻击者所期望的一个操作，比如以你的名义发送邮件、发消息，盗取你的账号，添加系统管理员，甚至于购买商品、虚拟货币转账等。 如下：其中Web A为存在CSRF漏洞的网站，Web B为攻击者构建的恶意网站，User C为Web A网站的合法用户。

  CSRF攻击攻击原理及过程如下：

       1. 用户C打开浏览器，访问受信任网站A，输入用户名和密码请求登录网站A；

       2.在用户信息通过验证后，网站A产生Cookie信息并返回给浏览器，此时用户登录网站A成功，可以正常发送请求到网站A；

       3. 用户未退出网站A之前，在同一浏览器中，打开一个TAB页访问网站B；

       4. 网站B接收到用户请求后，返回一些攻击性代码，并发出一个请求要求访问第三方站点A；


       5. 浏览器在接收到这些攻击性代码后，根据网站B的请求，在用户不知情的情况下携带Cookie信息，向网站A发出请求。网站A并不知道该请求其实是由B发起的，所以会根据用户C的Cookie信息以C的权限处理该请求，导致来自网站B的恶意代码被执行。

 CSRF攻击实例

    受害者 Bob 在银行有一笔存款，通过对银行的网站发送请求 http://bank.example/withdraw?account=bob&amount=1000000&for=bob2 可以使 Bob 把 1000000 的存款转到 bob2 的账号下。通常情况下，该请求发送到网站后，服务器会先验证该请求是否来自一个合法的 session，并且该 session 的用户 Bob 已经成功登陆。

    黑客 Mallory 自己在该银行也有账户，他知道上文中的 URL 可以把钱进行转帐操作。Mallory 可以自己发送一个请求给银行：http://bank.example/withdraw?account=bob&amount=1000000&for=Mallory。但是这个请求来自 Mallory 而非 Bob，他不能通过安全认证，因此该请求不会起作用。

    这时，Mallory 想到使用 CSRF 的攻击方式，他先自己做一个网站，在网站中放入如下代码： src=”http://bank.example/withdraw?account=bob&amount=1000000&for=Mallory ”，并且通过广告等诱使 Bob 来访问他的网站。当 Bob 访问该网站时，上述 url 就会从 Bob 的浏览器发向银行，而这个请求会附带 Bob 浏览器中的 cookie 一起发向银行服务器。大多数情况下，该请求会失败，因为他要求 Bob 的认证信息。但是，如果 Bob 当时恰巧刚访问他的银行后不久，他的浏览器与银行网站之间的 session 尚未过期，浏览器的 cookie 之中含有 Bob 的认证信息。这时，悲剧发生了，这个 url 请求就会得到响应，钱将从 Bob 的账号转移到 Mallory 的账号，而 Bob 当时毫不知情。等以后 Bob 发现账户钱少了，即使他去银行查询日志，他也只能发现确实有一个来自于他本人的合法请求转移了资金，没有任何被攻击的痕迹。而 Mallory 则可以拿到钱后逍遥法外。 


#### 防御CSRF攻击：

目前防御 CSRF 攻击主要有三种策略：验证 HTTP Referer 字段；在请求地址中添加 token 并验证；在 HTTP 头中自定义属性并验证。





