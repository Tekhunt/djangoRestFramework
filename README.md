# djangoRestFramework
## About DRF
The Django Rest Framework is a third-party package that empowers a Django app with REST API capabilities.

To install the package, run the command:

```
   pip install django-rest-framework
   
```

## Create a Sample App

This will be a Django app that displays data from the default Sqlite3 database. The data is a list of countries and their currency names.

Fire up a new Django project by running this command.

```
   django-admin startproject countries
   
```

Next, create an app and name it currency_country using this command.

```
   python3 manage.py startapp currency_country

```

The app is now set up. What remains is to develop the country model, the DRF API resources, and provide URLs to the API resources. The code block below shows the model and what fields make up the database table. Copy the code block into the models.py file.

```
from django.db import models

class Country(models.Model):
   country_name = models.CharField(max_length=20)
   local_currency = models.CharField(max_length=20)
   added_on = models.DateTimeField(auto_now_add=True)
   
   def __str__(self):
       return self.country_name

```

The model needs data. To add the data, access the admin panel that requires superuser access. The next step is to create a superuser account using the following command.

```
python manage.py createsuperuser

```

This will be an interactive process where you will give essential details and register your user.

For the app and model to be visible, they need to be registered with the Django project and admin.py. To register the app and rest framework, a third-party app, add their names to the list of installed apps in settings.py.

Copy the list below to your settings.py and replace the existing list.

```
   INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'currency_country',
    "rest_framework",
]

```

To ensure the Country model is visible on the admin panel, register it in the admin.py file.

Add the code block below to your admin.py file.

```
from django.contrib import admin
from .models import Country
admin.site.register(Country)

```

### DRF Serializers

DRF serializers convert Django data types, such as querysets, into a format that can be rendered into JSON or XML. For this app, you only need to create the Country serializer. In the currency_country app, create a file named serializers.py and add the code block below.

```
from rest_framework import serializers
from .models import Country


class CountrySerializer(serializers.ModelSerializer):
    class Meta:
        model = Country # this is the model that is being serialized
        fields = ('country_name', 'local_currency')
```

### API Requests

To service a GET or POST request, you need a view that will return all countries in a serialized fashion. To achieve this, create a view within the views.py file and add the view that will return all countries, serialized.

```
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Country
from .serializers import CountrySerializer


@api_view(['GET', 'POST'])
def country(request):
    
    if request.method == 'GET': # user requesting data 
        snippets = Country.objects.all()
        serializer = CountrySerializer(snippets, many=True)
        return Response(serializer.data)

    elif request.method == 'POST': # user posting data
        serializer = CountrySerializer(data=request.data)
        if serializer.is_valid():
            serializer.save() # save to db
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
        
```

### URL Configuration

To configure the URL, configure the main project's urls.py file to direct any traffic to the currency_country app using the path function, as in the codeblock below.

```
   from django.urls import path, include
from django.contrib import admin
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('currency_country.urls')),

]

```

Next, create a urls.py file and, using the path, direct the traffic to your view function.

```
   from django.urls import path
from .views import country


urlpatterns = [
    path('country/', country, name="countries")
]

```

### Run the App

When all has been set up, the final step is migrating the changes and running the server. To make migrations, run the command:

```
   python manage.py makemigrations
   
```

To migrate the model changes into the default database, run the command:

```
   python manage.py migrate
   
```

To start the server and run the app, run the command:

```
   python manage.py runserver
   
```

### Below is a sample API of countries and their respective currencies

![countryAPI](https://user-images.githubusercontent.com/65784601/131267942-67801f47-5ddc-49c4-8f85-913857f7df2f.png)

