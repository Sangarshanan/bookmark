# RUM Conjecture

### Design Tradeoffs of Data Access Methods

http://daslab.seas.harvard.edu/rum-conjecture/

Paper: https://scholar.harvard.edu/files/stratos/files/rum-tutorial.pdf?m=1461167186#:~:text=2.,time%2C%20for%20all%20three%20goals.


An access method is a collection of algorithms and data structures for organizing and accessing data. The way we physically organize data on storage devices defines and restricts the possible ways that we can read and update it.

Every access method tries to optimise for these three design parameters

- the read overhead
- the update overhead
- memory/ storage overhead

These three RUM overheads form a design space with a three-way balance: optimizing for any
two overheads negatively impacts the third

In order to understand the design space we need to understand the different design elements used in access methods.

### Logarithmic Design

In this data access method the key idea is to organize metadata in a way that the search cost of a specific element is logarithmic to the size of the dataset

Examples here include B-Trees, Tries and so on

**Reads** Most log structures offer fast read access but increase space overhead. especially tries which is why Adaptive Radix Tree tries to offer space-efficient trie indexing by adapting each node size based on the density of the domain for the corresponding values https://db.in.tum.de/~leis/papers/ART.pdf

**Updates** For updates log access methods typically follow a hierarchical log-structured approach. When the top level receives enough updates it is merged with lower levels and these updates gradually propagate in the tree. sequential writes to disk is factor and we can run compaction in intervals but there is a read overhead here when someone queries a key that was updated a long time ago and is not in the index, we can use bloon filters to avoid lookups of keys not present in the table but end of the day for present key read is still expensive

**Memory** most of the approaches here just leverage knowledge about the memory hierarchy, we want to reduce the number of cache misses and avoid random disk access


### Continuous Reorganization

This approach is taken by query-driven adaptive indexing and by differential updates. While the goal of the adaptive indexing is classically optimizing for read performance, differential updates aims at offering efficient updates.

**Read** Uses Adaptive indexes eg: Database Cracking which reorganizes the data in memory to match how one queries access data. But these data structures are typically only designed for a particular type of hardware and application.

https://db.in.tum.de/teaching/ws1718/seminarHauptspeicherdbs/paper/werner.pdf?lang=de#:~:text=Database%20cracking%20is%20an%20approach,in%20a%20self%2Dorganized%20way.

**Updates** Differential files stores only differential updates instead of in-place updates. The fundamental idea is to consolidate updates and apply them in bulk.

### Space-Efficient Designs

**Reads** eg: Hash Indexing is is a typical example of an access method that has small size because it has update and size overhead, For large datasets there is also bitmap indexing which saves on size at the expense of additional metadata and a small penalty in read performance


**Memory** there are two strategies here: data skipping and approximate indexing.
data skipping is a form of scan enhancement where we store metadata for zones of a column, such as min/max information, and a scan uses that metadata to decide whether to scan a zone or not. Such approaches work quite well when data is clustered or fully sorted. Approximate indexing includes the likes of probabilistic data structures like bloom filters

So we took a look at design elements and tradeoffs of access methods in one-dimensional, keyvalue or relational data. However, there is a wealth of use cases that face similar tradeoffs and design decisions and require different typically specialized solutions.

### High-Dimensional Access Methods

Spatial and other high-dimensional data require access methods that can navigate efficiently a high-dimensional space.

eg: R-Tree https://iq.opengenus.org/r-tree/

As the number of dimensions increases the efficiency of an R-Tree is dramatically reduced facing
the **curse of dimensionality**

- To reduce the  dimensional, we can break the space into small MBRs in order to have less overlap but this leads to more metadata needed for the indexing structure: updating the index is more expensive
- If we have higher overlap between MBRs: Reads are more expensive


### Time-Series Access Methods

Time series data can be represented through segments means which allowed for space-efficient representation at the expense of accuracy. The updates in time-series workloads are significantly different than in relational systems. Typically, the aforementioned approaches treat updates that are in effect new time-series in a large collection of time series leaving old data unaffected and requiring append-only style of updates. So we optimize for read performance at the
expense of both index size and update overhead.

### Graph Access Methods

Here the graph representation plays a big role in performance and each representation may require
a very different way to access the data; also, there has been a much smaller effort in standardization when compared with relational systems

A space efficient graph representation is the compressed spared row (CSR) representation which typically offers immutable data, a key difference from relational data


### End of the line

When designing access methods we set an upper bound for two of the RUM overheads, this implies a hard lower bound for the third overhead which cannot be further reduced

> **The application, the workload, and the hardware should dictate how we access our data, and not the constraints of our systems.**


Tuning access methods becomes increasingly important if, on top of big data and hardware, we consider the development of specialized systems and tools to cater data, aiming at servicing a narrow set of applications each. As more systems are built, the complexity of finding the right access method increases as well. we can also design access methods with dynamic and tunable RUM behavior.


