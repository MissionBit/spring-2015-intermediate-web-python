Turtle Graphics
===============

#Setup
Open your terminal by searching for it in Spotlight on the top right. We are going
to make a directory for you to put all of your projects in. Type
```bash
mkdir projects
```
to create the directory and then
```bash
cd projects
```
to enter it. Now lets create and enter a folder for this project
```bash
mkdir turtle
cd turtle
```

Now in Brackets or your text editor of choice, open the turtle folder with
`File -> Open Folder`. Press `Cmd-Shift-H` to jump to your home directory and
open the `turtle` folder inside `projects`. 

Now create a new file with `Cmd-N` and paste the following
```python
import turtle
winston = turtle.Turtle()


turtle.done()
```
Now save the file with `Cmd-S` and call it `winston.py`.

When we say `import turtle` we're telling python to load the code that comes with
the "module" called turtle. The second line creates a turtle that we can control,
and the last line keeps the window open until you close it yourself.

Now we're ready to run some code! In your terminal, navigate to the turtle folder
using `cd` and then run `winston.py` in Python:
```python
python winston.py
```
You should see a window open with your "turtle" in the center. You can close the window
to exit the program.

# Turtle Commands
Every turtle (in our case `winston`, or whatever you named it) can listen for commands
like the following:
```python
winston.forward(100)
winston.right(90)
winston.backward(100)
```

Try adding this snippet in the empty space before `turtle.done()` and see what it does.

Winston can respond to the following commands:
 * `forward` or `fd`: takes a distance in pixels to move forward
 * `backward` or `bk`: takes a distance in pixels to move backward
 * `left` or `lt`: takes an angle in degrees to turn left
 * `right` or `rt`: takes an angle in degrees to turn right
 * `write`: takes a string to write to the screen
 * `setposition` or `goto`: moves the turtle to the given (x,y) coordinates
 * `penup` or `pu`: when the turtle moves, no line will be drawn behind it
 * `pendown` or `pd`: when the turtle moves, a line will be drawn behind it
 * `pensize`: takes the width in pixels of the pen
 * `color`: takes 2 colors as strings for the pen and fill color
    ```python
    winston.color('red', 'blue')
    ```
 * `fill`: use this before and after drawing a shape to tell Winston to fill it
    ```python
    winston.fill(True)

    # now draw a triangle
    winston.forward(100)
    winston.right(60)
    winston.forward(100)
    winston.right(60)
    winston.forward(100)

    winston.fill(False)
    ```

You can see a full list of commands and examples here: https://docs.python.org/2/library/turtle.html#turtle-methods

# Drawing Functions
Now using these commands we can draw shapes pretty easily. To draw a square, we repeat
a forward and right command 4 times:
```python
winston.forward(100)
winston.right(90)
winston.forward(100)
winston.right(90)
winston.forward(100)
winston.right(90)
winston.forward(100)
winston.right(90)
```
which can be simplified by using a for loop
```python
for i in range(4):
  winston.forward(100)
  winston.right(90)
```
We can take this code and turn it into a function that we can reuse. The function
`square` will provide the size of the sides and winston will draw it
```python
def square(size):
  for i in range(4):
    winston.forward(size)
    winston.right(90)
```
and now if you call `square` with different sizes!

Now try writing the following function
```python
def polygon(n, size):
  # n is the number of sides for this polygon
  # size is the length of the sides
  # hint: can you find a formula for the size of the angle given the number of sides?
```

You can also add pen width, pen color and fill color to your polygon function.

# Make some art
Now let's make a picture using this function! Draw something cool and post it on
https://github.com/MissionBit/spring-2015-intermediate-web-python/issues/2
Remember that you can use for loops and functions to avoid copying and pasting
instructions!
