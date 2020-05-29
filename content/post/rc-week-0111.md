+++
date = "2016-11-21 08:09:16+00:00"
draft = false
tags = ["recursecenter"]
title = "RC Week 0111"
url = "/post/153465926785/rc-week-0111"
+++
This week most of the time I spent learning <a href="https://www.wikiwand.com/en/Distributed_hash_table" target="_blank">Distributed Hash Tables</a> and <a href="http://learnyousomeerlang.com/" target="_blank">Erlang</a>. That means I didn’t write a significant amount of code for the project.

I am reading DynamoDB paper which is Distributed Hash Table and Distributed Key/Value store. Some implementation like `` etcd `` and `` consul `` provides Distributed Key/Value store but replicates all data across nodes. So those aren’t Distributed Hash Table. I am reading the one sections at a time. DynamoDB paper is the first technical paper reading session and on fourth sections. There are a lot of new concepts for me like `` vector clocks, internode communication, consistent hashing `` etc … I will be implementing DHT in Erlang.

I am picking up Erlang, and the syntax is completely weird. While entering Erlang world, leave all your assumptions, expectation outside. The language astonishes me with its design decision. Certain design choice put me into deep thinking like functions declaration doesn’t start with `` def `` or `` fn `` or `` func ``. Here are few interesting examples.

*   Equal to operator `` =:= ``
*   Period `` . `` is the delimiter like `` Lang = [python, rust, erlang]. ``
*   Variable names start with Captial letter.
*

    Pattern matching in function



    can\_vote(X) when X >= 18 -> true;can\_vote(\_) -> false.



If the function `` can_vote `` is called with an argument whose value is equal or greater than `` 18 `` `` true `` is returned. The important point is to note is `` _ `` which means any other value. The pattern matching can be applied to a total number of arguments which I haven’t come across before. I don’t think so this is same as Polymorphism.

I have only started writing Erlang code and still got a long way to go to write a DHT. Implementing and maintaining DHT can be a good way to learn distributed systems.

On November 20, the temperature in NY/Brooklyn was around 6 degrees Celcius in the evening. This is the first time I am experiencing such a cold weather and the wind.

Every day near my residence, I find an old street person at a signal asking for alms. Once in a while, I speak to him and offer coins. Today while I was returning from the laundry in the night (8:30 PM) in the cold weather, he was waiting for a car to stop or passerby near the signal to share a spare change. I was running to home to escape hard winter weather. He spotted me and said me Good Night. I smiled and reciprocated the greeting. Once I reached home, this incident was oscillating in mind. Everyone in the alley was moving quick to get home, but he was doing his routine in adverse weather.

This lead me to ask the series of questions.- When was the last I was in adverse uncomfortable condition? - Why can’t he catch an early sleep or relax?- Has he become numb to the surroundings?
