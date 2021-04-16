### Http Header里的Content-Type
 * Content-Type 实体头部用于指示资源的MIME类型
    * 在响应中，Content-Type标头告诉客户端实际返回的内容的内容类型。浏览器会在某些情况下进行MIME查找，并不一定遵循此标题的值; 为了防止这种行为，可以将标题 X-Content-Type-Options 设置为 nosniff。
    * 用于定义网络文件的类型和网页的编码，决定浏览器将以什么形式、什么编码读取这个文件，这就是经常看到一些 PHP 网页点击的结果却是下载一个文件或一张图片的原因。

### 句法

    * Content-Type: text/html; charset=utf-8
    * Content-Type: multipart/form-data; boundary=something



#### 类型

 常见的媒体格式类型如下：

    * text/html ： HTML格式
    text/plain ：纯文本格式      
    text/xml ：  XML格式
    image/gif ：gif图片格式    
    image/jpeg ：jpg图片格式 
    image/png：png图片格式
   以application开头的媒体格式类型：

    application/xhtml+xml ：XHTML格式
    application/xml     ： XML数据格式
    application/atom+xml  ：Atom XML聚合格式    
    application/json    ： JSON数据格式
    application/pdf       ：pdf格式  
    application/msword  ： Word文档格式
    application/octet-stream ： 二进制流数据（如常见的文件下载）
    * application/x-www-form-urlencoded ： <form encType=””>中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）
    
另外一种常见的媒体格式是上传文件之时使用的：

        multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式


#### form的enctype属性为编码方式，常用有两种：application/x-www-form-urlencoded和multipart/form-data，默认为application/x-www-form-urlencoded。

    *  当action为get时候，浏览器用x-www-form-urlencoded的编码方式把form数据转换成一个字串（name1=value1&name2=value2...），
    然后把这个字串追加到url后面，用?分割，加载这个新的url

    *  当action为post时候，浏览器把form数据封装到http body中，然后发送到server。
     如果没有type=file的控件，用默认的application/x-www-form-urlencoded就可以了。 但是如果有type=file的话，就要用到multipart/form-data了。
     
    * 当action为post且Content-Type类型是multipart/form-data，浏览器会把整个表单以控件为单位分割，并为每个部分加上Content-Disposition(form-data或者file),
    Content-Type(默认为text/plain),name(控件name)等信息，并加上分割符(boundary)。