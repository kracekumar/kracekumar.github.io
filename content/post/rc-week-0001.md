
+++
date = "2016-10-09 05:09:09+00:00"
draft = false
tags = ["recursecenter", "python", "bittorrent"]
title = "RC week 0001"
url = "/post/151547342115/rc-week-0001"
+++
This week, I made considerable progress on the BitTorrent client which I started a week back. The client is in a usable state to download the data from the swarm. The source code is available on <a href="https://github.com/kracekumar/bt" target="_blank">GitHub</a>. The project uses Python 3.5 `` async/await `` and <a href="https://docs.python.org/3/library/asyncio.html" target="_blank">asyncio</a>. I presented the torrent client in RC Thursday five minute presentation evening slot. Here is the link to the <a href="http://slides.com/kracekumarramaraju/bittorrent-client" target="_blank">slides</a>.

<a href="http://slides.com/kracekumarramaraju/bittorrent-client" target="_blank">

<img src="https://lh3.googleusercontent.com/3bvl-5OwgFkwHvero3PiqS4NAf0rVLEK86hGa1WEoBqJsatbf1zi6EAEhsPql4OfWVh4GLUkTXDz_ZsjfcpKOdfwK63li8KGp78xRW9xVUW3OwTCOBYoK_XSKsN4iIKVZi9hgr65_5oQtTeSBfbQRtJdHPgv_23LXKHlpO5jTIjCtLvYj-aXm_EkxOcCaud0WRISIOkYWIXFUZhfcTELtglNfwQF99ZV8VSeUB644VRLK2lSt_W0mo6LD8saggHdTS9Nae0a8z_EebfN1YpFYjOOGbLIcqiYHMUEDRWMikCsTQzm5943DAHakLXFGtPcvEYYdUSaWBlpOojg-ym1U1BBIy9T5MUFj6OnDm_Ig-1jBGVVvq0BEPtzFl5EZG-lwnYT643OjuUmBmWbIkylGVwhd6OwhG--VsogDZGSYbzFXNvC_VvJnFaUNn5g-1To4O8uARF-L5ApcVd91p4u233LRPWZYxqCCLf_T6OcCjbqPZmXzGxUFhyc73dJFMAcnV4B18JhiWamjhXH7TiIYmN4NkK6MbKOk7roUw3RLp0GaNMZ3ORrF97dsUxTz1GuRclwjZr9yn2NTLFBw9LXxhDGX32IfTCU53haG24raHpIau55=w1133-h708-no" width="600"/>

</a>

Here is quick video demo recorded with asciinema.

<a href="https://asciinema.org/a/88321" target="_blank">

<img src="https://asciinema.org/a/88321.png" width="600"/>

</a>

In the demo, the client downloads a file `` flag.jpg `` of 1.3MB. Thanks a lot for <a href="https://twitter.com/ballingt" target="_blank">Thomas Ballinger</a> for hosting the tracker and the seeder. The tracker and the seeder are boons for developers writing torrent clients.

The downloader has two known major issue

*   The client has performance issue for a file of size greater than 50 MB.
*   The client doesn’t support UDP tracker - Clients interact with piratebay tracker only on UDP protocol whereas the other trackers support HTTP endpoint.

The week had long hours of debugging blocking code in asyncio, tweaking the client to support receive data from the deluge client - as soon as the handshake is successful, the client starts sending `` bitfield, have `` messages before receiving the `` interested `` message.

The next step is to integrate seeding functionality to the torrent client and enhance UDP tracker functionality.

On one of the weekday, I witnessed the momentous incident. I decided to join other two other recursers for lunch at a nearby unvisited outlet to pick up the lunch. The shop was spacious with bar setup and we decided to eat in the building. We ordered three Panner Tika and were munching our meal. The manager/owner/bartender showed up to fill the water, said to another person, “Let me fill water for you.” She greeted him and asked, “How’s your day going? and …”. He enthusiastically replied; filled the glass; enquired about the food and moved on. After few moments, he returned with the exuberant joy; handed over her handful of candies and stated this is a gift for her good manners. Speechless moment! What a revelation and life lesson! The small act of courtesy made the person feel elated and must have made his day. All day and night, we spend time thinking of lighting smiles on family, friends, mates and others. Take a moment to spread joy among the strangers. Later, I recalled a quote,

>  
> “How you treat totally irrelevant person defines who you are.”
> 
