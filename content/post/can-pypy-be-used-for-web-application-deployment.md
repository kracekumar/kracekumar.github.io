+++
date = "2012-11-25 20:11:00+00:00"
draft = false
tags = ["pypy", "python", "deployment"]
title = "Can PyPy be used for web application deployment ?"
url = "/post/36532666649/can-pypy-be-used-for-web-application-deployment"
+++
## What is PyPy ?

*   PyPy is an implementation of Python in Python which uses JIT(Just In Time) translation.

## Why to Use PyPy?

*   According to <a href="http://speed.python.org" target="_blank">benchmarks</a> “It depends greatly on the type of task being performed. The geometric average of all benchmarks is 0.18 or 5.6 times faster than CPython”.

## Experience?

I have used PyPy for sandboxing for my project <a href="http://github.com/kracekumar/pylive" target="_blank">pylive</a>, tested flask with pypy, ported brubeck to work on <a href="https://github.com/kracekumar/brubeck/blob/pypy/pypy.md" target="_blank">pypy</a>, tested <a href="https://github.com/hasgeek/lastuser" target="_blank">lastuser</a> in PyPy 2.0beta1. In the experiment `` pypy-sandbox, requests, flask, werkzeug, jinja2, SQLAlchemy(postgres + sqlite), cython, greenlet, eventlet, markdown, gunicorn, dictshield, json, zmq `` was tested.

If the library depends on C extensions more likely it needs rewrite using <a href="http://cffi.readthedocs.org/en/latest/" target="_blank">cffi</a>. Here is the bummer, so you can’t use your `` requirements.txt `` to directly deploy with `` PyPy ``. For an example: `` pyzmq `` library is written in C. So you can’t run in PyPy, so one needs to use <a href="https://github.com/felipecruz/zmqpy" target="_blank">zmqpy</a>, <a href="https://github.com/svpcom/pyzmq-ctypes" target="_blank">pyzmq</a>.

## Benchmarks

<a href="https://gist.github.com/4137006" target="_blank">Benchmark</a> of `` brubeck `` after porting shows `` PyPy `` din’t boost performance. This could be due to following reasons.

1. `` gevent `` doesn’t run on PyPy yet. :-(


2.

    `` zmq `` library blocks, `` eventlet `` support for zmq is <a href="https://github.com/nanonyme/eventlet-pypy/issues/1" target="_blank">buggy</a>.


3.

    `` ujson `` vs `` PyPy `` in built `` json `` performance.



## Does any one use PyPy in production

1. <a href="http://www.quora.com/Alex-Gaynor/Posts/Quora-is-now-running-on-PyPy" target="_blank">Quora runs on pypy</a> but no updates recently.


2. Twisted site run on PyPy.


3. There is a scientfic telescope project that runs on pypy(couldn’t locate the link).



## My take

1. `` Postgres driver `` runs without any issues, happy for that.


2.

    Waiting for `` gevent `` to support, after that there will be significant progress in speed of brubeck and adoption.


3.

    All my personal projects will be tested against pypy. Yes hopes are becoming true.
