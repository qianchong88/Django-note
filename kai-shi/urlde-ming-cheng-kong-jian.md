# URL的名称空间
## 移除模板中硬编码的 URLS
在polls/index.html模板中，我们链接到poll的链接是硬编码成这样子的
```
<li><a href="/polls/{{ poll.id }}/">{{ poll.question }}</a></li>
```

问题出在硬编码，紧耦合使得在大量的模板中修改URLS将是一件让人抓狂的事情。
不过，既然在 polls.urls模块中的url()函数中定义了命名参数，那么就可以在
模板中使用 \{% url %\}  模板标记来移除特定的URL 路径依赖:
```
<li><a href="{% url 'detail' poll.id %}">{{ poll.question }}</a>
</li>
```
注意
```
如果  {% url 'detail' poll.id %}  (含引号) 不能运行，但是{% url detail poll.id %}  (不含引号) 却能运行，那么意味着你使用的Djang 低于 < 1.5 版。这样的话，你需要在模板文件的顶部添加如下的声明:{% load url from future %}
```
其原理就是在 polls.urls 模块中寻找指定名称的URL 定义。 命名为 'detail' 的URL 就如下所示那样定义的一样：
```
# 'name' 的值由 {% url %} 模板标记来引用
url(r'^(?P<poll_id>\d+)/$', views.detail, name='detail'),
```
如果你想将polls的detail视图的URL改成其他样子，例如polls/specifics/12/
这样子，那就不需要在模板（或者模板集）中修改而只要在 polls/urls.py 修改就行了:

```
# 新增 'specifics'
url(r'^specifics/(?P<poll_id>\d+)/$', views.detail, name='detail
'),
```
## URL名称空间
本项目只有一个应用：polls 。在实际的 Django 项目中，可能有多个应用。Django 将如何区分它们的URL 名称的呢？
比如， polls应用有一个名称为 detail 视图，而同一个项目中的blog应该也有一个名称为detail的视图。 使用\{% url 'detail' poll.id %\}模板标记创建应用的url时Django 是如何知道选择哪个detail呢？
为了解决上述名称冲突问题，在你的 root URLconf 配置中添加命名空间。在 myweb/urls.py 文件 (项目的urls.py  ，不是应用的) 中，修改为包含命名空间的定义:
```
from django.conf.urls import patterns, include, url
from django.contrib import admin
admin.autodiscover()
urlpatterns = patterns('',
url(r'^polls/', include('polls.urls', namespace="polls")),
url(r'^admin/', include(admin.site.urls)),
)
```
现在将 polls/index.html 模板中原来的 detail 视图中的:
```
<li><a href="{% url 'detail' poll.id %}">{{ poll.question }}</a>
</li>
```
修改为包含命名空间的 detail 视图:
```
<li><a href="{% url 'polls:detail' poll.id %}">{{ poll.question
}}</a></li>
```
这样就Django就可以准确找到对应的detail视图
