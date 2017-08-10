# 启用管理后台
为了提高开发效率，Django自带了后台管理模块。Django1.11默认情况下 Django 管理后台已经开启创建超级管理员账户就可以登录。如果默认没有启用的话，启用管理后台，需要做三件事：
- 在 INSTALLED_APPS 设置中取消 "django.contrib.admin" 的注释。
- 运行 python manage.py syncdb 命令。既然你添加了新应用到INSTALLED_APPS 中，数据库表就需要更新。
- 编辑你的 mysite/urls.py 文件并且将有关管理的行取消注释 
## 创建超级管理员
切换到项目目录下，输入如下命令
```
E:\workspace\myweb\src\myweb>python manage.py createsuperuser
Username (leave blank to use 'administrator'): admin
Email address: admin@admin.com
Password:
Password (again):
This password is too short. It must contain at least 8 characters.
This password is too common.
Password:
Password (again):
Superuser created successfully.
````
超级用户创建成功后数据库里可以看到增加了一条记录

![](/assets/12.png)

## 登录管理后台
开启开发服务器，打开一个浏览器并在本地域名上访问 “/admin/” – 例如
http://127.0.0.1:8000/admin/将看到管理员的登录界面：

![](/assets/am.png)
输入刚刚创建的超级用户账号密码，将看到管理界面：

![](/assets/da.png)
在后台group和user表进行增删改查的操作

## 使 poll 应用的数据在管理网站中可编辑
要在管理后台中可以对Poll应用的数据进行编辑需要将数据类注册到admin中
在polls 目录下的admin.py文件中添加如下内容：
```

from django.contrib import admin
from polls.models import Poll
admin.site.register(Poll)
```
重启开发服务器将会看到管理后台多了刚刚注册的Poll。

![](/assets/pl.png)

## 忘记管理员密码

在项目目录下运行：Python manage.py shell



```
>>> from django.contrib.auth.models import User
>>> u = User.objects.get(username__exact='john')
>>> u.set_password('new password')
>>> u.save()
```



密码就被设置成了new password