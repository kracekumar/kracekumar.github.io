
+++
date = "2020-01-26 17:31:40+00:00"
draft = false
tags = ["python", "proxy", "HTTP"]
title = "Capture all browser HTTP[s] calls to load a web page"
url = "/post/190478935610/capture-all-browser-https-calls-to-load-a-web"
+++
How does one find out what network calls, browser requests to load web pages?

The simple method - download the HTML page, parse the page, find out all the network calls using web parsers like beautifulsoup.

The shortcoming in the method, what about the network calls made by your browser before requesting the web page? For example, firefox makes a call to `` ocsp.digicert.com `` to obtain revocation status on digital certificates. The protocol is <a href="https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol" target="_blank">Online Certificate Status Protocol</a>.

The network tab in the browser dev tool doesn’t display the network call to ocsp.digicert.com.

One of the ways to find out all the requests originating from the machine is by using a proxy.

### Proxy

<a href="https://mitmproxy.readthedocs.io/en/v2.0.2/" target="_blank">MITMProxy</a> is a python interactive man-in-the-middle proxy for HTTP and HTTPS calls. Installation instructions <a href="https://mitmproxy.readthedocs.io/en/v2.0.2/install.html" target="_blank">here</a>.

After installing the proxy, you can run the command `` mitmweb `` or `` mitmdump ``. Both will run the proxy in the localhost in the port 8080, with `` mitmweb ``, you get intuitive web UI to browse the request/response cycle and order of the web requests.

<figure class="tmblr-full" data-orig-height="1624" data-orig-src="https://i.imgur.com/clvrOHz.png" data-orig-width="2852"><img data-orig-height="1624" data-orig-src="https://i.imgur.com/clvrOHz.png" data-orig-width="2852" src="https://66.media.tumblr.com/93077115fcf6344fefdfa8381a37b0e7/0e08bfea412a18df-e1/s540x810/f620e229b6e5e8c1e8f2839a15f6665a5a5f4ecf.png" width="600"/></figure>

As the name indicate, “man in the middle,” the proxy gives the ability to modify the request and response. To do so, a simple python script with a custom function can do the trick. The script accepts two python functions, `` request `` and `` response. `` After every request, `` request `` and after every response, `` response `` method will be called. There are other supported concepts like addons.

To extract the details of the request and response. Here is a small script, <a href="https://gitlab.com/snippets/1933443." target="_blank">https://gitlab.com/snippets/1933443.</a>

<figure class="tmblr-full" data-orig-height="1644" data-orig-src="https://i.imgur.com/u4Tlvv5.png" data-orig-width="2868"><img data-orig-height="1644" data-orig-src="https://i.imgur.com/u4Tlvv5.png" data-orig-width="2868" src="https://66.media.tumblr.com/aa5b111e46116ccf93f16aa9f8236150/0e08bfea412a18df-78/s540x810/df18e4a309b2774c221318e3c96fc52b1d051181.png" width="600"/></figure>

After receiving a successful response, the MITM invokes the function `` response ``; the function collects the details and dumps the details to the JSON file. `` uuid4 `` will ensure a unique file name for every function call.

Even though MITM provides an option to dump the response, it’s in binary format and suited for data analysis.Next is to simulate the browser request.

### Selenium

Selenium is one of the tools used by web-developers and testers for an end to end web testing. Selenium helps developers to test the code on the browser and assert the HTML elements on the same page.

To get selenium Python web driver working, one needs to install Java first, followed by <a href="https://github.com/mozilla/geckodriver/releases" target="_blank">geckodriver</a>, and finally Python selenium driver(`` pip install selenium ``). Don’t forget to place geckodriver in `` $PATH. ``

Code: <a href="https://gitlab.com/snippets/1933454" target="_blank">https://gitlab.com/snippets/1933454</a>

<figure class="tmblr-full" data-orig-height="990" data-orig-src="https://i.imgur.com/uw9tmKg.png" data-orig-width="1956"><img data-orig-height="990" data-orig-src="https://i.imgur.com/uw9tmKg.png" data-orig-width="1956" src="https://66.media.tumblr.com/4cd6a49bd2df0e85cce3133721970de8/0e08bfea412a18df-be/s540x810/49105b664a6f0b573f55e72af1ab9ecec04878a0.png" width="600"/></figure>

Run the MITM in a terminal, then selenium another terminal, the output directory must be filled with JSON files.

Here is how the directory looks

<figure class="tmblr-full" data-orig-height="716" data-orig-src="https://i.imgur.com/3Jp0FvI.png" data-orig-width="2702"><img data-orig-height="716" data-orig-src="https://i.imgur.com/3Jp0FvI.png" data-orig-width="2702" src="https://66.media.tumblr.com/aea3bc56c0b3ece1455bde9911904e97/0e08bfea412a18df-48/s540x810/7aa463e4c9144e30494f66f5635c87ed0dd6b57a.png" width="600"/></figure>

The sample JSON files output

<figure class="tmblr-full" data-orig-height="286" data-orig-src="https://i.imgur.com/i3A62F9.png" data-orig-width="2852"><img data-orig-height="286" data-orig-src="https://i.imgur.com/i3A62F9.png" data-orig-width="2852" src="https://66.media.tumblr.com/5bf5aab579c02e06bdd506e7d67237cf/0e08bfea412a18df-1e/s540x810/0f5b718ee85f4f61d60fb06b0d1a37e71a312785.png" width="600"/></figure>
