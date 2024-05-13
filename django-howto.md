# Django Howto

## Manage Virtual Environment

### Ensure Virtual Environment

```
$ which python3
$ python3 --version
$ sudo apt update
$ sudo apt install python3-venv
```

### Create Vritual Environment

```
$ cd </path/to/workspace>
$ python3 -m venv <venv-name>
```

### Activate Virtual Environment

```
$ cd </path/to/workspace>
$ source <venv-name>/bin/activate
(venv-name)$ which python3
(venv-name)$ which pip3
(venv-name)$ which python
(venv-name)$ which pip
```

### Deactivate Virtual Environment

```
(venv-name)$ deactivate
```

## Install Django

```
(venv-name)$ pip install django
(venv-name)$ django-admin --version
```

## Manage Django Project and Apps

### Craft New Project

```
(venv-name)$ cd </path/to/proj-dir>
(venv-name)$ django-admin startproject <proj-name>
(venv-name)$ cd <proj-name>
(venv-name)$ vi <proj-name/>settings.py
ALLOWED_HOSTS = ["192.168.56.131"]
(venv-name)$ python manage.py runserver 0.0.0.0:8000
(venv-name)$ curl http://0.0.0.0:8000/
```

```
(venv-name)$ cd </path/to/proj-dir>
(venv-name)$ mkdir <proj-name>
(venv-name)$ cd <proj-name>
(venv-name)$ django-admin startproject <proj-name> .
(venv-name)$ vi <proj-name/>settings
ALLOWED_HOSTS = ["192.168.56.131"]
(venv-name)$ python manage.py runserver 0.0.0.0:8000
(venv-name)$ curl http://0.0.0.0:8000/
```

### Create New App

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ python manage.py startapp <app-name>
```

### Register New App

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <proj-name/>settings.py
# Application definition
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    '<app-name>',
]
```

## Manage Data Model for App

### Create Data Model

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <app-name/>models.py
# Create your models here.
class Snippet(models.Model):
    title = models.CharField(max_length=100)
    code = models.TextField()
    language = models.CharField(max_length=50)
    created_at = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.title
```

### Migrate Data Model

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ python manage.py makemigrations <app-name>
(venv-name)$ python manage.py migrate
```

### Register Model with Admin Interface

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <app-name/>admin.py
# Register your models here.
from .models import Snippet
admin.site.register(Snippet)
(venv-name)$ python manage.py runserver 0.0.0.0:8000
(venv-name)$ curl http://0.0.0.0:8000/admin
```

## Setup Account with Admin Interface

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ python manage.py createsuperuser
(venv-name)$ python manage.py runserver 0.0.0.0:8000
(venv-name)$ curl http://0.0.0.0:8000/admin
```