## flask中session的两种存储方式
1）使用flask默认的方式存储session，其实就是将seesion中的的数据保存在客户端的cookie中

2)在服务端中保存session数据，使用flask-session库，配置使用redis进行数据存储

## 模板
1)继承,block块

2)继承block中已填有的内容{{ super() }}

3)标签 {% 标签 %} if,for,block,url_for

4)变量 {{ 变量 }},loop
```
loop.index
loop.index0
loop.revindex
loop.revindex0
loop.first
loop.last
```
5)过滤器 upper lower safe length

## 模型层
```
使用flask-sqlalchemy库
	class x(db.Model):
		id = db.Column(.....)
	

    unique,nullable,default
    表名:__tablename__ == '表名'
    数据库连接格式:mysql+pymysql://
    root:123456@127.0.0.1:3306/db
    
    创建:db.create_all()
    删除:db.drop_all()
```