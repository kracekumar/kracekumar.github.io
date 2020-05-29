
+++
date = "2016-07-24 14:37:20+00:00"
draft = false
tags = ["python", "web", "exception"]
title = "HTTP Exception as control flow"
url = "/post/147895372680/http-exception-as-control-flow"
+++
As per <a href="https://en.wikipedia.org/wiki/Exception_handling" target="_blank">Wikipedia</a> , Exception handling is the process of responding to the occurrence, during computation, of exceptions – anomalous or exceptional conditions requiring special processing – often changing the flow of program execution.

In Python errors like `` SyntaxError ``, `` ZeroDivisionError `` are exceptions.Exception paves the way to alter the normal execution path.

While working with API, a web request goes through the following process,`` authentication, authorization, input validation, business logic `` and finally, the response is given out. Depending on complexity, few more steps can be involved.

Consider simple class based view. View looks similar to `` Django `` or `` Flask `` view.

<div class="gist"><a href="https://gist.github.com/kracekumar/ce25b463992b609a34eff7b92bd9c77a" target="_blank">https://gist.github.com/kracekumar/ce25b463992b609a34eff7b92bd9c77a</a></div>

The important point here is the number of if conditions inside `` post `` method. Though an another function can hold the code inside `` if validator.validate() `` block. Still, the `` if `` condition doesn’t go away.`` check_and_create_bucket `` returns a `` tuple ``. The caller needs to check the tuple and follow next steps. When the number of layers or functions increases, conditions also increases. These conditions are unnecessary and handled til bottom level.

On the other hand, if these `` function/method `` returns the object on success and raises `` exception ``, the code is easier to read. At a higher level, the exception can be handled.

Consider the modified code where all functions return `` an object `` or `` raise an exception ``.

<div class="gist"><a href="https://gist.github.com/kracekumar/78557dd6e2bf0e5176b3d6783300fdbb" target="_blank">https://gist.github.com/kracekumar/78557dd6e2bf0e5176b3d6783300fdbb</a></div>

In the above code, the validator raises an exception on any failure. Even if validator doesn’t raise an exception, the view will have only one `` if condition ``. At a higher level code is structured like a list of commands.

The illustrated example has only one business action post validation. When the number of business conditions increases, this method is smoother to manage.

Writing `` try/except `` code on every view is repetition. The little decorator can be handy.

<div class="gist"><a href="https://gist.github.com/kracekumar/eef4a2feb052c69bc2a536215756d9c6" target="_blank">https://gist.github.com/kracekumar/eef4a2feb052c69bc2a536215756d9c6</a></div>

<a href="https://wrapt.readthedocs.io/en/latest/" target="_blank">wrapt</a> is the useful library for writing decorators, wrappers, and monkey patching.

Designing code base around exceptions is highly useful. All the exceptions are handled at a higher level and reduce `` if/else `` complexity.

<a href="https://docs.djangoproject.com/en/1.9/ref/exceptions/" target="_blank">Django</a> and <a href="http://werkzeug.pocoo.org/docs/0.11/exceptions/" target="_blank">Flask</a> has inbuilt HTTP Exceptions support.
