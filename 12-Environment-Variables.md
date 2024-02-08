News App / 12. Environment Variables
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
environs[django]==9.5.0
```

* Rebuild the Docker image.
```shell
docker-compose down
docker-compose up -d --build
```

* Edit settings
```python
# django_project/settings.py

from pathlib import Path
from environs import Env

env = Env()
env.read_env()
```

* Update the docker-compose.yml file
``` docker-compose.yml
version: '3.9'
services:
  web:
    build: .
    command: python /code/manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - 8000:8000
    depends_on:
      - db
    environment:
      - "DJANGO_SECRET_KEY=django-insecure-iqdvmwfd$$=274106p+^45u(o#98wdj-bn@mgtb^ot=^sp7eve6"
  db:
    image: postgres:13
    volumes:
      - postgres_data:/var/lib/postgresql/data/
    environment:
      - "POSTGRES_HOST_AUTH_METHOD=trust"

volumes:
  postgres_data:
```

* Update the settings
```python
# django_project/settings.py
SECRET_KEY = env("DJANGO_SECRET_KEY")
```


* Restart the container
```shell
docker-compose down
docker-compose up -d
```

* Edit the settings again

```python
# django_project/settings.py
DEBUG = env.bool("DJANGO_DEBUG")
ALLOWED_HOSTS = ["localhost", "127.0.0.1"]
```

* Edit environment section in the docker-compose.yml file again.
``` docker-compose.yml
version: '3.9'
services:
  web:
    build: .
    command: python /code/manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - 8000:8000
    depends_on:
      - db
    environment:
      - "DJANGO_SECRET_KEY=django-insecure-iqdvmwfd$$=274106p+^45u(o#98wdj-bn@mgtb^ot=^sp7eve6"
      - "DJANGO_DEBUG=True"
  db:
    image: postgres:13
    volumes:
      - postgres_data:/var/lib/postgresql/data/
    environment:
      - "POSTGRES_HOST_AUTH_METHOD=trust"

volumes:
  postgres_data:
```

* Update the database configuration in the settings.
```python

# django_project/settings.py

DATABASES = {
    "default": env.dj_db_url("DATABASE_URL", default="postgres://postgres@db/postgres")
}
```

* Check it in your web browser: http://127.0.0.1:8000/
