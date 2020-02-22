# 公開機能の作成

下書きのままでは公開されないので、公開機能を追加します。

## 公開ボタンを作成

Editボタンの上にPublishボタンを追加します。

blog/templates/blog/post_detail.html
```html:blog/templates/blog/post_detail.html
{% if not post.published_date %}
  <a class="btn btn-warning" href="{% url 'post_publish' pk=post.pk %}" role="button">Publish</a>
{% endif %}
```

## 公開ボタンのurlを作成

blog/urls.py
```python:blog/urls.py
path('post/<pk>/publish/', views.post_publish, name='post_publish'),
```

## 公開ボタンのViewを作成

blog/views.py
```python:blog/views.py
def post_publish(request, pk):
  post = get_object_or_404(Post, pk=pk)
  post.publish()
  return redirect('post_detail', pk=pk)
```

下書きでPublishボタンをクリックすると、詳細ページにリダイレクトされます。

これで無事公開ができました。

トップページにも投稿内容が表示されているはずです。