
+++
date = "2017-05-10 18:13:33+00:00"
draft = false
tags = ["python", "book-review"]
title = "Book Review: Software Architecture with Python"
url = "/post/160521154075/book-review-software-architecture-with-python"
+++
The book <a href="https://www.packtpub.com/application-development/software-architecture-python" target="_blank">Software Architecture with Python</a> is by <a href="https://twitter.com/skeptichacker" target="_blank">Anand B Pillai</a>. The book explains various aspects of software architecture like testability, performance, scaling, concurrency and design patterns.

The book has ten chapters. The first chapter speaks about different architect roles like solution architect, enterprise architect, technical architect what is the role of an architect and difference between design and architecture. The book covers two lesser spoken topics debugging and code security which I liked. There is very few literature available in debugging. The author has provided real use cases of different debugging tips and tools without picking sides. The book has some good examples on OverFlowErrors in Python.

My favorite chapter in the book is ‘Writing Applications that scale’. The author explains all the available concurrency primitives like threading, multiprocess, eventlet, twisted, gevent, asyncio, queues, semaphore, locks, etc. The author doesn’t stop by explaining how to use them but paves the path to figuring out how to profile the code, find out where the bottleneck lies and when to use which concurrency primitives. This chapter is the longest in the book and deluged with examples and insights. The author’s approach of using `` time `` command to measure the performance rather than sticking with wall-clock time gives the developer understanding of where programming is spending most of the time. The infamous GIL is explained!

The book covers vital details on implementing Python Design Patterns and how Python’s dynamic nature brings joy while creating the creational, structural and design pattern. The showcased examples teach how Python metaclass works and it’s simplicity. The author avoids the infamous gang of four patterns.

The book documents significantly scattered wisdom in Python and Software Architecture world in one place. The book strongly focuses on design pattern and concurrency. The in-depth coverage like concurrency is missing to other chapters. The book is must read for all Python developers.

The book is indeed a long read and solid one in size and content. Having said that book is pretty hands on and loaded with trivial and non-trivial examples. I won’t be surprised if half the book covers the code snippets. The author hasn’t shied away skipping code snippets for explaining any concept. The book introduces a plethora of tools for developing software, and at the same time references required literature.
