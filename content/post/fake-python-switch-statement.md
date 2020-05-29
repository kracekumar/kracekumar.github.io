
+++
date = "2012-06-10 18:28:39+00:00"
draft = false
tags = []
title = "Fake Python switch statement"
url = "/post/24826905040/fake-python-switch-statement"
+++
Python has no _switch_ statement.

what is switch statement ? switch statement is an alternate to `` if - elseif - else `` statement.

_Example in C_

<pre class="prettyprint">

  int payment_status=1;
    switch(payment_status){
    case 1:
        process_pending_payment();
        break;
    case 2:
       process_paid();
        break;
    case 3:
        process_trans_failure();
       break;
    default:
       process_default();
}
</pre>

In _python_ we can achieve same behaviour using `` dict ``.

`` Fake switch statement in python ``

<pre class="prettyprint">
payment_functions = {
    1: process_pending_payment,
    2: process_paid,
    3: process_trans_failure
}
try:
    status =2
    payment_functions[status]()
except KeyError:
    process_default()
</pre>

In above code `` payment_functions `` is `` dict ``, where `` key `` is the one of the value of `` status `` and corresponding `` value `` is function to be invoked(but `` () `` is not present immediately).

When we try to access the dict element `` function `` name and `` () `` is present immediately so function is called. If `` key `` is absent `` KeyError `` exception is raised, `` default `` function is placed inside except block.
