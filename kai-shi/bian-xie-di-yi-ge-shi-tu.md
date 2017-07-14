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

url\(\) 函数有四个参数，两个必须的： regex 和  view  ， 两个可选的：  
kwargs  ， 以及  name  。

url\(\) 参数: regex

regex 是 regular expression 的简写，这是字符串中的模式匹配的一种语法， 在Django 中就是是 url 匹配模式。Django 将请求的 URL 从上至下依次匹配列表中的正则表达式，直到匹配到一个为止。需要注意的是，这些正则表达式不会匹配 GET和 POST 参数，以及域名。 例如：针对 [http://www.example.com/myapp/](http://www.example.com/myapp/) 这一请求，URLconf 将只查找myapp/ 。而在[http://www.example.com/myapp/?page=3](http://www.example.com/myapp/?page=3) 中 URLconf 也仅查找myapp/ 。

**这些正则表达式在 URLconf 模块第一次加载时会被编  
译。 因此它们速度超快**

url\(\) 参数： view

当 Django 匹配了一个正则表达式就会调用指定的视图功能，包含一个HttpRequest 实例作为第一个参数和正则表达式 “捕获” 的一些值的作为其他参数。

url\(\) 参数： kwargs

任意关键字参数可传一个字典至目标视图。

Django 这一特性。

url\(\) 参数： name  
命名你的 URL ，让你在 Django 的其他地方明确地引用它，特别是在模板中。 这一强大的功能可允许你通过一个文件就可全局修改项目中的 URL 模式。

