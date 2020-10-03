##### Where are environment variables stored for cloud
For cloud each cloud provider provides place to store environment variables. 

[Environment Variables](https://www.twilio.com/blog/2017/01/how-to-set-environment-variables.html)


##### When developing on your local 
Store these in local. e.g. PATH, User Variable etc. 

##### When developing on docker
In docker these are created in dockerfile using 
```
    ENV <key> <value>
    ENV <key>=<value> ...
    ENV abc=hello
    ENV abc=bye def=$abc
    ENV ghi=$abc
``` 

![Bamboo Env Variables](/images/Environment-Variables-Bamboo-Plan.png)

#### Killing service on a port
lsof -i :3000
kill -9 <PID>


[consul article on medium](https://medium.com/velotio-perspectives/a-practical-guide-to-hashicorp-consul-part-1-5ee778a7fcf4)


#### Quick Summary from Above Article
##### Service Discovery and Overcoming challenges of Load Balancers
As systems are moving to microservice architecture , services become independent and they could be running in any container having different endpoint. Service discovery tools such as consul help in finding service before making a call. 

Typically this is solved via **Load Balancer**. Load balancers would sit in front of each service with a static IP known to all other services. This load balancer IP is static and hard-coded within all other services, so services can skip discovery. Load balancers are managed **manually** and they have a **single point of failure**. Load balancers also increase **latency**. 

Consul programmatically manages registry, which gets updated when any new service registers itself and becomes available for receiving traffic.

#### Challenges of managing configurations in distributed systems
Consul’s solution for configuration management in a distributed environment is the central Key-Value store.

#### Networking
The **first zone** in our network is publicly accessible. The traffic coming to our application via the internet and reaching our load balancers.
The **second zone** is the traffic from our load balancers to our application. Mostly an internal network zone without direct public access.
The **third zone** is the closed network zone, primarily designated for data. This is considered to be an isolated zone.

*in distributed systems traffic is not in any sequential flow*

**Consul's Solution:** 
Consul Connect enrolls these policies of inter-service communication that we desire and implements it as part of the service graph. So, a policy might say service A can talk to service B, but B cannot talk to C, for example.

Read about similarity and differences between Consul and Zookeeper, doozerd, or etcd..
