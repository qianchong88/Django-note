# 玩转API

进入Python的交互式shell 体验一下Django提供的database API。在项目目录下执行如下命令

```
python manage.py shell
```

![](/assets/a1.png)

一旦进入了shell，就可通过 database API 来浏览数据：  
输入以下命令，引入我们写的model类

```
from polls.models import Poll, Choice
```

从Poll中查询所有Poll

```
#数据库中没有记录
>>>Poll.objects.all()
<QuerySet []>
```

创建一个Poll

```
# 因此 Django 希望为 pub_date 字段获取一个 datetime with tzinfo 。
使用了 timezone.now()
# 而不是 datetime.datetime.now() 以便获取正确的值。
>>> from django.utils import timezone
>>> p = Poll(question="What's new?", pub_date=timezone.now())
# 保存对象到数据库中。
>>> p.save()
# 现在对象拥有了一个ID 。请注意这可能会显示 "1L" 而不是 "1"，取决于
# 你正在使用的数据库。 这没什么大不了的，它只是意味着你的数据库后端
# 喜欢返回的整型数作为 Python 的长整型对象而已。
>>> p.id
1
# 通过 Python 属性访问数据库中的列。
>>> p.question
"What's new?"
>>> p.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)
# 通过改为属性值来改变值，然后调用 save() 方法。
>>> p.question = "What's up?"
>>> p.save()
# objects.all() 用以显示数据库中所有的 polls 。
>>> Poll.objects.all()
<QuerySet [<Poll: Poll object>]>
```

此时数据库里Poll表中会多出一条记录。不过\[&lt; Poll: Poll object&gt;\]这样显示对象毫无意义，按照手册修改Model给Poll，Choise分别添加unicode\(\)方法如下所示

```
class Poll(models.Model):
# ...
def __unicode__(self):
    return self.question

class Choice(models.Model):
# ...
def __unicode__(self):
    return self.choice_text
```

再次查询所有Poll

```
>>> Poll.objects.all()
<QuerySet [<Poll: Poll object>]>
```

发现并没有起作用，因为我用的是python3，**unicode**\(\)是Python2中的，所以需要改用**str**,如下所示

```
from django.db import models


class Poll(models.Model):
    question = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
    def __str__(self):
        return self.question
class Choice(models.Model):
    poll = models.ForeignKey(Poll)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
    def __str__(self):
        return self.choice_text
```

重新打开命令行再一次查询所有的Poll,发现已经起作用了

```
>>> from polls.models import Poll,Choice
>>> Poll.objects.all()
<QuerySet [<Poll: what's old?>]>
```

很少会一次性从数据库中取出所有的数据；通常都只针对一部分数据进行操作。 在Django API中，条件选取querySet的时候，filter表示=，exclude表示!=。：

```
>>> Poll.objects.filter(id=1)
<QuerySet [<Poll: what's old?>]>
#相当于sql语句select * from poll where id =1
```

传递多个参数到filter\(\)来缩小选取范围,多个参数会被转换成 AND SQL从句：

```
>>> Poll.objects.filter(id=1,question="what's old?")
<QuerySet [<Poll: what's old?>]>
#相当于sql语句select * from poll where id =1 and question="what's old?"
```

SQL缺省的 = 操作符是精确匹配的，可以使用关键字实现其它的条件匹配 \_\_contains包含like '%xxx%'

```
>>> Poll.objects.filter(question__contains="ol")
<QuerySet [<Poll: what's old?>]>
#相当于sql语句select * from poll where question like "%ol%"
```

\_\_exac精确等于like 'xxx'

```
>>> Poll.objects.filter(question__exact="ol")
<QuerySet []>
>>> Poll.objects.filter(question__exact="what's old?")
<QuerySet [<Poll: what's old?>]>
#select * from poll where question="what's old?" 或 select * from poll where question like "what's old?"
```

__iexact    精确等于忽略大小写

__gt 大于 

__gte大于等于
  
__lt    小于  

__lte    小于等于 
 
__in     存在于一个list范围内 
 
__startswith   以...开头  

__istartswith   以...开头 忽略大小写
  
__endswith     以...结尾  

__iendswith    以...结尾，忽略大小写
  
__range    在...范围内  

__year       日期字段的年份
  
__month    日期字段的月份 
 
__day        日期字段的日 
 
__isnull=True/False

从数据库中返回一条匹配的结果使用get()方法
```
>>> Poll.objects.get(id=1)
<Poll: what's old?>
#返回的是一个Poll对象而不是queryset
```

