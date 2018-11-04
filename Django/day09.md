## HTTP请求方式：
```	
	GET：获取数据
	POST：创建
	PUT：用于修改数据（修改全部属性）
	PATCH：用于修改数据（修改部分属性）
	DELETE：删除数据
```

## django中使用restful
```
pip install djangorestframework==3.4.6
pip install django-filter
```

## settings.py配置的修改
```
INSTALLED_APPS = [
	...

    'rest_framework',
]
```

## urls.py文件中
#### 获取路由对象
```
router = SimpleRouter()
```

#### 使用router注册的地址
```
127.0.01:8080/app/article/
router.register('article', views.ArticleView)
```

#### 设置访问路由地址
```
urlpatterns += router.urls
```

## 在视图views.py文件中定义StudentView类
```
class ArticleView(viewsets.GenericViewSet,
                  mixins.ListModelMixin,
                  mixins.DestroyModelMixin):


    # 查询数据
    queryset = Article.objects.all()
    # 序列化
    serializer_class = ArticleSerializer
```


## 定义序列化类
```
class ArticleSerializer(serializers.ModelSerializer):

    class Meta:
        # 指定序列化的模型
        model = Article
        # 序列化字段
        fields = ['id', 'title', 'desc']
```

## 常规
```
$.ajax({
	url: '127.0.0.1:8080/app/article/',
	type: 'POST/DELETE...',
	data: {'title': 'admin', 'desc': '123456'},
	dataType : 'json',
	success: function(msg){
		
	},
	error: function(data){

	}		
})
```

## 简略版
```
$.get(url,function(data){

})
$.post(url, data, function(data){

})
```







