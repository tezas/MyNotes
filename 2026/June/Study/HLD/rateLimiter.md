# RATE LIMITER

1. Define service TPS while onboarding client.
2. Do we need to enforce limit on client level? => YES

## Functional Requirement
1. service which allows/rejects every incoming request on the basis of requests allowed per second for a service.

## NFR
1. Scale of requests: 500K requests/sec
2. consistency: eventual consistent
3. latency: Lowest latency possible.


## Steps
1. checks TPS for a client for an API. [Config]
2. checks the [t - x, t] requests count. [OPEN]
    2.1. How to identify how much quota is left? => O[1]
    2.2. How to identify to evaluate the requests count in last x miliseconds.
3. How to avoid point of contention? [For multiple requests, different instances exposed with load balancer, each instances have threads defined based on load], Instances can scale as per the requests. 
   In this case I would use, ECS fargate so that I keep options open for sticky sessions.
4. If I write the data to DB [Time consuming]. Can we use cache? can we use sticky session?
    a. sticky sessions might create contention points as 1 session can be bombarded with all requests. But if possible then we can use in memory cache to write aggregate requests till time t.
    b. We can use distributed cache like redis [in memory dB]. This decision corelates or creates dependency of our rate limiter on redis. [Redis TPS - x] would and  our rate limit.
5. Best optimisation, as we are eventual consistent: every limiter instance writes data in memory and async in shared cache.


## Question:
1. What are the issues with sticky seccions? => a. It can create contention points. b. 
