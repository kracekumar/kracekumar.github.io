
+++
date = "2016-10-03 05:27:30+00:00"
draft = false
tags = ["recursecenter", "bittorrent", "python"]
title = "RC week 0000"
url = "/post/151276229490/rc-week-0000"
+++
<img height="500" src="https://lh3.googleusercontent.com/zGakGJDUqDyLltKki0mYpbTrHQXd8v4SQOdYOjzttNaiTf5eFXUlZPJUhA_c96Bfbx7cm5Kp6p-NVWL0SVD4TVazdIFyJgWaTmxMBwTZdxh4TduWNQF7BJlW8lUzCA8dZYeCzFOo7AXuq6LzgKKDpi1w38JYo8S5Oj1S-d1Z9xjND6POOJJfU4adAnppFPrMBQL5yJICjge_mR5HQvPmSPzweEoIfz45Bts3by5BVcsdJoEVXTqPLWHf3jSTvUGy5TDqKevYljh2EGvJL5p1tXOy2E-3zsvpYemmIWvgfQ4wfJ0O3q5NU-S9IyGniugWXZ49NfQg3igBKWUCLLW_4Uf1OKP2hLAq6AkpUrV_4CucX95aDVEm-7Dub36SeoQs4-v8NnPPx_2hdTRAncNJi7SkxjsdQZHz6-0fpB1SGIYCpL3aRusWMZx3PYK9LAMcbGZU_DGjVGcW5QKBr01EY_FgXiRjB7aXIuEW-LlxCdcnuh20gCXVzA7ZrBghuAyl-nZAmn3v_Fed1JRwEUVNO8fam2FzeKqMT5dEPARk518HvTn-YiAMZv0qKj3eeh4FrF_kiFugZgIMW1DskJvn1xF584pDDCuTCdiHTV_s6M5y-8eh=s800" width="500"/>

The long awaited <a href="https://recurse.com" target="_blank">Recurse Center</a> debut day, 26th Sep 2016 kick started with a welcome note by Nicholas Bergson-Shilcock and David Albert ; decorated by other events and activities to get to know the batchmates; the culture of RC and ended with closing note by Sonali Sridhar.

<a href="https://commons.wikimedia.org/wiki/File%3ABittorrent_new_logo.svg" target="_blank" title="By BitTorrent, Inc. ([1]) [Public domain], via Wikimedia Commons"></a>

<figure class="tmblr-full" data-orig-height="147" data-orig-src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/Bittorrent_new_logo.svg/512px-Bittorrent_new_logo.svg.png" data-orig-width="512"><img alt="Bittorrent new logo" data-orig-height="147" data-orig-src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/70/Bittorrent_new_logo.svg/512px-Bittorrent_new_logo.svg.png" data-orig-width="512" src="https://66.media.tumblr.com/7d9b4ba6cb8fcdfbfada9339208e6201/tumblr_inline_pk0028lKwW1qc390z_540.png" width="512"/></figure>

At the end of the day, I had decided to build a BitTorrent client as a first project. I was at the crossroad to choose Python or Rust or Go for the project. After a quick chat with batch mate, I decided to write the BitTorrent client in Python. I neither knew Rust well nor wrote a BitTorrent client in the past. Fighting two battles at the same time is hard.

My experience with network application is limited. I have maintained Web Socket server and fixed bugs at a previous job. The BitTorrent client is my first major network application.

As a first step towards building the client, I started reading <a href="https://www.wikiwand.com/en/BitTorrent" target="_blank">Wikipedia documentation</a> and <a href="http://www.bittorrent.org/beps/bep_0003.html" target="_blank">official proposal</a>. RC alum shared valuable resources, <a href="http://wiki.theory.org/BitTorrentSpecification" target="_blank">unofficial proposal</a> and <a href="http://www.kristenwidman.com/blog/33/how-to-write-a-bittorrent-client-part-1/" target="_blank">blog post</a> from an alum. I continued to read Wikipedia article grasped the higher level working of the BitTorrent working. In one afternoon, few batchmates got together and started to discuss the protocol; strategy recommended to download the data; security; jargon and authentication. We drew higher level steps in the life cycle of the download in a whiteboard. It was an enlightening session and helped me crystal the understanding.

<figure class="tmblr-full" data-orig-height="532" data-orig-src="https://lh3.googleusercontent.com/n2mB_QW2rQkxYvKt_My1RPTNtzAmnzTl5hggvKqnG5iLD8qRDereNyPUFKAGGRLALTSrIy2MXmod=w1265-h532-no" data-orig-width="1265"><img data-orig-height="532" data-orig-src="https://lh3.googleusercontent.com/n2mB_QW2rQkxYvKt_My1RPTNtzAmnzTl5hggvKqnG5iLD8qRDereNyPUFKAGGRLALTSrIy2MXmod=w1265-h532-no" data-orig-width="1265" src="https://66.media.tumblr.com/e6d168a9b8b32efbc6324e5b8d47a269/tumblr_inline_pk0029wiWn1qc390z_540.png" width="500"/></figure>

Resisting from coding was hard. I started to program the client by parsing torrent in <a href="https://www.wikiwand.com/en/Bencode" target="_blank">bencode</a> format. The significant portion of the data in the file is in binary format. Next step in the procedure is to gather how communication between tracker - server which stores information about connected and clients. The blog post suggested to capture all the packets during a torrent session. First I underestimated the value of the advice. Later while reading the spec and scribbling code, I found the value. Both Wireshark and tcpdump were helpful. I like tcpdump because of ease of use, lightweight and command line interface, but viewing captured packets is hard on the command line. The Wireshark infers data from the packet and renders in a useful format with a lot of switches like order by protocol, view the unencrypted packets, etc… Then I implemented communication layer for trackers using HTTP and not UDP. For example, the piratebay trackers completely use UDP.

The next piece is to build components which communicate with other seeders - clients who have complete or partial data. This part was time-consuming for me various reasons.

*   All the communication happens in binary data from the client. Having used JSON a lot and human readable format debugging binary data takes away a lot of time.
*   Understanding the message exchange format between clients and peer was tricky and different message encoding format. The client message carries data, type of data and length of data. This approach is entirely different coming from web application background where readability and usability are given importance.

Before starting to implement the crux of the client, `` asyncio `` clicked my mind and found reference <a href="https://github.com/eliasson/pieces" target="_blank">implementation</a>. The project is highly resourceful for me to build the torrent client. As of now, my client can parse the torrent file, contact the tracker to seeders information, contact peers, respond to a message and request a piece of data. All the above step constitute to 80% in the first milestone of a BitTorrent client. Yes, I can hear you murmuring 80-20 rule!

I’m on the verge of finishing the client and will release the source code in next few days. I’d suggest you to write a BitTorrent client if you haven’t.

OTH, RC’s address is 455, BroadWay, New York. The `` Broadway `` appealed to me and when I visited, the place was a multi laned road. Out of curiosity when I read more, it was revealing to know <a href="https://www.wikiwand.com/en/Broadway_(Manhattan)" target="_blank">broadway</a> is literal translation of Dutch words <a href="https://en.wiktionary.org/wiki/breed#Dutch" target="_blank">bret</a>, <a href="https://en.wiktionary.org/wiki/weg#Dutch" target="_blank">weg</a> and is one the oldest road stretching 53 km long through The New York City boroughs of Manhattan and the Bronx, New YorkThe county of Westchester, New York.
