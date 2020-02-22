# アプリケーション拡張

投稿の詳細ページを作成します。

### 詳細へのリンクを作成

post_list.htmlを変更しましょう。

タイトルのリンクを設定します。

blog/templates/blog/post_list.html
```html:blog/templates/blog/post_list.html
<a href="{% url 'post_detail' pk=post.pk %}">
  <h2 class="post-title">
    {{ post.title }}
  </h2>
</a>
```

### 投稿の詳細へのURLを作成

URLのパターンを指定します。

blog/urls.py
```python:blog/urls.py
urlpatterns = [
    path('', views.post_list, name='post_list'),
    path('post/<int:pk>/', views.post_detail, name='post_detail'),
]
```

### 詳細のViewを追加

view.pyにpost_detail関数を追加します。

blog/views.py
```python:blog/views.py
from django.shortcuts import render, get_object_or_404

def post_detail(request, pk):
    post = get_object_or_404(Post, pk=pk)
    return render(request, 'blog/post_detail.html', {'post': post})
```

### 詳細のテンプレートを追加

post_detail.htmlファイルを追加します。

blog/templates/blog/post_detail.html
```html:blog/templates/blog/post_detail.html
{% extends 'blog/base.html' %}

{% block header %}
<div class="post-heading">
  <h1>{{ post.title }}</h1>
  {% if post.published_date %}
    <span class="meta">{{ post.published_date }}</span>
  {% endif %}
</div>
{% endblock %}

{% block content %}
<p>
  {{ post.text|linebreaksbr }}
</p>
{% endblock %}
```

投稿をクリックすると、詳細画面が表示されました。
