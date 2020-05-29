
+++
date = "2014-05-22 20:35:00+00:00"
draft = false
tags = ["python", "unicode"]
title = "python source fileencoding"
url = "/post/86530203730/python-source-fileencoding"
+++
Some of the python source file starts with `` -*- coding: utf-8 -*- ``. This particular linetells python interpreter all the content (byte string) is `` utf-8 `` encoded. Lets see how it affects the code.

`` uni1.py ``:

    # -*- coding: utf-8 -*-
    print("welcome")
    print("animé")

`` output ``:

    ➜  code$ python2 uni1.py
       welcome
       animé

Third line had a accented character and it wasn’t explictly stated as unicode. `` print `` function passed successfully.Since first line instructed interpreter all the sequences from here on will follow `` utf-8 ``, so it worked.

What if first line was missing ?

`` uni2.py ``

    print("welcome")
    print("animé")

`` output ``:

<pre><code>code$  python2 uni2.py
File "uni2.py", line 2
SyntaxError: Non-ASCII character '\xc3' in file uni2.py on line 2, but no encoding declared; see <a href="http://www.python.org/peps/pep-0263.html" target="_blank">http://www.python.org/peps/pep-0263.html</a> for details
</code></pre>

Now python complains that Non-ASCII character is found since default encoding is ASCII.More about source encoding can be found in <a href="http://legacy.python.org/dev/peps/pep-0263/" target="_blank">PEP 263</a>

Always set `` encoding `` in first or second line of python file.
