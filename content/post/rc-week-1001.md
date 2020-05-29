
+++
date = "2016-12-05 07:34:14+00:00"
draft = false
tags = ["recursecenter"]
title = "RC week 1001"
url = "/post/154068164505/rc-week-1001"
+++
This week I spent most of the time on working with Zulip, Erlang and <a href="https://github.com/kracekumar/edht" target="_blank">EDHT</a>.

### Zulip

This week, we (<a href="https://stanzheng.com/" target="_blank">Stan</a>, Jennifer and I) continued efforts on implementing message reaction. We made decent progress and had a good create reaction frontend. During the journey, we discovered an <a href="https://github.com/zulip/zulip/issues/2492" target="_blank">interesting bug</a>. Debugging the bug on production minified JS with the debugger was fun. We are close to completing the completing the initial version. It’s still a few days away from closure.

### Erlang

Day by day I am getting comfortable with Erlang. I learned about <a href="http://erlang.org/doc/man/supervisor.html" target="_blank">supervisors</a> and <a href="http://learnyousomeerlang.com/what-is-otp" target="_blank">OTP</a>. Using supervisors along with process feels like writing an OS. Supervisors provide the ability to monitor the process or processes and take action on failure. The functionality is critical while writing production code.

### EDHT

Past three weeks I have been learning Erlang and reading upon on DHT - Dynamo DB paper. That means little or no code. <a href="https://github.com/granders" target="_blank">Geoff Anderson</a> and I paired on Saturday started implementing <a href="https://github.com/kracekumar/edht" target="_blank">DHT</a>. The client and server (still single node, the cluster is coming ) communicate over UDP using protobuf protocol. Still, the communication format among nodes, failure detection is unclear. But spawning is Erlang is child’s play. I can think how hard is to spawn a new thread in `` rust `` and think about parameter lifetime. Both has it’s own advantage and disadvantage. Cool, no flame wars now.

### Statue de la Liberté

I avoided tourist spots in NYC for past two months. A good friend from India is visiting NYC, so I accompanied to Statue de la Liberté. The lady was tall bearing a torch and standing still. She was performing her business. I couldn’t find a single soul profoundly speaking about liberty or current status quo. Everyone was joyfully photographing the lady in the background. The museum in <a href="https://en.wikipedia.org/wiki/Ellis_Island" target="_blank">Ellis Island</a> near Statue of Liberty holds excellent US immigration information over centuries. One of the section represents the country wise immigrant visualization across all US states. The immigrants from India mostly live in BayArea, New York. The immigrants from Mexico, Germany, Netherland, Britain are spread across all states in similar proportion. The museum provides an audio guide device for interested visitors. The device comes with a small display and nine buttons like a feature phone. Every section has a number. So pressing a number `` 3 `` and hitting the start buttons plays a recorded audio about immigration process in the past. Yes, I can hear you geek, what happens when the total number of sections exceeds 9?

I hit a personal human bottom and lost all my credibility. Please don’t ask.

Last two weeks of the RC batch.
