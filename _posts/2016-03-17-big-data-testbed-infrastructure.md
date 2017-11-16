---
ID: 610
post_title: Engage1 Infrastructure
author: kamfonik
post_excerpt: ""
layout: post
permalink: >
  https://massopen.cloud/blog/big-data-testbed-infrastructure/
published: true
post_date: 2016-03-17 16:16:54
---
&nbsp;

[toc]
<h3><span style="text-decoration: underline;"><strong>Overview</strong></span></h3>
The Big Data Testbed is a large, collaborative cluster which serves as a test site for a wide variety of innovative high performance computing technologies. It is located at the <a href="http://www.mghpcc.org/">Massachusetts Green High Performance Computing Center </a>(MGHPCC).

MOC's industry partners in the Big Data project include Brocade, Intel, Lenovo and Red Hat. The MIT Research Computing
<a id="core-team"></a>

<hr />

<h3><span style="text-decoration: underline;"><strong>Architecture</strong></span></h3>
The Big Data Testbed consists of 18 compute racks, one storage rack, and an experimental OpenStack rack.  MIT provides compute servers for the cluster, which are housed in MIT racks.  At scheduled times, MIT nodes are made available for provisioning via the MOC network, and then are moved back to the MIT compute pool when the time is up.  We are currently exploring rapid node deployment using <a href="http://massopen.cloud/blog/bare-metal-imaging/">Bare Metal Imaging (BMI)</a>. Future plans include moving nodes to between MIT and MOC more dynamically via requests to MIT's provisioning system.

Each compute rack will eventually feature an Intel cache server with two high performance SSDs, configured as cache tier of the cluster's Ceph storage backend. Currently 5 such servers are actively deployed.

The cluster is backed by Red Hat Ceph Storage.  The base Ceph tier, containing the majority of the storage capacity, is installed on ten Lenovo x3650 servers.  Each server is equipped with nine 4TB SATA drives, plus three faster SSDs for journaling.  The replicated nature of Ceph (three replication) means that the total working capacity of the base storage tier is about 103TB.  Three small Quanta servers serve as monitors for the cluster.

The cluster network is a special 'bifurcated ring' architecture designed for this project by engineers at Brocade.  The ring is designed to create short paths between any two points, yet still be expandable with minimum disruption to the existing infrastructure.  Currently, we have 22 Brocade VDX6740 switches in the cluster - one for each of the 18 compute racks, one in the OpenStack rack, and three in the storage rack.  The 10 servers are split among the 3 switches, so that the storage backend is accessible from 3 different points in the wider 'ring' architecture.

The last piece of the cluster is an MOC compute rack with 7 Quanta servers and 6 Lenovo System x3550 servers. This rack features a small OpenStack Liberty installation, an experimental OpenShift environment, and a multinode setup used for SDN research and an <a href="http://massopen.cloud/blog/openflow/">OpenFlow research project</a>. This rack also houses the <a href="http://massopen.cloud/blog/project-hil/">HIL master</a> which controls networking for the Engage1 deployment.
<a id="diagrams"></a>

<hr />

<h3><span style="text-decoration: underline;"><strong>Diagrams</strong></span></h3>
<h3>Big Data Testbed Infrastructure Diagram</h3>
<img class="alignnone size-full wp-image-1412" src="http://massopen.cloud/wp-content/uploads/2016/03/Engage1-1.png" alt="Engage1" width="720" height="540" />
<h3>RGW-Proxy Overview</h3>
<img class="alignnone size-full wp-image-1420" src="http://massopen.cloud/wp-content/uploads/2016/03/RGWsSSDcacheInMOC-1.png" alt="RGWsSSDcacheInMOC" width="1500" height="1125" />

<hr />

<h3 style="color: #ff0000;"><span style="text-decoration: underline;"><strong><span style="color: #000000; text-decoration: underline;">Project Team</span></strong></span></h3>
<p style="color: #ff0000;"><span style="text-decoration: underline;"><strong><span style="color: #000000; text-decoration: underline;">Core Project Team</span></strong></span></p>

<ul>
 	<li>Chris Hill, EAPS Principal Research Scientist; Earth, Atmospheric, and Planetary Sciences (MIT) <a href="mailto:cnh@mit.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Radoslav Milanov, Senior Infrastructure Engineer (Boston University)</li>
 	<li>Laura Kamfonik, Junior Infrastructure Engineer (Boston University) <a href="mailto:kamfonik@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Paul Hsi (MIT) <a href="mailto:hsi.mghpcc@gmail.com">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Rahul Sharma, Co-op Intern (Boston University) <a href="mailto:sharma.rah@husky.neu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Piyanai Saowarattitada, MOC Director of Engineering (Boston University) <a href="mailto:piyanai@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>
<p style="color: #ff0000;"><strong><span style="text-decoration: underline;"><span style="color: #000000; text-decoration: underline;">Contributors</span></span></strong></p>

<ul>
 	<li>Jon Bell (Boston University) <a href="mailto:jbell@bu.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Dave Cohen (Intel)</li>
 	<li>Rob Montgomery (Brocade)</li>
 	<li>Mark Presti (Brocade)</li>
 	<li>Bob Newton (Lenovo)</li>
 	<li>Jon Proulx (MIT - CSAIL) <a href="mailto:jon@csail.mit.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Garrett Wollman (MIT - CSAIL) <a href="mailto:wollman@csail.mit.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
 	<li>Tyler Brekke (Red Hat Enterprise Linux)</li>
 	<li>Huitae Kim (Red Hat Enterprise Linux)</li>
 	<li>Joe Fontecchio (University of Massachusetts) <a href="mailto:jfontecchio@umassp.edu">[icon name="envelope-o" class="" unprefixed_class=""]</a></li>
</ul>

<hr />

<h3><span style="text-decoration: underline;"><strong>Timeline</strong></span></h3>
<ul>
 	<li><span style="color: #0000ff;">July 2015</span>
<ul>
 	<li>Engineers from Lenovo install the 10 storage servers at MGHPCC.</li>
 	<li>Extensive planning of the Brocade fabric is begun.</li>
</ul>
</li>
 	<li><span style="color: #0000ff;">September 2015 </span>- The 22 VDX6740 Switches are configured by Rob M. (Brocade), and later installed in the racks.<span style="color: #0000ff;">
</span></li>
 	<li><span style="color: #0000ff;">October 2015</span> - The Brocade Fabric is installed, with over 100 cables connecting switches across two datacenter pods.</li>
 	<li><span style="color: #0000ff;">November 2015</span> - 10 small Quanta servers are installed, which later become admin/service nodes, Ceph monitors, and OpenStack nodes.</li>
 	<li><span style="color: #0000ff;">December 2015</span> - Brocade conducts two training sessions at BU.</li>
 	<li><span style="color: #0000ff;">January 2016</span> - Red Hat conducts a weeklong Ceph training at BU.  Tyler B. and  H. Kim (Red Hat) are on-site to help with the Ceph installation.</li>
 	<li><span style="color: #0000ff;"><span style="color: #0000ff;">February 2016 </span></span>- An initial set of Intel Cache servers are configured, and an experimental radosgw-proxy service is set up.  This service will be tested on the cluster with Hadoop in the near future.</li>
 	<li><span style="color: #0000ff;"><span style="color: #0000ff;">March 2016 </span></span>- Deploying a handful physical systems for Hadoop Bare metal deployment POC.  Exploring the possibility of being part of the RH Ceph High Touch Beta program for early access/support for the upstream Infernalist release.</li>
 	<li><span style="color: #0000ff;"><span style="color: #0000ff;">April 2016 </span></span>- Hadoop Bare metal deployment against additional hardware loaners from Lenovo as well as against 200+ MIT MRI systems. Evaluate Infernalist before deployment for a possible deployment...</li>
 	<li><span style="color: #0000ff;"><span style="color: #0000ff;">May 2016 </span></span>-  Depending on the road map of the Big Data user cases, deploy Infernalist.</li>
 	<li><span style="color: #0000ff;"><span style="color: #0000ff;">Summer 2016 </span></span>- Big Data  and HPC use case support. Testing of BMI deployment.</li>
 	<li><span style="color: #0000ff;">Fall 2016 </span>- Deploy Anycast setup for cache tiering experiments in the Brocade environment</li>
 	<li><span style="color: #0000ff;">Winter 2017</span> Upgrade to Mitaka</li>
 	<li><span style="color: #0000ff;">Spring 2017</span> Small staging area with 1 cache server and 3 compute nodes added to MOC rack.</li>
 	<li><span style="color: #0000ff;">Summer 2017 (Planned)</span>
<ul>
 	<li>Deploy User Automation</li>
 	<li>Upgrade to Newton</li>
 	<li>Upgrade to Ocata (pending RHEL release)</li>
</ul>
</li>
</ul>