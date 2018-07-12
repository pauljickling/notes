## Using the REST Framework

1. Enter your virtual environment and install `djangorestframework` in your Django project directory
2. Run the Django `startapp` command
3. In your settings.py file add `rest_framework` and your app config to the `INSTALLED_APPS` list
4. Create and define your models
5. Run migrations
6. Create a serializer.py file in your app directory. Per the documentation, using the ModelSerializer module your file will look like this:

```
class {Model Name}Serializer(serializers.ModelSerializer):
    class Meta:
        model = {Model Name}
        fields = ({Model keys})
```
7. Define your views.
8. Configure your urls.py files
