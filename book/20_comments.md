# コメントを実装

## コメントモデルを実装

models.pyに追記します。

blog/models.py
```python:blog/models.py
class Comment(models.Model):
  post = models.ForeignKey(
    'blog.Post', on_delete=models.CASCADE, related_name='comments')
  author = models.CharField(max_length=200)
  text = models.TextField()
  created_date = models.DateTimeField(default=timezone.now)
  approved_comment = models.BooleanField(default=False)

  def approve(self):
    self.approved_comment = True
    self.save()

  def __str__(self):
    return self.text
```

## データベースにコメントモデルのテーブルを追加

```
(myvenv) ~$ pthon3 manage.py makemigrations blog
(myvenv) ~$ python3 manage.py migrate blog
```
## 管理画面にコメントモデルを登録

blog/admin.py
```python:blog/admin.py
from django.contrib import admin
from .models import Post, Comment

admin.site.register(Post)
admin.site.register(Comment)
```

管理画面にアクセスするとコメントの追加や削除が出来るようになっています。

http://127.0.0.1:8000/admin/blog/comment/

## コメントを表示

最後の{% endblock %}の前に追記します。

blog/templates/blog/post_detail.html
```html:blog/templates/blog/post_detail.html
<p>
  {{ post.text|linebreaksbr }}
</p>

<hr>
{% for comment in post.comments.all %}
  <p class="comment-meta">{{ comment.created_date }}</p>
  <strong>{{ comment.author }}</strong>
  <p>{{ comment.text|linebreaks }}</p>
{% empty %}
  <p>まだコメントはありません。</p>
{% endfor %}

{% endblock %}
```

投稿ページでコメントの数を表示します。

blog/templates/blog/post_list.html
```html:blog/templates/blog/post_list.html
  <p>{{ post.text|linebreaksbr|truncatechars:100 }}</p>
  <a href="{% url 'post_detail' pk=post.pk %}">
    <p class="post-comment">Comments: {{ post.comments.count }}</p>
  </a>
</div>
<hr>
{% endfor %}
```

## コメントを投稿

forms.pyファイルを変更します。

blog/forms.py
```python:blog/forms.py
from django import forms
from .models import Post, Comment

class PostForm(forms.ModelForm):
  class Meta:
    model = Post
    fields = ('title', 'text',)


class CommentForm(forms.ModelForm):
  class Meta:
    model = Comment
    fields = ('author', 'text',)

```

### コメントを投稿するボタンを追加

blog/templates/blog/post_detail.html
```html:blog/templates/blog/post_detail.html
<hr>
<p>
  <a class="btn btn-success" href="{% url 'post_comment' pk=post.pk %}" role="button">Add comment</a>
</p>
{% for comment in post.comments.all %}
```

### urlを追加

blog/urls.py
```python:blog/urls.py
path('post/<int:pk>/comment/', views.post_comment, name='post_comment'),
```

### Viewを追加

blog/views.py
```python:blog/views.py
from .forms import PostForm, CommentForm

def post_comment(request, pk):
  post = get_object_or_404(Post, pk=pk)
  if request.method == "POST":
    form = CommentForm(request.POST)
    if form.is_valid():
      comment = form.save(commit=False)
      comment.post = post
      comment.save()
      return redirect('post_detail', pk=post.pk)
  else:
    form = CommentForm()
  return render(request, 'blog/post_comment.html', {'form': form})
```

### コメントを投稿するテンプレートを追加

blog/templates/blog/post_comment.html
```html:blog/templates/blog/post_comment.html
{% extends 'blog/base.html' %}

{% block header %}
<div class="site-heading">
  <h1>New comment</h1>
</div>
{% endblock %}

{% block content %}
  <form method="POST" class="post-form">
    {% csrf_token %}
    {{ form.as_p }}
    <div class="text-right">
      <button type="submit" class="save btn btn-success" role="button">Send</button>
    </div>
  </form>
{% endblock %}
```

## コメントを管理

コメントを承認または削除できるようにします。

RemoveボタンとApproveボタンを追加します。

blog/templates/blog/post_detail.html
```html:blog/templates/blog/post_detail.html
{% for comment in post.comments.all %}
  {% if not comment.approved_comment %}
  <p>
    <a class="btn btn-danger" href="{% url 'comment_remove' pk=comment.pk %}" role="button">Remove</a>
    <a class="btn btn-primary" href="{% url 'comment_approve' pk=comment.pk %}" role="button">Approve</a>
  </p>
  {% endif %}
```

### urlを追加

blog/urls.py
```python:blog/urls.py
  path('comment/<int:pk>/approve/', views.comment_approve, name='comment_approve'),
  path('comment/<int:pk>/remove/', views.comment_remove, name='comment_remove'),
```

### Viewを追加

blog/views.py
```python:blog/views.py
from .models import Post, Comment

@login_required
def comment_approve(request, pk):
  comment = get_object_or_404(Comment, pk=pk)
  comment.approve()
  return redirect('post_detail', pk=comment.post.pk)

@login_required
def comment_remove(request, pk):
  comment = get_object_or_404(Comment, pk=pk)
  comment.delete()
  return redirect('post_detail', pk=comment.post.pk)
```

これで、コメントの承認と削除ができるようになりました。

### 承認されたコメント数を表示

blog/templates/blog/post_list.html
```html:blog/templates/blog/post_list.html
  <p>{{ post.text|linebreaksbr|truncatechars:100 }}</p>
  <a href="{% url 'post_detail' pk=post.pk %}">
    <p class="post-comment">Comments: {{ post.approved_comments.count }}</p>
  </a>
</div>
<hr>
{% endfor %}
```

### モデルを追加

Postモデルに追加します。

blog/models.py
```python:blog/models.py
class Post(models.Model):

  def approved_comments(self):
    return self.comments.filter(approved_comment=True)
```
