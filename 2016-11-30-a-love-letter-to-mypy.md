---
layout: post
title: "A love letter to mypy"
comments: false
description: ""
keywords: ""
---

I love [mypy](http://mypy-lang.org/). **Optional Static Types** are a game changer for python. They have melted away my least favorite thing about my favorite language. It is HARD to keep track of what things are supposed to go in and come out of functions. Documentation can be stale or non existent (although type hints can also be non-existent, its tougher for them to be stale because your code won't pass the type check). I often find myself dropping into the debugger or running snippets of code in jupyter just to have some sense of what type of data is flowing through where. This can becomes even more challenging when you are collaborating on a large codebase.

Here's an example. What if I want to make a function to tell everyone my age. It's pretty easy right?

    def yo_this_is_my_age(age):
        print("Hey readers, this is my age: ", age)

Ok that's cool. But I can put a lot of stuff in there. I can put an `int`, a `str`, `float`, anything goes. But putting a random string in there doesn't make sense. I don't want someone to think they can use my function for nonsense like:

    yo_this_is_my_age("hungry")
    
Maybe I want to say my exact age so everyone knows I'm 25.65 years old. Maybe I'm self-conscious about it and I want to round down. Let's try that. We can use type hints to show how we expect our function to be used.

    def yo_this_is_my_age(age: int):
        print("Hey readers, this is my age: ", age)
        
Now, if someone tries to tell everyone my age without rounding down, the type checker is going to yell at them for me. 

    yo_this_is_my_age(25) # A-OK
    yo_this_is_my_age(25.65) # RAWR get out of here.

That makes me feel good. Type hints allows your code to self documenting, it makes a whole class of bugs float away effortlessly.

## Let's dig in to what we can use as type hints.
- [Built In Types](#built-in-types)
- [Classes](#classes)
- typing classes
- [Aliases](#aliases)
- [NewTypes](#newtypes)
- [Callables](#callables)

## Pycharm
This guide is going to describe a type hinting workflow using PyCharm. This is optional. You can use the mypy tool if you don't use PyCharm. Also it may be supported in other IDE's. This is out of the scope of this post.

## Built In Types
`int`  
`str`  
`float`  

## Classes
Just drop in your classes, they'll work. You'll have to import them. Let's say you want to make a tree out of some leaves. I know thats not how nature works, but suspend your disbelief. You wrote your leaves already in another file.

    from typing import List
    from leaf import Leaf

    class Tree():
        def init(self, leaves: List[Leaf]):
            self.leaves = leaves

easy dang peasy. Now your program knows what leaves means. 

### Hey but... what if I want to reference a class you are currently defining a type hint??
This one confused me at first, but it's actually not bad. Just reference the type as a string. If you're using PyCharm, you'll get a slightly different error (`unresolved reference`) if something goes wrong. Let's say your tree wants to keep track of all his tree buddies, it would look like this:

    from typing import List
    from leaf import Leaf

    class Tree():
        def init(self, leaves: List[Leaf], best_buddies: List['Tree']):
            self.leaves = leaves
            self.best_buddies = best_buddies


## Typing
- Dict
- Tuple
- List
- Iterable

## Aliases
To be honest, I don't feel like you get a ton of milage from these. It obscures the clarity advantage of having your types in the function declaration. Maybe if you can define it right next to the function it can be useful? But if your signature is so complex, it may warrant its own `namedtuple` or class. The example on the python website is:

    from typing import Tuple
    Address = Tuple[str, int]
    ...

They basically use aliases to compose very complex types. I haven't had the chance to use this yet, but I suspect if your data structures benefit from the clarity of type aliases, they probably benefit from the clarity of being organized into objects.
    
## NewType
This is basically a shortcut for making a subclass. 

    from typing import NewType
    Age = NewType("Age", int)
    
which allows you to do something nice like:

    my_age = Age(25)
    
and define nice functions like:

    def lie_about_my_age(age: Age):
        print("I'm in my early ", (age % 10) * 10, "s")

which will accept arguments like:

    my_age = Age(25)
    lie_about_my_age(Age(25))

but not:

    lie_about_my_age(25)
    
    
Why would we wan't to do this? The argument is clarity. When we create the variable we can show clearly what it is for and which functions it is meant to interact with. You could also argue, you are able to achieve most of the same clarity simply by choosing a good variable. This comes down to the situation and personal tastes. It allows your code to be more self documenting which can be useful if its appropriate, but can also lead you down a path of java-esqe redundancy, if you needlessly use it.


# mypy
tool for checking type annotations in python code

works with python 2 and 3
to support both python 2 and 3 you use the comment syntax

## It's easy to move your projects to static typing
You can do it gradually. Some functions can declared types while others don't in the same project.

Motivation - other people were coming up with 3rd party tools for checking and describing types for function args and return values
We type check off-line, like using a linter. We don't type check at runtime. Doing this still gives us many of the advantages of type checking while writing the code.

[Mypy](http://mypy-lang.org/) - the defactor type hints typechecker

## references
[Zulip blog post](http://blog.zulip.org/2016/10/13/static-types-in-python-oh-mypy/)
[Mypy cheat sheet](http://mypy.readthedocs.io/en/latest/cheat_sheet.html)
[PEP 484 - Type Hints](https://www.python.org/dev/peps/pep-0484/)
[typing python 3 doc](https://docs.python.org/3/library/typing.html)
[typing.py](https://hg.python.org/cpython/file/3.5/Lib/typing.py)
[Pycon Talk](https://www.dropbox.com/s/efatwr0pozsargb/PyCon%20mypy%20talk%202016.pdf?dl=0)
[Type Hinting in pycharm](https://blog.jetbrains.com/pycharm/2015/11/python-3-5-type-hinting-in-pycharm-5/)