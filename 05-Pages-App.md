News App / 5. Pages App
========================================================

* Create the pages app.

```shell
docker-compose exec web python manage.py startapp pages
```

* Update the TEMPLATES and INSTALLED_APPS sections in the settings.py
```shell
# django_project/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Local
    "accounts.apps.AccountsConfig",
    "pages.apps.PagesConfig",  # new
]

TEMPLATES = [
 ...
        'DIRS': [BASE_DIR / "templates"],  # new
 ...
]

```

* Create the templates folder.
* Create within the templates file _base.html and home.html.

```html
<!-- templates/_base.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{% block title %}SnapNews{% endblock title %}</title>
  </head>
  <body>
    <div class="container">
    {% block content %}    
    {% endblock content %}    
    </div>
  </body>
</html>
```

```html
<!-- templates/home.html -->
{% extends '_base.html' %}

{% block title %}SnapNews - Home{% endblock title %}

{% block content %}
  <h1>SnapNews - Home page.</h1>    
{% endblock content %}    
```

* Edit the urls.py within the django_project folder.
```python

# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", include("pages.urls")),
]
```

* Create another urls.py within the pages folder.

```python

# pages/urls.py

from django.urls import path

from .views import HomePageView 

urlpatterns = [
    path("", HomePageView.as_view(), name="home"),
]
```

* Write the view.
```python

# pages/views.py

from django.views.generic import TemplateView

class HomePageView(TemplateView):
    template_name = "home.html"
```

* Restart the server.
```shell
docker-compose down
docker-compose up -d
```