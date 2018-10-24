## MVC模型和django的MVT模型
MVC
- M(模型层)
- V(视图层)
- C(业务层)

Django ==> MVT
- M(模型层)：负责业务与数据库(ORM)的对象
- V(视图):处理业务逻辑
- T(模板Temlate):html

## VIRTUALENV虚拟环境搭建指南
#### 1.安装virtualenv
```
pip install virtualenv
```
#### 2.创建虚拟环境
```
纯净的虚拟环境：virtualenv --no-site-package
djenv6

多个pythton版本,python2.7 python3.6:
virtualenv --no-site-package -p D:\XXX\python3.6.exe name djenv6

python版本3.6 + django 1.11
```
#### 3.进入/退出env

```
进入 cd env\djenv6\Scripts 再activate命令
退出  deactivate
```
#### 4.pip使用
查看虚拟环境下安装的所有的包
```
pip list
```
查看虚拟环境通过pip安装的包
```
 pip freeze
```

## 创建Django项目
#### 1.创建一个运行Django项目的虚拟环境
```
pip install Django==1.11
pip install PyMySQL
```
#### 2.创建一个Django项目
```
django-admin startproject 项目名
```
项目目录
- manage.py:是Django用于管理本项目的管理集工具
- init.py:指明该目录结构是一个python包
- setting.py:Django项目的配置文件，其中定义了本项目的引用组件，项目名，数据库，静态资源，调试模式，域名限制等
- url.py:项目的URL路由映射
- wsgi.py:定义WSGI接口信息

#### 3.运行Django项目
```
IP:0.0.0.0    PORT: 80
python manage.py runserver IP:端口
```
#### 4.访问管理后台 admin

```
http://127.0.0.1:8080/admin
```
#### 5.修改数据库配置
    NEGINE,USER,PASSWORD,HOST,PORT,NAME
#### 6.映射模型到数据库中
    python manage.py migrate
#### 7.初始化数据库驱动__init__.py
    import pymysql
    pymysql.install_as_mysqldb()
#### 8.创建超级管理员命令
    python manage.py createsuperuser
​    