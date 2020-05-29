+++
date = "2017-05-13 15:51:29+00:00"
draft = false
tags = ["devops", "conference", "notes"]
title = "Notes from Root Conf Day 2 - 2017"
url = "/post/160622791375/notes-from-root-conf-day-2-2017"
+++
On day 2, I spent a considerable amount of time networking and attend only four sessions.

<a href="https://rootconf.talkfunnel.com/2017/72-spotswap-running-production-apis-on-spot-instances" target="_blank">Spotswap: running production APIs on Spot instance</a>

*   Amazon EC2 spot instances are cheaper than on-demand server costs. Spot instances run when the bid price is greater than market/spot instance price.
*   Mapbox API server uses spot instances which are part of auto-scaling server
*   Auto scaling group is configured with min, desired, max parameters.
*   Latency should be low and cost effective
*   EC2 has three types of instances: On demand, reserved and spot. The spot instance comes from unused space and unstable pricing.
*   Spot market starts with bid price and market price.
*   In winter 2015 traffic increased and price also increased increased
*   To spin up a new machine with code takes almost two minutes
*   Our machine fleet encompasses of spot and on-demand instances
*   When one spot machine from the fleet goes down, and auto scaling group spins up an on-demand machine.
*   Race condition: several instances go down at same time.
*   Aggressive spin up in on-demand machines when market is volatile.
*   Tag EC2 machines going down and then spin up AWS lambda.When spot instance returns shit down a lambda or on-demand instance. Auto Scaling group can take care of this.
*   Savings 50% to 80%
*   Source code: <a href="https://github.com/mapbox/spotswap" target="_blank">https://github.com/mapbox/spotswap</a>
*   No latency because over-provisioned
*   Set bid price as on-demand price.
*   Didn’t try to increase spot instance before going on-demand
*   Cfconfig to deploy and Cloud formation template from AWS

<a href="https://rootconf.talkfunnel.com/2017/1-adventures-in-postgres-management" target="_blank">Adventures with Postgres</a>

*   Speaker: I’m an Accidental DBA
*   The talk is a story of a Postgres debugging.
*   Our services include Real-time monitoring, on demand business reporting to e-commerce players. 4000 stores and 10 million events per day. Thousands of customers in a single database.
*   Postgres 9.4, M4.xlarge,16GB, 750 GB disk space with Extensive monitoring
*   Reads don’t block writes, Multi-Version Concurrency Model.
*   Two Clients A, B read X value as 3. When B updates the value X to 4, A reads the X value and gets back as 3. A reads the X value as 4 when B’s transaction succeeds.
*   Every transaction has a unique ID - XID.
*   XID - 32 bit, max transaction id is 4 billion.
*   After 2 billion no transaction happens.
*   All writes stop and server shutdown. Restarts in single user mode,
*   Read replicas work without any issue.
*   Our server reached 1 billion ids. 600k transaction per hour, so in 40 days transaction id will hit the maximum limit.
*   How to prevent?
*   Promote standby to master? But XID is also replicated.
*   Estimate the damage - txid\_current - Current Transaction ID
*   Every insert and update is wrapped inside a transaction
*   Now add begin and commit for a group of statements, this bought some time.
*   With current rate, 60 days is left to hit max transaction limit.
*   TOAST - The Oversized Attribute Storage Technique
*   Aggressive maintenance. Config tweaks: autovacuum\_workers, maintenance\_work\_mem, autovaccum\_nap\_time - knife to gun fight. Didn’t help
*   `` rds_superuser `` prevented from modifying pg system tables
*   Never thought about `` rds_superuser `` can be an issue.
*   VACUUM – garbage-collect and optionally analyze a database
*   vacuum freeze (\*) worked. Yay!
*   What may have caused issues - DB had a large number of tables. Thousands of tables
*   Better `` shard `` per customer
*   Understand the `` schema `` better
*   Configuration tweaks - max\_workers, nap\_time, cost\_limit, maintenance\_work\_mem
*   Keep an eye out XID; Long-lived transactions are problem
*   Parallel vacuum introduced in 9.5
*   `` pg_visibility `` improvements in 9.6
*   Similar problem faced other companies like GetSentry

<a href="https://rootconf.talkfunnel.com/2017/60-mysql-troubleshooting-tldr" target="_blank">MySQL troubleshooting</a>

*   Step 1 - Define the problem, know what is normal, read the manual
*   Step 2: collect diagnostics data (OS, MySQL). `` pt_stalk `` tool to collect diagnostics error
*   Lookup MySQL error log when DB misbehaves.
*   Check OOM killer
*   General performance issues - show global variables, show global status, show indexes, profile the query
*   Table corruption InnoDB, system can’t startup. Worst strategy force recovery and start from backup.
*   Log message for table corruption is marked as crashed
*   Replication issues - show master status, my.cnf/my.ini, show global variables, show slave status

OTR Session - Micro Service

*  OTR - Off The Record session is a group discussion. Few folks come together and moderate the session. Ramya, Venkat, Ankit and Anand C where key in answering and moderating the session.
*  What is service and micro service? Micro is independent, self-contained and owned by the single team. Growing code base is unmanageable, and the number of deploys increases. So break them at small scale. Ease of coupling with other teams. No clear boundary
*   Advantages of Microservices - team size, easy to understand, scale it. Security aspects. Two pizza team, eight-member team. Able to pick up right tools for the job, and change the data store to experiment, fix perf issues.
*   How to verify your app needs micro service?
*   Functional boundary, behavior which is clear. Check out and Delivery
*   PDF/Document parsing is a good candidate for Micro Service, and parsing is CPU intensive. Don’t create nano-service :-)
*   Failure is inevitable. Have logic for handling failures on another service. Say when MS 1 fails MS2 code base should handle gracefully.
*   Message queue Vs Simple REST service architecture. Sync Vs Async.The choice depends on the needs and functionality.
*   Service discovery? Service registry and discover from them.
*   Use swagger for API
*   Overwhelming tooling - you can start simple and add as per requirements
*   Good have to think from beginnings - how you deploy, build pipelines.
*   Auth for internal services - internal auth say Service level auth and user token for certain services. Convert monolithic to modular and then micro level.
*   API gateway to maintain different versions and rate limitingWhen to use role-based access and where does scope originate? Hard and no correct way. Experiment with one and move on.
*   Debugging in monolithic and micro service is different.
*   When you use vendor-specific software use mock service to test them. Also, use someone else micro service. Integration test for microservices are hard.
*   Use continuous delivery and don’t make large number of service deployment in one release.
*   The discussion went on far for 2 hours! I moved out after an hour. Very exhaustive discussion on the topic.
