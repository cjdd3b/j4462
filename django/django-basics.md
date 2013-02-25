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