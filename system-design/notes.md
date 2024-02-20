# System Design Primer Notes

## Scalability
Scalability can be achieved by horizontal or vertical scaling. Scaling helps to handle the increase in load.

* Vertical Scaling
    In vertical scaling the resources like vCPUs, RAMS, HDs are increased/scaled in a single server

* Horizontal Scaling
    In horizontal scaling the number of servers are increased. For example a website can be hosted in multiple server with a load balancer in front


Below are some of the scaling techniques,

* Load Balancer
* Caching
* Async Processing
* Clone of servers containing the application code
* Database Partition
* Database Replication
* Database Sharding


### Trade Offs

* Performance vs Scalablilty
    Performace is how the system performans for a single user
    Scalability is how the system performs under load ie when multiple users use it
* Availability vs Consistency
* Latency vs Throughput
    * Latency is the amount of time taken for packet to be transmitted
        * In same building/datacenter 1ms
        * In same continent 100ms
    * Throughput is the amount of data that can be sent per unit time. When running multiple instance of an app if it's tuned then TPS can be increased. Refer diagram,
        * TPS - 1r/s
        * TPS - 10r/s

![Alt text](image.png)

    * Bandwidth is how much data volume is sent in a unit of time
        * In 3G - 1Mb/s
        * In 4G - 10 Mb/s


### CAP Theorem


#### Resources
[System Design One for templates](https://github.com/systemdesign42/system-design/)

[Latency vs Throughput](https://www.youtube.com/playlist?list=PL9nWRykSBSFjU7UGR37SFfOb1oMYLNhag)
