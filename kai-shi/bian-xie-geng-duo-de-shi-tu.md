# 编写更多的视图

Django视图views.py模块中的一个方法，接受一个必须的httprequest参数，和其它的一些可选参数，在polls/views.py中添加如下代码：

```
#接收URL传递过来的参数
def detail(request, poll_id):
    return HttpResponse("You're looking at poll %s." % poll_id)
def results(request, poll_id):
    return HttpResponse("You're looking at the results of poll %s." % poll_id)
def vote(request, poll_id):
    return HttpResponse("You're voting on poll %s." % poll_id)
```

将新视图按如下所示的 url\(\) 方法添加到 polls.urls 模块中去：

```
urlpatterns = [
    url(r'^$',index), # /poll默认指向index
    url(r'^index$',index),
    #带参数的 ex /poll/5/ 
    url(r'^(?P<poll_id>\d+)/$', detail, name='detail'),
    #带参数的URL ex /poll/5/result/
    url(r'^(?P<poll_id>\d+)/results/$', results, name='results'),
    #带参数的URL ex /poll/5/vote/
    url(r'^(?P<poll_id>\d+)/vote/$', vote, name='vote'),
]
```

打开浏览器分别访问http://127.0.0.1:8000/poll/5/，http://127.0.0.1:8000/poll/5/vote,http://127.0.0.1:8000/poll/5/results 就会看到相应的效果。


