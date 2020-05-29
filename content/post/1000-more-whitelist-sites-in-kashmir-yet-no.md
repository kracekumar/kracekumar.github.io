+++
date = "2020-02-21 20:13:15+00:00"
draft = false
tags = ["kashmir", "censor"]
title = "1000 more whitelist sites in Kashmir, yet no Internet"
url = "/post/190951734050/1000-more-whitelist-sites-in-kashmir-yet-no"
+++
Kashmir is under lockdown for more than

``` bash
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
200,200,200,200,200,200,200,200,200,200,
```


200 days.

Last Friday(14th Feb, 2020), Government released a <a href="http://www.jkhome.nic.in/13TSTSof2020.pdf" target="_blank">document</a>with list of allowed whitelist sites at 2G speed. The text file with URLs extracted from the PDF - <a href="https://gitlab.com/snippets/1943725" target="_blank">https://gitlab.com/snippets/1943725</a>

In case you’re not interested in tech details, analysis, or short of time, look at the `` summary `` section at the bottom.

### DNS Query

The browser converts a domain name or URL into an IP address. This is step is called DNS querying.

I wrote the <a href="https://gitlab.com/snippets/1941923" target="_blank">script</a> to emulate the DNS request of the browser.

Out of 1464 entries in the PDF, only `` 90.9% (1331) `` of the URLs receive an answer(IP Address or sub-domain) for the DNS query.`` 9.1% (133) `` are either not domain names like (Airtel TV) and non-existent entities like <a href="http://www.unacademy.com" target="_blank">www.unacademy.com</a>.

<figure class="tmblr-full" data-orig-height="450" data-orig-src="https://i.imgur.com/1KCsXdJ.png" data-orig-width="700"><img data-orig-height="450" data-orig-src="https://i.imgur.com/1KCsXdJ.png" data-orig-width="700" src="https://66.media.tumblr.com/0a12232a67aed68c844a0a3eda426cdf/03ff75705f2ef1b0-2e/s540x810/d13c55c079889710bd8f521714ca7e3ec678d49b.png" width="600"/></figure>

Out of 1331 DNS answers, except one URL all other DNS query answer contains at least one IP address. That means no further DNS query and all the webserver is reachable.

<figure class="tmblr-full" data-orig-height="934" data-orig-src="https://i.imgur.com/0xshUU8.png" data-orig-width="2448"><img data-orig-height="934" data-orig-src="https://i.imgur.com/0xshUU8.png" data-orig-width="2448" src="https://66.media.tumblr.com/1747cce97ec7e45cd9400d89af3c9826/03ff75705f2ef1b0-ab/s540x810/0bbcdf99dbb38eb74764ba9fee5bad08475ea645.png" width="600"/></figure>

Out of `` 133 `` failed DNS queries, `` 113 `` are non-existent URLs/URIs(for simplicity you can assume non-existent hostname/sub-domain)like <code><a href="http://www.unacademy.com" target="_blank">www.unacademy.com</a>(the application is available at unacademy.com), <a href="http://www.testzone.smartkeeda.com" target="_blank">http://www.testzone.smartkeeda.com</a>, <a href="http://164.100.44.112/ficn/" target="_blank">http://164.100.44.112/ficn/</a></code> and 22 entries are invalid like `` Gradeup (App), 122.182.251.118 precision biometrics ``.

<figure class="tmblr-full" data-orig-height="898" data-orig-src="https://i.imgur.com/NbCvFyM.png" data-orig-width="1506"><img data-orig-height="898" data-orig-src="https://i.imgur.com/NbCvFyM.png" data-orig-width="1506" src="https://66.media.tumblr.com/5d0534e41f6fb2c03b83fb47b5292cdc/03ff75705f2ef1b0-3b/s540x810/9a97a0712ea55d47df6985e0ddb017bad8a6aebb.png" width="600"/></figure>

<code><a href="http://www.unacademy.com" target="_blank">www.unacademy.com</a></code> doesn’t have ‘A’ address associated, and browser or resolver will fail, and <code><a href="http://unacademy.com" target="_blank">http://unacademy.com</a></code> only has the A address.

<figure class="tmblr-full" data-orig-height="182" data-orig-src="https://i.imgur.com/Xak8tt7.png" data-orig-width="2730"><img data-orig-height="182" data-orig-src="https://i.imgur.com/Xak8tt7.png" data-orig-width="2730" src="https://66.media.tumblr.com/626b65c4cc13cb25e697d052cc7a0452/03ff75705f2ef1b0-e2/s540x810/8480e846807ffbb42be21009609cbd7f91f69d9e.png" width="600"/></figure>

<figure class="tmblr-full" data-orig-height="1672" data-orig-src="https://i.imgur.com/rVUYjmV.png" data-orig-width="2730"><img data-orig-height="1672" data-orig-src="https://i.imgur.com/rVUYjmV.png" data-orig-width="2730" src="https://66.media.tumblr.com/6d7ca79a1c1add58afc60ce5ae029586/03ff75705f2ef1b0-03/s540x810/6806db6b9294f9818dfe104c05949364b6f6da70.png" width="600"/></figure>

`` 11 `` entries in the list are IP addresses without hostname like <code><a href="http://164.100.44.112/ficn/" target="_blank">http://164.100.44.112/ficn/</a></code>.

Four out of eleven IP addresses refuse to accept the `` HTTP GET `` request. Two of the IP address doesn’t host any page, and two of the IP address refuse connection due to SSL Error, `` SSLCertVerificationError("hostname '59.163.223.219' doesn't match either of 'tin.tin.nsdl.com', 'onlineservices.tin.egov-nsdl.com" ``.

All of the explicit IPs in the list are some sort of government service.

### Whitelist group

Some of the entries in the PDF are from the same domain but with different sub-domain like `` example.com, api.example.com. ``

<figure class="tmblr-full" data-orig-height="886" data-orig-src="https://i.imgur.com/pkoASK4.png" data-orig-width="2214"><img data-orig-height="886" data-orig-src="https://i.imgur.com/pkoASK4.png" data-orig-width="2214" src="https://66.media.tumblr.com/48fc1db33861302e3b2a5183aa0d26d2/03ff75705f2ef1b0-68/s540x810/87559bdf9845e3f54a5dcf6b089e34e91df8bc6c.png" width="600"/></figure>

Analysis of the domain name association reveals, there are 23 URLs of trendmicro domain - a cybersecurity company,19 URLs of rbi(reserve bank of India), 10 URLs of belonging to Google(Alphabet) product yet google.co.in is not on the list.

<figure class="tmblr-full" data-orig-height="1360" data-orig-src="https://i.imgur.com/tKgery3.png" data-orig-width="2056"><img data-orig-height="1360" data-orig-src="https://i.imgur.com/tKgery3.png" data-orig-width="2056" src="https://66.media.tumblr.com/2d673c39c7e23be6bd6a187f9d52abc4/03ff75705f2ef1b0-56/s540x810/f3c7e64bcf450a56db9035be1ec1322a61aa20b0.png" width="600"/></figure>

### Spellings

In the last two PDFs, haj committee domain was misspelled. The misspelled still exists along with the correct spelling and irrelevant entry Haj\_Committee.gov.in.

The new misspelled domains are <a href="http://www.scientificsmerican.com" target="_blank">www.scientificsmerican.com</a>(scientific American?), flixcart.com(Flipkart?).

### Product Names

It’s unclear how the ISP will interpret the URL, `` http://Netflix. `` Netflix loads resources from the following places`` netflix.com, codex.nflxext.com, assets.nflxext.com, nflximg.com ``.Does the ISP download the netflix.com, figure out all the network calls, and whitelist all the network resources?Or they allow only the Netflix domain?

JIO chat mentioned in previous PDFs is missing in the current list.

A classification of URLs from the PDF

<figure class="tmblr-full" data-orig-height="450" data-orig-src="https://i.imgur.com/pLTdFHm.png" data-orig-width="700"><img data-orig-height="450" data-orig-src="https://i.imgur.com/pLTdFHm.png" data-orig-width="700" src="https://66.media.tumblr.com/26fa80710ab8f5354caecde27cf07304/03ff75705f2ef1b0-d9/s540x810/a0e994f7f72c99f43005ae82ce3e1c16f0ebb8db.png" width="600"/></figure>

### Site Reachability

Say if all the URLs DNS requests pass through(even though it’s not), how many URLs will return a response to the browser? The source code available in <a href="https://gitlab.com/snippets/1943713" target="_blank">GitLab</a>

Only `` 74.6% `` entries are reachable - the browser receives a proper response (HTTP 200 response for `` GET `` request).`` 20 % `` of the websites fail to respond to HTTP GET requests due to various errors like SSL issues, non-existent siteor technical misconfiguration, or unable to interpret the URL like Airtel TV.

`` 6% `` URLs may or may not work since some of the URLs doesn’t exist(<code><a href="https://www.khanacademy.org/math/in" target="_blank">https://www.khanacademy.org/math/in</a></code>)or accept GET request(<code><a href="https://blazenet.citruspay.com/" target="_blank">https://blazenet.citruspay.com/</a></code>), but the domain or sub-domain or IP address exists, or issues redirect or don’t accept GET request.

<figure class="tmblr-full" data-orig-height="450" data-orig-src="https://i.imgur.com/ysbmYOD.png" data-orig-width="700"><img data-orig-height="450" data-orig-src="https://i.imgur.com/ysbmYOD.png" data-orig-width="700" src="https://66.media.tumblr.com/6383ecd4dfb79b850581ef86bf2b80a9/03ff75705f2ef1b0-7d/s540x810/62630b2d5b2b9cb4a906b03ea25319c345bf5d67.png" width="600"/></figure>

The domain <a href="http://aiman.co" target="_blank">http://aiman.co</a> redirects to tumblr.com.

The list doesn’t contain domain `` safexpay `` but the load balancer(going by the convention of naming), `` safexpay-341596490.ap-south-1.elb.amazonaws.com `` is in the list.

Do human agents verify these URLs before adding to the list?

### Missing static content hosts

The Websites host JavaScripts, fonts, StyleSheet, static assets on third party services for fast load time. All cloud services provide a similar service. The important CDNs to load are present in the whitelist like`` googleapis.com, cdnjs.cloudflare.com, gstatic.com, fbcdn.net (facebook CDN), ``

Other popular services are missing like `` s3.amazonaws.com, nflxext.com, akamai ``.

All the problems mentioned in the <a href="https://kracekumar.com/post/190341665270/153-sites-allowed-in-kashmir-but-no-internet" target="_blank">post</a>, to load static assets and in-general continue.

### 2G speed

I picked up random sites as a sample set(less than 100 websites - worth analysisng entire list); on average, each website makes 68 calls to load the home page, and an average size is 2.5 MB.These 68 network calls are from the mixed bag - same host, sub-domains, third party sites for JS, CSS, and images, etc.

Theoretical 2G speed is 40 kilobits per second. 2.5 G speed is 384kilobitss per second. Even when the browser can load all the resources in parallel, it may take exponential time and not linear time while deducing from higher speed to lower speed.

The browser opens only six connections to the host. If the server timeouts, the browser doesn’t even retry.

In the chrome developer tool, there is no option to test a site in 2G. Allowed speed throttling is 3G, 4G.

In 20 Mbps speed, the browser takes 5 seconds to load a web page(consider all DNS queries pass through) of size 1.9 MB and, in a slow 3G connection, takes 13.79s in Chrome.

The real speed will be worse in 2G.

The chrome developer tools don’t provide an option for simulating 2G speed. It is clear, no one test web sites in 2G speed. Airtel plans to shut down 2G service by mid of 2020 and reliance already shut down 2G service.

The process of whitelisting websites raises pertinent questions.

*   who chooses these websites?
*   what is the methodology to choose the websites?
*   what is the rationale of choosing 2G speed and banning VPN usage?
*   why even whitelist?
*   more

### Summary

1.   DNS query resolves a domain name into an IP address. Out of 1464 entries in the PDF, only 90.9% (1331) of the URLs receive an answer(IP Address or sub-domain) for the DNS query.9.1% (133) are either not domain name like (Airtel TV) and non-existent entities like <a href="http://www.unacademy.com" target="_blank">www.unacademy.com</a>.
2.   Out of 1330, except one URL all other DNS query answer contains at least one IP address.
3.   Out of 133 failed DNS queries, 113 are non-existent URLs/URIs(for simplicity you can assume non-existent hostname/sub-domain)like <code><a href="http://www.unacademy.com" target="_blank">www.unacademy.com</a>(the application is available at unacademy.com), <a href="http://www.testzone.smartkeeda.com" target="_blank">http://www.testzone.smartkeeda.com</a>, <a href="http://164.100.44.112/ficn/" target="_blank">http://164.100.44.112/ficn/</a></code> and 22 entries are invalid like `` Gradeup (App), 122.182.251.118 precision biometrics ``.
4.   11 entries in the list are IP addresses without hostname like <code><a href="http://164.100.44.112/ficn/" target="_blank">http://164.100.44.112/ficn/</a></code>. Four out of eleven IP Addresses refuse to accept HTTP GET requests. Two of the IP address doesn’t host any page, and two of the IP address refuse connection due to SSL Error, \`SSLCertVerificationError(“hostname '59.163.223.219’ doesn’t match either of 'tin.tin.nsdl.com,’ 'onlineservices.tin.egov-nsdl.com”.
5.   Three entries are just hard to match keywords, unlike Airtel TV - umbrella, lancope, flexnetoperations.
6.   In the last two PDF haj committee domain was misspelled. The misspelled still exists along with the correct spelling and irrelevant entry Haj\_Committee.gov.in.
7.   Scientific American and Flipkart are misspelled - <a href="http://www.scientificsmerican.com" target="_blank">www.scientificsmerican.com</a>, flixcart.com.
8.   There are 21 URLs of trendmicro domain,19 URLs of rbi,10 URLs of belonging to Google(Alphabet) product, yet google.co.in is not on the list.
9.   74.6% entries are reachable - the browser receives proper response (HTTP 200 response for `` GET `` request).20% of the website fails to respond to HTTP GET requests due to various errors like SSL issues, non-existent sites. 6% URLs may or may not work since some of the URLs don’t exist, but the domainor sub-domain or IP address exist, or issues redirect or don’t accept GET request.
10.   Among the sample of URLs, on average, each website makes 68 calls to load the home page and an average size of 2.5 MB.
11.   Some of the CDNs to load JavaScripts, fonts, StyleSheet, static assets are present in the whitelist like`` googleapis.com, cdnjs.cloudflare.com, gstatic.com, fbcdn.net (facebook CDN), ``Other popular services are missing like `` s3.amazonaws.com, nflxext.com, akamai ``.
12.   The list doesn’t contain domain `` safexpay `` but the load balancer(going by the convention of naming),`` safexpay-341596490.ap-south-1.elb.amazonaws.com `` is in the list.
13.   All the problems mentioned in the post, <a href="https://kracekumar.com/post/190341665270/153-sites-allowed-in-kashmir-but-no-internet" target="_blank">https://kracekumar.com/post/190341665270/153-sites-allowed-in-kashmir-but-no-internet</a>continues - whitelist sites can’t provide complete access to the site and prevents the wholesome experience of the internet.
14.   2G speed is 40 kilobits per second. 2.5 G speed is 384 kilobits per second. In the chrome developer tool, there is no option to test a site in 2G. Allowed speed throttlings are 3G, 4G. In a slow 3G connection, the chrome browser takes 13.79s to load a 1.9 MB size webpage. It is clear, no one test sites in 2G speed. Airtel plans to shut down 2G service by mid of 2020 and reliance already shut down 2G service.

### Conclusion

> Internet (noun): an electronic communications network that connects computer networks and organizational computer facilities around the world

Overall, the internet cannot work by allowing only whitelist sites.Underhood, the browser makes calls to a plethora of locations mentioned by thedevelopers and cannot function entirely and unusable from the beginning to the end.

Credits:

*   HashFyre for converting the PDF to text file using OCR.
*   Kingsly for pointing out DNS specific analysis - <a href="https://twitter.com/kracetheking/status/1229155303062745088?s=20" target="_blank">https://twitter.com/kracetheking/status/1229155303062745088?s=20</a>
