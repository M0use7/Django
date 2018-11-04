## 1. 分页
修改settings.py配置文件，增加分页的配置信息
![image](4CFB7405D79F4E2BA78A60E6F70B9056)
结果
![image](446BDA56B939415BBBAE0444D247E651)
注意：在结果在data对应的value值中，有一个count的key，表示返回数据有3条，next表示下一个的url，previous表示上一页的url。
## 2. 过滤
修改settings.py配置文件，增加filter过滤的信息
#### 2.1 安装过滤的库
```
pip install django-filter
```
#### 2.2 配置settings.py的信息
配置DEFAULT_FILTER_BACKENDS
![image](8AF433496C2A4878B62B289E99BBA80B)
#### 2.3 views中指定filter_class
![image](424570AECD17455B85782FE4CB36D611)
#### 2.4 编写filter_class过滤信息
![image](B83B34AD1B8241BE8D44D5E9496FF491)
#### 2.5 实现方法
###### 2.5.1 查询学生的姓名中包含王的学生
使用filter_class进行过滤筛选：
```
http://127.0.0.1:8080/stu/student/?name=王
```
不使用filter_class进行筛选：

![image](3EC984A3925D4EA58F9FD9C69E6C8C0D)
###### 2.5.2 查询学生的创建时间在2018年1月1号到2018年6月1号的学生信息
使用filter_class进行过滤筛选：
```
http://127.0.0.1:8080/stu/student/?create_min=2018-02-01&create_max_max=2018-0-01
```
不使用filter_class进行筛选：

![image](395F9F364DE44F9A8C2578C81AEB9EA8)