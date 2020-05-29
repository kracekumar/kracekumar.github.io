
+++
date = "2016-12-12 05:18:55+00:00"
draft = false
tags = ["recursecenter"]
title = "RC Week 1010"
url = "/post/154363774320/rc-week-1010"
+++
This week has mostly been calm and cold in New York.

### EDHT

<a href="http://github.com/kracekumar/edht" target="_blank">Distributed Hash Table</a> implementation in Erlang is slowly coming along. The project now supports multi-node communication.

The project uses <a href="https://github.com/basho/bitcask/" target="_blank">bitcask</a> which <a href="https://github.com/basho/riak" target="_blank">riak</a> uses. Erlang’s Key/Value store data is local to the single process. Building Key/Value store from the ground up requires reinventing wheel and time consuming. The leveraging existing library made sense. Bitcask takes care of persisting the data to the disk and can access data without race conditions so far.

<a href="https://en.wikipedia.org/wiki/Consistent_hashing" target="_blank">Consistent hashing</a> is the key part of building distributed hash table. With partitioning and replication, the data is available when one or multiple nodes isn’t available. <a href="http://michaelnielsen.org/blog/consistent-hashing/" target="_blank">Michael Nielsen’s post</a> on consistent hashing with Python implementation was helpful in understanding key concepts. The post doesn’t talk about replication. <a href="https://github.com/carlosgaldino/concha/" target="_blank">Concha</a> library in Erlang implements consistent hashing and has nice API.

The read/write data partitioning and replication is next step!

My curiosity towards Erlang concurrency’s is growing day by day.

### Htop

<a href="http://hisham.hm/htop/" target="_blank">Htop</a> is my go-to tool for process memory debugging and process information. I prefer to use it on `` GNU/Linux `` and `` OSX ``. <a href="https://peteris.rocks/blog/htop/" target="_blank">I read the post by Pieter - htop explained</a>. The post explain how htop works, how Linux handles processes, where and how the running process information is stored. This post is useful for anyone interested in systems.

### Times Square

On Saturday - 10th Dec 2016, I visited <a href="https://en.wikipedia.org/wiki/Times_Square" target="_blank">Times Square</a> at 2:03 AM. I took a subway train from Nostrand Avenue to 50th Street at 1:15 AM. The train was half full at Nostrand Avenue. Few people got into every station in Brooklyn. Few were sleeping in the train stretching the legs in the adjacent seats. Some homeless folks (subway is their home) were resting in the public station seating and besides turnstiles. MTA groundsmen were cleaning the station, collecting the garbage and working in underground construction. The majority of passengers in the car were seniors. There was a crowd in every Manhattan station till 50th Street. The crowd was mostly youth. At 50th street, there was a little place to stand in the car. It was -1 degree Celsius at Times Square. Times Square was colorful with gigantic screens. All the screen was playing ads, movie trailers, and other commercials. Maximum of fifteen people was around <a href="https://en.wikipedia.org/wiki/George_M._Cohan" target="_blank">George M Cohan’s</a> statue. I walked down few streets and reached back home by 3:30 AM in the subway.

There was snow showers in Brooklyn on Sunday. Hope it snows heavily in next few days :-). Next week is the last week of Fall'02 batch.
