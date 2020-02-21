# 公開ボタンを追加

blog/templates/blog/post_detail.html
```html:blog/templates/blog/post_detail.html
{% if not post.published_date %}
  <a class="btn btn-warning" href="{% url 'post_publish' pk=post.pk %}" role="button">Publish</a>
{% endif %}
```

## urls.pyにurlを追加

blog/urls.py
```python:blog/urls.py
path('post/<pk>/publish/', views.post_publish, name='post_publish'),
```

## Viewを追加

blog/views.py
```python:blog/views.py
def post_publish(request, pk):
  post = get_object_or_404(Post, pk=pk)
  post.publish()
  return redirect('post_detail', pk=pk)
```
