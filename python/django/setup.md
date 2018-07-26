## Django Setup

### Terminal Interface

Creating a new project:

`django-admin startproject {projectname}`

Running the server:

`python manage.py runserver`

Creating app directory:

`python manage.py startapp {app name}`

Including any changes or adjustments made in the models:

`python manage.py makemigrations {app name}`

Applying migration changes:

`python manage.py migrate`

Listing migrations and their status:

`python manage.py showmigrations`

Displaying SQL statements for a migration:

`python manage.py sqlmigrate`

Create an admin superuser:

`python manage.py createsuperuser`

To get a full list of commands available for your django project you can type `python manage.py`

### settings.py

When your project is created it will have a settings.py file that you should immediately open and make some adjustments to. Most importantly, if you have a project that you are adding to a public repository you should create a separate JSON file (or whatever you want to use as a config), and add that json file to your .gitignore file. Then change your settings.py file to look like this:

```
import json
secrets = json.load(open("{project name}/secrets.json"))

...

SECRET_KEY = secrets['SECRET_KEY']

```

To use data from apps you create you will need to add the following to the `INSTALLED_APPS` list:

`'{app name}.apps.{App Name}Config'`

By default Django uses SQLite as its database. Most likely you will want to use a different database so that should be configured in the settings file as well.

You should also setup your static file directory so you can serve css, javascript, and image files. To do this, create a static directory in your root directory, and then include the following code in your settings:
```
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
    '/var/www/static/',
]
```

Then, if you wanted to include a file called main.css, you would write something like this in your template file:

```
{% load static %}
<link rel="stylesheet" href="{% static 'main.css' %}">
```
