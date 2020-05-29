+++
date = "2014-02-09 20:44:31+00:00"
draft = false
tags = ["book-review", "django"]
title = "Two scoops of django"
url = "/post/76145181921/two-scoops-of-django"
+++
<a href="http://twoscoopspress.org/products/two-scoops-of-django-1-5" target="_blank">Two Scoops of Django -1.5</a> is book by <a href="https://twitter.com/pydanny" target="_blank">Pydanny</a> and <a href="https://twitter.com/audreyr" target="_blank">Audrey Roy</a> focusing on writing clean and better Django application.

If you are using Django in production this is must read book.

Q: I am using django since 0.8 do I need this book ?

A: Yes, consider the book as starting point to validate your assumption.

Q: I just started using django, should I read this ?

A: Yes. I started to use django in production last month. Sometimes I felt I should finish this book before pushing any code further. For every two or three chapters I can clearly find mistakes and fix it.

### Enjoyed pieces:

*   100% support for breaking dependencies and settings into multiple files. I always say this to my friends, never have single `` settings.py ``. Having private repo isn’t a solution to store secret information.
*   Never use `` Meta.exclude `` in Model forms. I did this mistake when I started.
*   Covering security pointers.
*   Advocacy for Class Based Views. Class based views makes code cleaner by breaking into relevant methods. It is easy to have fat function which can stretch to 10 to 15 lines while accpeting `` GET `` and `` POST ``.
*   Advicing not store to `` sessions, files, logs `` in DB. Django session will cause pain while migrating database (MySQL -> Postgres). It is better to store sessions in `` redis `` or `` riak ``.

### Conclusion:

If you are using django, read this book and see how many changes you are making to code. Worth every penny spent !
