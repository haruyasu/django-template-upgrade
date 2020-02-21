# 下書き機能を追加

今までは投稿するとすぐに公開されましたが、下書きに保存することができます。

blog/views.pyのpost_new関数とpost_edit関数にあるpost.published_dateを削除します。

blog/views.py
```python:blog/views.py
post.published_date = timezone.now()
```

## Draftボタンを追加

blog/templates/blog/base.html
```html:blog/templates/blog/base.html
<li class="nav-item">
  <a class="nav-link" href="{% url 'post_draft_list' %}">Draft</a>
</li>
```

## urlを追加

blog/urls.py
```python:blog/urls.py
path('drafts/', views.post_draft_list, name='post_draft_list'),
```

## 下書き機能をViewに追加

blog/views.py
```python:blog/views.py
def post_draft_list(request):
  posts = Post.objects.filter(
      published_date__isnull=True).order_by('created_date')
  return render(request, 'blog/post_draft_list.html', {'posts': posts})
```

## 下書きテンプレートを追加

post_draft_list.htmlファイルを追加し、テンプレートを作成します。

blog/templates/blog/post_draft_list.html
```html:blog/templates/blog/post_draft_list.html
{% extends 'blog/base.html' %}

{% block header %}
<div class="site-heading">
  <h1>Draft</h1>
</div>
{% endblock %}

{% block content %}
  {% for post in posts %}
  <div class="post-preview">
    <a href="{% url 'post_detail' pk=post.pk %}">
      <h2 class="post-title">
        {{ post.title }}
      </h2>
    </a>
    <p class="post-meta">{{ post.created_date }}</p>
    <p>{{ post.text|truncatechars:200 }}</p>
  </div>
  <hr>
  {% endfor %}
{% endblock %}
```

draftsページを開くと下書きが表示されます。

http://127.0.0.1:8000/drafts/
