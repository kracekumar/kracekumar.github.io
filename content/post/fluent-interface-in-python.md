+++
date = "2014-10-25 09:32:01+00:00"
draft = false
tags = ["python", "fluent interface", "method chaining"]
title = "Fluent interface in python"
url = "/post/100897281440/fluent-interface-in-python"
+++
<a href="https://en.wikipedia.org/wiki/Fluent_interface" target="_blank">Fluent Interface</a> is an implementation of API which improves readability.

### Example

`` Poem('The Road Not Taken').indent(4).suffix('Robert Frost') ``.

Fluent Interface is similar to method chaining. I was wondering how to implement this in `` Python ``.Returning `` self `` during method call seemed good idea .

    class Poem(object):
        def __init__(self, content):
            self.content = content

        def indent(self, spaces=4):
            self.content = " " * spaces + self.content
            return self

        def suffix(self, content):
            self.content += " - {}".format(content)
            return self

        def __str__(self):
            return self.content

    >>>print Poem('Road Not Taken').indent(4).suffix('Rober Frost').content
        Road Not Taken - Rober Frost

Everything seems to be ok here.

### Side effects

Above approach has side effect. We are mutating the same object during every method call.Consider the following code

    >>>p = Poem('Road Not Taken')
    >>>q = p.indent(4)
    >>>r = p.indent(2)
    >>>print str(q) == str(r)
    True
    >>>print id(q), id(r)
    4459640464 4459640464

Clearly this isn’t expected. `` q `` and `` r `` is pointing to same instance.Ideally we should create a new instance during every method call and return the object.

### New object

`` Decorator `` is a function which takes a `` function `` as argument and returns a `` function `` (most cases).

    from functools import wraps

    def newobj(method):
        @wraps(method)
        # Well, newobj can be decorated with function, but we will cover the case
        # where it decorated with method
        def inner(self, *args, **kwargs):
            obj = self.__class__.__new__(self.__class__)
            obj.__dict__ = self.__dict__.copy()
            method(obj, *args, **kwargs)
            return obj
        return inner


    class NPoem(object):
        def __init__(self, content):
            self.content = content

        @newobj
        def indent(self, spaces=4):
            self.content = " " * spaces + self.content

        @newobj
        def suffix(self, content):
            self.content += " - {}".format(content)

        def __str__(self):
            return self.content

    >>>p = NPoem("foo")
    >>>q = p.indent(4)
    >>>r = p.indent(2)
    >>>print str(q) == str(r)
    False
    >>>print(q)
        foo
    >>>print(r)
      foo
    >>>print id(q), id(r)
     4459642640 4459639248

In the above approach `` newobj `` decorator creates a new instance, copies all the atrributes,calls the method with arguments and returns new `` instance ``. Rather than using decoratorprivate function can do the same.

Fluent Interface is used heavily in SQLAlchemy and Django. <a href="https://github.com/django/django/blob/master/django/db/models/query.py#L954" target="_blank">Django</a>uses `` _clone `` method to create new `` QuerySet `` and<a href="https://github.com/zzzeek/sqlalchemy/blob/1cb94d89d5c4db7a914d9f30ebe0b676ab4e175b/lib/sqlalchemy/sql/base.py#L308" target="_blank">SQLAlchemy</a> usesdecorator based approach to create new `` Query `` instance.

Thanks <a href="https://twitter.com/kracetheking/status/525207441206558722" target="_blank">Mike Bayer</a> and <a href="https://twitter.com/kracetheking/status/525206909612085248" target="_blank">Daniel Roy Greenfeld</a>helping me understand this.

Do you know any other way to implement this ? Feel free to comment.
