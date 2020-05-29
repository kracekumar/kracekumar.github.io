+++
date = "2014-10-19 11:14:08+00:00"
draft = false
tags = ["python", "global", "closure"]
title = "Python global keyword"
url = "/post/100399630630/python-global-keyword"
+++
Python’s `` global `` keyword allows to modify the variable which is out of current scope.

    In [13]: bar = 1

    In [14]: def foo():
    ....:     global bar
    ....:     bar = 2
    ....:

    In [15]: bar
    Out[15]: 1

    In [16]: foo()

    In [17]: bar
    Out[17]: 2

In the above example, `` bar `` was declared before `` foo `` function. global `` bar `` refers to the `` bar `` variablewhich is outside the `` foo `` scope. After `` foo `` invocation `` bar `` value was modified inside `` foo ``. The value ismodified globally.

What happens when `` bar `` becomes function inside `` foo `` ?

    In [19]: bar = None

    In [20]: def foo():
       ....:     global bar
       ....:     def bar():
       ....:         return 'I am bar'
       ....:

    In [21]: bar

    In [22]: foo()

    In [23]: bar
    Out[23]: <function __main__.bar>

`` bar `` was variable outside `` foo ``’s scope. Now creating a new function `` bar `` inside `` foo `` aliased `` bar `` functionto `` bar `` global variable.

This may not be obvious at first step, then you realize function can be aliased to variable.
