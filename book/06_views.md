# View追加

Viewを追加して、どのテンプレートファイルを使用するのかを指定します。

blog/views.py
```python:blog/views.py
from django.shortcuts import render

def post_list(request):
    return render(request, 'blog/post_list.html', {})
```
