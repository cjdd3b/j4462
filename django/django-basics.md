# Django basics

Now that you've got a bit of a background in Python, it's time to put that to use building some web applications with Django!

## What is Django?

Django is what's known as a web framework. There are many frameworks in many different languages (you may have heard of Ruby on Rails, for instance), but all of them have a common goal: to simplify the building and maintenance of websites and other web applications.

Django in particular was built with a newsroom environment in mind -- so much so that its tagline is "for perfectionists with deadlines."

It works by abstracting many common functions involved in building websites -- writing SQL queries, processing input with forms, formatting data for display, etc. -- and wrapping them into concise, easy-to-use statements and tools in the Python language. Using Django rather than writing everything from scratch ensures that you will be writing substantially less code; you'll be able to develop much faster; and you'll make fewer mistakes.

A great overview of the framework's core functionality can be found [here](https://docs.djangoproject.com/en/1.4/intro/overview/).

## Models, templates and views

As we discussed in class, the core concept to remember about Django is that all applications are built from three atomic units: models, views and templates. We'll be covering each of these in detail throughout the class, but here's what you need to know to get started:

**Models**: Think of models as your database. Really, they are just representations of database tables using specialized Python/Django syntax, but they open up a ton of functionality that will make our life substantially easier. Models are represented as Python classes, as you'll see in this example we used in class:

```
from django.db import models

class Crime(models.Model):
    type = models.CharField(max_length=50)
    location = models.CharField(max_length=255)
    date_time = models.DateField()
    lat = models.FloatField()
    lng = models.FloatField()
```

**Views**: Views contain the business logic of our applications. A typical view might query out the top X items from our database, sort them, and send them to a template for display.

**Templates**: Templates are basically HTML files with some specialized syntax designed to help us display our data in a web browser. They take the output of a view and turn it into something that a user can interact with on the web.

## Starting a new Django project

Django projects are divided into two basic units: a "Project," which composes the entirety of any given project you are going to build, and "Apps," which are all of the individual components of that project. If you whole Django project was, say, a newspaper website, you might have separate apps to handle blogs, stories, photos, interactives and other content types.

When you're just getting started, most of your projects will only contain one app. And that's where we're going to start. The sample app we're going to build in this class is going to provide a simple browsing interface for Columbia Police dispatch data. It will have one model: a simple representation of the data found here: http://www.gocolumbiamo.com/PSJC/Services/911/911dispatch/police.php

But before we can do that, we need to start a new Django project to contain all the code we're going to write. From the command line, open your desktop (or another directory that you would prefer to work in) and type the following.

```
django-admin.py startproject crime
```

Assuming you have Django properly installed, you should see a new folder appear with a few files in it. We'll explain those in a second. But first, django-admin.py is a built-in Django utility that contains a bunch of helpful commands to make our lives easier. As you might suspect, the ```startproject``` project command does exactly what the name implies: starts a new project.

Now let's take a look at the files that have been created automatically by the ```startproject``` command. Navigate into your crime directory and you'll see two things: a file called ```manage.py``` and another directory, also called ```crime```. The ```manage.py``` file does almost exactly the same thing as ```django-admin.py```, with one key difference: it's designed to work only with one project, rather than Django as a whole. That means it has an understanding of your project's apps and settings, which we'll be building later. Pretty much, you'll be using this for a lot of different things.

The second crime directory is the actual directory you'll be working in most of the time, except when you're using ```manage.py```. It's kind of confusing to have to directories of the same name nested within each other, but it's just a weird Django quirk you'll have to get used to. Go ahead and navigate into the second crime directory and you'll see a few more files:

- urls.py: The URLs file is basically a traffic routing system. It allows you to tell Django what to do when people go to different URLs under your project. For example, if someone goes to www.yourproject.com/about, you might want to send them to an "about" page. Or if they go to www.yourproject.com/contact, you may want to send them to a "contact" page. Those routing instructions are defined here (we'll get to them later).

- settings.py: Every Django project has a number of settings you'll need to tweak in order to do things like hook up to a database, turn certain features off and on, etc. This is where those are defined. 

- wsgi.py: Don't even worry about this. It's used when you deploy your app live to the web, which we're not going to be doing.

- __init__.py: Don't worry about this either. It's here so that Django can properly access the files in your folder, but it doesn't actually have any code inside.

## Starting a new app

Now that we've got our project built, we need a place to start putting our models and views for our crime application. Just like django-admin.py had its ```startproject``` command, it also has another command, ```startapp``` that will take care of that for us. Let's create a new app called data within the interior crime directory -- the same one that contains settings.py, urls.py, etc.

```django-admin.py startapp data```

Again, if all went well you should see a new folder called data appear. And if you navigate inside, you'll see a few more files:

- models.py: This is where we'll put our models

- views.py: This is where we'll put our views (we'll explain this later in the course)

- tests.py: For now, don't worry about this.

Now you're ready to start actually writing Django code. But the first thing you'll want to do (covered in the next section) is get your settings in order.