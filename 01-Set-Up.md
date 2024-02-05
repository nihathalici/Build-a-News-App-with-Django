News App / 1. Set-Up
========================================================

* Make a new directory

```shell
mkdir newspaper
```

* Create a virtual environment within this new directory. 

```shell
python -m venv .venv
```

* Activate the virtual environment called .venv
```shell
source .venv/bin/activate
```

* Install Django & psycopg2-binary==2.9.3
```shell
pip install django~=4.0.0 psycopg2-binary==2.9.3
```

* Create a new Django project called django_project. 
```shell
django-admin startproject django_project .
```


* Run the Django development server
```shell
python manage.py runserver
```

* Stop the server with CTRL + C. Create a requirements.txt file.
```shell
pip freeze > requirements.txt
```