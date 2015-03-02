More Templates and Static Files
===============================

# More Template Magic
Now that we are using a bunch of templates, you may have noticed that there's
a lot of duplicated HTML in each one. Using `template inheritance`, we can have
all of our templates share the same HTML strucutre, and even appearance! Let's create
a "base" template in the file `buildup/templates/base.html`:
```html
<html>
  <head>
    <title>{% block title %}My amazing site{% endblock %}</title>
    {% block head %}{% endblock %}
  </head>
  <body>
    <h1>Here's a big navbar thing that shows up on all templates</h1>

    <div class="container">
      {% block content %}{% endblock %}
    </div>
  </body>
</html>
```
In this template, we defined "blocks" called title, head, and content. Any template
which wants to "inherit" this base template's HTML can simply provide things to place
in each block! With this kind of inheritance, you can add things like a navbar or
footer to the base template and have it show up on every page!

Let's try changing an old template to inherit from the base template. We can
change `buildup/templates/new_fact.html` to look like this:
```html
{% extends "base.html" %}

{% block title %}Create a Fact{% endblock %}

{% block content %}
  <h1>New Fact</h1>
  <form action="{% url 'new_fact' %}" method="POST">
    {% csrf_token %}
    {{ form }}
    <input type="submit" value="Submit"/>
  </form>
{% endblock %}
```
We tell Django that this template is "extending" the `base.html` template using
`{% extends "base.html" %}` and then we provide some stuff to place in the title
and content blocks. In this case, Django will take the `new_fact.html` template
combined with the `base.html` template and produce the following, which will be
provided to the `render` function in `views.py`:
```html
<html>
  <head>
    <title>Create a Fact</title>
    {% block head %}{% endblock %}
  </head>
  <body>
    <h1>Here's a big navbar thing that shows up on all templates</h1>

    <div class="container">
      <h1>New Fact</h1>
      <form action="{% url 'new_fact' %}" method="POST">
        {% csrf_token %}
        {{ form }}
        <input type="submit" value="Submit"/>
      </form>
    </div>
  </body>
</html>
```
So in a sense, this lets you "template" your templates by using the `block` and
`endblock` directives.
![yodawg](http://i.imgur.com/P1nivtC.jpg)

## Convert Your Templates
Now try converting all your templates to use the base template. You can add a header
and footer to the base template and it should show up on all your pages.

# Static Files
Until now, every response that Django sends back to the browser is generated
on-demand. When you're building a webapp you might have images or CSS or Javascript
which needs to be served to the browser, but these files don't change very often.
For these "static" files, we can serve them directly to the user without any changes.
Django has a `staticfiles` application which will automatically do this for us -
any files placed in a particular folder (we'll use `buildup/static`) will be
available at http://127.0.0.1/static.

## Configuring Staticfiles
In `buildup/settings.py`, we need to add `"django.contrib.staticfiles"` to the
`INSTALLED_APPS` setting, and we need to define the URL for static files:
```python
STATIC_URL='/static/'
```

Now we'll need to modify `buildup/urls.py` so that Django knows which URLs belong
to static files. At the top of `urls.py`, import the following lines
```python
from django.conf import settings
from django.conf.urls.static import static
```
and then modify your url patterns so they look like this:
```python
urlpatterns = (
  # blahblahblah
) + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```
What we're doing is defining `urlpatterns` to be the sum of the patterns that you
have defined, along with the magic patterns defined by `static`, which enable you
to access static files from a URL.

## Add Some CSS
Now we can add a static CSS file to show on all the pages. Let's change some
colors in `buildup/static/style.css`:
```css
html {
  background-color: green;
  color: red;
}
```
and then let's load the CSS in `buildup/templates/base.html` by adding the line
```html
<link rel="stylesheet" href="/static/style.css"></link>
```
inside the `<head>` block. If everything worked, your website should now be loading
the static CSS file we created!

## Make It Less Ugly
These colors are pretty ugly, so let's change them up! You can go crazy and style
any part of your website from here. If you haven't used CSS before, you can start
with Mozilla's excellent reference to get started: https://developer.mozilla.org/en-US/Learn/CSS/CSS_properties

# Extras
If you're feeling ambitious, you can try adding Twitter's Bootstrap to your webapp. 
Bootstrap is a set of common elements like buttons, dropdowns, and sliders that
you can use to build your webapp. If you want to add Bootstrap to your webapp, 
you can add the following to the `<head>` of your `base.html` template:
```html
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap.min.css">

<!-- Optional theme -->
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/css/bootstrap-theme.min.css">

<!-- Latest compiled and minified JavaScript -->
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.2/js/bootstrap.min.js"></script>
```

These CSS and Javascript files will give you access to all of Bootstrap's components.
You can visit http://getbootstrap.com/components/ to see how to use each one.
Try adding a Bootstrap navbar to the top of your page!
