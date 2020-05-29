
+++
date = "2014-03-22 08:34:00+00:00"
draft = false
tags = ["python", "any", "next"]
title = "Stop iteration when condition is meet while iterating"
url = "/post/80342472437/stop-iteration-when-condition-is-meet-while"
+++
We are writing a small utility function called `` is_valid_mime_type ``. The function takes a `` mime_type ``as an argument and checks if the mime type is one of the allowed types. Code looks like

    ALLOWED_MIME_TYPE = ('application/json', 'text/plain', 'text/html')
    
    def is_valid_mimetype(mime_type):
        """Returns True or False.
    
        :param mime_type string or unicode: HTTP header mime type
        """
        for item in ALLOWED_MIME_TYPE:
            if mime_type.startswith(item):
                return True
        return False

Above code can refactored into single line using `` any ``.

    def is_valid_mimetype(mime_type):
        """Returns True or False.
    
        :param mime_type string or unicode: HTTP header mime type
        """
        return any([True for item in ALLOWED_MIME_TYPE
                                  if mime_type.startswith(item)])

One liner. It is awesome, but not performant. How about using `` next `` ?

    def is_valid_mimetype(mime_type):
        """Returns True or False.
    
        :param mime_type string or unicode: HTTP header mime type
        """
        return next((True for item in ALLOWED_MIME_TYPE 
                                  if mime_type.startswith(item)), False)

`` (True for item in ALLOWED_MIME_TYPE if mime_type.startswith(item) `` is <a href="https://stackoverflow.com/questions/1995418/python-generator-expression-vs-yield" target="_blank">generator expression</a>. When `` ALLOWED_MIME_TYPE `` is `` None `` or `` EMPTY `` exception will be raised. In order to avoid that `` False `` is passed as an argument to `` next ``.

### Edit:

    def is_valid_mimetype(mime_type):
        """Returns True or False.
    
        :param mime_type string or unicode: HTTP header mime type
        """
        return any(mime_type.startswith(item) for item in ALLOWED_MIME_TYPE)

Cleaner than `` Next ``.
