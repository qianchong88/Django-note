# Eclipse Django开发环境搭建
## 1 安装eclipse
安装eclipse需要安装配置jdk环境，折腾了好几次因为jdk没有配置好安装不成功
## 2 Eclipse安装pydev插件
### 安装
在线安装pydev，进入eclipse help–>install new software–>Add
Name==>pydev 
Location==>http://pydev.org/updates 
![](/assets/微信截图_20170707173053.png)
![](/assets/QQ截图20170707173237.png)
一路下一步安装完成
### 配置
为eclipse配置Python解析程序，让Python程序能够在eclipse里运行
Window–>Preferences–>PyDev–>interpreters–>Python Interpreter–>Ouick Auto-Config 然后apply。
![](/assets/QQ截图20170707174134.png)
新建项目时出现pydev选项说明安装成功
#3 安装Django
打开命令行输入 pip install django
用的是Python3.6安装的是Django1.11
#4 安装MySQL数据库
安装PHP集成开发环境时已经安装了MySQL不用再安装
#5 安装Python的MySQL驱动
打开命令行输入 pip install mysqldb 安装失败，查资料发现Python3之后的MySQL驱动改为pymysql。
pip install pymysql 安装成功

开发环境搭建成功
