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

## Install and Setup Django

### Install Django

```
(venv-name)$ pip install django
(venv-name)$ django-admin --version
```

### Setup Account with Admin Interface

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ python manage.py createsuperuser
(venv-name)$ python manage.py runserver 0.0.0.0:8000
(venv-name)$ curl http://0.0.0.0:8000/admin
```

### Integrate with PostgreSQL

```
(venv-name)$ pip install psycopg2
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <proj-name>/settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': '<proj-db>',
        'USER': '<db_username>',
        'PASSWORD': '<db_password>',
        'HOST': '<db_host>', # e.g. 'localhost'
        'PORT': '', # Leave as an empty string to use the default port.
    }
}
(venv-name)$ python manage.py migrate
(venv-name)$ python manage.py runserver 0.0.0.0:8000
```

## Manage Django Project and Apps

### Craft New Project

```
(venv-name)$ cd </path/to/proj-dir>
(venv-name)$ django-admin startproject <proj-name>
(venv-name)$ cd <proj-name>
(venv-name)$ vi <proj-name>/settings.py
ALLOWED_HOSTS = ["192.168.56.131"]
(venv-name)$ python manage.py runserver 0.0.0.0:8000
(venv-name)$ curl http://0.0.0.0:8000/
```

```
(venv-name)$ cd </path/to/proj-dir>
(venv-name)$ mkdir <proj-name>
(venv-name)$ cd <proj-name>
(venv-name)$ django-admin startproject <proj-name> .
(venv-name)$ vi <proj-name>/settings
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
(venv-name)$ vi <proj-name>/settings.py
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
(venv-name)$ vi <app-name>/models.py
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
(venv-name)$ vi <app-name>/admin.py
# Register your models here.
from .models import Snippet
admin.site.register(Snippet)
(venv-name)$ python manage.py runserver 0.0.0.0:8000
(venv-name)$ curl http://0.0.0.0:8000/admin
```

### Customize Model Display in Admin

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <app-name>/admin.py
# Register your models here.
from .models import Snippet

@admin.register(Snippet)
class SnippetAdmin(admin.ModelAdmin):
    list_display = ('title', 'language', 'created_at')
    list_filter = ('language',)
    search_fields = ('title', 'code')
```

### Customize Forms in Admin

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <app-name>/admin.py
from django import forms
from django.contrib import admin

from .models import Snippet

class SnippetAdminForm(forms.ModelForm):
    class Meta:
        model = Snippet
        fields = '__all__'
        help_texts = {
            'code': 'Enter your code snippet here.',
        }

@admin.register(Snippet)

class SnippetAdmin(admin.ModelAdmin):
    list_display = ('title', 'language', 'created_at')
    list_filter = ('language',)
    search_fields = ('title', 'code')
    form = SnippetAdminForm

    fieldsets = (
        (None, {
            'fields': ('title', 'language')
        }),
        ('Content', {
            'fields': ('code',),
            'description': 'Section for the code snippet itself.'
        }),
    )
```

## Manage View for App

### Create View

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <app-name>/views.py
from django.shortcuts import render
from django.http import HttpResponse

from .models import Snippet

def snippet_list(request):
    """A view to display a list of code snippets."""
    snippets = Snippet.objects.all() # query database
    snippets_list = ','.join([snippet.title for snippet in snippets])
    return HttpResponse(f"List of Snippets: {snippets_list}")
```

### Define URL Pattern for View

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ vi <proj-name>/urls.py
from django.contrib import admin
from django.urls import path

from snippets.views import snippet_list
urlpatterns = [
    path('admin/', admin.site.urls),
    path('snippets/', snippet_list, name='snippet_list'),
]
```

## Render Data with Templates

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ mkdir -p <app-name>/templates/snippets
```

### Create Template

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ mkdir -p <app-name>/templates/snippets
(venv-name)$ vi <app-name>/templates/snippets/snippet_list.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Snippet List</title>
  </head>

  <body>
    <h1>List of Snippets</h1>
    <ul>
      {% for snippet in snippets %}
      <li>{{ snippet.title }}: {{ snippet.code }}</li>
      {% empty %}
      <li>No snippets found.</li>
      {% endfor %}
    </ul>
  </body>
</html>
```

### Use Template in View

```
(venv-name)$ vi <app-name>/views.py
from django.shortcuts import render

from .models import Snippet

def snippet_list(request):
    """A view to display a list of code snippets."""
    snippets = Snippet.objects.all()
    return render(request, 'snippets/snippet_list.html', {'snippets': snippets})
```

### Use Form in View

```
(venv-name)$ cd </path/to/proj-name>
(venv-name)$ mkdir -p <app-name>/templates/snippets

(venv-name)$ vi <app-name>/forms.py
from django import forms
from .models import Snippet

class SnippetForm(forms.ModelForm):
    class Meta:
        model = Snippet
        fields = ['title', 'code', 'language']

(venv-name)$ vi <app-name>/views.py
from django.shortcuts import render, redirect

from .models import Snippet
from .forms import SnippetForm

def snippet_list(request):
    """A view to display a list of code snippets."""
    snippets = Snippet.objects.all()
    return render(request, 'snippets/snippet_list.html', {'snippets': snippets})

def submit_snippet(request):
    if request.method == 'POST':
        form = SnippetForm(request.POST)
        if form.is_valid():
            form.save()
            return redirect('snippet_list')
    else:
        form = SnippetForm()
        return render(request, 'snippets/submit_snippet.html', {'form': form})

(venv-name)$ vi <app-name>/templates/snippets/submit_snippet.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>Submit Snippet</title>
  </head>

  <body>
    <h1>Submit a New Snippet</h1>
    <form method="post">
      {% csrf_token %}
      {{ form.as_p }}
      <button type="submit">Submit</button>
    </form>
  </body>
</html>

(venv-name)$ vi <proj-name>/urls.py
from django.contrib import admin
from django.urls import path

from cmif.views import snippet_list, submit_snippet
urlpatterns = [
    path('admin/', admin.site.urls),
    path('snippets/', snippet_list, name='snippet_list'),
    path('submit/', submit_snippet, name='submit_snippet'),
]

(venv-name)$ curl http://0.0.0.0:8000/submit/
```

## Manage Complex Model Relationships

### OneToOneField Relationship

```
(venv-name)$ vi <app-name>/models.py
from django.conf import settings
from django.db import models

class UserProfile(models.Model):
    user = models.OneToOneField(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    bio = models.TextField()
```

### ForeignKey(Many-to-One) Relationship

```
(venv-name)$ vi <app-name>/models.py
class Snippet(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE, related_name='snippets')
    title = models.CharField(max_length=100)
    code = models.TextField()
```

### ManyToManyField Relationship

```
(venv-name)$ vi <app-name>/models.py
class Tag(models.Model):
   name = models.CharField(max_length=30)
   snippets = models.ManyToManyField(Snippet, related_name='tags')
```

#### GenericForeignKey Relationship

```
from django.contrib.contenttypes.fields import GenericForeignKey
from django.contrib.contenttypes.models import ContentType
from django.db import models

# A single comment model can associate with any model
# ContentType refers to Django's ContentType framework
class Comment(models.Model):
    content_type = models.ForeignKey(ContentType, on_delete=models.CASCADE)
    object_id = models.PositiveIntegerField()
    content_object = GenericForeignKey('content_type', 'object_id')
    text = models.TextField()
```

## Define Custom QuerySet and Custom Model Manager

```
(venv-name)$ vi <app-name>/models.py
from django.db import models

class SnippetQuerySet(models.QuerySet):
    def featured(self):
        return self.filter(is_featured=True)

    def by_language(self, language):
        return self.filter(language=language)

    def short_snippets(self, max_length=100):
        return self.filter(length__lte=max_length)

class SnippetManager(models.Manager):
    def get_queryset(self):
        return SnippetQuerySet(self.model, using=self._db)

    def featured(self):
        return self.get_queryset().featured()

    def by_language(self, language):
        return self.get_queryset().by_language(language)

    def short_snippets(self, max_length=100):
        return self.get_queryset().short_snippets(max_length)

class Snippet(models.Model):
    # model fields go here

    objects = SnippetManager()
```

## Utilize Signals for Model Changes

Django includes a set of built-in signals that send notifications when certain actions occur. The most commonly used signals are pre_save, post_save, pre_delete, and post_delete, which are dispatched before or after a model's save and delete methods are called, respectively.

Signals can be used for more complex operations beyond logging, such as automatically creating related records, sending emails, or invalidating caches. The power of signals lies in their ability to execute additional logic in response to changes in your database, all while keeping your models' code clean and focused on their primary responsibilities.

```
from django.db.models.signals import post_save
from django.dispatch import receiver

from .models import Snippet

@receiver(post_save, sender=Snippet)
def snippet_created(sender, instance, created, **kwargs):
    if created:
        print(f"New snippet created: {instance.title}")
```

## Implement Soft Deletion in Model

### Extend Model to Support Soft Deletion

```
from django.db import models

class Snippet(models.Model):
    title = models.CharField(max_length=100)
    code = models.TextField()
    language = models.CharField(max_length=50)
    created_at = models.DateTimeField(auto_now_add=True)
    is_deleted = models.BooleanField(default=False)

    def delete(self, *args, **kwargs):
        self.is_deleted = True
        self.save()

    def __str__(self):
        return self.title
```

### Customize Model Manager to Exclude Soft Deleted Records

Snippet.objects.all() will return only non-deleted records.

```
class SnippetManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(is_deleted=False)

class Snippet(models.Model):
    # Model fields as defined previously...
    title = models.CharField(max_length=100)
    code = models.TextField()
    language = models.CharField(max_length=50)
    created_at = models.DateTimeField(auto_now_add=True)
    is_deleted = models.BooleanField(default=False)

    objects = SnippetManager()

    def delete(self, *args, **kwargs):
        self.is_deleted = True
        self.save()

    def __str__(self):
        return self.title
```

### Retrieve Soft Deleted Records

Snippet.all_objects.all() will include both deleted and non-deleted records.

```
class AllSnippetsManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset()

class Snippet(models.Model):
    # Model fields and the default manager as defined previously...
    title = models.CharField(max_length=100)
    code = models.TextField()
    language = models.CharField(max_length=50)
    created_at = models.DateTimeField(auto_now_add=True)
    is_deleted = models.BooleanField(default=False)

    all_objects = AllSnippetsManager()
```

## Maintain Data Integrity

Ensuring data integrity is all about keeping data accurate, consistent, and reliable at all times. Multiple mechanisms in Django, such as model constraints, transaction management, and careful handling of model relationships, allow for this to be accomplished.

### Use Model Field Options

Django models offer a variety of field options that can be used to enforce data integrity at the database level. For instance, the unique attribute ensures that no two records in a table have the same value for a specific field. Additionally, null and blank can control whether a field can be empty, and choices limit the values that a field can accept.

```
class Snippet(models.Model):
    LANGUAGE_CHOICES = [
        ('PY', 'Python'),
        ('JS', 'JavaScript'),
        ('HTML', 'HTML'),
    ]

    title = models.CharField(max_length=100, unique=True)
    language = models.CharField(max_length=50, choices=LANGUAGE_CHOICES)
```

### Implement Custom Validators

```
from django.core.exceptions import ValidationError

def validate_code(value):
    if "import" in value:
        raise ValidationError("Code cannot contain import statements.")

class Snippet(models.Model):
    code = models.TextField(validators=[validate_code])
```

### Utilize Transaction Management

Here, the creation of a snippet and its associated tags is atomic, ensuring that either all operations succeed or none at all, preserving database integrity.
```
from django.db import transaction

def create_snippet_with_tags(title, code, tags):
    with transaction.atomic():
        snippet = Snippet.objects.create(title=title, code=code)
        for tag in tags:
            snippet.tags.add(tag)
```

### Overide  Save and Delete Methods

This is useful for implementing custom logic before saving or deleting an object, such as validation checks or cleanup of related data.

```
class Snippet(models.Model):
    # Model fields as defined previously...

    def save(self, *args, **kwargs):
        # Custom logic before saving...
        
        super().save(*args, **kwargs)

        # Custom logic after saving...
```

## Optimize Query and Index

### Add Index to Model

```
class Snippet(models.Model):
    title = models.CharField(max_length=100, db_index=True)
    # Other fields remain unchanged
```

### Use Meta Options for Compount Indexes

```
class Snippet(models.Model):
    # Model fields...
    
    class Meta:
        indexes = [
            models.Index(fields=['title', 'language']),
        ]
```

### Optimize Query with `select_related` and `prefetch_related`\

For ForeignKey relationships, `select_related` can be used to perform a SQL join and fetch related objects in a single query. Meanwhile, `prefetch_related` is used for ManyToMany and reverse ForeignKey relationships, fetching related objects in a separate query and joining them in Python, which can be more efficient than multiple database hits.

```
# Assuming a 'user' ForeignKey
snippets = Snippet.objects.select_related('user').all()

# Assuming a 'tags' ManyToManyField
snippets = Snippet.objects.prefetch_related('tags').all()
```
