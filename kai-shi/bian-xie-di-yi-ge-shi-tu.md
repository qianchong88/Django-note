# 编写第一个视图

在 Django 应用程序中，视图是一“类”具有特定功能和模板的网页。创建index页面来显示一段输出文字。打开polls/views.py 并在其中输入以下 Python 代码

```
from django.http import HttpResponse
def index(request):
    return HttpResponse("Hello, world. You're at the poll index.")
```

在 Django 中这可能是最简单的视图了。为了调用这个视图，需要将它映射到  
一个 URL – 为此需要配置一个URLconf 。打开myweb写的urls.py,在urlpatterns中加入一条路由后代码

```\`
from django.conf.urls import url
from django.contrib import admin
from polls.views import index #从poll.views中导入刚刚写的index方法。

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^poll/index$',index),#新增的路由
]
```

浏览器中访问[http://127.0.0.1:8000/poll/index](http://127.0.0.1:8000/poll/index)

![](/assets/b1.png)  
这样一个最简单的视图就完成了。但是把所有的URL配置都写在项目的配置文件中不太合理，改进一下将polls应用的相关的URL写到polls中对URL分级。在 polls 目录下创建一个名为 urls.py 的 URLconf 文档，应用目录现在看起来像这样：  
polls/  
    \_\_init\_\_.py  
    admin.py  
    models.py  
    tests.py  
    urls.py  
    views.py  
在urls.py中加入代码：

```
from django.conf.urls import url
from polls.views import index

urlpatterns = [
    url(r'^$',index), # /poll默认指向index
    url(r'^index$',index)
]
```

将 polls.urls 模块指向 root URLconf 修改myweb下的urls.py

```
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^poll/',include('polls.urls')),
]
```

在浏览器中访问[http://127.0.0.1:8000/poll/index](http://127.0.0.1:8000/poll/index) 将看到相同的效果。

