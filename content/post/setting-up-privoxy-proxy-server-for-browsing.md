+++
date = "2013-01-31 18:21:00+00:00"
draft = false
tags = ["ubuntu", "proxy", "privoxy"]
title = "Setting up privoxy proxy server for browsing"
url = "/post/41956153813/setting-up-privoxy-proxy-server-for-browsing"
+++
I wanted to setup proxy server for browsing. Tried&nbsp;<a href="http://www.squid-cache.org/" target="_blank">http://www.squid-cache.org/</a>&nbsp;felt cumbersome to configure though it has advanced features.

Finally decided to setup <a href="http://www.privoxy.org/" target="_blank">http://www.privoxy.org/</a>. I assume you have personal server where all the requests are forwarded.

__Installation__

__ Server Config__

_sudo apt-get install privoxy_

_sudo vim /etc/privoxy/config_

look for _listen-address_&nbsp;and add ip:port _listen-address 78.12.204.2:8118_

To enable logging for all requests uncomment _debug 1_(you will need to rotate log file using cronjob).Done with server config.

___Client Config___

Firefox users using ubuntu _Edit -> Preferences->Network->Connection->Settings_, choose manual proxy and enter ip address and port.

Chromium/Chrome uses system proxy, _Sytem->Network_&nbsp;look for _Configure HTTP Proxy_&nbsp;and enter details.

For command line use, add _HTTP\_PROXY=http://ip:port_ to _~/.bashrc_.
