# Todo WebApp in Django

When we're in a struggle of keeping track of the things that we want to do, it makes more sense to build a Django to-do list manager!

On this step-by-step process wherein we will be reviewing Django foundational concepts, we will be learning how Django can integrate with a Database that stores all our to-do items in lists that you can define. Each item will have a *title*, a *description*, and a *deadline*. With this app, we can manage our own deadlines and help our entire team stay on track!

Learning nuggets to be found as we finish through the project:

1. Create a *web app* using Django.
2. Build a *data model* with *one-to-many relationships*.
3. Use the *Django admin* interface to explore our data model and add test data.
4. Design templates for displaying your lists.
5. Leverage *class-based views* to handle the standard database operations
6. Control the Django *URL dispatcher* by creating *URL configurations*

## Project Overview

We will be designing our data model that represents the relationships between to-do items and lists. We will use Django's built-in object-relational mapping tool to automatically generate the database and tables that'll support this model.

We will be able to use Django's handy runserver command whenever you need to verify that things are working as expected. Django has also a built-in ready-made admin interface. In designing our web pages to display our app, we will be using templates in which these are skeleton HTML pages that can be populated with real application data.

Templates aren't meant to provide much logic, such as deciding which template to display and what data to send it. To perform that logic, you'll need views. *Django's views are the natural home for the application's logic.*

We will code views and templates for list creation and updates, as well as for the items that those lists will contain. We will be able to learn how to use *Django's URL dispatcher* to connect our pages and pass them the data that they need. Next, we'll add more views and templates that enable our users to delete lists and items.

After adding our views and templates for our list creation functionality. We will test our new user interface by adding, editing, and deleting to-do lists and to-do items.

### Creating our Django To-Do App

We will first be utilizing Django's tooling to perform a few project-specific steps. These include:

* Generating the parent project framework
* Creating the web app's framework
* Integrating the web app into the project

```[python]
(todo-web-app-dev) C:\Users\Clarence Vinzcent\Real-World-Python\ToDo-WebApp\todo_list> django-admin startproject todo_project .
```

### Scaffold the Parent Project

A python module is a single source file, and a *package* is a container for a bunch of modules. By organizing modules inside packages, we can reduce name pollution and improve code isolation.

Two of the files that are inside todo_list/todo_project/ are important for our application:

* *settings.py* ---> holds the project-wide configuration. This includes a list of all the apps that the project knows about, as well as a setting that describes which database it'll use.
* *urls.py* ---> has a list of all the URLs that the server must listen for.

### Getting Started on our Django To-Do list App

```[python]
(todo-web-app-dev) C:\Users\Clarence Vinzcent\Real-World-Python\ToDo-WebApp\todo_list>django-admin startapp todo_app
```

* The two __init__.py files indicate that the folders are contained as packages.
* The migrations/ subfolder will hold information about changes to our future database.
* The models.py file will define the data model for our app.
* The views.py file will handle the logc controlling the app display.

We will also create a few more files which includes a *data model*, new *views*, and new *templates*.

```[python]
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'todo_app',  # Adding our Todo App
]
```

In Django, SQLite3 database is setup by default for every new project that you create.

Important notes to take into consideration:

* Setting our secret key on a separate file like a config file to store our secret key / credentials safely
* *DEBUG* is a very useful setting while we're developing an app, but we should *be sure to set it to False* before our app goes out on the big bad Web, as it reveals way too much about the workings of our code.

Django's *URL Dispatcher* uses the elements in the urlpatterns array in this file to decide how to dispatch incoming requests. In this case, we will add a new urlpattern element to our array, which will cause the URL dispatcher to redirect incoming URL traffic

```[python]
# todo_list/todo_app/urls.py

urlpatterns = [
    
]
```

We can test our django setup by running the *runserver* command

```[python]
(todo-web-app-dev) C:\Users\Clarence Vinzcent\Real-World-Python\ToDo-WebApp\todo_list> py manage.py runserver
Watching for file changes with StatReloader
Performing system checks...

System check identified no issues (0 silenced).

You have 18 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.
May 17, 2022 - 14:14:39
Django version 3.2.9, using settings 'todo_project.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CTRL-BREAK.
[17/May/2022 14:14:52] "GET / HTTP/1.1" 200 10697
[17/May/2022 14:14:52] "GET /static/admin/css/fonts.css HTTP/1.1" 200 423
[17/May/2022 14:14:52] "GET /static/admin/fonts/Roboto-Bold-webfont.woff HTTP/1.1" 200 86184
[17/May/2022 14:14:52] "GET /static/admin/fonts/Roboto-Regular-webfont.woff HTTP/1.1" 200 85876
[17/May/2022 14:14:52] "GET /static/admin/fonts/Roboto-Light-webfont.woff HTTP/1.1" 200 85692
Not Found: /favicon.ico
[17/May/2022 14:14:52] "GET /favicon.ico HTTP/1.1" 404 2116
```

### Design your To-Do Data

We will design and code the application *data model* and the relations between application objects. Then you'll use Django's object-relational modeling tools to map that data model into database tables.

Each type of user data will require its own *data model*. Our to-do list app will contain two basic types of data:

1. *A ToDoList with a title:*
2. *A ToDoItem that is linked to a particular list:*

#### Defining our Data Models

In our *models.py* file, we have defined our data model in which we have created two model classes that handles the ToDoList Title data and the ToDoList Item data. On the next step, we'll use Django tooling to map the model to our database.

#### Create the Database

We will the *migrations* command from the Django library to connect our models to our database (for this instance is SQLite3)

```[python]
(todo-web-app-dev) C:\Users\Clarence Vinzcent\Real-World-Python\ToDo-WebApp\todo_list>py manage.py makemigrations todo_app
Migrations for 'todo_app':
  todo_app\migrations\0001_initial.py
    - Create model ToDoList
    - Create model ToDoItem

(todo-web-app-dev) C:\Users\Clarence Vinzcent\Real-World-Python\ToDo-WebApp\todo_list>py manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions, todo_app
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
  Applying todo_app.0001_initial... OK
  ```

  With *makemigrations*, we are telling Django that we've change the application's data model, and we would like to record these changes. We have defined two brand-new tables, one for each of your data models. Django has automatically created its own data models for the admin interface and for its own internal use.

  And every time that we have a change to our data model and  call makemigrations, Django adds a file to the folder todo_app/migrations/. In this repository, it stores a history of the changes that we've made to the database structure. This would allow us to revert and then reapply the changes later if need be.

  With the *migrate* command, we put the change into effect by running commands against the database.

### Adding our Sample To-Do Data

With the *admin interface*, this tool enables us to not only manage model data, but also to authenticate users, display and handle forms, and validate input.

#### The Django Admin Interface

Having a *superuser* would allow us to use the admin interface.

```[python]
(todo-web-app-dev) C:\Users\Clarence Vinzcent\Real-World-Python\ToDo-WebApp\todo_list> py manage.py createsuperuser
```

But before we can have access to our admin interface, we need to register the models first with the admin app

```[python]
# todo_list/todo_app/admin.py
from django.contrib import admin
from todo_app.models import ToDoItem, ToDoList

admin.site.register(ToDoItem)
admin.site.register(ToDoList)
```

#### Starting our To-Do List

The admin interface is great for use for quick and dirty hacking on the data, but it's not intended to be seen by regular users.

### Creating the Django Views

In creating the public interface for our application, this involves using Django's views and templates. A *view* is the code that orchestrates web pages, the presentation logic of our web app. The *template* is the component that more closely resembles an HTML page.

#### Our First Django View

A *view* is a Python code that tells Django how to navigate between pages and which data to send along to be displayed. A web application works by receiving an HTTP request from the browser, and then it decides first on what should it do, and then sending back a response. Then the application sits back and waits for the next request.

In Django, the *request-response* cycle is controlled by the view, which, in it's most basic form, is a Python function living inside the file views.py. The view function's main input data is an *HttpRequest Python object*, and its job is to return an *HttpResponse* Python object.

There are two styles in coding a view, in which we can do it by creating a function, and the other is by using a Python class, in which those methods will handle the requests. In either of those cases, we need to inform Django that our view is intended for handling a particular type of request.

Here are the advantages to using a class:

* *Consistency*: Each *HTTP request* is associated with a command to the server, known as its method or verb. This may be GET, POST, HEAD, or another, according to the response required. Each *verb* has its own matching method name in our class. For example, the method for handling an HTTP *GET* request is named .get().

* *Inheritance*: We can use the power of inheritance by extending existing classes that already do most of what you need the views to do.

In this project, we will be using the *class-based* approach and taking advantage of Django's pre-built *generic views* for maximum code reuse.

Fundamentally, the units of information called *records* have four basic operations that we can do:

1. *Create* records
2. *Read* records
3. *Update* records
4. *Delete* records

In Django, it provides developers with *class-based views*, which are pre-built views implemented as classes that already contain most of the code to do the CRUD operations.

```[python]
# todo_list/todo_app/views.py
from django.views.generic import ListView
from .models import ToDoList

class ListListView(ListView):
    model = ToDoList
    template_name = "todo_app/index.html"
```

From the above code that we have created, we need it to tell two things to the django.views.generic.ListView:

1. The data-model *class* that you'd like to fetch
2. The name of the *template* that'll format the list into a displayable form.

#### Understanding Templates

A *template* is a file containing HTML markup, with a few additional placeholders to accomodate dynamic data.

*Note*: We can return to the app's home page from anywhere in the app by clicking on the Django To-do Lists heading

#### Add a Home Page Template

In our new index.html file, we have extended the features from the base template. Except everything between and including the {% block content %} and {% endblock %} tags.

#### Building a Request Handler

The two most common HTTP request verbs are *GET* and *POST*. The action performed by a GET request is mostly defined by its URL, which not only routes the request back to the correct server, but also contains *parameters* that tell the server exactly what information the browser is requesting.

A *POST* request may also have URL parameters, but it behaves a little differently. POST sends some further informationm besides the URL parameters, to the server.

The *URL Dispatcher* does its job by consulting what's known as the URLconf, a set of URL patterns mapped to views. These mappings are conventionally stored in a file named urls.py.

In cooking-up our first home-baked view. The *request-response cycle* proceeds in our ToDo App.

#### Reuse Class-Based Generic Views

*Class-based generic views* take reusability to the next level. It needs to know just two things:

1. What data type it's listing
2. What template it'll use to render the HTML

#### Subclass ListView to Display a List of To-Do Items

Extending the ListView class with our custom class ListListView and ItemListView.

#### Show the Items in a To-Do List

In adding our url list/<int:list_id>/ means that this entry will match a URL like list/3/ and pass the named parameter list_id=3 to the ItemListView instance. If we revisit the ItemListView code in views.py, we'll notice that it references this parameter in the form self.kwargs["list_id"].

We have implemented just the *Read* part of the *CRUD* operations.

[Documentation of Pipe Filters](https://docs.djangoproject.com/en/3.2/ref/templates/builtins/)

### [TBC] Create and Update Model Objects in Django
