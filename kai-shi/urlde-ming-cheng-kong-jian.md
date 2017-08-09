# URL的名称空间
## 移除模板中硬编码的 URLS
在polls/index.html模板中，我们链接到poll的链接是硬编码成这样子的
```
<li><a href="/polls/{{ poll.id }}/">{{ poll.question }}</a></li>
```

问题出在硬编码，紧耦合使得在大量的模板中修改URLS将是一件让人抓狂的事情。
不过，既然在 polls.urls模块中的url()函数中定义了命名参数，那么就可以在
模板中使用  {% url %}  模板标记来移除特定的 URL 路径依赖:
```
<li><a href="{% url 'detail' poll.id %}">{{ poll.question }}</a>
</li>
```
注意
```
如果  {% url 'detail' poll.id %}  (含引号) 不能运行，但是{% url detail poll.id %}  (不含引号) 却能运行，那么意味着你使用的Djang 低于 < 1.5 版。这样的话，你需要在模板文件的顶部添加如下的声明:{% load url from future %}
```