+++
date = "2016-11-28 03:49:59+00:00"
draft = false
tags = ["recursecenter"]
title = "RC week 1000"
url = "/post/153759852775/rc-week-1000"
+++
This week has been quiet, bikeshedding, holiday week and unproductive week so far in RC.

### Zulip

<a href="https://github.com/zulip/zulip" target="_blank">Zulip</a> is a Python-based open source group chat. RC uses zulip for the internal chat. Unlike GitHub, zulip doesn’t support inline emoji reaction for messages. I am collaborating with <a href="https://github.com/arpith" target="_blank">Arpith</a> and <a href="https://github.com/stanzheng" target="_blank">Stan Zheng</a> to add the missing <a href="https://github.com/zulip/zulip/pull/2387" target="_blank">feature</a>. The first time I came across the <a href="https://github.com/zulip/zulip/pull/2387/commits/159854cc62f19c70193a56fc868853f157640730#diff-c29a639b9c074638e5b08d59f0be3997R974" target="_blank">limitation</a> of Django’s `` values `` method. If you’re considering setting up a group chat for your community or company, try out zulip!

### Erlang

Now I am getting the hang of the language. The uncomfortable part is writing recursive functions. I still think in loops; sometimes I write counterpart in Python and port to Erlang. I feel Erlang code is readable like Python. Now, I am reading up socket handling and message passing. Once I cross few more chapters, I will get my hands dirty on implementing DHT.

### Streisand

For very long time, I was thinking of using VPN. Finally, I asked the advice and setup <a href="https://github.com/jlund/streisand" target="_blank">streisand</a>. So what is Streisand? Quoting directly from GH description

>
> Streisand sets up a new server running L2TP/IPsec, OpenConnect, OpenSSH, OpenVPN, Shadowsocks, sslh, Stunnel, and a Tor bridge. It also generates custom configuration instructions for all of these services. At the end of the run you are given an HTML file with instructions that can be shared with friends, family members, and fellow activists.
>

The project comes with ansible playbook and orchestration is done from local computer for a chosen providers. So you roll out your VPN server without losing sanity! I decided to deploy in DigitalOcean. The default procedure had issues with picking up `` SSH Keys ``, so provisioned the machine and ran the command `` ansible-playbook playbooks/streisand.yml ``. The setup took ten minutes, and all the services were up and running. Till now no issues with the server at all.

The project auto generates documentation with instructions for using various supported protocols and devices. The generated instruction is super helpful and accurate except setting up `` L2TP/IPSec `` on Linux. The project supports different protocols; I choose `` L2TP/IPSec ``.

<a href="https://www.wikiwand.com/en/Layer_2_Tunneling_Protocol" target="_blank">L2TP - Layer 2 Tunneling Procotol</a> is a protocol for supporting virtual private network without encryption. For encryption <a href="https://www.wikiwand.com/en/IPsec" target="_blank">IPsec</a> is used. IPsec takes care of authentication and encryption. The device connects to a VPN server with `` username `` and `` password ``. Once the authentication is successful, all the traffic origination from the machine is directed to VPN server. So you can avoid incidents like <a href="https://medium.com/@karthikb351/airtel-is-sniffing-and-censoring-cloudflares-traffic-in-india-and-they-don-t-even-know-it-90935f7f6d98#.wtz9evom2" target="_blank">this</a> and get rid off geographic firewall, packet sniffing, and other threats.

Setting up `` L2TP/IPsec `` on `` iOS, Mac OSX, Andriod `` is a cake walk. But on `` Linux `` I burnt all my midnight oil. The Streisand has no documentation for setting on any Linux system. The tutorials from <a href="https://www.elastichosts.com/support/tutorials/linux-l2tpipsec-vpn-client/" target="_blank">elastichosts</a> and <a href="https://wiki.archlinux.org/index.php/Openswan_L2TP/IPsec_VPN_client_setup" target="_blank">ArchLinux wiki</a> was my saviour. I had never configured any of these before, and the configuration file format is different. I spent 3-4 hours battling with `` ipsec verify `` and `` xl2tpd `` to get up and running. Finally, `` syslog `` helped me debug the issue.

For the first time, I got a chance to use <a href="https://www.wikiwand.com/en/Iptables" target="_blank">iptables</a>. Iptables is a <a href="https://www.wikiwand.com/en/User_space" target="_blank">user-space</a> command line packet filtering tool. The tool provides options to filter the incoming and outgoing packets. So try the command `` ip route show `` in your terminal.

During the setup, multiple times I configured the wrong values in iptables and my machine was out of internet connection. I shot my foot several times :-( In `` L2TP/IPsec ``, iptables is used to forward all packets to VPN server. All the `` iptables `` entries live as long as the machine is running. After the restart or reboot system, all the values need to be reconfigured. So there is a project `` iptables-persistent `` to persist entries after the system reboot.

I am still having issues with automating `` init `` bash scripts for Linux. The iptables entries differ depending on the network subnets. My home network subnet is `` 192.168.0.0/24 ``, and at premise, the subnet is `` 10.0.0.0/16 ``, `` 10.0.0.0/24 ``. The values change for devices like `` wlan0 `` and `` eth0 ``. As of now I running the script every time, I restart the machine.

Here is a sample add entry

`` ip route add <vpn server> via 192.168.1.1 dev wlan0 ``

Here `` 192.168.1.1 `` is the gateway and `` wlan0 `` refers WiFi device.

### Thanksgiving

Last Thursday was <a href="https://www.wikiwand.com/en/Thanksgiving" target="_blank">Thanksgiving</a>, and the premise was quiet, and vibe of the place was totally missing.

>
> On Thursday, all my favorite veg brunch and dinner places were shut down; but all halal carts were on the streets. Few city blocks were dark, and all the building light was turned off but fine dining places were decorated and colorful. Few avenues had street vendors; the busiest roads were resting contentfully with occasional visitors. The subway car was quiet, folks were relaxed and more than half the seats were empty. The majority of humans departed the city but not the cars. At 10:30 PM street person was waiting for the traffic signal to turn red to collect few spare changes. So who celebrates Thanksgiving?
>

The countdown starts last three weeks of my RC batch.
