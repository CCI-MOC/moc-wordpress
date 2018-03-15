---
ID: 3172
post_title: 'D3N: A multi-layer cache for improving big-data applications performance in data centers with imbalanced networks'
author: Lucas
post_excerpt: ""
layout: page
permalink: https://massopen.cloud/d3n/
published: true
post_date: 2018-01-24 15:27:47
---
<strong>Overview &amp; Motivation</strong>

<span style="font-weight: 400">In today’s world, data is king. The success or failure of organizations can depend on the data they collect and the insights they can glean from it via Big Data analytics. As such, data lakes—low-cost object-storage repositories that can store vast volumes of data are becoming critical parts of organizations’ private datacenters . In large distributed organizations, centralized data lakes are often accessed by many compute clusters operated by different parts of the organization (e.g. business units within an enterprise). Even with a well-designed datacenter network, cluster-to-data lake bandwidth is typically much less than the bandwidth to storage within the compute clusters. Consequently, many users will manually copy a repeatedly accessed dataset to local (e.g. HDFS) storage, incurring complexity and performance overhead to manage data placement and replication, to maintain consistency between replicas and to copy data to local storage.</span>

<span style="font-weight: 400">D3N (datacenter-scale dataset delivery network) leverages insights drawn from CDNs by caching data on the access side of bandwidth bottlenecks (e.g., rack-to-rack and cluster-to-data lake), with CDN techniques used to direct I/O requests to the correct cache. D3N is designed to accelerate big data analytic workloads with strong locality and limited network connectivity between compute clusters and data storage.</span>

<strong>Architecture</strong>

<img class="alignnone wp-image-3223" src="https://massopen.cloud/wp-content/uploads/2018/01/D3n-300x142.png" alt="" width="408" height="193" />

<span style="font-weight: 400">D3N improves the performance of big-data jobs running in analysis clusters by speeding up reads to the data lake. The D3N architecture as shown in figure above consists of many components, the primary one being the cache. Cache servers are located in the datacenter on the access side of potential network bottlenecks, and organized into pools of different sizes, with cached data, in all but Level 1, distributed across these pools via consistent hashing. The resulting logical caches form a traditional caching hierarchy, where caches nearer the client have the lowest access latency and overhead, while caches in higher levels in the hierarchy are slower (requiring multiple hops to access), potentially larger (incorporating storage from more individual cache servers), and shared by more clients. The L1 cache server nearest to the client handles object requests by breaking them into blocks, returning any blocks which are cached locally, and forwarding missed requests to the block home location (as determined by consistent hashing) in the next layer. Cache misses are forwarded to successive logical caching layers until a miss at the top layer is resolved by a request to the data lake</span>

<b>Implementation</b>

<span style="font-weight: 400">We implement D3N for Ceph, a popular object store due to its scalability and performance. Although the architecture scales further, we implement only two layers of caching, corresponding to our current experimental environment of a data lake and a single multi-rack analysis cluster. D3N is implemented as modifications to the Ceph Object Store (also known as RADOS gateway or RGW), a Ceph component which interfaces with the native Ceph cluster protocol (RADOS) and provides Swift and S3 object APIs to clients. Both S3 and Swift protocols are supported by popular Big Data frameworks (e.g. Hadoop and Spark), via backend libraries which allow remote objects to be transparently accessed by analysis applications. </span>

&nbsp;

<span style="font-weight: 400">We modify RGW by adding c. 2500 lines of C++ code to implement D3N-RGW. As shown in Figure 3, two additional backends are added to RGW: local storage (SSD) for local cache access, and recursive RGW, which requests data from another RGW via S3 range requests. Client nodes send requests to their local first-level cache, which breaks the request into 4 MB blocks and handles each independently. Blocks are identified by their object ID and offset, and are cached (currently as individual files) in a local SSD-backed file system; if a block is present in cache then it is retrieved and returned directly to the client.</span>

<a href="http://www.bu.edu/rhcollab/projects/d3n/">D3N</a> is also a project of the <a href="http://www.bu.edu/rhcollab/">Red Hat Collaboratory at Boston University</a>.