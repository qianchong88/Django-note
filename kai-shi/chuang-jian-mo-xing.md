# 创建模型

项目环境搭建好了可以开工了。  
Django规定，如果要使用模型，必须要创建一个应用。通过 Djaong 编写的每个应用都是由Python包组成的，这些包存放在你的Python path 中并且遵循一定的命名规范。 Django 提供了个实用工具可以自动生成一个应用的基本目录架构，因此你可以专注于编写代码而不是去创建目录。

## 项目 \( Projects \) vs 应用 \( apps \)

项目与应用之间有什么不同之处？  
应用是一个提供功能的 Web 模块– 例如：一个博客系统、一个公共记录的数据库者一个简单的投票系统。 项目是针对一个特定的 Web 网站相关的配置和其应用的组合。一个项目可以包含多个应用。一个应用可以在多个项目中使用。应用可以存放在 Python path 中的任何位置。

## 创建投票应用

### 通过manage.py工具创建投票应用

要创建新的应用，需要在项目的 manage.py 文件在同一的目录下并输入以下命令：  
python manage.py startapp polls  
将创建一个polls目录，其展开的样子如下所示：  
.  
├── manage.py  
├── myweb  
│   ├── **init**.py  
│   ├── settings.py  
│   ├── urls.py  
│   └── wsgi.py  
└── polls  
    ├── admin.py  
    ├── apps.py  
    ├── **init**.py  
    ├── migrations  
    │   └── **init**.py  
    ├── models.py  
    ├── tests.py  
    └── views.py

### 通过djago-admin管理工具创建投票应用

在项目目录下输入一些命令  
django-admin startapp polls  
这也将创建一个polls目录，结构同上。  
各目录含义如下：

* polls：app的根目录

* admin.py：Django自带了一个管理后台界面，这个文件可以注册model在后台界面中管理

* **init**.py：表明polls也是一个包

* migrations：用来初始化数据库，在执行python manage.py makemigrations 的时候会自动生成一个文件在这里

* **init**.py：表明migrations也是一个包

* models.py：在这个文件里面定义model类

* tests.py：写测试代码

* views.py：视图，Django映射urls.py里面的url的时候，在views.py里面查找对应的处理方法

## 创建Poll和choise模型

Poll 有问题和发布日期两个字段，Choice有选项\( choice \)的文本内容和投票数，每一个 Choice 都与一个 Poll 关联。

Poll表：

| 字段名 | 描述 | 字段类型 |
| :--- | :--- | :--- |
| id | 逻辑id | int |
| question | 问题 | varchar |
| pub\_date | 发布时间 | timedate |

Choise表：

| 字段名 | 描述 | 字段类型 |
| :--- | :--- | :--- |
| id | 逻辑id | int |
| choice\_text | 选项的文本内容 | varchar |
| votes | 选项的投票数 | int |
| poll\_id | 外键关联的poll中对应 id | int |

这些概念都由简单的 Python 类来表现,每个类对应一张数据库中的表。编辑 polls/models.py 文件后如下所示：

```
from django.db import models

class Poll(models.Model):
    question = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')
class Choice(models.Model):
    poll = models.ForeignKey(Poll)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

每个字段由一个 Field 的实例来表现 – 比如 CharField 表示字符类型的字段和DateTimeField 表示日期时间型的字段。这会告诉 Django 每个字段都保存了什么类型的数据。每一个 Field 实例的名字就是字段的名字（如： question 或者 pub\_date ），其格式属于亲和机器式的。在 Python 的代码中会使用这个值，而数据库会将这个值作为表的列名。可以在初始化 Field 实例时使用第一个位置的可选参数来指定人类可读的名字。这在Django的内省部分中被使用到了，而且兼作文档的一部分来增强代码的可读性。若字段未提供该参数，Django 将使用符合机器习惯的名字。在本例中，我们仅定义了一个符合人类习惯的字段名 Poll.pub\_date 。对模型中的其他字段，机器名称就已经足够替代人类名称了。

一些 Field 实例是需要参数的。例如 CharField 需要你指定~django.db.models.CharField.max\_length  。这不仅适用于数据库结构，以后我们还会用于数据验证中。一个 Field 实例可以有不同的可选参数； 在本例中，我们将 votes 的 default 的值设为 0 。

最后，注意我们使用了 ForeignKey 定义了一个关联。它告诉 Django 每一个 Choice  关联一个 Poll 。 Django 支持常见数据库的所有关联：多对一（many-to-ones ），多对多（ many-to-manys ） 和 一对一 （ one-to-ones ）。

## 激活模型

再次编辑 settings.py 文件，在 INSTALLED\_APPS 设置中加入 'polls' 字符。因此结果如下所示：

```
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls'
]
```

同步到数据库，在项目目录下执行如下命令

```
python manage.py makemigrations
python manage.py migrate
```

或者在eclipse下 右键-&gt;django-&gt;make migrate.再右键-&gt;django-&gt; migrate

执行成功后会myweb数据库中会创建几张表出来其中就有我们刚刚创建的模型对应的表



![](/assets/123.png)

到此模型创建成功

