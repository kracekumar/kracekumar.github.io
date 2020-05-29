+++
date = "2013-03-16 15:31:21+00:00"
draft = false
tags = ["python", "variable", "parallel-assignment"]
title = "Python parallel assignment"
url = "/post/45502206532/python-parallel-assignment"
+++
Python supports parallel assignment like

    >>> lang, version = "python", 2.7
    >>> print lang, version
    python 2.7

values are assigned to each variable without any issues.

    >>> x, y, z = 1, 2, x + y
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    NameError: name 'x' is not defined

First python tries to evaluate `` x + y `` expression. Since `` x, y `` is defined in same line, python is unable to access the variable `` x `` and `` y ``, so `` NameError `` is raised.

    >>> x, y = 1, 2
    >>> z, a = x + y, 65
    >>> print x, a
    1 65

In above code `` x, y `` is referenced before so `` x + y `` is evaluated and the value is assigned to `` z ``.

__So don’t assign the values in same line and use it in expression__
