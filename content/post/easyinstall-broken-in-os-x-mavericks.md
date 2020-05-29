+++
date = "2013-11-22 17:02:21+00:00"
draft = false
tags = ["python", "easy_install", "setuptools"]
title = "easy_install broken in os x mavericks  "
url = "/post/67761708966/easyinstall-broken-in-os-x-mavericks"
+++
I hardly use `` easy_install ``. Nowadays all the python requirements are installed via `` pip ``.

IPython is my primary python console. After installing mavericks, I installed IPython and fired IPython console. Following warning message appeared

    ➜  ~  ipython
    /Library/Python/2.7/site-packages/IPython/utils/rlineimpl.py:94: RuntimeWarning:
    ******************************************************************************
    libedit detected - readline will not be well behaved, including but not limited to:
       * crashes on tab completion
       * incorrect history navigation
       * corrupting long-lines
       * failure to wrap or indent lines properly
    It is highly recommended that you install readline, which is easy_installable:
         easy_install readline
    Note that `pip install readline` generally DOES NOT WORK, because
    it installs to site-packages, which come *after* lib-dynload in sys.path,
    where readline is located.  It must be `easy_install readline`, or to a custom
    location on your PYTHONPATH (even --user comes after lib-dyload).
    ******************************************************************************
      RuntimeWarning)
    Python 2.7.5 (default, Aug 25 2013, 00:04:04)
    Type "copyright", "credits" or "license" for more information.

IPython complains `` readline `` is missing and insisting to use `` easy_install ``. Then I tried

    ➜   ~  sudo easy_install-2.7 --upgrade setuptools
    Traceback (most recent call last):
      File "/usr/bin/easy_install-2.7", line 7, in <module>
        from pkg_resources import load_entry_point
      File "/Library/Python/2.7/site-packages/pkg_resources.py", line 2797, in <module>
        parse_requirements(__requires__), Environment()
      File "/Library/Python/2.7/site-packages/pkg_resources.py", line 576, in resolve
        raise DistributionNotFound(req)
    pkg_resources.DistributionNotFound: setuptools==0.6c12dev-r88846

I know I can’t use pip, but `` distribute `` is already installed.

Now only way to install readline is to use `` easy_install `` in Python site-packages. Normally `` easy_install `` is installed in `` /usr/bin ``.

<pre><code>➜   ~  which easy_install
/usr/bin/easy_install
➜  ~  sudo python /Library/Python/2.7/site-packages/easy_install.py readline
Searching for readline
Reading <a href="https://pypi.python.org/simple/readline/" target="_blank">https://pypi.python.org/simple/readline/</a>
Best match: readline 6.2.4.1
Downloading <a href="https://pypi.python.org/packages/2.7/r/readline/readline-6.2.4.1-py2.7-macosx-10.7-intel.egg#md5=6ede61046a61219a6d97c44a75853c23" target="_blank">https://pypi.python.org/packages/2.7/r/readline/readline-6.2.4.1-py2.7-macosx-10.7-intel.egg#md5=6ede61046a61219a6d97c44a75853c23</a>
Processing readline-6.2.4.1-py2.7-macosx-10.7-intel.egg
creating /Library/Python/2.7/site-packages/readline-6.2.4.1-py2.7-macosx-10.7-intel.egg
Extracting readline-6.2.4.1-py2.7-macosx-10.7-intel.egg to /Library/Python/2.7/site-packages
Adding readline 6.2.4.1 to easy-install.pth file

Installed /Library/Python/2.7/site-packages/readline-6.2.4.1-py2.7-macosx-10.7-intel.egg
Processing dependencies for readline
Finished processing dependencies for readline

➜  ~  ipython
Python 2.7.5 (default, Aug 25 2013, 00:04:04)
Type "copyright", "credits" or "license" for more information.

IPython 1.1.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.
</code></pre>

Whenever issue pops up with `` easy_install `` and `` setuptools ``, remember using `` easy_install `` from `` site packages `` like `` sudo python /Library/Python/2.7/site-packages/easy_install.py ``
