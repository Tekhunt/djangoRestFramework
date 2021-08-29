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
