
+++
date = "2014-03-07 18:24:00+00:00"
draft = false
tags = ["python", "heapq"]
title = "Find n largest and smallest number in an iterable"
url = "/post/78863855937/find-n-largest-and-smallest-number-in-an-iterable"
+++
Python has `` sorted `` function which sorts iterable in ascending or descending order.

    # Sort descending
    In [95]: sorted([1, 2, 3, 4], reverse=True)
    Out[95]: [4, 3, 2, 1]
    
    # Sort ascending
    In [96]: sorted([1, 2, 3, 4], reverse=False)
    Out[96]: [1, 2, 3, 4]

`` sorted(iterable, reverse=True)[:n] `` will yield first `` n `` largest numbers. There is an alternate way.

Python has <a href="http://docs.python.org/2/library/heapq.html" target="_blank">heapq</a> which implements heap datastructure. `` heapq `` has function `` nlargest `` and `` nsmallest `` which take arguments `` n `` number of elements, `` iterable like list, dict, tuple, generator `` and optional argument `` key ``.

    In [85]: heapq.nlargest(10, [1, 2, 3, 4,])
    Out[85]: [4, 3, 2, 1]
    
    In [88]: heapq.nlargest(10, xrange(1000))
    Out[88]: [999, 998, 997, 996, 995, 994, 993, 992, 991, 990]
    
    In [89]: heapq.nlargest(10, [1000]*10)
    Out[89]: [1000, 1000, 1000, 1000, 1000, 1000, 1000, 1000, 1000, 1000]
    
    In [99]: heapq.nsmallest(3, [-10, -10.0, 20.34, 0.34, 1])
    Out[99]: [-10, -10.0, 0.34]

Let’s say `` marks `` is a list of dictionary containing students marks. Now with `` heapq `` it is possible to find highest and lowest mark in a subject.

    In [113]: marks = [{'name': "Ram", 'chemistry': 23},{'name': 'Kumar', 'chemistry': 50}, {'name': 'Franklin', 'chemistry': 89}]
    
    In [114]: heapq.nlargest(1, marks, key=lambda mark: mark['chemistry'])
    Out[114]: [{'chemistry': 89, 'name': 'Franklin'}]
    
    In [115]: heapq.nsmallest(1, marks, key=lambda mark: mark['chemistry'])
    Out[115]: [{'chemistry': 23, 'name': 'Ram'}]

`` heapq `` can be used for building priority queue.

Note: <a href="http://ipython.org" target="_blank">IPython</a> is used in examples where `` In [114]: `` means `` Input line number 114 `` and `` Out[114] `` means `` Output line number 114 ``.
