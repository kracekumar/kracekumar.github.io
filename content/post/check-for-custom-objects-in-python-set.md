+++
date = "2015-01-09 19:50:06+00:00"
draft = false
tags = ["python", "set"]
title = "Check for custom objects in Python set."
url = "/post/107618031945/check-for-custom-objects-in-python-set"
+++
Python set data structure is commonly used for removing duplicate entriesand make lookup faster (O(1)). Any hashable object can be stored in set.For example, `` list `` and `` dict `` can’t be stored.

User defined objects can be stored. Here is how it looks.

    class Person(object):
        def __init__(self, name, age):
            self.name, self.age = name, age


    In [25]: s = set()
    In [26]: s.add(Person('kracekumar', 25))

    In [27]: s
    Out[27]: set([<__main__.Person at 0x1033c5e10>])

    In [29]: Person('kracekumar', 25) in s
    Out[29]: False

### Implement equality check

Even though `` Person `` object with same value is present but check failed.This is because default python `` __eq__ `` checks for reference.

    class Person(object):
        def __init__(self, name, age):
            self.name, self.age = name, age

        def __eq__(self, other):
            return (isinstance(other, self.__class__) and
                getattr(other, 'name', None) == self.name and
                getattr(other, 'age', None) == self.age)

        def __hash__(self):
            return hash(self.name + str(self.age))

    In [38]: s = set()

    In [39]: s.add(Person('kracekumar', 25))

    In [40]: Person('kracekumar', 25) in s
    Out[40]: True

    In [41]: s
    Out[41]: set([<__main__.Person at 0x1033d0590>])

    In [42]: s.add(Person('kracekumar', 25))

    In [43]: s
    Out[43]: set([<__main__.Person at 0x1033d0590>])

`` __hash__ `` is used for calculating hash of the object and `` __eq__ `` is used by`` in `` during check after hash value match.
