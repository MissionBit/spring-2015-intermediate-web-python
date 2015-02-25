Django Authentication
=====================

# Configuring Authentication
Adding users to our site is handled by Django using the same concept of models.
Each user's username, email, and password (in encrypted form) are stored in the
database and can be used to authenticate users who try to login. 

We need to tell Django how to handle authentication in order for logging in
and out to work. In `buildup/settings.py`, add the following lines into the
`MIDDLEWARE_CLASSES` block so it looks like this:
```python
MIDDLEWARE_CLASSES = (
    'django.middleware.common.CommonMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
)
```
and then add the following lines to `INSTALLED_APPS` so it looks like this:
```python
INSTALLED_APPS = (
    'buildup',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
)
```
and finally, add the following settings to the bottom of the file:
```python
    LOGIN_REDIRECT_URL = '/facts/'
    LOGIN_URL='/login/'
    LOGOUT_URL='/logout/'
```
These settings tell Django to enable user authentication and also the default urls
for logging in and out.

Now we need to tell Django to create the User model for us. Run
```bash
python manage.py syncdb
```
and create a superuser with a username, email and password when it asks.

In `buildup/urls.py`, add a `/login/` and `/logout/` url. Note that these urls
will be handled by code in `django.contrib.auth`, so we don't have to create them!
```python
    url(r'^login/$', 'django.contrib.auth.views.login', name='login'),
    url(r'^logout/$', 'django.contrib.auth.views.logout', name='logout'),
```

Now let's create the folder `buildup/templates/registration` and add a `login`
and `logout` template. In `buildup/templates/registration/login.html`, put the following:
```html
<html>
  <head>
      <title>Login</title>
  </head>
  <body>
    {% if form.errors %}
    <p>Your username and password didn't match. Please try again.</p>
    {% endif %}
    
    <form method="post" action="{% url 'django.contrib.auth.views.login' %}">
        {% csrf_token %}
        {{ form }}
    
        <input type="submit" value="login" />
        <input type="hidden" name="next" value="{{ next }}" />
    </form>
  </body>
</html>
```
We're using a form to send data to Django, and then Django will check if the username
and password are correct. Notice how similar this template is to the `new_facts.html`
template we created earlier - we can simplify this later on by sharing parts of the
templates between the two.

In `buildup/templates/registration/logged_out.html`, put a simple message like this:
```html
You are now logged out.
```

Now when you run the server, you should be able to visit http://127.0.0.1:8000/login/
and enter your superuser name and password. When you log in successfully, you will be redirected
to the `/facts/` page. You can also use http://127.0.0.1:8000/logout/ to switch users.

# Authenticated Pages
Just having user authentication by itself is pretty boring - let's try modifying
the `/facts/new` page to only allow logged in users to create a fact. Django has
a helper `login_required` which will block pages for users who aren't logged in.
In `buildup/views.py` import the `login_required` helper at the top of the file:
```python
from django.contrib.auth.decorators import login_required
```
and then place `@login_required` above the `new_fact` function so it looks like
this:
```python
@login_required
def new_fact(request):
```
The `@login_required` is a "decorator", which is a way in Python to modify a function
(in this case, `new_fact`) with some other behavior: if we're not logged in, redirect
the user to the login page, otherwise run the "decorated" function.

Thats it! Now we have a login protected page for creating facts. 

# User Associated Models
Now `new_fact` requires a user to be logged in, but it still asks the user to
enter an author for the fact! We can do this automatically because we know the name
of the user. 

## Foreign Keys
First, let's modify the `Fact` model in `buildup/models.py` to use a "Foreign key"
to the user. Replace the `author` field in the `Fact` class with the following:
```python
author = models.ForeignKey('auth.User')
```
This tells Django that each `Fact` should have a foreign key to a User, or put simply,
each Fact is associated with a User.

## Migrate the Database
Now that we have modified the Fact model, we have to migrate the database. Normally
we can tell Django to "migrate" our models to the new definition, but since we
added the User model, we have to start from scratch. Let's remove the database
and any existing migrations:
```bash
rm buildup.db
rm -r buildup/migrations
```
and then we can initialize the database with the new model definitions:
```bash
python manage.py syncdb
```

## Saving Facts
Now our Fact model has a key to the User it was created by, but we still have to
change the `new_facts` view to reflect that. We want the author field of the Fact
to be set automatically by the view like `created_date`, so make the following changes:
 * The `FactForm` should no longer have an `author` field because we will set it automatically.
 * When creating the `Fact`, the `author` field should be set to a User model instead of a string. You can use `request.user` to get the current User model.

Once this is complete, you should be able to create facts after logging in with your account.

# Creating New Users
Since users are just Django models, we can create new ones in the Django shell
just like we did for Facts. Enter the shell with `python manage.py shell`
and try to create a new user:
```python
from django.contrib.auth.models import User
user = User.objects.create_user('bob', 'bob@gmail.com', 'password1234')
user.save()
```
Now you can create a bunch of users and add Facts associated with each one!

# Extras
## Better User Registration
Try adding a `/register` url which lets you create a new user on the website. Look
at the view and template for `new_fact` to see how you can create and save a model
based on user input.
