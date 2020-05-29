+++
date = "2012-09-11 19:23:17+00:00"
draft = false
tags = []
title = "How Python makes learning simpler"
url = "/post/31347683771/how-python-makes-learning-simpler"
+++
Python is a simple language and developed with programmer’s productivity and code readability in mind. Learning new language would be simple to complicated depending on language syntax, wierdness and many other factors.

Its universally accepted best way to learn programming language is to write programs and rewrite again.

Python makes learning curve easier. Python has certain features which makes easier to learn inside interpreter.

*   `help(object) ` ,`` help `` is function which takes a `` object `` or `` function `` and tells what exactly the object documentation.`` help(2) `` will print `` integer `` class properties and methods in nicer format.


*

    `` dir(object) ``,`` dir `` is a function list of methods associated with a particular object, `` dir('python') `` lists all functions associated with string. `` ___all__ `` is magic methods and `` title `` is normal functions.


*

    `` import os;
    os.__file___ ``In order to find the location of the module, it is convenient to use `` modulename.__file__ `` will return path of the source file. Note certain modules can be `` .py `` files, others are `` .pyc ``, few are `` .so ``.



Next time you code in python, open your favorite text editor and python interpreter and try all the hacks in python interpreter and read documentation.

__Ipython pro tip__Inside Ipython `` %psource modulename `` prints source code of the module, useful in lot of cases.

Find out what `` modulename.__all__ `` does

`` You're almost as happy as you think you are. ``
