News App / 10. Crispy Forms
========================================================

* Add crispy to the requirements.txt file 

```txt
asgiref==3.7.2
Django==4.0.10
psycopg2-binary==2.9.3
sqlparse==0.4.4
django-crispy-forms==1.14.0
crispy-bootstrap5==0.6
```

* Restart Docker
```shell
docker-compose down
docker-compose up -d --build
```

* Update the settings.py file
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Third-party
    "crispy_forms",            # new
    "crispy_bootstrap5",       # new
    # Local
    "accounts.apps.AccountsConfig",
    "pages.apps.PagesConfig",
]

# django-crispy-forms
CRISPY_ALLOWED_TEMPLATE_PACKS = "bootstrap5"  # new
CRISPY_TEMPLATE_PACK = "bootstrap5"           # new
```

* Now use crispy in template pages
```html
<!-- templates/registration/signup.html -->
{% extends '_base.html' %}
{% load crispy_forms_tags %}

{% block title %}Sign Up{% endblock title %}

{% block content %}
  <h2>Sign Up</h2>
  <form action="" method="post">
    {% csrf_token %}
    {{ form|crispy }}
    <button class="btn btn-success" type="submit">Sign Up</button>
  </form>    
{% endblock content %}
```

* Update login.html
```html
<!-- templates/registration/login.html -->
{% extends '_base.html' %}
{% load crispy_forms_tags %}

{% block title %}Log In{% endblock title %}

{% block content %}
  <h2>Log In</h2>
  <form action="" method="post">
    {% csrf_token %}
    {{ form|crispy }}
    <button class="btn btn-success" type="submit">Log In</button>
  </form>    
{% endblock content %}    
```

* Check it in your web browser: http://127.0.0.1:8000/