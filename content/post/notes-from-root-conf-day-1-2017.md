+++
date = "2017-05-11 21:11:23+00:00"
draft = false
tags = ["devops", "notes", "conference"]
title = "Notes from Root Conf Day 1 - 2017"
url = "/post/160561774060/notes-from-root-conf-day-1-2017"
+++
<a href="https://rootconf.in/2017/" target="_blank">Root Conf</a> is a conference on DevOps and Cloud Infrastructure. 2017 edition’s theme is service reliability. Following is my notes from Day 1.

1. <a href="https://rootconf.talkfunnel.com/2017/63-state-of-the-open-source-monitoring-landscape" target="_blank">State of the open source monitoring landscape</a>



    *   The speaker of the session is the co-founder of Icinga monitoring system. I missed first ten minutes of the talk.-The talk is a comparison of all available OSS options for monitoring, visualization.
    *   Auto-discovery is hard.
    *   As per 2015 monitoring tool usage survey, Nagios is widely used.
    *   <a href="https://en.wikipedia.org/wiki/Nagios" target="_blank">Nagios</a> is reliable and stable.
    *   <a href="https://en.wikipedia.org/wiki/Icinga" target="_blank">Icinga 2</a> is a fork of Nagios, rewrite in c++. It’s modern, web 2.0 with APIs, extensions and multiple backends.
    *   <a href="https://github.com/sensu/sensu" target="_blank">Sensu</a> has limited features on OSS side and a lot of features on enterprise version. OSS version isn’t useful much.
    *   <a href="https://en.wikipedia.org/wiki/Zabbix" target="_blank">Zabbix</a> is full featured, out of box monitoring system written in C. It provides logging and graphing features. Scaling is hard since all writes are written to single Postgres DB.
    *   <a href="http://riemann.io/" target="_blank">Riemann</a> is stream processor and written in Clojure. The DSL stream processing language needs knowledge of Clojure. The system is stateless.
    *   <a href="https://www.opennms.org/en" target="_blank">OpenNMS</a> is a network monitoring tool written in Java and good for auto discovery. Using plugins for a non-Java environment is slow.
    *   <a href="http://graphiteapp.org/" target="_blank">Graphite</a> is flexible, a popular monitoring tool for time series database.
    *   <a href="https://github.com/prometheus/prometheus" target="_blank">Prometheus</a> is flexible rule-based alerting and time series database metrics.
    *   <a href="https://www.elastic.co/products/x-pack/monitoring" target="_blank">Elastic</a> comes with Elastic search, log stash, and kibana. It’s picking up a lot of traction. Elastic Stack is extensible using X-PACK feature.
    *   <a href="https://grafana.com/" target="_blank">Grafana</a> is best for visualizing time series database. Easy to get started and combine multiple backends. - - Grafana annotations easy to use and tag the events.
    *   There is no one tool which fits everyone’s case. You have to start somewhere. So pick up a monitoring tool, see if it works for you else try the next one til you settle down.



2.

    <a href="https://rootconf.talkfunnel.com/2017/17-deployment-strategies-with-kubernetes" target="_blank">Deployment strategies with Kubernetes</a>



    *   This was talk with a live demo.
    *   Canary deployment: Route a small amount of traffic to a new host to test functioning.
    *   If new hosts don’t act normal roll back the deployment.
    *   <a href="https://www.martinfowler.com/bliki/BlueGreenDeployment.html" target="_blank">Blue Green Deployment</a> is a procedure to minimize the downtime of the deployment. The idea is to have two set of machines with identical configuration but one with the latest code, rev 2 and other with rev 1. Once the machines with latest code act correctly, spin down the machines with rev 1 code.
    *   Then a demo of `` kubectl `` with adding a new host to the cluster and roll back.



3.

    <a href="https://rootconf.talkfunnel.com/2017/7-a-little-bot-for-big-cause" target="_blank">A little bot for big cause</a>



    *   The talk is on a story on developing, push to GitHub, merge and release. And shit hits the fan. Now, what to do?
    *   The problem is developer didn’t get the code reviewed.
    *   How can automation help here?
    *   Enforcing standard like I unreviewed merge is reverted using GitHub API, Slack Bot, Hubot.
    *   As soon as developer opens a PR, <a href="https://github.com/moengage/alice" target="_blank">alice</a>, the bot adds a comment to the PR with the checklist. When the code is merged, bot verifies the checklist, if items are unchecked, the bot reverts the merge.
    *   The bot can do more work. DM the bot in the slack to issue commands and bot can interact with Jenkins to roll back the deployed code.
    *   The bot can receive commands via slack personal message.



4.

    <a href="https://rootconf.talkfunnel.com/2017/18-necessary-tooling-and-monitoring-for-performance-c" target="_blank">Necessary tooling and monitoring for performance critical applications</a>



    *   The talk is about collecting metrics for German E-commerce company Otto.
    *   The company receives two orders/sec, million visitors per day.On an average, it takes eight clicks/pages to complete an order.
    *   Monitor database, response time, throughput, requests/second, and measure state of the system
    *   Metrics everywhere! We talk about metrics to decide and diagnose the problem.
    *   <a href="http://metrics-clojure.readthedocs.io/en/latest/" target="_blank">Metrics</a> is a Clojure library to measure and record the data to the external system.
    *   The library offers various features like Counter, gauges, meters, timers, histogram percentile.
    *   Rather than extracting data from the log file, measure details from the code and write to the data store.
    *   Third party libraries are available for visualization.
    *   The demo used d3.js application for annotation and visualization. In-house solution.
    *   While measuring the metrics, measure from all possible places and store separately. If the web application makes a call to the recommendation engine, collect the metrics from the web application and recommendation for a single task and push to the data store.



5.

    <a href="https://rootconf.talkfunnel.com/2017/51-what-should-be-pid-1-in-a-container" target="_blank">What should be PID 1 in a container?</a>



    *   In older version of Docker, Docker doesn’t reap child process correctly. As a result, for every request, docker spawns a new application and never terminated. This is called <a href="https://rootconf.talkfunnel.com/2017/51-what-should-be-pid-1-in-a-container" target="_blank">PID 1 zombie problem</a>.
    *   This will eat all available PIDs in the container.
    *   Use `` Sysctl-a | grep pid_max `` to find maximum available PIDs in the container.
    *   In the bare metal machine, PID 1 is `` systemd `` or any init program.
    *   If the first process in the container is bash, then is PID 1 zombie process doesn’t occur.
    *   Using bash is to handle all signal handlers is messy.
    *   Yelp came up with <a href="https://github.com/Yelp/dumb-init" target="_blank">Yelp/dumb-init</a>. Now, `` dumb-init `` is PID 1 and no more zombie processes.
    *   Docker-1.13, introduced the flag, `` --init ``.
    *   Another solution uses `` system `` as PID 1
    *   Docker allows running `` system `` without privilege mode.
    *   Running system as PID 1 has other useful features like managing logs.



6.

    <a href="https://rootconf.talkfunnel.com/2017/9-razor-sharp-provisioning-for-baremetal-servers" target="_blank">‘Razor’ sharp provision for bare metal servers</a>



    *   I attended only first half of the talk, fifteen minutes.
    *   When you buy physical rack space in a data server how will you install the OS? You’re in Bangalore and server is in Amsterdam.
    *   First OS installation on bare metal is hard.
    *   There comes Network boot!
    *   <a href="http://www.syslinux.org/wiki/index.php?title=PXELINUX" target="_blank">PXELinux</a> is a syslinux derivative to boot OS from NIC card.
    *   Once the machine comes up, DHCP request is broadcasted, and DHCP server responds.
    *   <a href="https://cobbler.github.io/" target="_blank">Cobbler</a> helps in managing all services running the network.
    *   DHCP server, TFTP server, and config are required to complete the installation.
    *   Microkernel in placed in TFTP server.
    *   <a href="https://puppet.com/blog/introducing-razor-a-next-generation-provisioning-solution" target="_blank">Razor</a> is a tool to automate provisioning bare metal installation.
    *   Razor philosophy, consume the hardware resource like the virtual resource.
    *   Razor components - Nodes, Tags, Repository, policy, Brokers, Hooks



7.

    <a href="https://rootconf.talkfunnel.com/2017/77-freebsd-is-not-a-linux-distribution" target="_blank">FreeBSD is not a Linux distribution</a>



    *   FreeBSD is a complete OS, not a distribution
    *   Who uses? NetFlix, WhatsApp, Yahoo!, NetApp and more
    *   Great tools, mature release model, excellent documentation, friendly license.
    *   Now a lot of forks NetBSD, FreeBSD, OpenBSD and few more
    *   Good file system. UFS, and ZFS. UFS high performance and reliable. - If you don’t want to lose data use ZFS!
    *   Jails - GNU/Linux copied this and called containers!
    *   No GCC only llvm/clang.
    *   FreeBSD is forefront in developing next generation tools.
    *   Pluggable TCP stacks - BBR, RACK, CUBIC, NewReno
    *   Firewalls - Ipfw , PF
    *   Dummynet - live network emulation tool
    *   FreeBSD can run Linux binaries in userspace. It maps GNU/Linux system call with FreeBSD.
    *   It can run on 256 cores machine.
    *   Hard Ware - <a href="https://en.wikipedia.org/wiki/Non-uniform_memory_access" target="_blank">NUMA</a>, ARM64, Secure boot/UEFI
    *   Politics - Democratically elected core team
    *   Join the Mailing list and send patches, you will get a commit bit.
    *   Excellent mentor program - GSoC copied our idea.
    *   FreeBSD uses SVN and Git revision control.
    *   Took a dig at GPLV2 and not a business friendly license.
    *   Read out BSD license on the stage.
