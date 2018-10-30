## 上传文章
#### 1.setting.py配置文件中配置media文件路径
```
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```
#### 2.在day06/urls.py文件中，将media文件夹解析为静态文件夹
```
# django在debug为True的情况下，就可以访问media文件夹下的内容
urlpatterns += static(MEDIA_URL, document_root=MEDIA_ROOT)
```
#### 3.html页面中表单添加enctype
```
<form action="" method="post" enctype="multipart/form-data">
    <p>标题：<input type="text" name="title"></p>
    <p>描述：<input type="text" name="desc"></p>
    <p>图片：<input type="file" name="img"></p>
    <input type="submit" value="提交">
</form>
```

## 文章分页
#### 1.查询所有文章对象，并进行分页
```
articles = Acticle.objects.all()
```
#### 2. 将所有文章进行分页，每一页最多三条数据
```
paginator = Paginator(articles, 3)
```
#### 3.获取第一页的文章信息
```
arts = paginator.page(1)
```

## 日志
#### 1.setting.py文件的配置
```
# 配置日志
LOGGING = {
    # 必须是1
    'version': 1,
    # 默认为True，禁用日志
    'disable_existing_loggers': False,
    # 定义formatters组件，定义存储日志中的格式
    'formatters':{
        'default': {
            'format': '%(levelno)s %(name)s %(asctime)s'
        }
    },
    # 定义loggers组件，用于接收日志信息
    # 并且将日志信息丢给handlers去处理
    'loggers':{
        '':{
            'handlers': ['console'],
            'level': 'INFO'
        }
    },
    # 定义handlers组件，用户写入日志信息
    'handlers':{
        'console':{
            'level': 'INFO',
            # 定义存储日志的文件
            'filename': '%s/log.txt' % LOG_PATH,
            # 指定写入日志中信息的格式
            'formatter': 'default',
            # 指定日志文件超过5M就自动做备份
            'class': 'logging.handlers.RotatingFileHandler',
            'maxBytes': 5 * 1024 * 1024,
        }
    }
}
```
#### 2.定义日志处理的中间件，进行日志的打印处理
```
class LoggingMiddleware(MiddlewareMixin):
    def process_request(self, request):
        # 记录当前请求访问服务器的事件，请求参数，请求内容...
        request.init_time = time.time()
        request.init_body = request.body
        return None

    def process_response(self, request, response):
        try:
            # 记录返回响应的时间和访问服务器的时间的差，记录返回状态码
            times = time.time() - request.init_time
            # 响应状态码
            code = response.status_code
            # 响应内容
            res_body = response.content
            # 请求内容
            req_body = request.init_body

            # 日志信息
            msg = '%s %s %s %s' % (times, code, res_body, req_body)
            # 写入日志
            logging.info(msg)
        except Exception as e:
            logging.critical('log error, Exception: %s' % e)
        return response
```