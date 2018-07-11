## Django Setup

### Terminal Interface

Creating a new project:

`django-admin startproject {projectname}`

Running the server:

`python manage.py runserver`

Creating app directory:

`python manage.py startapp {app name}`

Including new models data in a migratation:

`python manage.py makemigrations {app name}`

Migrating data:

`python manage.py migrate`

### settings.py

When your project is created it will have a settings.py file that you should immediately open and make some adjustments to. Most importantly, if you have a project that you are adding to a public repository you should create a separate JSON file (or whatever you want to use as a config), and add that json file to your .gitignore file. Then change your settings.py file to look like this:

```
import json
secrets = json.load(open("{project name}/secrets.json"))

...

SECRET_KEY = secrets['SECRET_KEY']

```

To use data from apps you create you will need to add the following to the `INSTALLED_APPS` list:

`'{app name}.apps.DataConfig'`

By default Django uses SQLite as its database. Most likely you will want to use a different database so that should be configured in the settings file as well.
