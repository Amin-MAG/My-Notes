# Django

## Settings

The `project_name/settings.py` is the main file for your project configuration.

### Installed Apps

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # My apps
    'meetups',
    'osm'
]
```

### Database configurations

```python
DATABASES = {
		# For postgresql
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'gisdb',
        'USER': 'admin',
        'PASSWORD': 'admin',
        'HOST': 'localhost',
        'PORT': '5432',
    }
    # 'sqlite3': {
    #     'ENGINE': 'django.db.backends.sqlite3',
    #     'NAME': BASE_DIR / 'db.sqlite3',
    # }
}
```

## URLs

### Project folder urls.py

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('meetups/', include('meetups.urls')),
    path('osm/', include('osm.urls'))
]
```

### Apps urls.py

## Requests

Here we print out the important attributes of request object:

```python
def add_driver(request):
    print("request.scheme", request.scheme)
    print("request.body", request.body)
    print("request.path", request.path)
    print("request.path_info", request.path_info)
    print("request.method", request.method)
    print("request.encoding", request.encoding)
    print("request.content_params", request.content_params)
    print("request.COOKIES", request.COOKIES)
    print("request.GET", request.GET)
    print("request.POST", request.POST)
    print("request.FILES", request.FILES)
    print("request.META", request.META)
    print("request.headers", request.headers)
    return HttpResponse()
```

The request and the result:

```bash
# Request
curl --request GET \
  --url 'http://localhost:8000/osm/driver/add?query_param1=test1' \
  --header 'Content-Type: application/json' \
  --header 'header_test: header_value' \
  --data '{
	"name": "ahmad",
	"age": 32
}'

# Result
request.scheme http
request.body b'{\n\t"name": "ahmad",\n\t"age": 32\n}'
request.path /osm/driver/add
request.path_info /osm/driver/add
request.method GET
request.encoding None
request.content_params {}
request.COOKIES {}
request.GET <QueryDict: {'query_param1': ['test1']}>
request.POST <QueryDict: {}>
request.FILES <MultiValueDict: {}>
request.META {'CLICOLOR': '1', 'COLORFGBG': '7;0', 'COLORTERM': 'truecolor', 'COMMAND_MODE': 'unix2003', 'GOPATH': '/Users/snapp/go', 'HOME': '/Users/snapp', 'ITERM_PROFILE': 'Default', 'ITERM_SESSION_ID': 'w0t1p0:4987AA66-521A-4CEB-A271-5D76339B3767', 'LANG': 'en_US.UTF-8', 'LC_TERMINAL': 'iTerm2', 'LC_TERMINAL_VERSION': '3.4.10', 'LOGNAME': 'snapp', 'LSCOLORS': 'GxFxCxDxBxegedabagaced', 'OLDPWD': '/Users/snapp/django_course_site', 'PATH': '/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin:/Users/snapp/go/bin:/Users/snapp/go/bin', 'PWD': '/Users/snapp/django_course_site', 'SHELL': '/bin/zsh', 'SHLVL': '2', 'SSH_AUTH_SOCK': '/private/tmp/com.apple.launchd.rJZB5UxfEy/Listeners', 'TERM': 'screen', 'TERM_PROGRAM': 'tmux', 'TERM_PROGRAM_VERSION': '3.2a', 'TERM_SESSION_ID': 'w0t1p0:4987AA66-521A-4CEB-A271-5D76339B3767', 'TMPDIR': '/var/folders/4w/x4v2y15j4159tjj1mkkkzqhh0000gn/T/', 'TMUX': '/private/tmp/tmux-501/default,1392,2', 'TMUX_PANE': '%3', 'USER': 'snapp', 'XPC_FLAGS': '0x0', 'XPC_SERVICE_NAME': '0', '__CFBundleIdentifier': 'com.googlecode.iterm2', '__CF_USER_TEXT_ENCODING': '0x1F5:0x0:0x0', '_': '/usr/local/bin/python3', 'DJANGO_SETTINGS_MODULE': 'django_course_site.settings', 'TZ': 'UTC', 'RUN_MAIN': 'true', 'SERVER_NAME': '1.0.0.127.in-addr.arpa', 'GATEWAY_INTERFACE': 'CGI/1.1', 'SERVER_PORT': '8000', 'REMOTE_HOST': '', 'CONTENT_LENGTH': '32', 'SCRIPT_NAME': '', 'SERVER_PROTOCOL': 'HTTP/1.1', 'SERVER_SOFTWARE': 'WSGIServer/0.2', 'REQUEST_METHOD': 'GET', 'PATH_INFO': '/osm/driver/add', 'QUERY_STRING': 'query_param1=test1', 'REMOTE_ADDR': '127.0.0.1', 'CONTENT_TYPE': 'application/json', 'HTTP_HOST': 'localhost:8000', 'HTTP_USER_AGENT': 'insomnia/2021.5.2', 'HTTP_ACCEPT': '*/*', 'wsgi.input': <django.core.handlers.wsgi.LimitedStream object at 0x11019bdc0>, 'wsgi.errors': <_io.TextIOWrapper name='<stderr>' mode='w' encoding='utf-8'>, 'wsgi.version': (1, 0), 'wsgi.run_once': False, 'wsgi.url_scheme': 'http', 'wsgi.multithread': True, 'wsgi.multiprocess': False, 'wsgi.file_wrapper': <class 'wsgiref.util.FileWrapper'>}
request.headers {'Content-Length': '32', 'Content-Type': 'application/json', 'Host': 'localhost:8000', 'User-Agent': 'insomnia/2021.5.2', 'Accept': '*/*'}
```

## Manage.py

It contains some subcommands that can help you when developing your application.

```bash
# To run the server in development mode
python3 manage.py runserver

# To run shell!
python3 manage.py shell

# For migrations, to make and apply migrations
# You can not to specify the app name, then it will migrate for all
python3 manage.py makemigrations
python3 manage.py migrate

# Create super user for admin panel
python3 manage.py createsuperuser

```

## Models

[Models | Django documentation | Django](https://docs.djangoproject.com/en/3.2/topics/db/models/)

### Meta in models

Model Meta is basically used to change the behavior of your model fields like changing order options.

For example for ordering your model you should Add a sub class `Meta` for your model.

```python
class Car(models.Model):
    model = models.TextField()
    year = models.TextField()
    owner = models.ForeignKey("Driver", on_delete=models.SET_NULL, null=True)
    created = models.DateTimeField(auto_now_add=True)

    class Meta:
        ordering = ['created']
```

There are all of the options here

[Model Meta options | Django documentation | Django](https://docs.djangoproject.com/en/3.2/ref/models/options/)

# Django REST Framework

## Serialization

The first thing we need to get started on our Web API is to provide a way of serializing and deserializing the snippet instances into representations such as JSON.

### Types of serializers

- `serializers.Serializer`:
- `BaseSerializer` class that can be used to easily support alternative serialization and deserialization styles. [https://www.django-rest-framework.org/api-guide/serializers/#baseserializer](https://www.django-rest-framework.org/api-guide/serializers/#baseserializer)
- `serializers.ModelSerializer`: Often you'll want serializer classes that map closely to the Django model definitions.
- `serializers.HyperlinkedModelSerializer`: This is similar to the ModelSerializer class except that it uses hyperlinks to represent relationships, rather than primary keys.
- `serializer.ListSerializer`: class provides the behavior for serializing and validating multiple objects at once. You won't typically need to use ListSerializer directly, but should instead simply pass many=True when instantiating a serializer.

In `HyperlinkedModelSerializer` and  `ModelSerializer` you will have a `Meta` subclass to specify the `model` & `fields` that you want.

There are also other third party serializers [https://www.django-rest-framework.org/api-guide/serializers/#third-party-packages](https://www.django-rest-framework.org/api-guide/serializers/#third-party-packages)

## Request & Response

Request:

`request.POST`: Only handles form data.  Only works for the 'POST' method.
`request.data`: Handles arbitrary data.  Works for 'POST', 'PUT' and 'PATCH' methods.

Response:

- You can set the status code.

```python
Response(serializer.data, status=status.HTTP_201_CREATED)
```

## Class-based views

### Mixins

### Generics

## Permissions

We can specify the permissions in view classes.

```python
# Create your views here.
class DriverViewSet(viewsets.ModelViewSet):
    queryset = Driver.objects.all()
    serializer_class = DriverSerializer
    permission_classes = [AllowAny]
```

We can also create our own kind of permission.

```python
from rest_framework import permissions

class IsOwnerOrReadOnly(permissions.BasePermission):

    def has_object_permission(self, request, view, obj):
        # Read permissions are allowed to any request,
        # so we'll always allow GET, HEAD or OPTIONS requests.
        if request.method in permissions.SAFE_METHODS:
            return True

        # Write permissions are only allowed to the owner of the snippet.
        return obj.owner == request.user
```
