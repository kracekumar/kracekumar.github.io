
+++
date = "2014-02-26 19:07:00+00:00"
draft = false
tags = ["python", "dictionary"]
title = "Counting elements with dictionary"
url = "/post/77925927684/counting-elements-with-dictionary"
+++
Let’s say you want to find how many times each element is present in the list or tuple.

#### Normal approach

    words = ['a', 'the', 'an', 'a', 'an', 'the']
    d = {}
    for word in words:
        if word in d:
            d[word] += 1
        else:
            d[word] = 1
    print d
    {'a': 2, 'the': 2, 'an': 2} 

#### Better approach

    words = ['a', 'the', 'an', 'a', 'an', 'the']
    d = {}
    for word in words:
        d[word] = d.get(word, 0) + 1
    
    print d
    {'a': 2, 'the': 2, 'an': 2

Both the approach returned same values. The first one has 6 lines of logic and second has 3 lines of logic (less code less management).

Second approach uses `` d.get `` method. `` d.get(word, 0) `` return count of the word if key is present else `` 0 ``. If `` 0 `` isn’t passed `` get `` will return `` None ``.

#### Pythonic approach:

    import collections
    
    words = ['a', 'b', 'a']
    
    res = collections.Counter(words)
    
    print res
    Counter({'a': 2, 'b': 1})

Last approach is just one line and pythonic.

Snippet is extracted from <a href="https://www.youtube.com/watch?v=OSGv2VnC0go" target="_blank">Transforming Code into Beautiful, Idiomatic Python</a>. Do watch and enjoy.
