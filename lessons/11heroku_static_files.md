Static Files on Heroku
======================

There's a little more setup we have to do to get static files working on Heroku.
In `buildup/settings.py`, we need to specify a couple more things so Heroku
can properly configure static files:
```python
STATIC_ROOT='staticfiles'
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'buildup/static'),
)
STATICFILES_STORAGE = 'whitenoise.django.GzipManifestStaticFilesStorage'
```
We'll also need to modify `buildup/wsgi.py` to look like this:
```python
import os
from django.core.wsgi import get_wsgi_application
os.environ.setdefault("DJANGO_SETTINGS_MODULE", "buildup.settings")
application = get_wsgi_application()

from whitenoise.django import DjangoWhiteNoise
application = DjangoWhiteNoise(application)
```
which will let `whitenoise` serve static files without Django having to worry
about it.
Now in `requirements.txt`, add the dependency on `whitenoise`:
```python
whitenoise==1.0.6
```
And finally run
```bash
sudo pip install whitenoise==1.0.6
```

Now we're all set for Heroku to serve static files! You can push your app and see
all the style changes you have added.
