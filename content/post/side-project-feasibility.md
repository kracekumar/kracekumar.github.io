+++
date = "2016-11-16 08:27:53+00:00"
draft = false
tags = ["recursecenter", "sideproject"]
title = "Side Project Feasibility"
url = "/post/153252087240/side-project-feasibility"
+++
It’s common for developers to work on side projects. The reason why developers work on a side project is a long list of imaginable and unimaginable reasons. My main reason to work on a side project is to learn how things work or to build a utility program for use.

One of my recent projects is to monitor the internet traffic and aggregate the traffic based on the domain name - bandwidth monitor. Before jumping into the code, I read a little bit about OSI Layers, TCP, UDP, IP and Ethernet packets. The project revolves around “domain name.” The assumption was at some layer domain name will be available in the packet. After capturing the packets, decoding the packet to TCP layer, I realized domain name is present in the HTTP header. I was happy with the approach. Everything worked for HTTP traffic. During the conversation of the project with <a href="https://github.com/porterjamesj" target="_blank">James  J Porter</a>, I mentioned parsing HTTP header is the way to retrieve the domain name and aggregate traffic information. He gave an alternative idea of caching DNS requests. But I was stuck with the notion of parsing HTTP request.

While testing `` HTTPS `` traffic, this solution fell apart. Unless the program has the SSL certificate of the domain and the key, the code can’t decode HTTPS packet. I couldn’t find a solution to parse encrypted traffic. Accidently, I opened a terminal and typed the command `` curl -v <favsite> ``. Suddenly the laser beam appeared from the dark terminal in the form of a cache miss message, `` * Hostname was NOT found in DNS cache ``. What? `` curl `` uses DNS cache? If `` curl `` caches DNS requests, there is a profound reason for it. Why not replicate the same behavior, then pull out IP from the IPv4 packet and derive domain name from DNS answer? There must be a reason why James even suggested the approach. This decision was the core for completing the <a href="https://github.com/kracekumar/imon" target="_blank">project</a>.

While working on the project, this thought kept bouncing in my mind, “What would have happened to the project if you weren’t at RC?”. I had two answers

*   I must have dumped the project in the middle of the journey and pivoted the project.
*   Somehow, I must have found DNS cache approach while walking in the park.

Take away for future projects

*   While I am at RC, I can explain the project intention, implementation approach, what I already know and I don’t know to someone who has knowledge of the subject or to a willing soul to hear me out.
*   What can I do while I am at not RC? Write the known, unknown details and ask for an opinion in Email/Twitter/IRC.
*   For certain projects like building an API Client or implementing a paper finding a reference implementation and looking at different parts of the code answers a lot of questions. I followed this approach while implementing BitTorrent client.

If your intention of the side project is to measure the feasibility, then this post is not for you :-)

What are your thoughts? What method do you follow while creating something for the first time?
