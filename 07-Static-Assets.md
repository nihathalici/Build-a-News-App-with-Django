News App / 7. Static Assets
========================================================

* Create in total 4 folders. static at project level. css, js, and images within the new created static folder. 

```shell
mkdir static
mkdir static/css
mkdir static/js
mkdir static/images
```

* Now within this folders create 3 files: static/css/base.css, static/js/base.js, and static/images/.keep

* Update the settings.py file.
```python
# django_project/settings.py
STATIC_URL = "/static/"
STATICFILES_DIRS = [BASE_DIR / "static"]
```

* Write base.css
```css
/* static/css/base.css */
h1 {
    color: red;
}
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
    <!-- CSS -->
    <link rel="stylesheet" href="{% static 'css/base.css' %}">
  </head>
  <body>
    <div class="container">
    {% block content %}    
    {% endblock content %}    
    </div>
  </body>
</html>
```

* Check it in your web browser: http://127.0.0.1:8000/

* Save snapnews_logo_1.jpg into the directory static/images

* Update home.html by adding the {% load static %} tag at the top
```html
<!-- templates/home.html -->
{% extends '_base.html' %}
{% load static %}
{% block title %}SnapNews - Home{% endblock title %}

{% block content %}
  <h1>SnapNews - Home page</h1>
  
  {% if user.is_authenticated %}
    <p>Hi, {{ user.email }}!</p>
    <p><a href="{% url 'logout' %}">Log Out</a></p>
  {% else %}
    <p>You are not logged in</p>
    <a href="{% url 'login' %}">Log In</a> |
    <a href="{% url 'signup' %}">Sign Up</a>
  {% endif %}
        
{% endblock content %}  
```

* Update the base.css file

```css
/* static/css/base.css */
h1 {
    color: red;
}

.bookcover {
    height: 300px;
    width: auto;
}
```

* Edit the JavaScript file
```JavaScript
// static/js/base.js
console.log("JavaScript here!")
```

* Add JavaScript file to the _base.html
```html
<!-- templates/_base.html -->
{% load static %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{% block title %}SnapNews{% endblock title %}</title>
    <!-- CSS -->
    <link rel="stylesheet" href="{% static 'css/base.css' %}">
  </head>
  <body>
    <div class="container">
    {% block content %}    
    {% endblock content %}    
    </div>
  <!-- JavaScript -->
  <script src="{% static 'js/base.js' %}"></script>
  </body>
</html>
```

* Check it in your web browser: http://127.0.0.1:8000/
* Google Chrome/More Tools/Developer/Developer Tools/Console

* Update the settings.py file.
```python
# django_project/settings.py
STATIC_URL = "/static/"
STATICFILES_DIRS = [BASE_DIR / "static"]
STATIC_ROOT = BASE_DIR / "staticfiles"
STATICFILES_STORAGE = "django.contrib.staticfiles.storage.StaticFilesStorage"
```

* Run collectstatic
```shell
docker-compose exec web python manage.py collectstatic
```
