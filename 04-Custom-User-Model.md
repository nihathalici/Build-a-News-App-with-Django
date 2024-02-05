News App / 4. Switch to Docker
========================================================

* Create an app named accounts

```shell
docker-compose exec web python manage.py startapp accounts
```


* Write the model.
```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser
from django.db import models

class CustomUser(AbstractUser):
    pass
```

* Edit the settings file.
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Local
    "accounts.apps.AccountsConfig",  # new
]

AUTH_USER_MODEL = "accounts.CustomUser"  # new, at the bottom
```

* Apply the changes to the database.
```shell
docker-compose exec web python manage.py makemigrations accounts
docker-compose exec web python manage.py migrate
```

* Create forms.py within the accounts folder.
```python
from django.contrib.auth import get_user_model
from django.contrib.auth.forms import UserCreationForm, UserChangeForm

class CustomUserCreationForm(UserCreationForm):
    class Meta:
        model = get_user_model()
        fields = (
            "email",
            "username",
        )

class CustomUserChangeForm(UserChangeForm):
    class Meta:
        model = get_user_model()
        fields = (
            "email",
            "username",
        )
```

* Update accounts/admin.py file.

```python
# accounts/admin.py

from django.contrib import admin
from django.contrib.auth import get_user_model
from django.contrib.auth.admin import UserAdmin

from .forms import CustomUserCreationForm, CustomUserChangeForm

CustomUser = get_user_model()

class CustomUserAdmin(UserAdmin):
    add_form = CustomUserCreationForm
    form = CustomUserChangeForm
    model = CustomUser
    list_display = [
        "email",
        "username",
        "is_superuser",
    ]

admin.site.register(CustomUser, CustomUserAdmin)
```

* Create the superuser.
```shell
docker-compose exec web python manage.py createsuperuser
```