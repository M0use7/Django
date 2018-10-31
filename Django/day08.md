## 权限

表：用户表、权限表、角色表

思想：
1. 创建角色
2. 角色对应权限
3. 用户分配角色
4. （特殊情况）用户分配权限

```
用户表和权限表的ManyToManyFiled()为：user_permissions
用户表和组表的ManyToManyFiled()为：groups
组表和权限表的ManyToManyFiled()为：permissions

添加与删除，add()、remove()
```

## 查询
#### 1.通过用户查询权限
- 自己实现查询用户对应的权限方法：
```
user = MyUser.objects.filter(username='admin').first()
user.user_permissions.all()
user.groups.all()[0].permissions.all()
```
- django自带查询方法
```
user.get_group_permissions()
user.get_all_permissions()
```
#### 2.权限验证
- 自己实现权限验证
```
user.user_permissions.filter(codename='XXX')
user.groups.all()[0].permissions.filter(codename='XXX')	
```
- django实现权限验证
```
user.has_perm('名称.权限名')
```
#### 3.装饰器
- 自己实现
```
def a(func)
		def b(request):
			return func(request)
		return b
```
- django实现
```
@permission_required('名称.权限名')
```

## 操作
#### 1，创建权限
自定义权限
```
from django.contrib.auth.models import AbstractUser
from django.db import models


class MyUser(AbstractUser):
    # 扩展django自带的auth_user表，可以自定义新增的字段

    class Meta:
        # django默认给每个模型初始化三个权限
        # 默认的是change、delete、add权限
        permissions = (
            ('add_my_user', '新增用户权限'),
            ('change_my_user_username', '修改用户名权限'),
            ('change_my_user_password', '修改用户密码权限'),
            ('all_my_user', '查看用户权限')
        )

```
并在settings.py文件中添加如下设置：
```
# 告诉django, User模型修改为自定义的User模型
AUTH_USER_MODEL = 'user.MyUser'
```
#### 2.分配权限
###### 1）给用户直接添加某种权限
```
def add_user_permission(request):
    if request.method == 'GET':
        # 1.创建用户
        user = MyUser.objects.create_user(username='admin',
                                          password='123456')
        # 2.指定刚创建的用户，并分配给它权限（新增用户权限，查看用户权限）
        permissions = Permission.objects.filter(codename__in=['add_my_user', 'all_my_user']).all()
        for permission in permissions:
            # 多对多的添加
            user.user_permissions.add(permission)
        # 3.删除刚创建的用户的新增用户权限
        # user.user_permissions.remove(对象)
        return HttpResponse('创建用户权限成功')
```
###### 2）创建组并分配对应组的权限
```
def add_group_permission(request):
    if request.method == 'GET':
        # 创建超级管理员(所有权限)、创建普通管理员(修改/查看权限)
        group = Group.objects.create(name='审核组')

        ps = Permission.objects.filter(codename__in=['change_my_user_username',
                                                     'change_my_user_password',
                                                     'all_my_user']).all()
        # 给组添加权限
        for permission in ps:
            group.permissions.add(permission)

        return HttpResponse('创建组权限成功')
```
###### 3）分配用户和权限组
```
def add_user_group(request):
    if request.method == 'GET':
        # 给admin用户分配审查组
        user = MyUser.objects.filter(username='admin').first()
        group = Group.objects.filter(name='审核组').first()

        # 分配组
        user.groups.add(group)
        return HttpResponse('用户分配组成功')
```
#### 3.检测用户是否有某权限,和所有权限，组权限
```
def my_index(request):
    if request.method == 'GET':
        # 当前登录系统用户
        user = request.user
        # 获取组权限
        user.get_group_permission()
        # 获取当前用户的所有权限
        user.get_all_permission()
        # 判断是否有某个权限
        user.has_perm('应用app名.权限名')
        return render(request, 'my_index.html')
```
##### 4. 权限校验，使用permission_required装饰器
```
@permission_required('user.xxx')
def new_index(request):
    if request.method == 'GET':
        return HttpResponse('需要权限才能查看')
```
并在settings.py文件中添加如下设置：
```
# 登录验证/权限验证不通过，则跳转地址
LOGIN_URL = '/user/login'
```