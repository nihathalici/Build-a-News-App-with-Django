News App / 15. Comments App
========================================================

* Write the model.


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

* Apply it to the database
```shell
docker-compose exec web python manage.py makemigrations articles
docker-compose exec web python manage.py migrate
```

* Update the admin.py
```python
# articles/admin.py
from django.contrib import admin

from .models import Article, Comment

class CommentInline(admin.TabularInline):
    model = Comment

class ArticleAdmin(admin.ModelAdmin):
    inlines = [
        CommentInline,
    ]
    list_display = ("title", "body", "author",)

admin.site.register(Article, ArticleAdmin)
```

* Create a testuser and add reviews 

* Update the article_detail.html file.
```html
<!-- templates/articles/article_detail.html -->
{% extends '_base.html' %}

{% block title %}{{ article.title }}{% endblock title %}

{% block content %}
  <div class="article-detail">
    <h2><a href="">{{ article.title }}</a></h2>
    <p>Text: {{ article.body }}</p>
    <p>Author: {{ article.author }}</p>
<div>
  <h3>Comments</h3>
  <ul>
    {% for comment in article.comments.all %}
    <li>{{ comment }} ({{ comment.author }})</li>    
    {% endfor %}     
  </ul>
</div>
  </div>    
{% endblock content %}
```