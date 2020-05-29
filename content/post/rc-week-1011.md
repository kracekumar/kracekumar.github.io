
+++
date = "2016-12-19 08:21:35+00:00"
draft = false
tags = ["recursecenter"]
title = "RC Week 1011"
url = "/post/154669792910/rc-week-1011"
+++
### EDHT

I continued to work on the project and added the few features.

*   Data replication - One of the main features of DHT is replicate the data across a subset of nodes in the cluster. Remember not all nodes! Depending on the number of copies to store, say N, the data is stored in `` N - 1 `` nodes starting from the primary node in the anti-clockwise direction.
*   Routing - Every node in the cluster has equal responsibility, and there is no master. For `` key metamorphosis `` the data is stored in the nodes `` n1, n2, n3 `` as per consistent hashing. The node `` n4 `` receives the `` GET `` request for the `` key metamorphosis ``. Node `` n4 `` doesn’t have the data and acts as coordinator. The coordinator forwards the requests to any one of the nodes, `` n1, n2, n3 ``. Depending on the minimum number of successful response configuration, the receiver requests other nodes, collates the response and sends the response back to the coordinator.

I read multiple articles about Vector clocks - <a href="http://basho.com/posts/technical/why-vector-clocks-are-easy/" target="_blank">vector clocks are easy</a>, <a href="http://basho.com/posts/technical/why-vector-clocks-are-hard/" target="_blank">vector clocks are hard</a>, <a href="http://www.datastax.com/dev/blog/why-cassandra-doesnt-need-vector-clocks" target="_blank">why Cassandra doesn’t need vector clocks</a>. The vector clock is the next feature to implement in the project. The vector clock’s primary usage is conflict resolution. The DynamoDB paper suggests offloading the conflict resolution to the client.

### Good luck

Thursday was cold in New York, -4-degree Celsius and end of the batch party. The evening was an emotional roller coaster. In last twelve weeks, I picked up different perspective in programming and thinking; multiple thoughtful conversations; developed interests in particular streams; personal discovery, made friends and more. Thanks for all the batchmates and alum for making the stay memorable, educational, caring and helping in my development. I learned from every one of you through personal conversations, presentations, and observations. Thanks for all the RC staff without whom the batch wouldn’t be possible. Of course, I feel grateful for the US Embassy in Chennai for granting VISA on my second attempt - the First-time officer rejected the VISA request stating I may not return to India after the short overseas visit.

Good luck to everyone’s future!
