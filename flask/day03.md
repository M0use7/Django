## ORM===>CRUD
#### 1)增数据
```
db.session.add(对象)
db.session.add_all([对象1,对象2...])
db.session.commit()
```

#### 2)删数据
```
db.session.delete(对象)
db.session.commit()
```

#### 3)修改
```
模型.query.filter(xxx).update({'key':value, 'key2':value2})
```

#### 4)查询
```
filter(模型名.字段 == '值')
filter_by(字段 = '值')
all():获取查询的值，为列表
first():获取第一个查询值
get():获取主键所在行的对象信息
order_by():排序，升序'id'、'-id'
paginate():用于分页，paginate(页码，每一页数据条数)
```
a)模糊查询
```
contains、like、startswith、endswith
```
b)大小于
```
__lt__,__le__,__gt__,__ge__
```
c)筛选
```
offset()、limit()
```
d)范围之内
```
in_
```
e)与或非
```
not_、or_、and_
```

## 一对多（1:N）
```
class Student(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    s_name = db.Column(db.String(10), unique=True, nullable=True)
    s_gender = db.Column(db.Boolean, default=1)
    grade_id = db.Column(db.Integer, db.ForeignKey('grade.id'), nullable=True)

class Grade(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    g_name = db.Column(db.String(10), nullable=True, unique=True)
    student = db.relationship('Student', backref='g')
```