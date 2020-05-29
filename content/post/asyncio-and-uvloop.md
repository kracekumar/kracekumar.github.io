+++
date = "2016-05-08 20:18:48+00:00"
draft = false
tags = ["python", "asyncio", "uvloop"]
title = "Asyncio and uvloop"
url = "/post/144058400775/asyncio-and-uvloop"
+++
Today, I read an article about <a href="http://magic.io/blog/uvloop-make-python-networking-great-again/" target="_blank">uvloop</a>. I am aware of libuv and its behind nodejs. What caught me was “In fact, it is at least 2x faster than any other Python asynchronous framework.”. So I decided to give it a try with <a href="https://aiohttp.readthedocs.io/en/stable/web.html#websockets" target="_blank">aiohttp</a>.

The test program was simple websocket code which receives a text message, doubles the content and echoes back. Here is the complete <a href="https://gist.github.com/kracekumar/daf10b3be3191a78b037c0c79667c26c" target="_blank">snippet</a> with uvloop.

I ran naive benchmark using <a href="https://github.com/observing/thor" target="_blank">thor</a> and results favoured uvloop.

* uvloop was able to handle more connection on 8GB, non SSD Mac OSX. Asyncio was able to hold 154 connections and uvloop 243 connections with any socket errors.
* Response time for 154 connections, 10 messages for each connection took 1030ms in asyncio event loop and 915ms in uvloop. This is not full blown proper benchmark but proves the point.

If you haven’t tried <a href="https://docs.python.org/3/library/asyncio.html" target="_blank">asyncio</a>, you should!
