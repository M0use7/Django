## 重定向
#### 第一种重定向
```
return HttpResponseRedirect(reverse('/app/all_stu/'))
```
#### 第二种重定向，使用反向解析reverse('namespace:name')
```
return HttpResponseRedirect(reverse('dy:all'))
```

#### html中的url的反向解析
```
<a href="{% url 'dy:edit' stu.id %}">编辑</a>
```

```
访问：

没登录情况：GET http://127.0.0.1:8080/index/ ==> login.html

登录情况：GET http://127.0.0.1:8080/index/ ==> index.html

HTTP无状态协议

解决办法：

    cookie + session
```

## 令牌
#### 1.注册
#### 2.登录成功(给一个标识符)
1）登录向向页面的cookie中添加标识符：
```
set_cookie(key, value, max_age)
```
2)向后端的usertoken表中存入这个标识符和的登录的用户
#### 3.访问任何的路由，先校验你的标识符是否正确，如果正确就放行，如果标识符不正确，则不让访问
```
使用的是闭包（装饰器）

闭包三个条件:
1.外层函数套内层函数
2.内层函数调用外层函数的参数
3.外层函数返回内层函数
```

#### 注销
1)删除页面cookie中的标识符:delete_cookie(key)

2)删除后端usertoken表中标识符对应的哪一条数据