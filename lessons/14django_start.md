Start A Django Project
======================

## Create Django Files
Let's create a new repository for your project. The code for your webapp will be
in this directory. In your `project` folder (not in `django-buildup`), run the
following (replacing `<PROJECTNAME>` with your project's name) in the terminal
to create all the necessary files:
```
django-admin.py startproject <PROJECTNAME>
```

## Install PostgreSQL
Now let's download and install `Postgres.app`, which will let us run the database
for your webapp. You can download the app from http://postgresapp.com/

## Add Dependencies
Now let's add a `requirements.txt` file with the following:
```
Django==1.7.4
dj-database-url==0.3.0
gunicorn==19.1.1
psycopg2==2.5.4
whitenoise==1.0.6
wsgiref==0.1.2
```

And now let's install the dependencies from the terminal. We're going to be doing
this using `virtualenv` this time:
```bash
sudo pip install virtualenv
virtualenv venv
source venv/bin/activate
export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/9.4/bin
pip install -r requirements.txt
```
If everything was successful, you should see
```
Successfully installed Django-1.7.4 dj-database-url-0.3.0 gunicorn-19.1.1 psycopg2-2.5.4 whitenoise-1.0.6
```
printed at the bottom.

## Configure Some Stuff
Now let's configure some stuff. First, create a file called `.gitignore` that looks like this:
```
venv
```
This tells git to ignore the contents of the `venv` directory.

In `settings.py`, add
```python
import dj_database_url
```
to the top of the file, and replace the `DATABASES` setting with
```python
{ 'default': dj_database_url.config() }
```
Replace the `SECRET_KEY` setting with
```
SECRET_KEY = os.environ['DJANGO_SECRET']
```
and replace the `DEBUG` setting with
```
DEBUG = "DEBUG" in os.environ
```
Then add the some other settings:
```
STATIC_ROOT = 'staticfiles'
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
)
ALLOWED_HOSTS = ['*']
```

Now we can create the `.env` file which will specify all these values (replacing
`<PROJECTNAME>` with your projectname.
```
DATABASE_URL=postgresql://localhost/<PROJECTNAME>
DJANGO_SECRET=secretvalue
DEBUG=true
```

Now we need to create the database named `<PROJECTNAME>` in Postgres. In your
toolbar, there should be an elephant icon - click on it and go to `Open psql`.
Once you are in the `psql` shell, type the following:
```sql
CREATE DATABASE <PROJECTNAME>;
```

Now you can run `foreman run python manage.py syncdb` to create all the necessary
models.

Now let's set up static files. Replace the contents of `wsgi.py` with the follwing,
replacing `PROJECTNAME` with your project name:
```python
import os
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "PROJECTNAME.settings")

from django.core.wsgi import get_wsgi_application
from whitenoise.django import DjangoWhiteNoise
application = get_wsgi_application()
application = DjangoWhiteNoise(application)
```

Finally, let's add the `Procfile` for this app. Create the file with the following:
```
web: gunicorn playground.wsgi --log-file -
```

## Push To GitHub
Now you can add the folder to the GitHub app (or run `git init` from the command
line) and then commit and push your changes to a repository on GitHub!

## Push To Heroku
Now you can also create a Heroku app with `heroku create`.
You must set the `DJANGO_SECRET` variable with
```
heroku config:set DJANGO_SECRET=<SOMESECRETTEXT>
```
and then you can push your changes to the app with `git push heroku master`!
