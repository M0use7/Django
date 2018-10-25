## 1.模型定义
- AutoField
- CharField
- IntegerField
- BooleanField
- DateTimeField
- DataField

## 2.约束
- primary_key
- unique
- default
- auto_now：用于记录每一次修改时间
- auto_now_add:用于记录第一次的时间

## 3.定义表名
```
class Meta:
		db_table = 'student'

	如果指定db_table参数，则在迁移后的数据库中表名为student
	如果没有指定db_table参数，在数据库中表名为'应用app名称_模型'
```

## 4.迁移
#### 生成迁移文件
```
python manage.py makemigrations
```
#### 执行迁移
```
python manage.py migrate
```

## 5.创建的CRUD
#### 1)创建：模型名.objects.create()
#### 2)查询
**查询所有：**
```
模型名.objects.all()
```

**查询满足条件：**
```
模型名.objects.filter(条件1).filter(条件2)
模型名.objects.filter(条件1,条件2)
```
**查询第一个，最后一个**：first(),last()

**获取确定的某一个：**
```
模型名.objects.get(条件)
注意:条件不满足获取不到数据,则报错满足条件的数据多于一个对象,则报错
```
**排除满足条件的数据：**
```
模型名.objects.exclude(条件)
```
**返回的结果序列化：**
```
模型名.objects.values(字段1,字段2...)
```
**排序:**
```
升序:模型名.objects.order_by('id')
降序:模型名.objects.order_by('-id')
```
**查询过滤条件:**
```
字段__contains
contains,startswith,endswith,gt,gte,lt,lte,
```
#### 3)删除
```
模型名.objects.filter(条件).delete()
```
#### 4)修改
```
方式一
模型名.objects.filter(条件).update(字段1=xxx,..)

方式二
stu = Student.objects.filter(条件).first
stu.s_name = '...'
stu.save()
```

## 6.关联关系
#### 1)一对一：OnoToOneField
```
    class A:
		id = xxx
		b = OnoToOneField(B)
	class B:
		id = xxx
	已知a对象,查找b对象:a.b
	已知b对象,查找a对象:b.a
```
#### 2)一对多：ForeignKey
```
    class A:
		id = xxx
		b = ForeignKey(B)

	class B:
		id = xxxx

	已知a对象,查找b对象:a.b
	已知b对象,查找a对象:b.a_set
```