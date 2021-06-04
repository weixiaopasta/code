启动服务：
brew services start nginx
访问：http://localhost:8080/后，会有个欢迎来到nginx的页面。
停止服务：
brew services stop nginx

查看安装信息(经常用到, 比如查看安装目录等)
brew info nginx


brew search nginx //查询要安装的软件是否存在

### 查看报错文件的路径
tail -f *


### 查看nginx进程
ps -aux | grep nginx

#### 查看启动状态是否有报错
nginx -t

重新加载nginx
nginx -s reload
停止nginx
nginx -s stop



nginx  #启动nginx
nginx -s quit  #快速停止nginx
nginx -V #查看版本，以及配置文件地址
nginx -v #查看版本
nginx -s reload|reopen|stop|quit   #重新加载配置|重启|快速停止|安全关闭nginx
nginx -h #帮助


### 查看启动状态是否有报错  nginx -t


查看进程是否都起来了

~ ps -ef | grep nginx


```
 server{
        listen 443 ssl;
        server_name  j1.58cdn.com.cn;
        #ssl on;
        rewrite_log on;
        ssl_certificate   /opt/homebrew/etc/nginx/ssl/58_com-key/server.pem;
        ssl_certificate_key  /opt/homebrew/etc/nginx/ssl/58_com-key/server.key;        
        #房东委托
        location ~/frs/fangfe/fang-entrust-react {
            rewrite ^/frs/fangfe/fang-entrust-react/(.*?)/(.*) /$2 ;
            rewrite ^(.*?)_v(.*?)\.(.*) /$1.$3;
            break;
            root  /Users/wanglie/leo/work/2021/fangfe/fang-entrust-react/build;
        }
    }
```

```
server{
        listen 443 ssl;
        server_name  j1.58cdn.com.cn;
        #ssl on;
        rewrite_log on;
        ssl_certificate   /opt/homebrew/etc/nginx/ssl/58_com-key/server.pem;
        ssl_certificate_key  /opt/homebrew/etc/nginx/ssl/58_com-key/server.key;        
        #房东委托
        location ~/frs/fangfe/fang-entrust-react {
            rewrite ^/frs/fangfe/fang-entrust-react/(.*?)/(.*) /$2 ;
            rewrite ^(.*?)_v(.*?)\.(.*) /$1.$3;
            break;
            proxy_pass http://test.5   8.com:1024;
            
        }
    }
```

关键步骤：
host配置：
127.0.0.1 j1.58cdn.com.cn

最后：
重新启动nginx 记住本地charles 需要打开才能生效哦



https://xuexb.github.io/learn-nginx/example/proxy_pass.html#url-%E5%8C%85%E5%90%AB%E8%B7%AF%E5%BE%84