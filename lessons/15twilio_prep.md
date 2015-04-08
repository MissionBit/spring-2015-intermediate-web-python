Twilio Prep
===========

In preparation for the Twilio workshop next week, we need to prepare a couple
things. We will be building a Flask application which connects to the Twilio API
and allows you to send and recieve text messages and phone calls.

## Create a Twilio Account
Go to http://twilio.com/user/account, click "Sign up for free", and complete the 
registration process.

## Set up Twilio on your machine
Open the terminal and create a folder which we will use in the workshop.
```bash
mkdir twilio
cd twilio
```
Additionally, lets install the Twilio Python library and Flask
```bash
sudo pip install twilio
sudo pip install Flask
```

## Set up Ngrok
Ngrok is a tool for "tunnelling" traffic to a local webserver. Normally when you
run a server on your machine (like with `python manage.py runserver`), only you
and possibly people on your network can access it. Ngrok forwards traffic from
a public domain to your local server, which allows anyone in the world to acces it!

In your project folder, download and unzip Ngrok.
```bash
curl "https://api.equinox.io/1/Applications/ap_pJSFC5wQYkAyI0FIVwKYs9h1hW/Updates/Asset/ngrok.zip?os=darwin&arch=amd64&channel=stable" -o ngrok.zip
unzip ngrok.zip
```

## How Twilio works
You can control control Twilio through an API, or Abstract Public Interface. Thats
just a fancy way of saying that there is a list of public URLS which you can interact
with by sending data. For example, you could send a request to the URL http://api.example.com/send_email
with the destination email address, subject, and body of an email and the server
running example.com could send the email for you. 

With Twilio, you can send and recieve texts and phone calls via an API. You can see
the full list of actions here: https://www.twilio.com/docs/api/rest. Another powerful
feature that Twilio uses is webhooks. When you set up your Twilio account, you can provide
a webhook URL. When Twilio receives a text or call that is meant to reach your
account, it will send a request to your webhook url. Then your server can decide
how it wants to respond based on the text of the message, the time, or whatever
else you want. You can see Twilio's full webhook documentation here: https://www.twilio.com/platform/webhooks

We will be building a webapp to respond to these requests. You can run it locally,
and with Ngrok, you will get a public URL for your app. This URL will be used
for the Twilio webhooks and whenever your Twilio number gets a text or phone call,
it will ask your webapp how to respond.
