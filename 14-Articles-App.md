News App / 14. Articles App
========================================================


* Create the app named articles

```shell
docker-compose exec web python manage.py startapp articles
```

* Update the settings
```python
# django_project/settings.py 
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    "django.contrib.sites",
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Third-party
    "crispy_forms",
    "crispy_bootstrap5",
    "allauth",
    "allauth.account",
    # Local
    "accounts.apps.AccountsConfig",
    "pages.apps.PagesConfig",
    "articles.apps.ArticlesConfig",  # new
]
```

* Write the model
```python
# articles/models.py

from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=255)
    body = models.TextField()
    date = models.DateTimeField(auto_now_add=True)
    author = models.CharField(max_length=200)

    def __str__(self):
        return self.title
```

* Create a new migration record.
```shell
docker-compose exec web python manage.py makemigrations
docker-compose exec web python manage.py migrate
```

* Edit admin.py
```python
# articles/admin.py
from django.contrib import admin

from .models import Article

admin.site.register(Article)
```

* Go to http://127.0.0.1:8000/admin/ . Make a new entry.

* Update admin
```python
# articles/admin.py
from django.contrib import admin

from .models import Article

class ArticleAdmin(admin.ModelAdmin):
    list_display = ("title", "body", "author",)

admin.site.register(Article, ArticleAdmin)
```

* Update the urls.py within the django_project directory
```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    # Django admin
    path('admin/', admin.site.urls),
    # User management
    path("accounts/", include("allauth.urls")),
    # Local apps
    path("", include("pages.urls")),
    path("articles/", include("articles.urls")),
]
```

* Create a urls.py file for the articles app
```python
# articles/urls.py

from django.urls import path

from .views import ArticleListView

urlpatterns = [
    path("", ArticleListView.as_view(), name="article_list"),
]
```

* Write the view
```python
# articles/views.py

from django.views.generic import ListView

from .models import Article

class ArticleListView(ListView):
    model = Article
    template_name = "articles/article_list.html" 
```

* Within the templates directory create an app specific folder named articles.
```shell
mkdir templates/articles/
```

* Create the article_list.html file within the templates/articles folder.
```html
<!-- templates/articles/article_list.html -->
{% extends '_base.html' %}

{% block title %}Articles{% endblock title %}

{% block content %}

{% for article in object_list %}
  <div>
    <h2><a href="">{{ article.title }}</a></h2>
  </div>    
{% endfor %}
{% endblock content %}    
```

* Restart the Docker containers.
```shell
docker-compose down
docker-compose up -d
```

* Check http://127.0.0.1:8000/articles/ 

* Update views.py using context_object_name
```python

# articles/views.py

from django.views.generic import ListView

from .models import Article

class ArticleListView(ListView):
    model = Article
    context_object_name = "article_list"
    template_name = "articles/article_list.html"
```

* Modify the template file
```html
<!-- templates/articles/article_list.html -->
{% extends '_base.html' %}

{% block title %}Articles{% endblock title %}

{% block content %}

{% for article in article_list %}
  <div>
    <h2><a href="">{{ article.title }}</a></h2>
  </div>    
{% endfor %}
{% endblock content %}   
```

* Check http://127.0.0.1:8000/articles/

Individual Articel Page
========================================================

* Edit the urls.py within the articles folder
```python

# articles/urls.py

from django.urls import path

from .views import ArticleListView, ArticleDetailView

urlpatterns = [
    path("", ArticleListView.as_view(), name="article_list"),
    path("<int:pk>/", ArticleDetailView.as_view(), name="article_detail"),
]
```

* Write the view
```python
# articles/views.py

from django.views.generic import ListView, DetailView

from .models import Article

class ArticleListView(ListView):
    model = Article
    context_object_name = "article_list"
    template_name = "articles/article_list.html"

class ArticleDetailView(DetailView):
    model = Article
    template_name = "articles/article_detail.html"
```

* Create the template file article_detail.html 
```html
<!-- templates/articles/article_detail.html -->
{% extends '_base.html' %}

{% block title %}{{ object.title }}{% endblock title %}

{% block content %}
  <div class="article-detail">
    <h2><a href="">{{ object.title }}</a></h2>
    <p>Text: {{ object.body }}</p>
    <p>Author: {{ object.author }}</p>
  </div>    
{% endblock content %}
```

* Check http://127.0.0.1:8000/articles/1/

* Modify the view, and add context_object_name
```python
# articles/views.py

from django.views.generic import ListView, DetailView

from .models import Article

class ArticleListView(ListView):
    model = Article
    context_object_name = "article_list"
    template_name = "articles/article_list.html"

class ArticleDetailView(DetailView):
    model = Article
    context_object_name = "article"
    template_name = "articles/article_detail.html"
```

* Update the template file
```html
<!-- templates/articles/article_detail.html -->
{% extends '_base.html' %}

{% block title %}{{ article.title }}{% endblock title %}

{% block content %}
  <div class="article-detail">
    <h2><a href="">{{ article.title }}</a></h2>
    <p>Text: {{ article.body }}</p>
    <p>Author: {{ article.author }}</p>
  </div>    
{% endblock content %}
```

* Modify the template file article_list.html, point to detail page
```html
<!-- templates/articles/article_list.html -->
{% extends '_base.html' %}

{% block title %}Articles{% endblock title %}

{% block content %}

{% for article in article_list %}
  <div>
    <h2><a href="{% url 'article_detail' article.pk %}">{{ article.title }}</a></h2>
  </div>    
{% endfor %}
{% endblock content %}
```

* Check http://127.0.0.1:8000/articles/

* Add links to the navbar in templates/_base.html.
```html
<!-- templates/_base.html -->
{% load static %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{% block title %}SnapNews{% endblock title %}</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
    <!-- CSS -->
    <link rel="stylesheet" href="{% static 'css/base.css' %}">
  </head>
  <body>
    <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
      <div class="container-fluid">
        <a class="navbar-brand" href="#">SnapNews</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
        data-bs-target="#navbarCollapse" aria-controls="navbarCollapse"
        aria-expanded="false" aria-label="Toggle navigation">
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarCollapse">
          <ul class="navbar-nav me-auto mb-2 mb-md-0">
            <li class="nav-item">
              <a class="nav-link" href="{% url 'article_list' %}">News</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="{% url 'about' %}">About</a>
            </li>         
            {% if user.is_authenticated %}
              <li class="nav-item">
                <a class="nav-link" href="{% url 'account_logout' %}">Log Out</a>
              </li>
            {% else %}
            <li class="nav-item">
              <a class="nav-link" href="{% url 'account_login' %}">Log In</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="{% url 'account_signup' %}">Sign Up</a>
            </li>
            {% endif %} 
          </ul>
        </div>
      </div>
    </nav>  
      <div class="container">
      {% block content %}    
      {% endblock content %}    
      </div>
      <!-- Bootstrap JavaScript -->
      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-C6RzsynM9kWDrMNeT87bh95OGNyZPhcTNXj1NW7RuBCsyN/o0jlpcV8Qyq46cDfL" crossorigin="anonymous"></script>
      <!-- JavaScript -->
      <script src="{% static 'js/base.js' %}"></script>
    </body>
  </html>
```

* Check http://127.0.0.1:8000/

* Add the get_absolute_url method to the model
```python

# articles/models.py

from django.db import models
from django.urls import reverse

class Article(models.Model):
    title = models.CharField(max_length=255)
    body = models.TextField()
    date = models.DateTimeField(auto_now_add=True)
    author = models.CharField(max_length=200)

    def __str__(self):
        return self.title
    
    def get_absolute_url(self):
        return reverse("article_detail", args=[str(self.id)])
```


* Update the article_list.html
```html
<!-- templates/articles/article_list.html -->
{% extends '_base.html' %}

{% block title %}Articles{% endblock title %}

{% block content %}

{% for article in article_list %}
  <div>
    <h2><a href="{{ article.get_absolute_url }}">{{ article.title }}</a></h2>
  </div>    
{% endfor %}
{% endblock content %}
```

* Now switch from pk to uuid. First modify the model
```python
# articles/models.py

import uuid
from django.db import models
from django.urls import reverse


class Article(models.Model):
    id = models.UUIDField(
        primary_key = True,
        default=uuid.uuid4,
        editable=False
    )
    title = models.CharField(max_length=255)
    body = models.TextField()
    date = models.DateTimeField(auto_now_add=True)
    author = models.CharField(max_length=200)

    def __str__(self):
        return self.title
    
    def get_absolute_url(self):
        return reverse("article_detail", args=[str(self.id)])

```

* Modify the urls
```python
# articles/urls.py

from django.urls import path

from .views import ArticleListView, ArticleDetailView

urlpatterns = [
    path("", ArticleListView.as_view(), name="article_list"),
    path("<uuid:pk>/", ArticleDetailView.as_view(), name="article_detail"),
]
```

* Clean up the database
```shell
docker-compose exec web rm -r articles/migrations
docker-compose down
```

* Have a look at the database, and go on with resetting the database 
```shell
docker volume ls

docker volume rm newspaper_postgres_data
docker-compose up -d
docker-compose exec web python manage.py makemigrations articles
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
```

* Log in as admin and make new entries

* Check http://127.0.0.1:8000/