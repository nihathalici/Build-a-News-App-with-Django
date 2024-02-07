News App / 09. About Page
========================================================

* Create the template file within the templates folder  

```html
<!-- templates/about.html -->
{% extends '_base.html' %}

{% block title %}About{% endblock title %}

{% block content %}
<h1>About Page</h1>    
{% endblock content %}   
```

* Write the view.
```python
# pages/views.py

from django.views.generic import TemplateView

class HomePageView(TemplateView):
    template_name = "home.html"

class AboutPageView(TemplateView):  # new
    template_name = "about.html"
```

* Update the pages/urls.py file
```python
# pages/urls.py

from django.urls import path

from .views import HomePageView, AboutPageView 

urlpatterns = [
    path("about/", AboutPageView.as_view(), name="about"),
    path("", HomePageView.as_view(), name="home"),
]
```

* Update the _base.html
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
                <a class="nav-link" href="{% url 'logout' %}">Log Out</a>
              </li>
            {% else %}
            <li class="nav-item">
              <a class="nav-link" href="{% url 'login' %}">Log In</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="{% url 'signup' %}">Sign Up</a>
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