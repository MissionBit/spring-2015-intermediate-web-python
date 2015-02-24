Deploying to Heroku
===================

# Deployment
This entire time we've been running our web application on our own machine. But
if we want others to be able to use it, we need to put our code on a server that
has a public IP address (and preferably a DNS pointing a domain name to that
IP). The process of getting application code onto running servers is commonly
called "depoyment".

A couple of options exist for running servers. Either you can buy your own
hardware and internet connection (commonly colocated with other people's servers
in a storage facility called a "colo") or you can provision servers from a
cloud-based service like Amazon Web Services.

# What is the cloud?
You've probably heard the word "cloud" used, but what actually is "the cloud"?
It's pretty simple actually. Companies like Amazon have huge buildings full of
servers that are just waiting around to be used. Collectively, these servers are
called a "cloud". When somebody wants to use one of Amazon's servers, Amazon
assigns them a random server of the type they want. When the user is done with
the server, Amazon makes it available for somebody else to use, and the user is
only charged for the amount of time they were using the server. This means that
whether you're using 1 server or 100 servers, it's simple to get them at the
click of a button for an hour or for a year.

# Heroku
Heroku is a platform build on top of Amazon's cloud that makes it simple to
deploy and run web applications. After adding some files to our repository to
tell Heroku about our application, deploying is as easy as `git push heroku
master`. Let's get started:

1. [Sign up for Heroku](https://signup.heroku.com/www-header)
2. [Download](https://toolbelt.heroku.com/download/osx) and install the Heroku
   toolbelt. (This will put the `heroku` command on your path.)
3. In the shell, use the toolbelt to log in to Heroku:
```bash
$ heroku auth:login
Enter your Heroku credentials.
Email: my@email.com
Password (typing will be hidden): sekret
Authentication successful.
```

4. In your application's root directory, create the Heroku app:
```bash
$ heroku create
Creating falling-wind-1624... done, stack is cedar-14
http://falling-wind-1624.herokuapp.com/ |
https://git.heroku.com/falling-wind-1624.git
Git remote heroku added
```

# Heroku Postgres
So far we've been using sqlite as our database, but it's a bad idea to use it in
production for a number of reasons. Luckily, Heroku apps already come with a
database called Postgres. Heroku encourages users to store all of their app's
configuration in environment variables, and that's what they've done with the
Postgres connection information:

```bash
$ heroku config:get DATABASE_URL
postgres://user3123:passkja83kd8@ec2-117-21-174-214.compute-1.amazonaws.com:6212/db982398
```

But we need to tell Django how to use `DATABASE_URL` to connect to our database.
Open up `requirements.txt` and add:

```python
dj-database-url==0.3.0
psycopg2==2.5.4
```

to the end. Then `pip install` your requirements again:

```bash
$ pip install -r requirements.txt
```

Now open `buildup/settings.py`, find the lines that assign the `DATABASES`
setting, and replace them with a version that uses `dj-database-url`:
```python
import dj_database_url
DATABASES = {'default': dj_database_url.config()}
```

After committing our changes, we're all set to deploy to Heroku:

```bash
$ git push heroku master
```

# Heroku One-Off Commands
Now Heroku has our code, but before we can run our application, we need to tell
Heroku to run our database migrations. `heroku run` can be used to execute
commands remotely in the cloud. Let's use it to migrate our Heroku Postgres
database:

```bash
$ heroku run python manage.py migrate
```

We can use `heroku run` to run any command we want. For instance, if we wanted
to open a Django shell on a Heroku server, we'd do it the same way we do
locally:

```bash
$ heroku run python manage.py shell
```

# Heroku Dynos
Each process running on Heroku runs in a container they call a "dyno". We tell
Heroku what kinds of processes we want to run by creating a file called
`Procfile` and giving each process type a name. The Procfile we're using has 1
dyno type called `web`:

```yaml
web: gunicorn buildup.wsgi --log-file -
```

After we've deployed an application containing a Procfile, we can tell Heroku to
scale each process type up or down to the number of dynos we want. Heroku gives
us 1 dyno for free, but beyond that they charge, so let's just scale up 1 web
dyno:

```bash
$ heroku ps:scale web=1
```

Now our web application should be up and running. Use the toolbelt to open it in
a browser:

```bash
$ heroku apps:open
```

# Local Development with Environment Variables
Remember when we replaced the `DATABASES` setting to use `dj-database-url`? It
relies on an environment variable called `DATABASE_URL` to run. Since we don't
have one set locally yet, you may have noticed that runserver is broken. To make
developing locally with environment variables easier, let's use a program called
Foreman. Download and run the
[OS X installer](http://assets.foreman.io/foreman/foreman.pkg) and open a new
terminal window. You should now have the `foreman` command on your path.

Foreman looks for a file called `.env`. When it finds that special file, it
loads everything it finds into the environment before running our program. In
the our root project directory, let's create a `.env` file and add our
`DATABASE_URL` environment variable to it:

```bash
DATABASE_URL=sqlite:///buildup.db
```

Now we can run our normal runserver using foreman:

```bash
$ foreman run python manage.py runserver
```

Foreman also knows about your Procfile, so you can use it to easily run commands
just like you would on Heroku:

```bash
$ foreman run web
```

Note that the webserver we're running in our Procfile doesn't automatically
restart when you make changes to our web application, so be sure to manually
restart each time if you run your webserver this way.

As a shortcut, Foreman has a `start` command that will run 1 process of each
type in your Procfile. Since we only have 1 type (web) in ours, `foreman run
web` is functionally equivalent to `foreman start`.
