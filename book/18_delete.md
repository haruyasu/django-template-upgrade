# 削除機能を追加

## 削除ボタンを追加

編集ボタンの下に追加します。

blog/templates/blog/post_detail.html
```html:blog/templates/blog/post_detail.html
<a class="btn btn-danger" href="{% url 'post_remove' pk=post.pk %}" role="button">Delete</a>
```

## urlを追加

blog/urls.py
```python:blog/urls.py
path('post/<pk>/remove/', views.post_remove, name='post_remove'),
```

## Viewを追加

blog/views.py
```python:blog/views.py
def post_remove(request, pk):
  post = get_object_or_404(Post, pk=pk)
  post.delete()
  return redirect('post_list')
```

投稿を削除できるようになりました。
