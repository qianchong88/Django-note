# 编写第一个Django程序

## 1 创建Django项目

### 1.1 使用django-admin.py管理工具创建

进入到想存放代码的目录，打开命令行工具\(Windows下shift+右键\),执行命令

```
django-admin.py startproject myweb
```

这将在当前目录创建一个 myweb 目录,目录结构如下  
myweb/  
   manage.py  
   myweb/  
     **init**.py  
     settings.py  
     urls.py  
     wsgi.py  
各文件的作用：

* 外层 myweb/ 目录只是你项目的一个容器。对于 Django 来说该目录名并不重
  要, 你可以重命名为你喜欢的。
* manage.py: Django为项目提供的命令行工具，可以通过命令行的方式对项目进行管理
* 内层 myweb/ 目录是目的实际Python包。该目录名就是Python包名，通过它你可以导入它里面的任何东西。 \(e.g. import mysite.settings\).
* mysite/init.py: 一个空文件，告诉 Python 该目录是一个 Python 包。
* mysite/settings.py: 该 Django 项目的配置文件。
* mysite/urls.py: 该 Django 项目的 URL 声明\(路由规则定义\);
* mysite/wsgi.py: 一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。

### 1.2 使用eclipse集成开发环境创建Django项目

file-&gt;new-project

  
![](/assets/1.png)

下一步



![](/assets/2.png)

填写项目名称，选择所使用的Python版本，创建src目录并添加到PYTHONPATH变了中。  
下一步：选择关联项目，没有要关联的项目，直接下一步  
数据库配置，用的是MySQL，（项目所使用的库myweb需要事先创建）

  
![](/assets/3.png)



点击finish第一个Django项目创建成功  
![](/assets/4.png)

# 2 开发服务器

你已经启动了 Django 开发服务器，一个纯粹的由 Python 编写的轻量级 Web 服务器。我们在 Django 内包含了这个服务器，这样你就可以迅速开发了，在产品投入使用之前不必去配置一台生产环境下的服务器。

## 2.1开启开发服务器

### 命令行下

从外层 myweb 目录切换进去，运行命令  
python manage.py runserver  。  
你将会看到命令行输出如下内容：  
Performing system checks...  
0 errors found  
...  
浏览器中输入[http://127.0.0.1:8000/](http://127.0.0.1:8000/)  
![](/assets/5.png)

### 在eclipse中运行

项目目录上右键-&gt;Run as-&gt;pydev:djano

  
![](/assets/6.png)

第一个Django项目创建成功。

