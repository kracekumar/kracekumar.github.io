
+++
date = "2013-09-13 17:50:32+00:00"
draft = false
tags = ["python", "coverage", "unit testing"]
title = "coverage.py to test web application coverage without writing tests"
url = "/post/61125040813/coveragepy-to-test-web-application-coverage"
+++
Tests are mandatory for packages, modules, web apps. If you are lazy to write unit tests but wanted to see the coverage for web app here is the short cut.

Lets consider a Python web app uses Flask, `` runserver.py `` runs the development web server. Normally server is started by the command `` python runserver.py ``. Now use `` coverage run runserver.py ``.

`` coverage `` python package will run the python program til program exits or user halts. Once web app testing is complete via browser, run `` coverage report -m `` this will produce big list of lines covered in modules during execution.

One more handy feature is `` coverage html ``. This produces html file containing python code highlighting executed line and other. All the html files is produced inside `` hmtlcov `` directory.

Coverage <a href="http://nedbatchelder.com/code/coverage/" target="_blank">documentation</a>.

## Sample Output

<pre><code>➜ hacknight git:(issue-249) ✗ coverage run --source=hacknight runserver.py
/Library/Python/2.7/site-packages/flask_cache/__init__.py:100:   UserWarning: Flask-Cache: CACHE_TYPE is set to null, caching is  effectively disabled.
warnings.warn("Flask-Cache: CACHE_TYPE is set to null, "
* Running on <a href="http://0.0.0.0:8100/" target="_blank">http://0.0.0.0:8100/</a>
* Restarting with reloader
/Library/Python/2.7/site-packages/flask_cache/__init__.py:100: UserWarning: Flask-Cache: CACHE_TYPE is set to null, caching is effectively disabled.
warnings.warn("Flask-Cache: CACHE_TYPE is set to null, "
127.0.0.1 - - [13/Sep/2013 23:08:31] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [13/Sep/2013 23:08:34] "GET /kracekumar HTTP/1.1" 200 -
127.0.0.1 - - [13/Sep/2013 23:08:36] "GET /kracekumar/jsfoo-hackday HTTP/1.1" 200 -
^C%                                                                                       
➜  hacknight git:(issue-249) ✗ coverage report -m
Name                           Stmts   Miss  Cover   Missing
------------------------------------------------------------
hacknight/__init__                28      0   100%
hacknight/_version                 3      0   100%
hacknight/forms/__init__           0      0   100%
hacknight/forms/comment           12      0   100%
hacknight/forms/event             42      5    88%   58-59, 62-72
hacknight/forms/participant       12      0   100%
hacknight/forms/profile            6      0   100%
hacknight/forms/project            9      0   100%
hacknight/forms/sponsor            9      0   100%
hacknight/forms/venue             16      0   100%
hacknight/models/__init__         10      0   100%
hacknight/models/_profile         21     21     0%   3-37
hacknight/models/comment          61     18    70%   34-35, 41, 71-72, 78-91, 95, 98
hacknight/models/event           110     43    61%   45, 48-51, 84, 87-88, 91-93, 96-97, 100-109, 112-133
hacknight/models/participant      31      5    84%   34-37, 41
hacknight/models/project          72     31    57%   42-46, 50, 55, 58, 63, 68-85, 101-105
hacknight/models/sponsor          17      2    88%   25-26
hacknight/models/user             21      5    76%   20, 24, 28, 32, 35
hacknight/models/venue            21      2    90%   26-27
hacknight/models/vote             38     16    58%   17-18, 21-30, 33-36, 39
hacknight/tests/__init__           0      0   100%
hacknight/tests/test_data          3      3     0%   3-7
hacknight/tests/test_models       17     17     0%   4-93
hacknight/views/__init__           8      0   100%
hacknight/views/event            244    192    21%   28-33, 41-56, 70-87, 97-116, 131, 140-145, 155-182, 191-222, 231-260, 269-276, 285-301, 310-313, 325-344, 354-373
hacknight/views/index             36     23    36%   13-15, 20, 25, 30, 35, 40, 45-53, 58-65
hacknight/views/login             28     12    57%   15, 21-22, 28-30, 36-37, 42-45
hacknight/views/profile           36     22    39%   17-23, 30-47
hacknight/views/project          323    256    21%   30-49, 61-73, 85-108, 118, 127-240, 252-259, 270-277, 288-295, 306-315, 327-336, 346-357, 369-378, 387-397, 406-416, 426-440, 451-464, 475-501, 514
hacknight/views/sponsor           45     26    42%   18-30, 42-53, 65-68, 81
hacknight/views/venue             47     29    38%   15-16, 22, 28-38, 45-56, 64-66
hacknight/views/workflow          84     30    64%   42-46, 52, 57, 62, 67, 72, 77, 80, 114-119, 127-128, 136, 144, 152, 160, 168, 176, 182, 188, 194, 200, 206, 209
------------------------------------------------------------
TOTAL                           1410    758    46%
</code></pre>
