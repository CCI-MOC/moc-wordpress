---
ID: 327
post_title: Storage
author: piyanai
post_excerpt: ""
layout: page
permalink: https://massopen.cloud/storage/
published: true
post_date: 2016-02-11 03:15:13
---
<h3>Storage Working Group Meeting</h3>
<h3>Present:</h3>
Peter Desnoyers, Mania Abdi, Mohammad Hajkazemi, Ravi Gudimetla (Northeastern)
Orran Krieger, Piyanai Saowarattitada (BU)
Larry Rudolph, Mark Astley, Henglin Yang, Roland Wunderlich, two others? (Two Sigma)
David Cohen (Intel)
Matt Benjamin (Red Hat)
<h3>Topics discussed included:</h3>
MOC storage strategy and directions: - Accelerating MOC services (e.g. Big Data caching, bare-metal imaging)
- Cloud Dataverse
- Cross-institution data hub (NESE and pending proposal)
- New techologies (high-density flash, NVMe over fabrics, NVM?)
<h4>Current research results</h4>
- student presentation on Ceph tiering and caching for big data, results to date and future plans. Note that we are currently targeting late September for paper submission and further development to reach production quality.
<h4>Discussion of partner data architectures</h4>
- a significant portion of the meeting was spent discussing storage architecture and issues in the Two Sigma non-Open Stack and Open Stack environments, One of the key pieces of information from this discussion was the observation that in-memory caching was highly important, where 3rd-party trace evaluation had led the MOC team to assume that it would play a small role at best. Additional work is needed to reconcile these results.
- Ceph instance per cluster, cluster starts at 3 racks. ~800TB raw storage per cluster
<h4>Future meetings</h4>
- a monthly 1-hour phone meeting will be established for ongoing discussion and collaboration. In addition we will probably try to set up a meeting at the MGHPCC sometime in the fall, mostly so that partners can get a chance for a tour of the data center.
- shared document area - this will probably be added to<a href="http://massopen.cloud/" target="_blank" rel="noreferrer">massopen.cloud</a>, and will be used for sharing documents for future meetings and collaboration. Note that any mechanism will need to be compatible with the Two Sigma firewall. (e.g. not Slack)

Other avenues for partnership: - student interns, although this is not likely until next summer
- storage / computation traces - this depends on the level of detail needed in the traces and the amount of effort which would be needed to anonymize traces sufficiently.
<h4>Various Notes:</h4>
<ul>
	<li>Issues identified / comments made re: big data caching:
<ul>
	<li>cache management</li>
	<li>client instrumentation - used heavily by Two Sigma for determining storage workload</li>
	<li>cache resizing</li>
	<li>need for scan resistance in eviction policy</li>
</ul>
</li>
	<li>Two Sigma Open Stack VM storage: QCOW2, iSCSI served off HDD/SSD, Ceph (boot images only?)</li>
	<li>install / upgrade storm problems - correlated traffic from large numbers of VMs</li>
	<li>large fraction of Two Sigma Open Stack is currently running elastic search</li>
	<li>Google is using iSCSI for containers - need to look this up</li>
	<li>Coho data paper on (client?) profiling</li>
</ul>