+++
date = "2015-03-08 11:10:09+00:00"
draft = false
tags = ["python", "django"]
title = "django print exception to console"
url = "/post/113057636135/django-print-exception-to-console"
+++
Django has very good debug toolbar for debugging `` SQL ``. While working with `` Single Page Application `` and `` API `` exceptions can’t be displayed in browser. Exception is sent to front end. What if the exception can be printed to console ?

Django middleware gets called for every `` request/response ``. The small helper class looks like

<script src="https://gist.github.com/kracekumar/46ac62b2cffff12c72e0.js"></script>
Add the filename and class name to `` MIDDLEWARE_CLASSES `` in settings file like

<script src="https://gist.github.com/kracekumar/1a0543c48c80a5e01102.js"></script>

This is how exceptions looks

<script src="https://gist.github.com/kracekumar/20576674de2b620a5adc.js"></script>
