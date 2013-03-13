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

## A quick assignment

Now that you've got your data loaded, now would be a good time to finish the short assignment we discussed in class: importing this csv file into SQLite. Use the SQLite Manager software you should have already installed in Firefox and load the data into a table called **crimes_data**.

## Moving along: Models

Now that you've got your database up and running, it's time to talk about models.

As we've discussed, models are simply Django-friendly representations of your database, written using Python code. They are represented as classes, with attributes that define (or mirror) the different fields in the database. They can also have methods, which we'll get to later.

The model for the crime table we just created should look like this. It should be located in the models.py file of the ```data``` app you created above. We'll go over each part in detail:

```
from django.db import models

class Crime(models.Model):
    incident_num = models.CharField(max_length=20, primary_key=True)
    name = models.CharField(max_length=255)
    location = models.CharField(max_length=255)
    apt_lot = models.CharField(max_length=20)
    date_time = models.DateTimeField(blank=True, null=True)
    lat = models.FloatField(blank=True, null=True)
    lng = models.FloatField(blank=True, null=True)

    class Meta:
        db_table = 'crime_data'

    def __unicode__(self):
        return self.incident_num
```

The first thing our model does is import some help from Django. The line ```from django.db import models``` is telling our models file to go into Django's pre-built toolbox and come back with a tool called models, which we'll use in order to construct our crime model below. Like other imports, the top of our models file will almost always bring in all of the outside libraries and toolkits we need to make our program function properly.

The next line, ```class Crime(models.Model):```, is a class delcaration that should look a little familiar from our discussion of Python classes earlier in the semester. We're declaring a new class, called Crime (remember: classes always start with capital letters). The only slight difference you'll see in the syntax is that the Crime model is doing something known as <em>subclasing</em> -- inheriting some functionality from models.Model. You don't have to worry about the ins and outs of this yet -- you just need to know that pretty much every time you create a model in Django, you will declare it exactly like this. Note the colon at the end of the line, which means the next line must start with four spaces.

The next seven lines, from ```incident_num = models.CharField(max_length=20, primary_key=True)``` on down, follow a pretty standard pattern. In Python terms, they are class attributes represented as variables. But really, they're just Python/Django representations of the fields we have in our database table. Let's break down the syntax of the first one, ```incident_num```.

Really, ```incident_num``` can just be thought of as a variable, so it's being declared just like normal variables would be. But instead of being a conventional variable type, like a list or a string, the ```incident_num``` variable uses a special type of object designed to represent database fields. That's what the ```models.CharField(max_length=20, primary_key=True)``` is about.

You don't need to understand exactly how these fields work, but if you read it out loud, it starts to make a little sense. Somewhere within the models toolkit we imported above is something called a CharField, which corresponds to a conventional character field in a database. That field takes a couple of arguments, in this case ```max_length``` and ```primary_key```.

The ```max_length``` argument tells Django how long that character field is allowed to be. The trick is to make it long enough that it will fit any record in your dataset. The ```primary_key``` field tells Django to use this field as the table's primary key -- in other words, that every record in that field is distinct and unique, just like a primary key in a conventional database. If you don't define this, Django will create a hidden primary key field on its own.

The same pattern holds for the other fields, with tiny differences. You'll notice a couple other types of fields, namely ```DateTimeField``` and ```FloatField```, which are designed to hold date/time and float data, respectively. Each of those fields also takes slightly different arguments, namely ```blank``` and ```null```. Each field in Django is unique and has its own set of rules associated with it. Your best bet is to keep [this page](https://docs.djangoproject.com/en/dev/ref/models/fields/) handy when you're creating new models. It lays out all the options in detail.

The next section down looks like this:

```
    class Meta:
        db_table = 'crime_data'
```

It declares any [https://docs.djangoproject.com/en/dev/ref/models/options/](Meta options) you might want to assign to the class. You won't have to worry about this much to start, except in one important case. If you already have a table in your database that contains data, and you want to connect to it, you will need to define the ```db_table``` option here and point it at the name of your existing table. This tells Django that the model you just created should be hooked into the table of data you imported earlier.

The final section of our models file looks a little strange, but it's pretty intuitive once you understand it:

```
    def __unicode__(self):
        return self.incident_num
```

Remember that models are classes. And classes can have both attributes (in this case the fields) and methods (stuff the class can do). Also recall from earlier that method definitions look a lot like functions. They start with the word ```def```, are followed by a name, and then accept any number of arguments -- the first of which is always the argument ```self```, which will always correspond to a given instance of that class.

Pretty much all classes have a built-in method known as ```__unicode__```, with two underscores on each side. What that method does is represent any given instance of that class as a string. Going back to the car example we used in the Python section of the class, we might want to label my Prius with the name "Chase's Prius" whenever it pops up. That's what we're doing here.

In this case, each record in our database can be thought of as an instance of our model. We want the unicode method to return some information on that crime so we can tell it apart from the others. The incident number might make sense to start, since it's a unique ID for every crime in the dataset. That's why our method is returning ```self.incident_num```

Self can be a confusing concept. Basically, it corresponds to any given instance of our class -- or in this case, any given row of our database. So if we had a list of all the crimes in our dataset and wanted to print them out one at a time, this method would basically tell each of them to say their own name -- in this case, represented by each crime's incident number. In other words, what the unicode method does is basically enable each instance of our crime class to say its own name.

That will come in handy during our next lesson, when we cover Django's automatic administration interface.