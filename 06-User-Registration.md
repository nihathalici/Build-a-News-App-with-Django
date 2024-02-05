News App / 6. User Registration
========================================================

* Edit django_project/urls.py. 

```python
# django_project/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    # Django admin
    path('admin/', admin.site.urls),
    # User management
    path("accounts/", include("django.contrib.auth.urls")),  # new
    # Local apps
    path("", include("pages.urls")),
]
```

* Update home.html
```html
<!-- templates/home.html -->
{% extends '_base.html' %}

{% block title %}SnapNews - Home{% endblock title %}

{% block content %}
  <h1>SnapNews - Home page</h1>
  
  {% if user.is_authenticated %}
    <p>Hi {{ user.email }}!</p>
  {% else %}
    <p>You are not logged in</p>
    <a href="{% url 'login' %}">Log In</a>  
  {% endif %}        
{% endblock content %}    
```

* Check it in your web browser: http://127.0.0.1:8000/

* Create a new folder named registration within the templates folder.
```shell
mkdir templates/registration
```

* Create a login.html within the registration folder.
```html
<!-- templates/registration/login.html -->
{% extends '_base.html' %}

{% block title %}Log In{% endblock title %}

{% block content %}
  <h2>Log In</h2>
  <form action="" method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Log In</button>
  </form>    
{% endblock content %}
```  

* Add the new lines at the bottom of the settings.py file. 

```python
# django_project/settings.py

LOGIN_REDIRECT_URL = "home"
LOGOUT_REDIRECT_URL = "home"
```

* Modify the home.html.
```html
<!-- templates/home.html -->
{% extends '_base.html' %}

{% block title %}SnapNews - Home{% endblock title %}

{% block content %}
  <h1>SnapNews - Home page</h1>
  
  {% if user.is_authenticated %}
    <p>Hi, {{ user.email }}!</p>
    <p><a href="{% url 'logout' %}">Log Out</a></p>
  {% else %}
    <p>You are not logged in</p>
    <p><a href="{% url 'login' %}">Log In</a></p>   
  {% endif %}
        
{% endblock content %}  
```

* Create the urls.py within accounts folder.
```python
# accounts/urls.py

from django.urls import path
from .views import SignupPageView

urlpatterns = [
    path("signup/", SignupPageView.as_view(), name="signup"),
]
```

* Modify the urls.py within the django_project folder.
```python

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    # Django admin
    path('admin/', admin.site.urls),
    # User management
    path("accounts/", include("django.contrib.auth.urls")),
    # Local apps
    path("accounts/", include("accounts.urls")),
    path("", include("pages.urls")),
]
```

* Write the views.py within the accounts folder.
```python
# accounts/views.py

from django.urls import reverse_lazy
from django.views import generic

from .forms import CustomUserCreationForm

class SignupPageView(generic.CreateView):
    form_class = CustomUserCreationForm
    success_url = reverse_lazy("login")
    template_name = "registration/signup.html"
```

* Create the signup.html within the templates/registration folder.
```html
<!-- templates/registration/signup.html -->
{% extends '_base.html' %}

{% block title %}Sign Up{% endblock title %}

{% block content %}
  <h2>Sign Up</h2>
  <form action="" method="post">
    {% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Sign Up</button>
  </form>    
{% endblock content %}
```

* Modify the home.html and add the signup link.
```html
<!-- templates/home.html -->
{% extends '_base.html' %}

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