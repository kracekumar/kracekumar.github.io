
+++
date = "2012-05-06 13:06:00+00:00"
draft = false
tags = ["python", "in operator"]
title = "python `in` operator use cases"
url = "/post/22512660850/python-in-operator-use-cases"
+++
Python \*in\* operator is membership test operator.\*Examples:\*List—-

<pre class="prettyprint"><code>
In [1]: python_webframeworks = ['flask', 'django', 'pylons', 'pyramid', 'brubeck']

In [2]: 'flask' in python_webframeworks

Out[2]: True

In [3]: 'web.py' in python_webframeworks

Out[3]: False

</code></pre>

_in_ operator iterates over the list of elements and returns &nbsp;_True_ or _False_.

_What about nested list?_

<pre class="prettyprint"><code>

In [4]: webframeworks = [['flask', 'django', 'pyramid'],['rails', 'sintara'],['zend', 'symfony']]

In [5]: 'flask' in webframeworks
Out[5]: False

</code></pre>

_in_ isnt handy for nested list, unless it is&nbsp;overriden.&nbsp;

## Dict

&nbsp;_in_ operator against dictionary checks for the presence of key.

<pre class="prettyprint"><code>
In [7]: person = {'name': 'kracekumar', 
'country': 'India', 'os': 'Linux', 
'programming_languages': {'web': 'php', 'multi_paradigm': ['python', 'ruby', 'java', 'c#']}}

In [8]: 'name' in person

Out[8]: True
</code></pre>

_in_ doesnt check inside key if value is dict

<pre class="prettyprint"><code>
In [9]: 'web' in person

Out[9]: False

</code></pre>

In case if dict needs to look into keys whose value is dict, you need to override &nbsp;`` __contains__ ``. At the end of the blog post I will explain, now lets create a class which is inherited from dict and try the same experiment.

<pre class="prettyprint"><code>

In [10]: class Person(dict):
&nbsp; &nbsp;....: &nbsp; &nbsp; pass
&nbsp; &nbsp;....:&nbsp;

In [11]: p = Person()

In [12]: p

Out[12]: {}

In [13]: p['name'] = 'krace'

In [14]: p

Out[14]: {'name': 'krace'}

</code></pre>

To make things simple &nbsp;`` __init__ `` and other `` magic methods `` are omitted.&nbsp;

<pre class="prettyprint"><code>
In [15]: 'name' in p

Out[15]: True

</code></pre>

## Set

<pre class="prettyprint"><code>
In [16]: conferences_attended = set(['Pycon - India', 'code retreat', 'JSFOO', 'Meta Refresh'])&nbsp;

In [17]: 'Pycon - India' in conferences_attended

Out[17]: True

</code></pre>

## Generators

<pre class="prettyprint"><code>
In [18]: 2 in xrange(3)

Out[18]: True

</code></pre>

## Nested list

<pre class="prettyprint"><code>
In [19]: nested_list = [[1, 3, 5, 7], [2, 4, 6, 8], []]

In [20]: [] in nested_list

Out[20]: True

In [21]: [2, 4, 6, 8] in nested_list

Out[21]: True

</code></pre>

In case of nested list checking whether inner list is empty is pretty handy.

## Strings

<pre class="prettyprint"><code>
In [23]: message = " Python is simple and powerful programming language "

In [24]: "Python" in message

Out[24]: True

In [25]: message.find("Python")

Out[25]: 1

In [26]: message = "Python is simple and powerful programming language "

In [27]: message.find("Python")

Out[27]: 0

</code></pre>

_in_ can be used to check the presence of a sequence or substring against string. In other languages there will be function to check for substring, but in python its very straightforward. This is one of the reason why I love Python.

__NOTE:__

&nbsp;Dont use find method in string to find the existence of a substring because it will return the position.&nbsp;

<pre class="prettyprint"><code>
if message.find(text):
&nbsp; &nbsp; &nbsp; #lets do what we need do
else:
&nbsp; &nbsp; &nbsp; #fall back
</code></pre>

  
Above snippet is wrong since text might be present in the beginning which will result 0 and if condition will fail.

## Files&nbsp;

<pre class="prettyprint"><code>
In [29]: with open('test.txt', 'w') as f:

&nbsp; &nbsp; &nbsp;f.write(" some text for checking")
&nbsp; &nbsp;....:&nbsp;

In [32]: with open('test.txt', 'r') as f:

&nbsp; &nbsp; &nbsp;for text in f:

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;print 'some' in text
&nbsp; &nbsp;....: &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;
True

</code></pre>

## overriding `` __contains__ ``

  
&nbsp;Consider a `` class Person `` &nbsp;which is inherited from _dict_ and override `` __contains__ ``

<pre class="prettyprint"><code>

In [153]: class Person(dict):
&nbsp; &nbsp;.....: &nbsp; &nbsp; def __contains__(self, item):
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; for key in self:
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if key is item:
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; return True
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; else:
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if isinstance(self[key], dict):
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; for k in self[key]:
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if k is item:
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; return True
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; else:
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; return False
&nbsp; &nbsp;.....: &nbsp; &nbsp; &nbsp; &nbsp;&nbsp;

In [154]: p = Person()

In [155]: p['skills'] = {'programming_languages': {'web': ['php'], 'multi_paradigm':
['python', 'ruby', 'c#']}
}

In [156]: 'skills' in p

Out[156]: True

In [157]: 'programming_languages' in p

Out[157]: True

In [158]: 'web' in p

Out[158]: False

</code></pre>

In the above example _key_ will be looked for two level dict only.&nbsp;
