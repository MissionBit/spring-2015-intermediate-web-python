Intro to Python
===============
Python is a popular language that we will be using in this course! If you're
coming from Ruby or Python, most concepts will be the same, although the syntax
may be different.

In your terminal, you can use the `python` command to start the Python interpreter.
This will allow you to run snippets of Python code and see what happens.
```bash
$ python
Python 2.7.7 (default, Jun 14 2014, 23:12:13) 
[GCC 4.2.1 Compatible Apple LLVM 5.1 (clang-503.0.40)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> print('foo')
foo
```

In Python, indenting incorrectly will create an invalid program.
Bad python code
```
if True:
print(1)
else:
print(2)
```

Good python code
```python
if True:
  print(1)
else:
  print(2)
```

#Comments
Any to the right of `#` in Python is a comment and will be ignored by the computer.
We use them to make notes and help other programmers understand what is happening.
```python
# this is a comment
# so is this! x = 1 never gets run
``` 

#Values
There are different types of values you can use in Python. The main types are:
* int (`1`, `-214`)
* float (`0.234`, `3.14159`)
* String (`"hello"`, `""`, `"this is a string with symbols (!@)!@#}{"`)
* Boolean (`True`, `False`)

## Numbers
A number is exactly what you would expect. You can do regular math with numbers in Python.
```python
1 + 1
 => 2

4 - 3
 => 1

2 * 2
 => 4

12 / 4
 => 3

# % is called modulus - it tells you the remainder after division
# for example 11/5 = 2 remainder 1
11 % 5
 => 1
```

## Strings
A string is any text surrounded by quotes. You can perform simple operations on strings:
```python
len("a string")
 => 8
```

## Booleans
A boolean is value that can be either true or false. You can use the words `True` and `False` directly, or you can compare other values and get a boolean as the result. You can also combine booleans using the or and and operators.
```python
1 > 3
 => False

#to compare for equality, use the '==' operator 
12.3 == 12.3
 => True

#to compare for inequality, use the '!=' operator 
16 != 42
 => True

"foo".length < 5
 => True

#If both boolean values are true, the result is True.
"foo".length < 5 and "foo" != "bad string"
 => True
"foobar".length < 5 and "foobar" != "bad string"
 => False

#If either boolean value is true, the result is True.
"foobar".length < 5 || "foobar" != "bad string"
 => True
"bad string".length < 5 || "bad string" != "bad string"
 => False
```

#Output
`print` will print out the value or variable that you give it.
```python
print(1)
1

print("asdf")
asdf

print(False)
False
```

#Variables
##Declaring a variable
The first time you want to use a variable, you must give it a name and value.
```python
x = 1
y = "baz"
somethingelse = True
```
##Using a variable
Once a variable is declared, you can use it anywhere you would use a value.
```python
x = 1
print(x)
1

x2 = x + 1
print(x2)
2
```

##Modifying a variable
You can also change the value of a variable after it has been set.
```python
x = "foo"
print(x)
foo

x = "buh this isn't foo anymore"
print(x)
buh this isn't foo anymore
```

#If Statements
An if statement lets you run different code depending on the condition. The condition must be any value or expression which results in a boolean value.
```python
if True:
  print("the condition is true")
else:
  print("the condition is false")
 => the condition is true

#you can also only have an if case
y = "foobar"
if y.length < 12:
  print("the condition is true")
 => the condition is true

y = "foobar"
if y.length > 12:
  print("the condition is true")
 => <Nothing happens>
```

# Functions
You can define functions with arguments similar to Javascript and Ruby
```python
def add1(n):
  return n+1

add1(5)
 => 6
```

If you want to learn more Python syntax, start with http://www.codecademy.com/tracks/python
