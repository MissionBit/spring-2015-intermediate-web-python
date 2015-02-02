Intro to the Terminal
=====================

What is a terminal?
-------------------
Before Facebook and selfies and the internet, the first computers had displays
which could only show text. All interactions with the computer were done through
the "terminal", which is a very simple way to run commands on your computer. Nowadays,
most people don't need to use terminals because we have fancy graphical
apps that have the same functionality. 

For programming, it is often useful to use the terminal so we're going to learn
a couple of terminal commands. Each of these commands can be done without a terminal,
but you can think of using the terminal as lifting the hood of your car - you can
do a lot more stuff and you can see what's happening, but you can also break things
a lot faster!

Where is the terminal?
----------------------
All Macs ship with a terminal that you can use - to find it, search for `Terminal`
in Spotlight.

What do I do with this thing?
-----------------------------
You should see a line like
```bash
blahblahblah $
```
in your terminal. This is the prompt and you can type in commands here. Try typing
the following command and you should see your username printed out
```bash
$ whoami
cookiemonster
```
Note that the `$` is just a convention to denote that what follows is a terminal command.
You don't want to type the `$` into the terminal.

That's your first terminal command! Another useful command is `man`, which is
short for manual. Try the following
```bash
$ man whoami
```
which should print the manual for the command `whoami`. To exit the manual, press `q`.
Now let's go through the common commands that you'll be using.

ls
--
`ls` will list the files in the current folder (in terminal speak, this is usually
called a directory). If you run it, you should see something like this
```
$ ls
Applications Desktop Documents Downloads Library Movies Music Pictures Public
```
If you open Finder, you'll notice that the output matches up with what is in your
home directory - when the terminal starts, it automatically opens here.

pwd
---
`pwd` will print the working directory.
```bash
$ pwd
/Users/cookiemonster
```

cd
--
`cd` will change the directory. Note that `ls` and `pwd` will reflect the change
```bash
$ pwd
/Users/cookiemonster
$ ls
Applications Desktop Documents Downloads Library Movies Music Pictures Public
$ cd Downloads
$ pwd
/Users/cookiemonster/Downloads
$ ls
file-that-i-downloaded.jpg
```

Other commands
--------------
You can use `man` to learn more about these commands:
 * `mkdir`
 * `touch`
 * `cp`
 * `mv`
 * `rm`
