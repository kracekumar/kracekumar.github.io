
+++
date = "2016-10-17 06:04:45+00:00"
draft = false
tags = ["python", "recursecenter"]
title = "RC Week 0010"
url = "/post/151921371645/rc-week-0010"
+++
This week has been a mixed ride with the torrent client. I completed the two pending features seeding and UDP tracker. The torrent client has a major issue with downloading larger torrent file like `` ubuntu `` iso file. The client starts the downloads from a set of peers and slowly halts at <a href="https://github.com/kracekumar/bt/blob/curio/bt/protocol.py#L75" target="_blank">sock.recv</a> after exchanging a handful of packets. At this juncture CPU spikes to 100% when `` sock.recv `` blocks. Initially, the code relied on `` asyncio `` only features, now the code uses `` curio `` library. Next time you write `` async `` code in Python 3, I would suggest use curio. Curio’s single feature of tracking all `` tasks `` states is magical wand for debugging. The live debugging facility helped me track down the blocking part of my code. Here is how curio’s debug monitor looks

<div class="gist"><a href="https://gist.github.com/kracekumar/c0513a589f14ec0fd270d526cec127c3" target="_blank">https://gist.github.com/kracekumar/c0513a589f14ec0fd270d526cec127c3</a></div>

I am sure; the bug should be a logical error in the code, or I am doing async completely wrong. I will travel with a bug for a day or two and see where I land. This task is slowly emerging the toughest bug I have faced.

I am happy, people at RC are keen to help and assist in all different ways. In case you’re reading this and have asyncio expertise to assist me, I will glad to hear. The easiest way to replicate the bug is to check out the code, switch to `` curio `` branch, install requirements in Python 3.5 venv and run the command `` python cli.py download ~/Downloads/ubuntu-16.04.1-desktop-amd64.iso.torrent --loglevel=debug ``. After a minute or two you can see the program using 100% CPU in `` htop `` or `` top ``. Feel free to leave a comment or ping me in twitter.
