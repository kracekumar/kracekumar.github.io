
+++
date = "2013-04-12 21:12:08+00:00"
draft = false
tags = ["javascript", "facepalm", "iframe"]
title = "hardest feature request "
url = "/post/47806200096/hardest-feature-request"
+++
I was working on <a href="http://hasgeek.tv" target="_blank">Hgtv</a> feature, syncing `` slides `` and `` videos ``, when video is viewed slide changes automatically. Seems easy but guess what someone has to take pain to watch entire video and collect the details about timings of the video and slide number . Then pass on the info to <a href="https://github.com/ffissore/presentz.js" target="_blank">presentz.js</a> which syncs video and slides.

## Iframe

If you look into source code how slide show and video is inserted, it is iframe. All videos are from `` youtube ``, slides are from `` speakerdeck ``, `` slideshare ``. Now to sync video and slide, I need to fetch the slide number and current time of video. `` Youtube `` has js api, which was easy to figure, but speakerdeck and slideshare inserts images into iframe. When next button is clicked image is changed. If I can access DOM I am done, but unfortunately you cannot access the DOM of an Iframe for a Cross Origin Request. I found this info after one whole day of tinkering and trying all answers in stackoverflow. Then I looked into presentz JS how it handles slide changing. Speakerdeck receives postMessage, it accepts `` nextSlide ``, `` previousSlide ``, `` goToSlide `` messages. Once speakerdeck processed the messages and sends message to originator, and the received message has to be processed(`` window.addEventListener ``). Before figuring above messages I was brute forcing to get figure out how to get current slide. Once I figured it only accepts three message, then it was easy. Below is the code.

        var receiveMessage = function(event){
        var data;
        if (event.origin.indexOf("speakerdeck.com") === -1){
          if (event.origin.indexOf("slideshare.net") === -1){
            return;
          }
        }
        data = $.parseJSON(event.data);
        if (data[1]) {
          slideno = data[1].number;
        }};

Then register event listener

    window.addEventListener("message", receiveMessage, false);

Then slideshare was bit out of track. Slideshare had js api which requires swfobject and swfobject\_playerapi. Then it was bit easier still took me some time to figure out missing `` swfobject_playerapi `` is required.

It took me five days to finally get this feature and <a href="https://github.com/hasgeek/hasgeek.tv/pull/86" target="_blank">pull</a> is ready.

## Learning:

*   I learned coffeescript.
*   Parsing DOM of Iframe is not possible for CORS.
*   `` postMessage ``

This feature was so far the hardest one.

## Root Cause

The root cause for this problem, is my \*\*Ass u mption \*\*. I was trying to use presentz.js to get current slide and video player timings.
