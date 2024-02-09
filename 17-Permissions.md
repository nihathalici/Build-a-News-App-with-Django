News App / 17. Permissions
========================================================


* Modify the views.


```python
# articles/views.py

from django.contrib.auth.mixins import LoginRequiredMixin
from django.views.generic import ListView, DetailView

from .models import Article

class ArticleListView(LoginRequiredMixin, ListView):
    model = Article
    context_object_name = "article_list"
    template_name = "articles/article_list.html"
    login_url = "account_login"

class ArticleDetailView(LoginRequiredMixin, DetailView):
    model = Article
    context_object_name = "article"
    template_name = "articles/article_detail.html"
    login_url = "account_login"
```

* Check http://127.0.0.1:8000/ 

* Modify the model
```python
# articles/models.py

import uuid
from django.contrib.auth import get_user_model
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
    visual = models.ImageField(upload_to="visuals/", blank=True)

    class Meta:
        permissions = [
            ("special_status", "Can read all articles"),
        ]

    def __str__(self):
        return self.title
    
    def get_absolute_url(self):
        return reverse("article_detail", args=[str(self.id)])

class Comment(models.Model):
    article = models.ForeignKey(
        Article,
        on_delete=models.CASCADE,
        related_name="comments"
    )
    comment = models.CharField(max_length=255)
    author = models.ForeignKey(
        get_user_model(),
        on_delete=models.CASCADE,
    )
    def __str__(self):
        return self.comment

```

* Refresh the database
```
docker-compose exec web python manage.py makemigrations books
docker-compose exec web python manage.py migrate
```

* Go to http://127.0.0.1:8000/admin/ Navigate to the Users section.
* Create a new user: special@email.com. Scroll down to User permissions
* Select books | book | Can read all books. Save it.

* Modify the views
```python

# articles/views.py

from django.contrib.auth.mixins import (
    LoginRequiredMixin,
    PermissionRequiredMixin
)
from django.views.generic import ListView, DetailView

from .models import Article

class ArticleListView(LoginRequiredMixin, ListView):
    model = Article
    context_object_name = "article_list"
    template_name = "articles/article_list.html"
    login_url = "account_login"

class ArticleDetailView(LoginRequiredMixin, PermissionRequiredMixin, DetailView):
    model = Article
    context_object_name = "article"
    template_name = "articles/article_detail.html"
    login_url = "account_login"
    permission_required = "articles.special_status"

```

* Try out with different users http://127.0.0.1:8000/books/