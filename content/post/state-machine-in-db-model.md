+++
date = "2016-07-16 20:25:06+00:00"
draft = false
tags = ["python", "state machine"]
title = "State machine in DB model"
url = "/post/147507497215/state-machine-in-db-model"
+++
A state machine is an abstract machine that can be in one of a finite number of states. The machine is in only one state at a time; the state it is in at any given time is called the current state.

While using the database, individual records should be in allowed states. The database or application stores rules for the states. There are many ways to design the database schema to achieve this. The most followed methods are using `` int `` or `` string `` field, where each value represents a state. Above is the direct method approach. An indirect and unaware method is the use of multiple boolean values to calculate the state. Like `` is_archived `` and `` is_published ``. The combination of two fields shows four different states.

At work, the similar situation arose. First, it started with single boolean value and after some time, the second column showed up. Then, of course, the third one. That’s when I realized; use `` state machine ``. I spent time looking up a relevant library and meet <a href="https://github.com/tyarkoni/transitions" target="_blank">transition</a>. The library supports `` transitions ``, `` state change validation ``.

Consider a simple model `` Order `` with five states. `` 'placed', 'dispatched', 'delivered', 'canceled', 'returned' ``. Well, real world model will have way more states. The important point of these states, object transition should follow rules. An order can be in `` returned `` state when the previous state was `` delivered ``. And this requires custom validation code with multiple `` if `` conditions. Here is state machine diagram.

<img alt="Drawing" src="https://dl.dropboxusercontent.com/u/39367302/state.jpg" style="width: 600px;"/>

When the number of states increases, the code starts to fall apart with `` conditions `` and `` repetition ``.

`` Transition `` library supports the declaration of `` states ``, `` allowed triggers `` and takes care of validation. It provides a set of helper methods to do transitions. Here is a simple example.

<div class="gist"><a href="https://gist.github.com/kracekumar/dc5628713340acbda373c6f6f54e72bb" target="_blank">https://gist.github.com/kracekumar/dc5628713340acbda373c6f6f54e72bb</a></div>

`` Machine `` class takes a list of `` transitions ``. `` Transitions `` is a collection of iterable with `` trigger ``, `` start state `` and `` destination state `` as arguments respectively.

To trigger an event, invoke `` machine.model.<trigger>() ``. Method returns `` True `` or `` False ``. `` True `` when the trigger succeeds and `` False `` when the trigger isn’t allowed in the current state. The machine calculates the logic from the list of configured transitions.

<div class="gist"><a href="https://gist.github.com/kracekumar/352daef114d84d13d7805b71727e79e8" target="_blank">https://gist.github.com/kracekumar/352daef114d84d13d7805b71727e79e8</a></div>

Delivery isn’t possible with the canceled order!

`` ignore_invalid_triggers=True `` makes the method to return `` True `` or `` False ``. The library raise `` MachineError Exception `` when `` ignore_invalid_triggers `` is set to `` False ``.

Declarative style of creating state machine makes the library easy to use and prevents a lot of boilerplate code.
