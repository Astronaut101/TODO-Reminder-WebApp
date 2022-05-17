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

### [TBC] Design your To-Do Data
