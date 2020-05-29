+++
date = "2020-01-19 11:22:57+00:00"
draft = false
tags = ["kashmir", "censor"]
title = "153 sites allowed in Kashmir but no internet"
url = "/post/190341665270/153-sites-allowed-in-kashmir-but-no-internet"
+++
Kashmir is locked down without the internet for more than 167 days as of 19th Jan 2020 since 5th Aug 2019. The wire recently published <a href="https://thewire.in/government/kashmir-internet-whitelisted-websites" target="_blank">an article</a> wherein the Government of India whitelisted 153 websites access in Kashmir. Below is the list extracted from the <a href="https://gitlab.com/snippets/1931378" target="_blank">document</a>

<script src="https://gitlab.com/snippets/1931378.js"></script>

. The internet shutdown is becoming common in recent days during protests.

Anyone with little knowledge to create a web application or work can say, every web application will make network calls to other sites to load JavaScript, Style sheets, Maps, Videos, Images, etc.

### Accessing a Wikipedia page

The wikipedia.org is one of the whitelisted sites. If a user accesses the <a href="https://en.wikipedia.org/wiki/Freedom" target="_blank">Freedom Page</a>, they will see all the text in jumbled and <a href="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2e/OURS_TO_FIGHT_FOR._4_FREEDOMS_ON_ONE_SHEET_-_NARA_-_513635.jpg/440px-OURS_TO_FIGHT_FOR._4_FREEDOMS_ON_ONE_SHEET_-_NARA_-_513635.jpg" target="_blank">image</a> will be missing, since the image loads from _wikimedia.org_. wikimedia is not in the white listed sites. Even though you type one URL, your browser makes underhood requests to other sites.

<figure class="tmblr-full" data-orig-height="1814" data-orig-src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2e/OURS_TO_FIGHT_FOR._4_FREEDOMS_ON_ONE_SHEET_-_NARA_-_513635.jpg/1280px-OURS_TO_FIGHT_FOR._4_FREEDOMS_ON_ONE_SHEET_-_NARA_-_513635.jpg" data-orig-width="1280"><img data-orig-height="1814" data-orig-src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2e/OURS_TO_FIGHT_FOR._4_FREEDOMS_ON_ONE_SHEET_-_NARA_-_513635.jpg/1280px-OURS_TO_FIGHT_FOR._4_FREEDOMS_ON_ONE_SHEET_-_NARA_-_513635.jpg" data-orig-width="1280" src="https://66.media.tumblr.com/d11dbe320adc99b6c929c822beafa733/ac977494485d3af8-10/s540x810/47a228615a0350e32ded9d6aed9c5c0fb3bd3a0c.jpg" width="600"/></figure>

### Analysing these URLs

I wrote <a href="https://gitlab.com/snippets/1931380" target="_blank">a small script to download</a> all these sites’ home page and parsed the HTML to extract the potential network calls while loading JavaScript, StyleSheets, images, media, WebSocket, Fonts, etc.

These whitelisted sites make network calls to 1469 unique locations, including their sites and external sites like cdn.jsdelivr.net, googletagmanager.com, maps.googleapis.com. That means, to make these whitelisted sites work, the ISP needs to whitelist thousand more sites.

The whitelist sites contain top-level domains like ac.in, gov.in, nic.in - meaning allow users to access all the government sites.Can ISP filters pass on the traffic to specific top-level domains?

For example, IRCTC uses CDN(Content Delivery Network) to load bootstrap CSS(styling the web elements), font from cdn.jsdeliver.net and googleapis, theme from cdn.jsdeliver.net, google analytics service from googleanalytics.com, loads AI-enabled chatbot from <a href="https://corover.mobi/." target="_blank">https://corover.mobi/.</a> Failure to load all of these components will make the user unable to use the website - if not rendered by the browser, the site will be jumbled, the click actions on buttons will fail.

Other popular services that are missing in the whitelist - maps.googleapis.com, codex.nflxext.com, cdnjs.cloudflare.com, s.yimg.com, cdn.optimizely.com, code.jquery.com, gstatic.com, static.uacdn.net, cdn.sstatic.net, pixel.quantserve.com, nflxext.com, ssl-images-amazon.com, s3.amazonaws.com, rdbuz.com, oyoroomscdn.com, akamaized.net, etc.

Here is how amazon.in may look

<figure class="tmblr-full" data-orig-height="1784" data-orig-src="https://i.imgur.com/0gVszCi.png" data-orig-width="2868"><img alt="Amazon Home Page 1" data-orig-height="1784" data-orig-src="https://i.imgur.com/0gVszCi.png" data-orig-width="2868" src="https://66.media.tumblr.com/99e3373775d7ebd84c444ee8c060d318/ac977494485d3af8-28/s540x810/3dc5601c5e51edd0b394494af2a1e2dfb680c03b.png" width="600"/></figure>

<figure class="tmblr-full" data-orig-height="1784" data-orig-src="https://i.imgur.com/gAmyt5g.png" data-orig-width="2868"><img alt="Amazon Home Page 2" data-orig-height="1784" data-orig-src="https://i.imgur.com/gAmyt5g.png" data-orig-width="2868" src="https://66.media.tumblr.com/de9497d498a9326567a0fa01317e21f2/ac977494485d3af8-7c/s540x810/71cb69d044afa084746cc104a7f2d495fee09aac.png" width="600"/></figure>

There is a typo in one of the whitelist sites, <a href="http://www.hajcommitee.gov.in" target="_blank">www.hajcommitee.gov.in</a>, with a missing ’t,’ the correct URL is <a href="http://www.hajcommittee.gov.in" target="_blank">www.hajcommittee.gov.in</a>. These two sites: <a href="https://www.jkpdd.gov.in/," target="_blank">https://www.jkpdd.gov.in/,</a> <a href="https://www.jkpwdrb.nic.in" target="_blank">https://www.jkpwdrb.nic.in</a>, the browser fails to resolve.

Facebook, Twitter, Instagram, YouTube, WhatsApp, and other social media sites are blocked, whereas JioChat is allowed. The document mentions “JIO chat” does not specify the domain and application like www.

### Conclusion

> Internet (noun): an electronic communications network that connects computer networks and organizational computer facilities around the world

Overall, the internet cannot work by allowing only whitelist sites. As said earlier, underhood, the browser makes calls to a plethora of locations mentioned by the developers and cannot function entirely and unusable from the beginning to the end.
