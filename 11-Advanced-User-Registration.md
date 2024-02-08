News App / 11. Advanced User Registration
========================================================

* Update requirements.txt file.

```txt
asgiref==3.7.2
Django==4.0.10
psycopg2-binary==2.9.3
sqlparse==0.4.4
django-crispy-forms==1.14.0
crispy-bootstrap5==0.6
django-allauth==0.50.0
```

* Restart the server and rebuild the Docker image.
```shell
docker-compose down
docker-compose up -d --build
```

* Edit the settings file
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Third-party
    "crispy_forms",
    "crispy_bootstrap5",
    "allauth",           # new
    "allauth.account",   # new
    # Local
    "accounts.apps.AccountsConfig",
    "pages.apps.PagesConfig",
]

# django-allauth config
ACCOUNT_LOGOUT_REDIRECT = "home"    # new
SITE_ID = 1                 # new
SITE_ID = 1 
AUTHENTICATION_BACKENDS = (                             # new
    "django.contrib.auth.backends.ModelBackend",
    "allauth.account.auth_backends.AuthenticationBackend",
)
EMAIL_BACKEND = "django.core.mail.backends.console.EmailBackend"   # new
```

* Update the database
```shell
docker-compose exec web python manage.py migrate
```

* Update the urls.py file within the django_project folder.
``` python
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
]
```

* After this update the files accounts/urls.py and accounts/views.py were both useless and can be deleted. 

* Create the account directory within the templates folder.
* Move the existing login.html and signup.html to the account folder.

```shell
mkdir templates/account
mv templates/registration/login.html templates/account/login.html 
mv templates/registration/signup.html templates/account/signup.html
```

* After this update the templates/registration is useless and can be deleted. 

```shell
rm -r templates/registration
```

* Update _base.html
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

* Update the settings
```python
# django_project/settings.py
# django-allauth config

ACCOUNT_SESSION_REMEMBER = True
```

* Create the logout.html within the templates/account folder.
```html
<!-- templates/account/logout.html -->
{% extends '_base.html' %}
{% load crispy_forms_tags %}

{% block title %}Log Out{% endblock title %}

{% block content %}
  <h1>Log Out</h1>
  <p>Are you sure you want to log out?</p>
  <form action="{% url 'account_logout' %}" method="post">
    {% csrf_token %}
    {{ form|crispy }}
    <button class="btn btn-danger" type="submit">Log Out</button>
  </form>   
{% endblock content %}        
```

* Update the settings
```python
# django_project/settings.py
# django-allauth config

ACCOUNT_SIGNUP_PASSWORD_ENTER_TWICE = False
ACCOUNT_USERNAME_REQUIRED = False
ACCOUNT_AUTHENTICATION_METHOD = "email"
ACCOUNT_EMAIL_REQUIRED = True
ACCOUNT_UNIQUE_EMAIL = True
```

* Sign up and create an account for testing purposes