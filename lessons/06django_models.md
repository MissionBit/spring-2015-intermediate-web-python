Django Models
=============

# Recap
Now we have a way to return templated pages that share some structure. Your
server currently looks like this:

![Django stack 2](https://raw.githubusercontent.com/MissionBit/spring-2015-intermediate-web-python/master/img/django2.png)

* A request comes in, and we try to match it to one of the patterns in `urls.py`.
* We find the matching view for that pattern in `views.py` and respond with some
data that could be created from a template.

The next step is adding some way to save data. We want to be able to have user
accounts, save tweets, etc and have that information saved when we restart the
server. We can solve that using models in Django, turning our server into this:

![Django stack 3](https://raw.githubusercontent.com/MissionBit/spring-2015-intermediate-web-python/master/img/django3.png)

# Defining Models
You can think of a model as a declaration of some data that you want to store.
Under the hood, Django will use a database to actually save that data, but it
does all the heavy lifting so you don't have to worry about it.

We're going to be creating a fact model that lets us save random facts on our
website. First we have to configure some database settings so Django knows where
to store things. In `buildup/settings.py` add the following lines:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'buildup.db')
    }
}
INSTALLED_APPS = (
    'buildup',
)
```

And now create the file `buildup/models.py` in your project.
```python
from django.db import models

class Fact(models.Model):
    text = models.CharField(max_length=255)
    author = models.CharField(max_length=255)
    created_date = models.DateTimeField('date created')
```
We're declaring a model called `Fact` and saying that it should have `text`,
`author`, and `created_date` fields in it.

Now let's try saving some models! First we'll have to tell Django to create the
database using this model definition. In your terminal type
```bash
python manage.py makemigrations buildup
python manage.py syncdb
```
If everything went correctly, you should see a `buildup.db` file in your project.
This is the database where all your information is stored.

# Creating and Querying Models
Now let's try saving some data. We can experiment using the Django shell, which
you can enter with
```bash
python manage.py shell
```
This is a special Python shell that has loaded Django and will let us experiment
with models. First, let's import the model that we just created and some time 
utilties:
```python
from buildup.models import Fact
from datetime import datetime
```
And now lets retrieve all the Facts that are stored in the database:
```python
Fact.objects.all()
```
What you get back is `[]`, or an empty list - we haven't saved anything yet!

Let's try creating a model
```python
f = Fact(text='Giraffes sleep standing up', author='Steve Irwin', created_date=datetime.now())
```
Now you can access the fields of the model like so:
```
f.text
f.author
f.created_date
f.id
```
Django automatically adds a unique `id` field, which is used to track where this
model is stored in the database. Right now, the `id` is `None` because we haven't
saved the fact yet. After you run
```python
f.save()
```
the `id` should now be set to `1` (this is the first Fact we have saved). Now go
ahead and create and save some more facts. Once you have done so, we can try
retrieving all of the Facts again:
```python
facts = Fact.objects.all()
print facts
```
`all` returns a list of `Facts`, and you can access individual facts by "indexing".
Lists start with index 0, so if your list has 3 elements, index 0, 1, and 2 will
return the elements in order.
You can verify that the facts are correct by printing out the contents of each
one individually:
```python
for fact in facts:
    print fact.id, fact.text, fact.author, fact.created_date
```

# Template Gymnastics
We can use for loops inside our templates too! Lets add a template that will
list all the facts in the database. In `urls.py`, add a pattern for `/facts`
called `all_facts`, and in `views.py`, import the `Fact` model and create the
`all_facts` function:
```python
from buildup.models import Fact
# your other views.py code is here...
def all_facts(request):
    return render(request, "all_facts.html", { "facts": Fact.objects.all() })
```
Now add the corresponding template in `buildup/templates/all_facts.html`:
```html
<html>
  <head>
    <title>Random Facts</title>
  </head>
  <body>
    <h1>Random Facts</h1>
    {% for fact in facts %}
    <div>
      <h3>{{ fact.text }}</h3>
      - {{ fact.author }}, {{ fact.created_date }}
    </div>
    {% endfor %}
  </body>
</html>
```
Now when you add more facts in the shell, `/facts` will be updated on refresh!
