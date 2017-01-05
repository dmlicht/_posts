---
layout: post
title: "Expert Python: Decorators"
comments: false
description: ""
keywords: ""
---

## Concepts
- decorators
- first class functions
- closures / scope / free variables / locals / globals / nonlocal
- @lrucache

`@` is a magic shorcut for:

    def wrapper(arg_func):
        def inner():
            print('this is inner')
            arg_func()
        return inner


Then you can use the decorator like so:

    @wrapper
    def my_func():
        print('this is my func')

Which is basically just a magic shortcut for

    def my_func():
        print('this is my func')
        
    my_func = wrapper(my_func)


## Okay Let's write a caching decorator

    def cache(func_to_cache):
        our_cache = {} # (1) this is our free variable
        def inner(*args): # (2) this function is a closure because it references
            if args in our_cache:
                return our_cache[args]
            else:
                result = func_to_cache(args)
                our_cache[args] = result
                return result
        return inner
        
Not bad right? 10 lines of code and you've created the infrastructure to cache any of your functions for given input arguments!
NOTE: We just ignored kwargs. Gotta handle these.

## @lrucache (least recently used cache)
But you don't even have to write this. `@lrucache` already exists and is provided in the python standard library. It does exactly this, with additional features. For example you can set how param sets you want to cache:

    @lrucache(32)
    def avg_two(x: int, y: int) -> int:
        return (x + y) / 2

## Decorator params
Wait, how did they get those cool params to pass to their decorator?

    def with_params(cache_n)
        def cache(func_to_cache):
            our_cache = {} # (1) this is our free variable
            def inner(*args): # (2) this function is a closure because it references
                if args in our_cache:
                    return our_cache[args]
                else:
                    result = func_to_cache(args)
                    our_cache[args] = result
                    return result
            return inner