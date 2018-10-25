## 1.模板
#### 标签：
```
{% tag %}

{% endtag %}
for/if/ifequal/extends/block/comment
```
#### 变量：
```
{{var}}
```
#### 父模板base.html：block块，挖坑
```
{% block XXX %}
{% endblock %}
```
#### 子模板index.html
```
{% extends 'base.html' %}
{% block XXX%}
    <p>XXXX</p>
{% endblock %}

{{ block.super}}：将block块之前定义的内容拿过来
```

## 引入css/js
```
方式一
<link href='/static/css/index.css' rel='stylesheep'>

方式二
{% load static %}
<link href="{% static 'css/index.html'%}">

setting.py中添加：
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'static')]
```

## 3.标签
```
{% for i in stus %}
{% endfor %}

{% if XXX == 1 %}
{% endif %}

{% ifequal XXX XXX%}
{% endifequal %}
```

## 4.过滤器
- safe
- date
- lower
- upper
- tille
- add



