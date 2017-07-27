# 使用模板
## 在视图中查询数据并显示
每个视图只负责以下两件事中的一件：返回一个 HttpResponse 对象，其中包含了
所请求页面的内容，或者抛出一个异常例如 Http404 。现在使用Django提供的数据库api修改下 index() 视图， 让它显示系统中最新发布的 5 个调查问题，以逗号
分割并按发布日期排序：
```

from django.http import HttpResponse
from polls.models import Poll
def index(request):
   #获取最新的5条调查
    latest_poll_list = Poll.objects.order_by('-pub_date')[:5]
    output = ', '.join([p.question for p in latest_poll_list])
    return HttpResponse(output)
```

在这有个问题，页面的设计是硬编码在视图中的。如果你想改变页面的外观，就必须修改这里的 Python 代码。因此，让我们使用 Django 的模板系统创建一个模板给视图用，就使页面设计从 Python 代码中 分离出来了。

## 使用视图模板
首先，在 polls 目录下创建一个 templates 目录。 Django 将会在那寻找模板。在你刚才创建的templates 目录下，另外创建个名为polls 的目录，并在其中创建一个 index.html 文件。即模板应该polls/templates/polls/index.html 。
将以下代码添加到该模板中:

```
{% if latest_poll_list %}
<ul>
{% for poll in latest_poll_list %}
<li><a href="/polls/{{ poll.id }}/">{{ poll.question }}</a></li>
{% endfor %}
</ul>
{% else %}
<p>No polls are available.</p>
{% endif %}
```

现在在 index 视图中使用这个模板：
```
from django.template import loader,Context
def index(request):
   latest_poll_list = Poll.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context ={
        'latest_poll_list': latest_poll_list,
        }
    return HttpResponse(template.render(context))
```
浏览器中输入http://127.0.0.1:8000/poll/index

![](/assets/b2.png)

## 快捷方式: render()
这是一个非常常见的习惯用语，用于加载模板，填充上下文并返回一个含有模板渲
染结果的 HttpResponse 对象。 Django 提供了一种快捷方式。这里重写完整的
index() 视图

```

from django.shortcuts import render
from polls.models import Poll
def index(request):
    latest_poll_list = Poll.objects.all().order_by('-pub_date')[:5]
    context = {'latest_poll_list': latest_poll_list}
    return render(request, 'polls/index.html', context)
```