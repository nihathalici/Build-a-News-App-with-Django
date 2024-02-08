News App / 13. Email
========================================================

* Create an email directory within the templates/account directory.


```shell
mkdir templates/account/email
```

* Within the new email folder create two files: email_confirmation_subject.txt and email_confirmation_message.txt

* email_confirmation_subject.txt
```txt
{% load i18n %}
{% autoescape off %}
{% blocktrans %}Please Confirm Your E-mail Address{% endblocktrans %}
{% endautoescape %}
```

* Go to http://127.0.0.1:8000/admin/sites/ .
* Change the Domain Name to snapnews.com, and the Display Name as SnapNews.

* If the Sites doesn't display, edit the settings
```python
# django_project/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    "django.contrib.sites",  # new
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Third-party
    "crispy_forms",
    "crispy_bootstrap5",
    "allauth",
    "allauth.account",
    # Local
    "accounts.apps.AccountsConfig",
    "pages.apps.PagesConfig",
]
```

* Update the settings, and a new line at the bottom
```python
# django_project/settings.py
DEFAULT_FROM_EMAIL = "admin@djangobookstore.com"
```

* Go to http://127.0.0.1:8000/accounts/signup/ and create a new account for testing purposes

* Check the command line to see the message
```shell
docker-compose logs
```

* Create the email_confirm.html file within the templates/account folder.
```html
<!-- templates/account/email_confirm.html -->
{% extends '_base.html' %}
{% load i18n %}
{% load account %}

{% block head_title %}{% trans 'Confirm E-mail Address' %}{% endblock %}


{% block content %}
<h1>{% trans 'Confirm E-mail Address' %}</h1>

{% if confirmation %}
  {% user_display confirmation.email_address.user as user_display %}
  <p>
  {% blocktrans with confirmation.email_address.email as email %}Please confirm that <a href="mailto:{{ email }}">{{ email }}</a> is an e-mail address for user {{ user_display }}.{% endblocktrans %}</p>
  <form action="{% url 'account_confirm_email' confirmation.key %}" method="post">
{% csrf_token %}
<button class="btn btn-primary" type="submit">{% trans 'Confirm' %}</button>
  </form>
  {% else %}
  {% url 'account_email' as email_url %}
  <p>
  {% blocktrans %}This e-mail confirmation link expired or is invalid. Please <a href="{{ email_url }}">issue a new e-mail confirmation request</a>{% endblocktrans %}</p>
      
{% endif %}
        
{% endblock content %}        
```

* After obtaining a username and password with an email provider, update the settings
```python 
EMAIL_BACKEND = "django.core.mail.backends.smtp.EmailBackend"
```