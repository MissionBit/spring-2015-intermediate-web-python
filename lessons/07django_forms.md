Django Forms
============

# Recap
Now our app has "memory" - it knows which Facts we have created, and it can list
them when we ask. Recall that this required adding a Fact "model" which tells
Django what information it needs to store in the database. Right now, our 
server currently looks like this:

![Django stack 3](https://raw.githubusercontent.com/MissionBit/spring-2015-intermediate-web-python/master/img/django3.png)

* A request comes in, and we try to match it to one of the patterns in `urls.py`.
* We find the matching view for that pattern in `views.py` and run it.
* The view asks the Fact model for all of the Facts that are saved in the database.
* The view gets a list of all the Facts and renders the template with that data.

Now we want to be able to create Facts without having to open the Django shell.
In HTML, we can use a "form" to input data and send it back to Django for saving.
Fortunately, Django provides an easy way to build forms for our models. 

# Defining Forms
A form specifies what types of input it takes (checkboxes, text fields, dropdowns, etc)
and where to send the data when you click submit. In order for Django to know
what input the form takes, we need to define the fields similar to how we defined
the Fact model.

In `buildup/urls.py`, let's create the url for the new fact page.
```python
url(r'^facts/new/$', 'buildup.views.new_fact', name='new_fact')
```

Now in `buildup/views.py`, import the `forms` helper and some other tools at the top of the file:
```python
from django import forms
from django.core.urlresolvers import reverse
from django.http import HttpResponseRedirect
```
and then define what information a FactForm takes:
```python
class FactForm(forms.Form):
    text = forms.CharField(label="A random fact", max_length=255)
    author = forms.CharField(label="Your name", max_length=255)
```
We'll leave out the created_date from the form because that can be automatically
generated when we actually save the Fact.

Now let's create the `new_fact` view in `buildup/views.py`:
```python
def new_fact(request):
    # someone wants to create a fact
    if request.method == "GET":
        form = FactForm()
        return render(request, "new_fact.html", { "form": form })
    else:
        return HttpResponse("TODO: save the fact")
```

And finally, let's add the template in `buildup/templates/new_fact.html`:
```html
<html>
  <head>
    <title>Create a Fact</title>
  </head>
  <body>
    <h1>New Fact</h1>
    <form action="{% url 'new_fact' %}" method="POST">
      {% csrf_token %}
      {{ form }}
      <input type="submit" value="Submit"/>
    </form>
  </body>
</html>
```
There's a couple things to note here:
 * Django has a `url` helper which you can use in templates - `{% url 'new_fact' %}` will find the url named `new_fact` in `urls.py` and be replaced by the full url `/facts/new/`
 * The `<form>` tag has an "action", which specifies which url to send the data on submit
 * All HTTP requests have a "verb" which tells the server what you want to do with the url. The convention is to use the verb "GET" when you want to view something, and "POST" when you want to create something new.
    * The `<form>` tag uses the "POST" method because we want to tell the server to create a new fact based on the data from the form.
 * Django will automatically create all the necessary input fields when you say `{{ form }}`
 * The `csrf_token` is used to verify that the form with `action=/facts/new/` originates from the website itself, and not someone else. Without this, a hacker could recreate your form on their own website and submit any data they wanted to your server. You can learn more about this hackery and how to prevent it here: http://www.squarefree.com/securitytips/web-developers.html#CSRF

If you start up the server `python manage.py runserver` and navigate to http://127.0.0.1:8000/facts/new/,
you should see a form! When you try to submit it, you'll see that we still have to handle saving the Fact!

# Validating Forms
In `buildup/views.py`, lets replace the TODO with the logic for saving the fact.
After the `else:`, we can add the following:
```python
        # someone submitted the form so we need to save the data
        form = FactForm(request.POST)

        if form.is_valid():
            # TODO actually actually save the new fact

            # and redirect to the all_facts page
            return HttpResponseRedirect(reverse('all_facts'))
        else:
            # return the form and display the errors
            return render(request, "new_fact.html", { "form": form })
```

Django will automatically create a form from the `POST` data when you say `FactForm(request.POST)`,
and then we can check the form to see if everything looks good before saving. If not,
we can send the form back with errors.

Now we're a little bit closer! If you try to create a fact and you leave a field blank,
you will get the form back with some errors. If you submit valid data, you'll
be redirected to the 'all_facts' page, but your fact isn't actually saved!

# Actually Actually Saving the Fact
Now you want to replace the final `TODO` with your code for saving the Fact. You
will be able to create and save Facts just as you did in the shell last lesson!

Hint: You can retrieve the data from the form with `form.cleaned_data['text']` and `form.cleaned_data['author']`

Remember to add `created_date` to your `Fact` since the form doesn't do it for you!

# Extras
If you're looking to add some more to your Facts app, here's some more things to try:

## Add Some Styling
Add some CSS to your templates to make things look fancy! You can find a CSS
reference here: https://developer.mozilla.org/en-US/Learn/CSS/Using_CSS_in_a_web_page

## Random Facts
Add a `/facts/random/` url which returns a single random fact. You'll need to
add a new pattern to `urls.py`, a new view to `views.py`, and a new template to
display the fact.

Hint: You can use `Fact.objects.order_by('?')[0]` to retrieve a random Fact.

## Facts by Author
Add a `/facts/<AUTHOR>/` url which retrieves all the facts from a particular
author.

Hint: You can use `Fact.objects.filter(author='AUTHOR')` to retrieve only Facts by a certain author
