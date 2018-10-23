## 创建应用
```
python manage.py startapp app
```

#### app文件下的文件
models.py -- 模型

view.py -- 业务逻辑

## 创建新表

#### 在models.py文件写添加表的代码
```
class Student(models.Model):
    # 定义s_name字段，varchar类型，最长不超过6个字符，唯一
    s_name = models.CharField(max_length=6, unique=True)
    # 定义s_age字段，int类型
    s_age = models.IntegerField(default=18)
    # 定义s_gender字段，int类型
    s_gender = models.BooleanField(default=1)

    class Meta:
        # 定义模型迁移到数据库中的表名
        db_table = 'student'
```
#### 在setting.py文件下的INSTALLED_APPS中添加'app'
```
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app',
]
```

## 生成迁移文件
```
python manage.py makemigrations
```

## 迁移文件到数据库
```
python manage.py migrate
```

## 在数据库中添加记录
#### 在view.py中导入Studnet对象
```
from app.models import Studnet
```
#### 新建添加学生函数
```
def create_stu(request):
    # 创建学生
    stu = Student()
    stu.s_name = '小李子'
    stu.s_age = 20
    stu.save()
    return HttpResponse('创建成功')
```
#### 在urls.py添加url并执行程序
```
from app import views
urlpatterns = [
    # 127.0.0.1:8080/admin/
    url(r'^admin/', admin.site.urls),
    # 127.0.0.1:8080/create_stu/
    url(r'^create_stu/', views.create_stu)
]
```

## 添加学生
```
Student.objects.create(s_name='小周',s_age='20',s_gender=0)
```

## 实现查询
#### all( )查询所有对象
```
stus = Student.objects.all()    
```
#### filter( )过滤
```
stus = Student.objects.filter(s_name='小李子')
```
#### get( )获取对象
```
stus = Student.objects.get(s_age=20)
```

#### 模糊查询 like '%XXX%' 'X%' '%X'
```
stus = Student.objects.filter(s_name__contains='周')
stus = Student.objects.filter(s_name__startswith='小')
stus = Student.objects.filter(s_name__endswith='瑶')
```

#### 大于 gt/gte  小于lt/lte
```
stus = Student.objects.filter(s_age__gt=18)
```

#### 排序 order_by()
```
# 升序
stus = Student.objects.orderby('id')
# 降序
stus = Student.objects.orderby('-id')
```

#### 查询不满足条件的数据 exclude()
```
stus = Student.objects.exclude(s_age=18)
```

#### 计算统计的个数:count(),len()
```
stus_count = stus.count()
stus_count = len(stus)
```
