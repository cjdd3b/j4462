# Models and settings

As we discussed in the django-basics file, Django apps are composed of three things: models, views and templates. Models -- the representation of your database in Django terms -- form the core of any app that you will build. But picking up where we left off, first you need to configure Django to hook up to a database. We'll do that first.

## Settings.py

Go ahead an open your ```settings.py``` file in the text editor of your choice (I'd recommend [SublimeText2](http://www.sublimetext.com/2)). The sheer number of settings in here might look intimidating at first, but the good news is that you only have to worry about a couple of them. We'll start with the important one, DATABASES, which should look something like this

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
        'NAME': '',                      # Or path to database file if using sqlite3.
        # The following settings are not used with sqlite3:
        'USER': '',
        'PASSWORD': '',
        'HOST': '',                      # Empty for localhost through domain sockets or '127.0.0.1' for localhost through TCP.
        'PORT': '',                      # Set to empty string for default.
    }
}
```

A couple quick throwbacks to the Python lessons from earlier. First, you'll notice that the DATABASES setting is just a variable containing a dictionary. Its keys are strings and its values are more dictionaries. The second thing you'll notice is that the DATABASES setting is written in all caps. This is a special convention in Python known as a global variable. At this point, you won't be creating many globals, so just rememember for your own code that all variables (except these) should still be lowercase.

Moving right along, we want to do two things here. First, we want to define the type of database manager we're going to use; and second, we want to give a name to our new database, which will contain our crime data.

Django supports a bunch of different database managers -- Posgresql, MySQL, even Oracle -- but the one we're going to use is called SQLite. Even if you don't know it, your computer almost certainly has SQLite installed already. It's a simple, lightweight database that's perfect for prototyping and requires almost no setup to use. I use it all the time for my own projects.

The way we enable SQLite is to look at the ENGINE setting within the DATABASES dictionary. You'll see that most of the work has already been done for us -- we just need to plug in the name of the database manager we want to use, in this case ```sqlite3```. Go ahead and change that ENGINE setting to look like this:


```
'ENGINE': 'django.db.backends.sqlite3',
```

A cool feature of SQLite3 is that it doesn't require us to set up a username or password. We can simply give the database a name and get right to work. In this case, let's call our database ```crime.sqlite3``` and change the NAME setting accordingly. Once you're done, your DATABASES setting should look like this:

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3', # Add 'postgresql_psycopg2', 'mysql', 'sqlite3' or 'oracle'.
        'NAME': 'crime.sqlite',                      # Or path to database file if using sqlite3.
    }
}

```

There's just one more step we need to take in order for the database to be connected properly. Remember ```manage.py``` from the last document? We're going to use that to finish the job. Head back to your command line and navigate to the directory that contains ```manage.py```. We're going to use a command called ```syncdb```, which is going to take care of the rest of the setup for us. Go ahead and type it in:

```python manage.py syncdb```

Hopefully you'll see a bunch of output roll by about various tables being created, and you'll probably be prompted to create a superuser (just step through the process -- we'll be needing that later). If you list the contents of your directory, you'll also see a new file: crime.sqlite. This is your database, which the syncdb command just created.

What just happened there is pretty magical. Django looked at our settings file, created a database using the engine and name we specified, and then filled that database with a bunch of tables it needs in order to function properly. All without us having to enter a database manager or write a single line of SQL. In the old days of doing everything by hand, this process might take an hour or two. Now it only takes seconds.