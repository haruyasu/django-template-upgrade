# URL追加

各URLを追加するには、urls.pyを編集します。

今回は、トップページのURLを作成します。

トップページにアクセスした時には、blog.urlsへリダイレクトするよにします。

mysite/urls.py
```python:mysite/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')), #追加
]
```

## blogのURL追加

urls.pyファイルを作成します。

blog/urls.py
```python:blog/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```
