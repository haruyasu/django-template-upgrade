
# 新しいアプリケーションの作成

startappコマンドでアプリケーションを追加できます。

今回はブログを作成するので、名前はblogにします。

```
(myvenv) ~$ python3 manage.py startapp blog
```
```
├── blog
│   ├── admin.py
│   ├── apps.py
│   ├── __init__.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── db.sqlite3
├── manage.py
├── mysite
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
├── myvenv
│   └── ...
└── requirements.txt
```

### Djangoでアプリケーションを使えるように設定

INSTALLED_APPSに追加します。

mysite/settings.py
```python:mysite/settings.py
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
]
```

## モデルの作成

最初に投稿機能を追加しますので、Postモデルを追加します。

クラスを追加し、各プロパティを定義していきます。

ForeignKey、CharField、TextField、DateTimeFieldなどのフィールド解説は公式ドキュメントを参考にしてみて下さい。

https://docs.djangoproject.com/ja/2.2/ref/models/fields/#charfield

* ForeignKey：多対1の関係で他のモデルへのリンク
* CharField：文字列のフィールド
* TextField：多量のテキストのフィールド
* DateTimeField：日付と時刻のフィールド

わかりやすいまとまっています。

https://qiita.com/nachashin/items/f768f0d437e0042dd4b3

```python:blog/models.py
from django.conf import settings
from django.db import models
from django.utils import timezone


class Post(models.Model):
  author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
  title = models.CharField(max_length=200)
  text = models.TextField()
  created_date = models.DateTimeField(default=timezone.now)
  published_date = models.DateTimeField(blank=True, null=True)

  def publish(self):
    self.published_date = timezone.now()
    self.save()

  def __str__(self):
    return self.title
```

## データベースにモデルのためのテーブルを作成

モデルを変更したら、下記コマンドでデータベースを再構築が必要になります。

```
(myvenv) ~$ python3 manage.py makemigrations blog
(myvenv) ~$ python3 manage.py migrate blog
```
