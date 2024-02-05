News App / 8. Bootstrap
========================================================

* Add Bootstrap links to the _base.html 

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

* Add a fixed navbar to the _base.html
```html
<!-- templates/_base.html -->
{% load static %}
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{% block title %}Bookstore{% endblock title %}</title>
    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">
    <!-- CSS -->
    <link rel="stylesheet" href="{% static 'css/base.css' %}">
  </head>
  <body>
  <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Bookstore</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse"
      data-bs-target="#navbarCollapse" aria-controls="navbarCollapse"
      aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarCollapse">
        <ul class="navbar-nav me-auto mb-2 mb-md-0">          
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

* Update base.css
```css
/* static/css/base.css */
body {
    min-height: 75rem;
    padding-top: 4.5rem;
}

h1 {
    color: red;
}

img.bookcover {
    height: 300px;
    width: auto;
}
```

* Update home.html
```html
<!-- templates/home.html -->
{% extends '_base.html' %}
{% load static %}
{% block title %}SnapNews - Home{% endblock title %}

{% block content %}
  <h1>"SnapNews: Unfolding Stories in the Blink of an Eye!"</h1>
  <img class="bookcover" src="{% static 'images/snapnews_logo_1.jpg' %}" alt="">
{% endblock content %}
```

* Check it in your web browser: http://127.0.0.1:8000/