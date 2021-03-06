
# テンプレートの拡張

HTMLの共通部分を別のページでも使えるようにします。

こうすることで、ヘッダーやフッターなど各ページで同じことを書く必要がなくなります。

## base.html

先ほど変更したpost_list.htmlをコピーして、base.htmlを作成します。

```
blog
└───templates
    └───blog
        ├── base.html
        └── post_list.html
```

base.htmlのheader部分とcontent部分の内容を変更します。

\{% block xxx %\}と\{% endblock %\}で囲います。

xxxの部分は好きな名前に変えて下さい。

同じファイルにいくつもblockテンプレートタグを書くことができます。

blog/templates/blog/base.html
```html
  <!-- Page Header -->
  <header class="masthead">
    <div class="overlay"></div>
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          {% block header %}
          {% endblock %}
        </div>
      </div>
    </div>
  </header>

  <!-- Main Content -->
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-md-10 mx-auto">
        {% block content %}
        {% endblock %}
      </div>
    </div>
  </div>

```

## post_list.html

post_list.htmlには内容が変わる部分だけを記載します。

今回はheaderとcontentの内容がページによって変わります。

先頭には\{% extends 'blog/base.html' %\}を追記し、テンプレートを拡張することを指定します。

blog/templates/blog/post_list.html
```html
{% extends 'blog/base.html' %}

{% block header %}
<div class="site-heading">
  <h1>Django Startup</h1>
  <span class="subheading">This is your first step!</span>
</div>
{% endblock %}

{% block content %}
  {% for post in posts %}
  <div class="post-preview">
    <h2 class="post-title">
      {{ post.title }}
    </h2>
    <p class="post-meta">{{ post.published_date }}</p>
    <p>{{ post.text|linebreaksbr|truncatechars:100 }}</p>
  </div>
  <hr>
  {% endfor %}

  <!-- Pager -->
  <div class="clearfix">
    <a class="btn btn-primary float-right" href="#">Older Posts &rarr;</a>
  </div>
{% endblock %}
```
