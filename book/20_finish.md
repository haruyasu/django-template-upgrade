# JavaScriptで動きをつける

base.htmlにJavaScriptを追記します。

blog/templates/blog/base.html

```html:blog/templates/blog/base.html
  <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"></script>
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>
  <script src="{% static 'js/blog.js' %}"></script>

</body>

</html>
```

スクロールした時にナビゲーションが表示されるなど動きを付けます。

blog/static/jsフォルダを作成します。

blog.jsファイルを作成します。

JavaScript:blog/static/js/blog.js
```JavaScript:blog/static/js/blog.js
(function ($) {
  "use strict"; // Start of use strict

  // Floating label headings for the contact form
  $("body").on("input propertychange", ".floating-label-form-group", function (e) {
    $(this).toggleClass("floating-label-form-group-with-value", !!$(e.target).val());
  }).on("focus", ".floating-label-form-group", function () {
    $(this).addClass("floating-label-form-group-with-focus");
  }).on("blur", ".floating-label-form-group", function () {
    $(this).removeClass("floating-label-form-group-with-focus");
  });

  // Show the navbar when the page is scrolled up
  var MQL = 992;

  //primary navigation slide-in effect
  if ($(window).width() > MQL) {
    var headerHeight = $('#mainNav').height();
    $(window).on('scroll', {
      previousTop: 0
    },
      function () {
        var currentTop = $(window).scrollTop();
        //check if user is scrolling up
        if (currentTop < this.previousTop) {
          //if scrolling up...
          if (currentTop > 0 && $('#mainNav').hasClass('is-fixed')) {
            $('#mainNav').addClass('is-visible');
          } else {
            $('#mainNav').removeClass('is-visible is-fixed');
          }
        } else if (currentTop > this.previousTop) {
          //if scrolling down...
          $('#mainNav').removeClass('is-visible');
          if (currentTop > headerHeight && !$('#mainNav').hasClass('is-fixed')) $('#mainNav').addClass('is-fixed');
        }
        this.previousTop = currentTop;
      });
  }

})(jQuery); // End of use strict
```

## トップ画像を追加

blog/static/imgフォルダを作成します。

imgフォルダに好きな画像を追加します。

https://github.com/haruyasu/django-template-upgrade/tree/master/blog/static/img

## Aboutページ追加

固定ページを追加します。

自己紹介をするAboutページやコンタクトページなど自由に作成してみて下さい。

html:blog/templates/blog/base.html
```html:blog/templates/blog/base.html
<li class="nav-item">
  <a class="nav-link" href="{% url 'about' %}">About</a>
</li>
```

### urlを追加

blog/urls.py
```python:blog/urls.py
  path('about/', views.about, name='about'),
```

### Viewを追加

blog/views.py
```python:blog/views.py
def about(request):
  return render(request, 'page/about.html')
```

## 完成

ブログアプリケーションの構築が完了です。

記事の投稿、編集、削除、公開、コメント、ログイン、ログアウトなどの機能を操作して、問題がないか確かめて下さい。

問題がなければ、全世界に公開します。
