## Day 1

Source: https://interviewing.io/guides/system-design-interview
* Interviewers seeks holistic view of users and system. In case of ambiguities always prioritise users. Technical details are important but they are to serve humans.
* Read about multiple red and green flags during the interviews.
  * RED: Don't talk just for the sake for talking. That doesn't help at all and drains the interest of interviewer and your energy.
  * GREEN: If there is anything that you don't know or you feel stuck then fallback to first principles and try to solve it from there.
  * RED: If you find yourself pushing against the interviewer feedback. They might be giving some hints or feedbacks that you must listen.
  * GREEN: Interview must seem like a collaboration between the 2 teammates.
  * RED: If you skip over questions asked or a prompt by interviewer, you tend to miss the opportunity to give the interviewer a data point.
  * GREEN: As a senior engineer, you must be driving the focus and place of the interview. For a senior engineer, there shouldn't be multiple awkward silences.
  * RED: There cannot be multiple long silent stretches in an interview. Its always better to ask for help if you find yourself stuck rather than remaining silent.
  * GREEN: You tend to collect your thought before show-casing them.
  * GREEN: You don't just provide options but take decision to go with an option with an informed decision.

## Day 2
System design can be broken down in these following steps:
* **Problem Navigation**: Here you can show case the skill of breaking down the problem in smaller pieces and navigate through the pieces gracefully. Initially the problem is under specified and getting the clarity and creating a roadmap for the problem is important.
*  **Solution Desigfn**: Here we go through each smaller piece and try to solve them with Core Concepts of technology. You should be able to explain how you're solving these smaller pieves.
*  **Technical Excellence**: You must be aware of the new age technologies which apply to the solution. We can always leverage the benfit of the new tech. We cannot keep using the old tech here and need to keep up with the trends. You should be able to explain the standard patterns and inspirations taken from existing technology or reusing them.
*  **Communication**: This is a very important skill for a staff level engineer. It should feel like a collaboration then an interview. You need to identify the battles wisely. There would be some concepts that you can avoid go in deep as they are straightforward and discuss more about the more complex problems. 

## Day 3
#### Caching
Types of caching:
* **Client side**: browser caches the data. Session cookies
* **CDN Caching**: Static data and front end code.
* **Web Server**: Load balancer/API Gateway caches the data and instead of reaching application server returns the requirtaed response based on requests. This is also called reverse proxy which provides security to the application server as well.
* **Database caching**: Database often provides cached data for frequent requests.
* **Application server**: This is used to utilise in memory database. Since RAM is rare and doesn't have unlimited data we need to think of eviction policies here. Cache is used for different purpose:
  * row level data
  * Query level data
  * serialised object
  * Rendered HTML

 ##### Caches Updation/Eviction policy
 Cache has limited memory and its more expensive compared to databases, hence we need to think about how we can update and evict the data in cache.
 Following are the different techniques to update and evict data from the cache:
 1. Cache Aside / Lazy Loading -> If there is a cache miss then application fetches from DB and update cache.
 2. Write through: First write to cache and then to DB. This happens synchronously. Write becomes slower but reads are faster.
 3. Write Behind: Write to CACHE syncronously and fork async write for DB.
 
#### Load Balancer
There are 2 load balancers:
1. At Layer 4, which takes care of routing the data at transport layer and only has the information about destination and source. Also called as Network load balancer. Its not aware of the data packets being transmitted. [Generally, this involves the source, destination IP addresses, and ports in the header, but not the contents of the packet]
2. At later 7, This is application load balancer and takes the routing decision on the basis of which worker is busy and what data is being transmitted. They terminate network traffic.

Routing techniques
* Random
* Round robin
* Sticky session based on cookies
* Least busy worker

Load balancer also supports
* session storage
* Health checks of servers
* Descrypting SSL / SSL Termination
* Helps eliminating overloading a specific server [Consistent hashing]

#### Consistent Hashing
Lets state the problem that we want to solve with this approach:
When adding or removing nodes in a distributed cluster of servers we need shuffle the data on the basis of partition key distribution across the nodes. Consistent hashing reduces the percentage of data being shuffled from (n - 1) / n to ~1/n whle ensuring no server gets overloaded.

* If we use hash key with modulo (i.e., hash(pk) % (number of nodes)) then on addition or removal of nodes the outcome of hash also changes. With this technique it impacts 90% of data shuffling.
* To solve this, we consider nodes to be distributed at a ring where hash(node) creates a position in a ring and then data with pk also assigned a hash on a ring which. Then the data is being allocated to nearest clockwise neighbouring node. With this approach any addition/removal impacts a small sector in a ring to shift from one neighbor to another.
* But the above approach can lead to overloading specific node. For this problem, virtual nodes are being created and assigned multiple positions on ring. With this data is always uniformly distributed across different nodes.
* Node discovery generally happens using gossip protocol where nodes communicate with other nodes about the state understanding that they have about the cluster and then those node gossip with others.

## Day 4 
### CAP Theorem
In CAP, Consistency, Avalability and Partition Tolerance. Partition Tolerance is not a choice in a real distributed system. There can be unseen states where network break can occur between the 2 nodes.
So CAP theorem answers the dilemma in case of Partition between C & A and states that a real distributed system can only achieve either CP or AP state.

The complete flow is PACELC:
In case of Partition choose either A or C. Else choose L(Latency) or C(Consistency).

Different systems offer different setting:
DynamoDB/Cassandara: AP/L
SQL/RDS: CP/C
MongoDB: CP/C

Sometimes its not purely consistency vs Latency. Like Cassandra provides operarion level consistency based on W + R > N where N is no. of nodes, w is numbers of nodes acknowledging before commiting the write and R means reads from number of nodes.  This is also called Quorom. 

  - W=1, R=1  →  AP / low-latency  (stale reads OK — like a like-counter)
  - W=2, R=2  →  CP / strong       (read-your-writes — like inventory)
  - W=3, R=1  →  CP, fast reads, slow/fragile writes 
  - W=1, R=3  →  CP, fast writes, slow/fragile reads

### SQL vs NO-SQL
- SQL only supports structured data. It can enforce constraints on data which helps in data integrity at each field level. 
- NO-SQL supports un-structures data. This can be helpful when we are not aware of the data format or want to keep flexibility on fields. Each document can follow different fields.

- SQL supports ACID properties on every operation. They achieve atomicity using transactions.

- SQL can support complex adhoc queries spanning across multiple tables.
- NO-SQL is used for known access patterns.

Decision Points:
* Main driving decision between SQL / NO-SQL is the access pattern. If we know the access pattern then we can utilise NoSQL. Although there can be a case where we might need polyglot persistence layer where different types of data is being stored in different databases.
* We can also think about if we need atomicity between multiple types of data. DynamoDB support transactions but it has its own limitations. We have to take care of concurrency and race conditions.
* SQL buys you, The ability to enforce correctness invariants across multiple records, under concurrent access, automatically — via multi-record atomicity + serializable isolation + declarative constraints.
