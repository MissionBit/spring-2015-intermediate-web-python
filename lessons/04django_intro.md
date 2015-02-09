Django Intro
============

Django is a web framework that we will be using to build our webapps. It helps
you break down your app into components for storing data, returning content,
and all the logic in between.

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
on your screen.

# A quick primer on the web

## What happens when I visit a website?

When you open a website, your browser is requesting something (most likely an HTML file)
from the website's server. For example, when you visit `twitter.com/djangoproject`,
it looks something like this:
 * Browser: Yo `twitter.com`, can I have the page for `/djangoproject`?
 * Twitter: Ya sure here it is
 * Browser: K thanks, I'm gonna display this on the screen now

## How does Django make this easier?

Django is built to help you organize the components of your web application
in a way that's easy to extend. When Django gets a new request, it first tries
to "resolve" the URL and figure out what you're asking for. Once it knows
what you're requesting, it returns a "view" which is how Django responds with data.
In the Django world, a browser request would look
something like this:
![Django stack 1](https://raw.githubusercontent.com/MissionBit/spring-2015-intermediate-web-python/master/img/django1.png)
