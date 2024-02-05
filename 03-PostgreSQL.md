News App / 3. PostgreSQL
========================================================

* Stop running the Docker container.

```shell
docker-compose down
```

* Modify the settings.py
```python3
# django_project/settings.py

DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": "postgres",
        "USER": "postgres",
        "PASSWORD": "postgres",
        "HOST": "db",
        "PORT": 5432,
    }
}
```

* Spin up the containers.
```shell
docker-compose up
```

* Check it in your web browser: http://127.0.0.1:8000/

