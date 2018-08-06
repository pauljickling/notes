## Using the Django REST Framework

1. Enter your virtual environment and install `djangorestframework` in your Django project directory
2. Run the Django `startapp` command
3. In your settings.py file add `rest_framework` and your app config to the `INSTALLED_APPS` list
4. Create and define your models
5. Run migrations
6. Create a serializer.py file in your app directory. [Per the documentation](http://www.django-rest-framework.org/api-guide/serializers/#modelserializer), using the ModelSerializer module your file will look like this:

```
class {Model Name}Serializer(serializers.ModelSerializer):
    class Meta:
        model = {Model Name}
        fields = ({Model keys})
```
7. Define your views.
8. Configure your urls.py files

### API Wrappers

The REST framework provides several different API view wrappers:

1. The `@api_view` decorator for function-based views.
2. The `APIView` class for class-based views.
3. The `ViewSet` class that provides CRUD operations instead of HTTP method handlers.
