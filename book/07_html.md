# HTML追加

templatesフォルダとblogフォルダを追加します。

```
blog
└───templates
    └───blog
```

作成したblogフォルダにpost_list.htmlファイルを追加します。

blog/templates/blog/post_list.html
```html:blog/templates/blog/post_list.html
<html>
<body>
    <p>Hello!</p>
    <p>This is working.</p>
</body>
</html>
```

### Webサーバー開始

```
(myvenv) ~$ python3 manage.py runserver
```
http://127.0.0.1:8000/

ページが表示されました。
