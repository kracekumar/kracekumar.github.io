
+++
date = "2016-10-31 05:31:05+00:00"
draft = false
tags = ["rust", "recursecenter"]
title = "RC Week 0100"
url = "/post/152543733085/rc-week-0100"
+++
I had a decent progress with my rust HTTP traffic monitor. I read a lot about the different type of packets like Ethernet Packet, IPv4 Packet, TCP Packet, UDP Packet, DNS Packets. I wrote a parser for Physical Layer packet, TCP Packet, UDP Packet, DNS Packet. The parser helped me understand a lot about rust functions, data types like `` array, vectors ``, string and string literal. I am not enjoying the relationship with rust lifetimes. Lifetime is getting harder for me to grok with nested struct’s lifetime.

Big thanks to <a href="https://github.com/k4rtik" target="_blank">k4rtik</a> for solving my last <a href="https://github.com/ebfull/pcap/issues/58" target="_blank">week issue</a>. While writing the <a href="https://github.com/kracekumar/imon/commit/1d4ab6a2a300dfa36ca6ba7da6473119e950aeab" target="_blank">parser</a>, I missed <a href="https://docs.python.org/2/library/struct.html" target="_blank">Python’s struct module</a>. Python struct module helps writing network code easier by decoding different data types like `` string, char, unsigned and signed integer `` by specifying the format against a binary data. I think it’s a useful piece of utility to build in rust. The code is an ugly mess of pieces tied together to work. Once the utility works, I will convert the working bits into idiomatic rust.

Next step is to get the program to store the captured HTTP packet to store in SQLite table indexed by date.

Thanks to <a href="https://nick-platt.com/" target="_blank">Nick Platt</a> for answering my rust questions, assisting on debugging and deciphering rust errors messages.
