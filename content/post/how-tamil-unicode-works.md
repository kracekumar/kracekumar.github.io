+++
date = "2013-10-12 16:36:37+00:00"
draft = false
tags = ["unicode", "python", "Tamil"]
title = "How Tamil Unicode works"
url = "/post/63832712202/how-tamil-unicode-works"
+++
Tamil has 247 characters. No panic. It is simple. 12 uyir eluthu(அ,ஆ..ஔ), 18 mei eluthu(க்,ங்..) , 216 uyirmei eluthu(12 \* 18 க,ங ).1 ayutham(ஃ).

I assume you know what is unicode. If not read <a href="http://www.joelonsoftware.com/articles/Unicode.html" target="_blank">The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets</a> and then read wikipedia page. You will understand most of it. Back to the post.

Every character or letter in unicode has value called `` code point ``. This is similar to ASCII where value of `` a `` is 97. All code point value is represented in hexadecimal. Tamil unicode character range starts from `` 0B80 - 0BFF ``. Unicode consortium has a <a href="http://www.unicode.org/charts/PDF/U0B80.pdf" target="_blank">complete mappings</a>. So value of அ is `` 0B85 ``.

You don’t believe me ? I will show you the code. Fireup `` python2 `` console(I am using ipython).

    In [343]: print(unichr(0x0b85))
    அ

How to find hex of a character ?

    In [344]: print(hex(ord(u'க')))
    0xb95

What is the code point of கி?

    In [345]: print(hex(ord(u'கி')))
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-345-9e02bf5958df> in <module>()
    ----> 1 print(hex(ord(u'கி')))

    TypeError: ord() expected a character, but string of length 2 found

Python says len of the unicode character `` கி `` is 2. Lets check.

    In [346]: print(len(u'கி'))
    2

    In [348]: print(len(unichr(0x0b85)))
    1

    In [349]: print(unichr(0x0b85))
    அ

So clearly `` கி `` is composed of two characters.`` கி ``=`` க் ``+ `` இ `` from language point of view. Lets see what program says.

    In [355]: for c in u'கி':
        print c, hex(ord(c))
       .....:
    க 0xb95
     ி 0xbbf

When you are typing in browser or editor, you use `` க `` + `` இ `` after that it is the work of font and others work to replace it.

Below is the javascript code executed in chrome console.

    chr = "கி"
    for (var i in chr){console.log(chr[i]);}
    க
    ி
    undefined

So irrespective of the programming language or console or browser unicode is stored in the same way.

`` ி ``is dependent vowel sign. Let me list all of them.

    In [362]: for c in range(0xbbe, 0xbce):
        print c, unichr(c)
       .....:
    3006 ா
    3007 ி
    3008 ீ
    3009 ு
    3010 ூ
    3011 ௃
    3012 ௄
    3013 ௅
    3014 ெ
    3015 ே
    3016 ை
    3017 ௉
    3018 ொ
    3019 ோ
    3020 ௌ
    3021 ்

`` ௅ `` is a placeholder and that hex value doesn’t have any symbol or character. Dependent vowel will be used for replacing uyir eluthu.

Now lets read character by character in a word .

    In [366]: for i in u'விகடகவி':
            print i, hex(ord(i))
       .....:
    வ 0xbb5
     ி 0xbbf
    க 0xb95
    ட 0xb9f
    க 0xb95
    வ 0xbb5
     ி 0xbbf

Well above logic holds true for english but not for Tamil.

    In [395]: word = u'விகடகவி'

    In [396]: dependent_vowel_range = range(0xbbe, 0xbce)

    In [397]: pos, stop = 0, len(word) - 1

    In [398]: while not pos >= stop:
        #check if next char is dependent vowel if so, print together
        if ord(word[pos+1]) in dependent_vowel_range:
            print word[pos], word[pos+1]
            pos += 2
        else:
            print word[pos]
            pos += 1
    வ ி
    க
    ட
    க
    வ ி

Now try to write a program that will find a tamil word is palindrome or not and see the fun.

Note: I need to figure font that will club dependent vowel and print properly in terminal.
