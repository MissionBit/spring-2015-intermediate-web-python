Django Templates
============

# Recap
We've see how the request/response cycle works in Django, but so far all we
are able to do is return some text via an `HttpResponse`. Your server currently
looks like this:

![Django stack 1](https://raw.githubusercontent.com/MissionBit/spring-2015-intermediate-web-python/master/img/django1.png)

* A request comes in, and we try to match it to one of the patterns in `urls.py`.
* We find the matching view for that pattern in `views.py` and respond with some text.

We want a way to respond with files instead of text, turning our server into this:
![Django stack 2](https://raw.githubusercontent.com/MissionBit/spring-2015-intermediate-web-python/master/img/django2.png)
(Taken from http://littlegreenriver.com/weblog/2013/03/23/django-for-designers/)

# Rendering Templates
In Django, we can do this using templates. You can think of a template like
a Mad-libs page. You have a series of blanks in your file that can be filled in later
before giving the file back to the browser.

Let's try an example - first we have to add some settings. In `buildup/settings.py`,
add the following lines so Django knows where to look for templates.
```python
BASE_DIR = os.path.dirname(os.path.dirname(__file__))
TEMPLATE_DIRS = (
    os.path.join(BASE_DIR, "buildup/templates"),
)
```
And now create the folder `buildup/templates` so we can add files to it. In the
terminal, navigate to your project folder and create the folder using `mkdir`
```
cd ~/GitHub/projects/django-buildup/buildup
mkdir templates
cd ..
```

Now let's create the file `buildup/templates/hello.html`
```html
<html>
  <head>
    <title>A templated page: {{ yourname }}</title>
  </head>
  <body>
    <marquee>Hey hi your name is {{ yourname }}!</marquee>
    The value of foobar is {{ foobar }}
  </body>
</html>
```
Everything between the `{{` and `}}` is a "blank" part of the template and
will be filled in later.

In `urls.py`, add a pattern for `/hello_template/<YOURNAME>`:
```python
url(r'^hello_template/(?P<yourname>.+)/$', 'buildup.views.hello_template', name='hello_template')
```
In `views.py`, we're going to be using the `render` function, so add a line at
the top to import it:
```python
from django.shortcuts import render
```
Now add your `hello_template` function below. Remember that it also takes
`yourname` as an argument because the url captured that for us!
```python
def hello_template(request, yourname):
  return render(request, "hello.html", { "yourname": yourname, "foobar": 12 })
```
The `render` function takes the request, the name of a template to use, and
an optional "dictionary" to use for the context. A "dictionary" is a collection
of names (or keys) and values. In this case, the dictionary has the key "yourname"
whose value is what you provided in the url, and a key "foobar" whose value is 12.
The `render` function will use the `hello.html` template and replace "yourname"
and "foobar" in the template with the values you provided.

# Add some other templates

Modify your time, random, and say views to use templates - you'll want to create
a template file for each one in `buildup/templates` and modify the `buildup/views.py`
to use `render` instead of `HttpResponse`.
