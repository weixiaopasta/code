#### 你曾经一直在寻觅的 JavaScript 模板引擎就在这里！
```功能丰富且强大，并支持块级继承（block inheritance）、自动转义、宏（macro）、异步控制等等。完美继承了 jinja2 的衣钵。
快速 & 干练 并且高效。运行时代码经过压缩之后只有 8K 大小， 可在浏览器端执行预编译模板。
可扩展 性超强，用户可以自定义过滤器（filter）和扩展模块。
到处 可运行，无论是 node 还是任何浏览器都支持，并且还可以预编译模板。
```

```
{% extends "base.html" %}

{% block header %}
<h1>{{ title }}</h1>
{% endblock %}

{% block content %}
<ul>
  {% for name, item in items %}
  <li>{{ name }}: {{ item }}</li>
  {% endfor %}
</ul>
{% endblock %}
      
```



https://nunjucks.bootcss.com/