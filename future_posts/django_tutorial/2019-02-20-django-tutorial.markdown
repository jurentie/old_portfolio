## Django Design:
---

*To see how to set up a general django development environment see this
gist [django_dev_env.md](https://gist.github.com/jurentie/bb682cff73a7ed242af07368cfcc65ed)*

This documentation covers basic Django design patterns and walks through
an example. This tutorial was developed in large part from the following
sources:
* [Tutorials Point](https://www.tutorialspoint.com/django/)
* [Writing your first Django app](https://docs.djangoproject.com/en/2.1/intro/tutorial01/)

### Basic Django Elements:
---
The following section briefly covers some basic elements that go into
creating a django project.

#### Project vs. Application:

- Each site is a Django *project*. Each project is made up of individual
*apps* including the main django project default application.
Apps can be reused between different projects so it is best practice
to keep all files and related content associated with that stand-alone
app in one directory. For example, it is best practice to create a `urls.py`
file within each app's directory that holds the url mappings files within
that app.

- To create a new project:
```
py manage.py startproject new_django_project
```

- To create a new app within a project:
```
py manage.py startapp myapp
```

#### Views:
- A *view* is a python function that takes a web request and returns a
web response. Views need to be associated to a URL to access it through
the site.
- *Python code goes in views*
- *HTML goes in templates*

#### Templates:
- Templates are where the html lives in a django project. Templates .
are able to be referenced and displayed from calling views and through
url mapping.
- It is possible to pass parameters to templates and use those parameters
with special template syntax. `{{ parameter_name }}`
- Templates also allow shortcuts for things such as `if`, `for`,
and `extends` statements.
- `extends` allows the inheritance of "parent" html classes that may contain
the basic HTML code that should be incorporated into all individual pages.
- Templates can also make use of regular pythonic attributes on
objects `{{ date.year }}`, and filters on parameters `{{ string | lowercase }}`.

#### Models:
- A model represents a table or a collection in a Database.
- A model is a convenient way to access database elements without having
to directly hard code `SQL` commands into your project.
- Each class in a model file represents a new element in a database
and can specify individual databases to hold the data.

#### Forms:
- Most websites make use of forms at some point. Django is useful for
authentication purposes and allows for easy customization of the forms.
- Custom forms can be created for other user input as well.
- For example, a form can be created for a new user to your site and can
allow custom fields for that user.

#### Example:
The example below demonstrates how the separate elements of a django project
described above piece together to form a django site. This will
create a static django site that allows a user to sign into a login form
and once authenticated see if their username is contained within a custom
database.

1. Create a project `new_django_project`:
```
py manage.py new_django_project
```
... Creates the following directory structure:  
```
├── new_django_project
|   ├── new_django_project
|   |   ├── __init__.py
|   |   ├── settings.py
|   |   ├── urls.py
|   |   ├── wsgi.py
|   ├── db.sqlite3
|   ├── manage.py
```
`new_django_project` subdirectory is technically considered an app in
this project.

2. Create a new app `myapp`:
```
py manage.py startapp myapp
```
... Creates the updated directory structure:
```
├── new_django_project
|   ├── myapp
|   |   ├── __init__.py
|   |   ├── admin.py
|   |   ├── apps.py
|   |   ├── migrations/
|   |   |   ├── __init__.py
|   |   ├── models.py
|   |   ├── tests.py
|   |   ├── views.py
|   ├── new_django_project
|   |   ├── __init__.py
|   |   ├── settings.py
|   |   ├── urls.py
|   |   ├── wsgi.py
|   ├── db.sqlite3
|   ├── manage.py
```
More files will be added to `myapp` as the project is further
developed.  
Once creating a new app it needs to be added to your project in
`new_django_project/settings.py` under the `INSTALLED APPS` variable add
the app's configuration reference found in the default file
`myapp/apps.py`:
```
INSTALLED_APPS = [
    ...,
    `myapp.apps.MyappConfig`,
]
```

3. Create a new model in the file `myapp/models.py`.
From the [tutorial](https://www.tutorialspoint.com/django/django_models.htm)
this model class is named `Dreamreal` which will contain basic data for
these model objects.
```python
# myapp/models.py
from django.db import models
class Dreamreal(models.Model):

    website = models.CharField(max_length = 50)
    mail = models.CharField(max_length = 50)
    name = models.CharField(max_length = 50)
    phonenumber = models.IntegerField()

    class Meta:
        db_table = "dreamreal"
```
This creates a new database table named `dreamreal` that will hold data  
for `Dreamreal` objects including website, mail, name, and phonenumber
columns for each new entry.   
Anytime a change is made in the models file it needs to be migrated to
update the database in the project.  
This command creates SQL commands for the database:  
```
py manage.py makemigrations
```
This command runs the SQL commands created by the previous command:  
```
py manage.py migrate
```
Now this project has a model
that allows the entry of `Dreamreal` objects to be entered into the
database.

4. The following is an example of how to use the
[django shell](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#playing-with-the-api),
these commands can also be put into a view in the `views.py` file if
desired and then ran through the browser, but this is a good opportunity
to explore the django shell and API. The code below will demonstrate CRUD
(Create, Read, Update, Delete) operations on a model object in the
database.  
Start the shell:  
```
$ py manage.py shell
Python 3.7.2 (tags/v3.7.2:9a3ffc0492, Dec 23 2018, 23:09:28) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
```
CRUD operations:  
```
>>> from myapp.models import Dreamreal
>>> # Create an entry...
>>> alex_h = Dreamreal(
... website='https://hamiltonmusical.com/new-york/',
... mail='alex@hamilton.com',
... name='alexanderhamilton',
... phonenumber='5551236789'
... )
>>> alex_h.save()
>>> # Read all entries...
>>> objects = Dreamreal.objects.all()
>>> res = 'Printing all Dreamreal entries in the DB: '
>>> for elt in objects:
...     res += elt.name + ','
...
>>> res
'Printing all Dreamreal entries in the DB: alexanderhamilton,'
>>> # Read a specific entry:
>>> alex = Dreamreal.objects.get(name='alexanderhamilton')
>>> res = 'Printing 1 entry: '
>>> res += alex.name
>>> res
'Printing 1 entry: alexanderhamilton'
>>> # Delete an entry
>>> d = Dreamreal.objects.all()
>>> d
<QuerySet [<Dreamreal: Dreamreal object (2)>]>
>>> alex.delete()
(1, {'myapp.Dreamreal': 1})
>>> d
<QuerySet []>
>>> # Update entries
>>> alex_h = Dreamreal(
... website='https://hamiltonmusical.com/new-york/',
... mail='alex@hamilton.com',
... name='alexanderhamilton',
... phonenumber='5551236789'
... )
>>> alex_h.save()
>>> alex = Dreamreal.objects.get(name='alexanderhamilton')
>>> alex.name='alexh'
>>> alex.save()
>>> exit()
```
After the following operations the table `dreamreal` will contain one
entry of a `Dreamreal` object with the values
website='https://hamiltonmusical.com/new-york/', mail='alex@hamilton.com',
name='alexh', phonenumber='5551236789'.

4. Django allows customization of forms. Below is an example of a customized
form, requesting a username and password. If desired additional parameters
may be added. Create the file `forms.py` under `myapp`
directory. This is used in the `login()` view below.

```python
from django import forms
from .models import Dreamreal
class LoginForm(forms.Form):
    username = forms.CharField(max_length = 100)
    password = forms.CharField(widget = forms.PasswordInput())
```

5. The code below creates a view called `login` that retrieves data
via a `POST` request and, using some logic, allows the user to login
as long as they fill in both the username and password fields. It then
also checks the database created above to see if the user can be found
in `dreamreal`. These parameters are then passed to `loggedin.html` template.

```python
# myapp/views.py
from django.shortcuts import render
from myapp.forms import LoginForm
from myapp.models import Dreamreal

import datetime

def login(request):
    username = 'not logged in'

    if request.method == 'POST':
        # Get the posted form
        MyLoginForm = LoginForm(request.POST)

        if MyLoginForm.is_valid():
            username = MyLoginForm.cleaned_data['username']

    else:
        MyLoginForm = LoginForm()

    res = ''

    # Filtering data:
    db_user = Dreamreal.objects.filter(name=username)

    user_exists = db_user.exists()

    return render(request, 'loggedin.html', {'username':username, 'user_exists':user_exists})
```

6. Now create a couple templates that will create the HTML display for
the django site.
It is standard practice to add all templates in their own directory
named `templates` under `myapp`. Create this directory and add the
following templates to it.  

This first file will be the parent template that will create a simple
html header to be inherited by all other templates.
```html
{% raw %}
<!-- myapp/templates/basic.html -->
<!-- myapp/templates/base.html -->
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>{% block title %}Django Tutorial{% endblock %}</title>
</head>
<body>
    <div class="header">
        <h1>Header</h1>
        <p>My supercool header</p>
    </div>
    <main>
        {% block content %}
        {% endblock %}
    </main>
</body>
</html>
{% endraw %}
```
The following files make use of inheriting elements from `basic.html`
and show how different template parameters can work to insert the
child content into the parent using `block content` and `endblock`
```HTML
<!-- myapp/templates/login.html -->
<!-- myapp/templates/login.html -->
{% raw %}
{% extends 'basic.html'%}
{% block content %}
<form name="form" action="{% url 'login' %}"
      method="POST">
    {% csrf_token %}
    <div style="max-width:470px;">
        <center>
            <input type="text" style="margin-left:20%;"
                   placeholder="username" name="username" />
        </center>
    </div>
    <br>
    <div style="max-width:470px;">
        <center>
            <input type="password" style="margin-left:20%;"
                   placeholder="password" name="password" />
        </center>
    </div>
    <br>
    <div style="max-width:470px;">
        <center>
            <button style="border:0px; background-color:#4285F4; margin-top:8%;
                               height:35px; width:80%; margin-left:19%" type="submit"
                    value="Login">
                <strong>Login</strong></button>
        </center>
    </div>
</form>
{% endblock %}
{% endraw %}
```

```HTML
<!--- myapp/templates/loggedin.html -->
{% raw %}
{% extends 'basic.html' %}
{% block content %}
<p>You are: <strong>{{username}}</strong></p>
{% if user_exists %}
<p>User recognized in database</p>
{% else %}{% endraw %}
<p>User not found in database</p>
{% endif %}
{% endblock %}
{% endraw %}
```

7. Right now the header is pretty ugly. If wanting to add style to a
template using a .css stylesheet create the directory `myapp/static/css/main.css`.
Django has defaults to handle importing static files into templates.  

```css
/* myapp/static/css/main.css */

.header {
  padding: 10px;
  text-align: center;
  background: #1abc9c;
  color: white;
  font-size: 12px;
  width:30%
}
```
After creating a stylesheet file set some parameter in `new_django_project/settings.py`:
```
# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/2.1/howto/static-files/

STATIC_URL = '/static/'
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static"),
]
```
Import static files in templates by adding `{% load static %}` to the top
of `basic.html`. Add the dynamic path to the head of `basic.html`.
```html
<link rel="stylesheet" type="text/css" href="{% static 'css/main.css' %}">
```

7. To put it all together map the URL's of the project site so that
all elements can be accessed. Each project's main application contains
a `urls.py` file and a similar `urls.py` file should be created in each
applications directory, to contain all the URL mapping for that specific app.
First link the main project to the application `myapp`.
```python
# new_django_project/urls.py
from django.contrib import admin
from django.urls import include, path
urlpatterns = [
    path('admin/', admin.site.urls),
    path('myapp/', include('myapp.urls')),
]
```
Now create the file `urls.py` in `myapp` that will be used to map the
URLs within the application, specifically.
```python
# myapp/urls.py
from django.urls import path
from django.views.generic import TemplateView
from . import views
urlpatterns = [
   path('connection/',TemplateView.as_view(template_name = 'login.html')),
   path('login/', views.login, name='login'),
]
```
Start up the server running `py manage.py runserver` and navigate to the url localhost:8000/myapp/connection and it should show the following:  
![login](images/login.png)  
The page above is being displayed through a series of interconnected elements that has been defined above. The main application maps the path `myapp/` and the `myapp` `urls.py` file further maps `connection/` to the login page. Now if any information is entered into both the username and password fields the logged in page will be displayed.  
![logged in jurentie](images/loggedin_jurentie.png)  
Again, this page is being displayed using a series of elements. Once all of the URL mapping takes place, the form in `loggedin.html` specifies `action="{% url 'login' %}"` to reroute the application to `myapp/login/` using the name specified in the urlpatters above. Django handles the logic and calls views.login after the form is submitted. Data is processed and the function looks up the user to see if the exist in `Dreamreal`. Above we can see that 'jurentie' is not in the database, but if we enter the user 'alexh':  
![logged in alexh](images/loggedin_alexh.png)  
