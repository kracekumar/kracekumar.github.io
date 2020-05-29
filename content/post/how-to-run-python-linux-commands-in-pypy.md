+++
date = "2012-11-25 19:36:19+00:00"
draft = false
tags = ["PyPy", "apt-get"]
title = "How to run Python Linux commands in PyPy ?"
url = "/post/36529827327/how-to-run-python-linux-commands-in-pypy"
+++
I have been using PyPy from 1.6. Now PyPy 2.0beta1 is out. Most of python libraries which don’t have `` c extensions `` as dependencies work(exceptions are present).

E.g: `` requests, fabric, gunicorn ``

## Install PyPy2.0beta1

1. Grab 32 or 64 bit <a href="http://pypy.org/download.html" target="_blank">pypy2.0 beta1</a>. If you are using ubuntu grab `` libc2.5 ``.


2.

    `` bzip2 pypy-2.0-beta1-linux64-libc2.15.tar.bz2 ``


3.

    `` tar -xvf pypy-2.0-beta1-linux64-libc2.15.tar ``


4.

    <code>curl -O <a href="http://python-distribute.org/distribute_setup.py" target="_blank">http://python-distribute.org/distribute_setup.py</a></code>


5.

    <code>curl -O <a href="https://raw.github.com/pypa/pip/master/contrib/get-pip.py" target="_blank">https://raw.github.com/pypa/pip/master/contrib/get-pip.py</a></code>


6.

    `` ./pypy-2.0-beta1/bin/pypy distribute_setup.py ``


7.

    `` ./pypy-2.0-beta1/bin/pypy get-pip.py ``


8.

    `` ./pypy-2.0-beta1/bin/pip install virtualenv ``


9.

    Please feel free to create `` alias ``. Here is sample alias.



    alias pypy2b="/home/kracekumar/downloads/pypy-2.0-beta1/bin/pypy"
    alias pypy2b-pip='/home/kracekumar/downloads/pypy-2.0-beta1/bin/pip'
    alias pypy2b-venv='/home/kracekumar/downloads/pypy-2.0-beta1/bin/virtualenv'

##  Next ?

1. `` pypy2b-pip install fabric ``


2.  Add `pypy bin` directory to `$PATH, export PATH=$PATH:/home/kracekumar/downloads/pypy-2.0-beta1/bin` or open `` ~/.bashrc `` add `` export PATH=$PATH:/home/kracekumar/downloads/pypy-2.0-beta1/bin `` and `` source ~/.bashrc ``.


3.

        kracekumar@aadukalam:~/codes/python-wiktionary$ which fab
        /home/kracekumar/downloads/pypy-2.0-beta1/bin/fab


4.   <code><br/>
kracekumar@aadukalam:~/codes/python-wiktionary$ cat <code>which fab</code><br/><code>#!/home/kracekumar/downloads/pypy-2.0-beta1/bin/pypy</code><br/><code># EASY-INSTALL-ENTRY-SCRIPT: 'Fabric==1.5.1','console_scripts','fab'</code><br/><strong>requires</strong> = ‘Fabric==1.5.1’<br/>
import sys<br/>
from pkg_resources import load_entry_point<br/>
if <strong>name</strong> == ’<strong>main</strong>’:<br/>
sys.exit(
   load_entry_point('Fabric==1.5.1’, 'console_scripts’, 'fab’)()<br/>
)</code>

So `` fab `` now uses pypy interpreter rather than CPython.

But there are other commands like `` history, gwibber-service `` which are installed via `` apt-get `` can’t use PyPy since `` apt-get `` doesn’t support `` pypy `` since it is unstable.

## Thoughts

* There should be way developer can specify in debian/ubuntu repo to look for PyPy in the user installation if not use CPython.


*  `` apt-get `` is written in python, if distros ship `` pypy `` and `` apt-get `` works well, there is a chance `` PyPy `` will became main stream in linux packaging.
