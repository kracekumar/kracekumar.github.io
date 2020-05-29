+++
date = "2014-11-02 01:17:42+00:00"
draft = false
tags = ["python", "class", "decorator"]
title = "class as decorator"
url = "/post/101540141725/class-as-decorator"
+++
### Decorator

Decorator is a callable which can modify the `` function, method, class `` on runtime. Most ofthe decorators uses `` closure `` but it is possible to use `` class ``.

### Closure

    import functools


    def cache(f):
        storage = {}

        @functools.wraps(f)
        def inner(n):
            value = storage.get(n)
            if value:
                print("Returning value from cache")
                return value
            value = f(n)
            storage[n] = value
            return value
        return inner


    @cache
    def factorial(n):
        if n <= 1:
            return 1
        return n * factorial(n - 1)

    >>>factorial(20)
    2432902008176640000
    >>>factorial(20)
    Returning from cache
    2432902008176640000

`` cache `` is a function which takes `` function as an argument `` and returns a `` function ``.`` factorial `` is a function which calculates the factorial of a given number, decoratedby `` cache ``. If factorial of a number is calculated or less than already calculatednumber it is retrieved from `` storage ``.

The `` cache `` function can be written using `` Class ``.

### Class

    import functools


    class Cache(object):
        def __init__(self, func):
            self.func = func
            self.storage = {}

            functools.update_wrapper(self, func)

        def __call__(self, n):
            value = self.storage.get(n)

            if value:
                print("Returning from cache")
                return value

            value = self.func(n)
            self.storage[n] = value
            return value


    @Cache
    def factorial(n):
        if n <= 1:
            return 1
        return n * factorial(n - 1)


    >>>factorial(20)
    2432902008176640000
    >>>factorial(20)
    Returning from cache
    2432902008176640000

`` Cache `` class has two dunder methods, `` __init__, __call__ ``. `` __init__ `` methodis called when interpreter encounters `` @Cache `` and `` __call__ `` is calledwhen `` factorial `` function is called.
