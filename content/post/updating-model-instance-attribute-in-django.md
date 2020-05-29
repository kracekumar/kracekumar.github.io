
+++
date = "2014-02-08 09:01:00+00:00"
draft = false
tags = ["python", "django"]
title = "Updating model instance attribute in django"
url = "/post/75983294047/updating-model-instance-attribute-in-django"
+++
It is very common to update single attribute of a model instance (say update `` first name `` in user profile) and save it to db.

    In [18]: u = User.objects.get(id=1)
    
    In [19]: u.first_name = u"kracekumar"
    
    In [20]: u.save()

Very straight forward approach. How does django send the sql query to database ?

    In [22]: from django.db import connection
    
    In [22]: connection.queries
    Out[22]: 
    [... 
    {u'sql': u'UPDATE "auth_user" SET "password" = \'pbkdf2_sha256$12000$vsHWOlo1ZhZg$DrC46wq+a2jEtEzxmUEw4vQw8oV/rxEK7zVi30QLGF4=\', "last_login" = \'2014-02-01 06:55:44.741284+00:00\', "is_superuser" = true, "username" = \'kracekumar\', "first_name" = \'kracekumar\', "last_name" = \'\', "email" = \'me@kracekumar.com\', "is_staff" = true, "is_active" = true, "date_joined" = \'2014-01-30 18:41:18.174353+00:00\' WHERE "auth_user"."id" = 1 ', u'time': u'0.001'}]

Not happy. Honestly it should be `` UPDATE auth_user SET first_name = 'kracekumar' WHERE id = 1 ``. Django should ideally update modified fields.

Right way to do is

    In [23]: User.objects.filter(id=u.id).update(first_name="kracekumar")
    Out[23]: 1
    
    In [24]: connection.queries
    Out[24]:
    [...
    {u'sql': u'UPDATE "auth_user" SET "first_name" = \'kracekumar\' WHERE "auth_user"."id" = 1 ', u'time': u'0.001'}]

Yay! Though both queries took same amount of time, latter is better.

### Edit: There is one more cleaner way to do it.

    In [60]: u.save(update_fields=['first_name'])
    
    In [61]: connection.queries
    Out[61]: 
    [...
    {u'sql': u'UPDATE "auth_user" SET "first_name" = \'kracekumar\'  WHERE "auth_user"."id" = 1 ',
    u'time': u'0.001'}]
