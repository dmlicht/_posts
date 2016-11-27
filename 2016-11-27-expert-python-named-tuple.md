---
layout: post
title: "Use a named tuple instead of a data class"
comments: false
description: "Want to write cleaner python? In this post we'll explore using named tuples for data objects."
keywords: "python, namedtuple, clean code"
---


Do you ever find yourself writing classes simply to organize your data. The classes don't have any business logic. They just show that certain fields belong together. Think of a point on a two dimensional graph: A point doesn't need to do anything, but it has an `x` and `y` that belong together. Your point class could look like this:


```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

It feels like a lot of lines of code to simply say, `x` and `y` belong together, please let them be together! Is there a clearer and cleaner way to accomplish this? Yes! Enter named tuples. The above snippet could be as simple as:

    Point = namedtuple('Point', 'x y')

Let's unpack the line above. [`namedtuple`](https://docs.python.org/3/library/collections.html#collections.namedtuple) is a creating a tuple subclass with the typename `Point` with the fields `x` and `y`, we're taking this return value and storing it as `Point`. Now we can use Point just like if we had created it with an entire class.

    my_point = Point(10, 20)

### Let's look at an example file from the world of real code. 
The [halite](https://halite.io/) programming challenge has a file [htl.py](https://github.com/HaliteChallenge/Halite/blob/master/airesources/Python/hlt.py), with several such classes. Let's see if we can rewrite it using `namedtuple`


```python
class Location:
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

class Site:
    def __init__(self, owner=0, strength=0, production=0):
        self.owner = owner
        self.strength = strength
        self.production = production

class Move:
    def __init__(self, loc=0, direction=0):
        self.loc = loc
        self.direction = direction
```

Using `namedtuple`, we can convert each of these classes to one line of code.


```python
from collections import namedtuple
Location = namedtuple('Location', ['x', 'y']) # we define our tuples attributes with a list
Site = namedtuple('Site', ['owner strength production']) # We can also pass them in as a space delimited string
Move = namedtuple('Move', 'loc direction') # pretty nifty eh?
```

More concise, how nice? **Clear and brief code is what python is all about.**  
But wait, we just lost our default arguments.

As of now, `namedtuple` doesn't have a beautiful way to set argument defaults. It's quite easy though, if you don't mind getting your hands a little dirty. To create `Location` with argument defaults:


```python
Location = namedtuple('Location', ['x', 'y'])
Location.__new__.__defaults__ = (0,0)
```

It would be ugly to do this all the time, so let's wrap it in a handy little function:


```python
def namedtuple_defaults(typename, field_names, defaults):
    nt = namedtuple(typename, field_names)
    nt.__new__.__defaults__ = defaults
    return nt
```

Now we can do:


```python
Move = namedtuple_defaults('Location', 'x y', (0, 0))
```

Our final code will look like:


```python
from collections import namedtuple

def namedtuple_defaults(typename, field_names, defaults):
    nt = namedtuple(typename, field_names)
    nt.__new__.__defaults__ = defaults
    return nt

Location = namedtuple_defaults('Location', 'x y', (0,0))
Site = namedtuple_defaults('Site', 'owner strength production', (0,0,0))
Move = namedtuple_defaults('Move', 'loc direction', (0,0))
```

### What if you only want defaults for some of the arguments?
That's Ok! Defaults will be associated with the last arguments. For example, let's say you have four arguments `a`, `b`, `c`, `d` and you pass in two defaults `(10, 20)`. `a`, `b`, will remain required arguments while `c` and `d` will be optional arguments with defaults `10` and `20` respectively.

### Wrap Up

In this example, we've only saved ourself a few lines of code because we had to write a wrapper function. It's clear to see that the advantage to using `namedtuple` grows when we use more data objects. Conceptually, our code is cleaner because we've stripped out the boilerplate and repetition. 

#### Advantages
  + You can save yourself a lot of code if your project is filled with data objects.
  + Named tuples show their intent. If you are using one, its fully clear your object isn't meant to do anything, It's just here hold data and by used by other code. If you create a class, it's more ambigious. Your object could hold a little data here and do a little computing there, and the user will have to look at the class.

#### Disadvantages
 - Named Tuples aren't as commonly used as classes, so some pythonistas reading your code may have to learn a new concept to understand your work. This isn't too bad, because it's a pretty lightweight concept.
 - Handling default values is a bit ugly with namedtuple, though if you are doing it often, you could wrap the light uglyness in a function.
