
+++
date = "2013-07-18 18:26:20+00:00"
draft = false
tags = ["python", "why-this-kolaveri-di", "tanglish", "tamil"]
title = "Why This Kolaveri Di song words language "
url = "/post/55796363713/why-this-kolaveri-di-song-words-language"
+++
I was wondering how many words in <a href="https://www.youtube.com/watch?v=JJpCV31n7T8" target="_blank">why this kolaveri di</a> song belongs to english. So I wrote this code to evaluate.

    #! /usr/bin/env
    #! -*- coding: utf-8 -*-
    
    lyrics = """
    yo boys i am singing song
    soup song
    flop song
    why this kolaveri kolaveri kolaveri di
    why this kolaveri kolaveri kolaveri di
    rhythm correct
    why this kolaveri kolaveri kolaveri di
    maintain please
    why this kolaveri di
    
    distance la moon-u moon-u
    moon-u color-u white-u
    white background night-u night-u
    night-u color-u black-u
    
    why this kolaveri kolaveri kolaveri di
    why this kolaveri kolaveri kolaveri di
    
    white skin-u girl-u girl-u
    girl-u heart-u black-u
    eyes-u eyes-u meet-u meet-u
    my future dark
    
    why this kolaveri kolaveri kolaveri di
    why this kolaveri kolaveri kolaveri di
    
    maama notes eduthuko
    apdiye kaila snacks eduthuko
    pa pa paan pa pa paan pa pa paa pa pa paan
    sariya vaasi
    super maama ready
    ready 1 2 3 4
    
    whaa wat a change over maama
    
    ok maama now tune change-u
    
    kaila glass
    only english
    
    hand la glass
    glass la scotch
    eyes-u full-a tear-u
    empty life-u
    girl-u come-u
    life reverse gear-u
    love-u love-u
    oh my love-u
    you showed me bouv-u
    cow-u cow-u holy cow-u
    i want you hear now-u
    god i am dying now-u
    she is happy how-u
    
    this song for soup boys-u
    we dont have choice-u
    
    why this kolaveri kolaveri kolaveri di
    why this kolaveri kolaveri kolaveri di
    why this kolaveri kolaveri kolaveri di
    why this kolaveri kolaveri kolaveri di
    
    flop song
    """
    dict_file_path = "/usr/share/dict/words"
    
    
    def sanitize(words):
        for index, word in enumerate(words):
            if word.endswith("-u") or word.endswith("-a"):
                words[index] = word[:-2]
    
    
    if __name__ == "__main__":
        # Get all words
        words = [word for line in lyrics.split("\n") for word in line.split(" ") if word != ""]
        # Load english words
        dictionary_words = open(dict_file_path).readlines()
        # Remove \n in dictionary words
        dictionary_words = [word.split("\n")[0] for word in dictionary_words]
        # Add missing words
        dictionary_words.append("boys")
        dictionary_words.append("snacks")
        dictionary_words.append("eyes")
        dictionary_words.append("english")
        dictionary_words.append("1")
        dictionary_words.append("2")
        dictionary_words.append("3")
        dictionary_words.append("4")
        dictionary_words.append("notes")
        dictionary_words.append("ok")
        dictionary_words.append("showed")
        # Remove -u which sounds like Tamil words
        sanitize(words)
        # Find unique words
        unique_words = set(words)
        # Find english words
        eng_words = [word for word in unique_words if word in dictionary_words]
        non_eng_words = unique_words - set(eng_words)
        # Remove empty element
        non_eng_words = [word for word in non_eng_words if word != ""]
        print("==English Words==")
        print(eng_words)
        print("==Non English Words==")
        print(non_eng_words)
        print("Total unique words: %d,\n English words: %d,\n Non English words: %d,\n percentage of english words: %f" % (len(unique_words), len(eng_words), len(non_eng_words), float(len(eng_words))/len(unique_words) * 100))

_Output_

    ➜  lua  python why_this_kolaveri_di.py
    ==English Words==
    ['over', 'skin', 'la', 'only', 'black', '4', 'rhythm', 'yo', 'di', 'choice', 'dark', 'background', '2', 'now', 'tear', 'notes', 'she', 'night', 'girl', 'for', 'god', 'please', 'moon', '3', 'correct', 'we', 'full', 'how', 'super', 'change', 'ok', 'reverse', 'cow', 'oh', 'love', 'dont', 'color', 'singing', 'come', 'pa', 'white', 'wat', 'empty', 'happy', 'eyes', 'gear', 'holy', 'boys', 'hear', 'me', 'distance', 'showed', 'this', 'soup', 'future', 'meet', 'my', 'heart', 'have', 'snacks', 'is', 'am', 'want', 'ready', 'dying', 'song', '1', 'you', 'hand', 'why', 'tune', 'a', 'glass', 'i', 'scotch', 'flop', 'life', 'maintain', 'english']
    ==Non English Words==
    ['kaila', 'sariya', 'paa', 'apdiye', 'eduthuko', 'vaasi', 'maama', 'whaa',  'bouv', 'paan', 'kolaveri']
    Total unique words: 90,
    English words: 79,
    Non English words: 11,
    Percentage of english words: 87.777778

It turns out, song contains `` 90 `` unqiue words, `` 79 `` words are `` english `` and `` 11 `` are `` non english ``(Tamil). `` 87.8% `` words are english. So here after call `` why this kolaveri di `` song as `` Tanglish(Tamil + English) `` song.

Gist: <a href="https://gist.github.com/kracekumar/6031683" target="_blank">https://gist.github.com/kracekumar/6031683</a>
