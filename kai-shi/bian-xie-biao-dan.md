# 表单
更新poll 的 detail 模板，在模板中包含 HTML表单组件:
```
<h1>{{ poll.question }}</h1>
{% if error_message %}<p><strong>{{ error_message }}</strong></p
>{% endif %}
<form action="{% url 'polls:vote' poll.id %}" method="post">
{% csrf_token %}
{% for choice in poll.choice_set.all %}
<input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}" />
<label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br />
{% endfor %}
<input type="submit" value="Vote" />
</form>
```

效果如图：
![](/assets/11-1.png)

创建一个 Django 视图vote来处理提交的数据。 
1.添加路由：


```
url(r'^(?P<poll_id>\d+)/vote/$', views.vote, name='vote')
```


编写vote视图在polls/views.py 中添加如下代码:



```
from django.shortcuts import render,get_object_or_404
from django.http import Http404
from django.http import HttpResponse,HttpResponseRedirect
from polls.models import Poll,Choice
from django.template import loader,Context
from django.core.urlresolvers import reverse

......
def vote(request, poll_id):
    p = get_object_or_404(Poll, pk=poll_id)
    try:
        selected_choice = p.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the poll voting form.
        return render(request, 'polls/detail.html', {'poll': p,'error_message': "You didn't select a choice.",})
    else:
        selected_choice.votes += 1
        selected_choice.save()
        
    return HttpResponseRedirect(reverse('polls:results', args=(p.id,)))
```

request.POST 是一个类似字典的对象，可以让你 通过关键字名称来获取提交的数
据。在本例中， request.POST['choice'] 返回了所选择的投票项目的 ID ，以字符串的形式。 request.POST 的值永远是字符串形式的。
Django 也同样的提供了通过 request.GET 获取 GET 数据的方法
如果 choice在POST中不存在时request.POST['choice'] 将抛出 KeyError异常。上面的代码若检测到抛出的是 KeyError 异常就会向 poll 显示一条错误信息。

在增加了投票选项的统计数后，代码返回一个 HttpResponseRedirect 对象而不是常见的 HttpResponse 对象。 HttpResponseRedirect 对象需要一个参数：用户将被重定向的 URL 。

在 HttpResponseRedirect 的构造方法中使用了 reverse() 函数。此函数有助于避免在视图中硬编码 URL 的功能。它指定了我们想要的跳转的视图函数名以及视图函数中 URL 模式相应的可变参数。在本例中reverse() 将会返回类似如下所示的字符串'/polls/3/results/'

当有人投票后，vote() 视图会重定向到投票结果页。编写result视图

```
def results(request, poll_id):
    poll = get_object_or_404(Poll, pk=poll_id)
    return render(request, 'polls/results.html', {'poll': poll})
```

现在，创建一个 polls/results.html 模板:
```
<h1>{{ poll.question }}</h1>
<ul>
{% for choice in poll.choice_set.all %}
<li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
{% endfor %}
</ul>
<a href="{% url 'pollSs:detail' poll.id %}">Vote again?</a>
```

现在，在浏览器中访问 /polls/1/ 并完成投票。每次投票后你将会看到结果页数据都有更新。 如果你没有选择投票选项就提交了，将会看到错误的信息。
