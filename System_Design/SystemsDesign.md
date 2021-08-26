# BASICS:
* How to Fulfil the increased demand and requirements.
1. Optimize the processes and inreaes throughput using only single resouce. | VERTICAL SCALING
2. Optimizing processes : Preprocessing : Setting CRON jobs at non peak times.

* How to avoid single point of failure
3. Keep backup resources. | HORIZONTAL SCALING

* How to divide the responsibilities:
4. Divide the responsibilities amongst the different resources independent of each other.

* What if the whole backup and all resources crash/breakdown.
5. Distribute the responsibility to a DIFFERENT location. In most of the cases, there are servers(resource) every where at different 
locations. So that they can serve to the closest requests.

* Now when we have multiple data centers and server locations. We need a decision making system, to provide the fastest response.
6. Use load balancer.

* Now the response provider is independent of backend system functionality. Hence Different systems are independent of each otehr.
7. Decoupling the system: Separating out the independent systems inorder to easily maintain them.

* If the same part of the system needs to be used, we wouldn't like to copy that again, rather we will reuse it.
8. Extensibiltiy: Reusing the existing system.



