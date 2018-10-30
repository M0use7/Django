## 定义模板
- 定义父模板base.html
- 定义子模板注册页面register.html
- 定义子模板登录页面login.html
- 定义首页index.html

## 定义路由
```
from django.conf.urls import url
from django.contrib.auth.decorators import login_required

# 在hello/hello/urls.py配置如下路由
url(r'user/', include('users.urls', namespace='user')),

# 在hello/users/urls.py配置如下路由
# 定义注册URL
path(r'^register/', views.register, name='register'),
# 定义登录URL
path(r'^login/', views.login, name=login),
# 定义注销URL
path(r'^logout/', login_required(views.logout), name=logout),
# 首页
url(r'^index/', login_required(views.index), name='index'),
```

## 表单验证
```
from django import forms
from django.contrib.auth.models import User


class UserRegisterForm(forms.Form):
    """
    校验注册信息
    """
    username = forms.CharField(required=True, max_length=5, min_length=2,
                               error_messages={
                                   'required': '用户名必填',
                                   'max_length': '用户名不能超过5个字符',
                                   'min_length': '用户名不能少于2个字符'
                               })
    password = forms.CharField(required=True, min_length=6,
                               error_messages={
                                   'required': '密码必填',
                                   'min_length': '密码不能少于6个字符'
                               })
    password2 = forms.CharField(required=True, min_length=6,
                                error_messages={
                                    'required': '确认密码必填',
                                    'min_length': '确认密码不能少于6个字符'
                                })

    def clean(self):
        # 校验用户名是否已经注册过
        user = User.objects.filter(username=self.cleaned_data.get('username'))
        if user:
            # 如果已经注册过
            raise forms.ValidationError({'username': '用户名已存在，请直接登录'})
        # 校验密码和确认密码是否相同
        if self.cleaned_data.get('password') != self.cleaned_data.get('password2'):
            raise forms.ValidationError({'password': '两次密码不一致'})
        return self.cleaned_data
```
## 定义视图函数
#### 1.定义注册方法
```
def register(request):
	"""
	定义注册方法
	"""
    if request.method == 'GET':
        return render(request, 'register.html')

    if request.method == 'POST':
        # 校验页面中传递的参数，是否填写完整
        form = UserRegisterForm(request.POST)
        # is_valid():判断表单是否验证通过
        if form.is_valid():
            # 获取校验后的用户名和密码
            username = form.cleaned_data.get('username')
            password = form.cleaned_data.get('password')
            # 创建普通用户create_user，创建超级管理员用户create_superuser
            User.objects.create_user(username=username, password=password)
            # 实现跳转
            return HttpResponseRedirect(reverse('user:login'))
        else:
            return render(request, 'register.html', {'form': form})
        
        # 验证不通过
        else:
            return render(request, 'register.html', {'errors': form.errors})
            # form.errors = {'username': ['长度不能小于2个字符'], 'password': ['长度不能大于10个字符', '密码不一致']}
```

#### 2.定义登录方法
```
def login(request):
    if request.method == 'GET':
        return render(request, 'login.html')
    if request.method == 'POST':
        # 表单验证，用户名和密码是否填写，校验用户名是否注册
        form = UserLoginForm(request.POST)
        if form.is_valid():

            # 校验用户名和密码，判断返回的对象是否为空，如果不为空，则为user对象
            user = auth.authenticate(username = form.cleaned_data['username'],
                                     password = form.cleaned_data['password'])
            if user:
                # 用户名和密码是正确的,则登录
                auth.login(request, user)
                return HttpResponseRedirect(reverse('user:index'))
            else:
                # 密码不正确
                return render(request, 'login.html', {'error': '密码错误'})
        else:
            return render(request, 'login.html', {'form': form})
        
```
#### 3.定义首页方法
```
def index(request):
    if request.method == 'GET':
        return render(request, 'index.html')
```

#### 4.定义注销方法
```
def logout(request):
    if request.method == 'GET':
        # 注销
        auth.logout(request)
        return HttpResponseRedirect(reverse('user:login'))
```
