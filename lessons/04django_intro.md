Django Intro
============

Django is a web framework that we will be using to build our webapps. It helps
you break down your app into components for storing data, returning content,
and all the logic in between. Django comes with a lot of built in features
for user authentication, an admin panel, forms, uploading files, and more. You
don't need something like Django to build a webapp, but it makes it easier by
providing a lot of the common functionality for you and also encouraging
you to build your webapp in an organized manner.

# Setting up Django
In order to get started, we need to install a couple things. Open up your
terminal and run the following commands.

## Fork and clone the repo
On GitHub, fork the https://github.com/MissionBit/django-buildup repo onto
your account and then clone it onto your computer.

## Install pip
Pip is a tool which allows you to install Python libraries that you can use in
your code. You can install pip by running
```bash
sudo easy_install pip
```
in your terminal. Enter your password when it asks you (it won't show you any letters you type)
and then press Enter. If this is successful, you'll see a bunch of stuff printed. The
last line should look like this:
```bash
Finished processing dependencies for pip
```

## Install Django
Now that pip is installed, we can use it to install Django and all the other
libraries which our app uses. Navigate to the django-buildup project folder
```bash
cd ~/GitHub/django-buildup
```
where we can see the code for the app. The `requirements.txt` file contains a
list of "dependencies" or external libraries that this app requires to run. To
install all of the dependencies, type
```bash
sudo pip install -r requirements.txt
```

## Run the app
Now we are ready to run the application! In your project folder, run
```bash
python manage.py runserver
```
which will start the webserver for the app. You can visit http://127.0.0.1:8000/
to see your app running! If everything worked, you'll see 'Hello World' displayed
on your screen. You can press Ctrl-C to exit the server in your terminal.

# What happens when I visit a website?

When you open a website, your browser is requesting something (most likely an HTML file)
from the website's server. For example, when you visit `twitter.com/djangoproject`,
it looks something like this:
 * Browser: Yo `twitter.com`, can I have the page for `/djangoproject`?
 * Twitter: Ya sure here it is
 * Browser: K thanks, I'm gonna display this on the screen now

# How does Django make this easier?

Django is built to help you organize the components of your web application
in a way that's easy to extend. When Django gets a new request, it first tries
to "resolve" the URL and figure out what you're asking for. Once it knows
what you're requesting, it returns a "view" which is how Django responds with data.
In the Django world, a browser request would look
something like this:
![Django stack 1](https://raw.githubusercontent.com/MissionBit/spring-2015-intermediate-web-python/master/img/django1.png)
(Taken from http://littlegreenriver.com/weblog/2013/03/23/django-for-designers/)

# Django URLs

Let's look at how this works in Django. In the `buildup/urls.py` file, you'll
see the following:

```python
from django.conf.urls import patterns, url
urlpatterns = patterns('', url(r'^$', 'buildup.views.hello', name='hello'))
```

What this is doing is matching certain url patterns to their views. the `r'^$'`
pattern in the third line will match the "root" of the website e.g. website.com.
The 'buildup.views.hello' string says that we should run the hello function in
`views.py` whenever someone visits this URL. 

# Django Views

Now in the `buildup/views.py` file, you'll see the following:

```python
from django.http import HttpResponse
def hello(request):
    return HttpResponse("Hello world!")
```

The hello function is what gets run when you visit the root of the website,
and it will return "Hello world!" as the response.

# Add some other pages

Now let's try adding some more urls and views!

## Current time
In order to add a view which prints the current time, we'll need to add a pattern
for the url and also add a view which will return the response.

In `buildup/urls.py`, add a pattern for `/time` so that your file looks like this:
```python
from django.conf.urls import patterns, url
urlpatterns = patterns('',
  url(r'^$', 'buildup.views.hello', name='hello'),
  url(r'^time/$', 'buildup.views.time', name='time'))
```

and add a function called `time` to `buildup/views.py` so it looks like this:
```python
from datetime import datetime
from django.http import HttpResponse
def hello(request):
    return HttpResponse("Hello world!")

def time(request):
    return HttpResponse("The time is " + datetime.now())
```

Now you can restart the server with Ctrl-C and visit http://127.0.0.1:8000/time/. You
should see the time updated on every refresh. If it looks good, commit your work to the repo!

Now take a shot at adding these URLs to your app:
## Random Number

Add a url which displays a random number.

Hint:
```python
from random import randint
randint()
```
will give you a random integer. Remember not to name the view the same name as
anything else you have imported or defined!

## Hello World part 2

Add a url pattern for `/hello/NAME/` which responds with
"Hello, NAME". This should work for any name you put in the url.

Hint: You'll need a special pattern to match `/hello/...` and "capture"
the second part of the url. You can use the following pattern in `urls.py`:
```
r'^hello/(?P<yourname>.+)/$'
```
which will match any url like `/hello/bob/`, `/hello/foo bar/`, `/hello/123456/`,
capture the second part, and pass it to the view as the variable `yourname`. In
your view, you'll have to add another argument to the function which you can use
in your response
```python
def hello_name(request, yourname):
  ...
```

## Computer Speak

Add a url pattern for `/speak/SENTENCE/` which speaks the sentence
out loud and then responds with "Done"

Hint: Your computer comes with the `say` command and you can use it in python:
```python
import subprocess
subprocess.call(["say", "here is a sentence"])
```
