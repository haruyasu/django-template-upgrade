
# テンプレート拡張

HTMLの共通部分を取り出して、異なるページでも使えるようにします。

こうすることで、各ページで同じことを書く必要がなくなります。

先ほど変更したpost_list.htmlをコピーして、base.htmlを作成します。

```
blog
└───templates
    └───blog
        ├── base.html
        └── post_list.html
```

base.htmlのheader部分とcontent部分の内容を変更します。

blog/templates/blog/base.html
```html:blog/templates/blog/base.html
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

post_list.htmlには内容が変わる部分を記載します。

先頭にはテンプレートを拡張することを追記します。

blog/templates/blog/post_list.html
```html:blog/templates/blog/post_list.html
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