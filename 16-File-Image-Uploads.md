News App / 16. File/Image Uploads
========================================================

* Add pillow to the requirements.txt file.


```txt
asgiref==3.7.2
Django==4.0.10
psycopg2-binary==2.9.3
sqlparse==0.4.4
django-crispy-forms==1.14.0
crispy-bootstrap5==0.6
django-allauth==0.50.0
environs[django]==9.5.0
pillow==9.0.1
```

* Rebuild the Docker image
```shell
docker-compose down
docker-compose up -d --build
```

* Update the settings.py
```python
# django_project/settings.py
MEDIA_URL = "/media/"
MEDIA_ROOT = BASE_DIR / "media"
```

* Create a new directory named media. Another directory called covers within the media folder.
```shell
mkdir media
mkdir media/covers
```

* Modify the urls.py within the django_project folder
```python
# django_project/urls.py

from django.conf import settings
from django.conf.urls.static import static
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    # Django admin
    path('admin/', admin.site.urls),
    # User management
    path("accounts/", include("allauth.urls")),
    # Local apps
    path("", include("pages.urls")),
    path("books/", include("books.urls")),
] + static(
    settings.MEDIA_URL, document_root=settings.MEDIA_ROOT 
)

```

* Update the model
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

* Apply the update to the database
```shell
docker-compose exec web python manage.py makemigrations articles
docker-compose exec web python manage.py migrate
```

* Go to admin and add a picture

* Update article_detail.html file
```html
<!-- templates/articles/article_detail.html -->
{% extends '_base.html' %}

{% block title %}{{ article.title }}{% endblock title %}

{% block content %}
  <div class="article-detail">
    
    {% if article.visual %}
<img class="bookcover" src="{{ article.visual.url }}" alt="{{ article.title }}">     
    {% endif %}
      
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
