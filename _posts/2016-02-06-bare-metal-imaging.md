---
ID: 282
post_title: 'BMI: Bare Metal Imaging'
author: ravi
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/bare-metal-imaging/
published: true
post_date: 2016-02-06 22:00:00
---
&nbsp;

[toc]
<h3><strong><span style="text-decoration: underline">Overview</span></strong></h3>
The Bare Metal Imaging (BMI) is a core component of the Mass Open Cloud that (i) provisions numerous nodes as quickly as possible while preserving support for multitenancy using Hardware Isolation Layer (HIL) and (ii) introduces the image management techniques that are supported by virtual machines, with little to no impact on application performance.

<hr />

<h3><strong><span style="text-decoration: underline">Motivation</span></strong></h3>
Imagine thousands of nodes in a data center that supports a multitenant bare metal cloud. We need a central image management service that can quickly image the nodes to meet the customer’s needs. Upon completion of a customer’s project, the data center administrator should ideally be able to reallocate the resources within few minutes to use them for another customer. As of now, these techniques are in use for Virtual Machines (VMs), but not for bare metal systems. This project aims to bridge this gap by creating a service that can address the above mentioned issues.

Bare metal systems that support Infrastructure as a Service (IaaS) solutions are gaining traction in the cloud. Some of the advantages include:
<ul>
 	<li style="text-align: left">Best isolation with respect to containers or VMs.</li>
 	<li style="text-align: left">Predictable/stable performance when compared to VMs or containers, especially on input/output (I/O) intensive workloads such as Hadoop jobs, which need predictable storage and network I/O.</li>
 	<li style="text-align: left">Leveraging benefits of cloud services, such as economics of scale. As of now, VMs are scalable and elastic, as a customer pays for his/her usage based on resource consumption.</li>
 	<li style="text-align: left">The baremetal nodes could be used for other IaaS services like Openstack or applications like HPC which can consume ideal CPU cycles.</li>
</ul>
The main concerns of a bare metal system are the inherent slowness in provisioning the nodes and the lack of features, such as cloning, snapshotting, etc.

This project proposes a system that includes all of the above advantages and also addresses the fast provisioning issue for a bare metal system. Using this system, we propose to provision and release hundreds of nodes as quickly as possible with little impact on application performance.

<hr />

<h3><span style="text-decoration: underline"><strong>Current BMI (IMS) Architecture</strong></span></h3>
The current design consists of a pluggable architecture that exposes an API for such features as:
<ul>
 	<li>provision – Provisions a physical node with given image</li>
 	<li>snapshot - snapshots the given image, so that we can use this as golden image</li>
 	<li>rm – Removes the image from project</li>
 	<li>list – Lists the images available in project</li>
 	<li>upload – Uploads the image to library</li>
</ul>
<img class="alignnone wp-image-305" src="https://massopen.cloud/wp-content/uploads/2016/01/BMIFLOW-3.png" alt="BMIS Architecture" width="511" height="353" />

We use Ceph as a storage back-end to save OS images. For every application we support, we have a “golden image,” which acts as a source of truth. When a user logs-in and requests a big data environment, we clone from this golden image and provision nodes using the cloned image and a PXE bootloader. Hardware Isolation Layer (HIL) serves as a network isolation tool through which we achieve multitenancy. HIL provides a service for node allocation and deallocation. For more details about HIL, please visit <a href="https://xmail.bu.edu/owa/redir.aspx?SURL=1vkSii_YZnfcfbRJ0J4y42DKhLxXYA8ETo22D00NND64Xf0Pc0zTCGgAdAB0AHAAOgAvAC8AaQBuAGYAbwAuAG0AYQBzAHMAbwBwAGUAbgBjAGwAbwB1AGQALgBvAHIAZwAvAGIAbABvAGcALwAyADAAMQA2AC8AMAAyAC8AMAA2AC8AYgBhAHIAZQAtAG0AZQB0AGEAbAAtAGkAbQBhAGcAaQBuAGcALwBIAGEAYQBzAA..&amp;URL=http%3a%2f%2fmassopen.cloud%2fblog%2f2016%2f02%2f06%2fbare-metal-imaging%2fHaas">https://github.com/CCI-MOC/haas</a>.

<hr />

<h3><span style="text-decoration: underline"><strong>Project Team</strong></span></h3>
<strong><u>Professor(s)</u></strong>
<ul>
 	<li>Prof. Orran Krieger, Boston University <a href="mailto:okrieg@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Prof. Gene Cooperman, Northeastern University <a href="mailto:gene@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Prof. Peter Desnoyers, Northeastern University <a href="mailto:pjd@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li> 	
</ul>
<strong><u>Research Scientist(s), Postdoc(s) and Engineer(s)</u></strong>
<ul>
  <li>Dr. Ata Turk, Boston University <a href="mailto:ataturk@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Naved Ansari, MOC <a href="mailto:naved001@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<strong><u>Ph.D. Student(s)</u></strong>
<ul>
 <li>Apoorve Mohan, Northeastern University <a href="mailto:mohan.ap@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<strong><u>Undergraduate Student(s)</u></strong>
<ul>
 <li>Ian Ballou, Boston University <a href="mailto:iballou@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>

<strong><u>Past Contributors</u></strong>
<ul>
  <li>Dr. Jason Hennesey (Postdoc - now at NetApp Inc.) <a href="mailto:henn@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Nasibeh Teimouri (Ph.D. Student) <a href="mailto:teimouri.n@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Ugur Kaynar (Ph.D. Student) <a href="mailto:ukaynar@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Sahil Tikale (Ph.D. Student) <a href="mailto:tikale@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Ravi Santosh Gudimetla (Masters Student - now at Red Hat Inc.) <a href="mailto:ravig@ccs.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Sourabh Bollapragada (Masters Student - now at Arista Networks) <a href="mailto:sourabh.bollapragada@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Daniel Finn (Masters Student - now at Wayfair) <a href="mailto:djfinn14@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Pranay Surana (Masters Student - now at Lighthouse AI) <a href="mailto:surana.p@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
  <li>Sirushti Murugesan (Masters Student) <a href="mailto:murugesan.si@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>

<hr />

<h3><strong><span style="text-decoration: underline">Progress June'16-March'17</span></strong></h3>
<ul>
 	<li>Explore sensitivities of applications like OpenStack, HPC when using a network mounted system</li>
 	<li>Explore moving Operating systems from physical to virtual systems seamlessly.</li>
 	<li>Integrate support for attestation infrastructure for secure cloud.</li>
 	<li>A functional BMI on Engage1 cluster without modifications to existing infrastructure.</li>
 	<li>Ongoing support to Secure Cloud project.</li>
 	<li>Automated install setup for Redhat and other OS.</li>
 	<li>Performance evaluation for iSCSI server: IET vs TGT</li>
 	<li>Automated BMI install setup for ubuntu.</li>
 	<li>Multi-tenant iSCSI using TGT as backend.</li>
 	<li>Built a simple scheduler for dynamically moving the nodes across various clusters.</li>
 	<li>Performed experiments for improving the overall utilization of datacenter.</li>
 	<li>Built a proof of concept CI integration using Jenkins in Openshift.</li>
 	<li>Built OpenStack and HPC custom scripts for dynamically adding or removing nodes from a cluster.</li>
 	<li>Enhancements to code base e.g., Re-wrote exception, Database classes.</li>
 	<li>Poster accepted to SC’16 with initial findings on datacenter utilization improvements using BMI and HIL(close to 20% improvement with simple scheduler policies).</li>
</ul>

<hr />

<h3><strong><span style="text-decoration: underline">Timeline</span></strong></h3>
<ul>
 	<li>Performance analysis of BMI vs Ironic and other provisioning systems. - In progress.</li>
 	<li>Continuous Integration using Redhat Openshift. - In progress</li>
 	<li>Publish BMI paper - In Progress</li>
 	<li>Making BMI pluggable - to work with different Network Isolators iSCSI Servers and Storage Backends.</li>
 	<li>Testing/Deploying BMI in production MRI Cluster</li>
 	<li>Exploring iSCSI-Multipathing for load balacing and fault tolerance</li>
 	<li>Exploring security issues for provding a publically available provisioning service using BMI</li>
 	<li>Improving the overall UX(User Experience) by having a single interface that can talk to HIL and BMI.</li>
 	<li>Enhancements to the code base e.g., Complete unit tests for iSCSI, HIL etc - PR’s on the way.</li>
 	<li>Exploring other features of true virtualization on physical nodes like suspend and resume operations.</li>
 	<li>Exploring other scheduler framework policies to further improve datacenter utilizations.</li>
</ul>

<hr />

<h3><strong><span style="text-decoration: underline">Planning and Getting Involved</span></strong></h3>
To get involved in this project, please send email to (<a href="mailto:team@lists.massopen.cloud">MOC team-list</a>) and/or join the #moc irc channel on freenode.
<h3></h3>
&nbsp;