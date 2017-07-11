# 数据库设置

在使用eclipse创建Django项目时明明填写了数据库配置信息，在配置文件中的数据库配置还是默认配置，所以只能手动配置

编辑 myweb/settings.py 。该文件（模块），包含了代表

Django 设置的模块级变量。 更改 DATABASES 中 'default' 中以下键的值，以匹

配数据库连接设置。

ENGINE：从 'django.db.backends.postgresql\_psycopg2',

'django.db.backends.mysql', 'django.db.backends.sqlite3',

'django.db.backends.oracle' 中选一个。

NAME： 数据库名称。如果你使用 SQLite，该数据库将是你计算机上的一个

文件；在这种情况下，NAME 将是一个完整的绝对路径，而且还包含该文件的

名称。如果该文件不存在，它会在第一次同步数据库时自动创建。

当指定路径时，总是使用正斜杠，即使是在 Windows 下\(例

如： C:/homes/user/mysite/sqlite3.db  \) 。

USER：你的数据库用户名 \( SQLite 下不需要\) 。

PASSWORD：你的数据库密码 \( SQLite 下不需要\) 。

HOST ： 你的数据库主机地址。如果和你的数据库服务器是同一台物理机器，

请将此处保留为空 \(或者设置为 127.0.0.1\) \( SQLite 下不需要\) 。

将 TIME\_ZONE 修改为你所在的时区。默认值是美国中央时区（芝加哥）。

配置好后启动Django后报错

import MySQLdb as Database

ModuleNotFoundError: No module named 'MySQLdb'

网上查资料后，由于django支持的是MySQLdb，PyMySQL是作为停更的MySQLdb支持python3.x，因此需要在项目的\_\__init\_\_.py\(myweb/myweb/\_\_init\_\_.py\_\)添加如下设置：

```
import pymysql

pymysql.install_as_MySQLdb()
```

备注

SQLite 是内置在 Python 中的，因此你不需要安装任何东西来支持你的数据库。

如果你使用 PostgreSQL 或者 MySQL，确保你已经创建了一个数据库。 如果你使用 SQLite ，你不需要事先创建任何东西 - 在需要的时候，将会自动创建数据库文件。

同时，注意文件底部的 INSTALLED\_APPS 设置。它保存了当前 Django 实例已激

活的所有 Django 应用。每个应用可以被多个项目使用，而且你可以打包和分发给

其他人在他们的项目中使用。

默认情况下，INSTALLED\_APPS 包含以下应用，这些都是由 Django 提供的：

django.contrib.auth – 身份验证系统。

django.contrib.contenttypes – 内容类型框架。

django.contrib.sessions – session 框架。

django.contrib.sites – 网站管理框架。

django.contrib.messages – 消息框架。

django.contrib.staticfiles – 静态文件管理框架。

这些应用在一般情况下是默认包含的。

所有这些应用中每个应用至少使用一个数据库表，所以在使用它们之前我们需要创

建数据库中的表。要做到这一点，请运行以下命令：

python manage.py syncdb

1.7还是哪个版本开始，`django`移除了`syncdb`命令，同步（迁移）数据库用如下两个命令：

```
python manage.py makemigrations
python manage.py migrate
```



